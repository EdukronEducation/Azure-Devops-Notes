# Module 8: Kubernetes Security

## 8.1 Authentication and Authorization

### Authentication Methods

Kubernetes supports multiple authentication methods: **client certificates** (X.509 certificates), **bearer tokens** (static tokens, service account tokens), **authenticating proxy** (external authentication), **OpenID Connect** (OIDC), and **webhook token authentication** (external token validation). Authentication verifies user identity before allowing access.

Authentication flow: **user/process makes request**, **API server authenticates**, **authentication succeeds/fails**, **authorization checked if authenticated**. Understanding authentication helps you secure cluster access.

### Service Accounts

Service accounts are non-human identities for Pods to authenticate to the API server. Service accounts provide: **automatic token mounting** (tokens available in Pods), **API access** (Pods can call Kubernetes API), **RBAC integration** (permissions via Roles/RoleBindings), and **image pull secrets** (for private registries). Understanding service accounts helps you enable Pod-to-API communication.

### RBAC (Role-Based Access Control)

RBAC provides fine-grained access control based on roles and bindings. RBAC components: **Roles** (namespace-scoped permissions), **RoleBindings** (bind Roles to subjects), **ClusterRoles** (cluster-scoped permissions), **ClusterRoleBindings** (bind ClusterRoles to subjects). RBAC enables least-privilege access control.

RBAC example:
```yaml
apiVersion: rbac.authorization.k8s.io/v1
kind: Role
metadata:
  namespace: default
  name: pod-reader
rules:
- apiGroups: [""]
  resources: ["pods"]
  verbs: ["get", "watch", "list"]
---
apiVersion: rbac.authorization.k8s.io/v1
kind: RoleBinding
metadata:
  name: read-pods
  namespace: default
subjects:
- kind: User
  name: jane
  apiGroup: rbac.authorization.k8s.io
roleRef:
  kind: Role
  name: pod-reader
  apiGroup: rbac.authorization.k8s.io
```

### Roles and RoleBindings

Roles define permissions within a namespace. RoleBindings associate Roles with subjects (users, groups, service accounts). Roles specify: **resources** (what to access), **verbs** (what actions allowed), **apiGroups** (which API groups). Understanding Roles and RoleBindings helps you implement namespace-level access control.

### ClusterRoles and ClusterRoleBindings

ClusterRoles define cluster-wide permissions. ClusterRoleBindings associate ClusterRoles with subjects. ClusterRoles are used for: **cluster administration** (admin access), **system components** (kubelet, controllers), and **cross-namespace access** (cluster-wide resources). Understanding ClusterRoles helps you implement cluster-level access control.

---

## 8.2 RBAC

### RBAC Concepts

RBAC provides declarative access control through Roles and bindings. RBAC principles: **least privilege** (minimum necessary permissions), **separation of duties** (different roles for different tasks), **audit trail** (who did what), and **principle of least privilege** (minimum access needed). Understanding RBAC concepts helps you design secure access control.

### Role Resources

Roles define permissions for resources within a namespace. Roles specify: **apiGroups** (API groups to access), **resources** (resource types), **verbs** (actions: get, list, create, update, delete, patch, watch), **resourceNames** (specific resources), and **nonResourceURLs** (non-resource URLs). Understanding Role resources helps you define permissions precisely.

### RoleBinding Resources

RoleBindings associate Roles with subjects. RoleBindings specify: **subjects** (users, groups, service accounts), **roleRef** (which Role to bind), and **namespace** (where binding applies). Understanding RoleBindings helps you grant permissions.

### ClusterRole Resources

ClusterRoles define cluster-wide permissions. ClusterRoles can access: **cluster-scoped resources** (Nodes, PersistentVolumes), **non-resource URLs** (/healthz), **all namespaces** (via binding), and **aggregated rules** (combining multiple ClusterRoles). Understanding ClusterRoles helps you define cluster-level permissions.

### ClusterRoleBinding Resources

ClusterRoleBindings associate ClusterRoles with subjects cluster-wide. ClusterRoleBindings grant permissions across all namespaces. Understanding ClusterRoleBindings helps you implement cluster-level access.

---

## 8.3 Service Accounts

### Service Account Concepts

Service accounts provide identities for Pods. Service accounts enable: **API authentication** (Pods authenticate to API server), **RBAC integration** (Pods get permissions via RoleBindings), **image pull secrets** (access to private registries), and **automatic token mounting** (tokens available in Pods). Understanding service accounts helps you enable Pod-to-API communication.

### Default Service Accounts

Each namespace has a default service account. Pods without specified service accounts use the default. Default service accounts start with minimal permissions. Understanding defaults helps you configure service accounts appropriately.

### Service Account Tokens

