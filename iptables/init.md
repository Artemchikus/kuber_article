```bash
# Generated by iptables-save v1.8.7 on Sat Dec  9 11:26:25 2023
*mangle
:PREROUTING ACCEPT [0:0]
:INPUT ACCEPT [0:0]
:FORWARD ACCEPT [0:0]
:OUTPUT ACCEPT [0:0]
:POSTROUTING ACCEPT [0:0]
:KUBE-IPTABLES-HINT - [0:0]
:KUBE-KUBELET-CANARY - [0:0]
:KUBE-PROXY-CANARY - [0:0]
COMMIT
# Completed on Sat Dec  9 11:26:25 2023
# Generated by iptables-save v1.8.7 on Sat Dec  9 11:26:25 2023
*filter
:INPUT ACCEPT [0:0]
:FORWARD ACCEPT [0:0]
:OUTPUT ACCEPT [0:0]
:DOCKER - [0:0]
:DOCKER-ISOLATION-STAGE-1 - [0:0]
:DOCKER-ISOLATION-STAGE-2 - [0:0]
:DOCKER-USER - [0:0]
:KUBE-EXTERNAL-SERVICES - [0:0]
:KUBE-FIREWALL - [0:0]
:KUBE-FORWARD - [0:0]
:KUBE-KUBELET-CANARY - [0:0]
:KUBE-NODEPORTS - [0:0]
:KUBE-PROXY-CANARY - [0:0]
:KUBE-PROXY-FIREWALL - [0:0]
:KUBE-SERVICES - [0:0]
-A INPUT -m conntrack --ctstate NEW -m comment --comment "kubernetes load balancer firewall" -j KUBE-PROXY-FIREWALL
-A INPUT -m comment --comment "kubernetes health check service ports" -j KUBE-NODEPORTS
-A INPUT -m conntrack --ctstate NEW -m comment --comment "kubernetes externally-visible service portals" -j KUBE-EXTERNAL-SERVICES
-A INPUT -j KUBE-FIREWALL

-A FORWARD -m conntrack --ctstate NEW -m comment --comment "kubernetes load balancer firewall" -j KUBE-PROXY-FIREWALL
-A FORWARD -m comment --comment "kubernetes forwarding rules" -j KUBE-FORWARD
-A FORWARD -m conntrack --ctstate NEW -m comment --comment "kubernetes service portals" -j KUBE-SERVICES
-A FORWARD -m conntrack --ctstate NEW -m comment --comment "kubernetes externally-visible service portals" -j KUBE-EXTERNAL-SERVICES
-A FORWARD -j DOCKER-USER
-A FORWARD -j DOCKER-ISOLATION-STAGE-1
-A FORWARD -o docker0 -m conntrack --ctstate RELATED,ESTABLISHED -j ACCEPT
-A FORWARD -o docker0 -j DOCKER
-A FORWARD -i docker0 ! -o docker0 -j ACCEPT
-A FORWARD -i docker0 -o docker0 -j ACCEPT

-A OUTPUT -m conntrack --ctstate NEW -m comment --comment "kubernetes load balancer firewall" -j KUBE-PROXY-FIREWALL
-A OUTPUT -m conntrack --ctstate NEW -m comment --comment "kubernetes service portals" -j KUBE-SERVICES
-A OUTPUT -j KUBE-FIREWALL

-A DOCKER-ISOLATION-STAGE-1 -i docker0 ! -o docker0 -j DOCKER-ISOLATION-STAGE-2
-A DOCKER-ISOLATION-STAGE-1 -j RETURN
-A DOCKER-ISOLATION-STAGE-2 -o docker0 -j DROP
-A DOCKER-ISOLATION-STAGE-2 -j RETURN
-A DOCKER-USER -j RETURN
-A KUBE-FIREWALL ! -s 127.0.0.0/8 -d 127.0.0.0/8 -m comment --comment "block incoming localnet connections" -m conntrack ! --ctstate RELATED,ESTABLISHED,DNAT -j DROP
-A KUBE-FORWARD -m conntrack --ctstate INVALID -j DROP
-A KUBE-FORWARD -m comment --comment "kubernetes forwarding rules" -m mark --mark 0x4000/0x4000 -j ACCEPT
-A KUBE-FORWARD -m comment --comment "kubernetes forwarding conntrack rule" -m conntrack --ctstate RELATED,ESTABLISHED -j ACCEPT
COMMIT
# Completed on Sat Dec  9 11:26:25 2023
# Generated by iptables-save v1.8.7 on Sat Dec  9 11:26:25 2023
*nat
:PREROUTING ACCEPT [103:7853]
:INPUT ACCEPT [95:6730]
:OUTPUT ACCEPT [317:18960]
:POSTROUTING ACCEPT [318:19032]
:CNI-430e1a042a914a692a49a13c - [0:0]
:DOCKER - [0:0]
:DOCKER_OUTPUT - [0:0]
:DOCKER_POSTROUTING - [0:0]
:KUBE-KUBELET-CANARY - [0:0]
:KUBE-MARK-MASQ - [0:0]
:KUBE-NODEPORTS - [0:0]
:KUBE-POSTROUTING - [0:0]
:KUBE-PROXY-CANARY - [0:0]
:KUBE-SEP-IT2ZTR26TO4XFPTO - [0:0]
:KUBE-SEP-N4G2XR5TDX7PQE7P - [0:0]
:KUBE-SEP-VPILYQBSPPXYB66K - [0:0]
:KUBE-SEP-YIL6JZP7A3QYXJU2 - [0:0]
:KUBE-SERVICES - [0:0]
:KUBE-SVC-ERIFXISQEP7F7OF4 - [0:0]
:KUBE-SVC-JD5MR3NA4I4DYORP - [0:0]
:KUBE-SVC-NPX46M4PTMTKRN6Y - [0:0]
:KUBE-SVC-TCOU7JCQXEZGVUNU - [0:0]
-A PREROUTING -m comment --comment "kubernetes service portals" -j KUBE-SERVICES
-A PREROUTING -d 192.168.49.1/32 -j DOCKER_OUTPUT
-A PREROUTING -m addrtype --dst-type LOCAL -j DOCKER

-A OUTPUT -m comment --comment "kubernetes service portals" -j KUBE-SERVICES
-A OUTPUT -d 192.168.49.1/32 -j DOCKER_OUTPUT
-A OUTPUT ! -d 127.0.0.0/8 -m addrtype --dst-type LOCAL -j DOCKER

-A POSTROUTING -m comment --comment "kubernetes postrouting rules" -j KUBE-POSTROUTING
-A POSTROUTING -s 172.17.0.0/16 ! -o docker0 -j MASQUERADE
-A POSTROUTING -d 192.168.49.1/32 -j DOCKER_POSTROUTING
-A POSTROUTING -s 10.244.0.2/32 -m comment --comment "name: \"bridge\" id: \"f0e76026b7b3fed59044e7e8459d9373c9a95099ff7c785ab76db944a98c2a10\"" -j CNI-430e1a042a914a692a49a13c

-A CNI-430e1a042a914a692a49a13c -d 10.244.0.0/16 -m comment --comment "name: \"bridge\" id: \"f0e76026b7b3fed59044e7e8459d9373c9a95099ff7c785ab76db944a98c2a10\"" -j ACCEPT
-A CNI-430e1a042a914a692a49a13c ! -d 224.0.0.0/4 -m comment --comment "name: \"bridge\" id: \"f0e76026b7b3fed59044e7e8459d9373c9a95099ff7c785ab76db944a98c2a10\"" -j MASQUERADE
-A DOCKER -i docker0 -j RETURN
-A DOCKER_OUTPUT -d 192.168.49.1/32 -p tcp -m tcp --dport 53 -j DNAT --to-destination 127.0.0.11:37849
-A DOCKER_OUTPUT -d 192.168.49.1/32 -p udp -m udp --dport 53 -j DNAT --to-destination 127.0.0.11:40300
-A DOCKER_POSTROUTING -s 127.0.0.11/32 -p tcp -j SNAT --to-source 192.168.49.1:53
-A DOCKER_POSTROUTING -s 127.0.0.11/32 -p udp -j SNAT --to-source 192.168.49.1:53
-A KUBE-MARK-MASQ -j MARK --set-xmark 0x4000/0x4000
-A KUBE-POSTROUTING -m mark ! --mark 0x4000/0x4000 -j RETURN
-A KUBE-POSTROUTING -j MARK --set-xmark 0x4000/0x0
-A KUBE-POSTROUTING -m comment --comment "kubernetes service traffic requiring SNAT" -j MASQUERADE --random-fully
-A KUBE-SEP-IT2ZTR26TO4XFPTO -s 10.244.0.2/32 -m comment --comment "kube-system/kube-dns:dns-tcp" -j KUBE-MARK-MASQ
-A KUBE-SEP-IT2ZTR26TO4XFPTO -p tcp -m comment --comment "kube-system/kube-dns:dns-tcp" -m tcp -j DNAT --to-destination 10.244.0.2:53
-A KUBE-SEP-N4G2XR5TDX7PQE7P -s 10.244.0.2/32 -m comment --comment "kube-system/kube-dns:metrics" -j KUBE-MARK-MASQ
-A KUBE-SEP-N4G2XR5TDX7PQE7P -p tcp -m comment --comment "kube-system/kube-dns:metrics" -m tcp -j DNAT --to-destination 10.244.0.2:9153
-A KUBE-SEP-VPILYQBSPPXYB66K -s 192.168.49.2/32 -m comment --comment "default/kubernetes:https" -j KUBE-MARK-MASQ
-A KUBE-SEP-VPILYQBSPPXYB66K -p tcp -m comment --comment "default/kubernetes:https" -m tcp -j DNAT --to-destination 192.168.49.2:8443
-A KUBE-SEP-YIL6JZP7A3QYXJU2 -s 10.244.0.2/32 -m comment --comment "kube-system/kube-dns:dns" -j KUBE-MARK-MASQ
-A KUBE-SEP-YIL6JZP7A3QYXJU2 -p udp -m comment --comment "kube-system/kube-dns:dns" -m udp -j DNAT --to-destination 10.244.0.2:53
-A KUBE-SERVICES -d 10.96.0.1/32 -p tcp -m comment --comment "default/kubernetes:https cluster IP" -m tcp --dport 443 -j KUBE-SVC-NPX46M4PTMTKRN6Y
-A KUBE-SERVICES -d 10.96.0.10/32 -p tcp -m comment --comment "kube-system/kube-dns:metrics cluster IP" -m tcp --dport 9153 -j KUBE-SVC-JD5MR3NA4I4DYORP
-A KUBE-SERVICES -d 10.96.0.10/32 -p udp -m comment --comment "kube-system/kube-dns:dns cluster IP" -m udp --dport 53 -j KUBE-SVC-TCOU7JCQXEZGVUNU
-A KUBE-SERVICES -d 10.96.0.10/32 -p tcp -m comment --comment "kube-system/kube-dns:dns-tcp cluster IP" -m tcp --dport 53 -j KUBE-SVC-ERIFXISQEP7F7OF4
-A KUBE-SERVICES -m comment --comment "kubernetes service nodeports; NOTE: this must be the last rule in this chain" -m addrtype --dst-type LOCAL -j KUBE-NODEPORTS
-A KUBE-SVC-ERIFXISQEP7F7OF4 ! -s 10.244.0.0/16 -d 10.96.0.10/32 -p tcp -m comment --comment "kube-system/kube-dns:dns-tcp cluster IP" -m tcp --dport 53 -j KUBE-MARK-MASQ
-A KUBE-SVC-ERIFXISQEP7F7OF4 -m comment --comment "kube-system/kube-dns:dns-tcp -> 10.244.0.2:53" -j KUBE-SEP-IT2ZTR26TO4XFPTO
-A KUBE-SVC-JD5MR3NA4I4DYORP ! -s 10.244.0.0/16 -d 10.96.0.10/32 -p tcp -m comment --comment "kube-system/kube-dns:metrics cluster IP" -m tcp --dport 9153 -j KUBE-MARK-MASQ
-A KUBE-SVC-JD5MR3NA4I4DYORP -m comment --comment "kube-system/kube-dns:metrics -> 10.244.0.2:9153" -j KUBE-SEP-N4G2XR5TDX7PQE7P
-A KUBE-SVC-NPX46M4PTMTKRN6Y ! -s 10.244.0.0/16 -d 10.96.0.1/32 -p tcp -m comment --comment "default/kubernetes:https cluster IP" -m tcp --dport 443 -j KUBE-MARK-MASQ
-A KUBE-SVC-NPX46M4PTMTKRN6Y -m comment --comment "default/kubernetes:https -> 192.168.49.2:8443" -j KUBE-SEP-VPILYQBSPPXYB66K
-A KUBE-SVC-TCOU7JCQXEZGVUNU ! -s 10.244.0.0/16 -d 10.96.0.10/32 -p udp -m comment --comment "kube-system/kube-dns:dns cluster IP" -m udp --dport 53 -j KUBE-MARK-MASQ
-A KUBE-SVC-TCOU7JCQXEZGVUNU -m comment --comment "kube-system/kube-dns:dns -> 10.244.0.2:53" -j KUBE-SEP-YIL6JZP7A3QYXJU2
COMMIT
# Completed on Sat Dec  9 11:26:25 2023
```

