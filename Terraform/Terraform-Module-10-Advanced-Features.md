# Module 10: Terraform Advanced Features

## 10.1 Dynamic Blocks

### Dynamic Block Syntax

Dynamic blocks create nested blocks dynamically based on collections. Syntax uses `dynamic` keyword followed by block type name and `for_each` or `content` block.

Dynamic block syntax:
```hcl
dynamic "block_type" {
  for_each = collection
  content {
    # Block content
    argument = each.value
  }
}
```

Dynamic block example:
```hcl
variable "security_rules" {
  type = list(object({
    port        = number
    protocol    = string
    cidr_blocks = list(string)
  }))
  default = [
    { port = 80, protocol = "tcp", cidr_blocks = ["0.0.0.0/0"] },
    { port = 443, protocol = "tcp", cidr_blocks = ["0.0.0.0/0"] },
    { port = 22, protocol = "tcp", cidr_blocks = ["10.0.0.0/8"] }
  ]
}

resource "aws_security_group" "web" {
  name = "web-sg"
  
  dynamic "ingress" {
    for_each = var.security_rules
    content {
      from_port   = ingress.value.port
      to_port     = ingress.value.port
      protocol    = ingress.value.protocol
      cidr_blocks = ingress.value.cidr_blocks
    }
  }
}
```

### Dynamic Block Use Cases

Dynamic blocks are useful for: **variable numbers of nested blocks** (security rules, tags, volumes), **reducing repetition** (avoid duplicating similar blocks), **configuration-driven** (blocks from variables/data), **complex resources** (resources with many optional blocks). Understanding use cases helps you apply dynamic blocks effectively.

### Nested Dynamic Blocks

Dynamic blocks can be nested for complex structures. Nested dynamic blocks enable creating hierarchical structures dynamically.

Nested dynamic block example:
```hcl
variable "server_configs" {
  type = map(object({
    volumes = list(object({
      size = number
      type = string
    }))
    networks = list(object({
      subnet = string
      ip     = string
    }))
  }))
}

resource "some_resource" "server" {
  for_each = var.server_configs
  
  dynamic "volume" {
    for_each = each.value.volumes
    content {
      size = volume.value.size
      type = volume.value.type
    }
  }
  
  dynamic "network" {
    for_each = each.value.networks
    content {
      subnet = network.value.subnet
      ip     = network.value.ip
    }
  }
}
```

### Dynamic Block Limitations

Dynamic block limitations: **can't use count** (use for_each instead), **content block required** (must have content), **each.value/each.key** (access iteration values), **nested complexity** (can become hard to read). Understanding limitations helps you use dynamic blocks appropriately.

### Dynamic Block Best Practices

Dynamic block best practices: **use when beneficial** (avoid overuse), **keep readable** (simple structures), **document** (explain complex blocks), **test** (verify behavior), **consider alternatives** (modules for complex cases). Following best practices ensures maintainable configurations.

---

## 10.2 Conditional Resources

### count Parameter

The `count` parameter creates multiple resource instances from a list. Count enables: **multiple instances** (create N instances), **conditional creation** (0 or 1 instance), **indexed access** (access by index). Count is useful for simple cases.

Count examples:
```hcl
# Create multiple instances
resource "aws_instance" "web" {
  count = 3
  
  ami           = "ami-0c55b159cbfafe1f0"
  instance_type = "t2.micro"
  
  tags = {
    Name = "Web Server ${count.index + 1}"
  }
}

# Conditional creation
resource "aws_instance" "monitoring" {
  count = var.enable_monitoring ? 1 : 0
  
  ami           = "ami-0c55b159cbfafe1f0"
  instance_type = "t2.micro"
}

# Access by index
output "first_instance_id" {
  value = aws_instance.web[0].id
}
```

### for_each Parameter

The `for_each` parameter creates resource instances from a map or set. For_each enables: **keyed instances** (each instance has a key), **map-based** (create from map), **set-based** (create from set), **better than count** (no dependency on order). For_each is preferred for most cases.

