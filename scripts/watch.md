## Bash скрипт, который осуществляет просмотр всех ресурсов kubernets, с записью всех событий в файл с именем ресурса:
```bash 
#!/bin/bash

resources=`kubectl api-resources -o name`

mkdir watch

for resource in $resources; do
    echo "${resource}"
    kubectl get --output-watch-events --watch-only -o wide -A $resource >> watch/$resource &
done

read end

for resource in $resources; do
    out=`cat watch/$resource`
    if [[ -z $out ]]; then
        rm watch/$resource
    fi
done

kill -9 `pgrep kubectl`
```