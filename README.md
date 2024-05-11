# Architecture Of Kubernetes.
![image](https://github.com/Diligite-HRM/hrm_dlg_backend/assets/68004638/bf5a1e2f-769b-4126-b58f-c64c258e5bd7)

### Let's Understand one by One.
Kubernetes Cluster have 2 type of node

**1)Master node / Controll panel:** The control plane is responsible for container orchestration and maintaining the desired state of the cluster. It has the following components.
  - kube-apiserver
  - etcd
  - kube-scheduler
  - kube-controller-manager
  - cloud-controller-manager

**2)Worker node:** The Worker nodes are responsible for running containerized applications. The worker Node has the following components.

  - kubelet
  - kube-proxy
  - Container runtime
## Kubernetes Control Plane Components
### **1)kube-apiserver:** 
The kube-api server is the central hub of the Kubernetes cluster that exposes the Kubernetes API. It is highly scalable and can handle large number of concurrent requests.

End users, and other cluster components, talk to the cluster via the API server. Very rarely monitoring systems and third-party services may talk to API servers to interact with the cluster.

So when you use kubectl to manage the cluster, at the backend you are actually communicating with the API server through HTTP REST APIs. However, the internal cluster components like the scheduler, controller, etc talk to the API server using gRPC.

The communication between the API server and other components in the cluster happens over **TLS** to prevent unauthorized access to the cluster.

