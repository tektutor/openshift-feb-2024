# Day 1
## Virtualization (Hypervisors)
- If your Desktop/Laptop/Servers has a processor that supports Virtualization
- Processor
  - AMD ( Virtualization Feature - AMD-V )
  - Intel ( VT-X )
- Two types
  - Type 1 - Bare Metal Hypervisor - Used in Workstations/Servers (Doesn't require OS to create Virtual Machines )
  - Type2 - Used in Desktops/Laptops/Workstations (requires Host OS)
- Examples
  - VMWare Workstation
  - VMWare vSphere/vCenter
  - Oracle VirtualBox
  - Parallels
  - Microsoft Hyper-V
- This helps in running many Operating System side by side on your laptop/desktop simultaneously
- Many OS can be actively running at the same time on your Laptop/Desktop/Workstation/Server
- This type of virtualization is heavy-weight
  - Because it requires allocated dedicated Hardware resources to each Virtual Machine
  - We need to allocate CPU Core for each Virtual Machine(Guest OS)
  - We need to allocate RAM for each Virtual Machine(Guest OS)
  - We need to allocate Storage/Hardisk for each Virtual Machine(Guest OS)
  - the hypervisor software ensure the hardware resources allocated to one OS is not used by other OS running on the same base machine
  - Each Virtual Machine represents one OS
  
## Docker Overview
- is a Linux containerization technology powered by Linux Kernel
- lightweight application virtualization
- container doesn't represent OS
- containers comes with one single application with all its dependent libraries/software tools
- Linux Kernel supports
  - CGroups ( this helps in apply hardware quota restrictions to every containers )
  - Namespace (this helps isolating one container from other containers )
- LXC
- Container Runtime (runC, CRI-O) vs Container Engine (Docker, Podman)
- each container represents one application
- each container get its own file system
- each container has its own network stack ( 7 OSI Layers )
- it's a software defined network
- each container get its own IP address ( Private IP )

## Container Runtime Overview
- Container Runtime software tools manages containers
- they know
  - how to create a container
  - list a container
  - delete a continer
  - start/stop/restart
  - kill/abort containers
- these are low-level software which are not so user friendly, hence end-users like us normally won't use this directly
- these softwares are used by Container Engines to manage containers
Examples
- runC
- CRI-O

## Container Engine
- high-level software that depends on Container Runtime to manage containers
- they offer user-friendly commands to manage images and containers without need to underdstand low-level kernel stuffs
- Examples
  - Docker ( uses containerd which in turn uses runC container runtime )
  - Podman ( uses CRI-O container runtime ) - Red Hat openshift starting from v4.x support only Podman
  - Containerd ( uses runC container runtime )


## Container Orchestration Platforms
- helps us provide an eco-system to make our application highly-available (HA)
- your application has to be deployed within container orchestratino platform as containerized applications
- supports inbuilt monitoring features to check the health/liveliness of application and can heal/repair when there is a need
- when the user traffic to your application increase, additional container instances can be created to cope up with the heavy user traffic
- when the user traffic to your application comes down, then certain containerized application workloads can be removed
- in case you need to upgrade your live application from one version to other latest version without any downtime
- Examples
  - Docker SWARM ( supports only Docker - Docker Inc's native orchestration platform )
  - Google Kubernetes ( supports multiple types of Containers including Docker )
  - Red Hat OpenShift ( supports only Podman containerized application workloads )

## Google Kubernetes
- The master and worker nodes they can use any of the below Operating System
  - Ubuntu Linux
  - RHEL
  - Oracle Linux
  - Amazon Linux

## Red Hat OpenShift
- container orchestration platforms is setup a cluster of servers
- the servers could be physical machines or virtual machines or public cloud based virtual machines
- there are two types of Nodes(Servers)
  - master node ( Control Plane components runs here )
  - worker node ( user application workloads runs here )
- OpenShift master Nodes
  - supports only RHCOS (Red Hat Enterprise Core OS)
- OpenShift worker Nodes
  - supports two options (Either we could use RHEL or RHCOS OS)
- Control Plane Components ( only runs in master nodes )
  - API Server
  - etcd (key/value datastore/db server)
  - scheduler
  - controller managers ( a collection of many controllers - monitoring )

#### RedHat Enterprise Core OS
- this is an optimized OS for Container Orchestration Platforms like OpenShift
- the configuration to boot this OS comes a special file called ignition which is not part of the RHCOS
- the ignition files that has boot configuration of RHCOS are normally kept in a load balancer running on an external VM/Physical machine
- this OS has puts lot of restrications ( imposes standards & conventions - enforces best practices )
- it only allows to modify files in certain folders
- for instance many folders have only ready only access
- many ports are reserved for OpenShift's private use, applications can't use the reserved ports

#### Openshift/Kubernetes master nodes
- normally it is recommended to run only Control Plane components
- ideally, we should not configure the master nodes to take up user application workloads
- but, if required the master nodes can be configured to take up user application workloads in addition to runninig control plane components

#### What is a Pod?
- Pod is a collection of related containers
- Any application that is deployed inside the Kubernetes/OpenShift they are deployed as Pods
- Every Pod has atleast two containers
  - Secret Infra Container (Pause container) that supports the Network Stack
  - Application container
  - Application container and the Secret Infra Container shares the same Network Stack, hence they share the same IP address, hostname, ports, etc.,
  - IP address is assigned on the Pod level in Kubernetes/Openshift

#### Kubernetes/OpenShift objects/resources
- Pod - Our containerized application runs within the Pod
- ReplicaSet - This tells how many Pods of our application should be running
- Deployment - This tells, what is the name of application deployment, what container image should be used, how many Pod instances should be running
- DaemonSet - If your openshift cluster has 5 nodes, then it would one Pod per node
- Job - any one time task can be achieved via Job
- CronJob - any repeatative tasks 
- StatefulSet - application that requires peristent volume(external storage)
- Service - This is how we can expose an application either within the cluster or outside the cluster

#### Worker Node Components
- kubelet - Container Agent that interacts with CRI-O Container Runtime via the CRI(Container Runtime Interface)
- Kubelet is the one that downloads the required container images and creates the Pod and updates the API Server with the status of each Pod

## Chain of things that happens when we create a deployment
The below command is used to deploy nginx web server in Openshift
```
oc create deploy nginx --image=bitnami/nginx:latest
```

<pre>
1. oc client tool will make a REST call to API Server requesting to create a Deployment db record in etcd database
2. API Server will receive the REST call from oc client, then it creates a Deployment with name nginx in the etcd db server
3. Anytime there is an update in etcd, that results in event trigger, an event something like New Deployment created will be triggered.
4. New Deployment Created event is received by Deployment Controller, which then makes a REST call to API Server to create one Pod using the bitnami/nginx:latest docker image.
5. API Server receives the request from Deployment Controller, then it request the API Server to create a ReplicaSet db entry.
6. API Server creates a ReplicaSet db record, and triggers New ReplicaSet created event.
7. ReplicaSet Controller receives this event, and it will request the API Server to create number of Pods mentioned in the ReplicaSet.
6. This results in a new event triggered something like New Pod created.
7. The scheduler receives the New Pod created event and it responds with recommendation on which node the new Pod could be deployed. Scheduler sends its scheduling recommendation as REST call to API Server.
8. API Server receives the recommendataion from Scheduler, it retrieves the already present Pod record from etcd db server, updates the scheduling details.
9. This results in Pod scheduled event, which is then received by kubelet running on respective nodes.  Kubelet then downloads the required container image and creates the containers associated to certain Pods.
10. Once the containers related to a pod are created by kubelet, it updates the API Server with the status of these containers on heart-beat like fashion periodically.
11. Once the API Server receives the status, the status of those Pods are updated in the etcd db server.

</pre>

  

## Lab - Listing the RedHat Openshift Nodes
In the below commands, oc and kubectl is the client tool we would be using to interact with Openshift cluster

```
oc version
oc get nodes
oc get nodes -o wide
kubectl get nodes
```

Expected output
![Openshift nodes](nodes.png)
![Openshift nodes](kubeconfig.png)

## Lab - Create a project before deploying applications

In the below command, replace 'jegan' with your name.
```
oc new-project jegan
```

Expected output
<pre>
[jegan@tektutor.org ~]$ oc new-project jegan
Now using project "jegan" on server "https://api.ocp4.training.tektutor:6443".

You can add applications to this project with the 'new-app' command. For example, try:

    oc new-app rails-postgresql-example

to build a new example application in Ruby. Or use kubectl to deploy a simple Kubernetes application:

    kubectl create deployment hello-node --image=registry.k8s.io/e2e-test-images/agnhost:2.43 -- /agnhost serve-hostname
</pre>

## Lab - Listing projects in Openshift
```
oc get project
```
Expected output
<pre>
[jegan@tektutor.org ~]$ oc get projects
NAME                                               DISPLAY NAME   STATUS
default                                                           Active
jegan                                                             Active
kube-node-lease                                                   Active
kube-public                                                       Active
kube-system                                                       Active
openshift                                                         Active
openshift-apiserver                                               Active
openshift-apiserver-operator                                      Active
openshift-authentication                                          Active
openshift-authentication-operator                                 Active
openshift-cloud-controller-manager                                Active
openshift-cloud-controller-manager-operator                       Active
openshift-cloud-credential-operator                               Active
openshift-cloud-network-config-controller                         Active
openshift-cluster-csi-drivers                                     Active
openshift-cluster-machine-approver                                Active
openshift-cluster-node-tuning-operator                            Active
openshift-cluster-samples-operator                                Active
openshift-cluster-storage-operator                                Active
openshift-cluster-version                                         Active
openshift-config                                                  Active
openshift-config-managed                                          Active
openshift-config-operator                                         Active
openshift-console                                                 Active
openshift-console-operator                                        Active
openshift-console-user-settings                                   Active
openshift-controller-manager                                      Active
openshift-controller-manager-operator                             Active
openshift-dns                                                     Active
openshift-dns-operator                                            Active
openshift-etcd                                                    Active
openshift-etcd-operator                                           Active
openshift-host-network                                            Active
openshift-image-registry                                          Active
openshift-infra                                                   Active
openshift-ingress                                                 Active
openshift-ingress-canary                                          Active
openshift-ingress-operator                                        Active
openshift-insights                                                Active
openshift-kni-infra                                               Active
openshift-kube-apiserver                                          Active
openshift-kube-apiserver-operator                                 Active
openshift-kube-controller-manager                                 Active
openshift-kube-controller-manager-operator                        Active
openshift-kube-scheduler                                          Active
openshift-kube-scheduler-operator                                 Active
openshift-kube-storage-version-migrator                           Active
openshift-kube-storage-version-migrator-operator                  Active
openshift-machine-api                                             Active
openshift-machine-config-operator                                 Active
openshift-marketplace                                             Active
openshift-monitoring                                              Active
openshift-multus                                                  Active
openshift-network-diagnostics                                     Active
openshift-network-node-identity                                   Active
openshift-network-operator                                        Active
openshift-node                                                    Active
openshift-nutanix-infra                                           Active
openshift-oauth-apiserver                                         Active
openshift-openstack-infra                                         Active
openshift-operator-lifecycle-manager                              Active
openshift-operators                                               Active
openshift-ovirt-infra                                             Active
openshift-route-controller-manager                                Active
openshift-sdn                                                     Active
openshift-service-ca                                              Active
openshift-service-ca-operator                                     Active
openshift-user-workload-monitoring                                Active
openshift-vsphere-infra                                           Active
</pre>  

## Lab - Creating your first deployment in imperative style
```
oc create deploy nginx --image=bitnami/nginx:latest
```

Expected output
<pre>
[jegan@tektutor.org ~]$ oc create deploy nginx --image=bitnami/nginx:latest
deployment.apps/nginx created
</pre>

## Lab - Listing the deployments
```
oc get deployments
oc get deployment
oc get deploy
```

Expected output
<pre>
[jegan@tektutor.org ~]$ oc get deployments
NAME    READY   UP-TO-DATE   AVAILABLE   AGE
nginx   0/1     1            0           8s
[jegan@tektutor.org ~]$ oc get deployment
NAME    READY   UP-TO-DATE   AVAILABLE   AGE
nginx   1/1     1            1           11s
[jegan@tektutor.org ~]$ oc get deploy
NAME    READY   UP-TO-DATE   AVAILABLE   AGE
nginx   1/1     1            1           14s    
</pre>
