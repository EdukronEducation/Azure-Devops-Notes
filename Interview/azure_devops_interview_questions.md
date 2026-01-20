# Azure DevOps Interview Questions and Answers


## Foundational Concepts and Overview

### 1. What is DevOps?
**Answer:**
DevOps is not just a tool or a team; it is a cultural and technical philosophy that unites development (Dev) and operations (Ops) teams. 
*   **Goal:** To shorten the systems development life cycle while delivering features, fixes, and updates frequently in close alignment with business objectives.
*   **Key Aspects:** It emphasizes communication, collaboration, integration, and automation.
*   **CAMS Model:** Often described using the CAMS model: **C**ulture, **A**utomation, **M**easurement, and **S**haring.
*   **Outcome:** Breaking down silos to allow for continuous delivery of high-quality software.

### 2. Why do we use DevOps?
**Answer:**
Organizations adopt DevOps to gain a competitive advantage through speed and stability.
*   **Speed:** Faster time to market for new features.
*   **Stability:** Lower failure rate of new releases and faster recovery time (MTTR) if failures occur.
*   **Collaboration:** improved communication reduces friction between teams.
*   **Efficiency:** Automating repetitive tasks allows engineers to focus on high-value work.
*   **Customer Satisfaction:** Continuous feedback loops ensure the product meets user needs.

### 3. What are the key principles of DevOps?
**Answer:**
The core principles that drive DevOps success include:
1.  **Continuous Integration (CI):** Merging code changes frequently to detect issues early.
2.  **Continuous Delivery (CD):** ensuring code is always in a deployable state.
3.  **Infrastructure as Code (IaC):** Managing infrastructure through configuration files.
4.  **Monitoring and Logging:** Gaining real-time visibility into application and system performance.
5.  **Microservices:** Building applications as a suite of small services.
6.  **Communication and Collaboration:** Breaking down silos between teams.

### 4. What is Azure DevOps?
**Answer:**
Azure DevOps is a comprehensive Software as a Service (SaaS) platform from Microsoft that provides an end-to-end toolchain for developing and deploying software. It is cloud-agnostic and language-agnostic.
*   **Capabilities:** It covers the entire lifecycle from planning and coding to building, testing, and deploying.
*   **Flexibility:** It can be used as a full suite or as individual services mixed with other tools (e.g., using Azure Boards with GitHub Repos).

### 5. Name the major components of Azure DevOps.
**Answer:**
Azure DevOps consists of five main services:
1.  **Azure Boards:** Agile planning, work item tracking, visualization, and reporting (Kanban/Scrum).
2.  **Azure Repos:** Cloud-hosted private Git repositories and TFVC for version control.
3.  **Azure Pipelines:** CI/CD services for building, testing, and deploying to any target.
4.  **Azure Artifacts:** Package feed management for Maven, npm, NuGet, and Python packages.
5.  **Azure Test Plans:** Manual and exploratory testing tools.

### 6. How does Azure DevOps differ from traditional software development approaches?
**Answer:**
*   **Integration:** Traditional models (like Waterfall) often have separate phases with handoffs. Azure DevOps integrates these phases continuously.
*   **Cycle Time:** Traditional cycles are long (months). Azure DevOps enables daily or weekly releases.
*   **Automation:** Traditional Ops often relies on manual server configuration. Azure DevOps relies on IaC and automated pipelines.
*   **Feedback:** DevOps provides immediate feedback loops through automated testing and monitoring, whereas traditional models often test only at the end.

### 7. What is CI/CD in the context of Azure DevOps?
**Answer:**
In Azure DevOps, CI/CD is the backbone of the automated software factory.
*   **Continuous Integration (CI):** Azure Pipelines triggers a build whenever code is committed to Azure Repos. It compiles code, runs unit tests, and produces valid artifacts.
*   **Continuous Delivery (CD):** The pipeline automatically takes the valid artifacts and deploys them to various environments (Dev, QA, Staging, Prod). It may include approval gates and integration tests.

