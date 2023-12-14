## /etc/kubernetes/kubelet.conf:
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
    user: system:node:minikube
  name: system:node:minikube@mk
current-context: system:node:minikube@mk
kind: Config
preferences: {}
users:
- name: system:node:minikube
  user:
    client-certificate-data: <base64 data>
    client-key-data: <base64 data>
```
## kube-system/kubelet-config-1.18:
```yaml
- apiVersion: v1
  data:
    kubelet: |
      apiVersion: kubelet.config.k8s.io/v1beta1
      authentication:
        anonymous:
          enabled: false
        webhook:
          cacheTTL: 0s
          enabled: true
        x509:
          clientCAFile: /var/lib/minikube/certs/ca.crt
      authorization:
        mode: Webhook
        webhook:
          cacheAuthorizedTTL: 0s
          cacheUnauthorizedTTL: 0s
      cgroupDriver: cgroupfs
      clusterDNS:
      - 10.96.0.10
      clusterDomain: cluster.local
      cpuManagerReconcilePeriod: 0s
      evictionHard:
        imagefs.available: 0%
        nodefs.available: 0%
        nodefs.inodesFree: 0%
      evictionPressureTransitionPeriod: 0s
      failSwapOn: false
      fileCheckFrequency: 0s
      hairpinMode: hairpin-veth
      healthzBindAddress: 127.0.0.1
      healthzPort: 10248
      httpCheckFrequency: 0s
      imageGCHighThresholdPercent: 100
      imageMinimumGCAge: 0s
      kind: KubeletConfiguration
      nodeStatusReportFrequency: 0s
      nodeStatusUpdateFrequency: 0s
      rotateCertificates: true
      runtimeRequestTimeout: 15m0s
      staticPodPath: /etc/kubernetes/manifests
      streamingConnectionIdleTimeout: 0s
      syncFrequency: 0s
      volumeStatsAggPeriod: 0s
  kind: ConfigMap
  metadata:
    creationTimestamp: "2023-11-06T17:08:57Z"
    name: kubelet-config-1.18
    namespace: kube-system
    resourceVersion: "170"
    selfLink: /api/v1/namespaces/kube-system/configmaps/kubelet-config-1.18
    uid: 23f00e91-3e6c-4b24-bfef-2f4d21983462
```