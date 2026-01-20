# Terraform Interview Questions and Answers


## Basic Terraform Interview Questions

### 1. What is Terraform, and what is its primary purpose?
**Answer:**
Terraform is an open-source Infrastructure as Code (IaC) tool created by HashiCorp.
*   **Purpose:** It allows you to build, change, and version infrastructure safely and efficiently.
*   **Mechanism:** You define infrastructure in config files (HCL), and Terraform generates an execution plan to provision those resources via API calls to providers (AWS, Azure, GCP).
*   **Key Feature:** It is "Declarative" (You say what you want) and "Cloud Agnostic" (Works with many providers).

### 2. How does Terraform differ from other IaC tools like CloudFormation or Ansible?
**Answer:**
*   **Vs CloudFormation:** Terraform is open-source and multi-cloud (AWS, Azure, GCP). CloudFormation is proprietary and AWS-only.
*   **Vs Ansible:**
    *   **Terraform:** Orchestration tool. Best at provisioning *infrastructure* (VPC, VMs, LBs). It is declarative and manages state.
    *   **Ansible:** Configuration Management tool. Best at configuring *software* inside the VM (installing Nginx, patching OS). It is imperative/procedural.

### 3. What are the key Terraform commands, and what do they do?
**Answer:**
*   `terraform init`: Downloads provider plugins and initializes the backend (state storage).
*   `terraform plan`: Dry-run. Compares config vs state vs real world. Shows what *will* happen.
*   `terraform apply`: Executes the plan. Creates/Updates/Deletes resources.
*   `terraform destroy`: Destroys all objects managed by the configuration.
*   `terraform validate`: Checks syntax correctness.
*   `terraform fmt`: Rewrites config files to standard format.

### 4. What is a Terraform state file, and why is it important?
**Answer:**
*   **Definition:** `terraform.tfstate` is a JSON file that maps your HCL configuration to real-world resources (IDs).
*   **Function:**
    *   **Mapping:** Knows that `aws_instance.web` corresponds to `i-0123456789abcde`.
    *   **Metadata:** Tracks dependnecies.
    *   **Performance:** Caches attribute values to speed up `plan`.
*   **Crucial:** If you lose the state file, Terraform loses track of your infrastructure and may try to recreate (duplicate) everything.

### 5. What are Terraform providers, and why are they important?
**Answer:**
Providers are plugins that teach Terraform how to talk to a specific API.
*   **Role:** Terraform Core doesn't know about AWS or Azure. The `aws` provider translates Terraform logic into AWS API calls.
*   **Examples:** AWS, Azure, Google, Kubernetes, Datadog, GitHub.
*   **Config:** You define them in the `required_providers` block.

### 6. Explain Infrastructure as Code (IaC) and its benefits.
**Answer:**
IaC is managing hardware through software specifications.
*   **Versioning:** Infrastructure history is stored in Git. You can rollback changes.
*   **Consistency:** Eliminates configuration drift (Snowflake servers).
*   **Automation:** Speed up deployments.
*   **Documentation:** The code *is* the documentation of what is deployed.

### 7. What are Terraform modules, and how do you use them?
**Answer:**
*   **Definition:** A container for multiple resources that are used together.
*   **Analogy:** Like a "Function" or "Class" in programming.
*   **Usage:**
    *   **Root Module:** Your working directory.
    *   **Child Module:** A reusable block called by the root. e.g., A "Network Module" that creates a VPC, Subnets, and Route Tables all in one go.
    ```hcl
    module "vpc" {
      source = "./modules/vpc"
      cidr   = "10.0.0.0/16"
    }
    ```

### 8. What is the difference between declarative and imperative approaches in infrastructure provisioning?
**Answer:**
*   **Declarative (Terraform):** You define the **End State**. "I want 3 servers." Terraform figures out if it needs to create 3, or add 2 to an existing 1.
*   **Imperative (Scripting/Ansible):** You define the **Steps**. "Create server. Create server. Create server." If you run it twice, you might end up with 6 servers (unless you add complex logic).

