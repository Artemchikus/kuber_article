4026534267 mnt         1  6705 65535 /pause
4026534268 uts         1  6705 65535 /pause
4026534269 ipc         4  6705 65535 /pause
4026534270 pid         1  6705 65535 /pause
4026534271 net         4  6705 65535 /pause
4026534341 cgroup      1  6705 65535 /pause
4026534344 mnt         3  6922 root  /usr/bin/dumb-init /bin/sh /usr/local/bin/docker-entrypoint.sh server -dev
4026534345 uts         3  6922 root  /usr/bin/dumb-init /bin/sh /usr/local/bin/docker-entrypoint.sh server -dev
4026534346 pid         3  6922 root  /usr/bin/dumb-init /bin/sh /usr/local/bin/docker-entrypoint.sh server -dev
4026534347 cgroup      3  6922 root  /usr/bin/dumb-init /bin/sh /usr/local/bin/docker-entrypoint.sh server -dev

ps uax
PID   USER     TIME  COMMAND
    1 root      0:00 {docker-entrypoi} /usr/bin/dumb-init /bin/sh /usr/local/bin/docker-entrypoint.sh server -dev
    7 vault     0:01 vault server -config=/vault/config -dev-root-token-id= -dev-listen-address=0.0.0.0:8200 -dev
   34 root      0:00 sh
   43 root      0:00 ps uax

docker ps
CONTAINER ID   IMAGE                       COMMAND                  CREATED         STATUS         PORTS     NAMES
596fbd56d9aa   vault                       "docker-entrypoint.s…"   3 minutes ago   Up 3 minutes             k8s_vault_vault_default_e7f78f2e-3935-4b2d-9360-21781a472876_0
66d72da7ea45   registry.k8s.io/pause:3.9   "/pause"                 3 minutes ago   Up 3 minutes             k8s_POD_vault_default_e7f78f2e-3935-4b2d-9360-21781a472876_0
a11bd09751c6   6848d7eda034                "/usr/local/bin/kube…"   6 minutes ago   Up 6 minutes             k8s_kube-proxy_kube-proxy-mvzlw_kube-system_25e06ff7-86b1-47b0-a008-c9df810a4faf_0
a2769b0db40e   registry.k8s.io/pause:3.9   "/pause"                 6 minutes ago   Up 6 minutes             k8s_POD_kube-proxy-mvzlw_kube-system_25e06ff7-86b1-47b0-a008-c9df810a4faf_0
8037529ddabc   6e38f40d628d                "/storage-provisioner"   7 minutes ago   Up 7 minutes             k8s_storage-provisioner_storage-provisioner_kube-system_5da5d460-beb9-470b-9b83-17e82e86e8a1_1
b5e343bc7c3c   ead0a4a53df8                "/coredns -conf /etc…"   8 minutes ago   Up 8 minutes             k8s_coredns_coredns-5d78c9869d-49fbz_kube-system_4efe6215-5693-4a62-b265-b74d03443057_0
095628f69b85   registry.k8s.io/pause:3.9   "/pause"                 8 minutes ago   Up 8 minutes             k8s_POD_coredns-5d78c9869d-49fbz_kube-system_4efe6215-5693-4a62-b265-b74d03443057_0
00b7b8becd2b   registry.k8s.io/pause:3.9   "/pause"                 8 minutes ago   Up 8 minutes             k8s_POD_storage-provisioner_kube-system_5da5d460-beb9-470b-9b83-17e82e86e8a1_0
8ce05f17e730   e7972205b661                "kube-apiserver --ad…"   8 minutes ago   Up 8 minutes             k8s_kube-apiserver_kube-apiserver-minikube_kube-system_5e0f0a31366f39ecc73732c27a3c3b71_0
7de43abba2a8   86b6af7dd652                "etcd --advertise-cl…"   8 minutes ago   Up 8 minutes             k8s_etcd_etcd-minikube_kube-system_31b85d1347b7ac6c29f53ae2fc84fd90_0
f7f8c98579b3   98ef2570f3cd                "kube-scheduler --au…"   8 minutes ago   Up 8 minutes             k8s_kube-scheduler_kube-scheduler-minikube_kube-system_217307423dbcf2998fde272a9c85bfbb_0
8c2fdc3795f5   f466468864b7                "kube-controller-man…"   8 minutes ago   Up 8 minutes             k8s_kube-controller-manager_kube-controller-manager-minikube_kube-system_ea783d51171557261265d2435b4c5eec_0
7082ac1e75c8   registry.k8s.io/pause:3.9   "/pause"                 8 minutes ago   Up 8 minutes             k8s_POD_kube-scheduler-minikube_kube-system_217307423dbcf2998fde272a9c85bfbb_0
bc0021631645   registry.k8s.io/pause:3.9   "/pause"                 8 minutes ago   Up 8 minutes             k8s_POD_kube-controller-manager-minikube_kube-system_ea783d51171557261265d2435b4c5eec_0
c3ea587d5af4   registry.k8s.io/pause:3.9   "/pause"                 8 minutes ago   Up 8 minutes             k8s_POD_kube-apiserver-minikube_kube-system_5e0f0a31366f39ecc73732c27a3c3b71_0
63e43e4a6726   registry.k8s.io/pause:3.9   "/pause"                 8 minutes ago   Up 8 minutes             k8s_POD_etcd-minikube_kube-system_31b85d1347b7ac6c29f53ae2fc84fd90_0

