## kube-system/coredns:
```yaml
- apiVersion: v1
  data:
    Corefile: |
      .:53 {
          log
          errors
          health {
             lameduck 5s
          }
          ready
          kubernetes cluster.local in-addr.arpa ip6.arpa {
             pods insecure
             fallthrough in-addr.arpa ip6.arpa
             ttl 30
          }
          prometheus :9153
          hosts {
             192.168.39.1 host.minikube.internal
             fallthrough
          }
          forward . /etc/resolv.conf
          cache 30
          loop
          reload
          loadbalance
      }
    Corefile-backup: |
      .:53 {
          log
          errors
          health {
             lameduck 5s
          }
          ready
          kubernetes cluster.local in-addr.arpa ip6.arpa {
             pods insecure
             fallthrough in-addr.arpa ip6.arpa
             ttl 30
          }
          prometheus :9153
          hosts {
             192.168.39.1 host.minikube.internal
             fallthrough
          }
          forward . /etc/resolv.conf
          cache 30
          loop
          reload
          loadbalance
      }
  kind: ConfigMap
  metadata:
    creationTimestamp: "2023-11-06T17:08:58Z"
    name: coredns
    namespace: kube-system
    resourceVersion: "5454"
    selfLink: /api/v1/namespaces/kube-system/configmaps/coredns
    uid: ab37da5c-af48-4eb0-a2e2-0568bebc71ef
```