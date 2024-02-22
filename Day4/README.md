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

## Lab - Securing our application route with edge route (https)

Upgrading the openssl in CentOS
https://webhostinggeeks.com/howto/install-update-openssl-centos/

Installing build tools to compile openssl from source code
```
sudo yum groupinstall 'Development Tools'
sudo yum install perl-IPC-Cmd perl-Test-Simple
cd /usr/src
wget https://www.openssl.org/source/openssl-3.0.0.tar.gz
tar -zxf openssl-3.0.0.tar.gz
rm openssl-3.0.0.tar.gz
cd /usr/src/openssl-3.0.0
./config
make
make test
make install
ln -s /usr/local/lib64/libssl.so.3 /usr/lib64/libssl.so.3
ln -s /usr/local/lib64/libcrypto.so.3 /usr/lib64/libcrypto.so.3
openssl version
```

Let's deployment a microservice and create an edge route as shown below
```
openssl genrsa -out key.key
openssl req -new -key key.key -out csr.csr -subj="/CN=hello-jegan.apps.ocp.tektutor-ocp-labs"
openssl x509 -req -in csr.csr -signkey key.key -out crt.crt
oc create route edge --service spring-ms --hostname hello-jegan.apps.ocp.tektutor-ocp-labs --key key.key --cert crt.crt
```
