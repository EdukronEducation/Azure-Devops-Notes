# Module 5: Azure Artifacts (Package Management)

## 5.1 Introduction to Package Management

### What are Packages and Artifacts?

Packages are collections of files, metadata, and dependencies that are bundled together for distribution and reuse. In software development, packages typically contain compiled code, libraries, configuration files, and documentation that can be shared across projects or teams. Packages enable code reuse, dependency management, and version control for shared components. Common examples include NuGet packages for .NET, npm packages for Node.js, Maven packages for Java, and Python packages.

Artifacts are the outputs of build processes, such as compiled applications, Docker images, or deployment packages. While packages are typically designed for reuse and distribution, artifacts are the specific outputs created during a build that are ready for deployment. In Azure DevOps, the term "artifacts" can refer to both build outputs and packages stored in Azure Artifacts feeds. Understanding the distinction helps teams use the right tool for the right purpose—packages for reusable components and artifacts for deployment-ready outputs.

Package management involves creating, storing, versioning, and consuming packages. It includes processes for publishing packages to repositories, resolving dependencies, and ensuring that applications use the correct versions of packages. Effective package management is essential for modern software development, where applications depend on many external libraries and components. Azure Artifacts provides a comprehensive package management solution that integrates with Azure DevOps workflows.

### Package Management Concepts

Package management involves several key concepts that teams need to understand. Repositories or feeds are storage locations where packages are published and from which they're consumed. Versioning is critical, as packages evolve over time and applications need to reference specific versions. Dependency resolution ensures that when an application requires a package, all of that package's dependencies are also retrieved. Semantic versioning (semver) is a common versioning scheme that uses major.minor.patch version numbers to indicate the type of changes.

Package managers are tools that handle the creation, installation, and management of packages. Different programming languages and ecosystems have their own package managers, such as NuGet for .NET, npm for Node.js, Maven for Java, and pip for Python. These tools integrate with repositories to download and install packages, resolve dependencies, and manage package versions in projects.

Security is a critical aspect of package management, as packages can contain vulnerabilities or malicious code. Package management systems need to support security scanning, vulnerability detection, and license compliance. Understanding these concepts helps teams implement effective package management practices that ensure code reuse, dependency management, and security.

### Supported Package Types

Azure Artifacts supports multiple package types, making it a versatile solution for diverse technology stacks. NuGet packages are supported for .NET development, allowing teams to publish and consume .NET libraries and tools. npm packages support Node.js development, enabling teams to manage JavaScript and TypeScript dependencies. Maven packages support Java development, providing dependency management for Java applications. Python packages (PyPI format) support Python development, allowing teams to manage Python dependencies.

Universal packages are a format unique to Azure Artifacts that can contain any type of file, making them useful for scenarios that don't fit into standard package formats. Cargo packages support Rust development, and Azure Artifacts continues to add support for additional package types as they become popular. This multi-format support enables organizations to use a single platform for package management across different technology stacks.

Each package type has its own characteristics, versioning schemes, and tooling, but Azure Artifacts provides a unified interface for managing them all. Understanding the supported package types helps teams choose the right format for their needs and leverage Azure Artifacts effectively across different projects and technologies.

### Feed Types (Organization, Project, Public)

Azure Artifacts supports different feed types that determine who can access packages and how feeds are organized. Organization feeds are shared across all projects in an organization, making them ideal for packages that multiple teams or projects need to access. They provide a centralized location for shared packages and enable consistent package management across the organization.

Project feeds are scoped to specific projects, providing isolation for packages that are only relevant to a particular project or team. Project feeds are useful when packages are project-specific or when teams want to maintain separate package management for different initiatives. They provide better isolation but may lead to duplication if the same packages are needed in multiple projects.

Public feeds allow anonymous access to packages, making them suitable for open-source packages or scenarios where packages need to be accessible without authentication. Public feeds can be useful for sharing packages externally or for scenarios where authentication isn't practical. Understanding feed types helps organizations structure their package management to balance accessibility, isolation, and security based on their specific needs.

---

## 5.2 Working with Azure Artifacts

### Creating and Managing Feeds

Creating feeds in Azure Artifacts is straightforward and can be done through the Azure DevOps web interface. When creating a feed, you choose the feed type (organization or project), name the feed, and configure access permissions. Feeds can be created for different purposes, such as hosting internal packages, mirroring public packages, or organizing packages by team or project. Well-organized feeds make it easier to find and manage packages.

