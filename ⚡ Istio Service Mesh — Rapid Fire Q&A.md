## ⚡ Istio Service Mesh — Rapid Fire Q&A
| 🔢 Q#   | ❓ Question                                  | 💡 Answer                                                                    |
| ------- | -------------------------------------------- | ---------------------------------------------------------------------------- |
| 🔹 Q1   | What is Istio?                               | 👉 Istio — Open-source service mesh for `managing microservice communication.` |
| 🔹 Q2   | What problem does Istio solve?               | 👉 `Traffic management`, `security`, `observability`, and `reliability.`        |
| 🔹 Q3   | What is a service mesh?                      | 👉 Infrastructure layer `managing service-to-service communication`.           |
| 🏗️ Q4  | Main components of Istio?                    |  Control Plane <br> Data Plane                                             |
| 🏗️ Q5  | What is the control plane?                   | 👉 Manages configuration and policies.                                       |
| 🏗️ Q6  | What is the data plane?                      | 👉 Sidecar proxies `handling traffic.  `                                       |
| 🌐 Q7   | Which proxy does Istio use?                  | 👉 `Envoy  `                                                                   |
| 🌐 Q8   | What is a sidecar proxy?                     | 👉 Proxy container attached to application pod.                              |
| 🌐 Q8   | what is proxy container?                     | proxy container is a `helper container` deployed alongside a main application to` handle network traffic` |
| ⚙️ Q9   | How to enable automatic sidecar injection?   | 👉 `kubectl label namespace default istio-injection=enabled`                 |
| ⚙️ Q10  | What happens after injection enabled?        | 👉 `Envoy sidecar automatically added to pods. `                               |
| 🚦 Q11  | What is VirtualService?                      | 👉 Defines `traffic routing rules. `                                           |
| 🚦 Q12  | What is DestinationRule?                     | 👉 Defines `traffic policies` for services.                                    |
| 🚦 Q13  | What is Gateway in Istio?                    | 👉 `Entry point for external traffic.  `                                       |
| 🔀 Q14  | What is traffic splitting?                   | 👉 Route traffic between service versions.                                   |
| 🔀 Q15  | Example use case?                            | 👉 Canary deployments.                                                       |
| 🔀 Q16  | What is retry policy?                        | 👉 Automatically retries failed requests.                                    |
| 🔀 Q17  | What is timeout policy?                      | 👉 `Limits request duration.`                                                  |
| 🛡️ Q18 | What is circuit breaking?                    | 👉 Prevents `cascading failures. `                                           |
| 🛡️ Q18 | What is cascading failure?                 | In Istio, a cascading failure is a chain reaction where failure of one microservice causes other dependent services to fail, eventually affecting the entire application system. |
| 🛡️ Q19 | 🔹 Key Goal in Istio "Fail fast instead of fail everywhere."  | 👉 quickly stopping requests to a `failing service` so the failure does not spread across the entire system.   |
| 🛡️ Q19 | What is fault injection?                     | 👉 Simulates delays/errors for testing.                                      |
| 🔐 Q20  | Does Istio support mTLS?                     | 👉 ✅` Yes`                                                                     |
| 🔐 Q21  | What is mTLS?                                | 👉 Mutual TLS for encrypted service communication.                           |
| 🔐 Q22  | What is PeerAuthentication?                  | 👉 Configures mTLS behavior.                                                 |
| 🔒 Q23  | What is AuthorizationPolicy?                 | 👉 Controls service `access permissions.`                                      |
| 🔒 Q24  | Why use authorization policies?              | 👉 `Zero-trust security. `                                                     |
| 📊 Q25  | What telemetry does Istio provide?           | 👉 `Metrics`, `logs`, `traces`.                                                    |
| 📊 Q26  | Monitoring tools commonly integrated?        |  Prometheus <br> Grafana <br> Jaeger                                       |
| 🔍 Q27  | What is distributed tracing?                 | 👉 Tracking requests across microservices.                                   |
| ☸️ Q28  | Does Istio work natively with Kubernetes?    | 👉 ✅ Yes                                                                     |
| ☸️ Q29  | How is Istio usually installed?              | 👉 `istioctl`, Helm, or Operator.                                            |
| 🌍 Q30  | What is Ingress Gateway?                     | 👉 Handles incoming external traffic.                                        |
| 🌍 Q31  | What is Egress Gateway?                      | 👉 Controls outbound external traffic.                                       |
| 🚀 Q32  | How does Istio help `blue-green deployments`?| 👉 Advanced traffic routing.                                                 |
| 🚀 Q33  | How does Istio support `canary deployments`?  | 👉 Weighted traffic splitting.                                               |
| ⚡ Q34   | Does Istio add latency?                      | 👉 ✅ Slightly (due to sidecar proxies).                                      |
| ⚡ Q35   | Why resource usage increases with Istio?     | 👉 Envoy sidecars consume `CPU/memory. `                                       |
| 🛠️ Q36 | Sidecar not injected — checks?               | 👉 Namespace label + webhook.                                                |
| 🛠️ Q37 | Traffic routing not working — checks?        | 👉` VirtualService + Gateway config.  `                                        |
| 🌐 Q38  | What is multi-cluster Istio?                 | 👉 Single mesh across multiple Kubernetes clusters.                          |
| 🏭 Q39  | Why use Istio in microservices architecture? | 👉 Centralized traffic/security management.                                  |
| 🏭 Q40  | Why not use Istio for small apps?            | 👉 Added `complexity` and `overhead`.                                            |
| 🔎 Q41  | What is service discovery in Istio?          | 👉 `Automatic detection of services in mesh.   `                               |
| 🔎 Q42  | What is mesh expansion?                      | 👉 `Extending mesh` outside Kubernetes.                                        |
| 🔄 Q43  | How does Istio improve deployments?          | 👉 Safer rollouts using traffic shifting.                                    |
| 🔄 Q44  | Common deployment strategy with Istio?       | 👉 `Canary deployment.`                                                        |
| 🔐 Q45  | Why enable strict mTLS?                      | 👉 `Encrypt all internal communication. `                                      |
| 🔐 Q46  | Why use least-privilege authorization?       | 👉 Reduce attack surface.                                                    |
| 🆚 Q47  | Istio vs Kubernetes native networking?       | 👉 Istio adds advanced traffic/security/observability features.              |
| 🆚 Q48  | Istio vs Linkerd?                            |  Istio = feature-rich <br> 👉 Linkerd = lightweight/simple                 |
| 🧪 Q49  | Check proxy status?                          | 👉 `istioctl proxy-status`                                                   |
| 🧪 Q50  | Analyze Istio config issues?                 | 👉 `istioctl analyze`                                                        |
