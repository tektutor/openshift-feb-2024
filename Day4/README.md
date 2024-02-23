# Day 4

## Lab - Deploying Hello Microservice application from source code using github code repository url
```
oc project jegan
oc new-app https://github.com/tektutor/spring-ms.git --strategy=docker
oc logs -f buildconfig/spring-ms
oc get svc 
oc expose svc/spring-ms
oc get route
```

Expected output

## Lab - Building the application within OpenShift with source strategy using source code from GitHub url
```
oc new-app registry.access.redhat.com/ubi8/openjdk-11~https://github.com/tektutor/spring-ms.git --strategy=source
```

Expected output
![image](https://github.com/tektutor/openshift-nov-2023/assets/12674043/e0b7ba66-9bd4-47e9-b6c5-314d129e82ae)

Let's check the build status as shown below
```
oc logs -f buildconfig/spring-ms
```
Expected output
![image](https://github.com/tektutor/openshift-nov-2023/assets/12674043/0905997e-8259-4fa4-8fa4-a5d00dff6618)
![image](https://github.com/tektutor/openshift-nov-2023/assets/12674043/91720e4c-f791-4c70-8f81-711d2bb40d89)

You may try listing the deployment
```
oc get deploy,rs,po
oc get svc
oc expose svc/spring-ms 
```

![image](https://github.com/tektutor/openshift-nov-2023/assets/12674043/372969d5-26f3-47e6-b444-4918cbabe6a3)
![image](https://github.com/tektutor/openshift-nov-2023/assets/12674043/5b745b1f-54aa-42d1-8778-a742916a4605)
![image](https://github.com/tektutor/openshift-nov-2023/assets/12674043/d7d4da56-f9d5-4b64-bba1-4f9fcb34e7fe)

## Lab - Securing our application with edge route (https)

Upgrading the openssl in CentOS
https://webhostinggeeks.com/howto/install-update-openssl-centos/

Find your Openshift cluster domain
```
oc get ingresses.config/cluster -o jsonpath={.spec.domain}
```
Expected similar output
<pre>
[jegan@tektutor.org openshift-feb-2024]$ oc get ingresses.config/cluster -o jsonpath={.spec.domain}
apps.ocp4.training.tektutor  
</pre>


Installing build tools to compile openssl from source code
```
sudo yum groupinstall 'Development Tools'
sudo yum install perl-IPC-Cmd perl-Test-Simple
cd /usr/src
wget https://www.openssl.org/source/openssl-3.0.0.tar.gz
tar -zxf openssl-3.0.0.tar.gz
rm openssl-3.0.0.tar.gz
dnf install perl
cd /usr/src/openssl-3.0.0
./config
make
make test
make install
ln -s /usr/local/lib64/libssl.so.3 /usr/lib64/libssl.so.3
ln -s /usr/local/lib64/libcrypto.so.3 /usr/lib64/libcrypto.so.3
openssl version
```

Let's deploy a microservice and create an edge route as shown below
```
openssl genrsa -out key.key
openssl req -new -key key.key -out csr.csr -subj="/CN=hello-jegan.apps.ocp.tektutor-ocp-labs"
openssl x509 -req -in csr.csr -signkey key.key -out crt.crt
oc create route edge --service spring-ms --hostname hello-jegan.apps.ocp4.training.tektutor --key key.key --cert crt.crt
```