RETURN - пакет перестанет путешествовать по цепочке, в которой он попал в правило. Если это подцепочка другой цепочки, пакет продолжит движение по вышестоящим цепочкам, как будто ничего не произошло
ACCEPT - пакет будет принят и не будет продолжать обход текущей цепочки или других цепочек в той же таблице
MASQUERADE - у пакета переписывается IP-адресс отправителя на IP-адресс сетевого интерфейса 
MARK - для пакета  устанавливается значение метки Netfilter
DNAT - у пакета переписывается IP-адрес назначения
SNAT - у пакета переписывается IP-адрес отправителя
DROP - пакет будет заблокирован, не будет никакой дальнейшей обработки.

NEW - пакет инициирует новое соединение, либо связан с соединением, по которому ещё не проходили пакеты в обоих направлениях
RELATED - пакет инициирует новое соединение, но также связан с уже существующим соединением, например при передаче данных по FTP или сообщении об ошибке ICMP
ESTABLISHED - пакет связан с соединением, по которому уже проходили пакеты в обоих направлениях
DNAT - исходный адрес получателя отличен от адреса отправителя ответа
INVALID - пакет не связан с каким-либо известным соединением

PREROUTING -  входящие пакеты
OUTPUT - локально сгенерированные пакеты перед их отправлением
POSTROUTING - все исходящие пакеты