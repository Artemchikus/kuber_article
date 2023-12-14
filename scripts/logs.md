## Bash скрипт, который осуществляет просмотр логов всех компонентов control-plane kubernets, с последующей записью их в файл с именем компонента:
```bash
#!/bin/bash

pods=`kubectl get pods -o name -A`

mkdir logs

for pod in $pods; do
    pod=`echo $pod | awk -F "/" '{print $2}'`
    kubectl logs -f -n kube-system $pod >> logs/$pod &
done

node_names=`docker ps --format '{{.Names}}' | grep minikube`

for node_name in $node_names; do
    docker exec $node_name journalctl -u kubelet -x >> logs/kubelet-$node_name
done

read end

kill -9 `pgrep kubectl`
```