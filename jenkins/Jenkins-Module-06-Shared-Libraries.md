# Module 6: Jenkins Shared Libraries

## 6.1 Shared Library Concepts

### What are Shared Libraries?

Shared libraries in Jenkins are repositories of reusable pipeline code that can be shared across multiple Jenkinsfiles. They enable you to define common functions, steps, and pipeline patterns once and reuse them across projects. Shared libraries are stored in version control (typically Git) and are loaded into Jenkins pipelines using the `@Library` annotation or directive.

Shared libraries provide several key benefits: code reuse (define logic once, use everywhere), consistency (ensuring all pipelines follow the same patterns), maintainability (update logic in one place), and testing (libraries can be tested independently). Shared libraries are essential for organizations with many pipelines, as they reduce duplication and ensure consistency.

Shared libraries consist of three main directories: `vars` (for global variables and custom steps), `src` (for Groovy classes and utilities), and `resources` (for non-Groovy files like scripts or templates). Understanding shared libraries helps you organize and reuse pipeline code effectively.

#### Shared Library Architecture

```mermaid
graph TD
    A[Shared Library Repository] --> B[vars/]
    A --> C[src/]
    A --> D[resources/]
    
    B --> B1[Global Variables]
    B --> B2[Custom Steps]
    
    C --> C1[Groovy Classes]
    C --> C2[Utilities]
    
    D --> D1[Scripts]
    D --> D2[Templates]
    
    E[Jenkins Pipeline] --> F[@Library]
    F --> A
    
    style A fill:#0078d4,color:#fff
    style E fill:#e1f5ff
```

### Library Structure

Shared library structure follows a standard organization: the `vars` directory contains global variables (files that define custom steps), the `src` directory contains Groovy source code (classes and utilities), and the `resources` directory contains non-code resources (scripts, templates, configuration files). This structure provides organization and makes libraries easy to navigate and maintain.

The library structure also includes a `Jenkinsfile` (optional) for testing the library itself, and documentation (README.md) explaining how to use the library. Understanding library structure helps you create well-organized shared libraries that are easy to use and maintain.

### Library Versioning

Shared libraries support versioning, allowing you to specify which version of a library to use in pipelines. Versioning enables you to update libraries without breaking existing pipelines, test new library versions, and roll back to previous versions if needed. Libraries can be versioned using Git tags, branches, or commit hashes.

Versioning examples:
```groovy
@Library('my-library@1.0.0') _  // Specific version
@Library('my-library@main') _   // Branch
@Library('my-library@abc123') _ // Commit hash
```

Library versioning is essential for managing library evolution and ensuring pipeline stability. Understanding library versioning helps you manage library updates and maintain pipeline reliability.

### Library Benefits

Shared libraries provide numerous benefits: code reuse reduces duplication and maintenance effort, consistency ensures all pipelines follow the same patterns and best practices, maintainability allows updates in one place to affect all pipelines, testing enables libraries to be tested independently, and documentation provides a central place to document pipeline patterns.

Libraries also enable: standardization of build and deployment processes, easier onboarding of new team members (who can use documented library functions), better code organization (separating reusable logic from project-specific code), and collaboration (teams can contribute to shared libraries). Understanding library benefits helps you appreciate why shared libraries are valuable.

### Library Use Cases

Common shared library use cases include: build functions (standardized build processes), deployment functions (reusable deployment logic), notification functions (consistent notification patterns), utility functions (common operations like parsing, formatting), and pipeline templates (complete pipeline definitions that can be customized).

Use case examples:
- Build library: Functions for building Java, Node.js, Python applications
- Deploy library: Functions for deploying to various environments
- Test library: Functions for running different types of tests
- Notify library: Functions for sending notifications via various channels

Understanding library use cases helps you identify opportunities to create shared libraries and organize library code effectively.

---

## 6.2 Creating Shared Libraries

### Library Repository Structure

Creating a shared library starts with setting up a Git repository with the proper structure. The repository should have: a `vars` directory for global variables, a `src` directory for Groovy classes, a `resources` directory for non-code files, a `Jenkinsfile` for testing the library, and a `README.md` for documentation.

