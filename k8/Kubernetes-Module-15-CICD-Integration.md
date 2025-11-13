# Module 15: Kubernetes CI/CD Integration

## 15.1 CI/CD Concepts

CI/CD with Kubernetes automates application deployment. **Continuous Integration** (automated builds and tests), **Continuous Deployment** (automated deployment to clusters), **GitOps** (Git as source of truth). Understanding CI/CD enables automated Kubernetes workflows.

## 15.2 GitOps with Kubernetes

GitOps uses Git as the single source of truth for cluster configuration. GitOps principles: **declarative configuration** (in Git), **version control** (all changes tracked), **automated synchronization** (tools sync Git to cluster), **pull-based deployment** (agents pull from Git). GitOps tools: **ArgoCD** (popular GitOps tool), **Flux** (CNCF project). Understanding GitOps enables declarative deployments.

ArgoCD example:
```bash
# Install ArgoCD
kubectl create namespace argocd
kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml

# Create application
kubectl apply -f - <<EOF
apiVersion: argoproj.io/v1alpha1
kind: Application
metadata:
  name: myapp
  namespace: argocd
spec:
  project: default
  source:
    repoURL: https://github.com/myorg/myapp
    targetRevision: HEAD
    path: k8s
  destination:
    server: https://kubernetes.default.svc
    namespace: default
  syncPolicy:
    automated:
      prune: true
      selfHeal: true
EOF
```

## 15.3 Jenkins with Kubernetes

Jenkins integrates with Kubernetes for dynamic build agents and deployments. Jenkins Kubernetes plugin provides: **dynamic agents** (Pods created for builds), **pod templates** (agent configurations), **containerized builds** (consistent environments). Understanding Jenkins integration enables CI/CD pipelines.

Jenkinsfile example:
```groovy
pipeline {
    agent {
        kubernetes {
            yaml '''
apiVersion: v1
kind: Pod
spec:
  containers:
  - name: docker
    image: docker:latest
  - name: kubectl
    image: bitnami/kubectl:latest
'''
        }
    }
    stages {
        stage('Build') {
            steps {
                container('docker') {
                    sh 'docker build -t myapp:${BUILD_NUMBER} .'
                }
            }
        }
        stage('Deploy') {
            steps {
                container('kubectl') {
                    sh 'kubectl apply -f k8s/'
                }
            }
        }
    }
}
```

## 15.4 Azure DevOps with Kubernetes

Azure DevOps provides Kubernetes integration through Azure Pipelines. Features: **Kubernetes tasks**, **Helm integration**, **kubectl commands**, **service connections**. Understanding Azure DevOps enables Microsoft-centric CI/CD.

Azure Pipeline example:
```yaml
trigger:
- main

pool:
  vmImage: 'ubuntu-latest'

variables:
  imageName: 'myapp'

stages:
- stage: Build
  jobs:
  - job: BuildImage
    steps:
    - task: Docker@2
      inputs:
        command: 'buildAndPush'
        repository: '$(imageName)'
        tags: '$(Build.BuildId)'

- stage: Deploy
  jobs:
  - job: DeployToK8s
    steps:
    - task: Kubernetes@1
      inputs:
        command: 'apply'
        arguments: '-f k8s/'
```

---

## 15.5 GitHub Actions with Kubernetes

GitHub Actions provides native CI/CD for Kubernetes deployments. Features: **workflow automation**, **Kubernetes actions**, **secrets management**, **matrix builds**, and **deployment strategies**.

GitHub Actions workflow example:
```yaml
name: Deploy to Kubernetes

on:
  push:
    branches: [main]

jobs:
  deploy:
    runs-on: ubuntu-latest
    steps:
    - uses: actions/checkout@v2
    
    - name: Build Docker image
      run: docker build -t myapp:${{ github.sha }} .
    
    - name: Push to registry
      run: docker push myapp:${{ github.sha }}
    
    - name: Deploy to Kubernetes
      uses: azure/k8s-deploy@v1
      with:
        manifests: |
          k8s/deployment.yaml
          k8s/service.yaml
        images: |
          myapp:${{ github.sha }}
```

---

## 15.6 CI/CD Best Practices

### Pipeline Design

Pipeline best practices: **separate stages** (build, test, deploy), **parallel execution** (faster pipelines), **caching** (faster builds), **secrets management** (secure credentials), **rollback capability** (quick recovery), and **testing** (comprehensive tests).

### Deployment Strategies

Deployment strategies include: **blue-green** (instant switchover), **canary** (gradual rollout), **rolling** (default Kubernetes), **A/B testing** (traffic splitting). Understanding strategies helps you choose appropriate approaches.

### Security in CI/CD

Security practices: **image scanning** (vulnerability detection), **secrets rotation** (regular updates), **RBAC** (least privilege), **audit logging** (track changes), **signed images** (image signing), and **policy enforcement** (automated policies).

---

## 15.7 Continuous Deployment Patterns

### GitOps Workflow

GitOps workflow: **Git as source of truth** (all configs in Git), **automated sync** (tools sync Git to cluster), **pull-based** (agents pull changes), **declarative** (desired state), **versioned** (all changes tracked), and **auditable** (change history).

### Progressive Delivery

Progressive delivery patterns: **feature flags** (gradual feature rollout), **canary deployments** (traffic splitting), **A/B testing** (experiment with users), **blue-green** (instant switchover), and **shadow traffic** (test with production traffic).

---

## Quick Reference

### CI/CD Tools

**GitOps:**
- ArgoCD
- Flux

**CI/CD Platforms:**
- Jenkins
- GitHub Actions
- Azure DevOps
- GitLab CI

**Kubernetes Tools:**
- kubectl
- Helm
- Kustomize

---

## Common Pitfalls

### Pitfall 1: Not Testing Before Deployment
**Problem**: Bugs in production
**Solution**: Comprehensive testing in pipeline
**Prevention**: Automated testing stages

### Pitfall 2: Manual Deployments
**Problem**: Inconsistent deployments, human error
**Solution**: Fully automated CI/CD pipelines
**Prevention**: Eliminate manual steps

### Pitfall 3: Not Having Rollback Plan
**Problem**: Difficult recovery from failed deployments
**Solution**: Automated rollback procedures
**Prevention**: Test rollback processes

---

## Best Practices

1. **Automate Everything**: No manual deployment steps
2. **Test Thoroughly**: Unit, integration, e2e tests
3. **Use GitOps**: Git as source of truth
4. **Implement Rollback**: Quick recovery capability
5. **Secure Pipelines**: Secrets management, RBAC
6. **Monitor Deployments**: Track deployment health
7. **Use Progressive Delivery**: Gradual rollouts
8. **Version Everything**: Tag images and configs
9. **Document Pipelines**: Clear documentation
10. **Review Regularly**: Optimize pipeline performance

---

## Further Reading

### Official Documentation
- [GitOps](https://www.gitops.tech/)
- [ArgoCD](https://argo-cd.readthedocs.io/)
- [Flux](https://fluxcd.io/)

### Related Topics
- Workloads (Module 6)
- Configuration Management (Module 12)
- Advanced Topics (Module 16)

---

*This module covers Kubernetes CI/CD integration. Understanding CI/CD integration enables automated application deployment to Kubernetes clusters.*

