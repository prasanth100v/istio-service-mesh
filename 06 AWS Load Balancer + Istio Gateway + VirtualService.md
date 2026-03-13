# 🌐 AWS Load Balancer + Istio Gateway + VirtualService (EKS External Traffic Guide)

This guide explains **how external traffic enters an Istio service mesh running on Amazon EKS** using:

* AWS Load Balancer
* Istio Ingress Gateway
* Istio Gateway
* VirtualService

These components work together to route **internet traffic to Kubernetes pods**.

---

# 🧭 High-Level Architecture

```text
🌍 Internet (User)
        │
        ▼
🌐 AWS Load Balancer (ELB / ALB)
        │
        ▼
🚪 istio-ingressgateway (Envoy Proxy)
        │
        ▼
📡 Istio Gateway
        │
        ▼
🚦 VirtualService
        │
        ▼
📦 Kubernetes Service
        │
        ▼
🚀 Application Pods
```

---

# ✅ Step 1: Istio Ingress Gateway (LoadBalancer Service)

When Istio is installed, it automatically creates a Kubernetes Service called:

```
istio-ingressgateway
```

You **do NOT need to create your own service**.

This service is typically created as:

```
type: LoadBalancer
```

In **Amazon EKS**, this automatically provisions an **AWS External Load Balancer**.

---

## Example Service Configuration

```yaml
apiVersion: v1
kind: Service
metadata:
  name: istio-ingressgateway
  namespace: istio-system
spec:
  type: LoadBalancer
  selector:
    istio: ingressgateway
  ports:
  - port: 80
    targetPort: 8080
    name: http
  - port: 443
    targetPort: 8443
    name: https
```

### Key Points

✅ AWS automatically creates an **external Load Balancer**
✅ The Load Balancer receives **public traffic**
✅ Traffic is forwarded to **istio-ingressgateway pods**

---

# 🚪 Step 2: Istio Gateway Resource

The **Istio Gateway** defines how traffic **enters the service mesh**.

It works similarly to **Kubernetes Ingress**, but with **more advanced routing control**.

---

## Gateway Example

```yaml
apiVersion: networking.istio.io/v1alpha3
kind: Gateway
metadata:
  name: my-gateway
  namespace: istio-system
spec:
  selector:
    istio: ingressgateway
  servers:
  - port:
      number: 80
      name: http
      protocol: HTTP
    hosts:
    - "myapp.example.com"
```

### What This Gateway Does

* Listens on **port 80**
* Accepts traffic only for **myapp.example.com**
* Routes traffic through **istio-ingressgateway**

---

# 🚦 Step 3: VirtualService (Traffic Routing)

A **VirtualService** defines how traffic should be routed **inside the service mesh**.

It connects the **Gateway → Kubernetes services**.

---

## VirtualService Example

```yaml
apiVersion: networking.istio.io/v1alpha3
kind: VirtualService
metadata:
  name: myapp-vs
  namespace: default
spec:
  hosts:
  - "myapp.example.com"
  gateways:
  - istio-system/my-gateway
  http:
  - match:
    - uri:
        prefix: /app
    route:
    - destination:
        host: myapp-service
        port:
          number: 80
```

---

## What This Configuration Means

When a request arrives:

```
http://myapp.example.com/app
```

Istio will:

1️⃣ Match the `/app` path
2️⃣ Route traffic to **myapp-service**
3️⃣ Forward traffic to **pods behind that service**

---

# ⚠️ DestinationRule Requirement

If your application **does not have multiple versions**, you **do NOT need a DestinationRule**.

Example where it is required:

* Canary deployments
* Blue-green deployments
* Version-based routing

For **basic routing**, VirtualService alone is enough.

---

# 🌍 DNS Configuration

After the AWS Load Balancer is created, it receives a **DNS name** like:

```
a1b2c3d4e5f6g7h8.elb.amazonaws.com
```

You must configure your DNS provider.

Example:

```
myapp.example.com → a1b2c3d4e5f6g7h8.elb.amazonaws.com
```

Once configured, users can access:

```
http://myapp.example.com/app
```

---

# 🔄 Complete Traffic Flow

## Step-by-Step

### 1️⃣ User Sends Request

```
http://myapp.example.com/app
```

---

### 2️⃣ AWS Load Balancer Receives Request

```
User → AWS Load Balancer
```

The load balancer forwards traffic to **istio-ingressgateway**.

---

### 3️⃣ Istio Ingress Gateway Handles Traffic

Envoy proxy inside **istio-ingressgateway** receives the request.

---

### 4️⃣ Istio Gateway Accepts Traffic

The Gateway checks:

* Hostname
* Port
* Protocol

---

### 5️⃣ VirtualService Routes Traffic

VirtualService determines **which Kubernetes service should receive the request**.

---

### 6️⃣ Kubernetes Service Forwards Traffic

```
myapp-service → Pod
```

---

### 7️⃣ Application Pod Processes Request

The request reaches the **application container**.

---

# 📊 Visual Traffic Flow

```text
🌍 Internet (User)
        │
        ▼
🌐 AWS Load Balancer
        │
        ▼
🚪 istio-ingressgateway (Envoy Proxy)
        │
        ▼
📡 Istio Gateway
        │
        ▼
🚦 VirtualService
        │
        ▼
📦 Kubernetes Service
        │
        ▼
🚀 Application Pods
```

---

# 🎯 Key Benefits of This Architecture

### 🌐 Scalable Entry Point

AWS Load Balancer automatically scales.

### 🚦 Advanced Traffic Routing

Istio enables:

* Path routing
* Header routing
* Canary deployments

### 🔐 Security

Supports:

* TLS termination
* mTLS inside the mesh

### 📊 Observability

Integrated with:

* Prometheus
* Jaeger
* Kiali

---

# 🧠 Final Takeaway

In **Amazon EKS with Istio**, external traffic is handled by three main components:

| Component         | Role                          |
| ----------------- | ----------------------------- |
| AWS Load Balancer | Entry point from the internet |
| Istio Gateway     | Defines ports & protocols     |
| VirtualService    | Routes traffic to services    |

Together they provide a **powerful and flexible way to expose microservices running in Kubernetes**.

---

⭐ **Istio + EKS gives you enterprise-grade traffic control, security, and observability for microservices.**