### 8. What is Infrastructure as Code (IaC) and how is it implemented in Azure DevOps?
**Answer:**
IaC is the management of infrastructure (networks, VMs, load balancers) using machine-readable configuration files instead of physical hardware configuration or interactive configuration tools.
*   **Implementation:** In Azure DevOps, you define infrastructure using templates like **ARM templates** (JSON), **Bicep**, or **Terraform**.
*   **Execution:** These templates are stored in the repo and deployed via Azure Pipelines tasks (e.g., "Terraform Apply" or "ARM Template Deployment"). This ensures infrastructure is versioned and reproducible.

### 9. What is the difference between Azure DevOps Services and Azure DevOps Server?
**Answer:**
*   **Azure DevOps Services:** The cloud-hosted, SaaS version. It is managed by Microsoft, automatically updated, highly scalable, and accessible from anywhere. It has a 99.9% SLA.
*   **Azure DevOps Server:** (Formerly Team Foundation Server / TFS). This is the on-premises version installed on your own hardware. It is chosen by organizations that need data data strictly on-prem due to compliance or have no internet access. You manage the updates and backups yourself.

### 10. How does Azure DevOps support agile methodologies?
**Answer:**
Azure DevOps is built with Agile in mind via **Azure Boards**.
*   **Process Templates:** Supports Basic, Agile, Scrum, and CMMI process templates.
*   **Backlogs:** Allows creation of product backlogs and portfolio backlogs.
*   **Sprints:** Tools for sprint planning, capacity planning, and burndown charts.
*   **Kanban Boards:** Visualizes work flow with customizable columns and Swimlanes.
*   **Dashboards:** Customizable widgets to track velocity, cumulative flow, and other metrics.

## Azure Boards

### 11. What are Azure Boards?
**Answer:**
Azure Boards is the project management hub within Azure DevOps.
*   **Purpose:** It tracks all "Work Items" – which can be bugs, tasks, user stories, features, or epics.
*   **Features:** It provides interactive Kanban boards, backlogs, team dashboards, and custom reporting. It ensures traceability by linking code commits and deployments back to the original work item.

### 12. How do you manage technical debt in Azure Boards?
**Answer:**
Managing technical debt requires visibility and prioritization.
1.  **Create Work Items:** Create specific work items (e.g., "Tech Debt" or "Refactoring") for identified debt.
2.  **Tagging:** Use a tag like "Technical Debt" to easily query and report on these items.
3.  **Backlog Management:** Treat debt like any other feature. Prioritize it in the backlog during sprint planning discussions.
4.  **Allocation:** Dedicate a percentage of sprint capacity (e.g., 20%) specifically to clearing technical debt.

## Azure Repos

### 13. What is Azure Repos?
**Answer:**
Azure Repos provides enterprise-grade Git repositories and Team Foundation Version Control (TFVC).
*   **Features:** It offers unlimited private repositories, semantic code search, branch policies, webhooks, and pull request management.
*   **Security:** It integrates with Azure Active Directory (Entra ID) for varying access levels (Read, Write, Contribute) and supports audit logging.

### 14. What is the difference between Git and TFVC in Azure Repos?
**Answer:**
*   **Git:** Distributed Version Control System (DVCS). Every developer has a full copy of the repo including history. It excels at branching and merging. It is the industry standard and recommended for most new projects.
*   **TFVC:** Centralized Version Control System (CVCS). Developers only check out the specific version of files they are working on. It allows for granular file-level permissions and locking.

### 15. How do you ensure code quality and consistency across multiple development teams in Azure DevOps?
**Answer:**
1.  **Branch Policies:** Enforce rules on target branches (e.g., `main`) requiring code review and passing builds.
2.  **Pull Requests:** Mandate PRs for all changes. Use PR templates to ensure description standards.
3.  **Build Validation:** Run automated CI builds (unit tests, linting, formatting checks) as part of the PR policy.
4.  **Static Analysis:** Integrate tools like SonarQube or WhiteSource in the pipeline to scan for maintainability and security issues.

### 16. What Git branching strategy would you recommend for a team and why?
**Answer:**
The recommendation depends on the team's workflow:
*   **Git Flow:** Recommended for projects with scheduled releases. It uses `develop`, `release`, `master`, and `feature` branches. Good for strict control but complex.
*   **Trunk-Based Development:** Recommended for CI/CD and high-velocity teams. Developers merge to `main` frequently (daily). It simplifies history and reduces merge conflicts, but requires robust automated testing.
*   **GitHub Flow:** A simplified workflow with `main` and feature branches, ideal for continuous deployment.