### 9. How does Terraform handle dependencies between resources?
**Answer:**
*   **Implicit Dependency:** Terraform analyzes references. If Resource B references Resource A's ID (`instance_id = aws_instance.a.id`), Terraform knows to create A first.
*   **Explicit Dependency:** User adds `depends_on = [aws_s3_bucket.example]` meta-argument implies a dependency even if no attributes are referenced directly.

### 10. What are Terraform variables, and how can you use them?
**Answer:**
Variables parametrize the configuration.
*   **Input Variables:** Arguments passed into the module. defined in `variables.tf`.
    *   Usage: `var.region`.
*   **Local Values:** Temporary internal constants.
    *   Usage: `local.common_tags`.
*   **Output Values:** Return values from a module.
    *   Usage: `module.vpc.subnet_id`.

## Intermediate Terraform Interview Questions

### 11. How does Terraform manage remote state, and why is it important for team collaboration?
**Answer:**
*   **Local State:** Stored on the developer's laptop (`terraform.tfstate`). Bad for teams (no locking, out of sync).
*   **Remote State:** Stored in a specialized backend (S3, Azure Blob, Terraform Cloud).
    *   **Benefits:** Everyone references the same state file.
    *   **Locking:** Prevents two people from running `apply` at the same time (preventing corruption).

### 12. Explain state locking and concurrency issues in Terraform.
**Answer:**
*   **Issue:** If Alice and Bob both run `apply` simultaneously, they might both update the state file, causing race conditions and data corruption.
*   **Locking:** When Terraform starts an operation, it acquires a "Lock" on the state backend (e.g., creating a record in DynamoDB).
*   **Mechanism:** If Bob tries to run `apply` while Alice has the lock, Terraform will fail with "Error acquiring the state lock".

### 13. What are Terraform workspaces, and when should you use them?
**Answer:**
*   **Definition:** Workspaces allow multiple state files for the *same* configuration code.
*   **Use Case:** Managing identical environments (Dev, Stage, Prod) using the exact same `.tf` files.
*   **Command:** `terraform workspace new dev`.
*   **Caveat:** For complex setups, separate directories/folders for environments (Dev/Prod) are often preferred over workspaces for better isolation and clarity.

### 14. How do you handle secrets management in Terraform?
**Answer:**
**Never** store secrets in plain text `variables.tf`.
1.  **Environment Variables:** `export TF_VAR_db_password="sdkfjhsw"`.
2.  **External Stores:** Use `data` sources to fetch from AWS Secrets Manager or HashiCorp Vault during runtime.
3.  **State File Warning:** **Crucial:** Even if you pass secrets securely, Terraform stores the *result* in the `tfstate` file in plain text. Hence, the state backend *must* be encrypted and access-controlled.

### 15. What is drift detection in Terraform, and how can it be addressed?
**Answer:**
*   **Drift:** When the real infrastructure changes (manual console edit) and no longer matches the TF code.
*   **Detection:** Run `terraform plan`. It will show "No/Changes" or specific changes needed to revert the drift.
*   **Address:**
    *   **Accept:** Update code to match the manual change.
    *   **Reject:** Run `terraform apply` to overwrite the manual change and restore the original desired state.

### 16. How does Terraform handle importing existing infrastructure?
**Answer:**
`terraform import` brings unmanaged resources into the state file.
*   **Command:** `terraform import aws_instance.myvm i-12345`
*   **Constraint:** Prior to Terraform 1.5, it only updated the State. You still had to manually write the HCL code to match.
*   **Improvement:** Newer versions (`import` block) can help generate configuration.

### 17. What is the purpose of the `lifecycle` block in a resource?
**Answer:**
It overrides default lifecycle behavior.
*   `create_before_destroy`: Create the new replacement before killing the old one (Zero downtime).
*   `prevent_destroy`: Returns an error if you try to delete this resource (Safety for Databases).
*   `ignore_changes`: Don't update the resource if specific attributes change (e.g., ignore `tags` changed by an external auto-tagger).

