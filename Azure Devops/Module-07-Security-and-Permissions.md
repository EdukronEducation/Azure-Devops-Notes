# Module 7: Security and Permissions

## 7.1 Security Overview

### Azure DevOps Security Model

Azure DevOps implements a comprehensive security model that protects data, controls access, and ensures compliance with security standards. The security model is built on multiple layers including network security, identity and access management, data encryption, and audit logging. Azure DevOps Services runs on Microsoft's Azure infrastructure, which provides enterprise-grade security including physical security, network isolation, and compliance certifications. The platform uses industry-standard security practices and undergoes regular security audits and penetration testing.

The security model includes authentication to verify user identity, authorization to control what users can do, and encryption to protect data at rest and in transit. Azure DevOps supports multiple authentication methods including Microsoft accounts, Azure Active Directory, and personal access tokens. Authorization is managed through a hierarchical permission system that controls access at the organization, project, and resource levels. Data is encrypted using industry-standard encryption algorithms, and encryption keys are managed by Microsoft with options for customer-managed keys.

Understanding the Azure DevOps security model helps organizations configure security appropriately and leverage built-in security features. The model is designed to be secure by default while remaining flexible enough to support various organizational requirements. Organizations should understand how authentication, authorization, and encryption work together to protect their data and resources.

### Authentication Methods

Azure DevOps supports multiple authentication methods to accommodate different organizational needs and security requirements. Microsoft account authentication uses Microsoft's identity system and is suitable for individuals and small teams. Azure Active Directory (Azure AD) integration provides enterprise-grade identity management with features like single sign-on, multi-factor authentication, conditional access policies, and centralized user management. Personal access tokens (PATs) provide programmatic access for scripts, tools, and services that need to authenticate with Azure DevOps.

Azure AD integration is recommended for organizations as it provides enhanced security features and centralized management. It enables single sign-on across Microsoft services, supports multi-factor authentication, and allows organizations to enforce conditional access policies based on device, location, or other factors. Azure AD integration also provides better auditability and user lifecycle management.

Personal access tokens are useful for automation and integration scenarios but should be managed carefully as they provide full access to the resources the user can access. Tokens should be scoped to minimum necessary permissions, have expiration dates, and be rotated regularly. Understanding authentication methods helps organizations choose the right approach for their needs and implement security best practices.

### Authorization and Permissions

Authorization in Azure DevOps controls what authenticated users can do. Permissions are managed through a hierarchical system with permissions at the organization, project, and resource levels. More specific permissions override more general ones, allowing fine-grained access control. Permissions can be granted to individual users or groups, and Azure DevOps provides built-in groups with common permission sets.

The permission system supports both allow and deny permissions, with deny permissions taking precedence. This provides flexibility to grant broad permissions and then deny specific permissions where needed, or to grant specific permissions to groups while denying broader permissions. Permission inheritance means that permissions granted at higher levels apply to lower levels unless overridden.

Understanding authorization and permissions is essential for implementing proper access control. Organizations should follow the principle of least privilege, granting users only the permissions they need to perform their work. Regular permission reviews help ensure that access remains appropriate as roles and responsibilities change. Understanding the permission system helps organizations configure access control effectively.

### Security Best Practices

Security best practices for Azure DevOps include using Azure AD for authentication, implementing multi-factor authentication, following the principle of least privilege for permissions, regularly reviewing and auditing access, using secure connections, managing secrets properly, and keeping the platform updated. Organizations should also implement security policies, train users on security practices, and monitor for suspicious activity.

Multi-factor authentication significantly improves security by requiring additional verification beyond passwords. It should be enabled for all users, especially those with administrative permissions. Regular access reviews help ensure that permissions remain appropriate and that former employees or changed roles don't retain unnecessary access. Secret management should use Azure DevOps's secure storage features rather than hard-coding secrets in code or configuration files.

Security best practices should be tailored to organizational needs and risk tolerance. They should balance security with usability, ensuring that security measures don't unduly hinder productivity. Understanding security best practices helps organizations implement effective security measures that protect their data and resources while supporting efficient workflows.