Repository structure example:
```
my-shared-library/
├── vars/
│   ├── buildApp.groovy
│   └── deployApp.groovy
├── src/
│   └── com/
│       └── company/
│           └── BuildUtils.groovy
├── resources/
│   └── scripts/
│       └── deploy.sh
├── Jenkinsfile
└── README.md
```

Proper repository structure makes libraries easy to navigate, use, and maintain. Understanding repository structure helps you create well-organized shared libraries.

### vars Directory

The `vars` directory contains global variables that define custom steps available in pipelines. Files in `vars` are automatically loaded and their names become step names. For example, a file `vars/buildApp.groovy` creates a step `buildApp()` that can be called in pipelines.

vars file example:
```groovy
// vars/buildApp.groovy
def call(Map config) {
    echo "Building ${config.name} version ${config.version}"
    sh "mvn clean package -Dversion=${config.version}"
    archiveArtifacts artifacts: 'target/*.jar', fingerprint: true
}
```

The `vars` directory is the primary way to create custom steps that can be used in pipelines. Understanding the vars directory helps you create reusable pipeline steps.

### src Directory

The `src` directory contains Groovy source code organized in packages, similar to Java. Classes in `src` can be imported and used in pipeline code or in `vars` files. The `src` directory is useful for complex logic, utilities, and classes that need to be tested independently.

src class example:
```groovy
// src/com/company/BuildUtils.groovy
package com.company

class BuildUtils {
    static def buildJavaApp(String version) {
        // Build logic
    }
    
    static def buildNodeApp(String version) {
        // Build logic
    }
}
```

The `src` directory provides a place for complex, reusable logic that benefits from being organized as classes. Understanding the src directory helps you create well-structured shared libraries.

### resources Directory

The `resources` directory contains non-Groovy files that can be loaded and used in pipelines. Common resources include: shell scripts, configuration files, templates, and other files needed by library functions. Resources are accessed using the `libraryResource` step.

resources usage example:
```groovy
// In a vars file
def call() {
    def script = libraryResource 'scripts/deploy.sh'
    writeFile file: 'deploy.sh', text: script
    sh 'chmod +x deploy.sh && ./deploy.sh'
}
```

The `resources` directory enables libraries to include and use non-code files. Understanding the resources directory helps you create complete shared libraries that include all necessary files.

### Library Documentation

Library documentation explains how to use the library, what functions are available, what parameters they accept, and provides examples. Good documentation is essential for library adoption and reduces the learning curve for users. Documentation typically includes: overview of the library, list of available functions, function parameters and return values, usage examples, and version information.

Documentation should be kept up to date as the library evolves. It can be in the form of a README.md file, inline code comments, or separate documentation. Understanding the importance of documentation helps you create libraries that are easy to use and adopt.

---

## 6.3 Using Shared Libraries

### Library Configuration

Shared libraries must be configured in Jenkins before they can be used. Library configuration is done in "Manage Jenkins" > "Configure System" > "Global Pipeline Libraries". Configuration includes: library name (used in @Library directive), default version (branch or tag), and repository URL and credentials.

Library configuration can also include: retrieval method (modern SCM or legacy SCM), implicit libraries (automatically loaded in all pipelines), and library caching (for performance). Understanding library configuration helps you set up libraries correctly and manage library access.

### Importing Libraries

Libraries are imported into pipelines using the `@Library` annotation (for Scripted Pipeline) or `@Library` directive (for Declarative Pipeline). The library name and optionally a version are specified. Libraries can be imported at the top of a Jenkinsfile or within pipeline code.

Import examples:
```groovy
@Library('my-library') _
// or
@Library('my-library@1.0.0') _

pipeline {
    agent any
    stages {
        stage('Build') {
            steps {
                buildApp name: 'myapp', version: '1.0.0'
            }
        }
    }
}
```

Understanding library import helps you use shared libraries in your pipelines.

### Calling Library Functions

Library functions are called like any other pipeline step. Functions defined in `vars` are available as steps, and classes in `src` can be imported and used. Function calls can include parameters, and functions can return values that are used in pipeline logic.

Function call examples:
```groovy
// Calling a vars function
buildApp name: 'myapp', version: '1.0.0'

// Using a src class
import com.company.BuildUtils
BuildUtils.buildJavaApp('1.0.0')
```

