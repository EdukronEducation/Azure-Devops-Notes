# Module 14: Kubernetes in Cloud Environments

## 14.1 Managed Kubernetes Services

Managed Kubernetes services provide cloud-hosted Kubernetes with automated control plane management. Services include: **Amazon EKS** (Elastic Kubernetes Service), **Azure AKS** (Azure Kubernetes Service), **Google GKE** (Google Kubernetes Engine). Managed services provide: **control plane management**, **automatic upgrades**, **integrated monitoring**, **cloud integration**. Understanding managed services helps you leverage cloud Kubernetes.

## 14.2 Amazon EKS

Amazon EKS is AWS's managed Kubernetes service. EKS provides: **managed control plane**, **AWS integration** (IAM, VPC, ELB), **multiple deployment options** (EC2, Fargate), **automatic upgrades**. Understanding EKS helps you deploy Kubernetes on AWS.

Creating EKS cluster:
```bash
# Using eksctl
eksctl create cluster \
  --name my-cluster \
  --region us-west-2 \
  --nodegroup-name standard-workers \
  --node-type t3.medium \
  --nodes 3

# Configure kubectl
aws eks update-kubeconfig --name my-cluster --region us-west-2
```

## 14.3 Azure Kubernetes Service (AKS)

AKS is Azure's managed Kubernetes service. AKS provides: **managed control plane**, **Azure integration** (Azure AD, Azure Monitor), **multiple deployment options**, **automatic upgrades**. Understanding AKS helps you deploy Kubernetes on Azure.

Creating AKS cluster:
```bash
# Create resource group
az group create --name myResourceGroup --location eastus

# Create AKS cluster
az aks create \
  --resource-group myResourceGroup \
  --name myAKSCluster \
  --node-count 3 \
  --enable-addons monitoring \
  --generate-ssh-keys

# Get credentials
az aks get-credentials --resource-group myResourceGroup --name myAKSCluster
```

## 14.4 Google Kubernetes Engine (GKE)

GKE is Google's managed Kubernetes service. GKE provides: **managed control plane**, **Google Cloud integration**, **autopilot mode** (fully managed nodes), **automatic upgrades**. Understanding GKE helps you deploy Kubernetes on Google Cloud.

Creating GKE cluster:
```bash
# Create cluster
gcloud container clusters create my-cluster \
  --zone us-central1-a \
  --num-nodes 3 \
  --machine-type n1-standard-2

# Get credentials
gcloud container clusters get-credentials my-cluster --zone us-central1-a
```

Comparison table:

| Feature | EKS | AKS | GKE |
|---------|-----|-----|-----|
| **Provider** | AWS | Azure | Google Cloud |
| **Control Plane Cost** | $0.10/hour | Free | Free |
| **Managed Nodes** | EC2, Fargate | VM Scale Sets | Standard, Autopilot |
| **Auto-upgrade** | Yes | Yes | Yes |
| **Best For** | AWS users | Azure users | GCP users |

---

## 14.5 Comparison and Selection

### Choosing a Managed Service

When selecting a managed Kubernetes service, consider: **cloud provider** (existing infrastructure), **cost** (control plane and node costs), **features** (specific capabilities needed), **integration** (with other cloud services), **compliance** (regulatory requirements), and **support** (vendor support level).

### Cost Considerations

Cost factors include: **control plane** (EKS charges $0.10/hour, AKS/GKE free), **node costs** (VM pricing), **networking** (load balancers, egress), **storage** (persistent volumes), and **add-ons** (monitoring, logging). Understanding costs helps you budget effectively.

### Migration Between Services

Migrating between managed services involves: **application portability** (Kubernetes is portable), **data migration** (persistent volumes), **configuration** (YAML files are portable), **networking** (service configurations), and **testing** (validate in new environment). Understanding migration helps you switch providers if needed.

---

## 14.6 Best Practices for Cloud Kubernetes

### Security Best Practices

Security practices include: **IAM integration** (use cloud IAM), **network policies** (control traffic), **encryption** (at rest and in transit), **secrets management** (cloud key management), **RBAC** (fine-grained access), and **image scanning** (vulnerability scanning).

### Cost Optimization

Cost optimization strategies: **right-sizing nodes** (appropriate instance types), **autoscaling** (scale based on demand), **spot instances** (for non-critical workloads), **reserved instances** (for predictable workloads), **monitoring costs** (track spending), and **cleanup** (remove unused resources).

### Monitoring and Observability

Cloud integration provides: **cloud monitoring** (native monitoring tools), **logging** (cloud logging services), **tracing** (distributed tracing), **alerts** (automated alerting), and **dashboards** (visualization). Understanding monitoring helps you maintain healthy clusters.

---

## Quick Reference

### Managed Service Commands

**EKS:**
```bash
# Create cluster
eksctl create cluster --name my-cluster

# Get credentials
aws eks update-kubeconfig --name my-cluster
```

**AKS:**
```bash
# Create cluster
az aks create --name myAKSCluster --resource-group myResourceGroup

# Get credentials
az aks get-credentials --name myAKSCluster --resource-group myResourceGroup
```

**GKE:**
```bash
# Create cluster
gcloud container clusters create my-cluster

# Get credentials
gcloud container clusters get-credentials my-cluster
```

---

## Common Pitfalls

### Pitfall 1: Not Understanding Cloud-Specific Features
**Problem**: Missing cloud integrations, higher costs
**Solution**: Learn cloud-specific features and integrations
**Prevention**: Review cloud provider documentation

### Pitfall 2: Ignoring Cost Management
**Problem**: Unexpected cloud bills
**Solution**: Monitor costs, use cost optimization strategies
**Prevention**: Set up billing alerts

### Pitfall 3: Not Configuring Cloud IAM
**Problem**: Security vulnerabilities, access issues
**Solution**: Integrate with cloud IAM, use RBAC
**Prevention**: Follow security best practices

---

## Best Practices

1. **Choose Right Service**: Based on cloud provider and needs
2. **Integrate Cloud Services**: Use cloud-native features
3. **Monitor Costs**: Track and optimize spending
4. **Use Cloud IAM**: Integrate authentication
5. **Enable Monitoring**: Use cloud monitoring tools
6. **Configure Networking**: Proper VPC/networking setup
7. **Backup Data**: Use cloud backup services
8. **Plan for HA**: Multi-zone deployments
9. **Use Managed Add-ons**: When available
10. **Review Regularly**: Optimize configuration

---

## Further Reading

### Official Documentation
- [Amazon EKS](https://docs.aws.amazon.com/eks/)
- [Azure AKS](https://docs.microsoft.com/azure/aks/)
- [Google GKE](https://cloud.google.com/kubernetes-engine/docs)

### Related Topics
- Core Concepts (Module 2)
- Security (Module 8)
- Monitoring and Logging (Module 9)

---

*This module covers Kubernetes in cloud environments. Understanding managed services helps you leverage cloud providers for Kubernetes deployments.*