Service account tokens authenticate Pods to the API server. Tokens are automatically mounted at `/var/run/secrets/kubernetes.io/serviceaccount/token`. Tokens are used for API authentication. Understanding tokens helps you enable Pod API access.

### Image Pull Secrets

Image pull secrets authenticate to container registries. Secrets can be: **associated with service accounts** (automatic for Pods), **specified in Pod specs** (per-Pod secrets), or **default for namespace** (via service account). Understanding image pull secrets helps you access private registries.

### Service Account Best Practices

Service account best practices: **create dedicated accounts** (one per application), **use RBAC** (grant minimum permissions), **rotate tokens** (regular rotation), **audit usage** (monitor access), and **document permissions** (explain why). Following best practices ensures secure service account usage.

---

## 8.4 Pod Security

### Pod Security Policies

Pod Security Policies (PSP) control Pod creation parameters. PSPs define: **privileged containers** (allow/disallow), **host namespaces** (hostNetwork, hostPID), **volumes** (allowed volume types), **host paths** (allowed host paths), **capabilities** (Linux capabilities), and **seccomp profiles** (security profiles). PSPs are deprecated in favor of Pod Security Standards.

### Security Contexts

Security contexts define privileges and access control for Pods and containers. Security contexts specify: **runAsUser** (user ID), **runAsGroup** (group ID), **fsGroup** (file system group), **capabilities** (Linux capabilities), **readOnlyRootFilesystem** (read-only root), and **seccompProfile** (security profile). Understanding security contexts helps you restrict Pod privileges.

Security context example:
```yaml
spec:
  securityContext:
    runAsNonRoot: true
    runAsUser: 1000
    fsGroup: 2000
  containers:
  - name: app
    securityContext:
      allowPrivilegeEscalation: false
      capabilities:
        drop:
        - ALL
      readOnlyRootFilesystem: true
```

### Pod Security Standards

Pod Security Standards define three policies: **Privileged** (unrestricted), **Baseline** (minimally restrictive), **Restricted** (highly restrictive). Standards provide consistent security policies. Understanding standards helps you implement pod security.

### Network Policies

Network Policies control network traffic (covered in Module 4). Network Policies provide network-level security and isolation. Understanding Network Policies helps you implement network security.

### Security Best Practices

Pod security best practices: **run as non-root** (use non-root users), **drop capabilities** (remove unnecessary capabilities), **read-only root filesystem** (when possible), **use security contexts** (restrict privileges), **implement Network Policies** (control traffic), and **scan images** (check for vulnerabilities). Following best practices ensures secure Pods.

---

## Quick Reference

### RBAC Components
- `Role` - Namespace-scoped permissions
- `ClusterRole` - Cluster-scoped permissions
- `RoleBinding` - Bind Role to subjects
- `ClusterRoleBinding` - Bind ClusterRole to subjects

### Security Commands
```bash
# Get roles
kubectl get roles
kubectl get clusterroles

# Get bindings
kubectl get rolebindings
kubectl get clusterrolebindings

# Check permissions
kubectl auth can-i create pods
```

---

## Common Pitfalls

### Pitfall 1: Overly Permissive RBAC
**Problem**: Security risk, excessive access
**Solution**: Follow least privilege principle
**Prevention**: Regular RBAC audits

### Pitfall 2: Running as Root
**Problem**: Security vulnerability
**Solution**: Use non-root users, security contexts
**Prevention**: Enforce security policies

### Pitfall 3: Not Rotating Service Account Tokens
**Problem**: Compromised tokens remain valid
**Solution**: Implement token rotation
**Prevention**: Use token management

---

## Best Practices

1. **Follow Least Privilege**: Minimum required permissions
2. **Use RBAC**: Fine-grained access control
3. **Run as Non-Root**: Use security contexts
4. **Drop Capabilities**: Remove unnecessary privileges
5. **Implement Network Policies**: Control traffic
6. **Rotate Credentials**: Regular rotation
7. **Scan Images**: Check for vulnerabilities
8. **Audit Access**: Monitor RBAC usage
9. **Use Pod Security Standards**: Consistent policies
10. **Document Security Policies**: Clear guidelines

---

## Further Reading

### Official Documentation
- [Kubernetes Security](https://kubernetes.io/docs/concepts/security/)
- [RBAC](https://kubernetes.io/docs/reference/access-authn-authz/rbac/)
- [Pod Security Standards](https://kubernetes.io/docs/concepts/security/pod-security-standards/)

### Related Topics
- Networking (Module 4)
- Security Best Practices (Module 18)
- Troubleshooting (Module 17)

---

*This module covers Kubernetes security including authentication, authorization, RBAC, service accounts, and pod security. Understanding security is essential for protecting Kubernetes clusters and applications.*

