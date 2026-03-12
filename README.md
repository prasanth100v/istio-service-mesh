# Istio Service Mesh -- Complete DevOps Glossary (100+ Terms)

A comprehensive glossary of common Istio and service mesh concepts
useful for DevOps, Kubernetes, and cloud engineers.

------------------------------------------------------------------------

## A

**API Gateway**\
A gateway that manages external access to services including routing,
authentication, and rate limiting.

**Access Log**\
Logs produced by Envoy proxies that record request details such as
response code, latency, and path.

**Authentication**\
Process of verifying the identity of a service or user before allowing
access.

**Authorization Policy**\
Istio security configuration that defines which workloads can access
other workloads.

**Auto mTLS**\
Istio feature that automatically enables mutual TLS between services
when possible.

------------------------------------------------------------------------

## B

**Blue‑Green Deployment**\
A release strategy where two environments run simultaneously to reduce
downtime during deployments.

**Backend Service**\
The internal service that processes requests sent by other services.

**Bypass Traffic**\
Traffic that avoids the service mesh proxies.

------------------------------------------------------------------------

## C

**Canary Deployment**\
Gradual rollout of a new application version to a subset of users.

**Circuit Breaker**\
A resilience mechanism that stops sending traffic to unhealthy services.

**Citadel**\
Istio component previously responsible for certificate management (now
merged into Istiod).

**Control Plane**\
Istio components responsible for configuration, policy management, and
service discovery.

**Configuration Distribution**\
Process of pushing configuration updates from Istiod to Envoy proxies.

**CRD (Custom Resource Definition)**\
Kubernetes extension used to define Istio resources such as
VirtualService and DestinationRule.

------------------------------------------------------------------------

## D

**Data Plane**\
Network of Envoy proxies handling service‑to‑service communication.

**Destination Rule**\
Defines traffic policies applied after routing decisions.

**Distributed Tracing**\
Tracking requests across multiple services using tracing tools.

**Dynamic Configuration**\
Configuration updates applied without restarting services.

------------------------------------------------------------------------

## E

**Egress Traffic**\
Traffic leaving the service mesh to external services.

**Egress Gateway**\
Dedicated gateway controlling outbound traffic from the mesh.

**Envoy Proxy**\
High‑performance proxy used by Istio as the sidecar container.

**External Service**\
A service located outside the Kubernetes cluster.

------------------------------------------------------------------------

## F

**Fault Injection**\
Technique used to test resilience by injecting delays or errors.

**Forward Proxy**\
Proxy that forwards client requests to external servers.

------------------------------------------------------------------------

## G

**Gateway**\
Istio resource that manages inbound and outbound traffic at the edge of
the mesh.

**Grafana**\
Visualization tool used to display metrics from Prometheus.

------------------------------------------------------------------------

## H

**HTTP Routing**\
Routing decisions based on HTTP headers, URI paths, or cookies.

**Health Check**\
Method used to determine if a service instance is healthy.

------------------------------------------------------------------------

## I

**Ingress Gateway**\
Entry point for traffic entering the service mesh.

**Istio**\
Open‑source service mesh providing traffic management, security, and
observability.

**Istiod**\
Unified control plane component combining Pilot, Citadel, and Galley.

**Identity**\
Unique identity assigned to workloads for secure communication.

------------------------------------------------------------------------

## J

**JWT (JSON Web Token)**\
Token used for authentication and authorization.

**Jaeger**\
Distributed tracing system commonly used with Istio.

------------------------------------------------------------------------

## K

**Kiali**\
Service mesh observability dashboard for Istio.

**Kubernetes**\
Container orchestration platform where Istio commonly runs.

------------------------------------------------------------------------

## L

**Latency**\
Time taken for a request to travel from client to service and back.

**Load Balancing**\
Distribution of traffic among multiple service instances.

**Locality Load Balancing**\
Routing traffic to the closest service instance.

------------------------------------------------------------------------

## M

**mTLS (Mutual TLS)**\
Encryption protocol ensuring both client and server authenticate each
other.

**Mesh Policy**\
Security rules applied across the entire service mesh.

**Metrics**\
Performance data collected from services.

------------------------------------------------------------------------

## N

**Namespace**\
Kubernetes isolation boundary for resources.

**Network Policy**\
Rules controlling network communication between pods.

------------------------------------------------------------------------

## O

**Observability**\
Ability to monitor system behavior through logs, metrics, and traces.

**OpenTelemetry**\
Framework for collecting telemetry data.

------------------------------------------------------------------------

## P

**Pilot**\
Istio component responsible for traffic management configuration (now
part of Istiod).

**Policy Enforcement**\
Applying rules that govern traffic behavior.

**Prometheus**\
Monitoring system used for metrics collection in Istio.

**Proxy Status**\
Command used to check Envoy proxy configuration.

------------------------------------------------------------------------

## Q

**Quota**\
Limit placed on resource usage.

**Query Routing**\
Routing requests based on query parameters.

------------------------------------------------------------------------

## R

**Rate Limiting**\
Restricting the number of requests allowed in a given time.

**RBAC (Role‑Based Access Control)**\
Security mechanism restricting access based on roles.

**Retries**\
Automatic retry of failed service requests.

------------------------------------------------------------------------

## S

**Service Mesh**\
Infrastructure layer that manages service‑to‑service communication.

**Sidecar Proxy**\
Proxy container deployed alongside application containers.

**Service Entry**\
Istio configuration enabling access to external services.

**Subset**\
A versioned group of service instances.

**Service Discovery**\
Process of identifying service instances dynamically.

------------------------------------------------------------------------

## T

**Telemetry**\
Collection of metrics, logs, and traces from services.

**Traffic Shifting**\
Gradually redirecting traffic between service versions.

**Timeout**\
Maximum time allowed for a request before failure.

------------------------------------------------------------------------

## V

**Virtual Service**\
Defines routing rules for traffic within the service mesh.

**Version Routing**\
Sending traffic to specific application versions.

------------------------------------------------------------------------

## W

**Workload**\
Application instance running inside the mesh.

**Workload Identity**\
Identity assigned to workloads for authentication.

------------------------------------------------------------------------

## X

**xDS API**\
Envoy configuration APIs used by Istio control plane.

------------------------------------------------------------------------

## Z

**Zero Trust Security**\
Security model where every request is verified regardless of origin.
