### pods:
| EVENT    | NAMESPACE | NAME  | READY | STATUS            | RESTARTS | AGE | IP         | NODE     | NOMINATED  NODE | READINESS  GATES |
| -------- | --------- | ----- | ----- | ----------------- | -------- | --- | ---------- | -------- | --------------- | ---------------- |
| ADDED    | default   | vault | 0/1   | Pending           | 0        | 0s  | `<none>`   | `<none>` | `<none>`        | `<none>`         |
| MODIFIED | default   | vault | 0/1   | Pending           | 0        | 0s  | `<none>`   | minikube | `<none>`        | `<none>`         |
| MODIFIED | default   | vault | 0/1   | ContainerCreating | 0        | 0s  | `<none>`   | minikube | `<none>`        | `<none>`         |
| MODIFIED | default   | vault | 1/1   | Running           | 0        | 10s | 10.244.0.3 | minikube | `<none>`        | `<none>`         |