---

## 7.2 User and Group Management

### Adding Users to Organizations

Adding users to Azure DevOps organizations involves inviting users by email address and assigning them appropriate access levels and permissions. Users can be added individually or in bulk, and they can be assigned to projects and teams. When users are added, they receive invitations that they must accept to gain access. User management should follow organizational policies and procedures to ensure that only authorized users are granted access.

User addition should be part of a structured onboarding process that includes assigning appropriate access levels, adding users to relevant projects and teams, and providing training on Azure DevOps usage. User management should be coordinated with HR processes to ensure that access is granted when users join and revoked when they leave. Understanding how to add users helps administrators manage access effectively.

Azure DevOps provides tools for adding users, managing their access, and tracking their activity. User management should be performed by administrators who understand organizational policies and security requirements. Understanding user management helps organizations maintain appropriate access control.

### Azure Active Directory (Azure AD) Integration

Azure AD integration connects Azure DevOps organizations to Azure AD tenants, providing enterprise-grade identity management. Integration enables single sign-on, centralized user management, conditional access policies, and better security features. When integrated, Azure AD becomes the source of truth for user identities, and user management happens in Azure AD rather than individually in Azure DevOps.

Azure AD integration provides several benefits including simplified user management, enhanced security through conditional access, better auditability, and integration with other Microsoft services. It enables organizations to enforce security policies consistently across services and provides centralized control over user access. Integration requires Azure AD tenant configuration and Azure DevOps organization configuration.

Understanding Azure AD integration helps organizations leverage enterprise identity management capabilities. Integration is recommended for organizations that want centralized user management, enhanced security features, and better integration with Microsoft's ecosystem. Understanding how to configure and use Azure AD integration helps organizations implement effective identity management.

### Group Management

Groups in Azure DevOps are collections of users that can be granted permissions collectively, making permission management more efficient. Azure DevOps provides built-in groups with common permission sets, and organizations can create custom groups for specific needs. Groups can be nested, allowing for hierarchical organization. Group membership can be managed manually or synchronized with Azure AD groups.

Effective group management involves creating groups that reflect organizational structure or roles, assigning appropriate permissions to groups, and managing group membership. Groups should be organized logically, and membership should be kept current. Group-based permission management is more efficient than individual user management and helps ensure consistent permissions across team members.

Understanding group management helps organizations implement efficient permission management. Groups should be used instead of individual user permissions whenever possible, as they're easier to manage and maintain. Understanding how to create, configure, and manage groups helps organizations implement effective access control.

### License Management

Azure DevOps uses a licensing model where different access levels provide different features. Access levels include Stakeholder (free, limited features), Basic (free for up to 5 users, then paid), Basic + Test Plans (paid, includes test management features), and Visual Studio (included with Visual Studio subscriptions). License management involves assigning appropriate access levels to users based on their needs and ensuring compliance with licensing terms.

License management should balance user needs with cost, assigning the minimum access level that provides necessary features. Organizations should understand which features require which access levels and assign licenses accordingly. License usage should be monitored to ensure compliance and optimize costs.

Understanding license management helps organizations optimize costs while ensuring users have appropriate access. License assignments should be reviewed regularly to ensure they remain appropriate as needs change. Understanding licensing helps organizations make informed decisions about access level assignments.

### Access Levels (Basic, Basic + Test Plans, Stakeholder, Visual Studio)

Access levels in Azure DevOps determine which features users can access. Stakeholder access is free and provides limited features, suitable for users who need to view work items and provide feedback but don't need full development features. Basic access is free for up to 5 users per organization and provides full access to most Azure DevOps features except test management.

Basic + Test Plans includes all Basic features plus Azure Test Plans, enabling test case management and execution. Visual Studio access is included with Visual Studio subscriptions and provides all features. Access levels should be assigned based on user needs, with most developers needing Basic or Visual Studio access, testers needing Basic + Test Plans, and stakeholders needing Stakeholder access.

