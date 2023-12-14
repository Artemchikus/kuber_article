##  /var/lib/minikube/kubeconfig:
```yaml
apiVersion: v1
clusters:
- cluster:
    certificate-authority: /var/lib/minikube/certs/ca.crt
    extensions:
    - extension:
        last-update: Thu, 09 Nov 2023 20:18:47 MSK
        provider: minikube.sigs.k8s.io
        version: v1.31.2
      name: cluster_info
    server: https://localhost:8443
  name: ""
contexts:
- context:
    cluster: ""
    extensions:
    - extension:
        last-update: Thu, 09 Nov 2023 20:18:47 MSK
        provider: minikube.sigs.k8s.io
        version: v1.31.2
      name: context_info
    user: ""
  name: ""
current-context: ""
kind: Config
preferences: {}
users:
- name: ""
  user:
    client-certificate: /var/lib/minikube/certs/apiserver.crt
    client-key: /var/lib/minikube/certs/apiserver.key
```
## kube-public/cluster-info:
```yaml
- apiVersion: v1
  data:
    kubeconfig: |
      apiVersion: v1
      clusters:
      - cluster:
          certificate-authority-data: <base64 data>
          server: http://control-plane.minikube.internal:3333
        name: ""
      contexts: null
      current-context: ""
      kind: Config
      preferences: {}
      users: null
  kind: ConfigMap
  metadata:
    creationTimestamp: "2023-11-06T17:08:58Z"
    name: cluster-info
    namespace: kube-public
    resourceVersion: "5108"
    selfLink: /api/v1/namespaces/kube-public/configmaps/cluster-info
    uid: 244b0f6f-9b4f-4bb8-9fcf-0b44306d66db
```