Managing feeds involves configuring permissions, setting up upstream sources, configuring retention policies, and monitoring usage. Feed permissions control who can read packages, who can publish packages, and who can administer the feed. Upstream sources allow feeds to automatically pull packages from public repositories when they're requested, providing a seamless experience while maintaining control over package sources.

Feed management also includes monitoring package usage, identifying unused packages, and managing feed storage. Understanding how to create and manage feeds effectively helps organizations structure their package management to support their development workflows and security requirements.

### Publishing Packages

Publishing packages to Azure Artifacts feeds makes them available for consumption by other projects or teams. The publishing process varies by package type but typically involves using the appropriate package manager tool (like NuGet, npm, or Maven) to publish to the feed. Packages are versioned, and new versions can be published as packages evolve. Publishing requires appropriate permissions on the feed.

The publishing process typically involves authenticating with the feed, building or preparing the package, and then publishing it using package manager commands or CI/CD pipelines. Azure Artifacts provides clear instructions for publishing each package type, and the process is well-integrated with Azure DevOps authentication. Publishing through CI/CD pipelines ensures that packages are created consistently and automatically as part of the build process.

Understanding how to publish packages helps teams share code and components effectively. Publishing should follow versioning best practices, and teams should consider the impact of breaking changes when publishing new versions. Effective publishing practices ensure that packages are available when needed and that versioning is managed appropriately.

### Consuming Packages

Consuming packages from Azure Artifacts feeds involves configuring projects to use the feed as a package source and then installing packages using standard package manager tools. The process varies by package type but typically involves adding the feed URL to package manager configuration and then installing packages as usual. Azure Artifacts provides clear instructions for configuring each package manager to use feeds.

Consuming packages from Azure Artifacts is transparent to developers—they use the same package manager commands they're familiar with, and packages are retrieved from the Azure Artifacts feed instead of public repositories. This transparency makes it easy to adopt Azure Artifacts without changing developer workflows. Upstream sources can automatically fetch packages from public repositories if they're not in the feed, providing a seamless experience.

Understanding how to consume packages helps teams leverage shared components and manage dependencies effectively. Teams should understand feed permissions, how to configure package managers, and how upstream sources work to consume packages successfully.

### Package Versioning Strategies

Package versioning is critical for managing package evolution and ensuring that applications use compatible versions. Semantic versioning (semver) is the most common versioning scheme, using major.minor.patch version numbers where major versions indicate breaking changes, minor versions indicate new features, and patch versions indicate bug fixes. Following semantic versioning helps consumers understand the impact of version updates.

Versioning strategies should consider how packages are consumed and the impact of version changes. Pre-release versions (like alpha, beta, or release candidates) allow teams to publish packages for testing before making them generally available. Version ranges in dependency specifications allow some flexibility while maintaining compatibility. Understanding versioning strategies helps teams manage package evolution effectively and communicate changes to consumers.

Azure Artifacts supports standard versioning schemes for each package type and provides features for managing package versions, such as promoting versions between feeds or marking versions as deprecated. Understanding package versioning strategies helps teams publish packages that are easy to consume and maintain.

### Package Promotion

Package promotion allows teams to move packages between feeds, typically from development or testing feeds to production feeds. This enables a controlled release process where packages are validated in lower environments before being made available for production use. Promotion can be manual or automated through CI/CD pipelines, and it provides an audit trail of package movement through environments.

Promotion workflows help ensure that only validated packages reach production and provide a clear process for package release. They enable teams to test packages in development environments before promoting them for broader use. Promotion can also be used to move packages between feeds for organizational reasons, such as moving packages from project feeds to organization feeds as they mature.

Understanding package promotion helps teams implement controlled release processes for packages. Promotion workflows should be defined based on organizational needs and can be automated to reduce manual effort while maintaining control over package releases.

### Upstream Sources

Upstream sources allow Azure Artifacts feeds to automatically pull packages from public repositories (like npmjs.com, NuGet.org, or Maven Central) when they're requested but not found in the feed. This provides a seamless experience where developers can use packages from both the feed and public repositories without managing multiple package sources. Upstream sources act as a proxy, caching packages locally for faster subsequent access.

