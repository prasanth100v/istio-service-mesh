# 🛡 AuthorizationPolicy in Istio

*(Fine-Grained Access Control for Microservices)*

**AuthorizationPolicy** is Istio’s security feature that controls **who can access what** in your service mesh.

It works **after authentication** (JWT or mTLS) and decides whether a request should be **allowed or denied**.

AuthorizationPolicy can evaluate:

* 👤 **Source** – IP, namespace, service account
* 📡 **Request attributes** – path, HTTP method, port
* 🪪 **User identity** – extracted from JWT tokens or mTLS certificates

Think of it like a **firewall rule for microservices**.

---

# 🔐 Example: Allow Only GET Requests

```yaml
apiVersion: security.istio.io/v1beta1
kind: AuthorizationPolicy
metadata:
  name: only-get
  namespace: default
spec:
  selector:
    matchLabels:
      app: myapp
  rules:
  - to:
    - operation:
        methods: ["GET"]
```

---

## 🔍 What This Policy Does

This policy applies to:

```text
Pods labeled: app=myapp
```

Allowed requests:

```text
GET /api/products
GET /api/orders
```

Blocked requests:

```text
POST /api/products
PUT /api/orders
DELETE /api/orders
```

Istio behavior:

```text
Allowed → GET
Denied → POST, PUT, DELETE
```

---

# ⚠ Default Authorization Behavior

| Scenario                   | Behavior                |
| -------------------------- | ----------------------- |
| No AuthorizationPolicy     | All traffic allowed     |
| AuthorizationPolicy exists | Only allowed rules pass |

Once a policy exists, **Istio switches to deny-by-default**.

---

# 💡 Common Authorization Rule Types

AuthorizationPolicy can enforce many access patterns.

### 🌐 Allow All Traffic

```text
Allow all services to access a workload
```

---

### 🪪 Allow Authenticated Users (JWT)

Allow requests only if a **valid JWT token exists**.

---

### 🌍 IP Whitelisting

Allow traffic only from trusted IP ranges.

Example:

```text
192.168.1.0/24
```

---

### 🧭 Namespace-Based Access

Allow only services from a specific namespace.

Example:

```text
frontend namespace → access payment service
```

---

### 📡 Path-Based Rules

Allow only specific API endpoints.

Example:

```text
GET /api/orders
```

---

### ⏰ Time-Based Restrictions

Allow access during specific time windows (custom implementations).

---

# 🪪 JWT (JSON Web Token) – Real-Life Analogy

Think of JWT like a **digital ID card**.

Example:

```text
User logs in → Auth Server issues JWT token
```

The token is attached to every request:

```text
Authorization: Bearer <JWT Token>
```

When the request reaches Istio:

```text
Envoy Proxy verifies the JWT
```

Decision:

```text
Valid token → Access allowed
Invalid token → Access denied
```

---

# 🔍 Viewing JWT and Authorization Policies

Check configured policies using:

```bash
kubectl get requestauthentication -A
kubectl get authorizationpolicy -A
```

---

# 🧠 Why Use JWT in Istio?

JWT provides **stateless authentication** for microservices.

Benefits:

* 🔐 Secure API access
* 🚫 Block requests without valid tokens
* 🛡 Protect against man-in-the-middle attacks
* ⚡ Scalable authentication
* 🔑 Works well in **Zero-Trust architectures**

---

# 🔒 Securing Services with Istio

A full Istio security setup usually includes three layers.

```text
PeerAuthentication
        │
        ▼
RequestAuthentication
        │
        ▼
AuthorizationPolicy
```

---

## Step 1 – Enable mTLS

Secure **service-to-service communication**.

```text
PeerAuthentication → mTLS
```

---

## Step 2 – Validate JWT Tokens

Authenticate **user identity**.

```text
RequestAuthentication → JWT validation
```

---

## Step 3 – Define Access Rules

Allow or deny requests.

```text
AuthorizationPolicy → access control
```

---

# 🌐 What is Ingress Authentication?

**Ingress Authentication** protects services exposed through the **Istio Ingress Gateway**.

It ensures **only authorized users from the internet** can access internal services.

Architecture:

```text
Internet User
     │
     ▼
Istio Ingress Gateway
     │
     ▼
JWT Validation
     │
     ▼
Authorization Policy
     │
     ▼
Backend Service
```

---

# 🔐 Example Use Case

Suppose your application is exposed via:

```text
https://api.myapp.com
```

You want **only authenticated users** to access it.

Steps:

1️⃣ Configure **RequestAuthentication** to validate JWT
2️⃣ Apply **AuthorizationPolicy** to enforce token usage
3️⃣ Apply policy on the **Ingress Gateway**

---

# 🔎 Selecting the Ingress Gateway

You can target the gateway using labels.

Example:

```yaml
selector:
  matchLabels:
    istio: ingressgateway
```

This ensures the security policy applies **before traffic enters the mesh**.

---

# 🎯 Key Benefits of AuthorizationPolicy

### 🔐 Fine-Grained Security

Control access by **user, namespace, service, or path**.

### 🛡 Zero Trust Security

No service is trusted by default.

### 🧩 Flexible Rules

Works with **JWT, mTLS, IP, headers, and namespaces**.

### 🌐 Protect External APIs

Secure applications behind **Istio Ingress Gateway**.

---

# 🧠 Final Takeaway

AuthorizationPolicy is **Istio’s RBAC system for microservices**.

| Feature               | Purpose                       |
| --------------------- | ----------------------------- |
| PeerAuthentication    | Encrypt service communication |
| RequestAuthentication | Verify JWT identity           |
| AuthorizationPolicy   | Enforce access control        |

Together they create a **Zero-Trust security model in the service mesh**.

⭐ **AuthorizationPolicy acts like a firewall rule system for your Kubernetes microservices.**
