# Day5

## Info - Installing JFrog Artifactory Pro in RHEL/CentOS

#### Pre-requisites
- JFrog expects an official email-id to request for a trail license, hence *** I suggest not to try this ***
- You need to be an Administrator on the Linux Server
- You need to have a trial license of JFrog Artifactory Pro

```
sudo -i

pwd

wget -O jfrog-rpm-installer.tar.gz "https://releases.jfrog.io/artifactory/jfrog-prox/org/artifactory/pro/rpm/jfrog-platform-trial-prox/[RELEASE]/jfrog-platform-trial-prox-[RELEASE]-rpm.tar.gz"
```

Expected output
<pre>
[root@tektutor.org ~]# wget -O jfrog-rpm-installer.tar.gz "https://releases.jfrog.io/artifactory/jfrog-prox/org/artifactory/pro/rpm/jfrog-platform-trial-prox/[RELEASE]/jfrog-platform-trial-prox-[RELEASE]-rpm.tar.gz"
Warning: wildcards not supported in HTTP.
--2024-02-23 08:10:55--  https://releases.jfrog.io/artifactory/jfrog-prox/org/artifactory/pro/rpm/jfrog-platform-trial-prox/[RELEASE]/jfrog-platform-trial-prox-[RELEASE]-rpm.tar.gz
Resolving releases.jfrog.io (releases.jfrog.io)... 3.221.179.99, 54.221.82.220, 50.17.28.229
Connecting to releases.jfrog.io (releases.jfrog.io)|3.221.179.99|:443... connected.
HTTP request sent, awaiting response... 302 
Location: https://releases-cdn.jfrog.io/filestore/a6/a6076b9f3925dc71d6ad8d44f2b62640986419cf?response-content-type=application/x-gzip&response-content-disposition=attachment%3Bfilename%3D%22jfrog-platform-trial-prox-7.77.5-rpm.tar.gz%22&x-jf-traceId=c056171222e55c8f&X-Artifactory-repositoryKey=jfrog-prox&X-Artifactory-projectKey=default&X-Artifactory-artifactPath=org%2Fartifactory%2Fpro%2Frpm%2Fjfrog-platform-trial-prox%2F7.77.5%2Fjfrog-platform-trial-prox-7.77.5-rpm.tar.gz&X-Artifactory-username=anonymous&X-Artifactory-repoType=local&X-Artifactory-packageType=generic&Expires=1708656116&Signature=P4IeXA2o8G46tEeMz0hNGGJTUP9TAManaOEM-czYvYB-G619SfXQySwSl0MaQu3wpiAqaol37GSqBKfEstA-7KfLfeUNroWTYymWJxGVnFtf3WKdaYhzoqhciAGLMLKtBtSCToNvE3~hZTMAdjtiKPAYMPN7OSJPp2HUI2CGxWHuVCWewkRxe7ro2tKMxpSnx03FQqOtRrBVb9gpuNpJnSlweFSOWQmAxzrSlkshKPtdc~uMXrApJPvmYDfNG8F5PoMgGbgO1cDojVvaY4H58NiFJ~-x79inIUVDSa0bXZrIFgOAjBWxjGUGiFkiI717KJu4pQ4f87XoCroOOykKTQ__&Key-Pair-Id=APKAJ6NHFWMVU3M6DPBA [following]
--2024-02-23 08:10:56--  https://releases-cdn.jfrog.io/filestore/a6/a6076b9f3925dc71d6ad8d44f2b62640986419cf?response-content-type=application/x-gzip&response-content-disposition=attachment%3Bfilename%3D%22jfrog-platform-trial-prox-7.77.5-rpm.tar.gz%22&x-jf-traceId=c056171222e55c8f&X-Artifactory-repositoryKey=jfrog-prox&X-Artifactory-projectKey=default&X-Artifactory-artifactPath=org%2Fartifactory%2Fpro%2Frpm%2Fjfrog-platform-trial-prox%2F7.77.5%2Fjfrog-platform-trial-prox-7.77.5-rpm.tar.gz&X-Artifactory-username=anonymous&X-Artifactory-repoType=local&X-Artifactory-packageType=generic&Expires=1708656116&Signature=P4IeXA2o8G46tEeMz0hNGGJTUP9TAManaOEM-czYvYB-G619SfXQySwSl0MaQu3wpiAqaol37GSqBKfEstA-7KfLfeUNroWTYymWJxGVnFtf3WKdaYhzoqhciAGLMLKtBtSCToNvE3~hZTMAdjtiKPAYMPN7OSJPp2HUI2CGxWHuVCWewkRxe7ro2tKMxpSnx03FQqOtRrBVb9gpuNpJnSlweFSOWQmAxzrSlkshKPtdc~uMXrApJPvmYDfNG8F5PoMgGbgO1cDojVvaY4H58NiFJ~-x79inIUVDSa0bXZrIFgOAjBWxjGUGiFkiI717KJu4pQ4f87XoCroOOykKTQ__&Key-Pair-Id=APKAJ6NHFWMVU3M6DPBA
Resolving releases-cdn.jfrog.io (releases-cdn.jfrog.io)... 18.161.246.114, 18.161.246.20, 18.161.246.82, ...
Connecting to releases-cdn.jfrog.io (releases-cdn.jfrog.io)|18.161.246.114|:443... connected.
HTTP request sent, awaiting response... 200 OK
Length: 1495130159 (1.4G) [application/x-gzip]
Saving to: ‘jfrog-rpm-installer.tar.gz’

