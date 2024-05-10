# Learn Why-k8s


## Why Do We Use Kubernetes?
One of the benefits of Kubernetes is that it makes building and running complex applications much simpler. Here’s a handful of the many Kubernetes features:

- Standard services like local DNS and basic load-balancing that most applications need, and are easy to use.
- Standard behaviors (e.g., restart this container if it dies) that are easy to invoke, and do most of the work of keeping applications running, available,   and performant.
- A standard set of abstract “objects” (called things like “pods,” “replicasets,” and “deployments”) that wrap around containers and make it easy to build configurations around collections of containers.

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