Configuring upstream sources enables feeds to serve as a single source for packages while maintaining access to the broader package ecosystem. When a package is requested, Azure Artifacts first checks the feed, and if not found, it retrieves the package from the upstream source and caches it in the feed. This provides the benefits of local caching and control while maintaining access to public packages.

Understanding upstream sources helps teams configure feeds to provide comprehensive package access while maintaining control and performance. Upstream sources should be configured based on organizational policies about which public repositories to trust and how to handle package requests.

---

## 5.3 Package Types

### NuGet Packages (.NET)

NuGet is the package manager for .NET, and Azure Artifacts provides full support for NuGet packages. NuGet packages contain .NET assemblies, dependencies, and metadata, and they're the standard way to share .NET code and libraries. Teams can publish NuGet packages to Azure Artifacts feeds and consume them in .NET projects using standard NuGet tooling.

Publishing NuGet packages involves creating a .nuspec file or using project files to define package metadata, building the package using NuGet or MSBuild, and then publishing to the feed using NuGet commands or CI/CD pipelines. Consuming NuGet packages involves adding the feed as a NuGet source in Visual Studio or configuring NuGet.config, and then installing packages using Package Manager Console or by editing project files.

Azure Artifacts integrates seamlessly with .NET development workflows, and NuGet packages published to feeds are available in Visual Studio and other .NET tools. Understanding NuGet package management in Azure Artifacts helps .NET teams share code effectively and manage dependencies across projects.

### npm Packages (Node.js)

npm (Node Package Manager) is the package manager for Node.js, and Azure Artifacts supports npm packages for JavaScript and TypeScript development. npm packages are defined by package.json files and can contain JavaScript code, TypeScript definitions, and dependencies. Teams can publish npm packages to Azure Artifacts feeds and consume them using standard npm commands.

Publishing npm packages involves creating a package.json file with appropriate metadata, building the package if needed, and publishing using npm publish commands configured to use the Azure Artifacts feed. Consuming npm packages involves configuring npm to use the feed as a registry and then installing packages using standard npm install commands. Azure Artifacts provides .npmrc configuration that makes it easy to set up feeds.

npm package management in Azure Artifacts works seamlessly with Node.js development workflows, and packages are consumed using standard npm tooling. Understanding npm package management helps Node.js teams leverage Azure Artifacts for dependency management and code sharing.

### Maven Packages (Java)

Maven is a build tool and package manager for Java, and Azure Artifacts supports Maven packages (artifacts) for Java development. Maven packages are defined by pom.xml files and contain Java classes, dependencies, and metadata. Teams can publish Maven packages to Azure Artifacts feeds and consume them in Maven projects using standard Maven tooling.

Publishing Maven packages involves configuring Maven settings to publish to the Azure Artifacts feed and using Maven deploy commands. Consuming Maven packages involves adding the feed as a Maven repository in pom.xml or settings.xml files and then declaring dependencies as usual. Azure Artifacts provides Maven configuration that integrates with standard Maven workflows.

Maven package management in Azure Artifacts enables Java teams to share libraries and manage dependencies effectively. Understanding Maven package management helps Java teams use Azure Artifacts for their dependency management needs.

### Python Packages (PyPI)

Azure Artifacts supports Python packages in PyPI (Python Package Index) format, enabling Python teams to manage dependencies and share code. Python packages are defined by setup.py or pyproject.toml files and contain Python modules, dependencies, and metadata. Teams can publish Python packages to Azure Artifacts feeds and consume them using pip or other Python package managers.

Publishing Python packages involves building the package using standard Python tools and publishing to the feed using twine or pip. Consuming Python packages involves configuring pip to use the feed as an index and installing packages using standard pip install commands. Azure Artifacts provides configuration that integrates with Python development workflows.

Python package management in Azure Artifacts enables Python teams to share code and manage dependencies. Understanding Python package management helps Python teams leverage Azure Artifacts for their package management needs.

### Universal Packages

Universal packages are a format unique to Azure Artifacts that can contain any type of file, making them useful for scenarios that don't fit into standard package formats. Universal packages are versioned and can be published and consumed using Azure Artifacts tools or the REST API. They're useful for distributing configuration files, scripts, documentation, or any other files that need to be versioned and shared.