![image](https://github.com/Diligite-HRM/hrm_dlg_backend/assets/68004638/dfbb654a-0c0e-462f-b519-83704515ccdd)

**Kubernetes api-server is responsible for the following.**

- **API management**: Exposes the cluster API endpoint and handles all API requests. The API is version and itsupports multiple API versions simultaneously.
- **Authentication** (Using client certificates, bearer tokens, and HTTP Basic Authentication) and Authorization (ABAC and RBAC evaluation)
- Processing API requests and validating data for the API objects like pods, services, etc. **(Validation and Mutation Admission controllers)**
- It is the only component that communicates with etcd.
- api-server coordinates all the processes between the control plane and worker node components.
- api-server has a built-in apiserver proxy. It is part of the API server process. It is primarily used to enable access to ClusterIP services from outside the cluster, even though these services are typically only reachable within the cluster itself.
- API server also contianes an aggreagation layer which allows you to extend Kubernetes API to create custom APIs resources and controllers.
- The API server also supports watching resources for changes. For example, clients can establish a watch on specific resources and receive real-time notifications when those resources are created, modified, or deleted

### **2) etcd**
Kubernetes is a distributed system and it needs an efficient distributed database like etcd that supports its distributed nature. It acts as both a backend service discovery and a database. You can call it the brain of the Kubernetes cluster.

etcd is an open-source strongly consistent, distributed key-value store. So what does it mean?

- **Strongly consistent**: If an update is made to a node, strong consistency will ensure it gets updated to all the other nodes in the cluster immediately. Also if you look at CAP theorem, achieving 100% availability with strong consistency and & Partition Tolerance is impossible.
- **Distributed**: etcd is designed to run on multiple nodes as a cluster without sacrificing consistency.
- **Key Value Store**: A nonrelational database that stores data as keys and values. It also exposes a key-value API. The datastore is built on top of **BboltDB (Ben Johnson's Bolt key/value store)** which is a fork of BoltDB.

**So how does etcd work with Kubernetes?**

To put it simply, when you use kubectl to get kubernetes object details, you are getting it from etcd. Also, when you deploy an object like a pod, an entry gets created in etcd.

In a nutshell, here is what you need to know about etcd.

- etcd stores all configurations, states, and metadata of Kubernetes objects (pods, secrets, daemonsets, deployments, configmaps, statefulsets, etc).
- etcd allows a client to subscribe to events using Watch() API . Kubernetes api-server uses the etcd’s watch functionality to track the change in the state of an object.
- etcd exposes key-value API using gRPC. Also, the gRPC gateway is a RESTful proxy that translates all the HTTP API calls into gRPC messages. This makes it an ideal database for Kubernetes.
- etcd stores all objects under the **/registry** directory key in key-value format. For example, information on a pod named Nginx in the default namespace can be found under **/registry/pods/default/nginx**

![image](https://github.com/Diligite-HRM/hrm_dlg_backend/assets/68004638/7026a638-9380-4f73-a7a6-c259afae418f)


### **3) kube-scheduler**
The kube-scheduler is responsible for **scheduling Kubernetes pods on worker nodes**.

When you deploy a pod, you specify the pod requirements such as CPU, memory, affinity, taints or tolerations, priority, persistent volumes (PV),  etc. The scheduler’s primary task is to identify the create request and choose the best node for a pod that satisfies the requirements.

The following image shows a high-level overview of **how the scheduler works.**

![image](https://github.com/Diligite-HRM/hrm_dlg_backend/assets/68004638/7e9135a7-ac7e-4477-affe-6a923a6571f1)

In a Kubernetes cluster, there will be more than one worker node. So how does the scheduler select the node out of all worker nodes?

**Here is how the scheduler works.**

- To choose the best node, the Kube-scheduler uses **filtering and scoring operations**.
- In filtering, the scheduler finds the best-suited nodes where the pod can be scheduled. For example, if there are five worker nodes with resource availability to run the pod, it selects all five nodes. If there are no nodes, then the pod is unschedulable and moved to the scheduling queue. If It is a large cluster, let’s say 100 worker nodes, and the scheduler doesn’t iterate over all the nodes. There is a scheduler configuration parameter called percentageOfNodesToScore. The default value is typically 50%. So it tries to iterate over 50% of nodes in a round-robin fashion. If the worker nodes are spread across multiple zones, then the scheduler iterates over nodes in different zones. For very large clusters the default percentageOfNodesToScore is 5%.
- In the scoring phase, the scheduler ranks the nodes by assigning a score to the filtered worker nodes. The scheduler makes the scoring by calling multiple scheduling plugins. Finally, the worker node with the highest rank will be selected for scheduling the pod. If all the nodes have the same rank, a node will be selected at random.
- Once the node is selected, the scheduler creates a binding event in the API server. Meaning an event to bind a pod and node.

**Here is shat you need to know about a scheduler.**

- It is a controller that listens to pod creation events in the API server.
- The scheduler has two phases. **Scheduling cycle** and the **Binding cycle**. Together it is called the scheduling context. The scheduling cycle selects a worker node and the binding cycle applies that change to the cluster.
- The scheduler always places the high-priority pods ahead of the low-priority pods for scheduling. Also, in some cases, after the pod starts running in the selected node, the pod might get evicted or moved to other nodes. If you want to understand more, read the Kubernetes pod priority guide
- You can create custom schedulers and run multiple schedulers in a cluster along with the native scheduler. When you deploy a pod you can specify the custom scheduler in the pod manifest. So the scheduling decisions will be taken based on the custom scheduler logic.
- The scheduler has a pluggable scheduling framework. Meaning, that you can add your custom plugin to the scheduling workflow.

### **4)Kube Controller Manager**
What is a controller? Controllers are programs that run infinite control loops. Meaning it runs continuously and watches the actual and desired state of objects. If there is a difference in the actual and desired state, it ensures that the kubernetes resource/object is in the desired state.

As per the official documentation,
**In Kubernetes, controllers are control loops that watch the state of your cluster, then make or request changes where needed. Each controller tries to move the current cluster state closer to the desired state.**
Let’s say you want to create a deployment, you specify the desired state in the manifest YAML file (declarative approach). For example, 2 replicas, one volume mount, configmap, etc. The in-built deployment controller ensures that the deployment is in the desired state all the time. If a user updates the deployment with 5 replicas, the deployment controller recognizes it and ensures the desired state is 5 replicas.

Kube controller manager is a component that manages all the Kubernetes controllers. Kubernetes resources/objects like pods, namespaces, jobs, replicaset are managed by respective controllers. Also, the Kube scheduler is also a controller managed by the Kube controller manager.

![image](https://github.com/Diligite-HRM/hrm_dlg_backend/assets/68004638/eb41e7f2-16d6-4057-a9fe-f54fb7ad20e8)

**Following is the list of important built-in Kubernetes controllers.**

- Deployment controller
- Replicaset controller
- DaemonSet controller 
- Job Controller (Kubernetes Jobs)
- CronJob Controller
- endpoints controller
- namespace controller
- service accounts controller.
- Node controller

**Here is what you should know about the Kube controller manager.**

- It manages all the controllers and the controllers try to keep the cluster in the desired state.
- You can extend kubernetes with custom controllers associated with a custom resource definition.

### **5)Cloud Controller Manager (CCM)**
Cloud Controller Manager (CCM)
When kubernetes is deployed in cloud environments, the cloud controller manager acts as a bridge between Cloud Platform APIs and the Kubernetes cluster.

This way the core kubernetes core components can work independently and allow the cloud providers to integrate with kubernetes using plugins. (For example, an interface between kubernetes cluster and AWS cloud API)

![image](https://github.com/Diligite-HRM/hrm_dlg_backend/assets/68004638/0ec23023-df8b-4709-ac10-033e0d0d47a6)

Cloud Controller Manager contains a set of cloud platform-specific controllers that ensure the desired state of cloud-specific components (nodes, Loadbalancers, storage, etc). Following are the three main controllers that are part of the cloud controller manager.

- **Node controller**: This controller updates node-related information by talking to the cloud provider API. For example, node labeling & annotation, getting hostname, CPU & memory availability, nodes health, etc.
- **Route controller**: It is responsible for configuring networking routes on a cloud platform. So that pods in different nodes can talk to each other.
- **Service controller**: It takes care of deploying load balancers for kubernetes services, assigning IP addresses, etc.

Following are some of the classic examples of cloud controller manager.

- Deploying Kubernetes Service of type Load balancer. Here Kubernetes provisions a Cloud-specific Loadbalancer and integrates with Kubernetes Service.
- Provisioning storage volumes (PV) for pods backed by cloud storage solutions.


Overall **Cloud Controller Manager manages** the lifecycle of cloud-specific resources used by kubernetes.
Cloud controller integration allows Kubernetes cluster to provision cloud resources like instances (for nodes), Load Balancers (for services), and Storage Volumes (for persistent volumes).


## Kubernetes Worker Node Components

### **1)Kubelet**


Kubelet is an agent component that runs on every node in the cluster. t does not run as a container instead runs as a **daemon**, managed by systemd.

It is responsible for registering worker nodes with the API server and working with the podSpec (Pod specification – YAML or JSON) primarily from the API server. podSpec defines the containers that should run inside the pod, their resources (e.g. CPU and memory limits), and other settings such as environment variables, volumes, and labels.

It then brings the podSpec to the desired state by creating containers.

**To put it simply, kubelet is responsible for the following.**

- Creating, modifying, and deleting containers for the pod.
- Responsible for handling liveliness, readiness, and startup probes.
- Responsible for Mounting volumes by reading pod configuration and creating respective directories on the host for the volume mount.
- Collecting and reporting Node and pod status via calls to the API server with implementations like cAdvisor and CRI.

Kubelet is also a controller that watches for pod changes and utilizes the node’s container runtime to pull images, run containers, etc.

Other than PodSpecs from the API server, kubelet can accept podSpec from a file, HTTP endpoint, and HTTP server. A good example of “podSpec from a file” is Kubernetes static pods.

**Static pods are controlled by kubelet, not the API servers.**

This means you can create pods by providing a pod YAML location to the Kubelet component. However, static pods created by Kubelet are not managed by the API server.

Here is a real-world example use case of the static pod.

While bootstrapping the control plane, kubelet starts the api-server, scheduler, and controller manager as static pods from podSpecs located at /etc/kubernetes/manifests

**Following are some of the key things about kubelet.**

- Kubelet uses the CRI (container runtime interface) gRPC interface to talk to the container runtime.
- It also exposes an HTTP endpoint to stream logs and provides exec sessions for clients.
- Uses the CSI (container storage interface) gRPC to configure block volumes.
- It uses the CNI plugin configured in the cluster to allocate the pod IP address and set up any necessary network routes and firewall rules for the pod.

 ![image](https://github.com/Diligite-HRM/hrm_dlg_backend/assets/68004638/2c87cb0d-1446-42ef-afce-258a20ab6557)

### **2)Kube proxy**

To understand Kube proxy, you need to have a basic knowledge of Kubernetes Service & endpoint objects.

Service in Kubernetes is a way to expose a set of pods internally or to external traffic. When you create the service object, it gets a virtual IP assigned to it. It is called clusterIP. It is only accessible within the Kubernetes cluster.

The Endpoint object contains all the IP addresses and ports of pod groups under a Service object. The endpoints controller is responsible for maintaining a list of pod IP addresses (endpoints). The service controller is responsible for configuring endpoints to a service.

You cannot ping the ClusterIP because it is only used for service discovery, unlike pod IPs which are pingable.

Now let’s understand Kube Proxy.

**Kube-proxy is a daemon that runs on every node as a daemonset**. It is a proxy component that implements the Kubernetes Services concept for pods. (single DNS for a set of pods with load balancing). It primarily proxies UDP, TCP, and SCTP and **does not understand HTTP**.

When you expose pods using a Service (ClusterIP), Kube-proxy creates network rules to send traffic to the backend pods (endpoints) grouped under the Service object. Meaning, all the load balancing, and service discovery are handled by the Kube proxy.

**So how does Kube-proxy work?**

Kube proxy talks to the API server to get the details about the Service (ClusterIP) and respective pod IPs & ports (endpoints). It also monitors for changes in service and endpoints.

Kube-proxy then uses any one of the following modes to create/update rules for routing traffic to pods behind a Service

- **IPTables**: It is the default mode. In IPTables mode, the traffic is handled by IPtable rules. This means that for each service, IPtable rules are created. These rules capture the traffic coming to the ClusterIP and then forward it to the backend pods. Also, In this mode, kube-proxy chooses the backend pod random for load balancing. Once the connection is established, the requests go to the same pod until the connection is terminated.
- **IPVS**: For clusters with services exceeding 1000, IPVS offers performance improvement. It supports the following load-balancing algorithms for the backend.

          rr: round-robin : It is the default mode.

          lc: least connection (smallest number of open connections)

          dh: destination hashing

          sh: source hashing

          sed: shortest expected delay

          nq: never queue

- **Userspace** (legacy & not recommended)
- **Kernelspace**: This mode is only for Windows systems.

![image](https://github.com/Diligite-HRM/hrm_dlg_backend/assets/68004638/ca6509b6-3ba3-41fc-b740-86c8956c21c6)


## **3)Container Runtime**
You probably know about Java Runtime (JRE). It is the software required to run Java programs on a host. In the same way, container runtime is a software component that is required to run containers.

Container runtime runs on all the nodes in the Kubernetes cluster. It is responsible for pulling images from container registries, running containers, allocating and isolating resources for containers, and managing the entire lifecycle of a container on a host.

To understand this better, let’s take a look at two key concepts:

- Container Runtime Interface (CRI): It is a set of APIs that allows Kubernetes to interact with different container runtimes. It allows different container runtimes to be used interchangeably with Kubernetes. The CRI defines the API for creating, starting, stopping, and deleting containers, as well as for managing images and container networks.
- Open Container Initiative (OCI): It is a set of standards for container formats and runtimes

Kubernetes supports multiple container runtimes (CRI-O, Docker Engine, containerd, etc) that are compliant with Container Runtime Interface (CRI). This means, all these container runtimes implement the CRI interface and expose gRPC CRI APIs (runtime and image service endpoints).

So how does Kubernetes make use of the container runtime?

As we learned in the Kubelet section, the kubelet agent is responsible for interacting with the container runtime using CRI APIs to manage the lifecycle of a container. It also gets all the container information from the container runtime and provides it to the control plane.

Let’s take an example of CRI-O container runtime interface. Here is a high-level overview of how container runtime works with kubernetes.

![image](https://github.com/Diligite-HRM/hrm_dlg_backend/assets/68004638/4454d6c7-496f-4351-9f40-a254bf02f568)

- When there is a new request for a pod from the API server, the kubelet talks to CRI-O daemon to launch the required containers via Kubernetes Container Runtime Interface.
- CRI-O checks and pulls the required container image from the configured container registry using containers/image library.
- CRI-O then generates OCI runtime specification (JSON) for a container.
- CRI-O then launches an OCI-compatible runtime (runc) to start the container process as per the runtime specification.
