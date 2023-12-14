## Bash скрипт, который осуществляет скан всех созданных ресурсов kubernets, с последующей их записью в файл с именем ресурса:
```bash
#!/bin/bash

resources=`kubectl api-resources -o name`

mkdir scan

for resource in $resources; do
    kubectl get -o wide -A $resource >> scan/$resource
    out=`cat scan/$resource`
    if [[ -z $out ]]; then
        rm scan/$resource
    fi
done
```
