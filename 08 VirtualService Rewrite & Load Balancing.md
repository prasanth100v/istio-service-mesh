# 🔄 Istio VirtualService Rewrite & Load Balancing (EKS Guide)

Istio provides powerful **traffic management features** such as:

* 🔁 URL Rewriting
* ⚖️ Load Balancing
* 🎯 Version-based routing
* 🚦 Traffic control

These features are mainly configured using:

* **VirtualService**
* **DestinationRule**

---

# 🔄 URL Rewriting in VirtualService

Sometimes you want to **change the request path before it reaches the backend service**.

Example:

```text
/v1/old  →  /v2/new
```

This is called **URI rewriting**.

---

## Example VirtualService (Rewrite)

```yaml id="qg2p0m"
apiVersion: networking.istio.io/v1beta1
kind: VirtualService
metadata:
  name: rewrite-path
spec:
  hosts:
  - "myapp.example.com"
  - myapp.default.svc.cluster.local
  gateways:
  - my-gateway
  - mesh
  http:
  - match:
    - uri:
        prefix: /v1/old
    rewrite:
      uri: /v2/new
    route:
    - destination:
        host: myapp-service
        port:
          number: 80
```

---

## What This Configuration Does

When a user sends:

```text
http://myapp.example.com/v1/old/products
```

Istio rewrites it internally to:

```text
/v2/new/products
```

before sending the request to **myapp-service**.

---

# 🌐 Hosts Explanation

```yaml
hosts:
- "myapp.example.com"
- myapp.default.svc.cluster.local
```

### External Host

Handles traffic from **external users**.

```text
myapp.example.com
```

### Internal Host

Handles **service-to-service communication inside Kubernetes**.

```text
myapp.default.svc.cluster.local
```

---

# 🚪 Gateways Section

```yaml
gateways:
- my-gateway
- mesh
```

### my-gateway

Used for **external traffic** entering the cluster.

### mesh

Used for **internal traffic within the service mesh**.

This allows **both internal and external traffic routing**.

---

# ⚖️ Load Balancing in Istio

Load balancing ensures **traffic is distributed across multiple service instances (pods)**.

Istio performs load balancing using **Envoy sidecar proxies**.

The configuration is defined in a **DestinationRule**.

---

# 📄 Example DestinationRule (Load Balancing)

```yaml id="2sgn9l"
apiVersion: networking.istio.io/v1beta1
kind: DestinationRule
metadata:
  name: my-service-lb
spec:
  host: my-service.default.svc.cluster.local
  trafficPolicy:
    loadBalancer:
      simple: ROUND_ROBIN
      consistentHash:
        httpHeaderName: "x-user-id"
```

---

# 🔁 Load Balancing Algorithms

Istio supports several algorithms.

### 🔄 ROUND_ROBIN (Default)

Requests are distributed evenly across pods.

```text
Request 1 → Pod A
Request 2 → Pod B
Request 3 → Pod C
```

---

### ⚡ LEAST_CONN

Traffic goes to the pod with **fewest active connections**.

Good for **long-running requests**.

---

### 🎲 RANDOM

Pods are selected randomly.

---

### 🔒 Consistent Hash (Session Affinity)

Keeps the same user connected to the **same pod**.

Example:

```yaml
consistentHash:
  httpHeaderName: "x-user-id"
```

This ensures:

```text
User A → Pod 1
User A → Pod 1
User A → Pod 1
```

Useful for:

* login sessions
* shopping carts
* user state

---

# 🔗 AWS Load Balancer + Istio Load Balancing (EKS)

In **Amazon EKS**, traffic balancing happens at **two levels**.

---

# 🌐 Level 1: AWS Load Balancer (External)

AWS handles **incoming internet traffic**.

```text
User
 │
 ▼
AWS Load Balancer
 │
 ▼
Istio Ingress Gateway
```

Responsibilities:

* distribute traffic across gateway pods
* provide external IP/DNS
* TLS termination (optional)

---

# 🚦 Level 2: Istio Load Balancing (Internal)

Once traffic enters the mesh:

```text
Ingress Gateway
 │
 ▼
VirtualService
 │
 ▼
DestinationRule
 │
 ▼
Service Pods
```

Istio decides:

* which service receives traffic
* which pod receives the request
* how traffic is split between versions

---

# 🔄 Complete Traffic Flow (EKS + Istio)

```text
🌍 User
   │
   ▼
🌐 AWS Load Balancer
   │
   ▼
🚪 Istio Ingress Gateway
   │
   ▼
🚦 VirtualService
   │
   ▼
⚖️ DestinationRule (Load Balancing)
   │
   ▼
📦 Kubernetes Service
   │
   ▼
🚀 Application Pods
```

---

# 🎯 Example Scenario

Imagine a **payment service** with multiple pods.

```text
payment-service
 ├── payment-pod-1
 ├── payment-pod-2
 └── payment-pod-3
```

Traffic flow:

```text
User → AWS LB → Gateway → VirtualService → DestinationRule
```

Then Istio distributes traffic across pods.

---

# 🧠 Key Responsibilities

| Component         | Role                          |
| ----------------- | ----------------------------- |
| AWS Load Balancer | Handles external traffic      |
| Istio Gateway     | Entry point into service mesh |
| VirtualService    | Routes traffic                |
| DestinationRule   | Controls load balancing       |

---

# 🎯 Benefits of This Architecture

### 🚀 Smart Traffic Control

Advanced routing without changing application code.

### ⚖️ Intelligent Load Balancing

Multiple algorithms available.

### 🔐 Security

Supports mTLS between services.

### 📊 Observability

Integrates with:

* Prometheus
* Grafana
* Kiali
* Jaeger

---

# 🧠 Final Takeaway

Istio splits responsibilities between **cloud infrastructure and the service mesh**.

```text
AWS Load Balancer
↓
Handles external traffic
```

```text
Istio VirtualService
↓
Controls routing rules
```

```text
DestinationRule
↓
Controls load balancing and subsets
```

Together they provide **fine-grained traffic management for Kubernetes microservices**.

---

⭐ **Istio enables intelligent traffic control, safer deployments, and advanced load balancing for cloud-native applications.**
