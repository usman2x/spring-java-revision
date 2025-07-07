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
