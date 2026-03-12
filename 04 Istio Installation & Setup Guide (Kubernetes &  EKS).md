# 🚀 Istio Installation & Setup Guide (Kubernetes / EKS)

This guide covers **installing Istio, verifying the installation, deploying applications, managing traffic, and troubleshooting the service mesh**.

Istio extends Kubernetes using **Custom Resource Definitions (CRDs)** to control traffic, security, and observability.

---

# 🔧 1️⃣ Install Istio

### Download Istio

```bash
curl -L https://istio.io/downloadIstio | sh -
```

---

### Move into the Istio directory

```bash
cd istio-*
```

---

### Add `istioctl` to your PATH

```bash
export PATH=$PWD/bin:$PATH
```

---

### Install Istio

```bash
istioctl install --set profile=demo -y
```

### Profiles

| Profile | Use Case                |
| ------- | ----------------------- |
| demo    | Learning / testing      |
| default | Production environments |

For production:

```bash
istioctl install --set profile=default -y
```

---

# 🧩 2️⃣ Enable Sidecar Injection

Istio works by injecting **Envoy sidecar proxies** into application pods.

Enable sidecar injection in your namespace:

```bash
kubectl label namespace default istio-injection=enabled
```

---

### Verify Sidecar Injection

Check if the **Envoy sidecar (`istio-proxy`)** is injected.

```bash
kubectl get pod <pod-name> -o jsonpath="{.spec.containers[*].name}"
```

Expected output:

```
app-container istio-proxy
```

If `istio-proxy` appears, the injection worked successfully.

---

# ✅ 3️⃣ Verify Istio Installation

Run the verification command:

```bash
istioctl verify-install
```

This checks whether **all required Istio components are installed correctly**.

---

# 📦 4️⃣ Deploy Applications

Deploy your Kubernetes applications into the mesh.

```bash
kubectl apply -f <deployment>.yaml
```

Since sidecar injection is enabled, **Envoy proxies will automatically be added to the pods**.

---

# 🔍 5️⃣ View Istio Components

Check Istio system components:

```bash
kubectl get pods -n istio-system
kubectl get svc -n istio-system
```

Typical components include:

* `istiod`
* `istio-ingressgateway`
* `kiali`
* `prometheus`
* `grafana`

---

# 🚦 6️⃣ Traffic Routing

Istio controls traffic using **VirtualService** and **DestinationRule**.

Apply configuration:

```bash
kubectl apply -f virtualservice.yaml
```

Describe resource:

```bash
kubectl describe virtualservice <name>
```

---

# 📊 7️⃣ Check Istio Resources

List Istio configuration resources:

```bash
kubectl get gateway
kubectl get virtualservices
kubectl get destinationrule
kubectl get serviceentry
kubectl get authorizationpolicy
kubectl get peerauthentication
```

These resources control:

* traffic routing
* security policies
* external service access

---

# 📈 8️⃣ Access Kiali (Service Mesh Dashboard)

Kiali provides a **visual representation of the service mesh**.

Run port-forward:

```bash
kubectl port-forward svc/kiali -n istio-system 20001:20001
```

Open in browser:

```
http://localhost:20001
```

Kiali helps visualize:

* service dependencies
* traffic flow
* error rates
* latency

---

# 🛠 9️⃣ Troubleshooting Commands

### Detect configuration issues

```bash
istioctl analyze
```

Detects problems like:

* incorrect routes
* missing services
* invalid policies

---

### Check mTLS between services

```bash
istioctl authn tls-check
```

This verifies whether **mutual TLS encryption is working correctly**.

---

### Check proxy sync status

```bash
istioctl proxy-status
```

Shows if **Envoy proxies are synchronized with Istio control plane**.

---

# 🔍 Describe Istio Resources

View detailed configuration:

```bash
kubectl describe virtualservice <vs-name>
kubectl describe destinationrule <dr-name>
```

Useful for debugging routing or policy issues.

---

# 📜 Check Sidecar Logs

If traffic issues occur, check the **Envoy proxy logs**.

```bash
kubectl logs <pod-name> -c istio-proxy
```

Common issues include:

* JWT errors
* routing failures
* policy mismatches

---

# 🧠 Pro Tip

You can use `--help` with any `istioctl` command to see options.

Example:

```bash
istioctl proxy-config --help
```

---

# 🗑 Uninstall Istio

Remove Istio from the cluster.

```bash
istioctl uninstall --purge
```

Delete the namespace:

```bash
kubectl delete namespace istio-system
```

---

# 🧩 What Are Istio CRDs?

Istio extends Kubernetes using **Custom Resource Definitions (CRDs)**.

These CRDs define **how traffic flows and how services communicate in the mesh**.

---

# 📦 Main Istio CRDs

### 🚪 Gateway

Defines **how external traffic enters the mesh**.

---

### 🚦 VirtualService

Defines **traffic routing rules**.

Examples:

* URL-based routing
* traffic splitting
* canary deployments

---

### ⚖️ DestinationRule

Defines **policies applied after routing**.

Examples:

* load balancing
* subsets (service versions)
* circuit breaking

---

### 🔐 PeerAuthentication

Controls **mTLS settings for services**.

---

### 🛡 AuthorizationPolicy

Controls **service-to-service access permissions**.

---

### 🌍 ServiceEntry

Allows Istio services to **communicate with external services outside the mesh**.

Example:

* external APIs
* third-party services

---

# 🎯 Final Summary

Istio enhances Kubernetes with:

✅ **Advanced traffic routing**
✅ **Secure service communication (mTLS)**
✅ **Powerful observability tools**
✅ **Fine-grained traffic control**
✅ **Service mesh visibility**

Instead of managing networking inside application code, **Istio handles it at the infrastructure level using Envoy proxies**.

---

# 🚀 DevOps Cheat Sheet

```bash
# Install Istio
istioctl install --set profile=demo -y

# Enable sidecar injection
kubectl label namespace default istio-injection=enabled

# Verify installation
istioctl verify-install

# Analyze configuration
istioctl analyze

# Check proxy sync
istioctl proxy-status

# Access Kiali
kubectl port-forward svc/kiali -n istio-system 20001:20001
```

---

⭐ **Istio transforms Kubernetes into a powerful service mesh platform with security, reliability, and observability built in.**