Understanding access levels helps organizations assign appropriate access and optimize costs. Access level requirements should be considered when planning Azure DevOps adoption and budgeting. Understanding access levels helps organizations ensure users have the features they need while managing costs effectively.

---

## 7.3 Permissions and Access Control

### Security Groups

Security groups in Azure DevOps are collections of users that share permissions, enabling efficient permission management. Azure DevOps provides built-in security groups like Project Administrators, Contributors, and Readers, each with predefined permission sets. Organizations can also create custom security groups for specific needs. Groups can be nested, and permissions can be granted to groups at various levels.

Security groups should be used instead of individual user permissions whenever possible, as they're easier to manage and maintain. Groups should reflect organizational structure or roles, and membership should be kept current. Understanding security groups helps organizations implement efficient, maintainable permission management.

Azure DevOps security groups work with the permission system to control access. Understanding how to create, configure, and manage security groups helps organizations implement effective access control that balances security with usability.

### Permission Inheritance

Permission inheritance in Azure DevOps means that permissions granted at higher levels (like organization or project) apply to lower levels (like repositories or builds) unless overridden. This inheritance simplifies permission management by allowing administrators to set permissions at appropriate levels and have them apply broadly. Inheritance can be overridden at lower levels when more specific control is needed.

Understanding permission inheritance helps administrators configure permissions efficiently. Permissions should be granted at the highest appropriate level, with overrides only where necessary. This approach simplifies management and ensures consistency. Understanding inheritance helps organizations implement effective permission strategies.

### Project-Level Permissions

Project-level permissions control access to projects and their resources. They apply to all resources within a project unless overridden by more specific permissions. Project permissions include the ability to view, contribute to, or administer projects. Project administrators have full control over project settings, permissions, and resources.

Project-level permissions should be configured to provide appropriate access to project resources while maintaining security. They should follow the principle of least privilege, granting users only the permissions they need. Understanding project-level permissions helps administrators configure access control effectively at the project level.

### Area Path Permissions

Area paths organize work items within projects, and permissions can be set on area paths to control access to work items in those areas. This enables teams to restrict access to sensitive work items or to organize permissions by feature area or team. Area path permissions work with project permissions, providing additional granularity where needed.

Area path permissions are useful for scenarios where different teams work on different areas and access should be restricted. They enable fine-grained control over work item access while maintaining project-level permissions for other resources. Understanding area path permissions helps organizations implement appropriate access control for work items.

### Iteration Path Permissions

Iteration paths define time periods (sprints or iterations) and can have permissions that control who can view or modify work items in those iterations. This is less commonly used than area path permissions but can be useful for controlling access to work items based on time periods. Iteration path permissions work similarly to area path permissions, providing additional control where needed.

Understanding iteration path permissions helps organizations implement time-based access control when needed. These permissions are typically less important than area path permissions but can be useful in specific scenarios.

### Repository Permissions

Repository permissions control access to source code repositories, including the ability to read, write, or administer repositories. Permissions can be set at the project level (applying to all repositories) or at the repository level (applying to specific repositories). Repository permissions work with branch policies to provide comprehensive source control security.

Repository permissions should follow the principle of least privilege, with most users having read access and only those who need to contribute having write access. Repository administrators should be limited to those who need to manage repository settings. Understanding repository permissions helps organizations protect source code while enabling appropriate access.

### Build and Release Permissions

Build and release permissions control access to pipelines, builds, and releases. They include the ability to view, queue, edit, or delete pipelines and the ability to approve or manage releases. These permissions are important for controlling who can trigger builds, modify pipeline configurations, and approve deployments.

Build and release permissions should be configured to ensure that only authorized users can modify pipelines or approve deployments, especially for production environments. Understanding build and release permissions helps organizations protect their CI/CD processes while enabling appropriate access for development and operations teams.

---

## 7.4 Service Connections and Service Principals

### Creating Service Connections

