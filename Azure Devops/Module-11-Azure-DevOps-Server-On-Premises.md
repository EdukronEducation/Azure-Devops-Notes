# Module 11: Azure DevOps Server (On-Premises)

## 11.1 Azure DevOps Server Overview

### Azure DevOps Server vs. Azure DevOps Services

Azure DevOps Server (formerly Team Foundation Server) is the on-premises version of Azure DevOps that organizations can install and manage on their own infrastructure. It provides similar functionality to Azure DevOps Services but gives organizations complete control over data, infrastructure, and configuration. The choice between Server and Services depends on requirements for data sovereignty, compliance, network connectivity, and organizational preferences.

Azure DevOps Services is the cloud-hosted version that runs on Microsoft's infrastructure, providing automatic updates, high availability, and no infrastructure management. Azure DevOps Server requires organizations to manage servers, databases, and infrastructure, but provides complete control and can operate in air-gapped environments. Server is typically chosen when organizations have strict data residency requirements, need to comply with specific regulations, or want to integrate with on-premises systems that cannot be exposed to the internet.

Both platforms offer similar functionality, but Services receives updates more frequently and automatically, while Server requires manual upgrades. Server provides more control but requires more management. Understanding the differences helps organizations choose the right platform for their needs.

### Installation and Requirements

Installing Azure DevOps Server involves meeting hardware and software requirements, installing prerequisite software, and running the installation wizard. Requirements include Windows Server, SQL Server, and sufficient hardware resources. Installation should be planned carefully, with consideration for scalability, high availability, and integration requirements.

Installation requirements vary based on the number of users and expected workload. Organizations should plan for adequate hardware, network connectivity, and storage. Installation should be tested in a non-production environment first, and installation should follow best practices for security and configuration.

Understanding installation and requirements helps organizations plan and execute successful Azure DevOps Server deployments. Installation should be performed by administrators with appropriate knowledge and should follow Microsoft's guidance.

### Server Architecture

Azure DevOps Server architecture includes application tiers, database tiers, and optional components like build agents and reporting services. Architecture should be designed for performance, scalability, and high availability. Understanding architecture helps organizations design effective deployments.

Server architecture can be simple for small deployments or complex for large, high-availability deployments. Architecture should be designed based on requirements for performance, availability, and scalability. Understanding architecture helps organizations design appropriate deployments.

Azure DevOps Server architecture should be designed to meet organizational requirements. Understanding architecture helps organizations plan effective deployments.

### Upgrade Strategies

Upgrading Azure DevOps Server involves moving to newer versions to gain new features and security updates. Upgrade strategies should minimize downtime and risk, and upgrades should be tested before production deployment. Upgrades should be planned and should include rollback plans.

Upgrade strategies should consider current version, target version, data migration needs, and downtime requirements. Upgrades should be tested in non-production environments and should be performed during maintenance windows. Understanding upgrade strategies helps organizations maintain current Azure DevOps Server versions.

Azure DevOps Server upgrades should be planned and executed carefully. Understanding upgrade strategies helps organizations maintain current versions while minimizing risk.

---

## 11.2 Administration

### Server Administration

Server administration involves managing Azure DevOps Server configuration, users, projects, and resources. Administration includes configuring server settings, managing security, monitoring performance, and maintaining the server. Administration should be performed by trained administrators who understand Azure DevOps Server.

Server administration should follow best practices for security, performance, and maintenance. Administration should be documented and should include regular reviews and updates. Understanding server administration helps organizations maintain effective Azure DevOps Server deployments.

Azure DevOps Server administration requires knowledge of Windows Server, SQL Server, and Azure DevOps Server itself. Understanding administration helps organizations maintain effective deployments.

### Project Collection Administration

Project collection administration involves managing project collections, which are containers for projects in Azure DevOps Server. Administration includes creating and managing collections, configuring collection settings, and managing collection resources. Collection administration should balance isolation with resource sharing.

