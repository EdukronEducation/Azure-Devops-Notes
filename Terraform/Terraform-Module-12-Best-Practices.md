# Module 12: Terraform Best Practices

## Table of Contents
- [12.1 Code Organization](#121-code-organization)
- [12.2 Security Best Practices](#122-security-best-practices)
- [12.3 Performance Optimization](#123-performance-optimization)
- [12.4 Testing and Validation](#124-testing-and-validation)
- [12.5 Version Control Integration](#125-version-control-integration)
- [Quick Reference](#quick-reference)
- [Common Pitfalls](#common-pitfalls)
- [Best Practices Summary](#best-practices-summary)
- [Further Reading](#further-reading)

---

## 12.1 Code Organization

### Why Code Organization Matters

Code organization is fundamental to maintaining Terraform projects as they grow in size and complexity. Well-organized code is easier to understand, modify, and troubleshoot. Poor organization leads to confusion, merge conflicts, and difficulty finding the right files when making changes. As infrastructure grows from a few resources to hundreds or thousands, organization becomes critical for team productivity and code quality.

Good organization enables: **team collaboration** (multiple developers can work without conflicts), **code reuse** (modules can be shared and reused), **maintainability** (easy to find and modify code), **scalability** (structure supports growth), and **onboarding** (new team members can understand the codebase quickly). Understanding code organization principles helps you build maintainable Terraform projects.

### File Structure Best Practices

Terraform projects should follow consistent file structure patterns:

**Standard Files**: Every Terraform project should have `main.tf` (primary resources), `variables.tf` (input variables), `outputs.tf` (output values), `versions.tf` (provider requirements), and optionally `terraform.tfvars` (variable values). This structure separates concerns and makes files easy to locate.

**Module Structure**: Modules should follow the same structure as root modules. Each module directory should be self-contained with its own `main.tf`, `variables.tf`, and `outputs.tf`. Modules should be organized by function (network, compute, storage) or by service (vpc, ec2, rds).

**Environment Structure**: Environments (dev, staging, prod) can be organized as separate directories, each with their own Terraform configuration. Alternatively, use workspaces with shared configuration and environment-specific variable files. Choose based on team preferences and requirements.

#### Recommended Project Structure

```
terraform-project/
├── modules/
│   ├── network/
│   │   ├── main.tf
│   │   ├── variables.tf
│   │   ├── outputs.tf
│   │   ├── versions.tf
│   │   └── README.md
│   ├── compute/
│   │   ├── main.tf
│   │   ├── variables.tf
│   │   ├── outputs.tf
│   │   └── README.md
│   └── database/
│       ├── main.tf
│       ├── variables.tf
│       ├── outputs.tf
│       └── README.md
├── environments/
│   ├── dev/
│   │   ├── main.tf
│   │   ├── variables.tf
│   │   ├── outputs.tf
│   │   ├── terraform.tfvars
│   │   └── backend.tf
│   ├── staging/
│   │   ├── main.tf
│   │   ├── variables.tf
│   │   ├── outputs.tf
│   │   ├── terraform.tfvars
│   │   └── backend.tf
│   └── prod/
│       ├── main.tf
│       ├── variables.tf
│       ├── outputs.tf
│       ├── terraform.tfvars
│       └── backend.tf
├── scripts/
│   ├── validate.sh
│   ├── format.sh
│   └── deploy.sh
├── .gitignore
├── .terraform.lock.hcl
└── README.md
```

### Naming Conventions

Consistent naming conventions make code more readable and maintainable:

**Resource Naming**: Use descriptive, consistent names. Format: `{resource_type}_{purpose}_{environment}` or `{resource_type}_{component}_{identifier}`. Examples: `aws_instance_web_prod`, `aws_s3_bucket_logs`, `aws_vpc_main`. Avoid abbreviations unless widely understood.

**Variable Naming**: Use snake_case for variables. Be descriptive: `instance_type` not `type`, `database_password` not `db_pwd`. Group related variables with prefixes: `vpc_cidr`, `vpc_name`, `vpc_tags`.

**Module Naming**: Use kebab-case for module directories: `network-module`, `compute-cluster`, `database-postgres`. Module names should indicate purpose clearly.

**File Naming**: Use standard Terraform file names: `main.tf`, `variables.tf`, `outputs.tf`. For additional files, use descriptive names: `data.tf`, `locals.tf`, `providers.tf`.

### Code Organization Patterns

Common organization patterns:

**Monolithic Pattern**: All resources in one directory. Simple for small projects, becomes unwieldy as it grows. Suitable for: small projects, single-environment deployments, learning.

**Module Pattern**: Resources organized into reusable modules. Promotes code reuse and separation of concerns. Suitable for: medium to large projects, multi-environment deployments, team collaboration.

**Environment Pattern**: Separate directories per environment. Each environment has its own configuration. Suitable for: strict environment separation, different configurations per environment.

**Hybrid Pattern**: Combination of modules and environments. Modules for reusable components, environments for deployment. Suitable for: large projects, complex infrastructure, enterprise deployments.

### Documentation Standards

Documentation is crucial for maintainability:

**README Files**: Every module and project should have a README explaining: purpose, usage, inputs, outputs, examples, requirements. READMEs help others understand and use your code.

**Code Comments**: Comment complex logic, non-obvious decisions, and workarounds. Use comments to explain "why" not "what" (code should be self-documenting). Comments should add value, not repeat what code says.

**Variable Descriptions**: Every variable should have a description explaining its purpose, acceptable values, and defaults. Descriptions appear in `terraform console` and documentation.

**Output Descriptions**: Every output should have a description explaining what it provides and how to use it. Outputs are the interface to your infrastructure.

Documentation example:
```hcl
# variables.tf
variable "instance_type" {
  description = <<-EOT
    EC2 instance type for web servers.
    
    For production, use t3.medium or larger.
    For development, t3.micro is sufficient.
    
    Valid values: t3.micro, t3.small, t3.medium, t3.large
  EOT
  type        = string
  default     = "t3.micro"
  
  validation {
    condition = can(regex("^t3\\.[a-z]+$", var.instance_type))
    error_message = "Instance type must be t3 family."
  }
}
```

### Code Organization Best Practices

Best practices for code organization:

1. **Separate Concerns**: Group related resources together
2. **Use Modules**: Create modules for reusable patterns
3. **Consistent Structure**: Follow the same structure across projects
4. **Clear Naming**: Use descriptive, consistent names
5. **Document Everything**: READMEs, comments, descriptions
6. **Version Control**: Store all code in Git
7. **Review Regularly**: Refactor as projects grow
8. **Follow Conventions**: Use community-standard patterns
9. **Keep It Simple**: Don't over-engineer organization
10. **Iterate**: Improve organization as you learn

---

## 12.2 Security Best Practices

### Security Fundamentals

Security in Terraform involves protecting sensitive data, controlling access, and preventing unauthorized changes. Infrastructure code often contains or manages sensitive information like passwords, API keys, and network configurations. Security breaches can lead to data exposure, unauthorized access, or infrastructure compromise.

Terraform security encompasses: **secret management** (protecting sensitive data), **access control** (who can make changes), **state security** (protecting state files), **provider credentials** (securing cloud access), **code security** (scanning for vulnerabilities), and **compliance** (meeting regulatory requirements). Understanding security best practices helps you protect infrastructure and data.

### Secret Management

Never commit secrets to version control:

**Problem**: Secrets in code are exposed in Git history, accessible to anyone with repository access, and difficult to rotate. Once committed, secrets are in Git history forever, even if removed later.

**Solutions**: 
- Use secret management tools (HashiCorp Vault, AWS Secrets Manager, Azure Key Vault)
- Use environment variables for secrets
- Use Terraform Cloud variables (marked as sensitive)
- Use external data sources to fetch secrets
- Never hard-code secrets in `.tf` files

Secret management example:
```hcl
# ❌ BAD - Never do this
variable "db_password" {
  default = "MySecretPassword123!"
}

# ✅ GOOD - Use external secret management
data "aws_secretsmanager_secret_version" "db_password" {
  secret_id = "production/database/password"
}

resource "aws_db_instance" "main" {
  password = data.aws_secretsmanager_secret_version.db_password.secret_string
}

# ✅ GOOD - Use environment variables
# Set TF_VAR_db_password in CI/CD or locally
variable "db_password" {
  type        = string
  sensitive   = true
  description = "Database password from environment variable"
}
```

### State File Security

State files contain sensitive information and must be protected:

**State File Contents**: State files contain resource IDs, IP addresses, and sometimes secrets (if stored in resources). State files are JSON and readable by anyone with access.

**Security Measures**:
- **Encryption**: Encrypt state files at rest (S3 encryption, Azure Storage encryption)
- **Access Control**: Restrict access to state storage (IAM policies, RBAC)
- **Remote State**: Use remote state backends, never commit state to Git
- **State Locking**: Enable state locking to prevent concurrent modifications
- **Backup**: Regularly backup state files securely
- **Audit Logging**: Log all state access for compliance

State security example:
```hcl
# Secure backend configuration
terraform {
  backend "s3" {
    bucket         = "terraform-state-bucket"
    key            = "prod/terraform.tfstate"
    region         = "us-west-2"
    encrypt        = true  # Enable encryption
    dynamodb_table = "terraform-locks"  # Enable locking
    kms_key_id     = "arn:aws:kms:..."  # Use KMS encryption
  }
}
```

### Provider Credentials

Secure provider authentication:

**Best Practices**:
- Use IAM roles/service principals instead of access keys when possible
- Rotate credentials regularly
- Use separate credentials per environment
- Grant minimum necessary permissions (least privilege)
- Never commit credentials to version control
- Use credential management tools

**AWS Example**:
```bash
# ❌ BAD - Hard-coded credentials
provider "aws" {
  access_key = "AKIAIOSFODNN7EXAMPLE"
  secret_key = "wJalrXUtnFEMI/K7MDENG/bPxRfiCYEXAMPLEKEY"
}

# ✅ GOOD - Environment variables
export AWS_ACCESS_KEY_ID="..."
export AWS_SECRET_ACCESS_KEY="..."

# ✅ GOOD - IAM role (EC2 instance)
# No credentials needed, uses instance role

# ✅ GOOD - AWS profile
provider "aws" {
  profile = "production"
  region  = "us-west-2"
}
```

### Code Security Scanning

Scan Terraform code for security issues:

**Scanning Tools**:
- **tfsec**: Fast security scanner for Terraform
- **checkov**: Policy-as-code security scanner
- **Terrascan**: Security and compliance scanner
- **TFLint**: Linter with security rules

**Integration**: Run scanners in CI/CD pipelines, fail builds on critical issues, review and fix findings, keep scanners updated.

Security scanning example:
```bash
# Install tfsec
brew install tfsec  # macOS
# or
go install github.com/aquasecurity/tfsec/cmd/tfsec@latest

# Scan code
tfsec .

# Scan with specific rules
tfsec . --exclude AWS002,AWS003

# Generate report
tfsec . --format json --out results.json
```

### Least Privilege Principles

Apply least privilege to all access:

**IAM Policies**: Grant minimum necessary permissions. Use separate policies for different operations (read-only for plan, write for apply). Regularly review and audit permissions.

**Service Accounts**: Use dedicated service accounts for Terraform. Don't use personal accounts. Rotate service account credentials regularly.

**Network Security**: Restrict network access to state storage. Use VPC endpoints for cloud storage. Use private networks when possible.

### Security Best Practices Summary

1. **Never commit secrets** - Use secret management tools
2. **Encrypt state files** - At rest and in transit
3. **Use least privilege** - Minimum necessary permissions
4. **Rotate credentials** - Regularly change passwords and keys
5. **Scan code** - Use security scanners in CI/CD
6. **Restrict access** - Control who can modify infrastructure
7. **Audit changes** - Log all infrastructure modifications
8. **Use remote state** - Never commit state to Git
9. **Enable locking** - Prevent concurrent modifications
10. **Review regularly** - Audit security practices periodically

---

## 12.3 Performance Optimization

### Performance Considerations

Terraform performance affects development velocity and deployment speed. Slow Terraform operations delay deployments, reduce productivity, and increase costs (especially with cloud provider API rate limits). Performance optimization is important for large infrastructures and frequent deployments.

Performance factors include: **provider API calls** (number and speed of API requests), **resource dependencies** (parallel vs. sequential creation), **state file size** (large state files slow operations), **network latency** (remote state and provider APIs), **resource count** (more resources take longer), and **provider efficiency** (some providers are faster than others). Understanding performance factors helps you optimize Terraform operations.

### Targeted Operations

Use `-target` to limit operations to specific resources:

**When to Use**: During development, for quick iterations, when fixing specific resources, or when you know only certain resources changed. Targeted operations skip unrelated resources, significantly reducing execution time.

**Limitations**: Targeted operations can miss dependencies. Use carefully and understand resource relationships. Don't use in production without understanding implications.

Targeted operation example:
```bash
# Apply only specific resource
terraform apply -target=aws_instance.web

# Apply multiple specific resources
terraform apply \
  -target=aws_instance.web \
  -target=aws_security_group.web

# Plan with targets
terraform plan -target=module.network
```

### Parallelization

Terraform creates resources in parallel when possible:

**Automatic Parallelization**: Terraform automatically parallelizes independent resources. Resources with no dependencies are created simultaneously, maximizing throughput.

**Optimizing Parallelization**: Minimize unnecessary dependencies. Use `depends_on` only when truly needed. Structure resources to maximize independence. Review dependency graphs to identify optimization opportunities.

**Limitations**: Provider rate limits may require throttling. Some resources must be created sequentially. Network bandwidth may limit parallel operations.

### State File Optimization

Large state files slow Terraform operations:

**State File Size**: State files grow with number of resources. Large state files (100MB+) can significantly slow operations. Monitor state file size and optimize when needed.

**Optimization Strategies**:
- Remove unused resources from state
- Use `terraform state rm` for resources no longer managed
- Split large configurations into smaller modules
- Use separate state files per environment
- Archive old state files

**State File Maintenance**:
```bash
# List all resources in state
terraform state list

# Remove unused resource
terraform state rm aws_instance.old_instance

# Move resource (refactoring)
terraform state mv aws_instance.web aws_instance.webserver

# Show state file size
ls -lh terraform.tfstate
```

### Provider Optimization

Optimize provider usage:

**Provider Caching**: Cache providers locally to avoid repeated downloads. Terraform caches providers in `.terraform/providers/`. Share cache across projects when possible.

**Provider Versions**: Use specific provider versions. Latest versions may have performance improvements. Test version upgrades before production.

**API Rate Limits**: Be aware of provider API rate limits. Some providers throttle requests. Use retry logic and exponential backoff. Consider provider-specific optimizations.

### Performance Best Practices

1. **Use Targeted Operations**: When appropriate for development
2. **Minimize Dependencies**: Only use `depends_on` when needed
3. **Optimize State Files**: Keep state files small
4. **Cache Providers**: Reuse provider cache
5. **Parallel Operations**: Structure for maximum parallelization
6. **Monitor Performance**: Track operation times
7. **Use Appropriate Resources**: Choose efficient resource types
8. **Optimize Modules**: Keep modules focused and efficient
9. **Review Regularly**: Identify performance bottlenecks
10. **Test Changes**: Measure performance impact of changes

---

## 12.4 Testing and Validation

### Testing Strategies

Testing Terraform code ensures correctness and prevents errors:

**Validation Testing**: Syntax validation, type checking, and configuration validation. Catches errors before deployment. Fast and should run on every change.

**Planning Testing**: Dry-run testing with `terraform plan`. Shows what would change without making changes. Essential for understanding impact before deployment.

**Integration Testing**: Testing with actual cloud providers. Creates real resources, tests functionality, then destroys. More comprehensive but slower and costs money.

**Policy Testing**: Validating against organizational policies. Ensures compliance with standards. Can use Sentinel, OPA, or custom tools.

### Terraform Validate

`terraform validate` checks configuration syntax:

**What It Validates**: Syntax errors, type mismatches, missing required arguments, invalid references, provider configuration. Fast validation that catches common errors.

**Usage**: Run `terraform validate` before every plan. Integrate into CI/CD pipelines. Fail builds on validation errors.

Validation example:
```bash
# Basic validation
terraform validate

# Validate with variables
terraform validate -var="instance_type=t3.micro"

# Validate specific directory
terraform validate terraform/modules/network
```

### Terraform Format

`terraform fmt` ensures consistent formatting:

**Purpose**: Standardizes code formatting across team. Makes code more readable. Reduces merge conflicts from formatting differences.

**Usage**: Run `terraform fmt` before committing. Use `terraform fmt -check` in CI/CD to enforce formatting. Consider pre-commit hooks.

Formatting example:
```bash
# Format all .tf files
terraform fmt -recursive

# Check formatting (CI/CD)
terraform fmt -check -recursive

# Format specific files
terraform fmt main.tf variables.tf
```

### Testing Frameworks

Use testing frameworks for comprehensive testing:

**Terratest**: Go-based testing framework. Can test Terraform modules end-to-end. Creates real infrastructure, tests it, then destroys. Suitable for module testing.

**kitchen-terraform**: Test Kitchen plugin for Terraform. Uses InSpec for validation. Integrates with CI/CD. Good for infrastructure testing.

**Terraform Testing**: Built-in testing (Terraform 1.6+). Uses HCL for tests. Can test modules and configurations. Native Terraform solution.

Terratest example:
```go
package test

import (
    "testing"
    "github.com/gruntwork-io/terratest/modules/terraform"
    "github.com/stretchr/testify/assert"
)

func TestTerraformModule(t *testing.T) {
    terraformOptions := &terraform.Options{
        TerraformDir: "../modules/network",
    }
    
    defer terraform.Destroy(t, terraformOptions)
    terraform.InitAndApply(t, terraformOptions)
    
    vpcId := terraform.Output(t, terraformOptions, "vpc_id")
    assert.NotEmpty(t, vpcId)
}
```

### Pre-Commit Hooks

Automate validation before commits:

**Benefits**: Catches errors before they're committed. Enforces standards automatically. Saves time in code review.

**Common Hooks**: Format check, validate, security scan, documentation check.

Pre-commit hook example:
```bash
#!/bin/bash
# .git/hooks/pre-commit

terraform fmt -check -recursive
if [ $? -ne 0 ]; then
    echo "Terraform files are not formatted. Run 'terraform fmt -recursive'"
    exit 1
fi

terraform validate
if [ $? -ne 0 ]; then
    echo "Terraform validation failed"
    exit 1
fi

tfsec .
if [ $? -ne 0 ]; then
    echo "Security scan found issues"
    exit 1
fi
```

### Testing Best Practices

1. **Validate Early**: Run validation on every change
2. **Format Consistently**: Use terraform fmt
3. **Test Plans**: Always review plan output
4. **Use Frameworks**: For comprehensive testing
5. **Automate Testing**: Integrate into CI/CD
6. **Test Modules**: Test reusable modules thoroughly
7. **Test Policies**: Validate against organizational policies
8. **Review Regularly**: Update tests as code evolves
9. **Document Tests**: Explain what tests verify
10. **Fail Fast**: Catch errors as early as possible

---

## 12.5 Version Control Integration

### Git Workflows

Version control is essential for Terraform:

**Benefits**: Track changes, collaborate with team, review code, rollback changes, audit history, branch and experiment safely. Git enables infrastructure as code practices.

**Best Practices**: Commit frequently with meaningful messages, use branches for features, review all changes via pull requests, tag releases, maintain changelogs, and never commit secrets or state files.

### Branching Strategies

Choose appropriate branching strategy:

**GitFlow**: Feature branches, develop branch, release branches, main branch. Good for: large teams, complex release cycles, multiple environments.

**Trunk-Based**: Short-lived feature branches, frequent merges to main. Good for: small teams, fast iteration, continuous deployment.

**Environment Branches**: Separate branches per environment (dev, staging, prod). Good for: different configurations per environment, strict environment separation.

Branching example:
```
main (production)
├── develop (integration)
│   ├── feature/new-vpc
│   └── feature/security-groups
├── release/v1.2.0
└── hotfix/critical-fix
```

### Pull Request Practices

Use pull requests for all changes:

**Benefits**: Code review, discussion, testing before merge, documentation of changes, approval process. Pull requests provide quality gate before changes reach main branch.

**Best Practices**: Require reviews (at least one approver), run validation in PRs, show plan output in PR comments, require approval for production, document changes clearly, keep PRs focused (one concern per PR).

### Commit Message Standards

Write meaningful commit messages:

**Format**: Use conventional commits format: `type(scope): description`. Types: feat, fix, docs, refactor, test, chore. Be descriptive and explain why, not just what.

**Examples**:
```
feat(network): add VPC module with public/private subnets
fix(compute): correct instance type validation
docs(readme): update installation instructions
refactor(modules): reorganize module structure
```

### Version Tagging

Tag releases for version control:

**Purpose**: Mark stable releases, enable rollback to known good state, track versions, document what's deployed. Tags provide reference points in history.

**Tagging Strategy**: Use semantic versioning (v1.2.3), tag after successful deployment, tag major releases, document tag contents.

Tagging example:
```bash
# Tag current state
git tag -a v1.2.0 -m "Release version 1.2.0"

# Push tags
git push origin v1.2.0

# List tags
git tag -l

# Checkout specific tag
git checkout v1.2.0
```

### .gitignore Configuration

Properly configure .gitignore:

**What to Ignore**: State files (`*.tfstate`, `*.tfstate.*`), lock files (`.terraform.lock.hcl`), crash logs (`crash.log`), override files (`override.tf`, `override.tf.json`), CLI config (`.terraformrc`), variable files with secrets (`*.tfvars` with secrets).

.gitignore example:
```
# Local .terraform directories
**/.terraform/*

# .tfstate files
*.tfstate
*.tfstate.*

# Crash log files
crash.log
crash.*.log

# Exclude all .tfvars files (may contain secrets)
*.tfvars
*.tfvars.json

# Ignore override files
override.tf
override.tf.json
*_override.tf
*_override.tf.json

# Ignore CLI configuration files
.terraformrc
terraform.rc
```

### Version Control Best Practices

1. **Commit Frequently**: Small, focused commits
2. **Meaningful Messages**: Clear, descriptive commit messages
3. **Use Branches**: Feature branches for changes
4. **Pull Requests**: Review all changes
5. **Tag Releases**: Mark stable versions
6. **Never Commit Secrets**: Use .gitignore
7. **Never Commit State**: State files should be remote
8. **Document Changes**: Changelogs and release notes
9. **Review History**: Regularly review commit history
10. **Backup Repository**: Ensure repository is backed up

---

## Quick Reference

### Code Organization
- **Structure**: modules/, environments/, standard files
- **Naming**: snake_case for resources, kebab-case for modules
- **Documentation**: READMEs, comments, descriptions

### Security
- **Secrets**: Never commit, use secret management
- **State**: Encrypt, restrict access, use remote
- **Credentials**: IAM roles, rotate regularly
- **Scanning**: tfsec, checkov, Terrascan

### Performance
- **Targeting**: Use -target for focused operations
- **Parallelization**: Minimize dependencies
- **State**: Keep state files small
- **Caching**: Cache providers

### Testing
- **Validate**: terraform validate
- **Format**: terraform fmt
- **Plan**: Review plan output
- **Frameworks**: Terratest, kitchen-terraform

### Version Control
- **Commits**: Meaningful messages
- **Branches**: Feature branches
- **PRs**: Review all changes
- **Tags**: Version releases

---

## Common Pitfalls

### Pitfall 1: Committing Secrets
**Problem**: Secrets committed to Git, exposed in history
**Solution**: Use secret management, never commit secrets
**Prevention**: .gitignore, pre-commit hooks, code review

### Pitfall 2: Large State Files
**Problem**: Huge state files slow operations
**Solution**: Split configurations, remove unused resources
**Prevention**: Monitor state size, organize by modules

### Pitfall 3: Poor Organization
**Problem**: Hard to find code, merge conflicts
**Solution**: Follow consistent structure, use modules
**Prevention**: Establish standards, review regularly

### Pitfall 4: No Testing
**Problem**: Errors discovered in production
**Solution**: Implement testing strategy, use CI/CD
**Prevention**: Automate testing, require tests

### Pitfall 5: Inconsistent Naming
**Problem**: Confusion, hard to search
**Solution**: Establish naming conventions, document
**Prevention**: Code review, linting

---

## Best Practices Summary

### Code Organization
1. Use consistent file structure
2. Organize into modules
3. Follow naming conventions
4. Document thoroughly
5. Keep it simple

### Security
1. Never commit secrets
2. Encrypt state files
3. Use least privilege
4. Scan for vulnerabilities
5. Rotate credentials

### Performance
1. Use targeted operations
2. Minimize dependencies
3. Optimize state files
4. Cache providers
5. Monitor performance

### Testing
1. Validate early and often
2. Format consistently
3. Review plans
4. Use testing frameworks
5. Automate in CI/CD

### Version Control
1. Commit frequently
2. Use meaningful messages
3. Review via PRs
4. Tag releases
5. Never commit secrets/state

---

## Further Reading

### Official Documentation
- [Terraform Best Practices](https://www.terraform.io/docs/cloud/guides/recommended-practices/)
- [Terraform Style Guide](https://www.terraform.io/docs/language/syntax/style.html)

### Security Resources
- [Terraform Security Best Practices](https://www.terraform.io/docs/cloud/guides/recommended-practices/part2.html)
- [tfsec Documentation](https://aquasecurity.github.io/tfsec/)

### Testing Resources
- [Terratest Documentation](https://terratest.gruntwork.io/)
- [Terraform Testing](https://www.terraform.io/docs/language/modules/testing.html)

### Related Topics
- Modules (Module 6)
- State Management (Module 5)
- CI/CD Integration (Module 13)

---

*This module covers Terraform best practices in detail. Following these practices ensures secure, maintainable, performant, and well-tested infrastructure code that scales with your organization and supports team collaboration effectively.*

