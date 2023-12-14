### endpoints:
| EVENT    | NAMESPACE | NAME     | ENDPOINTS                       | AGE |
| -------- | --------- | -------- | ------------------------------- | --- |
| ADDED    | default   | react-np | `<none>`                        | 0s  |
| MODIFIED | default   | react-np | 10.244.0.3:3000                 | 20s |
| MODIFIED | default   | react-np | 10.244.0.3:3000,10.244.1.2:3000 | 21s |
### endpoinslices:
| EVENT    | NAMESPACE | NAME           | ADDRESSTYPE | PORTS     | ENDPOINTS             | AGE |
| -------- | --------- | -------------- | ----------- | --------- | --------------------- | --- |
| ADDED    | default   | react-np-t7zqb | IPv4        | `<unset>` | `<unset>`             | 0s  |
| MODIFIED | default   | react-np-t7zqb | IPv4        | 3000      | 10.244.0.3            | 20s |
| MODIFIED | default   | react-np-t7zqb | IPv4        | 3000      | 10.244.0.3,10.244.1.2 | 21s |
### services:
| EVENT | NAMESPACE | NAME     | TYPE     | CLUSTER-IP     | EXTERNAL-IP | PORT(S)      | AGE | SELECTOR     |
| ----- | --------- | -------- | -------- | -------------- | ----------- | ------------ | --- | ------------ |
| ADDED | default   | react-np | NodePort | 10.105.124.252 | `<none>`    | 80:30123/TCP | 0s  | app=frontend |

