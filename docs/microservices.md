# Core Microservices Concepts


| Concept                    | Spring Component                            | Cloud-Native Alternative                      |
| -------------------------- | ------------------------------------------- | --------------------------------------------- |
| **Service Creation**       | Spring Boot (`@RestController`)             | Quarkus, Micronaut                            |
| **Routing / API Gateway**  | Spring Cloud Gateway                        | Kubernetes Ingress, Istio Gateway, Ambassador |
| **Service Discovery**      | Eureka (Netflix OSS)                        | Kubernetes DNS, Consul, Istio                 |
| **Config Management**      | Spring Cloud Config Server (Git-backed)     | K8s ConfigMap/Secret, HashiCorp Vault         |
| **Circuit Breaker**        | Resilience4j / Spring Cloud Circuit Breaker | Istio, Envoy, Hystrix (legacy)                |
| **Load Balancing**         | Spring Cloud LoadBalancer / Ribbon          | Kubernetes Service (`ClusterIP`)              |
| **Security (AuthN/AuthZ)** | Spring Security + OAuth2 + Keycloak         | Istio AuthorizationPolicy, AWS Cognito, IAM   |
| **Messaging / Events**     | Spring Kafka / Spring AMQP                  | Apache Kafka, RabbitMQ, NATS, Pub/Sub         |
| **Observability**          | Micrometer + Actuator                       | Prometheus + Grafana, OpenTelemetry           |
| **Distributed Tracing**    | Spring Cloud Sleuth + Zipkin                | OpenTelemetry, Jaeger                         |
| **Logging**                | Logback + Logstash / Filebeat               | Fluentd / FluentBit + Elasticsearch           |
| **Testing**                | Spring Boot Test, Testcontainers            | REST Assured, Postman, K6, Cypress            |
| **Deployment**             | JAR + Docker                                | Kubernetes, Knative, Cloud Run                |
| **Scaling**                | HPA (metrics-based)                         | K8s HPA/VPA, Cluster Autoscaler               |
| **CI/CD**                  | Jenkins, GitHub Actions                     | ArgoCD, Tekton, GitLab CI/CD                  |


# Microservices Design Patterns

### **What is a Saga?**

A Saga is a sequence of local transactions across microservices, coordinated to ensure eventual consistency without distributed transactions.

Each step in a Saga has a corresponding compensating action to undo it if a later step fails. This ensures system rollback and data consistency.

### **Real-Time Scenario Implementation**

1. **Orchestration** – A central Saga orchestrator calls each service and handles success/failure.
2. **Choreography** – Services communicate via events, reacting to each other's state changes.
3. **Forward flow** – Execute business steps (e.g., Create Order → Charge Payment → Ship Order).
4. **Failure handling** – Trigger compensations (e.g., Refund Payment → Cancel Order) in reverse order using events.


### **Best Practices & Considerations**

* Design **compensating transactions** explicitly; they are not automatic.
* Ensure all services are **idempotent** and **resilient to retries**.
* Use **message brokers** (Kafka, RabbitMQ) for event-driven communication.
* Track saga state if using choreography (e.g., via a state machine or Saga log).
* Prefer orchestration when control flow needs to be tightly managed.
* Avoid long-running locks or tight coupling between services.
* Log failures and alert support for manual recovery if needed.
