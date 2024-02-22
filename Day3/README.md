# Day3

## Info - Troubleshooting NFS mount issues

Reference - https://docs.openshift.com/container-platform/4.14/storage/persistent_storage/persistent-storage-nfs.html

In case your Pods are unable to mount NFS shared folder, you need to enable nfs mounting within Openshift nodes as shown below

```
oc debug node/master-1.ocp4.training.tektutor
chroot /host /bin/bash
setsebool -P virt_use_nfs 1
exit

oc debug node/master-2.ocp4.training.tektutor
chroot /host /bin/bash
setsebool -P virt_use_nfs 1
exit

oc debug node/master-3.ocp4.training.tektutor
chroot /host /bin/bash
setsebool -P virt_use_nfs 1
exit

oc debug node/worker-1.ocp4.training.tektutor
chroot /host /bin/bash
setsebool -P virt_use_nfs 1
exit

oc debug node/worker-2.ocp4.training.tektutor
chroot /host /bin/bash
setsebool -P virt_use_nfs 1
exit
```

Expected output
<pre>
[jegan@tektutor.org wordpress-with-configmap-and-secrets]$ oc debug node/master-2.ocp4.training.tektutor
Temporary namespace openshift-debug-7zzkh is created for debugging node...
Starting pod/master-2ocp4trainingtektutor-debug-g8hj7 ...
To use host binaries, run `chroot /host`
chroot /host /bin/bash
Pod IP: 192.168.122.123
If you don't see a command prompt, try pressing enter.
sh-4.4# chroot /host /bin/bash
[root@master-2 /]# setsebool -P virt_use_nfs 1
[root@master-2 /]# exit
exit
sh-4.4# exit
exit

Removing debug pod ...
Temporary namespace openshift-debug-7zzkh was removed.
</pre>

## Info - In case you are curious to see the containers running inside openshift nodes
```
oc debug node/worker-1.ocp4.training.tektutor
chroot /host /bin/bash
podman version
podman info
crictl ps -a
```

Expected output
<pre>
[jegan@tektutor.org wordpress-with-configmap-and-secrets]$ oc debug node/worker-1.ocp4.training.tektutor
Temporary namespace openshift-debug-cpxmp is created for debugging node...
Starting pod/worker-1ocp4trainingtektutor-debug-gr4q9 ...
To use host binaries, run `chroot /host`
Pod IP: 192.168.122.206
If you don't see a command prompt, try pressing enter.
sh-4.4# chroot /host /bin/bash
  
[root@worker-1 /]# podman version
Client:       Podman Engine
Version:      4.4.1
API Version:  4.4.1
Go Version:   go1.20.12
Built:        Thu Feb  8 05:04:04 2024
OS/Arch:      linux/amd64
[root@worker-1 /]# podman info
host:
  arch: amd64
  buildahVersion: 1.29.0
  cgroupControllers:
  - cpuset
  - cpu
  - io
  - memory
  - hugetlb
  - pids
  - rdma
  - misc
  cgroupManager: systemd
  cgroupVersion: v2
  conmon:
    package: conmon-2.1.7-3.2.rhaos4.14.el9.x86_64
    path: /usr/bin/conmon
    version: 'conmon version 2.1.7, commit: 0cfc49682f4e9723c654c26ad6be1a3aacd7dcea'
  cpuUtilization:
    idlePercent: 99.12
    systemPercent: 0.29
    userPercent: 0.59
  cpus: 8
  distribution:
    distribution: '"rhcos"'
    variant: coreos
    version: "4.14"
  eventLogger: journald
  hostname: worker-1.ocp4.training.tektutor
  idMappings:
    gidmap: null
    uidmap: null
  kernel: 5.14.0-284.52.1.el9_2.x86_64
  linkmode: dynamic
  logDriver: journald
  memFree: 8187338752
  memTotal: 16375459840
  networkBackend: cni
  ociRuntime:
    name: crun
    package: crun-1.14-1.rhaos4.14.el9.x86_64
    path: /usr/bin/crun
    version: |-
      crun version 1.14
      commit: 667e6ebd4e2442d39512e63215e79d693d0780aa
      rundir: /run/crun
      spec: 1.0.0
      +SYSTEMD +SELINUX +APPARMOR +CAP +SECCOMP +EBPF +CRIU +YAJL
  os: linux
  remoteSocket:
    path: /run/podman/podman.sock
  security:
    apparmorEnabled: false
    capabilities: CAP_CHOWN,CAP_DAC_OVERRIDE,CAP_FOWNER,CAP_FSETID,CAP_KILL,CAP_NET_BIND_SERVICE,CAP_SETFCAP,CAP_SETGID,CAP_SETPCAP,CAP_SETUID
    rootless: false
    seccompEnabled: true
    seccompProfilePath: /usr/share/containers/seccomp.json
    selinuxEnabled: true
  serviceIsRemote: false
  slirp4netns:
    executable: /usr/bin/slirp4netns
    package: slirp4netns-1.2.0-3.el9.x86_64
    version: |-
      slirp4netns version 1.2.0
      commit: 656041d45cfca7a4176f6b7eed9e4fe6c11e8383
      libslirp: 4.4.0
      SLIRP_CONFIG_VERSION_MAX: 3
      libseccomp: 2.5.2
  swapFree: 0
  swapTotal: 0
  uptime: 2h 27m 29.00s (Approximately 0.08 days)
