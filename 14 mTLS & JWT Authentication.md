# 🔐 mTLS & JWT Authentication in Istio

Istio provides **strong security mechanisms** to protect communication in microservices architectures.

Two major security mechanisms are:

* 🔒 **mTLS (Mutual TLS)** – secures service-to-service communication
* 🪪 **JWT Authentication** – verifies end-user identity in requests

These are implemented using **PeerAuthentication**, **RequestAuthentication**, and **AuthorizationPolicy** resources.

---

# 🔒 What is mTLS in Istio?

**Mutual TLS (mTLS)** encrypts communication between services and ensures both sides verify each other's identity.

Without mTLS:

```text
Service A → HTTP → Service B
```

Traffic is **unencrypted** and vulnerable.

With mTLS:

```text
Service A → 🔒 mTLS → Service B
```

Traffic is **encrypted and authenticated**.

---

# 🧠 How mTLS Works in Istio

Istio uses **Envoy sidecar proxies** to enforce encryption automatically.

Architecture:

```text
Service A
   │
   ▼
Envoy Proxy
   │
   🔒 mTLS Encryption
   ▼
Envoy Proxy
   │
   ▼
Service B
```

Certificates are automatically issued by **Istio Citadel (now part of Istiod)**.

These certificates verify **service identity** inside the mesh.

---

# 🔑 PeerAuthentication in Istio

**PeerAuthentication** controls how services authenticate each other.

It enforces **mTLS policies**.

---

## Example: Enforce mTLS in a Namespace

```yaml
apiVersion: security.istio.io/v1beta1
kind: PeerAuthentication
metadata:
  name: default
  namespace: payment
spec:
  mtls:
    mode: STRICT
```

---

# 🔍 What This Configuration Does

| Setting   | Effect                                          |
| --------- | ----------------------------------------------- |
| Namespace | Applies to all workloads in `payment` namespace |
| Mode      | Enforces mutual TLS                             |
| Traffic   | Only encrypted traffic allowed                  |

Result:

```text
Service A → 🔒 mTLS → Service B
```

Plaintext traffic is rejected.

---

# ⚙ mTLS Modes

Istio supports three modes.

| Mode       | Behavior                       |
| ---------- | ------------------------------ |
| STRICT     | Only mTLS allowed              |
| PERMISSIVE | Accept both mTLS and plaintext |
| DISABLE    | mTLS turned off                |

---

# 🔍 Verify mTLS

You can verify if mTLS is working using:

```bash
istioctl authn tls-check frontend-pod backend.default.svc.cluster.local
```

Expected output:

```text
OK (mTLS enabled)
```

---

# ⚠ Troubleshooting mTLS

Check existing policies:

```bash
kubectl get peerauthentication -n istio-system
```

Common problems:

* sidecar not injected
* conflicting policies
* namespace mismatch

---

# 🪪 What is JWT in Istio?

**JWT (JSON Web Token)** is used for **user authentication**.

It verifies **who is making the request**.

JWT tokens are commonly issued by:

* Auth0
* Keycloak
* Google Identity
* OAuth providers

Example request:

```text
Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI...
```

Istio verifies the token before allowing access.

---

# 🔑 RequestAuthentication in Istio

**RequestAuthentication** verifies JWT tokens in incoming requests.

---

## Example JWT Validation

```yaml
apiVersion: security.istio.io/v1beta1
kind: RequestAuthentication
metadata:
  name: jwt-auth
  namespace: default
spec:
  selector:
    matchLabels:
      app: my-app
  jwtRules:
  - issuer: "https://example.com"
    jwksUri: "https://example.com/.well-known/jwks.json"
```

---

# 🔍 Configuration Explanation

| Field    | Purpose                             |
| -------- | ----------------------------------- |
| selector | Apply only to specific pods         |
| issuer   | Trusted token issuer                |
| jwksUri  | Public key used to verify signature |

This ensures:

```text
Only requests with valid JWT tokens are accepted
```

---

# ⚠ Important Note

`RequestAuthentication` **only validates tokens**.

It does **not decide whether access is allowed**.

That decision is handled by **AuthorizationPolicy**.

---

# 🛡 AuthorizationPolicy Example

```yaml
apiVersion: security.istio.io/v1beta1
kind: AuthorizationPolicy
metadata:
  name: allow-jwt-users
spec:
  rules:
  - from:
    - source:
        requestPrincipals: ["*"]
```

This policy allows requests **only if a valid JWT exists**.

---

# 🔄 Complete Security Flow

```text
User Request
     │
     ▼
JWT Authentication
     │
     ▼
Authorization Policy
     │
     ▼
mTLS Service Communication
     │
     ▼
Backend Service
```

Security layers:

1️⃣ JWT verifies the user
2️⃣ AuthorizationPolicy checks permissions
3️⃣ mTLS secures service communication

---

# 🎯 Real-World Security Setup

A typical production setup includes:

```text
PeerAuthentication → enforce mTLS
RequestAuthentication → validate JWT
AuthorizationPolicy → allow/deny requests
```

This provides:

* encrypted service communication
* verified user identity
* role-based access control

---

# 🚀 Benefits of mTLS + JWT in Istio

### 🔒 Secure Communication

All service traffic encrypted.

### 🪪 Identity Verification

JWT tokens verify user identity.

### 🛡 Zero Trust Security

Services authenticate each other.

### ⚙ Policy-Based Access Control

Authorization policies control access.

---

# 🧠 Final Takeaway

Istio security combines **network encryption and identity verification**.

| Feature               | Purpose                       |
| --------------------- | ----------------------------- |
| PeerAuthentication    | Enforce mTLS between services |
| RequestAuthentication | Validate JWT tokens           |
| AuthorizationPolicy   | Control access rules          |

⭐ **Istio enables zero-trust security for Kubernetes microservices.**