Project collection administration should be performed by administrators who understand collection architecture and management. Collections should be organized logically and should be sized appropriately. Understanding collection administration helps organizations manage projects effectively.

Azure DevOps Server project collections provide isolation and organization. Understanding collection administration helps organizations manage collections effectively.

### Backup and Restore

Backup and restore are essential for protecting Azure DevOps Server data and ensuring business continuity. Backups should be performed regularly and should be tested to ensure they can be restored. Backup strategies should include both database backups and configuration backups.

Backup and restore procedures should be documented and should be tested regularly. Backups should be stored securely and should be retained appropriately. Understanding backup and restore helps organizations protect their Azure DevOps Server data.

Azure DevOps Server backup and restore should be part of regular maintenance. Understanding backup and restore helps organizations protect data effectively.

### High Availability

High availability involves configuring Azure DevOps Server to minimize downtime and ensure service availability. High availability typically involves multiple servers, load balancing, and database clustering. High availability should be designed based on availability requirements and should be tested.

High availability configurations can be complex and require additional infrastructure. Configurations should be designed based on requirements and should be tested thoroughly. Understanding high availability helps organizations design resilient deployments.

Azure DevOps Server high availability should be designed based on organizational requirements. Understanding high availability helps organizations design resilient deployments.

### Performance Tuning

Performance tuning involves optimizing Azure DevOps Server configuration and infrastructure for better performance. Tuning includes database optimization, server configuration, and infrastructure optimization. Performance tuning should be data-driven and should focus on identified bottlenecks.

Performance tuning should be ongoing and should be based on performance monitoring. Tuning should balance performance with resource usage and cost. Understanding performance tuning helps organizations maintain optimal performance.

Azure DevOps Server performance tuning should be based on monitoring and analysis. Understanding performance tuning helps organizations maintain effective performance.

---

## Quick Reference

### Deployment Options
- **Azure DevOps Services**: Cloud-hosted (SaaS)
- **Azure DevOps Server**: On-premises

### Key Differences
- **Infrastructure**: Managed vs. self-managed
- **Updates**: Automatic vs. manual
- **Scalability**: Automatic vs. manual
- **Data Location**: Cloud vs. on-premises

---

## Common Pitfalls

### Pitfall 1: Not Understanding Requirements
**Problem**: Wrong deployment choice
**Solution**: Evaluate requirements carefully
**Prevention**: Understand cloud vs. on-premises trade-offs

### Pitfall 2: Underestimating Maintenance
**Problem**: High maintenance overhead
**Solution**: Plan for ongoing maintenance
**Prevention**: Understand maintenance requirements

### Pitfall 3: Not Planning for Updates
**Problem**: Outdated software, security risks
**Solution**: Plan regular updates
**Prevention**: Schedule update cycles

---

## Best Practices

1. **Evaluate Requirements**: Cloud vs. on-premises
2. **Plan Infrastructure**: Adequate resources
3. **Plan Maintenance**: Ongoing support
4. **Schedule Updates**: Regular upgrades
5. **Backup Regularly**: Protect data
6. **Monitor Performance**: Track metrics
7. **Secure Properly**: Security configuration
8. **Document Architecture**: Clear documentation
9. **Train Administrators**: Technical expertise
10. **Review Regularly**: Optimize configuration

---

## Further Reading

### Official Documentation
- [Azure DevOps Server](https://docs.microsoft.com/azure/devops/server/)
- [Installation](https://docs.microsoft.com/azure/devops/server/install/get-started)
- [Upgrade](https://docs.microsoft.com/azure/devops/server/upgrade/get-started)

### Related Topics
- Introduction to Azure DevOps (Module 1)
- Security and Permissions (Module 7)
- Best Practices and Patterns (Module 10)

---

*This module covers Azure DevOps Server on-premises deployment. Understanding on-premises deployment helps organizations choose the right deployment model and implement Azure DevOps Server effectively when cloud deployment is not suitable.*