ip a
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN qlen 1000
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
    inet 127.0.0.1/8 scope host lo
       valid_lft forever preferred_lft forever
2: eth0@if5: <BROADCAST,MULTICAST,UP,LOWER_UP,M-DOWN> mtu 1500 qdisc noqueue state UP 
    link/ether 4e:e6:31:f3:81:a8 brd ff:ff:ff:ff:ff:ff
    inet 10.244.0.3/16 brd 10.244.255.255 scope global eth0
       valid_lft forever preferred_lft forever
/ # ip r
default via 10.244.0.1 dev eth0 
10.244.0.0/16 dev eth0 scope link  src 10.244.0.3

root        6901  0.0  0.0 720504 10728 ?        Sl   15:19   0:00 /usr/bin/containerd-shim-runc-v2 -namespace moby -id 596fbd56d9aaffb980c97
root        6922  0.0  0.0    216     0 ?        Ss   15:19   0:00  \\_ /usr/bin/dumb-init /bin/sh /usr/local/bin/docker-entrypoint.sh server 
_apt        6935  0.3  0.3 889160 114276 ?       Ssl  15:19   0:02      \\_ vault server -config=/vault/config -dev-root-token-id= -dev-listen

apt = vault один id

mount
overlay on / type overlay (rw,seclabel,relatime,lowerdir=/var/lib/docker/overlay2/l/HADIUDBRK6K6CA2EAYIPIN54CF:/var/lib/docker/overlay2/l/ZEDDRHSJ5IEREEVGU2PV2YSNGI:/var/lib/docker/overlay2/l/DEEBZ4NIHQEJX7VSYESA2YWCAJ:/var/lib/docker/overlay2/l/CPE4IDREOEF54DX2XOUTMT3U2S:/var/lib/docker/overlay2/l/FQBHQ3BPW6VV3YPG4WEKW4WMEB:/var/lib/docker/overlay2/l/6IPXNYC6WPK4IGINXKMNVIQ4BH,upperdir=/var/lib/docker/overlay2/443259d564638ef254e193583dbc23bd5e5a49f5930dd710edff45177454cc9a/diff,workdir=/var/lib/docker/overlay2/443259d564638ef254e193583dbc23bd5e5a49f5930dd710edff45177454cc9a/work)
proc on /proc type proc (rw,nosuid,nodev,noexec,relatime)
tmpfs on /dev type tmpfs (rw,seclabel,nosuid,size=65536k,mode=755,inode64)
devpts on /dev/pts type devpts (rw,seclabel,nosuid,noexec,relatime,gid=5,mode=620,ptmxmode=666)
sysfs on /sys type sysfs (ro,seclabel,nosuid,nodev,noexec,relatime)
cgroup on /sys/fs/cgroup type cgroup2 (ro,seclabel,nosuid,nodev,noexec,relatime,nsdelegate,memory_recursiveprot)
mqueue on /dev/mqueue type mqueue (rw,seclabel,nosuid,nodev,noexec,relatime)
/dev/nvme0n1p3 on /dev/termination-log type btrfs (rw,seclabel,relatime,compress=zstd:1,ssd,discard=async,space_cache=v2,subvolid=257,subvol=/root)
/dev/nvme0n1p3 on /vault/file type btrfs (rw,seclabel,relatime,compress=zstd:1,ssd,discard=async,space_cache=v2,subvolid=257,subvol=/root)
/dev/nvme0n1p3 on /vault/logs type btrfs (rw,seclabel,relatime,compress=zstd:1,ssd,discard=async,space_cache=v2,subvolid=257,subvol=/root)
/dev/nvme0n1p3 on /etc/resolv.conf type btrfs (rw,seclabel,relatime,compress=zstd:1,ssd,discard=async,space_cache=v2,subvolid=257,subvol=/root)
/dev/nvme0n1p3 on /etc/hostname type btrfs (rw,seclabel,relatime,compress=zstd:1,ssd,discard=async,space_cache=v2,subvolid=257,subvol=/root)
/dev/nvme0n1p3 on /etc/hosts type btrfs (rw,seclabel,relatime,compress=zstd:1,ssd,discard=async,space_cache=v2,subvolid=257,subvol=/root)
shm on /dev/shm type tmpfs (rw,seclabel,nosuid,nodev,noexec,relatime,size=65536k,inode64)
tmpfs on /run/secrets/kubernetes.io/serviceaccount type tmpfs (ro,seclabel,relatime,size=32756628k,inode64)
proc on /proc/bus type proc (ro,nosuid,nodev,noexec,relatime)
proc on /proc/fs type proc (ro,nosuid,nodev,noexec,relatime)
proc on /proc/irq type proc (ro,nosuid,nodev,noexec,relatime)
proc on /proc/sys type proc (ro,nosuid,nodev,noexec,relatime)
proc on /proc/sysrq-trigger type proc (ro,nosuid,nodev,noexec,relatime)
tmpfs on /proc/asound type tmpfs (ro,seclabel,relatime,inode64)
tmpfs on /proc/acpi type tmpfs (ro,seclabel,relatime,inode64)
tmpfs on /proc/kcore type tmpfs (rw,seclabel,nosuid,size=65536k,mode=755,inode64)
tmpfs on /proc/keys type tmpfs (rw,seclabel,nosuid,size=65536k,mode=755,inode64)
tmpfs on /proc/latency_stats type tmpfs (rw,seclabel,nosuid,size=65536k,mode=755,inode64)
tmpfs on /proc/timer_list type tmpfs (rw,seclabel,nosuid,size=65536k,mode=755,inode64)
tmpfs on /proc/scsi type tmpfs (ro,seclabel,relatime,inode64)
tmpfs on /sys/firmware type tmpfs (ro,seclabel,relatime,inode64)

bridge link show
4: veth3a1d4011@docker0: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 master bridge state forwarding priority 32 cost 2 
5: veth41e3a79e@docker0: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 master bridge state forwarding priority 32 cost 2 

ip neig
10.244.0.2 dev bridge lladdr 4a:c2:7c:e0:60:09 REACHABLE
192.168.49.1 dev eth0 lladdr 02:42:fc:6e:9e:42 REACHABLE
10.244.0.3 dev bridge lladdr 1e:96:85:81:1d:07 REACHABLE
