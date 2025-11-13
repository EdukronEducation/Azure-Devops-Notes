# Module 5: Jenkins Pipeline (Scripted)

## 5.1 Scripted Pipeline Basics

### Scripted Pipeline Syntax

Scripted Pipeline uses Groovy syntax to define pipelines, providing maximum flexibility and programmatic control. Unlike Declarative Pipeline's structured syntax, Scripted Pipeline is essentially Groovy code that uses Jenkins Pipeline APIs. Scripted Pipeline syntax starts with `node` blocks that define where code executes, and uses Groovy control structures for flow control.

Scripted Pipeline basic structure:
```groovy
node {
    stage('Build') {
        sh 'mvn clean package'
    }
    stage('Test') {
        sh 'mvn test'
    }
}
```

Scripted Pipeline provides complete programmatic control, allowing you to use Groovy features like loops, conditionals, functions, and classes. This flexibility makes Scripted Pipeline powerful but also more complex than Declarative Pipeline. Scripted Pipeline is useful when you need functionality that can't be expressed in Declarative syntax, but for most use cases, Declarative Pipeline is recommended.

### Groovy Fundamentals

Groovy is a dynamic language for the Java Virtual Machine that provides a more concise syntax than Java. Understanding Groovy fundamentals is essential for writing Scripted Pipelines. Key Groovy concepts include: variables (def keyword, dynamic typing), strings (single quotes for literals, double quotes for interpolation), collections (lists and maps with simple syntax), closures (anonymous functions), and operators (Groovy-specific operators).

Groovy syntax examples:
```groovy
def name = "Jenkins"  // Variable
def list = [1, 2, 3]  // List
def map = [key: 'value']  // Map
def closure = { param -> println param }  // Closure
```

Groovy's concise syntax and powerful features make it well-suited for pipeline definitions. However, Groovy knowledge is required for Scripted Pipelines, which increases the learning curve. Understanding Groovy fundamentals helps you write effective Scripted Pipelines.

### Node Blocks

Node blocks in Scripted Pipeline specify where pipeline code executes. The `node` block allocates an executor on an agent and provides a workspace. Node blocks can specify agent labels, allowing you to control where code runs. Multiple node blocks can be used in a pipeline, enabling different stages to run on different agents.

Node block examples:
```groovy
node {
    // Runs on any available agent
    sh 'mvn clean package'
}

node('linux') {
    // Runs on agent with 'linux' label
    sh 'mvn test'
}

node('docker') {
    docker.image('maven:3.8.6').inside {
        sh 'mvn clean package'
    }
}
```

Node blocks are the foundation of Scripted Pipeline execution. Understanding node blocks helps you control where pipeline code runs and manage agent resources effectively.

### Stage Blocks

Stage blocks in Scripted Pipeline organize work into logical phases, similar to Declarative Pipeline stages. However, in Scripted Pipeline, stages are optional and are primarily for visualization. Stage blocks use the `stage` function to define stages:

```groovy
node {
    stage('Build') {
        sh 'mvn clean package'
    }
    stage('Test') {
        sh 'mvn test'
    }
    stage('Deploy') {
        sh 'deploy.sh'
    }
}
```

Stages in Scripted Pipeline provide organization and visualization but don't enforce structure like Declarative Pipeline stages. You can use Groovy control structures within and between stages, providing flexibility but requiring discipline to maintain organization. Understanding stage blocks helps you organize Scripted Pipelines effectively.

### Scripted vs. Declarative

Scripted and Declarative Pipelines serve different needs. Declarative Pipeline is recommended for most use cases because it's simpler, has better error messages, and includes built-in best practices. Scripted Pipeline is useful when you need functionality that can't be expressed in Declarative syntax, such as complex loops, dynamic stage generation, or advanced Groovy features.

When to use Scripted Pipeline: when you need dynamic stage generation based on data, when you need complex conditional logic that's difficult in Declarative, when you need to use advanced Groovy features, or when you're already comfortable with Groovy. For most scenarios, Declarative Pipeline is the better choice.

Understanding when to use Scripted vs. Declarative helps you choose the right approach for your needs. Many teams use Declarative Pipeline for standard workflows and Scripted Pipeline (or shared libraries) for complex, reusable logic.

---

## 5.2 Groovy in Jenkins

### Groovy Syntax

