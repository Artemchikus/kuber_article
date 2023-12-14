## kube-system/kube-proxy:
```yaml
- apiVersion: v1
  data:
    config.conf: |-
      apiVersion: kubeproxy.config.k8s.io/v1alpha1
      bindAddress: 0.0.0.0
      clientConnection:
        acceptContentTypes: ""
        burst: 0
        contentType: ""
        kubeconfig: /var/lib/kube-proxy/kubeconfig.conf
        qps: 0
      clusterCIDR: 10.244.0.0/16
      configSyncPeriod: 0s
      conntrack:
        maxPerCore: 0
        min: null
        tcpCloseWaitTimeout: 0s
        tcpEstablishedTimeout: 0s
      detectLocalMode: ""
      enableProfiling: false
      healthzBindAddress: ""
      hostnameOverride: ""
      iptables:
        masqueradeAll: false
        masqueradeBit: null
        minSyncPeriod: 0s
        syncPeriod: 0s
      ipvs:
        excludeCIDRs: null
        minSyncPeriod: 0s
        scheduler: ""
        strictARP: false
        syncPeriod: 0s
        tcpFinTimeout: 0s
        tcpTimeout: 0s
        udpTimeout: 0s
      kind: KubeProxyConfiguration
      metricsBindAddress: 0.0.0.0:10249
      mode: ""
      nodePortAddresses: null
      oomScoreAdj: null
      portRange: ""
      showHiddenMetricsForVersion: ""
      udpIdleTimeout: 0s
      winkernel:
        enableDSR: false
        networkName: ""
        sourceVip: ""
    kubeconfig.conf: |-
      apiVersion: v1
      kind: Config
      clusters:
      - cluster:
          certificate-authority: /var/run/secrets/kubernetes.io/serviceaccount/ca.crt
          server: https://control-plane.minikube.internal:8443
        name: default
      contexts:
      - context:
          cluster: default
          namespace: default
          user: default
        name: default
      current-context: default
      users:
      - name: default
        user:
          tokenFile: /var/run/secrets/kubernetes.io/serviceaccount/token
  kind: ConfigMap
  metadata:
    creationTimestamp: "2023-11-06T17:08:58Z"
    labels:
      app: kube-proxy
    name: kube-proxy
    namespace: kube-system
    resourceVersion: "5457"
    selfLink: /api/v1/namespaces/kube-system/configmaps/kube-proxy
    uid: d625cdf7-2c90-4704-86d9-9ba58e80f793
```