jfrog-rpm-installer.tar.gz  100%[===========================================>]   1.39G  11.6MB/s    in 1m 56s  

2024-02-23 08:12:53 (12.3 MB/s) - ‘jfrog-rpm-installer.tar.gz’ saved [1495130159/1495130159]
</pre>

Let's extract the tar gunzip file
```
tar -xvzf jfrog-rpm-installer.tar.gz
```

Expected output
<pre>
[root@tektutor.org ~]# tar -xvzf jfrog-rpm-installer.tar.gz
jfrog-platform-trial-prox-7.77.5-rpm/bin/rpmDebHelper.sh
jfrog-platform-trial-prox-7.77.5-rpm/artifactory/artifactory.rpm
jfrog-platform-trial-prox-7.77.5-rpm/install.sh
jfrog-platform-trial-prox-7.77.5-rpm/README.md
jfrog-platform-trial-prox-7.77.5-rpm/third-party/yq/yq
jfrog-platform-trial-prox-7.77.5-rpm/third-party/yq/LICENSE
jfrog-platform-trial-prox-7.77.5-rpm/third-party/postgresql/createPostgresUsers.sh
jfrog-platform-trial-prox-7.77.5-rpm/bin/systemYamlHelper.sh  
</pre>

Start the installation
```
cd jfrog-platform-trial-pro*
./install.sh
```
Expected output
<pre>
[root@tektutor.org ~]# cd jfrog-platform-trial-pro*
[root@tektutor.org jfrog-platform-trial-prox-7.77.5-rpm]# ./install.sh

Beginning JFrog Platform Trial Pro X setup


This script will install JFrog Platform Trial Pro X and its dependencies.
After installation, logs can be found at /root/jfrog-platform-trial-prox-7.77.5-rpm/install.log
Downloading JFrog xray rpm and its dependencies (requires internet access, this may take several minutes)...

RabbitMQ dependencies Installation

Installing/Verifying RabbitMQ dependencies (this may take several minutes)...
Installing /root/jfrog-platform-trial-prox-7.77.5-rpm/third-party/rabbitmq/socat-1.7.4.1-5.el9.x86_64.rpm ...
Installing /root/jfrog-platform-trial-prox-7.77.5-rpm/third-party/rabbitmq/erlang-25.0.3-1.el9.x86_64.rpm ...
warning: /root/jfrog-platform-trial-prox-7.77.5-rpm/third-party/misc/libdb-utils-5.3.28-53.el9.x86_64.rpm: Header V3 RSA/SHA256 Signature, key ID 8483c65d: NOKEY

JFrog Platform Trial Pro X Installation

Installing/Verifying (/root/jfrog-platform-trial-prox-7.77.5-rpm/artifactory/artifactory.rpm) (this may take several minutes)...
Installing/Verifying (/root/jfrog-platform-trial-prox-7.77.5-rpm/xray/xray.rpm) (this may take several minutes)...

PostgreSQL Installation

Creating PostgreSQL data folder on /var/opt/jfrog/postgres/data
Installing/Verifying PostgreSQL (this may take several minutes)...
Installing /root/jfrog-platform-trial-prox-7.77.5-rpm/third-party/postgresql/postgresql13-libs-13.10-1PGDG.rhel9.x86_64.rpm ...
Installing /root/jfrog-platform-trial-prox-7.77.5-rpm/third-party/postgresql/postgresql13-13.10-1PGDG.rhel9.x86_64.rpm ...
Installing /root/jfrog-platform-trial-prox-7.77.5-rpm/third-party/postgresql/postgresql13-server-13.10-1PGDG.rhel9.x86_64.rpm ...
Initializing database ... OK

Created symlink /etc/systemd/system/multi-user.target.wants/postgresql-13.service → /usr/lib/systemd/system/postgresql-13.service.
Starting postgresql-13
Redirecting to /bin/systemctl restart postgresql-13.service

Installation complete. Installation log can be found at [/root/jfrog-platform-trial-prox-7.77.5-rpm/install.log]

Start JFrog Platform Trial Pro X with:
1. systemctl start artifactory.service
2. systemctl start xray.service

Check JFrog Platform Trial Pro X status with:
1. systemctl status artifactory.service
2. systemctl status xray.service

Installation directory was set to /opt/jfrog/
You can find more Artifactory information in the log directory /opt/jfrog/artifactory/var/log
You can find more Xray information in the log directory /opt/jfrog/xray/var/log

Once the application has started, it can be accessed at [http://208.91.198.52:8082]  
</pre>

We need to start the JFrog Artifactory service, xray is optional
```
sudo systemctl start artifactory.service
sudo systemctl start xray.service
```

Once the JFrog Artifactory Server is up and running, we may access its dashboard 
```
http://localhost:8082/
```