Service connections in Azure DevOps provide secure, reusable connections to external services that pipelines and other features need to access. They store authentication information securely, allowing pipelines to interact with services like Azure subscriptions, Docker registries, or other services without exposing credentials in pipeline code. Service connections are configured at the project or organization level and can be shared across multiple pipelines.

Creating service connections involves specifying the service type, providing authentication information, and configuring connection settings. Azure DevOps supports many service types out of the box, and custom service connections can be created for other services. Service connections should be created with appropriate permissions, following the principle of least privilege.

Understanding how to create service connections helps teams configure secure access to external services. Service connections should be created by administrators who understand the services being connected and the security implications. Understanding service connections helps teams use external services securely in their pipelines.

### Service Principal Authentication

Service principals are Azure AD identities used for applications and services to authenticate and access Azure resources. They're used by Azure DevOps service connections to authenticate with Azure services. Service principals provide a way to grant applications access to Azure resources without using user credentials, improving security and enabling automation.

Service principal authentication involves creating a service principal in Azure AD, granting it appropriate permissions, and configuring Azure DevOps to use it for authentication. Service principals should be granted only the permissions they need, following the principle of least privilege. They should be regularly reviewed and rotated as needed.

Understanding service principal authentication helps teams configure secure access to Azure resources. Service principals are essential for automation scenarios where applications need to access Azure resources. Understanding how to create and manage service principals helps teams implement secure automation.

### Managed Identities

Managed identities are Azure AD identities that are automatically managed by Azure and don't require credential management. They provide a more secure alternative to service principals for Azure resources, as credentials are managed automatically and don't need to be stored or rotated. Managed identities can be system-assigned (tied to a specific resource) or user-assigned (standalone and can be assigned to multiple resources).

Azure DevOps can use managed identities for service connections to Azure services, eliminating the need to manage service principal credentials. This improves security by removing the need to store and rotate credentials. Managed identities should be used when possible as they provide better security and simpler management.

Understanding managed identities helps teams implement more secure authentication for Azure resources. Managed identities are recommended for new implementations and should be considered when updating existing service connections. Understanding managed identities helps teams improve security and simplify credential management.

### Secure Connection Management

Secure connection management involves creating, configuring, and maintaining service connections securely. Connections should be created with appropriate permissions, credentials should be stored securely, and connections should be reviewed regularly. Connections should use the most secure authentication method available, such as managed identities when possible.

Connection management should include regular reviews to ensure that connections are still needed, have appropriate permissions, and use secure authentication methods. Unused connections should be removed, and connections with excessive permissions should be updated. Understanding secure connection management helps teams maintain secure access to external services.

### Connection Security

Connection security involves ensuring that service connections are configured securely and that credentials are protected. Connections should use secure authentication methods, credentials should never be exposed in logs or code, and connections should be regularly reviewed and updated. Connection security is essential for protecting access to external services and preventing unauthorized access.

Connection security should be part of overall security practices, with connections created and managed following security best practices. Understanding connection security helps teams configure and maintain secure service connections that protect access to external services.

---

## 7.5 Secrets Management

### Variable Groups and Secrets

Variable groups in Azure Pipelines can store variables including secrets that are encrypted and protected. Secrets in variable groups are never displayed in logs or UI, and they're encrypted at rest and in transit. Variable groups can be shared across multiple pipelines, providing a centralized way to manage secrets and configuration.

Secrets should be stored in variable groups rather than hard-coded in pipeline code or configuration files. Variable groups should be secured with appropriate permissions, and access should be limited to those who need it. Understanding variable groups and secrets helps teams manage sensitive information securely in pipelines.

### Azure Key Vault Integration

Azure Key Vault is a cloud service for storing and managing secrets, keys, and certificates. Azure DevOps can integrate with Azure Key Vault to retrieve secrets during pipeline execution, providing a more secure and centralized secret management solution. Key Vault integration enables organizations to manage secrets in a dedicated service with advanced features like versioning, access policies, and audit logging.

Key Vault integration involves creating a service connection to Azure Key Vault and configuring pipelines to retrieve secrets from the vault. This approach provides better security, centralized management, and integration with other Azure services. Key Vault should be used for sensitive secrets that need enhanced security and management features.