plugins:
  authorization: null
  log:
  - k8s-file
  - none
  - passthrough
  - journald
  network:
  - bridge
  - macvlan
  - ipvlan
  volume:
  - local
registries:
  search:
  - registry.access.redhat.com
  - docker.io
store:
  configFile: /etc/containers/storage.conf
  containerStore:
    number: 1
    paused: 0
    running: 0
    stopped: 1
  graphDriverName: overlay
  graphOptions:
    overlay.skip_mount_home: "true"
  graphRoot: /var/lib/containers/storage
  graphRootAllocated: 53082042368
  graphRootUsed: 10045403136
  graphStatus:
    Backing Filesystem: xfs
    Native Overlay Diff: "true"
    Supports d_type: "true"
    Using metacopy: "false"
  imageCopyTmpDir: /var/tmp
  imageStore:
    number: 23
  runRoot: /run/containers/storage
  transientStore: false
  volumePath: /var/lib/containers/storage/volumes
version:
  APIVersion: 4.4.1
  Built: 1707368644
  BuiltTime: Thu Feb  8 05:04:04 2024
  GitCommit: ""
  GoVersion: go1.20.12
  Os: linux
  OsArch: linux/amd64
  Version: 4.4.1

[root@worker-1 /]# crictl ps -a
CONTAINER           IMAGE                                                                                                                    CREATED             STATE               NAME                                 ATTEMPT             POD ID              POD
ecc1a7baeadee       0e06c0f2c42bd0c734b0f48d16df04e7b97128f4426abe2d7089b5bac0d578a4                                                         11 minutes ago      Running             container-00                         0                   8bf530cdf3a93       worker-1ocp4trainingtektutor-debug-gr4q9
cbdf81deaa10b       cd08464f8e6e5145736152e423985a40b51dad242b9f407192fe76ff64d737a6                                                         23 minutes ago      Running             wordpress                            0                   62e243d3ab13d       wordpress-658fc8c5b9-2mqwr
9442b68ada46e       quay.io/openshift-release-dev/ocp-v4.0-art-dev@sha256:8b27c7da20a626e7ff25028beeb1022b7def370f67b7a8fcd384a7645a94706e   2 hours ago         Running             network-check-target-container       0                   b72264e779c39       network-check-target-tljqs
deba86b5d8145       b80ae5477b314111e459b66db252527ca8fc090d98b99fe1465c3a3f439c942d                                                         2 hours ago         Running             kube-rbac-proxy                      0                   d3c28cff27228       network-metrics-daemon-2g8dp
8af14733dc02a       quay.io/openshift-release-dev/ocp-v4.0-art-dev@sha256:693fef5f95dd1facfdcb6df3514a001a425a6939676a53406734c3e49ebdbecb   2 hours ago         Running             network-metrics-daemon               0                   d3c28cff27228       network-metrics-daemon-2g8dp
dd0aed94ef61b       8cd3b7da296077275c861caf6b36866bfbc99a55c6f4b8bbac8ac46585a6582e                                                         2 hours ago         Running             kube-multus-additional-cni-plugins   0                   dcf97b2e6093f       multus-additional-cni-plugins-dkkrd
cdd218c096a82       73045715f6c5d4a8e20a069621417bcb14cb6487579e2d9017b724037a3a33cd                                                         2 hours ago         Exited              whereabouts-cni                      0                   dcf97b2e6093f       multus-additional-cni-plugins-dkkrd
7cee0a34831b3       quay.io/openshift-release-dev/ocp-v4.0-art-dev@sha256:0ca8943e16c7d52f1be259061170c7f666c431d1d34ccbd64aea38d4735de45c   2 hours ago         Exited              whereabouts-cni-bincopy              0                   dcf97b2e6093f       multus-additional-cni-plugins-dkkrd
407fcd19d8bfc       quay.io/openshift-release-dev/ocp-v4.0-art-dev@sha256:930b5bd27584df4fb521c5ec0377603e51f1d633a3f99dc3d3a833fd600fc35a   2 hours ago         Running             serve-healthcheck-canary             0                   03477eb55f75f       ingress-canary-qmlkc
e91fc52fa7512       b80ae5477b314111e459b66db252527ca8fc090d98b99fe1465c3a3f439c942d                                                         2 hours ago         Running             kube-rbac-proxy                      0                   fd0cce7c593ea       dns-default-9b54c
db8e84f475674       quay.io/openshift-release-dev/ocp-v4.0-art-dev@sha256:6b38d75b297fa52d1ba29af0715cec2430cd5fda1a608ed0841a09c55c292fb3   2 hours ago         Running             dns                                  0                   fd0cce7c593ea       dns-default-9b54c
f6f95c09fc18b       quay.io/openshift-release-dev/ocp-v4.0-art-dev@sha256:198f5a31c424606b1ea67581b132a51efe71c416c56ec7652f8fd445af44773f   2 hours ago         Exited              routeoverride-cni                    0                   dcf97b2e6093f       multus-additional-cni-plugins-dkkrd
d0ce549b8929e       quay.io/openshift-release-dev/ocp-v4.0-art-dev@sha256:7afae9702bc149efe67fb2090704b0d57998b7f656c11d93a45f9a11a235c4b3   2 hours ago         Exited              bond-cni-plugin                      0                   dcf97b2e6093f       multus-additional-cni-plugins-dkkrd
f02cfa9127ae4       quay.io/openshift-release-dev/ocp-v4.0-art-dev@sha256:38ff2b79106b412567008136569e1fe9eccab3b6eae7b19d8c47c9bc2cf1f99a   2 hours ago         Exited              cni-plugins                          0                   dcf97b2e6093f       multus-additional-cni-plugins-dkkrd
51797c9b5c19e       quay.io/openshift-release-dev/ocp-v4.0-art-dev@sha256:27fb71a6cbae202f99d429f625a10a1bb30ebd3d79e7379297bd01d4bda25e75   2 hours ago         Running             kube-multus                          0                   a23ee6af46612       multus-9kpbs
8a039f96cd110       b80ae5477b314111e459b66db252527ca8fc090d98b99fe1465c3a3f439c942d                                                         2 hours ago         Running             kube-rbac-proxy                      0                   4ec7ee120c29e       sdn-df86w
0cf91799745bb       quay.io/openshift-release-dev/ocp-v4.0-art-dev@sha256:28a764758233131ef71261efc1d9aa1050e5aa28d348a0f421e013d1ffb3fdc5   2 hours ago         Running             tuned                                0                   6f62d4d97093b       tuned-h4b8v
e1b4843e90128       quay.io/openshift-release-dev/ocp-v4.0-art-dev@sha256:ac59d66362f1cebe713714e3ffff83a1d13b63997a6f29c921d02153e3318dc3   2 hours ago         Running             sdn                                  0                   4ec7ee120c29e       sdn-df86w
625727614d826       quay.io/openshift-release-dev/ocp-v4.0-art-dev@sha256:d7a26cf7bc38061639ee3b8515bc2a8cb8c64f5ebbb70250dd43e633222cb19c   2 hours ago         Running             dns-node-resolver                    0                   ee4158e4ee23b       node-resolver-7kdbr
7421367a30a27       quay.io/openshift-release-dev/ocp-v4.0-art-dev@sha256:0437b6e70a2364cca080fe64e5b481a1f397b433f6e188423c9e47512120c178   2 hours ago         Running             node-ca                              0                   6a6fe64808783       node-ca-5btz8
6ec9a9a4205a3       quay.io/openshift-release-dev/ocp-v4.0-art-dev@sha256:bae1c43ad895bf85ea535042f71842b60148a25a0e356a081913bf67c83f5855   2 hours ago         Exited              egress-router-binary-copy            0                   dcf97b2e6093f       multus-additional-cni-plugins-dkkrd
35d30a270c1c5       quay.io/openshift-release-dev/ocp-v4.0-art-dev@sha256:b3cc4a7529a6c70b983803c59ee744c5a0a989ca6f8c6eab15d041d96399df88   2 hours ago         Running             kube-rbac-proxy                      0                   3ad9366847070       machine-config-daemon-nv8cs
737329b3e0875       quay.io/openshift-release-dev/ocp-v4.0-art-dev@sha256:b3cc4a7529a6c70b983803c59ee744c5a0a989ca6f8c6eab15d041d96399df88   2 hours ago         Running             kube-rbac-proxy                      0                   0e644968e91ec       node-exporter-bs6bl
028b82500e031       65d35907a90182d763291f5acdb1fa709b61082dba129dcd73b409a94f9d1df5                                                         2 hours ago         Running             node-exporter                        0                   0e644968e91ec       node-exporter-bs6bl
017a754b11670       quay.io/openshift-release-dev/ocp-v4.0-art-dev@sha256:33835833fc124d5220af83ef4b85338c2bee24daee48eb0f775dfb39a742acd6   2 hours ago         Exited              init-textfile                        0                   0e644968e91ec       node-exporter-bs6bl
1d649dbb6828c       51b5d7bfff504a6a1eb57616ae304482352cce4c105b4234b4816215534cd4ef                                                         2 hours ago         Running             machine-config-daemon                0                   3ad9366847070       machine-config-daemon-nv8cs
[root@worker-1 /]# crictl ps -a | grep wordpress
cbdf81deaa10b       cd08464f8e6e5145736152e423985a40b51dad242b9f407192fe76ff64d737a6                                                         23 minutes ago      Running             wordpress                            0                   62e243d3ab13d       wordpress-658fc8c5b9-2mqwr  
</pre>

