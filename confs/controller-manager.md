## /etc/kubernetes/controller-manager.conf:
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
    user: system:kube-controller-manager
  name: system:kube-controller-manager@mk
current-context: system:kube-controller-manager@mk
kind: Config
preferences: {}
users:
- name: system:kube-controller-manager
  user:
    client-certificate-data: <base64 data>
    client-key-data: <base64 data>
```
## /etc/kubernetes/manifests/kube-controller-manager.yaml:
```yaml
apiVersion: v1
kind: Pod
metadata:
  creationTimestamp: null
  labels:
    component: kube-controller-manager
    tier: control-plane
  name: kube-controller-manager
  namespace: kube-system
spec:
  containers:
  - command:
    - kube-controller-manager
    - --allocate-node-cidrs=true
    - --authentication-kubeconfig=/etc/kubernetes/controller-manager.conf
    - --authorization-kubeconfig=/etc/kubernetes/controller-manager.conf
    - --bind-address=127.0.0.1
    - --client-ca-file=/var/lib/minikube/certs/ca.crt
    - --cluster-cidr=10.244.0.0/16
    - --cluster-name=mk
    - --cluster-signing-cert-file=/var/lib/minikube/certs/ca.crt
    - --cluster-signing-key-file=/var/lib/minikube/certs/ca.key
    - --controllers=*,bootstrapsigner,tokencleaner
    - --kubeconfig=/etc/kubernetes/controller-manager.conf
    - --leader-elect=false
    - --node-cidr-mask-size=24
    - --requestheader-client-ca-file=/var/lib/minikube/certs/front-proxy-ca.crt
    - --root-ca-file=/var/lib/minikube/certs/ca.crt
    - --service-account-private-key-file=/var/lib/minikube/certs/sa.key
    - --service-cluster-ip-range=10.96.0.0/12
    - --use-service-account-credentials=true
    image: k8s.gcr.io/kube-controller-manager:v1.18.0
    imagePullPolicy: IfNotPresent
    livenessProbe:
      failureThreshold: 8
      httpGet:
        host: 127.0.0.1
        path: /healthz
        port: 10257
        scheme: HTTPS
      initialDelaySeconds: 15
      timeoutSeconds: 15
    name: kube-controller-manager
    resources:
      requests:
        cpu: 200m
    volumeMounts:
    - mountPath: /etc/ssl/certs
      name: ca-certs
      readOnly: true
    - mountPath: /usr/libexec/kubernetes/kubelet-plugins/volume/exec
      name: flexvolume-dir
    - mountPath: /var/lib/minikube/certs
      name: k8s-certs
      readOnly: true
    - mountPath: /etc/kubernetes/controller-manager.conf
      name: kubeconfig
      readOnly: true
    - mountPath: /usr/share/ca-certificates
      name: usr-share-ca-certificates
      readOnly: true
  hostNetwork: true
  priorityClassName: system-cluster-critical
  volumes:
  - hostPath:
      path: /etc/ssl/certs
      type: DirectoryOrCreate
    name: ca-certs
  - hostPath:
      path: /usr/libexec/kubernetes/kubelet-plugins/volume/exec
      type: DirectoryOrCreate
    name: flexvolume-dir
  - hostPath:
      path: /var/lib/minikube/certs
      type: DirectoryOrCreate
    name: k8s-certs
  - hostPath:
      path: /etc/kubernetes/controller-manager.conf
      type: FileOrCreate
    name: kubeconfig
  - hostPath:
      path: /usr/share/ca-certificates
      type: DirectoryOrCreate
    name: usr-share-ca-certificates
status: {}
```