### 18. How do you reuse code across multiple Terraform projects?
**Answer:**
Using **Modules**.
*   **Source:** Modules can be loaded from local paths, Git repositories (`git::https://...`), or the Terraform Registry.
*   **Versioning:** Pin modules to specific tags (`ref=v1.0.0`) to ensure stability across projects.

### 19. Explain the workflow of Terraform Core.
**Answer:**
1.  **Load Config:** Read `.tf` files.
2.  **Refresh:** Query the providers (API) to get current state of live resources. Updates the state file.
3.  **Compare:** Compare Config vs Live State.
4.  **Plan:** Calculate the "diff" (Add/Change/Destroy steps).
5.  **Apply:** Call APIs to execute the steps.

### 20. How would you recover from a failed `terraform apply` or a corrupted state file?
**Answer:**
*   **Failed Apply:** Usually safe. The state is only updated for resources that finished processing. Run `apply` again after fixing the error.
*   **Corrupted State:**
    *   **Versioning:** Restore the previous version of the state file from the Backend (S3 Versioning).
    *   **State commands:** Use `terraform state push` to upload a backup.
    *   **Refresh/Import:** In worst case, delete state and re-import resources.

## Advanced Terraform Interview Questions

### 21. How do you implement custom validation for input variables in Terraform?
**Answer:**
Using the `validation` block inside a variable.
```hcl
variable "instance_id" {
  type = string
  validation {
    condition     = length(var.instance_id) > 4 && substr(var.instance_id, 0, 2) == "i-"
    error_message = "The instance_id value must match regex pattern i-*."
  }
}
```
This fails fast (during plan) if the input is invalid.

### 22. How do you handle multi-cloud deployments with Terraform?
**Answer:**
Use multiple `provider` blocks.
```hcl
provider "aws" { region = "us-east-1" }
provider "azurerm" { features {} }

resource "aws_instance" "app" { ... }
resource "azurerm_virtual_machine" "db" { ... }
```
You can essentially pipe the output of one (e.g., AWS Public IP) into the input of the other (e.g., Azure Firewall Allow list).

### 23. How do you integrate Terraform into a CI/CD pipeline?
**Answer:**
*   **Pull Request:**
    *   Step 1: `terraform fmt -check`
    *   Step 2: `terraform validate`
    *   Step 3: `terraform plan` -> Output this to the PR comment for review.
*   **Merge to Main:**
    *   Step 4: `terraform apply -auto-approve`

### 24. How do you manage resource naming conflicts and ensure uniqueness?
**Answer:**
*   **Random Provider:** Use `random_id` or `random_pet` resource to generate unique suffixes.
*   **Naming Standard:** Use variables: `${var.project}-${var.env}-${var.app}-resourcename`.
*   **Tags:** Use `locals` to define common tagging strategies.

### 25. What is a null_resource, and when would you use it?
**Answer:**
A resource that does nothing on the cloud provider but has a lifecycle managed by Terraform.
*   **Use Case:** Running a local script (`local-exec`) or remote script (`remote-exec`) *after* a resource is created, but no other suitable trigger exists.
*   **Triggers:** You can use `triggers` map to force it to re-run when a value changes.

### 26. Explain the use of `count`, `for_each`, and `dynamic` blocks in Terraform.
**Answer:**
*   `count`: "Loop" based on index. Good for identical copies. *Risk:* Removing item 0 shifts all indices, causing massive recreation.
*   `for_each`: "Loop" based on map keys/set strings. Safer. Removing "key_a" only destroys "key_a".
*   `dynamic`: Used inside resource blocks (e.g., `ingress` rules). Generates nested blocks programmatically.

