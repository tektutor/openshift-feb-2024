# Day3

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