## Azure Pipelines

### 17. What is Azure Pipelines?
**Answer:**
Azure Pipelines is a cloud service that automates builds and deployments.
*   **Capabilities:** It works with any language (Python, Java, Node.js, C#, Go) and any platform (Windows, Linux, macOS).
*   **Integration:** It integrates natively with GitHub, GitHub Enterprise, Bitbucket, and Azure Repos.
*   **Usage:** It combines CI (Build) pipelines to compile/test code and CD (Release) pipelines to deploy applications.

### 18. Explain the difference between a build pipeline and a release pipeline.
**Answer:**
While both can be defined in YAML now, conceptually:
*   **Build Pipeline (CI):** Focuses on compiling code, running tests, and producing an "Artifact" (e.g., a .zip file, JAR, or Docker image). It usually triggers on every commit.
*   **Release Pipeline (CD):** Focuses on taking that Artifact and deploying it to servers or cloud services. It manages environments, approvals, and release gates. It triggers when a new artifact is available.

### 19. What is a YAML pipeline?
**Answer:**
A YAML pipeline defines the CI/CD process using a text file (usually `azure-pipelines.yml`) stored in the root of the repository.
*   **Benefits:** This represents "Pipeline as Code." The pipeline versioning follows the code versioning. You can review pipeline changes via PRs, revert changes easily, and reuse logic via templates.

### 20. How does continuous integration process work in Azure DevOps?
**Answer:**
The flow is as follows:
1.  Developer commits code to Azure Repos (or linked repo).
2.  Azure Pipelines detects the commit via a trigger.
3.  An Agent is allocated from an Agent Pool.
4.  The Agent checks out the code.
5.  Tasks defined in the pipeline run (Restore dependencies -> Build -> Test -> Publish Artifacts).
6.  If successful, an artifact is published. If failed, the team is notified immediately via email/Slack/Teams.

### 21. How are variable groups used in Azure Pipelines?
**Answer:**
Variable groups allow you to define variables once and share them across multiple pipelines.
*   **Use Case:** Storing environment metadata (e.g., `EnvironmentName: Staging`, `DatabaseConnectionStr: ...`).
*   **Linking to Key Vault:** A variable group can strictly link to an Azure Key Vault, mapping secrets to pipeline variables directly without storing sensitive data in Azure DevOps.

### 22. How do you manage secrets in Azure DevOps pipelines?
**Answer:**
1.  **Azure Key Vault:** The most secure method. Store secrets in Key Vault and link them via a Variable Group.
2.  **Secret Variables:** In the pipeline UI "Variables" tab, define a variable and click the "Lock" icon. This encrypts the value and masks it as `***` in logs.
3.  **Secure Files:** Upload certificates or SSH keys to the "Secure Files" library, which can be downloaded by the agent during the run but are not committed to the repo.

### 23. What are agent pools in Azure Pipelines?
**Answer:**
An agent pool is a collection of agents available to run your jobs.
*   **Organization:** You can organize agents by team or capability (e.g., "Linux Pool", "GPU Pool").
*   **Scopes:** Pools can be scoped to the entire Organization or specific Projects.
*   **Hierarchy:** When a pipeline runs, it requests an agent from a specified pool that satisfies its "demands" (e.g., `npm` must be installed).

### 24. When would you use a self-hosted agent instead of a Microsoft-hosted agent?
**Answer:**
You choose self-hosted when:
*   **Performance:** You need more CPU/RAM than the standard MS-hosted tier offers.
*   **Caching:** You want to persist build caches (Maven/.npm) between runs to speed up large builds.
*   **Networking:** The deployment target is inside a private network (VNet/On-prem) that the public internet cannot access.
*   **Software:** You need custom, licensed, or legacy software pre-installed that takes too long to install during every pipeline run.

### 25. How can you optimize an Azure DevOps pipeline for better performance?
**Answer:**
1.  **Parallelization:** Run independent jobs in parallel using `dependsOn: []` or the `matrix` strategy.
2.  **Caching:** Use the `Cache@2` task to save dependencies (node_modules, nuget packages) so they aren't downloaded every run.
3.  **Artifacts:** Publish only necessary files. Don't publish the entire source tree as an artifact.
4.  **Conditionals:** Use `condition:` checks to skip steps that aren't needed (e.g., skip deployment on non-main branches).
5.  **Self-Hosted Agents:** As mentioned, for persistent caching and faster hardware.

### 26. Explain the different types of deployment strategies in Azure DevOps.
**Answer:**
Azure DevOps supports various strategies, often defined in Deployment Jobs:
*   **runOnce:** The standard strategy. Deploys to all targets simultaneously.
*   **rolling:** Deploys to a percentage of targets at a time (e.g., 20%). Replaces instances gradually to maintain availability.
*   **canary:** Deploys to a small baseline first. You validate validation metrics, then proceed to the rest.
*   **blueGreen:** (Requires environment setup). Deploys to an idle environment (Green), verifies it, then swaps traffic from the active (Blue) environment.

### 27. How do you implement rollback strategies in Azure DevOps?
**Answer:**
*   **Auto-Redeploy Trigger:** In Classic Release Pipelines, configure a stage to auto-redeploy the "last successful release" if the current release fails.
*   **YAML Pipelines:** Use a logic step. If a deployment job fails, trigger a subsequent job that runs a rollback script (e.g., `kubectl rollout undo` or swapping staging slots back).
*   **Feature Flags:** Instead of code rollback, simply toggle the feature flag off if usage metrics degrade.

### 28. How do you troubleshoot pipeline failures in Azure DevOps?
**Answer:**
1.  **Logs:** Click the failed run and view the raw logs. Search for keywords like "Error" or "Exception".
2.  **Debug Mode:** Re-run the pipeline with the system variable `system.debug` set to `true`. This provides verbose logging.
3.  **Worker Diagnosis:** Check if the agent ran out of disk space or memory.
4.  **Change Isolation:** Review the specific commit that triggered the build to see if a code change broke the logic.
5.  **Local Reproduction:** Try to run the same build commands locally to see if they fail outside the pipeline.

### 29. What is the role of container orchestration (e.g., Kubernetes, AKS) in Azure DevOps?
**Answer:**
Container orchestration is a target deployment environment. Azure DevOps integrates tightly with it:
*   **Service Connections:** Connects securely to AKS or generic K8s clusters.
*   **Tasks:** Provides tasks like `Kubernetes@1` or `HelmDeploy` to create namespaces, apply YAML manifests, or upgrade charts.
*   **Environments:** Maps Kubernetes namespaces to Azure DevOps Environments to show pod health and deployment history directly in the Azure DevOps portal.

### 30. How do you use Terraform with Azure DevOps?
**Answer:**
1.  **Extension:** Install the "Terraform by Microsoft DevLabs" or similar extension.
2.  **State Management:** Configure the pipeline to initialize Terraform using a remote backend (Azure Storage Account) to store the state file.
3.  **Pipeline Steps:**
    *   `terraform init`: Prepare working directory.
    *   `terraform plan`: Generate a plan and output it to a file.
    *   `terraform show`: (Optional) Convert plan to JSON for policy checking.
    *   `terraform apply`: Apply the plan to provisioning resources.

### 31. What is the purpose of YAML in Azure Pipelines?
**Answer:**
YAML (Yet Another Markup Language) serves as the syntax for defining "Infrastructure as Code" for pipelines.
*   **Human Readable:** It is structured and easy to read.
*   **Version Controlled:** It lives with the application code.
*   **Complex Workflows:** It supports advanced structures like loops, multi-stage dependencies, and template inheritance that are simpler to manage in text than in a drag-and-drop UI.

## Azure Artifacts

### 32. What is Azure Artifacts?
**Answer:**
Azure Artifacts is a highly scalable package repository service.
*   **Universal:** It supports NuGet (C#), npm (JavaScript), Python (PyPi), Maven (Java), and Universal Packages.
*   **Upstream Sources:** It can proxy public registries (like npmjs.com). When you download a package through Azure Artifacts, it caches it. If the public registry goes down or deletes the package, your build still works.
*   **Versioning:** It handles semantic versioning and immutability of released packages.

### 33. How does one manage packages and artifacts in Azure DevOps?
**Answer:**
*   **Publishing:** Builds compile code and "publish" the result to a Feed in Azure Artifacts.
*   **Consuming:** Developers add a `nuget.config` or `.npmrc` file to their project pointing to the Azure Artifacts Feed URL.
*   **Permissions:** Feeds have separate permissions (Reader/Contributor/collaborator) to control who can publish releases vs. who can just restore them.
*   **Retention Policies:** You configure policies to automatically delete old, unused development versions of packages to save storage.

## Azure Test Plans

### 34. What are Azure Test Plans?
**Answer:**
Azure Test Plans provides tools for comprehensive testing.
*   **Manual Testing:** Allows QAs to define test cases, organize them into test plans/suites, and execute them with a "Test Runner" that records screenshots and steps.
*   **Exploratory Testing:** A browser extension allows testers to navigate the app, capture bugs, annotate screenshots, and create bugs in Azure Boards directly from the browser.
*   **Automated Testing Integration:** It aggregates results from automated tests run in Pipelines to provide pass/fail trends and coverage analysis.

## Security and Best Practices

### 35. How do you ensure security in your Azure DevOps processes?
**Answer:**
Security is a layered approach:
*   **Identity:** Use Azure AD (Entra ID) with MFA.
*   **Access:** Use the least privilege principle. Add users to "Readers" groups unless they need "Contributor" access.
*   **Static Scanning:** Run SAST (Static Application Security Testing) in CI pipelines.
*   **Dependency Scanning:** Scan open-source libraries for known vulnerabilities (e.g., OWASP Dependency Check).
*   **Secrets:** Never commit secrets. Use Key Vault.
*   **Audit:** Review the "Audit Logs" in Organization Settings to track critical changes.

### 36. What are some best practices for Azure DevOps?
**Answer:**
1.  **Pipeline as Code:** Always use YAML pipelines over Classic UI.
2.  **Pull Requests:** Never allow direct commits to `main`.
3.  **Immutable Artifacts:** Build once, deploy everywhere. Do not rebuild code for Staging or Prod; promote the exact binary that passed testing in Dev.
4.  **Tagging:** Tag git commits and Docker images with build numbers for traceability.
5.  **Infrastructure as Code:** Do not manually click in the Azure Portal; use Terraform/Bicep pipelines.

### 37. How do you integrate Azure DevOps with other services for monitoring and feedback?
**Answer:**
*   **Service Hooks:** Use Service Hooks to send events (e.g., Build Failed) to Slack, Teams, or Jenkins.
*   **Azure Monitor/App Insights:** pipelines can deploy "Availability Tests" or configure App Insights instrumentation keys. Release Gates can verify alerts from Azure Monitor before proceeding.
*   **Marketplace:** Use extensions from the Visual Studio Marketplace to integrate with ServiceNow, Jira, Trello, etc.

### 38. How do you implement feature flags in Azure DevOps?
**Answer:**
Azure DevOps couples well with **Azure App Configuration** or **LaunchDarkly**.
*   **Development:** Developers wrap new code in a feature flag check `if (featureEnabled("NewUI")) { ... }`.
*   **Management:** The flag state is stored externally (not in code).
*   **Release:** The code is deployed to prod but "off".
*   **Activation:** The flag is turned "on" for a subset of users via the external dashboard, without requiring a new deployment.

## Advanced Topics and Scenarios

### 39. How would you migrate an existing Jenkins setup to Azure DevOps?
**Answer:**
1.  **Audit:** List all Jenkins jobs, plugins, and credentials.
2.  **Code Migration:** Move source code to Azure Repos (optional, Pipelines works with GitHub too).
3.  **Pipeline Conversion:** Rewrite `Jenkinsfile` logic into `azure-pipelines.yml`.
    *   Map Jenkins Stages to Azure Stages.
    *   Map Shared Libraries to YAML Templates.
4.  **Agents:** Decide whether to use MS-hosted agents or install the Azure Pipelines Agent on the existing Jenkins nodes.
5.  **Cutover:** Run both in parallel, verify artifacts match, then disable Jenkins.

### 40. How would you manage configurations across multiple environments in Azure DevOps?
**Answer:**
*   **Variable Groups:** Create groups named `VarGroup-Dev`, `VarGroup-Prod`.
*   **Pipeline References:** In the pipeline YAML, reference the specific group based on the stage:
    ```yaml
    stages:
    - stage: DeployDev
      variables:
      - group: VarGroup-Dev
    ```
*   **Tokenization:** Compile a configuration file with tokens (e.g., `__DBConnection__`) and use a "Replace Tokens" task during deployment to swap them with values from the Variable Group.

### 41. What is the role of the `.yml` file in Azure Pipelines?
**Answer:**
The `.yml` file is the blueprint of the automated process.
*   **Definitions:** It specifies *triggers* (when to run), *pool* (where to run), *variables*, and *steps* (script, task, powershell).
*   **Schema:** It follows a strict schema defined by Microsoft.
*   **Location:** It usually resides at the root of the repository, ensuring the build logic changes evolve alongside the application logic.

### 42. How would you integrate a third-party tool like SonarQube for code quality analysis in an Azure Pipeline?
**Answer:**
1.  **Service Connection:** Create a service connection in Azure DevOps Project Settings to authenticate with the SonarQube server.
2.  **Pipeline Tasks:** Add the specific SonarQube tasks to the YAML pipeline:
    *   `SonarQubePrepare`: Configures the project key and sources.
    *   `[Build Task]`: Run your build (Maven/Gradle/DotNet).
    *   `SonarQubeAnalyze`: Uploads the code analysis report.
    *   `SonarQubePublish`: Publishes the Quality-Gate result to the build summary.

### 43. Describe a scenario where you used a multi-stage pipeline in Azure DevOps.
**Answer:**
**Scenario:** A web application requiring deployment to Dev, QA, and Prod.
**Implementation:**
*   **Stage 1 - CI:** Triggers on commit. Builds code, runs unit tests, publishes ZIP artifact.
*   **Stage 2 - Dev:** Triggers after CI. Downloads artifact, deploys to Dev App Service.
*   **Stage 3 - QA:** Triggers after Dev. Deploys to QA. Runs Selenium functional tests.
*   **Stage 4 - Prod:** Does not trigger automatically. Requires a "Manual Approval" check in the Environment settings. Once approved by a manager, it deploys to Prod.

### 44. How do you handle large-scale deployments or deployments to multiple regions in Azure DevOps?
**Answer:**
*   **Templating:** Use YAML templates to define the deployment logic once (e.g., `deploy-to-region.yml`).
*   **Looping:** In the main pipeline, iterate (using `each` syntax) over a list of regions: `regions: ['eastus', 'westus', 'europenorth']`.
*   **Traffic routing:** Deploy to the new region, verify health, and then update Azure Front Door or Traffic Manager to route user traffic to the new deployment.

### 45. What is the purpose of deployment groups in Azure DevOps?
**Answer:**
Deployment Groups are for deploying to Virtual Machines (on-prem or cloud-based IaaS).
*   **Agent:** You install a lightweight Azure Pipelines agent on each target VM.
*   **Grouping:** You tag VMs (e.g., "Web", "DB", "US-East").
*   **Logic:** In the pipeline, you target the "Deployment Group" and filter by tags. The job runs locally on those machines in parallel. This is essential for scenarios where you cannot just "swap a slot" like in PaaS.

### 46. How do you manage dependencies for your applications within Azure DevOps?
**Answer:**
Dependency management is handled by **Azure Artifacts**.
*   **Upstream Sources:** Configure the Artifacts feed to include npmjs or NuGet.org as upstream sources.
*   **Workflow:** When a build runs, it requests a package. Azure Artifacts checks if it has it. If not, it fetches it from the upstream, caches it, and serves it.
*   **Benefit:** This guarantees deterministic builds. Even if a package is deleted from the public internet (left-pad incident), your copy remains in Azure Artifacts.

### 47. What is the significance of "Environment" in Azure Pipelines?
**Answer:**
An "Environment" represents a logical target (e.g., "Production", "Staging").
*   **Traceability:** It records exactly what code and artifacts are currently running in that environment.
*   **Checks and Gates:** You can define approvals (manual user sign-off), branch control (only allow `main` branch), or business hours checks on the Environment itself. The pipeline pauses execution until these checks pass.

### 48. How do you ensure traceability from a work item to a deployment in Azure DevOps?
**Answer:**
Azure DevOps handles this via its integrated data model.
1.  **Commit Linking:** When committing code, developers include `#123` (Work Item ID). Azure Repos links the commit to the Story/Bug.
2.  **Build Linking:** The Pipeline builds that commit. The Build summary now lists the Work Item.
3.  **Release Linking:** The Release deploys that Build.
4.  **Result:** You can open a User Story on the Board and see the "Development" section showing: "Deployed to Production".

### 49. How would you integrate Azure DevOps with GitHub for source control?
**Answer:**
Microsoft owns both, so integration is seamless.
1.  **Service Connection:** Create a GitHub Service Connection using OAuth or a Personal Access Token (PAT).
2.  **Pipeline Trigger:** In the YAML file, specify:
    ```yaml
    resources:
      repositories:
        - repository: myRepo
          type: github
          name: user/repo
          endpoint: myGithubConnection
    ```
3.  **Status Checks:** Azure Pipelines reports "Build Status" back to the GitHub PR UI, blocking merges if valid checks fail.

### 50. What is the role of "gates" in Azure Pipelines release processes?
**Answer:**
Gates allow for **automated** approvals based on health signals, removing human intervention.
*   **Example:** A "Query Azure Monitor" gate.
*   **Workflow:** After deploying to Canary, the pipeline waits. The gate queries Azure Monitor: "Is CPU usage < 80% AND Error Rate < 1%?".
*   **Evaluation:** It samples this metrics repeatedly (e.g., every 5 mins for 1 hour). If successful, it proceeds to the full Production rollout. If fails, it stops or rolls back.

## Real-time Scenarios and Advanced Concepts

### 51. Scenario: A multi-stage pipeline successfully builds and deploys an application to AKS, but the application becomes unreachable after deployment. How would you diagnose and resolve this issue?
**Answer:**
*   **Diagnosis Steps:**
    1.  **Pod Status:** Run `kubectl get pods`. Checking for `CrashLoopBackOff` or `ImagePullBackOff`.
    2.  **Logs:** Run `kubectl logs <pod-name>`. Look for application startup errors (e.g., database connection failure).
    3.  **Service:** Run `kubectl get svc`. Check if the `EXTERNAL-IP` is pending or if the `ClusterIP` port matches the container port.
    4.  **Selector:** Run `kubectl describe svc`. Ensure the `Selector` matches the labels on the Pods. This is a common error.
    5.  **Ingress:** Check `kubectl get ingress`. Verify the host rule matches the URL being accessed.
*   **Resolution:** If the logs show a missing secret, update the Kubernetes Secret. If the Service selector is wrong, patch the Service YAML.

### 52. Scenario: Your Azure DevOps pipeline takes too long to build and deploy. How would you optimize it?
**Answer:**
*   **Analysis:** Look at the timeline view to see which task takes the longest.
*   **Action Plan:**
    1.  **Test Parallelization:** Split tests into batches and run them on multiple agents.
    2.  **Cache Dependencies:** Ensure `npm install` or `mvn install` isn't downloading the internet every time. Use the Pipeline Cache task.
    3.  **Docker Layers:** Optimize Dockerfiles. Put frequent changes (source code copy) at the bottom and static installs (apt-get) at the top to leverage layer caching.
    4.  **Agent Spec:** Upgrade the agent SKU if the build is CPU bound.

### 53. Scenario: Frequent merge conflicts with multiple developers. What strategy to recommend?
**Answer:**
*   **Root Cause:** Long-lived feature branches. The longer a branch exists separately from main, the harder it is to merge.
*   **Strategy:** Move to **Trunk-Based Development**.
*   **Tactics:**
    1.  Developers pull from main and merge their changes *locally* multiple times a day.
    2.  Use **Feature Flags** to hide incomplete code so it can be merged safely without breaking production.
    3.  Enforce smaller, more atomic Pull Requests (e.g., < 20 files).

### 54. How do you handle failed releases and automated rollback?
**Answer:**
*   **Strategy:** "Fail Forward" or "Roll Back".
*   **Helm/K8s:** Use `helm rollback <release> 0` to revert to the previous revision.
*   **App Service:** Use "Deployment Slots". Deploy to a "Staging" slot. If warm-up fails, do not swap. If swapped and issues occur, "Swap" back immediately to restore the previous version.
*   **Database:** This is the hardest part. Rollbacks usually don't include data. We use backward-compatible schema changes (e.g., add column, don't rename) so the old code works with the new schema.

### 55. Difference between Microsoft-hosted and Self-hosted agents?
**Answer:**
*   **Microsoft-hosted:** "Serverless" build agents.
    *   *Pros:* Zero maintenance, fresh environment every time, simple billing.
    *   *Cons:* 60-min timeout limit (free tier), limited disk space, no persistent state (slower builds), public IP ranges (security).
*   **Self-hosted:** You install the agent on your VM/Container.
    *   *Pros:* Persistent cache, access to intranet resources, unlimited timeouts, specific hardware (GPUs).
    *   *Cons:* You must patch the OS, update the agent, and manage scaling.

### 56. YAML Pipeline vs Classic Editor?
**Answer:**
*   **Classic Editor:** Visual designer.
    *   *Pro:* Easy discovery of tasks, good for quick prototypes.
    *   *Con:* Stored in proprietary JSON database, hard to audit diffs, cannot be branched/merged.
*   **YAML:** Text-based.
    *   *Pro:* Stored in Git. Follows standard PR process. Supports "Pipeline as Code". Supports Templates for reuse.
    *   *Con:* Steeper learning curve, requires knowing syntax.
    *   *Verdict:* Use YAML for all new projects.

### 57. Explain "Shift-Left" in Azure DevOps.
**Answer:**
It stands for moving quality and security checks to the earlier stages of the pipeline (left side of the timeline).
*   **Traditional:** Code -> Build -> Deploy -> QA Test -> Security Scan. (Bugs found here are expensive).
*   **Shift-Left:** Code -> (Unit Test + SAST Scan + Linter) -> Build.
*   **Implementation:** We block the PR merge if a security vulnerability is found, effectively preventing bad code from ever entering the main branch.

### 58. How do you secure secrets in Azure Pipelines?
**Answer:**
Hardcoding secrets is a critical vulnerability.
1.  **Azure Key Vault:** Create a Key Vault. specific access policies for the DevOps Service Principal.
2.  **Variable Group Link:** In DevOps Library, create a Variable Group, toggle "Link secrets from an Azure Key Vault".
3.  **Pipeline Access:**
    ```yaml
    variables:
    - group: prod-secrets
    steps:
    - script: echo $(MySecretPassword)
    ```
    (Note: The echo output will actually be `***` because DevOps automatically masks known secrets).

### 59. What are Branch Policies?
**Answer:**
Branch policies are the primary defense for the `main` branch.
*   **Minimum Reviewers:** Requires at least 2 humans to approve.
*   **Work Item Linking:** Forces traceability.
*   **Build Validation:** Pre-merges the changes and runs the build. If the build fails, the merge is blocked.
*   **Comment Resolution:** Cannot complete merge if there are active conversations/rejects in the code review.

### 60. How does Azure DevOps integrate with containers?
**Answer:**
Azure DevOps is a first-class citizen for container workflows.
*   **Build:** The `Docker@2` task allows building Dockerfiles and tagging images.
*   **Push:** It authenticates via Service Connections to push images to Azure Container Registry (ACR), Docker Hub, or GCR.
*   **Scan:** Tasks like Trivy or Aqua Security can scan the image layers during the pipeline.
*   **Deploy:** The `KubernetesManifest@0` task takes the image and deploys it to a cluster, handling secret creation for pulling the image from private registries.