Universal packages provide flexibility for teams that need to share files that aren't traditional code packages. They can be used for deployment artifacts, configuration templates, or any other versioned files that teams need to distribute. Understanding universal packages helps teams leverage Azure Artifacts for scenarios beyond traditional package management.

### Cargo Packages (Rust)

Azure Artifacts supports Cargo packages for Rust development, enabling Rust teams to manage dependencies and share code. Cargo is the package manager for Rust, and packages are defined by Cargo.toml files. Teams can publish Cargo packages to Azure Artifacts feeds and consume them in Rust projects using standard Cargo tooling.

Cargo package management in Azure Artifacts works similarly to other package types, with configuration to use the feed as a registry and standard Cargo commands for publishing and consuming packages. Understanding Cargo package management helps Rust teams use Azure Artifacts for their dependency management needs.

---

## 5.4 Integration with Pipelines

### Publishing Artifacts in Pipelines

Azure Pipelines can publish packages to Azure Artifacts feeds as part of the build and release process. This enables automated package creation and publishing whenever code changes, ensuring that packages are always up to date and that the publishing process is consistent and repeatable. Publishing in pipelines typically involves building the package and then using package manager commands or Azure DevOps tasks to publish to the feed.

Publishing packages in pipelines ensures that packages are created from the same source code that's being built and tested, providing consistency and traceability. Pipeline publishing can be conditional, only publishing packages when certain conditions are met, such as successful tests or specific branch builds. This enables teams to publish pre-release packages from feature branches and stable packages from main branches.

Understanding how to publish packages in pipelines helps teams automate package creation and ensure consistency. Pipeline publishing should be integrated with versioning strategies to ensure that package versions are managed appropriately.

### Consuming Packages in Pipelines

Azure Pipelines can consume packages from Azure Artifacts feeds during builds, enabling builds to use internal packages and dependencies. Consuming packages in pipelines involves configuring the pipeline to authenticate with the feed and then using standard package manager commands to restore and install packages. Azure Pipelines provides built-in tasks for common package managers that handle authentication automatically.

Consuming packages in pipelines ensures that builds use the correct versions of dependencies and that internal packages are available during builds. Pipeline consumption can be configured to use specific package versions or version ranges, and it can cache packages to improve build performance. Understanding how to consume packages in pipelines helps teams ensure that builds have access to required dependencies.

### Package Restore in Builds

Package restore is the process of downloading and installing packages that are required by a project but aren't already present. In Azure Pipelines, package restore typically happens automatically at the beginning of builds when using standard build tasks, but it can also be explicitly configured. Package restore ensures that all dependencies are available before compilation or testing begins.

Efficient package restore is important for build performance, and Azure Pipelines supports caching to speed up restore operations. Understanding package restore helps teams optimize build times and ensure that builds have all required dependencies. Restore should be fast and reliable to maintain quick build feedback.

### Artifact Retention Policies

Artifact retention policies control how long packages and build artifacts are kept in Azure Artifacts feeds. Retention policies help manage storage costs and ensure that old or unused packages don't accumulate indefinitely. Policies can be configured at the feed level or organization level, and they can specify different retention periods for different package types or versions.

Retention policies should balance the need to keep packages available with the cost of storage. Pre-release packages might have shorter retention periods than stable packages, and packages that are no longer used can be removed more aggressively. Understanding retention policies helps teams manage feed storage effectively and control costs while maintaining access to needed packages.

### Build Artifacts vs. Package Artifacts

Build artifacts are the outputs of build processes, such as compiled applications, Docker images, or deployment packages. They're typically consumed by deployment pipelines and are specific to a particular build. Package artifacts are reusable components that are published to feeds and consumed by multiple projects or builds. Understanding the distinction helps teams use the right mechanism for the right purpose.

Build artifacts are typically stored temporarily and consumed soon after creation, while package artifacts are stored long-term in feeds for ongoing consumption. Build artifacts are usually created as part of CI processes and consumed by CD processes, while packages are published for reuse across projects and time. Teams should use build artifacts for deployment-ready outputs and packages for reusable components.

---

## 5.5 Security and Compliance

### Package Security Scanning

Package security scanning identifies vulnerabilities in packages, helping teams understand and address security risks in their dependencies. Azure Artifacts can integrate with security scanning tools to analyze packages for known vulnerabilities, outdated dependencies, and other security issues. Security scanning should be part of the package management process to ensure that teams are aware of and can address security risks.