## 

## Deploying Wordpress and MySql multi-pod application
```
cd ~/openshift-feb-2024
git pull
cd Day3/wordpress

oc apply -f mysql-pv.yml
oc apply -f mysql-pvc.yml
oc apply -f mysql-deploy.yml
oc apply -f mysql-svc.yml

oc apply -f wordpress-pv.yml
oc apply -f wordpress-pvc.yml
oc apply -f wordpress-deploy.yml
oc apply -f wordpress-svc.yml
oc apply -f wordpress-route.yml
```

## Deploying Wordpress, MySql multi-pod application with configmaps and secrets
```
cd ~/openshift-feb-2024
git pull
cd Day3/wordpress

oc apply -f wordpress-cm.yml
oc apply -f wordpress-secrets.yml

oc apply -f mysql-pv.yml
oc apply -f mysql-pvc.yml
oc apply -f mysql-deploy.yml
oc apply -f mysql-svc.yml

oc apply -f wordpress-pv.yml
oc apply -f wordpress-pvc.yml
oc apply -f wordpress-deploy.yml
oc apply -f wordpress-svc.yml
oc apply -f wordpress-route.yml
```

In case if wish to run the script to avoid typing the above commands
```
cd ~/openshift-feb-2024
git pull
cd Day3/wordpress
./create-all.sh
```

