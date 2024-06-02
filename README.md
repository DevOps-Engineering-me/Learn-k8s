# Ingress
**Ingress is a Kubernetes resource that manages external access to services within a cluster, typically HTTP/HTTPS. It provides a unified way to define routing rules, SSL termination, and virtual hosts.**

## Why Ingress?
**It's Helps**

- **Centralized Routing:** Manage all external traffic routing in one place.
- **SSL Termination:** Handle SSL/TLS termination for secure communication.
- **Load Balancing:** Distribute traffic evenly across multiple pods.
- **Path-Based Routing:** Route traffic to different services based on URL paths.
- **Host-Based Routing:** Serve multiple applications using the same IP address based on the host header.

## Components of Ingress

**1) Ingress Resource:**
- Defines the rules for routing traffic.
- an specify multiple rules for different hosts and paths.

**2) Ingress Controller:**
- Manages the Ingress resources.
- Implements the actual routing and load balancing.
- Common controllers include Nginx, Traefik, and HAProxy.


## Let's take Demo with real world 

**1) Enable Ingress in Minikube:**
   ```
   minikube addons enable ingress
   ```
**2) Define Kubernetes Resources:**
   - Deployment: Manages the application pods.
   - Service: Exposes the application pods internally within the cluster.
   - Ingress: Defines how external traffic is routed to the services.

**Example Configurations**

**Deployment (deployment.yaml)**

```
apiVersion: apps/v1
kind: Deployment
metadata:
  name: my-portfolio
spec:
  replicas: 3
  selector:
    matchLabels:
      app: my-portfolio
  template:
    metadata:
      labels:
        app: my-portfolio
    spec:
      containers:
      - name: my-portfolio
        image: youknowwhoitssaymon/my-portfolio:latest
        ports:
        - containerPort: 80
```


**Service (service.yaml)**

```
apiVersion: v1
kind: Service
metadata:
  name: my-portfolio-service
spec:
  type: ClusterIP
  selector:
    app: my-portfolio
  ports:
    - protocol: TCP
      port: 80
      targetPort: 80
```

**Ingress (ingress.yaml)**

```
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: my-portfolio-ingress
  annotations:
    nginx.ingress.kubernetes.io/rewrite-target: /
spec:
  rules:
  - host: my-portfolio.local
    http:
      paths:
      - path: /
        pathType: Prefix
        backend:
          service:
            name: my-portfolio-service
            port:
              number: 80
```


## Traffic Flow in Ingress

![ingress](https://github.com/MdShafiqulSaymon/Portfolio/assets/68004638/8ac9f869-9a16-482e-9cf5-6000758a0760)


**1)DNS Resolution:**

- Client sends a request to http://my-portfolio.local.
- The local DNS resolves my-portfolio.local to the Minikube IP address.

**2)Ingress Controller:**

- The request reaches the Nginx Ingress Controller running in the Minikube cluster.
- The Ingress Controller inspects the host and path of the request.
  
**3)Ingress Resource:**

- The Ingress resource defines routing rules based on the host (my-portfolio.local) and path (/).
- Matches the incoming request to the correct backend service (my-portfolio-service).


**4)Service:**

- The service routes the request to one of the pods running the application.
- Load balances traffic among available pods.

**5)Pod:**

- The selected pod processes the request.
- The pod serves the response back to the client through the service and ingress controller.


## Updating Local Hosts File
To access your application via the defined hostname, update your local hosts file:

**1)Get Minikube IP:**

```
minikube ip
```

**2)Edit Hosts File:**

Add an entry to your local **hosts** file (usually located at /etc/hosts on macOS/Linux or C:\Windows\System32\drivers\etc\hosts on Windows):

```
<minikube-ip> my-portfolio.local
```
Replace <minikube-ip> with the actual IP address from the minikube ip command.

open on your browser: http://my-portfolio.local.


