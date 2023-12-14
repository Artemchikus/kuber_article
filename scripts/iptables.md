## Bash скрипт, для удобного извлечения iptables:
```bash
#!/bin/bash

node_names=`docker ps --format '{{.Names}}' | grep minikube`

echo -n "" > iptables

for node_name in $node_names; do
    echo $node_name >> iptables
    docker exec $node_name iptables-save >> iptables
    printf "\n\n" >> iptables
done
```