Once you are done with the lab exercise, kindly delete the deployments as shown below
```
cd ~/openshift-feb-2024
git pull
cd Day3/wordpress
./delete-all.sh
```

## Lab - Installing Helm in your RPS CentOS Lab Machine
```
curl -fsSL -o get_helm.sh https://raw.githubusercontent.com/helm/helm/main/scripts/get-helm-3
chmod 700 get_helm.sh
./get_helm.sh
```

Expected output
<pre>
[jegan@tektutor.org Day3]$ curl -fsSL -o get_helm.sh https://raw.githubusercontent.com/helm/helm/main/scripts/get-helm-3
chmod 700 get_helm.sh
./get_helm.sh
Downloading https://get.helm.sh/helm-v3.14.2-linux-amd64.tar.gz
Verifying checksum... Done.
Preparing to install helm into /usr/local/bin
[sudo] password for jegan: 
helm installed into /usr/local/bin/helm
  
[jegan@tektutor.org Day3]$ helm version
WARNING: Kubernetes configuration file is group-readable. This is insecure. Location: /home/jegan/.kube/config
version.BuildInfo{Version:"v3.14.2", GitCommit:"c309b6f0ff63856811846ce18f3bdc93d2b4d54b", GitTreeState:"clean", GoVersion:"go1.21.7"}  
</pre>