### 27. How do you perform a rolling update in Terraform for services without downtime?
**Answer:**
*   **Infrastructure:** Terraform is not an orchestrator like K8s. However, for a simple update (e.g., ASG Launch Template):
    *   Create new Launch Template.
    *   Terraform updates the ASG to use the new template.
    *   Wait for ASG "Instance Refresh" (if supported directly) or rely on external agent to rotate instances.
*   **Create Before Destroy:** Use lifecycle block to spin up the new replacement before killing the old one.

### 28. What are the best practices for Terraform module development?
**Answer:**
1.  **Encapsulation:** Hide complexity. Expose only necessary variables.
2.  **Defaults:** Provide sensible defaults for optional variables.
3.  **Outputs:** Output resource IDs so other modules can consume them.
4.  **Readme:** Include generated docs (using `terraform-docs`).
5.  **Pin Versions:** Constrain provider versions `required_providers { aws = ">= 3.0" }`.

### 29. How do you manage cross-account resource access and provisioning in Terraform?
**Answer:**
Use AWS IAM Roles and `assume_role` in the provider.
```hcl
provider "aws" {
  alias  = "prod"
  region = "us-west-1"
  assume_role {
    role_arn = "arn:aws:iam::123456789:role/TerraformExecution"
  }
}
```
You can then use `provider = aws.prod` in resources to deploy to that account.

### 30. Explain how Sentinel policies can be used in Terraform.
**Answer:**
Sentinel is Policy as Code (Enterprise feature).
*   **Logic:** It runs *between* Plan and Apply.
*   **Rules:** "Ensure all S3 buckets are private", "Ensure instance type is not m5.large".
*   **Enforcement:** Hard mandatory (stops apply), Soft mandatory (can override).

### 31. How do you handle large-scale refactoring of resources without downtime?
**Answer:**
Use `terraform state mv` to move resources.
*   **Scenario:** You have a resource `aws_instance.main` and you want to move it into a module `module.app.aws_instance.main`.
*   **Action:** If you just change the code, TF will plan "Destroy Old, Create New".
*   **Fix:** Run `terraform state mv aws_instance.main module.app.aws_instance.main`. Terraform now knows they are the same object. No recreation occurs.

### 32. When should you use a data block versus an input variable to get information into your Terraform configuration?
**Answer:**
*   **Input Variable:** When the user decides the value (e.g., "I want 3 servers").
*   **Data Block:** When the cloud decides the value, or it already exists (e.g., "Find the ID of the latest Ubuntu AMI", "Get the VPC ID named 'default'").

### 33. How can you ensure idempotency in Terraform deployments?
**Answer:**
Terraform is designed to be idempotent.
*   **Ensure:** Do not use `null_resource` or `local-exec` provisioners which run shell scripts blindly unless necessary. Scripts are often not idempotent.
*   **Best Practice:** Rely on native resources and providers.

### 34. What is the difference between a root module and a child module?
**Answer:**
*   **Root Module:** The directory where you run `terraform apply`. It holds the provider config and state backend.
*   **Child Module:** Any module called *by* the root module. It typically should not define providers or backends.

### 35. How do you test Terraform configurations?
**Answer:**
1.  **Static Analysis:** `tflint`, `terraform validate`.
2.  **Unit/Integration Tests:** `Terratest` (Go library). It spins up the infrastructure, runs a test (e.g., cURL the HTTP endpoint), validates the response, and then destroys the infra.

### 36. How do you downgrade or upgrade Terraform versions safely?
**Answer:**
*   **Upgrade:** Run `terraform init -upgrade`. Then `plan/apply`. Terraform will update the state file format.
*   **Downgrade:** **Very difficult.** Once a state file is upgraded to a newer version (e.g., 1.5 -> 1.6), older versions cannot read it. You rely on S3 bucket versioning to revert the state file to the old format.

