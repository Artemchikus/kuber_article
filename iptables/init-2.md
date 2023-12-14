#### kubernetes load balancer firewall:
```bash
-A INPUT -m conntrack --ctstate NEW -j KUBE-PROXY-FIREWALL
```
#### kubernetes health check service ports:
```bash
-A INPUT -j KUBE-NODEPORTS
```
#### kubernetes externally-visible service portals:
```bash
-A INPUT -m conntrack --ctstate NEW -j KUBE-EXTERNAL-SERVICES
```
#### block incoming localnet connections:
```bash
-A INPUT -j KUBE-FIREWALL
-A KUBE-FIREWALL ! -s 127.0.0.0/8 -d 127.0.0.0/8 -m conntrack ! --ctstate RELATED,ESTABLISHED,DNAT -j DROP
```


#### kubernetes load balancer firewall:
```bash
-A FORWARD -m conntrack --ctstate NEW -j KUBE-PROXY-FIREWALL
```
#### kubernetes forwarding rules:
```bash
-A FORWARD -j KUBE-FORWARD
-A KUBE-FORWARD -m conntrack --ctstate INVALID -j DROP
-A KUBE-FORWARD -m mark --mark 0x4000/0x4000 -j ACCEPT
-A KUBE-FORWARD -m comment --comment "kubernetes forwarding conntrack rule" -m conntrack --ctstate RELATED,ESTABLISHED -j ACCEPT
```
#### kubernetes service portals:
```bash
-A FORWARD -m conntrack --ctstate NEW -j KUBE-SERVICES
```
#### kubernetes externally-visible service portals:
```bash
-A FORWARD -m conntrack --ctstate NEW -j KUBE-EXTERNAL-SERVICES
```


#### kubernetes load balancer firewall:
```bash
-A OUTPUT -m conntrack --ctstate NEW -j KUBE-PROXY-FIREWALL
```
#### kubernetes service portals:
```bash
-A OUTPUT -m conntrack --ctstate NEW -j KUBE-SERVICES
...
```
#### block incoming localnet connections:
```bash
-A OUTPUT -j KUBE-FIREWALL
-A KUBE-FIREWALL ! -s 127.0.0.0/8 -d 127.0.0.0/8 -m conntrack ! --ctstate RELATED,ESTABLISHED,DNAT -j DROP
```


#### kubernetes service portals:
```bash
-A PREROUTING -j KUBE-SERVICES
...
```