Understanding Azure Key Vault integration helps teams implement enterprise-grade secret management. Key Vault provides better security and management features than variable groups for highly sensitive secrets. Understanding Key Vault integration helps organizations implement appropriate secret management based on their security requirements.

### Secure Files

Secure files in Azure DevOps are encrypted files that can be used in pipelines, such as certificates, configuration files, or scripts containing secrets. Secure files are stored encrypted and can only be accessed by pipelines that have permission. They're downloaded to agents during pipeline execution and can be used by pipeline tasks.

Secure files should be used for files that contain sensitive information and need to be available in pipelines. They should be managed with appropriate permissions, and access should be limited. Understanding secure files helps teams manage sensitive files securely in pipelines.

### Pipeline Secrets

Pipeline secrets are sensitive values used in pipelines that are encrypted and protected. They can be defined as pipeline variables marked as secret, stored in variable groups, or retrieved from Azure Key Vault. Pipeline secrets are never displayed in logs or UI, and they're encrypted at rest and in transit.

Pipeline secrets should be used for all sensitive values like passwords, API keys, or connection strings. They should never be hard-coded in pipeline code or committed to source control. Understanding pipeline secrets helps teams protect sensitive information in CI/CD processes.

### Secret Scanning

Secret scanning involves automatically detecting secrets that might be exposed in code, configuration files, or other artifacts. Azure DevOps can integrate with secret scanning tools to detect and alert on potential secret exposure. Secret scanning helps prevent accidental secret exposure and identifies security risks.

Secret scanning should be part of security practices, with scans run as part of CI/CD processes. Detected secrets should be investigated and remediated promptly. Understanding secret scanning helps teams prevent secret exposure and maintain security.

---

## Quick Reference

### Security Features
- **Azure AD Integration**: Enterprise authentication
- **Multi-Factor Authentication**: Additional security
- **Personal Access Tokens**: Programmatic access
- **Branch Policies**: Code protection

### Permission Levels
- **Stakeholder**: Limited access
- **Basic**: Standard user
- **Basic + Test Plans**: Testing features
- **Visual Studio**: Full access

---

## Common Pitfalls

### Pitfall 1: Not Using Azure AD
**Problem**: Weak authentication, management overhead
**Solution**: Integrate with Azure AD
**Prevention**: Set up Azure AD from the start

### Pitfall 2: Over-Privileged Users
**Problem**: Security risks, accidental changes
**Solution**: Follow least privilege principle
**Prevention**: Grant minimum necessary permissions

### Pitfall 3: Not Rotating Secrets
**Problem**: Compromised credentials, security risks
**Solution**: Regular secret rotation
**Prevention**: Set expiration dates for PATs

---

## Best Practices

1. **Use Azure AD**: Enterprise authentication
2. **Enable MFA**: Multi-factor authentication
3. **Follow Least Privilege**: Minimum permissions
4. **Rotate Secrets**: Regular credential updates
5. **Use Branch Policies**: Protect important branches
6. **Review Permissions**: Regular access reviews
7. **Secure Service Connections**: Proper authentication
8. **Scan for Secrets**: Automated secret detection
9. **Monitor Access**: Track user activity
10. **Document Security**: Clear security policies

---

## Further Reading

### Official Documentation
- [Security and Permissions](https://docs.microsoft.com/azure/devops/organizations/security/)
- [Azure AD Integration](https://docs.microsoft.com/azure/devops/organizations/accounts/connect-organization-to-azure-ad)
- [Permissions](https://docs.microsoft.com/azure/devops/organizations/security/permissions)

### Related Topics
- Introduction to Azure DevOps (Module 1)
- Azure Repos (Module 2)
- Best Practices and Patterns (Module 10)

---

*This module covers security and permissions in Azure DevOps in detail. Understanding security is essential for protecting code, data, and resources, and Azure DevOps provides comprehensive security features that support enterprise security requirements.*

