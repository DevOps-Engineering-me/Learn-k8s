# Learn Why-k8s


## Why Do We Use Kubernetes?
One of the benefits of Kubernetes is that it makes building and running complex applications much simpler. Here’s a handful of the many Kubernetes features:

- Standard services like local DNS and basic load-balancing that most applications need, and are easy to use.
- Standard behaviors (e.g., restart this container if it dies) that are easy to invoke, and do most of the work of keeping applications running, available,   and performant.
- A standard set of abstract “objects” (called things like “pods,” “replicasets,” and “deployments”) that wrap around containers and make it easy to build configurations around collections of containers.

## Key Features of Kubernetes
![image](https://github.com/Diligite-HRM/hrm_dlg_backend/assets/68004638/e7a386a3-ddf8-4648-ac9f-eed255a4c4bd)

### 1) Scalability
If there's one thing Kubernetes is really, really good at, it's scalability. For workloads large, small and in-between, Kubernetes works equally well.

You can operate a single cluster or dozens with Kubernetes. You can deploy workloads using a single-node cluster that runs on your laptop, or you can build your production-grade Kubernetes clusters using thousands of nodes spread across multiple data centers. You can host a single app in Kubernetes or thousands.

On balance, we should note that there are some limitations to Kubernetes scalability. Your cluster can't have more than 5,000 nodes, for example, and you're limited to 300,000 containers. But it's a safe bet that at least 99.5 percent of Kubernetes use cases can work within these scalability constraints.

### 2) Container Orchestration
The ability to orchestrate containers is another key feature in Kubernetes. Container orchestration means pulling container images from a registry, then provisioning, deploying and scaling containers on the servers that host them.

![image](https://github.com/Diligite-HRM/hrm_dlg_backend/assets/68004638/d4ca045b-bd30-4f00-a9ec-7c315fdd66a9)

This feature is crucial because orchestrating containers by hand is not just feasible at scale. If you tried to download container images by hand, then decide which servers should host which containers manually, and you also attempted to update your deployment configuration in response to shifts in workload performance or request rates, you'd quickly find that doing so for more than just a handful of containers is not practical.

But with Kubernetes, you can outsource the hard work of container orchestration to the Kubernetes control plane, making it easy to manage your containerized applications whether you have a couple containers to run, or a couple thousand.

### 3) Storage Orchestration
Storage orchestration is another important Kubernetes feature, at least for any team that runs stateful apps – meaning apps that need to store data persistently. Through storage orchestration, Kubernetes automatically connects containers that need storage resources to the infrastructure that can provide it.

![image](https://github.com/Diligite-HRM/hrm_dlg_backend/assets/68004638/30e145ae-d75c-46df-a722-0ee374257623)

The exact way Kubernetes does this will vary depending on factors like which type of storage infrastructure you're using and what your containers are using it for. Writing logs files to a local storage volume is different from pushing massive amounts of data to a MySQL database, for example. Either way, Kubernetes's storage orchestration features make it possible to do what you need.

### 4) Self-Healing
Kubernetes offers self-healing features that help workloads recover autonomously when something goes wrong. If it detects failed containers or Pods, for instance, Kubernetes will automatically attempt to restart them. Likewise, if a node becomes unreachable, Kubernetes will automatically try to reschedule any workloads it was hosting to other, healthy nodes.

![image](https://github.com/Diligite-HRM/hrm_dlg_backend/assets/68004638/a3075a27-9ce4-45eb-a12f-4e03798e459e)

Self-healing features are a big deal because they allow Kubernetes environments to recover from many types of failure without waiting for human engineers to step in and save the day. Kubernetes can't automatically fix major issues, such as file corruption in container images or the failure of all of a cluster's control-plane nodes, but it can handle many routine problems on its own.


### 5) Service Discovery and Load Balancing
Workloads orchestrated by Kubernetes don't require engineers to define most network services or configure load balancing manually. Instead, Kubernetes automatically exposes applications as network services and balances traffic between them.

Without this feature, admins would have to assign network addresses to each Pod or container, tell other Pods and containers what those addresses are, and configure networking rules that balance traffic between all of them. That would require a huge amount of effort, and it wouldn't scale. Fortunately, since Kubernetes does most of this work automatically, service discovery and load balancing at scale are simple.

### 6) Rolling Updates and Rollbacks
If you want to update an application in a Kubernetes environment, you don't have to take it offline, deploy the update, and then restore service. Instead, thanks to Kubernetes's built-in rolling updates feature, you can incrementally apply updates by replacing older versions of Pods with new ones, until all have been updated. You can perform rollbacks (meaning the reversion to an earlier version of an application) in a similar fashion.

In addition to saving time and effort on the part of Kubernetes admins, this feature benefits users because it minimizes the risk of downtime or disruption due to app updates.

## Top Kubernetes Benefits
### 1) Easy Deployment and Updates
It would be wrong to say that deploying or updating Kubernetes is always easy. On the contrary, Kubernetes is very complex, and setting it up from scratch entirely on your own takes a ton of work.

However, as of 2024, there are plenty of approaches to Kubernetes deployment and updates that make the process simple. You could use a managed Kubernetes service, like EKS or AKS, to run containers using Kubernetes without having to deploy a control plane or set up host infrastructure yourself. Kubernetes deployment and management platforms like Rancher and OpenShift also make for a simple process.

And once Kubernetes is up and running, deploying apps and application updates is a breeze: In most cases, you simply write some YAML code that tells Kubernetes what you want to run and how to run it, and it does the rest.

### 2) App Stability and Availability in a Cloud Environment
Kubernetes also does a great job of keeping applications stable and available once they're running.

![image](https://github.com/Diligite-HRM/hrm_dlg_backend/assets/68004638/538d1ee9-0114-4f44-a038-3875ddebbe13)

Again, it's not magic, and it can't do things like recover an environment whose underlying infrastructure has totally failed – which is why it's also important to have a Kubernetes observability and monitoring strategy in place to catch major failures. But it can automatically deploy and scale applications in ways that help them to maintain maximum availability and performance levels, even in complex cloud environments where conditions are always changing.

### 3) Multi-Cloud Capability

Kubernetes can run virtually anywhere – on any public cloud, in a private cloud or on simple on-prem servers. This makes Kubernetes a great solution if you want to deploy containerized applications on multiple clouds.

![image](https://github.com/Diligite-HRM/hrm_dlg_backend/assets/68004638/86d7fad3-6e29-4d1c-b247-90040b9e37ab)

It's worth noting that running multi-cloud Kubernetes can be somewhat challenging. Some Kubernetes distributions or services, like EKS and AKS, don't work with other clouds, and managing Kubernetes clusters spread across multiple clouds through a single control plane requires special network configurations.

That said, multi-cloud architectures are complex no matter how you orchestrate containerized applications. Ultimately, Kubernetes is arguably the best way to take advantage of multiple clouds, since it can manage and scale multi-cloud workloads in a consistent way once it's configured to do so.

### 4) Portability Across Cloud Providers
The fact that Kubernetes works consistently across cloud providers and is free in most cases from vendor lock in is also a benefit for organizations that switch between clouds, not just those who use multiple clouds at once. If you deploy your workloads using Kubernetes, you can typically move them to a different cloud without having to overhaul your apps or configuration.

Some adjustments may be necessary, especially if you are using a particular cloud provider's managed Kubernetes service. But in general, Kubernetes offers a degree of portability that alternative container orchestration platform options can't match.

### 5) Availability of Resources Online
Because Kubernetes is the most popular container orchestration tool, there is a dynamic Kubernetes community that produces excellent resources online, including information, management tools, and extensions.

In addition to the excellent Kubernetes documentation, admins can benefit from a plethora of third-party Kubernetes guides and tutorials. They can also find a wide range of Kubernetes add-ons, as well as apps ready to be deployed on Kubernetes, on GitHub and similar repositories.
### 6) Cost-Effectiveness
As an open source solution, the Kubernetes platform is free of cost. You might pay fees if you use a Kubernetes management service, but they are typically minimal; for example, EKS charges a cluster management fee of just $2.40 per day – which is not a lot when you're deploying hundreds of containers.

At the same time, Kubernetes's ability to manage and scale apps across a cluster of servers efficiently can save money because it helps to make the most of the infrastructure available. Without Kubernetes, you might have some servers sitting idle because they are not hosting any apps while others are maxing out their resources because they are hosting too many. But Kubernetes helps you strike the ideal balance.

Both of these benefits combine to make Kubernetes a cost-effective way to deploy and manage applications. You get a free or low-cost tool that optimizes the resource efficiency of your apps and infrastructure. And when you invest in Kubernetes cost optimization, which helps prevent unnecessary Kubernetes spending, you get even more value from the orchestrator.

### 7) Increased DevOps Efficiency for Microservices Architecture
Kubernetes helps DevOps teams work more efficiently, too. With Kubernetes, DevOps engineers can build, test, and deploy microservices apps in the same platform. This benefit eliminates the headache and risk that occur when apps developed in one platform have to move to a different one for production.

![image](https://github.com/Diligite-HRM/hrm_dlg_backend/assets/68004638/bb0d3926-df9b-44e6-b088-659bfac6b96d)

We're not saying Kubernetes alone makes all aspects of DevOps efficient and simple. But when it comes to app development and deployment, it does add efficiency and lower the risk of problems that could disrupt application release cycles.