### 37. What is the purpose of `terraform taint` and `terraform untaint`?
**Answer:**
*   **Taint:** explicitly tells Terraform "This resource is degraded/bad, destroy and recreate it next time."
*   **Untaint:** "Just kidding, keep it."
*   **Note:** Deprecated in favor of `terraform apply -replace="aws_instance.foo"`.

### 38. How can you use Terraform to manage non-cloud infrastructure resources?
**Answer:**
As long as a **Provider** exists, Terraform can manage it.
*   **Examples:** Active Directory, VMware vSphere, Cisco ACI, Local files (`local_file`), Random strings.
*   **Custom:** You can write your own Provider in Go if an API exists.

### 39. Describe how you can use Terraform with infrastructure deployment tools like Ansible or Chef.
**Answer:**
*   **Pattern:** Terraform builds the house (Updates Infrastructure), Ansible furnishes it (Configures Software).
*   **Integration:**
    *   Terraform `local-exec` triggers Ansible playbook.
    *   Better: Terraform creates "Inventory file" for Ansible to use later.
    *   Better: Terraform passes `user_data` script that installs Ansible-pull on the instance.

### 40. How do you handle module versioning in Terraform?
**Answer:**
Always specify the `version` argument.
```hcl
module "vpc" {
  source  = "terraform-aws-modules/vpc/aws"
  version = "3.14.0"
}
```
This protects your production infra from breaking changes released in version 4.0.0 of the module.

### 41. What is the `terraform graph` command used for?
**Answer:**
Output dependency graph in DOT format.
*   **Usage:** `terraform graph | dot -Tsvg > graph.svg`.
*   **Visual:** Shows dependencies bubbles. Useful for debugging circular dependencies.

### 42. What are the common strategies for organizing Terraform code for large projects?
**Answer:**
**Terragrunt Pattern / Directory Separation:**
`env/dev/vpc`, `env/dev/app`, `env/prod/vpc`.
*   Each folder has its own state file.
*   Low blast radius. Breaking Dev VPC doesn't break Prod App.

### 43. How do you optimize Terraform performance for large configurations?
**Answer:**
1.  **Split State:** Don't possess a "Monolith" state file. Use many small states.
2.  **Target:** `apply -target` (Use sparingly).
3.  **Parallelism:** `apply -parallelism=30` (Default is 10). Increases concurrent API calls.

### 44. What is the benefit of `terraform console`?
**Answer:**
Interactive REPL.
*   **Use:** Test syntax logic.
*   `> cidrsubnet("10.0.0.0/16", 8, 1)` -> Returns `10.0.1.0/24`.
*   Great for debugging complicated variable interpolations without running a plan.

### 45. How do you manage custom providers or extend existing Terraform providers?
**Answer:**
*   **Write:** Use the **Terraform Plugin SDK** (Go).
*   **Publish:** Publish to Terraform Registry or host locally (`~/.terraform.d/plugins`).

### 46. What is the difference between `terraform apply -auto-approve` and regular `terraform apply`?
**Answer:**
*   `regular`: Shows plan, creates an interactive prompt "Enter a value: yes".
*   `auto-approve`: Non-interactive. Executes immediately. Necessary for CI/CD pipelines.

### 47. How do you ensure proper cleanup of resources after `terraform destroy`?
**Answer:**
*   **Dependencies:** Ensure resources created *manually* inside a TF-managed VPC are deleted first (e.g., Lambda creates ENIs left behind).
*   **Provisioners:** `when = destroy` provisioner can run cleanup scripts (e.g., deregister from Active Directory).

### 48. When would you use `terraform refresh`?
**Answer:**
*   **Role:** Updates state file with real-world data.
*   **Usage:** Usually not needed manually (Plan does it automatically).
*   **Scenario:** If you know someone deleted a resource and you want the State to reflect that immediately without running a full plan.

### 49. Explain the concept of `backend` in Terraform and its significance.
**Answer:**
Backend determines exactly **where** and **how** state is stored.
*   **Local:** Disk.
*   **Remote:** S3, Consul, GCS, HTTP.
*   **Significance:** Enables Lock, Encryption, and Collaboration. Changing backend requires `terraform init -migrate-state`.

