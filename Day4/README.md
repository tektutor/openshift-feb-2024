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