Understanding how to call library functions helps you use shared libraries effectively in your pipelines.

### Library Versioning

Library versioning (covered in 6.1) is used when importing libraries to specify which version to use. Versioning allows you to: use stable versions in production pipelines, test new library versions, and roll back if needed. Understanding library versioning helps you manage library evolution and maintain pipeline stability.

### Library Best Practices

Library best practices include: following naming conventions, documenting functions well, versioning libraries appropriately, testing libraries independently, keeping libraries focused (single responsibility), and maintaining backward compatibility when possible. Following best practices makes libraries more useful and maintainable.

---

## 6.4 Library Patterns

### Common Patterns

Common shared library patterns include: the builder pattern (constructing complex objects), the factory pattern (creating objects), utility functions (common operations), and pipeline templates (complete pipelines). These patterns help organize library code and solve common problems.

Pattern examples help solve recurring problems in pipelines. Understanding common patterns helps you create effective shared libraries.

### Reusable Functions

Reusable functions are the core of shared libraries. Functions should be: well-named (clear purpose), documented (parameters and behavior), tested (verified to work), and focused (single responsibility). Reusable functions reduce duplication and ensure consistency across pipelines.

### Custom Steps

Custom steps (defined in `vars`) extend pipeline functionality with domain-specific operations. Custom steps should follow Jenkins step conventions and integrate well with pipeline syntax. Custom steps make pipelines more readable and domain-specific.

### Pipeline Templates

Pipeline templates are complete pipeline definitions that can be customized with parameters. Templates provide a starting point for new pipelines and ensure consistency. Templates can be defined in shared libraries and customized per project.

### Best Practices

Best practices for shared libraries include all the practices mentioned throughout this module, plus: keeping libraries small and focused, versioning appropriately, testing thoroughly, documenting well, and maintaining backward compatibility. Following best practices ensures libraries are useful and maintainable.

---

## Quick Reference

### Shared Library Structure
```
shared-library/
├── vars/
│   └── myStep.groovy
├── src/
│   └── com/example/
│       └── Utils.groovy
└── resources/
    └── scripts/
```

### Using Shared Libraries
```groovy
@Library('my-library') _
pipeline {
    agent any
    stages {
        stage('Build') {
            steps {
                myStep()
            }
        }
    }
}
```

---

## Common Pitfalls

### Pitfall 1: Not Versioning Libraries
**Problem**: Breaking changes, unpredictable behavior
**Solution**: Use version tags or branches
**Prevention**: Always version shared libraries

### Pitfall 2: Over-Complex Libraries
**Problem**: Hard to use and maintain
**Solution**: Keep libraries focused and simple
**Prevention**: Single responsibility principle

### Pitfall 3: Not Testing Libraries
**Problem**: Bugs in production pipelines
**Solution**: Test libraries thoroughly
**Prevention**: Automated testing for libraries

---

## Best Practices

1. **Version Libraries**: Use Git tags or branches
2. **Document Functions**: Clear documentation
3. **Test Thoroughly**: Unit and integration tests
4. **Keep Focused**: Single responsibility
5. **Follow Conventions**: Jenkins library conventions
6. **Maintain Backward Compatibility**: When possible
7. **Review Changes**: Code review for libraries
8. **Use Meaningful Names**: Clear function names
9. **Handle Errors**: Proper error handling
10. **Share Knowledge**: Document usage patterns

---

## Further Reading

### Official Documentation
- [Shared Libraries](https://www.jenkins.io/doc/book/pipeline/shared-libraries/)
- [Library Structure](https://www.jenkins.io/doc/book/pipeline/shared-libraries/#library-structure)
- [Writing Libraries](https://www.jenkins.io/doc/book/pipeline/shared-libraries/#writing-libraries)

### Related Topics
- Pipeline Declarative (Module 4)
- Pipeline Scripted (Module 5)
- Best Practices (Module 15)

---

*This module covers Jenkins Shared Libraries in detail. Shared libraries are essential for code reuse and consistency across multiple pipelines. Understanding shared libraries helps you organize pipeline code effectively and reduce duplication.*

