### deployments:
| EVENT    | NAMESPACE | NAME    | READY | UP-TO-DATE | AVAILABLE | AGE |       | CONTAINERS                                   | IMAGES SELECTOR   |
| -------- | --------- | ------- | ----- | ---------- | --------- | --- | ----- | -------------------------------------------- | ----------------- |
| ADDED    | default   | react-d | 0/2   | 0          | 0         | 0s  | react | ifilyaninitmo/itdt-contained-frontend:master | app in (frontend) |
| MODIFIED | default   | react-d | 0/2   | 0          | 0         | 0s  | react | ifilyaninitmo/itdt-contained-frontend:master | app in (frontend) |
| MODIFIED | default   | react-d | 0/2   | 0          | 0         | 0s  | react | ifilyaninitmo/itdt-contained-frontend:master | app in (frontend) |
| MODIFIED | default   | react-d | 0/2   | 2          | 0         | 0s  | react | ifilyaninitmo/itdt-contained-frontend:master | app in (frontend) |
| MODIFIED | default   | react-d | 1/2   | 2          | 1         | 26s | react | ifilyaninitmo/itdt-contained-frontend:master | app in (frontend) |
| MODIFIED | default   | react-d | 2/2   | 2          | 2         | 26s | react | ifilyaninitmo/itdt-contained-frontend:master | app in (frontend) |
### pods:
| EVENT    | NAMESPACE | NAME                    | READY | STATUS            | RESTARTS | AGE | IP         | NODE         | NOMINATED NODE | READINESS  GATES |
| -------- | --------- | ----------------------- | ----- | ----------------- | -------- | --- | ---------- | ------------ | -------------- | ---------------- |
| ADDED    | default   | react-d-86c848f59-qj6vq | 0/1   | Pending           | 0        | 0s  | `<none>`   | `<none>`     | `<none>`       | `<none>`         |
| ADDED    | default   | react-d-86c848f59-7mwt4 | 0/1   | Pending           | 0        | 0s  | `<none>`   | `<none>`     | `<none>`       | `<none>`         |
| MODIFIED | default   | react-d-86c848f59-qj6vq | 0/1   | Pending           | 0        | 0s  | `<none>`   | minikube-m02 | `<none>`       | `<none>`         |
| MODIFIED | default   | react-d-86c848f59-7mwt4 | 0/1   | Pending           | 0        | 0s  | `<none>`   | minikube     | `<none>`       | `<none>`         |
| MODIFIED | default   | react-d-86c848f59-qj6vq | 0/1   | ContainerCreating | 0        | 0s  | `<none>`   | minikube-m02 | `<none>`       | `<none>`         |
| MODIFIED | default   | react-d-86c848f59-7mwt4 | 0/1   | ContainerCreating | 0        | 0s  | `<none>`   | minikube     | `<none>`       | `<none>`         |
| MODIFIED | default   | react-d-86c848f59-qj6vq | 1/1   | Running           | 0        | 26s | 10.244.1.2 | minikube-m02 | `<none>`       | `<none>`         |
| MODIFIED | default   | react-d-86c848f59-7mwt4 | 1/1   | Running           | 0        | 26s | 10.244.0.3 | minikube     | `<none>`       | `<none>`         |
### replica-sets:
| EVENT    | NAMESPACE | NAME              | DESIRED | CURRENT | READY | AGE | CONTAINERS | IMAGES                                       | SELECTOR                                      |     |
| -------- | --------- | ----------------- | ------- | ------- | ----- | --- | ---------- | -------------------------------------------- | --------------------------------------------- | --- |
| ADDED    | default   | react-d-86c848f59 | 2       | 0       | 0     | 0s  | react      | ifilyaninitmo/itdt-contained-frontend:master | app in (frontend),pod-template-hash=86c848f59 |     |
| MODIFIED | default   | react-d-86c848f59 | 2       | 0       | 0     | 0s  | react      | ifilyaninitmo/itdt-contained-frontend:master | app in (frontend),pod-template-hash=86c848f59 |     |
| MODIFIED | default   | react-d-86c848f59 | 2       | 2       | 0     | 0s  | react      | ifilyaninitmo/itdt-contained-frontend:master | app in (frontend),pod-template-hash=86c848f59 |     |
| MODIFIED | default   | react-d-86c848f59 | 2       | 2       | 1     | 26s | react      | ifilyaninitmo/itdt-contained-frontend:master | app in (frontend),pod-template-hash=86c848f59 |     |
| MODIFIED | default   | react-d-86c848f59 | 2       | 2       | 2     | 26s | react      | ifilyaninitmo/itdt-contained-frontend:master | app in (frontend),pod-template-hash=86c848f59 |     |
