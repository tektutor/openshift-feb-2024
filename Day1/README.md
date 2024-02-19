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
 