#### default/kubernetes:https service:
```bash
-A OUTPUT -j KUBE-SERVICES
-A KUBE-SERVICES -d 10.96.0.1/32 -p tcp -m comment --comment "default/kubernetes:https cluster IP" -m tcp --dport 443 -j KUBE-SVC-NPX46M4PTMTKRN6Y
-A KUBE-SVC-NPX46M4PTMTKRN6Y ! -s 10.244.0.0/16 -d 10.96.0.1/32 -p tcp -m comment --comment "default/kubernetes:https cluster IP" -m tcp --dport 443 -j KUBE-MARK-MASQ
-A KUBE-MARK-MASQ -j MARK --set-xmark 0x4000/0x4000
-A KUBE-SVC-NPX46M4PTMTKRN6Y -m comment --comment "default/kubernetes:https -> 192.168.49.2:8443" -j KUBE-SEP-VPILYQBSPPXYB66K
-A KUBE-SEP-VPILYQBSPPXYB66K -s 192.168.49.2/32 -m comment --comment "default/kubernetes:https" -j KUBE-MARK-MASQ
-A KUBE-SEP-VPILYQBSPPXYB66K -p tcp -m comment --comment "default/kubernetes:https" -m tcp -j DNAT --to-destination 192.168.49.2:8443
```
#### kube-system/kube-dns:metrics service:
```bash
-A OUTPUT -j KUBE-SERVICES
-A KUBE-SERVICES -d 10.96.0.10/32 -p tcp -m comment --comment "kube-system/kube-dns:metrics cluster IP" -m tcp --dport 9153 -j KUBE-SVC-JD5MR3NA4I4DYORP
-A KUBE-SVC-JD5MR3NA4I4DYORP ! -s 10.244.0.0/16 -d 10.96.0.10/32 -p tcp -m comment --comment "kube-system/kube-dns:metrics cluster IP" -m tcp --dport 9153 -j KUBE-MARK-MASQ
-A KUBE-MARK-MASQ -j MARK --set-xmark 0x4000/0x4000
-A KUBE-SVC-JD5MR3NA4I4DYORP -m comment --comment "kube-system/kube-dns:metrics -> 10.244.0.2:9153" -j KUBE-SEP-N4G2XR5TDX7PQE7P
-A KUBE-SEP-N4G2XR5TDX7PQE7P -s 10.244.0.2/32 -m comment --comment "kube-system/kube-dns:metrics" -j KUBE-MARK-MASQ
-A KUBE-MARK-MASQ -j MARK --set-xmark 0x4000/0x4000
-A KUBE-SEP-N4G2XR5TDX7PQE7P -p tcp -m comment --comment "kube-system/kube-dns:metrics" -m tcp -j DNAT --to-destination 10.244.0.2:9153
```
#### kube-system/kube-dns:dns service:
```bash
-A OUTPUT -j KUBE-SERVICES
-A KUBE-SERVICES -d 10.96.0.10/32 -p udp -m comment --comment "kube-system/kube-dns:dns cluster IP" -m udp --dport 53 -j KUBE-SVC-TCOU7JCQXEZGVUNU
-A KUBE-SVC-TCOU7JCQXEZGVUNU ! -s 10.244.0.0/16 -d 10.96.0.10/32 -p udp -m comment --comment "kube-system/kube-dns:dns cluster IP" -m udp --dport 53 -j KUBE-MARK-MASQ
-A KUBE-MARK-MASQ -j MARK --set-xmark 0x4000/0x4000
-A KUBE-SVC-TCOU7JCQXEZGVUNU -m comment --comment "kube-system/kube-dns:dns -> 10.244.0.2:53" -j KUBE-SEP-YIL6JZP7A3QYXJU2
-A KUBE-SEP-YIL6JZP7A3QYXJU2 -s 10.244.0.2/32 -m comment --comment "kube-system/kube-dns:dns" -j KUBE-MARK-MASQ
-A KUBE-MARK-MASQ -j MARK --set-xmark 0x4000/0x4000
-A KUBE-SEP-YIL6JZP7A3QYXJU2 -p udp -m comment --comment "kube-system/kube-dns:dns" -m udp -j DNAT --to-destination 10.244.0.2:53
```
#### kube-system/kube-dns:dns-tcp:
```bash
-A OUTPUT -m comment --comment "kubernetes service portals" -j KUBE-SERVICES
-A KUBE-SERVICES -d 10.96.0.10/32 -p tcp -m comment --comment "kube-system/kube-dns:dns-tcp cluster IP" -m tcp --dport 53 -j KUBE-SVC-ERIFXISQEP7F7OF4
-A KUBE-SVC-ERIFXISQEP7F7OF4 ! -s 10.244.0.0/16 -d 10.96.0.10/32 -p tcp -m comment --comment "kube-system/kube-dns:dns-tcp cluster IP" -m tcp --dport 53 -j KUBE-MARK-MASQ
-A KUBE-MARK-MASQ -j MARK --set-xmark 0x4000/0x4000
-A KUBE-SVC-ERIFXISQEP7F7OF4 -m comment --comment "kube-system/kube-dns:dns-tcp -> 10.244.0.2:53" -j KUBE-SEP-IT2ZTR26TO4XFPTO
-A KUBE-SEP-IT2ZTR26TO4XFPTO -s 10.244.0.2/32 -m comment --comment "kube-system/kube-dns:dns-tcp" -j KUBE-MARK-MASQ
-A KUBE-MARK-MASQ -j MARK --set-xmark 0x4000/0x4000
-A KUBE-SEP-IT2ZTR26TO4XFPTO -p tcp -m comment --comment "kube-system/kube-dns:dns-tcp" -m tcp -j DNAT --to-destination 10.244.0.2:53
```
#### kubernetes service nodeports:
```bash
-A OUTPUT -j KUBE-SERVICES
-A KUBE-SERVICES -m addrtype --dst-type LOCAL -j KUBE-NODEPORTS
```


#### kubernetes postrouting rules:
```bash
-A POSTROUTING -j KUBE-POSTROUTING
-A KUBE-POSTROUTING -m mark ! --mark 0x4000/0x4000 -j RETURN
-A KUBE-POSTROUTING -j MARK --set-xmark 0x4000/0x0
-A KUBE-POSTROUTING -m comment --comment "kubernetes service traffic requiring SNAT" -j MASQUERADE --random-fully
```
#### CoreDNS Pod:
```bash
-A POSTROUTING -s 10.244.0.2/32 -j CNI-430e1a042a914a692a49a13c
-A CNI-430e1a042a914a692a49a13c -d 10.244.0.0/16 -j ACCEPT
-A CNI-430e1a042a914a692a49a13c ! -d 224.0.0.0/4 -j MASQUERADE
```