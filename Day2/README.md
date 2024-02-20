# Day 2

## Adavantages of ClusterIP 
- it is free as this type of loadbalancing service is totally implemented by Kubernetes/Openshift, hence no extra charges from AWS/Azure or any public cloud

### Advantages of using NodePort service
- it is free as it is implemented by Kubernetes/Openshift
- it won't attract any extra charges from public cloud

## Disadvantages of using NodePort service
- for each NodePort service Openshift will open one port on every node in the range 30000-32627
- the more number of NodePort services we create it is going to open too many ports, this is security issue
- it is not so user-friendly
- Example
  - In order to access node port service
    http://node-ip:node-port
  - We need to know the node ip and node-port, the node ip is going to be different on different openshift cluster

## Disadvantages of using LoadBalancer service
- it is expensive
- for each loadbalancer service we create in aws managed openshift (ROSA) or Azure managed openshift it will spin an external load balancer, this will attract additional charges from AWS/Azure

## What is the recommended technique used to expose an external service in Openshift?
Openshift route is alternate feature of odePort service.  Route makes your application accessible to the outside world without the complications of NodePort service with an user-friendly url.

```
oc new-project jegan
oc create deploy nginx --image=bitnami/nginx:latest --replicas=3
oc expose deploy/nginx --port=8080
oc get svc
oc expose svc/nginx
oc get route
curl nginx-jegan.apps.ocp4.training.tektutor
```

Expected output
```
[jegan@tektutor.org openshift-feb-2024]$ oc new-project jegan
Already on project "jegan" on server "https://api.ocp4.training.tektutor:6443".

You can add applications to this project with the 'new-app' command. For example, try:

    oc new-app rails-postgresql-example

to build a new example application in Ruby. Or use kubectl to deploy a simple Kubernetes application:

    kubectl create deployment hello-node --image=registry.k8s.io/e2e-test-images/agnhost:2.43 -- /agnhost serve-hostname

[jegan@tektutor.org openshift-feb-2024]$ oc create deploy nginx --image=bitnami/nginx:latest --replicas=3
deployment.apps/nginx created

[jegan@tektutor.org openshift-feb-2024]$ oc expose deploy/nginx --port=8080
service/nginx exposed

[jegan@tektutor.org openshift-feb-2024]$ oc get svc
NAME    TYPE        CLUSTER-IP       EXTERNAL-IP   PORT(S)    AGE
nginx   ClusterIP   172.30.195.217   <none>        8080/TCP   5s

[jegan@tektutor.org openshift-feb-2024]$ oc expose svc/nginx
route.route.openshift.io/nginx exposed

[jegan@tektutor.org openshift-feb-2024]$ oc get route
NAME    HOST/PORT                                 PATH   SERVICES   PORT   TERMINATION   WILDCARD
nginx   nginx-jegan.apps.ocp4.training.tektutor          nginx      8080                 None

[jegan@tektutor.org openshift-feb-2024]$ curl nginx-jegan.apps.ocp4.training.tektutor
<!DOCTYPE html>
<html>
<head>
<title>Welcome to nginx!</title>
<style>
html { color-scheme: light dark; }
body { width: 35em; margin: 0 auto;
font-family: Tahoma, Verdana, Arial, sans-serif; }
</style>
</head>
<body>
<h1>Welcome to nginx!</h1>
<p>If you see this page, the nginx web server is successfully installed and
working. Further configuration is required.</p>

<p>For online documentation and support please refer to
<a href="http://nginx.org/">nginx.org</a>.<br/>
Commercial support is available at
<a href="http://nginx.com/">nginx.com</a>.</p>

<p><em>Thank you for using nginx.</em></p>
</body>
</html>
```



