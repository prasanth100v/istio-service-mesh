# 🔄 Istio VirtualService Rewrite & Load Balancing (EKS Guide)
 * Istio provides powerful **traffic management features** such as:
    * 🔁 URL Rewriting
    * ⚖️ Load Balancing
    * 🎯 Version-based routing
    * 🚦 Traffic control
 * These features are mainly configured using:
       * **VirtualService**
       * **DestinationRule**

## 🔄 URL Rewriting in VirtualService
 * Sometimes you want to **change the request path before it reaches the backend service**.
 * Example:
```hcl
/v1/old  →  /v2/new
```
 * This is called **URI rewriting**.

### Example VirtualService (Rewrite)
```yaml
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

## What This Configuration Does
 * When a user sends:
```hcl
http://myapp.example.com/v1/old/products
```
 * Istio rewrites it internally to:
```hcl
/v2/new/products
```
 * before sending the request to **myapp-service**.

## 🌐 Hosts Explanation
```yaml
hosts:
- "myapp.example.com"
- myapp.default.svc.cluster.local
```

### External Host vs Internal Host
| 🧩 **Host Type**     | 🎯 **Purpose**                   | 📖 **Description**                                                                  | 🌍 **Accessible From** | 💡 **Example**                    |
| -------------------- | -------------------------------- | ----------------------------------------------------------------------------------- | ---------------------- | --------------------------------- |
| 🌐 **External Host** | Internet-facing traffic          | Used by external users to access applications through Ingress Gateway/Load Balancer | Internet               | `myapp.example.com`               |
| ☸️ **Internal Host** | Service-to-service communication | Used for communication between services inside the Kubernetes cluster               | Kubernetes Cluster     | `myapp.default.svc.cluster.local` |

### 🚪 Gateways Section
```hcl
gateways:
- my-gateway
- mesh
```
 * my-gateway = Used for **external traffic** entering the cluster.
 * mesh = Used for **internal traffic within the service mesh**.
 * This allows **both internal and external traffic routing**.
     * External hosts are used for `internet-facing access` through an Istio Gateway (e.g., `myapp.example.com`),
     * Internal hosts use `Kubernetes DNS names` (e.g., `myapp.default.svc.cluster.local`) for service-to-service communication within the cluster. 🚀☸️🌐

---

## ⚖️ Load Balancing in Istio
 * Load balancing ensures **traffic is distributed across multiple service instances (pods)**.
 * Istio performs load balancing using **Envoy sidecar proxies**.
 * The configuration is defined in a **DestinationRule**.

### 📄 Example DestinationRule (Load Balancing)
```yaml
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

## ⚖️ Istio Load Balancing Algorithms
| 🧩 **Algorithm**             | 📖 **Description**                                  | ⚙️ **How It Works** & 🎯 **Example**                                                                         |
| ---------------------------- | --------------------------------------------------- | -------------------------------------------------------------------------------------------------------------- | 
| 🔄 **ROUND_ROBIN (Default)** | Distributes requests evenly across all pods         | Each request is sent to the next available pod in sequence ( `Req1→PodA`, `Req2→PodB`, `Req3→PodC`   )          |
| ⚡ **LEAST_CONN**             | Sends traffic to pod with fewest active connections | Selects the least busy pod   ( `New request → Pod with lowest connections `)                                  |
| 🎲 **RANDOM**                | Chooses pods randomly                               | Traffic is distributed randomly across healthy pods ( Simple load distribution -- `Request → Random Pod `)      |
| 🔒 **CONSISTENT_HASH**       | Routes same client to same pod                      | Keeps the same user connected to the **same pod**    (`User A → Pod A consistently` )                          |
| 🌐 **PASSTHROUGH**           | Forwards traffic without load balancing             | Advanced networking scenarios    ( `Direct endpoint routing`  )                                                 |


### 🔒 Consistent Hash (Session Affinity)
 * Keeps the same user connected to the **same pod**.
 * Example:
```yaml
consistentHash:
  httpHeaderName: "x-user-id"
```
 * This ensures:
```hcl
User A → Pod 1
User A → Pod 1
User A → Pod 1
```

---

# 🔗 AWS Load Balancer + Istio Load Balancing (EKS)
 * In **Amazon EKS**, traffic balancing happens at **two levels**.

## 🌐 Level 1: AWS Load Balancer (External)
 * AWS handles **incoming internet traffic**.
```hcl
User
 │
 ▼
AWS Load Balancer
 │
 ▼
Istio Ingress Gateway
```
 * Responsibilities:
     * distribute traffic across gateway pods
     * provide external IP/DNS
     * TLS termination (`optional`)

# 🚦 Level 2: Istio Load Balancing (Internal)
 * Once traffic enters the mesh:
```hcl
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

 * Istio decides:
   * which service receives traffic
   * which pod receives the request
   * how traffic is split between versions

---

## 🔄 Complete Traffic Flow (EKS + Istio)
```hcl
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

### 🎯 Example Scenario
 * Imagine a **payment service** with multiple pods.
```hcl
payment-service
 ├── payment-pod-1
 ├── payment-pod-2
 └── payment-pod-3
```
 * Traffic flow:
```hcl
User → AWS LB → Gateway → VirtualService → DestinationRule
```
 * Then Istio distributes traffic across pods.

---

# 🧠 Key Responsibilities
| 🧩 **Component**         | 🎯 **Role**                   | 📖 **Description**                                                  | ⚙️ **Key Responsibility**                              | 💡 **Example**                  |
| ------------------------ | ----------------------------- | --------------------------------------------------------------------- | ------------------------------------------------------ | ------------------------------- |
| ⚖️ **AWS Load Balancer** | Handles external traffic      | Receives requests from internet users and forwards them to Kubernetes | Internet-facing entry point                            | User → Load Balancer            |
| 🌐 **Istio Gateway**     | Entry point into service mesh | Defines hosts, ports, and protocols for incoming traffic              | Accepts HTTP/HTTPS/TLS traffic                         | `myapp.example.com` on port 443 |
| 🚦 **VirtualService**    | Routes traffic                | Defines routing rules and traffic distribution                        | Path-based routing, canary releases, retries, timeouts | 90% → v1, 10% → v2              |
| 🎯 **DestinationRule**   | Controls load balancing       | Defines subsets (`versions`) and traffic policies                     | Load balancing, circuit breaking, connection pools     | ROUND_ROBIN, LEAST_CONN         |

### 🚀 End-to-End Traffic Flow
```hcl
🌍 Internet User
        ↓
⚖️ AWS Load Balancer
        ↓
🌐 Istio Gateway
        ↓
🚦 VirtualService
        ↓
🎯 DestinationRule
        ↓
📦 Service Pods (v1 / v2)
```
 * In an Istio architecture, the AWS Load Balancer receives `external traffic`, the `Istio Gateway` allows traffic into the service mesh,
 * the `VirtualService` determines where traffic should go (Controls routing rules), and the `DestinationRule` Controls traffic load balancing and managed among service versions (subsets). 🚀☸️🌐

### 🎯 Benefits of This Architecture
 * 🚀 Smart Traffic Control -- Advanced routing without changing application code.
 * ⚖️ Intelligent Load Balancing -- Multiple algorithms available.
 * 🔐 Security  --  Supports mTLS between services.
 * 📊 Observability -- Integrates with:
       * Prometheus
       * Grafana
       * Kiali
       * Jaeger

⭐ **Istio enables intelligent traffic control, safer deployments, and advanced load balancing for cloud-native applications.**