## Lab - Deploying MongoDB into OpenShift

Let's first deploy mongodb 
```
cd ~/openshift-feb-2024
git pull
cd Day3/mongodb
./create-all.sh
```

Let's create a mongodb client pod
```
kubectl run --namespace jegan mongodb-client --rm --tty -i --restart='Never' --env="MONGODB_ROOT_PASSWORD=root@123" --image docker.io/bitnami/mongodb:7.0.5-debian-12-r4 --command -- bash

mongosh mongodb.jegan.svc.cluster.local -u root -p
```

Expected output
```
[jegan@tektutor.org mongodb]$ kubectl run --namespace jegan mongodb-client --rm --tty -i --restart='Never' --env="MONGODB_ROOT_PASSWORD=root@123" --image docker.io/bitnami/mongodb:7.0.5-debian-12-r4 --command -- bash
Warning: would violate PodSecurity "restricted:v1.24": allowPrivilegeEscalation != false (container "mongodb-client" must set securityContext.allowPrivilegeEscalation=false), unrestricted capabilities (container "mongodb-client" must set securityContext.capabilities.drop=["ALL"]), runAsNonRoot != true (pod or container "mongodb-client" must set securityContext.runAsNonRoot=true), seccompProfile (pod or container "mongodb-client" must set securityContext.seccompProfile.type to "RuntimeDefault" or "Localhost")
If you don't see a command prompt, try pressing enter.

1001@mongodb-client:/$ mongosh mongodb.jegan.svc.cluster.local -u admin -p
Enter password: ********
Current Mongosh Log ID:	65d6e9463e7a5c09894c82d8
Connecting to:		mongodb://<credentials>@mongodb.jegan.svc.cluster.local:27017/?directConnection=true&appName=mongosh+2.1.5
MongoServerError: Authentication failed.
1001@mongodb-client:/$ mongosh tektutor@mongodb.jegan.svc.cluster.local -u admin -p
MongoshInvalidInputError: [COMMON-10001] Invalid URI: tektutor@mongodb.jegan.svc.cluster.local
1001@mongodb-client:/$ mongosh --host tektutor@mongodb.jegan.svc.cluster.local -u admin -p
MongoshInvalidInputError: [COMMON-10001] The --host argument contains an invalid character: @
1001@mongodb-client:/$ mongosh tektutor@mongodb.jegan.svc.cluster.local -u root -p
MongoshInvalidInputError: [COMMON-10001] Invalid URI: tektutor@mongodb.jegan.svc.cluster.local
1001@mongodb-client:/$ mongosh mongodb.jegan.svc.cluster.local -u root -p
Enter password: ********
Current Mongosh Log ID:	65d6ea1748cd6e041aa51179
Connecting to:		mongodb://<credentials>@mongodb.jegan.svc.cluster.local:27017/?directConnection=true&appName=mongosh+2.1.5
Using MongoDB:		7.0.5
Using Mongosh:		2.1.5

For mongosh info see: https://docs.mongodb.com/mongodb-shell/


To help improve our products, anonymous usage data is collected and sent to MongoDB periodically (https://www.mongodb.com/legal/privacy-policy).
You can opt-out by running the disableTelemetry() command.

------
   The server generated these startup warnings when booting
   2024-02-22T05:39:33.595+00:00: /sys/kernel/mm/transparent_hugepage/enabled is 'always'. We suggest setting it to 'never'
   2024-02-22T05:39:33.595+00:00: vm.max_map_count is too low
------

test> 
```



```

