# Day5

## Info - Installing JFrog Artifactory Pro in RHEL/CentOS
```
sudo -i

pwd

wget -O jfrog-rpm-installer.tar.gz "https://releases.jfrog.io/artifactory/jfrog-prox/org/artifactory/pro/rpm/jfrog-platform-trial-prox/[RELEASE]/jfrog-platform-trial-prox-[RELEASE]-rpm.tar.gz"

tar -xvzf jfrog-rpm-installer.tar.gz

cd jfrog-platform-trial-pro*
./install.sh
```

We need to start the JFrog Artifactory service
```
sudo systemctl start artifactory.service
```

Once the JFrog Artifactory Server is up and running, we may access its dashboard 
```
http://localhost:8082/
```
