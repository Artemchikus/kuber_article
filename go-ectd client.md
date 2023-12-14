## Клиент на go для получения списка ключей и значений напрямую из etcd:
```go
package main

import (
	"bytes"
	"context"
	"crypto/tls"
	"encoding/base64"
	"encoding/json"
	"fmt"
	"io"
	"net/http"
	"os/exec"
	"time"

	clientv3 "go.etcd.io/etcd/client/v3"
)

type key struct {
	Key   string `json:"key"`
	Value string `json:"value"`
}

type keys struct {
	Kvs []*key `json:"kvs"`
}

// Можно выбрать формат вывода: keys(только ключи), values(только значения), all(все вместе)
// PS кривой выод значений не моя вина, это kuber че-то попутал (видимо смотреть значения только через kube API)
const OutputType string = "keys"

// Нужно изменить, если узлов в кластере больше одного, так как в таком случае меняется название контейнера с minikube на minikube-node-1
const MultipleNodes bool = false

var (
	nodeIp string
)

func main() {
	setAliases()
	// curl()
	// httpsclient()
	etcdClient()
}

func setAliases() {
	contName := "minikube"

	if MultipleNodes {
		contName = "minikube-node-1"
	}

	getPeerCrt := exec.Command("docker", "cp", contName+":/var/lib/minikube/certs/etcd/peer.crt", "peer.crt")

	err := getPeerCrt.Run()
	if err != nil {
		panic(err)
	}

	getPeerKey := exec.Command("docker", "cp", contName+":/var/lib/minikube/certs/etcd/peer.key", "peer.key")

	err = getPeerKey.Run()
	if err != nil {
		panic(err)
	}

	getIp := exec.Command("kubectl", "get", "nodes", "-o", "jsonpath='{.items[0].status.addresses[0].address}'")

	ip, err := getIp.Output()
	if err != nil {
		panic(err)
	}

	nodeIp = string(ip[1 : len(ip)-1])
}

func curl() {
	keys := &keys{}

	https := "https://" + nodeIp + ":2379/v3/kv/range"

	cmd := exec.Command("curl", "--insecure", "--cert", "peer.crt", "--key", "peer.key", "-X", "POST", https, "-d", `{"key": "AA==", "range_end": "AA=="}`)

	out, err := cmd.Output()
	if err != nil {
		panic(err)
	}

	json.Unmarshal(out, keys)

	for _, k := range keys.Kvs {
		kbytes, _ := base64.StdEncoding.DecodeString(k.Key)
		vbytes, _ := base64.StdEncoding.DecodeString(k.Value)
		k.Key = string(kbytes)
		k.Value = string(vbytes)
		if OutputType == "keys" {
			fmt.Printf("key: %v\n", k.Key)
		} else if OutputType == "values" {
			fmt.Printf("value: {%v}\n", k.Value)
		} else if OutputType == "all" {
			fmt.Printf("key: %v\n", k.Key)
			fmt.Printf("value: {%v}\n", k.Value)
		}
	}
}

func httpsclient() {
	kp, _ := tls.LoadX509KeyPair("peer.crt", "peer.key")

	certs := []tls.Certificate{kp}

	tr := &http.Transport{
		TLSClientConfig: &tls.Config{InsecureSkipVerify: true, Certificates: certs},
	}

	client := &http.Client{Transport: tr}

	data := []byte(`{"key": "AA==", "range_end": "AA=="}`)

	bodyReader := bytes.NewReader(data)

	https := "https://" + nodeIp + ":2379/v3/kv/range"

	resp, err := client.Post(https, "application/json", bodyReader)
	if err != nil {
		panic(err)
	}
	defer resp.Body.Close()

	fmt.Printf("resp: %v\n", resp.Body)

	keys := &keys{}

	out, err := io.ReadAll(resp.Body)
	if err != nil {
		fmt.Println("Error reading response:", err)
		return
	}

	json.Unmarshal(out, keys)

	for _, k := range keys.Kvs {
		kbytes, _ := base64.StdEncoding.DecodeString(k.Key)
		vbytes, _ := base64.StdEncoding.DecodeString(k.Value)
		k.Key = string(kbytes)
		k.Value = string(vbytes)
		if OutputType == "keys" {
			fmt.Printf("key: %v\n", k.Key)
		} else if OutputType == "values" {
			fmt.Printf("value: {%v}\n", k.Value)
		} else if OutputType == "all" {
			fmt.Printf("key: %v\n", k.Key)
			fmt.Printf("value: {%v}\n", k.Value)
		}
	}
}

func etcdClient() {
	kp, _ := tls.LoadX509KeyPair("peer.crt", "peer.key")

	certs := []tls.Certificate{kp}

	tcp := nodeIp + ":2379"

	cli, err := clientv3.New(clientv3.Config{
		Endpoints:   []string{tcp},
		DialTimeout: 5 * time.Second,
		TLS:         &tls.Config{InsecureSkipVerify: true, Certificates: certs},
	})
	if err != nil {
		panic(err)
	}
	defer cli.Close()

	resp, _ := cli.KV.Get(context.Background(), "", clientv3.WithPrefix())

	for _, k := range resp.Kvs {
		if OutputType == "keys" {
			fmt.Printf("key: %s\n", k.Key)
		} else if OutputType == "values" {
			fmt.Printf("value: {%s}\n", k.Value)
		} else if OutputType == "all" {
			fmt.Printf("key: %s\n", k.Key)
			fmt.Printf("value: {%s}\n", k.Value)
		}
	}
}
```