## /etc/kubernetes/scheduler.conf:
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
    user: system:kube-scheduler
  name: system:kube-scheduler@mk
current-context: system:kube-scheduler@mk
kind: Config
preferences: {}
users:
- name: system:kube-scheduler
  user:
    client-certificate-data: <base64 data>
    client-key-data: <base64 data>
```
## /etc/kubernetes/manifests/kube-scheduler.yaml:
```yaml
apiVersion: v1
kind: Pod
metadata:
  creationTimestamp: null
  labels:
    component: kube-scheduler
    tier: control-plane
  name: kube-scheduler
  namespace: kube-system
spec:
  containers:
  - command:
    - kube-scheduler
    - --authentication-kubeconfig=/etc/kubernetes/scheduler.conf
    - --authorization-kubeconfig=/etc/kubernetes/scheduler.conf
    - --bind-address=127.0.0.1
    - --kubeconfig=/etc/kubernetes/scheduler.conf
    - --leader-elect=false
    image: k8s.gcr.io/kube-scheduler:v1.18.0
    imagePullPolicy: IfNotPresent
    livenessProbe:
      failureThreshold: 8
      httpGet:
        host: 127.0.0.1
        path: /healthz
        port: 10259
        scheme: HTTPS
      initialDelaySeconds: 15
      timeoutSeconds: 15
    name: kube-scheduler
    resources:
      requests:
        cpu: 100m
    volumeMounts:
    - mountPath: /etc/kubernetes/scheduler.conf
      name: kubeconfig
      readOnly: true
  hostNetwork: true
  priorityClassName: system-cluster-critical
  volumes:
  - hostPath:
      path: /etc/kubernetes/scheduler.conf
      type: FileOrCreate
    name: kubeconfig
status: {}
```