For_each examples:
```hcl
# From map
resource "aws_instance" "app" {
  for_each = {
    app1 = { instance_type = "t3.small", zone = "us-west-2a" }
    app2 = { instance_type = "t3.medium", zone = "us-west-2b" }
  }
  
  ami           = "ami-0c55b159cbfafe1f0"
  instance_type = each.value.instance_type
  availability_zone = each.value.zone
  
  tags = {
    Name = each.key
  }
}

# From set
resource "aws_s3_bucket" "data" {
  for_each = toset(["bucket1", "bucket2", "bucket3"])
  
  bucket = "${each.value}-${var.environment}"
}

# Access by key
output "app1_id" {
  value = aws_instance.app["app1"].id
}
```

### Conditional Creation

Conditional creation creates resources based on conditions: **count** (0 or 1), **for_each** (empty map/set), **conditional logic** (if/else patterns). Conditional creation enables flexible infrastructure.

Conditional examples:
```hcl
# Using count
resource "aws_instance" "backup" {
  count = var.create_backup ? 1 : 0
  # ...
}

# Using for_each
resource "aws_instance" "optional" {
  for_each = var.enable_feature ? { instance = {} } : {}
  # ...
}

# Complex condition
resource "aws_instance" "conditional" {
  count = var.environment == "prod" && var.enable_monitoring ? 1 : 0
  # ...
}
```

### Conditional Attributes

Conditional attributes set values based on conditions: **ternary operator** (condition ? true : false), **coalesce** (first non-null value), **try** (handle errors). Conditional attributes enable dynamic configuration.

Conditional attribute examples:
```hcl
resource "aws_instance" "web" {
  instance_type = var.environment == "prod" ? "t3.large" : "t2.micro"
  
  ebs_optimized = var.environment == "prod" ? true : false
  
  # Coalesce - first non-null
  ami = coalesce(var.custom_ami, data.aws_ami.ubuntu.id)
  
  # Try - handle errors
  user_data = try(base64encode(file("${path.module}/userdata.sh")), "")
}
```

### Conditional Patterns

Common conditional patterns: **environment-based** (different per environment), **feature flags** (enable/disable), **resource selection** (choose resources), **default values** (provide defaults). Understanding patterns helps you implement conditional logic effectively.

---

## 10.3 Terraform Provisioners

### What are Provisioners?

Provisioners execute scripts on local or remote machines after resource creation. Provisioners enable: **configuration** (configure resources after creation), **initialization** (setup scripts), **data migration** (copy data), **service startup** (start services). However, provisioners are generally discouraged in favor of better alternatives.

Provisioner types: **local-exec** (run on machine running Terraform), **remote-exec** (run on created resource), **file** (copy files to resource). Understanding provisioners helps you know when (rarely) to use them.

### Local Provisioners

Local provisioners run commands on the machine running Terraform. Useful for: **local operations** (not on resource), **external tools** (call external systems), **notifications** (send alerts), **logging** (write logs).

Local-exec example:
```hcl
resource "aws_instance" "web" {
  # ...
  
  provisioner "local-exec" {
    command = "echo 'Instance created: ${self.public_ip}' >> instances.txt"
  }
}
```

### Remote Provisioners

Remote provisioners run commands on created resources. Useful for: **initial configuration** (setup scripts), **software installation** (install packages), **service configuration** (configure services). Remote provisioners require SSH or WinRM access.

Remote-exec example:
```hcl
resource "aws_instance" "web" {
  # ...
  
  connection {
    type        = "ssh"
    user        = "ubuntu"
    private_key = file("~/.ssh/id_rsa")
    host        = self.public_ip
  }
  
  provisioner "remote-exec" {
    inline = [
      "sudo apt-get update",
      "sudo apt-get install -y nginx",
      "sudo systemctl start nginx"
    ]
  }
}
```

### Provisioner Limitations

Provisioner limitations: **not idempotent** (may run multiple times), **error handling** (failures can leave resources), **dependency issues** (timing problems), **not recommended** (better alternatives exist). Understanding limitations helps you avoid provisioners when possible.

### Provisioner Alternatives

