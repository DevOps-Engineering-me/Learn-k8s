# Deployment Of Kubernetes

A Deployment provides declarative updates for Pods and ReplicaSets.

You describe a desired state in a Deployment, and the Deployment Controller changes the actual state to the desired state at a controlled rate. You can define Deployments to create new ReplicaSets, or to remove existing Deployments and adopt all their resources with new Deployments


**Actual Overflow of Deployment**

**Most Important part about to notice that, AUTO Healing behaviours**

![ddp](https://github.com/MdShafiqulSaymon/Portfolio/assets/68004638/12b64202-b74e-4aa6-a987-2dada6a0dc4d)


### Let's Dive into the actual Implimentations

**Create deployment.yaml file**

```
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx-deployment
  labels:
    app: nginx
spec:
  replicas: 3
  selector:
    matchLabels:
      app: nginx
  template:
    metadata:
      labels:
        app: nginx
    spec:
      containers:
      - name: nginx
        image: nginx:1.14.2
        ports:
        - containerPort: 80
```



- **apiVersion: apps/v1**
  - Specifies the API version for the Deployment object. apps/v1 is the stable version for managing applications.
- **kind: Deployment**
  - Defines the type of Kubernetes object. In this case, it's a Deployment, which manages a set of replicated Pods.
- **metadata**:
  - Contains data to uniquely identify the Deployment object.
  - **name: nginx-deployment**
    - The name of the Deployment. It uniquely identifies this Deployment within the namespace.
  - **labels:**
    - A set of key-value pairs for categorizing and selecting the Deployment.
    - app: nginx
      - A label with key app and value nginx to categorize this Deployment as part of the nginx application.
- **spec**:
  - Defines the desired state for the Deployment.
  - **replicas**: 3
    - Specifies the number of Pod replicas to run. In this case, 3 Pods.
  - **selector**:
    - Determines how the Deployment finds which Pods to manage.
    - **matchLabels**:
      - A set of key-value pairs that the Deployment uses to select Pods.
      - **app: nginx**
        - Selects Pods with the label app: nginx.
  - **template**:
    - Describes the Pods that will be created by this Deployment.
    - **metadata**:
      - Metadata for the Pods.
      - **labels**:
        - Labels assigned to the Pods created by this template.
        - **app: nginx**
          - Pods will be labeled with app: nginx.
    - **spec**:
      - Specifies the configuration for the Pods.
      - **containers**:
        - A list of containers that will run in each Pod.
        - **name: nginx**
          - Defines a container named nginx.
        - **image: nginx:1.14.2**
          - Specifies the container image to use. In this case, it uses nginx version 1.14.2
        - **ports**:
          - Defines a list of ports to expose from the container.
          - **containerPort: 80**
            - Exposes port 80 from the container, which is the default HTTP port for nginx.

**to execute the deployment.yaml file**
```
kubectl apply -f deployment.yaml
```
![Screenshot from 2024-05-24 14-24-04](https://github.com/MdShafiqulSaymon/Portfolio/assets/68004638/4acf93c2-724f-4819-8a64-6a653a10a6f3)

### Let's Test some AUTO Healing Feature,how actually work

1) We will Delete a pod
2) In Other terminal we will see the live change which replicaset try to heal 


![auto_healing](https://github.com/MdShafiqulSaymon/Portfolio/assets/68004638/6d5498e7-b7e8-480e-8e98-f1b2192e56e5)