Groovy syntax in Jenkins Pipelines follows standard Groovy with some Jenkins-specific additions. Key syntax elements include: variable declarations (def keyword or direct assignment), string interpolation (using ${} in double-quoted strings), method calls (parentheses optional for single-argument methods), and collection literals (simple syntax for lists and maps).

Groovy syntax examples in pipelines:
```groovy
def version = "1.0.0"
def message = "Building version ${version}"
def tools = ['maven', 'gradle', 'npm']
def config = [env: 'prod', version: version]

echo message
sh "mvn clean package -Dversion=${version}"
```

Groovy's flexible syntax makes pipeline code more concise than it would be in Java, but it also requires understanding Groovy conventions. Understanding Groovy syntax helps you write effective Scripted Pipelines.

### Variables and Data Types

Variables in Groovy are dynamically typed, meaning you don't need to declare types explicitly. The `def` keyword declares variables, but it's optional in many contexts. Groovy supports common data types: strings, numbers, booleans, lists, maps, and objects. Variables can be reassigned to different types, providing flexibility but requiring care.

Variable examples:
```groovy
def name = "Jenkins"  // String
def count = 10  // Number
def active = true  // Boolean
def items = [1, 2, 3]  // List
def config = [key: 'value']  // Map
```

Variables in Scripted Pipelines are available within their scope (node block, stage block, or script level). Understanding variables and data types helps you manage data in pipelines effectively.

### Control Structures

Control structures in Groovy include: conditionals (if/else), loops (for, while), switches, and exception handling (try/catch). These structures work similarly to Java but with Groovy's more concise syntax. Control structures are essential for implementing logic in Scripted Pipelines.

Control structure examples:
```groovy
if (env.BRANCH_NAME == 'main') {
    sh 'deploy.sh production'
} else {
    sh 'deploy.sh staging'
}

for (int i = 0; i < 10; i++) {
    echo "Iteration ${i}"
}

try {
    sh 'risky-command.sh'
} catch (Exception e) {
    echo "Error: ${e.message}"
}
```

Control structures enable complex logic in Scripted Pipelines. Understanding control structures helps you implement conditional execution, loops, and error handling.

### Functions and Closures

Functions and closures in Groovy provide reusable code blocks. Functions are defined using the `def` keyword and can be called from anywhere in scope. Closures are anonymous functions that can be passed as parameters. Both are useful for organizing and reusing pipeline code.

Function and closure examples:
```groovy
def buildApp() {
    sh 'mvn clean package'
}

def deploy(env) {
    sh "deploy.sh ${env}"
}

def stages = ['Build', 'Test', 'Deploy']
stages.each { stageName ->
    stage(stageName) {
        echo "Running ${stageName}"
    }
}
```

Functions and closures help organize Scripted Pipeline code and reduce duplication. Understanding functions and closures helps you write maintainable Scripted Pipelines.

### Groovy Best Practices

Groovy best practices for Jenkins Pipelines include: using meaningful variable names, organizing code into functions, handling errors appropriately, avoiding overly complex expressions, and documenting complex logic. Following best practices makes Scripted Pipelines more maintainable and easier to understand.

Best practices also include: using Declarative Pipeline when possible (simpler and more maintainable), using shared libraries for reusable logic (rather than duplicating code), testing pipeline code, and keeping Scripted Pipeline code simple (complex logic should be in shared libraries or external scripts).

Understanding Groovy best practices helps you write effective Scripted Pipelines that are maintainable and reliable.

---

## 5.3 Advanced Scripted Pipelines

### Complex Workflows

Complex workflows in Scripted Pipeline can include: dynamic stage generation, parallel execution with complex dependencies, loops with stages, conditional stage execution, and integration with external systems. Scripted Pipeline's flexibility enables workflows that would be difficult or impossible in Declarative Pipeline.

Complex workflow example:
```groovy
node {
    def environments = ['dev', 'test', 'prod']
    def stages = [:]
    
    environments.each { env ->
        stages[env] = {
            stage("Deploy to ${env}") {
                sh "deploy.sh ${env}"
            }
        }
    }
    
    parallel stages
}
```

Complex workflows require careful design and testing. Understanding how to implement complex workflows helps you handle advanced scenarios in Scripted Pipelines.

### Dynamic Stages

Dynamic stages in Scripted Pipeline are stages that are generated programmatically based on data. This is useful when you don't know at pipeline definition time how many stages you need or what they should be. Dynamic stages are created using Groovy loops and closures.

