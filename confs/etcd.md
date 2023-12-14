## /etc/kubernetes/manifests/etcd.yaml:
```yaml
apiVersion: v1
kind: Pod
metadata:
  annotations:
    kubeadm.kubernetes.io/etcd.advertise-client-urls: https://192.168.39.43:2379
  creationTimestamp: null
  labels:
    component: etcd
    tier: control-plane
  name: etcd
  namespace: kube-system
spec:
  containers:
  - command:
    - etcd
    - --advertise-client-urls=https://192.168.39.43:2379
    - --cert-file=/var/lib/minikube/certs/etcd/server.crt
    - --client-cert-auth=true
    - --data-dir=/var/lib/minikube/etcd
    - --initial-advertise-peer-urls=https://192.168.39.43:2380
    - --initial-cluster=minikube=https://192.168.39.43:2380
    - --key-file=/var/lib/minikube/certs/etcd/server.key
    - --listen-client-urls=https://127.0.0.1:2379,https://192.168.39.43:2379
    - --listen-metrics-urls=http://127.0.0.1:2381
    - --listen-peer-urls=https://192.168.39.43:2380
    - --name=minikube
    - --peer-cert-file=/var/lib/minikube/certs/etcd/peer.crt
    - --peer-client-cert-auth=true
    - --peer-key-file=/var/lib/minikube/certs/etcd/peer.key
    - --peer-trusted-ca-file=/var/lib/minikube/certs/etcd/ca.crt
    - --proxy-refresh-interval=70000
    - --snapshot-count=10000
    - --trusted-ca-file=/var/lib/minikube/certs/etcd/ca.crt
    image: k8s.gcr.io/etcd:3.4.3-0
    imagePullPolicy: IfNotPresent
    livenessProbe:
      failureThreshold: 8
      httpGet:
        host: 127.0.0.1
        path: /health
        port: 2381
        scheme: HTTP
      initialDelaySeconds: 15
      timeoutSeconds: 15
    name: etcd
    resources: {}
    volumeMounts:
    - mountPath: /var/lib/minikube/etcd
      name: etcd-data
    - mountPath: /var/lib/minikube/certs/etcd
      name: etcd-certs
  hostNetwork: true
  priorityClassName: system-cluster-critical
  volumes:
  - hostPath:
      path: /var/lib/minikube/certs/etcd
      type: DirectoryOrCreate
    name: etcd-certs
  - hostPath:
      path: /var/lib/minikube/etcd
      type: DirectoryOrCreate
    name: etcd-data
status: {}
```