Security scanning can happen when packages are published, when they're consumed, or both. Scanning results can block package publishing or consumption, alert teams to issues, or provide information for risk assessment. Understanding package security scanning helps teams implement security practices that protect their applications and infrastructure.

### License Compliance

License compliance involves ensuring that packages used in projects comply with organizational policies and legal requirements. Different packages have different licenses, and organizations need to understand and comply with license terms. Azure Artifacts can track package licenses and help teams identify packages that might not comply with organizational policies.

License compliance is important for legal and business reasons, and teams should understand the licenses of packages they use. Azure Artifacts provides visibility into package licenses, and teams can configure policies to alert or block packages with non-compliant licenses. Understanding license compliance helps teams use packages responsibly and avoid legal issues.

### Package Access Control

Package access control determines who can read, publish, and administer packages in feeds. Access control is configured through Azure DevOps permissions, and it can be set at the feed level or organization level. Proper access control ensures that packages are only accessible to authorized users and that publishing is controlled appropriately.

Access control should balance accessibility with security, ensuring that teams can access the packages they need while preventing unauthorized access or publishing. Understanding package access control helps organizations implement security practices that protect their packages and ensure that package management follows organizational policies.

### Retention Policies

Retention policies control how long packages are kept in feeds, helping manage storage costs and ensure that old packages don't accumulate. Policies can specify different retention periods for different package types, versions, or feeds, and they can be configured to automatically delete packages that exceed retention periods. Retention policies should balance the need to keep packages available with storage costs.

Understanding retention policies helps teams manage feed storage effectively and control costs. Policies should be configured based on organizational needs, considering how long packages are likely to be needed and the cost of storage. Effective retention policies help maintain organized, cost-effective package management.

### Package Deletion and Restore

Packages can be deleted from feeds when they're no longer needed or when retention policies require removal. Deletion can be manual or automatic based on retention policies, and it can be permanent or allow for restore within a certain period. Understanding package deletion helps teams manage feeds effectively and recover from accidental deletions if needed.

Package restore from deletion depends on retention policies and backup strategies. Some deletions can be reversed if they're recent, while others might be permanent. Teams should understand deletion and restore capabilities to manage packages effectively and recover from mistakes if necessary.

---

## Quick Reference

### Package Types
- **NuGet**: .NET packages
- **npm**: Node.js packages
- **Maven**: Java packages
- **Python**: Python packages
- **Universal**: Any file type

### Feed Types
- **Organization Feed**: Shared across projects
- **Project Feed**: Project-specific
- **Public Feed**: Anonymous access

---

## Common Pitfalls

### Pitfall 1: Not Using Feeds
**Problem**: External dependencies, security risks
**Solution**: Use Azure Artifacts feeds
**Prevention**: Set up feeds from the start

### Pitfall 2: Not Configuring Upstream Sources
**Problem**: Slower builds, external dependencies
**Solution**: Configure upstream sources
**Prevention**: Set up upstreams during feed creation

### Pitfall 3: Not Managing Package Versions
**Problem**: Dependency conflicts, breaking changes
**Solution**: Use semantic versioning
**Prevention**: Follow versioning best practices

---

## Best Practices

1. **Use Feeds**: Centralized package management
2. **Configure Upstream Sources**: Automatic package caching
3. **Use Semantic Versioning**: Clear versioning scheme
4. **Set Retention Policies**: Manage feed storage
5. **Secure Feeds**: Appropriate permissions
6. **Monitor Usage**: Track package consumption
7. **Document Packages**: Clear package descriptions
8. **Scan for Vulnerabilities**: Security scanning
9. **Use Organization Feeds**: For shared packages
10. **Review Regularly**: Clean up unused packages

---

## Further Reading

### Official Documentation
- [Azure Artifacts](https://docs.microsoft.com/azure/devops/artifacts/)
- [Package Management](https://docs.microsoft.com/azure/devops/artifacts/overview)
- [Feeds](https://docs.microsoft.com/azure/devops/artifacts/feeds/)

### Related Topics
- Azure Pipelines (Module 3)
- Security and Permissions (Module 7)
- Best Practices and Patterns (Module 10)

---

*This module covers Azure Artifacts and package management in detail. Understanding package management is essential for managing dependencies and sharing code across projects, and Azure Artifacts provides comprehensive package management capabilities.*

