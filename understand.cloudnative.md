A couple of decades ago, deploying an application means the use of physical hardware. Own the hardware means also maintaining the hardware by the infrastructure guy, and when running an application, all the resources will be used at 100% all the time. For more traffic, teams either buy more servers or run several copies of the same application in the same server to do "scaling".

However, in the cloud, the idea is to pay on-demand. The more traffic you have, the more resource scaling you need. 

Cloud-Native Computing is an approach that aims to build and run scalable applications in modern, dynamic environments, even public, private, or hybrid clouds.

Containers, microservices, serverless functions, service meshes, and immutable infrastructure are common elements of this architectural style. 

These technologies enable loosely coupled, resilient, manageable, and observable systems. 

The Cloud Native Computing Foundation (CNCF) tries to drive this paradigm's adoption by promoting an ecosystem of open-source, vendor-neutral projects.

Visit [CNCF Cloud Native](https://landscape.cncf.io/) for a look at the Cloud Native Interactive Landscape map.

In Cloud Native Computing, each microservice or service is packaged into its own container, and those containers are then dynamically orchestrated to optimize resource utilization. Docker and Kubernetes are some of the implementations of containers and orchestrators, respectively.

Docker is a set of platform-as-a-service (PaaS) products that use OS-level virtualization to deliver software.

By 2000, when a company had a new application, they used to buy a new server. Teams could not predict server capacities, so we usually ended up with an expensive big server and an application using 10% of the available resources. Then came virtualization. If a new application was coming along, we reuse an existing server to run another VM. Servers were now more used (80% instead of 10%).

A full virtualized system gets its own set of resources allocated to it and does minimal sharing.

So, let's say an application will need 1 GB to run; deploying a full VM will need 1 GB x number of VMs as the RAM total capacity.

A container runs on a physical server; it doesn't need virtualization.

With containers and AuFS, the bulk of the 1 GB can be shared between all the containers, and if 10 containers are running, the 1 GB of space for the containers OS will be used and maximum shared.

Instead of installing several operating systems per application, we install only one operating system per server and a container per application. Therefore, each application starts quickly because there is no need to start the operating system; it's already started.

```
+--------------------+  +--------------------+  +-------------------------+
|                    |  |                    |  |                         |
|                    |  |                    |  |                         |
|                    |  | +-------+ +------+ |  |                         |
|                    |  | | App1  | | App2 | |  |                         |
|                    |  | +-------+ +------+ |  |                         |
|                    |  | +-------+ +------+ |  | +---------+ +---------+ |
|                    |  | |  OS   | |  OS  | |  | |  App1   | |   App2  | |
|                    |  | +-------+ +------+ |  | +---------+ +---------+ |
| +----------------+ |  | +-------+ +------+ |  | +---------+ +---------+ |
| |      App       | |  | |  VM   | |  VM  | |  | |Container| |Container| |
| +----------------+ |  | +-------+ +------+ |  | +---------+ +---------+ |
| +----------------+ |  | +----------------+ |  | +---------------------+ |
| |      OS        | |  | |   hypervisor   | |  | |          OS         | |
| +----------------+ |  | +----------------+ |  | +---------------------+ |
| +----------------+ |  | +----------------+ |  | +---------------------+ |
| |physical server | |  | |physical server | |  | |   physical server   | |
| +----------------+ |  | +----------------+ |  | +---------------------+ |
+--------------------+  +--------------------+  +-------------------------+
     Bare metal                  VM                     Container
```

Executing two or three containers on a single machine is one thing, but having to execute hundreds of containers and manage them is another story. That's why you need an orchestrator such as Kubernetes.

Kubernetes (a.k.a. K8s) is an orchestrator for containerized applications. 

Kubernetes is platform agnostic. Package an application in a container, declare the desired state of the application on a manifest file, give the all lot to Kubernetes, and manage it. Kubernetes job is watching the cluster's state and instantiates/ stop containers based on the load heavy or low.

Kubernetes cluster comprises a master, one or several nodes where one or several pods are deployed. Applications inside containers are deployed into Pods to run on Nodes. 

A pod is a sandbox to run multiple containers and contains a network, kernel namespaces, volumes, etc. All containers in a single pod share the same pod environment. A node can be a virtual or physical machine.

```
+---------------------+
| +-----------------+ |
| |    API Server   | |  // Front controller amd REST API
| +-----------------+ |
| +-----------------+ |
| |    Scheduler    | | // Watches and assigns work to nodes
| +-----------------+ |
| +-----------------+ |
| |Cntroller Manager| | // Manages node controller, endpoint controller, ..
| +-----------------+ |
| +-----------------+ |
| |  Cluster Store  | | // state and configuration (uses etcd)
| +-----------------+ |
| +-----------------+ |
| |      Master     | |
| +-----------------+ |
+---------------------+ 
         |
         |
         +------------------------+
         |                        |
         |                        +
+--------+-----------+            ...
|                    |
|                    |
| +---+ +---+ +---+  |
| |Pod| |Pod| |Pod|  |
| +---+ +---+ +---+  |
| +----------------+ |
| |      Node      | |
| +----------------+ |
+--------------------+

```

But Docker for containers and Kubernetes as containers orchestrator aren't the only ones available today.

OpenContainer, Cloud Foundry, Containers, CoreOS rkt, and Hyper-V Containers are some alternatives to Docker.

Apache Mesos, AWS Fargate, Cloudify, IronWorker, Kontena, and Nomad are alternatives to Kubernetes.

Final words, 

Cloud-native is designed for the cloud to take advantage of its unique possibilities for scaling in/out and typically consist of collections of loosely coupled services running in containers, dynamically orchestrated, so each part is actively scheduled and managed to optimize resource utilization, and microservices-oriented to increase the overall agility and maintainability of applications.