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