Better alternatives to provisioners: **user data/cloud-init** (initialization scripts), **configuration management** (Ansible, Chef, Puppet), **container images** (pre-configured images), **custom images** (AMIs with software), **Terraform providers** (use provider features). Understanding alternatives helps you choose better approaches.

---

## 10.4 Null Resources

### null_resource Use Cases

Null resources are placeholder resources that don't create actual infrastructure. Use cases: **triggers** (run provisioners when dependencies change), **local-exec** (run local commands), **remote-exec** (run remote commands), **orchestration** (coordinate operations).

Null resource example:
```hcl
resource "null_resource" "setup" {
  triggers = {
    instance_id = aws_instance.web.id
  }
  
  provisioner "local-exec" {
    command = "./setup.sh ${aws_instance.web.public_ip}"
  }
}
```

### Triggers

Triggers cause null resources to re-run when values change. Triggers enable: **dependency tracking** (run when dependencies change), **change detection** (detect configuration changes), **orchestration** (coordinate operations). Triggers are the main feature of null resources.

Trigger example:
```hcl
resource "null_resource" "deploy" {
  triggers = {
    app_version = var.app_version
    instance_id = aws_instance.web.id
  }
  
  provisioner "local-exec" {
    command = "deploy.sh ${var.app_version}"
  }
}
```

### Local-exec Provisioner

Local-exec with null resources runs commands locally. Useful for: **external tools** (call APIs), **notifications** (send alerts), **logging** (write logs), **orchestration** (coordinate operations).

### Remote-exec Provisioner

Remote-exec with null resources runs commands on remote machines. Requires connection configuration. Useful for: **post-creation setup** (configure after creation), **software installation** (install packages), **service management** (manage services).

### Null Resource Patterns

Null resource patterns: **deployment triggers** (deploy on changes), **health checks** (verify resources), **cleanup** (cleanup operations), **notifications** (send alerts), **orchestration** (coordinate multiple resources). Understanding patterns helps you use null resources effectively.

---

## Quick Reference

### Dynamic Blocks
```hcl
dynamic "block_type" {
  for_each = collection
  content {
    argument = each.value
  }
}
```

### Conditional Resources
```hcl
resource "type" "name" {
  count = condition ? 1 : 0
  # ...
}
```

### Null Resources
```hcl
resource "null_resource" "name" {
  triggers = {
    key = value
  }
  provisioner "local-exec" {
    command = "script.sh"
  }
}
```

---

## Common Pitfalls

### Pitfall 1: Overusing Provisioners
**Problem**: Fragile, hard to maintain
**Solution**: Use provisioners sparingly, prefer cloud-init
**Prevention**: Consider alternatives first

### Pitfall 2: Complex Dynamic Blocks
**Problem**: Hard to read and debug
**Solution**: Keep dynamic blocks simple, use modules
**Prevention**: Document complex blocks

### Pitfall 3: Null Resource Triggers
**Problem**: Not triggering when expected
**Solution**: Understand trigger behavior
**Prevention**: Test trigger logic

---

## Best Practices

1. **Use Sparingly**: Advanced features when needed
2. **Keep Simple**: Avoid unnecessary complexity
3. **Document**: Explain why advanced features used
4. **Test Thoroughly**: Verify behavior
5. **Consider Alternatives**: Modules, cloud-init
6. **Use Provisioners Carefully**: Last resort
7. **Test Dynamic Blocks**: Verify iteration works
8. **Monitor Performance**: Advanced features can be slow
9. **Review Regularly**: Simplify when possible
10. **Follow Patterns**: Use established patterns

---

## Further Reading

### Official Documentation
- [Dynamic Blocks](https://www.terraform.io/docs/language/expressions/dynamic-blocks.html)
- [Provisioners](https://www.terraform.io/docs/language/resources/provisioners/index.html)
- [Null Resource](https://registry.terraform.io/providers/hashicorp/null/latest/docs/resources/resource)

### Related Topics
- Resources (Module 4)
- Functions and Expressions (Module 9)

---

*This module covers Terraform advanced features including dynamic blocks, conditional resources, provisioners, and null resources. Understanding these features enables you to create more flexible and powerful Terraform configurations.*

