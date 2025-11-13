# Module 16: Kubernetes Advanced Topics

## 16.1 Custom Resources (CRDs)

Custom Resource Definitions (CRDs) extend Kubernetes API with custom resources. CRDs enable: **domain-specific resources** (application-specific objects), **API extension** (new resource types), **declarative management** (kubectl with custom resources), **operator pattern** (custom controllers). Understanding CRDs enables Kubernetes extensibility.

CRD example:
```yaml
apiVersion: apiextensions.k8s.io/v1
kind: CustomResourceDefinition
metadata:
  name: databases.example.com
spec:
  group: example.com
  versions:
  - name: v1
    served: true
    storage: true
    schema:
      openAPIV3Schema:
        type: object
        properties:
          spec:
            type: object
            properties:
              size:
                type: string
              version:
                type: string
  scope: Namespaced
  names:
    plural: databases
    singular: database
    kind: Database
```

Using custom resource:
```yaml
apiVersion: example.com/v1
kind: Database
metadata:
  name: my-database
spec:
  size: "10Gi"
  version: "13"
```

## 16.2 Operators

Operators are Kubernetes extensions that use CRDs and custom controllers to manage complex applications. Operators encode operational knowledge, automating: **deployment**, **scaling**, **backup/restore**, **upgrades**, **failure recovery**. Popular operators: **Prometheus Operator**, **MySQL Operator**, **PostgreSQL Operator**. Understanding Operators enables automated application management.

Operator Framework simplifies operator development. Operator SDK provides tools for building operators. Understanding the Operator pattern helps you automate complex applications.

## 16.3 Service Mesh

Service mesh provides infrastructure layer for service-to-service communication. Service mesh features: **traffic management** (routing, load balancing), **security** (mTLS, authentication), **observability** (metrics, tracing), **resilience** (retries, timeouts). Popular service meshes: **Istio**, **Linkerd**, **Consul**. Understanding service mesh enables advanced microservices management.

Istio example:
```yaml
apiVersion: networking.istio.io/v1beta1
kind: VirtualService
metadata:
  name: reviews
spec:
  hosts:
  - reviews
  http:
  - match:
    - headers:
        end-user:
          exact: jason
    route:
    - destination:
        host: reviews
        subset: v2
  - route:
    - destination:
        host: reviews
        subset: v1
```

## 16.4 High Availability

High availability ensures cluster and application resilience. HA strategies: **multi-master clusters** (redundant control planes), **etcd HA** (etcd clustering), **node redundancy** (multiple nodes), **pod replicas** (multiple instances), **pod disruption budgets** (availability guarantees), **multi-zone deployment** (geographic distribution). Understanding HA ensures reliable Kubernetes deployments.

HA control plane:
```
┌───────────────┐    ┌───────────────┐    ┌───────────────┐
│ Master Node 1 │    │ Master Node 2 │    │ Master Node 3 │
│   API Server  │    │   API Server  │    │   API Server  │
│   Scheduler   │    │   Scheduler   │    │   Scheduler   │
│   Controller  │    │   Controller  │    │   Controller  │
└───────┬───────┘    └───────┬───────┘    └───────┬───────┘
        │                    │                    │
        └────────────────────┴────────────────────┘
                            │
                      ┌─────┴─────┐
                      │   etcd    │
                      │  Cluster  │
                      └───────────┘
```

---

## 16.5 Operator Development

### Operator SDK

Operator SDK provides tools for building operators: **scaffolding** (project structure), **code generation** (boilerplate), **testing** (unit and e2e tests), and **packaging** (distribution). Understanding Operator SDK helps you build custom operators.

### Operator Patterns

Common operator patterns: **stateful applications** (databases, queues), **complex deployments** (multi-component apps), **lifecycle management** (backup, restore, upgrade), and **integration** (external systems). Understanding patterns helps you design operators.

---

## 16.6 Service Mesh Deep Dive

### Istio Architecture

Istio components: **Control Plane** (Pilot, Citadel, Galley), **Data Plane** (Envoy proxies), **Management** (configuration, policies). Understanding Istio architecture helps you deploy and manage service mesh.

### Service Mesh Benefits

Service mesh provides: **traffic management** (routing, load balancing), **security** (mTLS, authentication), **observability** (metrics, tracing, logging), **resilience** (retries, timeouts, circuit breakers), and **policy enforcement** (rate limiting, access control).

---

## 16.7 High Availability Patterns

### Multi-Zone Deployment

Multi-zone deployment ensures: **geographic distribution** (across availability zones), **fault tolerance** (zone failures), **load distribution** (balanced across zones), and **compliance** (data residency). Understanding multi-zone helps you design resilient clusters.

### Pod Disruption Budgets

Pod Disruption Budgets (PDBs) ensure: **availability guarantees** (minimum available Pods), **voluntary disruptions** (controlled maintenance), **involuntary disruptions** (node failures), and **application resilience** (maintain service during disruptions).

---

## Quick Reference

### CRD Commands
```bash
# Get CRDs
kubectl get crds

# Describe CRD
kubectl describe crd <name>

# Get custom resources
kubectl get <custom-resource>
```

### Operator Commands
```bash
# Install operator
kubectl apply -f operator.yaml

# Check operator status
kubectl get operators
```

### Service Mesh Commands
```bash
# Istio installation
istioctl install

# Check Istio status
istioctl verify-install
```

---

## Common Pitfalls

### Pitfall 1: Over-Engineering with CRDs
**Problem**: Unnecessary complexity
**Solution**: Use standard resources when possible
**Prevention**: Evaluate need for CRDs

### Pitfall 2: Service Mesh Complexity
**Problem**: Difficult to manage, performance overhead
**Solution**: Start simple, add complexity gradually
**Prevention**: Understand service mesh requirements

### Pitfall 3: Inadequate HA Planning
**Problem**: Single points of failure
**Solution**: Design for multi-zone, redundancy
**Prevention**: Plan HA from the start

---

## Best Practices

1. **Use CRDs When Needed**: Extend Kubernetes appropriately
2. **Start Simple**: Add complexity gradually
3. **Design for HA**: Multi-zone, redundancy
4. **Monitor Operators**: Track operator health
5. **Test Service Mesh**: Verify traffic policies
6. **Document Custom Resources**: Clear documentation
7. **Use Established Patterns**: Follow best practices
8. **Plan for Scale**: Design for growth
9. **Security First**: Secure custom resources
10. **Regular Reviews**: Optimize advanced features

---

## Further Reading

### Official Documentation
- [Custom Resources](https://kubernetes.io/docs/concepts/extend-kubernetes/api-extension/custom-resources/)
- [Operators](https://kubernetes.io/docs/concepts/extend-kubernetes/operator/)
- [Istio](https://istio.io/latest/docs/)
- [High Availability](https://kubernetes.io/docs/setup/best-practices/multiple-zones/)

### Related Topics
- Core Concepts (Module 2)
- Workloads (Module 6)
- CI/CD Integration (Module 15)

---

*This module covers advanced Kubernetes topics including CRDs, Operators, Service Mesh, and High Availability. Understanding these topics enables sophisticated Kubernetes deployments.*

