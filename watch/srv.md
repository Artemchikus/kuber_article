### endpoints:
| EVENT    | NAMESPACE | NAME  | ENDPOINTS       | AGE |
| -------- | --------- | ----- | --------------- | --- |
| ADDED    | default   | vault | `<none>`        | 0s  |
| MODIFIED | default   | vault | 10.244.0.3:8200 | 9s  |
### endpointslices:
| EVENT    | NAMESPACE | NAME        | ADDRESSTYPE | PORTS     | ENDPOINTS  | AGE |
| -------- | --------- | ----------- | ----------- | --------- | ---------- | --- |
| ADDED    | default   | vault-wgjvs | IPv4        | `<unset>` | `<unset>`  | 0s  |
| MODIFIED | default   | vault-wgjvs | IPv4        | 8200      | 10.244.0.3 | 9s  |
### services:
| EVENT | NAMESPACE | NAME  | TYPE     | CLUSTER-IP    | EXTERNAL-IP | PORT(S)        | AGE | SELECTOR  |
| ----- | --------- | ----- | -------- | ------------- | ----------- | -------------- | --- | --------- |
| ADDED | default   | vault | NodePort | 10.104.205.29 | `<none>`    | 8200:30074/TCP | 0s  | app=vault |
