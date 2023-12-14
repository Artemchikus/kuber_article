## /etc/kubernetes/admin.conf:
```yaml
apiVersion: v1
clusters:
- cluster:
    certificate-authority-data: <base64 data>
    server: https://control-plane.minikube.internal:8443
  name: mk
contexts:
- context:
    cluster: mk
    user: kubernetes-admin
  name: kubernetes-admin@mk
current-context: kubernetes-admin@mk
kind: Config
preferences: {}
users:
- name: kubernetes-admin
  user:
    client-certificate-data: <base64 data>
    client-key-data: <base64 data>
```
## kube-system/kubeadm-config:
```yaml
- apiVersion: v1
  data:
    ClusterConfiguration: |
      apiServer:
        certSANs:
        - 127.0.0.1
        - localhost
        - 192.168.39.43
        extraArgs:
          authorization-mode: Node,RBAC
          enable-admission-plugins: NamespaceLifecycle,LimitRanger,ServiceAccount,DefaultStorageClass,DefaultTolerationSeconds,NodeRestriction,MutatingAdmissionWebhook,ValidatingAdmissionWebhook,ResourceQuota
        timeoutForControlPlane: 4m0s
      apiVersion: kubeadm.k8s.io/v1beta2
      certificatesDir: /var/lib/minikube/certs
      clusterName: mk
      controlPlaneEndpoint: control-plane.minikube.internal:3333
      controllerManager:
        extraArgs:
          allocate-node-cidrs: "true"
          leader-elect: "false"
      dns:
        type: CoreDNS
      etcd:
        local:
          dataDir: /var/lib/minikube/etcd
          extraArgs:
            proxy-refresh-interval: "70000"
      imageRepository: k8s.gcr.io
      kind: ClusterConfiguration
      kubernetesVersion: v1.18.0
      networking:
        dnsDomain: cluster.local
        podSubnet: 10.244.0.0/16
        serviceSubnet: 10.96.0.0/12
      scheduler:
        extraArgs:
          leader-elect: "false"
    ClusterStatus: |
      apiEndpoints:
        minikube:
          advertiseAddress: 192.168.39.43
          bindPort: 8443
      apiVersion: kubeadm.k8s.io/v1beta2
      kind: ClusterStatus
  kind: ConfigMap
  metadata:
    creationTimestamp: "2023-11-06T17:08:57Z"
    name: kubeadm-config
    namespace: kube-system
    resourceVersion: "2870"
    selfLink: /api/v1/namespaces/kube-system/configmaps/kubeadm-config
    uid: 5f34033c-86f0-477f-92f2-37624124c096
```