Dynamic stage example:
```groovy
node {
    def components = ['api', 'web', 'mobile']
    
    components.each { component ->
        stage("Build ${component}") {
            sh "./build.sh ${component}"
        }
    }
}
```

Dynamic stages provide flexibility but can make pipelines harder to understand. They should be used when necessary and documented well. Understanding dynamic stages helps you implement flexible pipelines.

### Error Handling

Error handling in Scripted Pipeline uses Groovy's try/catch/finally blocks. Proper error handling ensures that pipelines fail gracefully and provide useful error messages. Error handling can include: catching specific exceptions, logging errors, cleaning up resources, and notifying stakeholders.

Error handling example:
```groovy
node {
    try {
        stage('Build') {
            sh 'mvn clean package'
        }
    } catch (Exception e) {
        echo "Build failed: ${e.message}"
        currentBuild.result = 'FAILURE'
        throw e
    } finally {
        // Cleanup
        sh 'rm -rf target'
    }
}
```

Error handling is essential for reliable pipelines. Understanding error handling helps you create pipelines that fail gracefully and provide useful feedback.

### Shared Libraries

Shared libraries (covered in Module 6) can be used in Scripted Pipelines to provide reusable functions and steps. Shared libraries help organize complex Scripted Pipeline code and enable code reuse across multiple pipelines. Libraries can provide functions, custom steps, and pipeline templates.

Using shared libraries in Scripted Pipeline:
```groovy
@Library('my-shared-library') _

node {
    myLibrary.build()
    myLibrary.deploy('production')
}
```

Shared libraries complement Scripted Pipelines by providing reusable logic. Understanding shared libraries helps you organize and reuse Scripted Pipeline code effectively.

### Scripted Pipeline Patterns

Common Scripted Pipeline patterns include: the builder pattern (constructing complex objects step by step), the template method pattern (defining skeleton algorithms), the strategy pattern (selecting algorithms at runtime), and the factory pattern (creating objects). These patterns help organize complex Scripted Pipeline code.

Pattern examples help solve common problems in Scripted Pipelines. Understanding patterns helps you write effective, maintainable Scripted Pipelines. However, for most use cases, Declarative Pipeline with shared libraries is simpler and more maintainable than complex Scripted Pipeline patterns.

---

## Quick Reference

### Scripted Pipeline Structure
```groovy
node {
    stage('Build') {
        sh 'make'
    }
    stage('Test') {
        sh 'make test'
    }
}
```

### Key Differences from Declarative
- More flexible, less structured
- Full Groovy language support
- Requires Groovy knowledge
- More complex error handling

---

## Common Pitfalls

### Pitfall 1: Overusing Scripted Pipeline
**Problem**: Unnecessary complexity, harder to maintain
**Solution**: Use Declarative when possible
**Prevention**: Prefer Declarative, use Scripted only when needed

### Pitfall 2: Not Handling Errors
**Problem**: Unclear failures, no cleanup
**Solution**: Use try-catch blocks
**Prevention**: Always handle errors

### Pitfall 3: Complex Groovy Code
**Problem**: Hard to read and maintain
**Solution**: Use shared libraries for complex logic
**Prevention**: Keep pipeline code simple

---

## Best Practices

1. **Use When Needed**: Only for advanced scenarios
2. **Handle Errors**: Try-catch blocks
3. **Use Shared Libraries**: Reuse complex logic
4. **Document Code**: Clear comments
5. **Test Thoroughly**: Validate before production
6. **Keep Simple**: Avoid unnecessary complexity
7. **Version Control**: Store in Git
8. **Follow Patterns**: Use established patterns
9. **Review Code**: Code review for pipelines
10. **Consider Declarative**: Evaluate if Declarative works

---

## Further Reading

### Official Documentation
- [Scripted Pipeline](https://www.jenkins.io/doc/book/pipeline/syntax/#scripted-pipeline)
- [Groovy Syntax](https://groovy-lang.org/syntax.html)
- [Pipeline Examples](https://www.jenkins.io/doc/pipeline/examples/)

### Related Topics
- Pipeline Declarative (Module 4)
- Shared Libraries (Module 6)
- CI/CD Patterns (Module 16)

---

*This module covers Scripted Pipeline in detail. Scripted Pipeline provides maximum flexibility but requires Groovy knowledge and is more complex than Declarative Pipeline. Understanding Scripted Pipeline helps you handle advanced scenarios that can't be expressed in Declarative syntax.*