### 50. What is the role of `output` values in Terraform?
**Answer:**
*   **CLI:** Prints value at end of apply (`LoadBalancer IP: 1.2.3.4`).
*   **Modules:** Returns value to parent module.
*   **Remote State Data Source:** Allows *other* Terraform projects to read these outputs. (Project B reads VPC ID from Project A's output).

## Advanced Terraform & State Management

### 51. Risks of local state and mitigation?
**Answer:**
*   **Risks:**
    *   **Single Point of Failure:** Laptop crashes, state is gone. Infra is orphaned.
    *   **Security:** Secrets in plain text on laptop.
    *   **Team:** "It works on my machine".
*   **Mitigation:** S3 Backend + DynamoDB (Locking) + KMS (Encryption) + Versioning.

### 52. How do you handle sensitive data in state files?
**Answer:**
The state file maps input config to real world, so it *must* store attribute references.
If you create a password, the password *is* in the state file.
*   **Solution:** Encrypt the State File at rest (S3 SSE-KMS). Strictly control who has Read access to the S3 bucket.

### 53. Recovering from a lost/corrupted state file?
**Answer:**
1.  **Backup:** Restore from S3 Versioning history.
2.  **Reconstruction:** If no backup, use `terraform import` to verify existing resources and rebuild the state file one by one. This is tedious but necessary to regain control.

### 54. What is `terraform state` command used for?
**Answer:**
It performs "surgery" on the state file.
*   `rm`: "Forget this resource (don't delete from cloud)".
*   `mv`: "Rename this resource".
*   `push/pull`: Manually upload/download state.

### 55. What is Infrastructure Drift and how to detect it?
**Answer:**
*   **Drift:** Deviation between Code and Reality.
*   **Detection:** `terraform plan`. It says "Infrastructure is up-to-date" (No Drift) or "1 to change" (Drift detected).
*   **External tools:** `Driftctl` provides comprehensive drift reports covering unmanaged resources too.

### 56. Difference between `count` and `for_each`?
**Answer:**
*   **Count:** List based. `count = 3` produces `instance[0], instance[1], instance[2]`. If you remove `1`, `2` becomes `1`. This forces resource `2` to be destroyed and recreated as `1`. Bad for stateful resources.
*   **For_each:** Map based. Uses stable keys (`instance["web"]`, `instance["db"]`). Removing `web` only touches `web`.

### 57. How do you handle multi-cloud deployments?
**Answer:**
Configuration is unified, but providers are distinct.
You need credentials for both clouds.
```hcl
resource "aws_route53_zone" "primary" { ... }
resource "google_dns_managed_zone" "secondary" { ... }
```
Terraform manages the dependencies (Create AWS Zone -> Get NS records -> Update Google Config).

### 58. Immutable Infrastructure with Terraform?
**Answer:**
Terraform favors replacement over in-place update for many attributes.
*   **Example:** Changing `user_data` usually forces a new instance.
*   **Benefit:** No "configuration drift" over years of patches. Every deployment is a fresh, known state.

### 59. What is Terragrunt?
**Answer:**
A "Thin Wrapper" around Terraform.
*   **Solves:** DRY (Don't Repeat Yourself) backend config.
*   **Solves:** Managing dependencies between multiple Terraform folders (Run Stack: VPC -> then App).
*   **Solves:** Passing arguments to all commands based on directory structure.

### 60. Troubleshooting `terraform apply` failure for missing resource?
**Answer:**
**Error:** "Resource not found".
**Scenario:** Someone manually deleted the EC2 instance in the console, but the State file still thinks it exists. Terraform tries to modify it and fails.
**Fix:** Run `terraform refresh`. Terraform will query AWS, see it's gone, remove it from the state file. Then run `apply` (Terraform will create a new one).
