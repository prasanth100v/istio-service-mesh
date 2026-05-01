| 🔢 Q#   | ❓ Question                                 | 💡 Answer                                                                     |
| ------- | ------------------------------------------ | ----------------------------------------------------------------------------- |
| 🔹 Q1   | What is Istio?                             | 👉 An open-source service mesh that manages service-to-service communication. |
| 🔹 Q2   | What problem does Istio solve?             | 👉 Traffic management, security, observability in microservices.              |
| 🔹 Q3   | What is a service mesh?                    | 👉 Infrastructure layer that controls communication between services.         |
| 🏗️ Q4  | Main components of Istio?                  | 👉 Data plane + control plane                                                 |
| 🏗️ Q5  | What is data plane?                        | 👉 Sidecar proxies handling traffic.                                          |
| 🏗️ Q6  | What is control plane?                     | 👉 Manages configuration and policies.                                        |
| 🏗️ Q7  | What proxy does Istio use?                 | 👉 Envoy                                                                      |
| 🔄 Q8   | What is a sidecar proxy?                   | 👉 Proxy container injected into each pod.                                    |
| 🔄 Q9   | How is sidecar injected?                   | 👉 Automatic injection using namespace label.                                 |
| 🔄 Q10  | Why sidecar pattern?                       | 👉 Separates application logic from networking logic.                         |
| 🌐 Q11  | What is VirtualService?                    | 👉 Defines routing rules.                                                     |
| 🌐 Q12  | What is DestinationRule?                   | 👉 Defines policies for traffic (load balancing, subsets).                    |
| 🌐 Q13  | What is Gateway?                           | 👉 Entry point for external traffic.                                          |
| 🌐 Q14  | What is canary deployment in Istio?        | 👉 Splitting traffic between versions.                                        |
| 🔐 Q15  | Does Istio support mTLS?                   | 👉 ✅ Yes                                                                      |
| 🔐 Q16  | What is mTLS?                              | 👉 Mutual TLS — encrypts service-to-service communication.                    |
| 🔐 Q17  | What is PeerAuthentication?                | 👉 Configures mTLS settings.                                                  |
| 🔐 Q18  | What is AuthorizationPolicy?               | 👉 Controls access between services.                                          |
| 📊 Q19  | What telemetry does Istio provide?         | 👉 Metrics, logs, traces.                                                     |
| 📊 Q20  | Integration tools?                         | 👉 Prometheus, Grafana, Jaeger                                                |
| 📊 Q21  | What is distributed tracing?               | 👉 Tracks request flow across services.                                       |
| ☸️ Q22  | How to enable Istio in a namespace?        | 👉 Label namespace: kubectl label namespace default istio-injection=enabled   |
| ☸️ Q23  | What happens after enabling injection?     | 👉 Sidecar proxy automatically added to pods.                                 |
| 🚦 Q24  | What is traffic splitting?                 | 👉 Route % of traffic to different versions.                                  |
| 🚦 Q25  | What is circuit breaking?                  | 👉 Prevents cascading failures.                                               |
| 🚦 Q26  | What is retry mechanism?                   | 👉 Automatically retries failed requests.                                     |
| 🚦 Q27  | What is timeout?                           | 👉 Limits request duration.                                                   |
| 🧪 Q28  | What is fault injection?                   | 👉 Simulating failures (delay/errors).                                        |
| 🧪 Q29  | Why use it?                                | 👉 Test system resilience.                                                    |
| ⚙️ Q30  | How to install Istio?                      | 👉 istioctl / Helm                                                            |
| ⚙️ Q31  | What is IstioOperator?                     | 👉 Custom resource for installation config.                                   |
| 🚀 Q32  | Does Istio add overhead?                   | 👉 ✅ Yes (sidecar latency + resource usage)                                   |
| 🚀 Q33  | How to reduce overhead?                    | 👉 Optimize config, reduce telemetry, use sidecar selectively.                |
| 🛠️ Q34 | No traffic routing working?                | 👉 Check VirtualService + Gateway config.                                     |
| 🛠️ Q35 | mTLS issues?                               | 👉 Check PeerAuthentication settings.                                         |
| 🛠️ Q36 | Sidecar not injected?                      | 👉 Check namespace label.                                                     |
| 🌍 Q37  | What is multi-cluster Istio?               | 👉 Managing multiple Kubernetes clusters with one mesh.                       |
| 🌍 Q38  | What is ingress gateway?                   | 👉 Handles external traffic into mesh.                                        |
| 🌍 Q39  | What is egress gateway?                    | 👉 Controls outbound traffic.                                                 |
| 🎯 Q40  | Why use Istio over Kubernetes native?      | 👉 Advanced traffic control, security, observability.                         |
| 🎯 Q41  | How to implement zero-downtime deployment? | 👉 Canary + traffic shifting.                                                 |
| 🎯 Q42  | Why latency increased after Istio?         | 👉 Sidecar overhead.                                                          |
| 🎯 Q43  | When NOT to use Istio?                     | 👉 Small/simple applications                                                  |
