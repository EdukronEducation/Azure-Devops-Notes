# Module 2: Terraform Configuration Language (HCL)

## 2.1 HCL Syntax Basics

### HCL Syntax Fundamentals

HashiCorp Configuration Language (HCL) is Terraform's configuration language. HCL is designed to be both human-readable and machine-friendly. HCL syntax includes: **blocks** (containers for configuration), **arguments** (key-value pairs within blocks), **expressions** (values computed from other values), **comments** (documentation), and **identifiers** (names for resources, variables, etc.).

HCL is declarative - you describe what you want, not how to achieve it. HCL supports multiple data types, functions, and expressions that enable complex configurations. Understanding HCL syntax is fundamental to writing Terraform configurations.

Basic HCL example:
```hcl
# This is a comment
resource "aws_instance" "web" {
  ami           = "ami-0c55b159cbfafe1f0"  # Argument
  instance_type = "t2.micro"              # Argument
  
  tags = {                                 # Map argument
    Name = "Web Server"
    Environment = "Production"
  }
}
```

### Blocks, Arguments, and Expressions

**Blocks** are containers for configuration. Blocks have a type (like `resource`, `variable`, `provider`) and labels (identifiers). Blocks contain arguments and can be nested. **Arguments** are key-value pairs that assign values to attributes. **Expressions** compute values from other values, functions, or references.

Block structure:
```hcl
block_type "label1" "label2" {
  argument1 = "value1"
  argument2 = 42
  argument3 = var.example
}
```

Expression examples:
```hcl
# String interpolation
name = "server-${var.environment}"

# Function call
bucket_name = lower(var.bucket_name)

# Reference
instance_id = aws_instance.web.id

# Arithmetic
count = var.instance_count * 2
```

Understanding blocks, arguments, and expressions helps you write correct HCL.

### Comments and Formatting

HCL supports **single-line comments** (`# comment`) and **multi-line comments** (`/* comment */`). Comments are useful for documenting configuration. **Formatting** is important for readability - use `terraform fmt` to automatically format files.

Comment examples:
```hcl
# Single-line comment
resource "aws_instance" "web" {
  # Inline comment
  ami = "ami-0c55b159cbfafe1f0"
}

/*
Multi-line comment
Can span multiple lines
*/
```

Formatting:
```bash
# Format all .tf files
terraform fmt

# Format specific directory
terraform fmt ./modules/web
```

### Data Types (Strings, Numbers, Booleans, Lists, Maps)

HCL supports several data types: **strings** (text values, can use single or double quotes), **numbers** (integers and floats), **booleans** (`true` or `false`), **lists** (ordered collections, `[item1, item2]`), **maps** (key-value pairs, `{key = "value"}`), and **null** (absence of value).

Data type examples:
```hcl
# String
name = "my-instance"
description = 'Instance description'

# Number
count = 3
price = 19.99

# Boolean
enabled = true
public = false

# List
regions = ["us-east-1", "us-west-2", "eu-west-1"]
ports = [80, 443, 8080]

# Map
tags = {
  Name = "Web Server"
  Environment = "Production"
  Owner = "DevOps Team"
}

# Null
optional_value = null
```

Understanding data types helps you use appropriate values in configurations.

### Variables and Outputs

**Variables** are input values that can be provided when running Terraform. Variables make configurations reusable and parameterizable. **Outputs** are values that Terraform displays after applying configuration. Outputs can be used by other configurations or displayed to users.

Variable example:
```hcl
variable "instance_type" {
  description = "EC2 instance type"
  type        = string
  default     = "t2.micro"
}
```

Output example:
```hcl
output "instance_id" {
  description = "ID of the EC2 instance"
  value       = aws_instance.web.id
}
```

Understanding variables and outputs enables reusable, flexible configurations.

---

## 2.2 Terraform Configuration Files

### main.tf, variables.tf, outputs.tf

Common Terraform file patterns: **main.tf** (primary configuration, resources, providers), **variables.tf** (variable declarations), **outputs.tf** (output definitions). This organization separates concerns and improves readability.

File structure example:
```hcl
# main.tf
provider "aws" {
  region = var.aws_region
}

resource "aws_instance" "web" {
  ami           = var.ami_id
  instance_type = var.instance_type
}

# variables.tf
variable "aws_region" {
  type    = string
  default = "us-west-2"
}

variable "ami_id" {
  type = string
}

variable "instance_type" {
  type    = string
  default = "t2.micro"
}

# outputs.tf
output "instance_id" {
  value = aws_instance.web.id
}

output "instance_public_ip" {
  value = aws_instance.web.public_ip
}
```

### terraform.tfvars Files

`.tfvars` files provide variable values. Multiple `.tfvars` files can exist (e.g., `terraform.tfvars`, `dev.tfvars`, `prod.tfvars`). Terraform automatically loads `terraform.tfvars` and `*.auto.tfvars` files.

terraform.tfvars example:
```hcl
aws_region     = "us-west-2"
ami_id         = "ami-0c55b159cbfafe1f0"
instance_type  = "t3.small"
instance_count = 3
```

Using specific tfvars:
```bash
terraform apply -var-file="prod.tfvars"
```

### .tfvars File Management

Best practices for `.tfvars` files: **don't commit sensitive values** (use environment variables or secret management), **use different files per environment** (`dev.tfvars`, `prod.tfvars`), **document variables** (explain what each variable does), **validate values** (use variable validation), and **use `.tfvars.example`** (template files without sensitive data).

### File Organization Best Practices

File organization best practices: **separate concerns** (resources, variables, outputs in different files), **use consistent naming** (follow conventions), **group related resources** (network resources together), **use modules** (for reusable components), **document structure** (README explaining organization), and **keep files focused** (one concern per file).

Example organization:
```
project/
├── main.tf              # Core resources
├── variables.tf         # Variable declarations
├── outputs.tf           # Output definitions
├── versions.tf          # Provider versions
├── backend.tf           # State backend
├── terraform.tfvars     # Variable values
└── modules/
    ├── network/
    ├── compute/
    └── storage/
```

### Configuration Structure

Configuration structure should be: **logical** (organized by purpose), **scalable** (easy to extend), **maintainable** (easy to understand and modify), **modular** (reusable components), and **documented** (clear purpose). Understanding structure helps you organize large Terraform projects.

---

## 2.3 Variables and Outputs

### Variable Declarations and Types

Variables are declared with type, description, default, and validation. Variable types include: **string**, **number**, **bool**, **list(type)**, **set(type)**, **map(type)**, **object({...})**, **tuple([...])**, and **any**.

Variable declaration examples:
```hcl
variable "instance_name" {
  type        = string
  description = "Name of the instance"
  default     = "web-server"
}

variable "instance_count" {
  type        = number
  description = "Number of instances"
  default     = 1
}

variable "enable_monitoring" {
  type        = bool
  description = "Enable CloudWatch monitoring"
  default     = true
}

variable "tags" {
  type = map(string)
  default = {
    Environment = "dev"
    Project     = "webapp"
  }
}

variable "ports" {
  type        = list(number)
  description = "List of ports to open"
  default     = [80, 443]
}
```

### Variable Defaults and Validation

Variables can have **default values** (used when not provided) and **validation rules** (ensure values meet requirements). Validation helps catch errors early.

Validation example:
```hcl
variable "instance_type" {
  type        = string
  description = "EC2 instance type"
  
  validation {
    condition     = can(regex("^t[23]\\.[a-z]+$", var.instance_type))
    error_message = "Instance type must be t2 or t3 family."
  }
}

variable "instance_count" {
  type        = number
  description = "Number of instances"
  default     = 1
  
  validation {
    condition     = var.instance_count > 0 && var.instance_count <= 10
    error_message = "Instance count must be between 1 and 10."
  }
}
```

### Output Values

Outputs expose values from Terraform configuration. Outputs can be: **simple values** (strings, numbers), **resource attributes** (IDs, IPs), **computed values** (from expressions), and **sensitive** (hidden from display).

Output examples:
```hcl
output "instance_id" {
  description = "ID of the EC2 instance"
  value       = aws_instance.web.id
}

output "instance_public_ip" {
  description = "Public IP address"
  value       = aws_instance.web.public_ip
}

output "instance_private_ip" {
  description = "Private IP address"
  value       = aws_instance.web.private_ip
  sensitive   = true
}

output "connection_string" {
  description = "Database connection string"
  value       = "postgresql://${var.db_user}:${var.db_password}@${aws_db_instance.main.endpoint}/${var.db_name}"
  sensitive   = true
}
```

### Local Values

Local values are computed values used within a configuration. Locals help: **avoid repetition** (compute once, use many times), **organize complex expressions** (simplify configuration), and **improve readability** (meaningful names).

Local values example:
```hcl
locals {
  common_tags = {
    Environment = var.environment
    Project     = var.project_name
    ManagedBy   = "Terraform"
  }
  
  instance_name = "${var.project_name}-${var.environment}-${var.instance_type}"
  
  allowed_cidrs = [
    "10.0.0.0/8",
    "172.16.0.0/12",
    "192.168.0.0/16"
  ]
}

resource "aws_instance" "web" {
  # ...
  tags = local.common_tags
}
```

### Data Sources

Data sources fetch information from providers without creating resources. Data sources are read-only and useful for: **referencing existing resources** (resources created outside Terraform), **getting provider information** (available regions, AMIs), **querying external APIs** (DNS, time), and **reading configuration** (files, HTTP).

Data source examples:
```hcl
# Get current AWS region
data "aws_region" "current" {}

# Get available AMIs
data "aws_ami" "ubuntu" {
  most_recent = true
  owners      = ["099720109477"]
  
  filter {
    name   = "name"
    values = ["ubuntu/images/hvm-ssd/ubuntu-jammy-22.04-amd64-server-*"]
  }
}

# Reference existing VPC
data "aws_vpc" "existing" {
  id = "vpc-12345678"
}

# Use data source
resource "aws_instance" "web" {
  ami           = data.aws_ami.ubuntu.id
  instance_type = "t2.micro"
  subnet_id     = data.aws_vpc.existing.default_subnet_id
}
```

---

## 2.4 Expressions and Functions

### String Functions

String functions manipulate text: **lower()**, **upper()**, **title()**, **trim()**, **substr()**, **replace()**, **split()**, **join()**, **format()**, **regex()**, and more.

String function examples:
```hcl
# Case conversion
name = lower(var.instance_name)        # "MY-INSTANCE" -> "my-instance"
title = title(var.instance_name)       # "my instance" -> "My Instance"

# Trimming
clean = trim(var.input, " ")           # Remove spaces

# Substring
short = substr(var.name, 0, 10)        # First 10 characters

# Replacement
updated = replace(var.text, "old", "new")

# Splitting and joining
parts = split("-", "a-b-c")            # ["a", "b", "c"]
joined = join("-", ["a", "b", "c"])    # "a-b-c"

# Formatting
formatted = format("Hello, %s!", var.name)

# Regular expressions
match = regex("^[a-z]+$", var.input)
```

### Numeric Functions

Numeric functions perform calculations: **abs()**, **ceil()**, **floor()**, **max()**, **min()**, **pow()**, **signum()**, and more.

Numeric function examples:
```hcl
# Absolute value
abs_value = abs(-5)                    # 5

# Rounding
ceiling = ceil(4.3)                    # 5
flooring = floor(4.7)                  # 4

# Min/Max
maximum = max(1, 5, 3)                 # 5
minimum = min(1, 5, 3)                 # 1

# Power
power = pow(2, 3)                      # 8

# Sign
sign = signum(-5)                      # -1
```

### Collection Functions

Collection functions work with lists, maps, and sets: **length()**, **keys()**, **values()**, **element()**, **lookup()**, **contains()**, **merge()**, **flatten()**, **distinct()**, and more.

Collection function examples:
```hcl
# Length
count = length(var.items)              # Number of items

# Keys and values
map_keys = keys(var.tags)              # List of keys
map_values = values(var.tags)           # List of values

# Element access
first = element(var.list, 0)           # First element
last = element(var.list, length(var.list) - 1)

# Lookup
value = lookup(var.map, "key", "default")

# Contains
has_item = contains(var.list, "item")

# Merge
merged = merge(var.map1, var.map2)

# Flatten
flat = flatten([[1, 2], [3, 4]])      # [1, 2, 3, 4]

# Distinct
unique = distinct([1, 2, 2, 3])       # [1, 2, 3]
```

### Type Conversion Functions

Type conversion functions change data types: **tostring()**, **tonumber()**, **tobool()**, **tolist()**, **tomap()**, **toset()**.

Type conversion examples:
```hcl
# To string
str = tostring(42)                     # "42"

# To number
num = tonumber("42")                   # 42

# To bool
bool_val = tobool("true")              # true

# To list
list_val = tolist(var.set)             # Convert set to list

# To map
map_val = tomap(var.object)            # Convert object to map

# To set
set_val = toset(var.list)              # Convert list to set (removes duplicates)
```

### Conditional Expressions

Conditional expressions choose values based on conditions: **ternary operator** (`condition ? true_value : false_value`).

Conditional examples:
```hcl
# Simple conditional
instance_type = var.environment == "prod" ? "t3.large" : "t2.micro"

# Nested conditionals
count = var.enable_feature ? (var.environment == "prod" ? 3 : 1) : 0

# With functions
name = var.use_prefix ? "prod-${var.name}" : var.name

# Complex conditions
port = var.protocol == "https" ? 443 : (var.protocol == "http" ? 80 : 8080)
```

---

## Quick Reference

### HCL Syntax
```hcl
# Block
resource "type" "name" {
  argument = "value"
}

# Variable
variable "name" {
  type = string
}

# Output
output "name" {
  value = resource.type.name.attr
}
```

### Data Types
- `string` - Text
- `number` - Integer/Float
- `bool` - true/false
- `list` - Ordered collection
- `map` - Key-value pairs
- `object` - Structured data

---

## Common Pitfalls

### Pitfall 1: Incorrect Indentation
**Problem**: HCL parsing errors
**Solution**: Use 2 spaces, terraform fmt
**Prevention**: Use editor with HCL support

### Pitfall 2: Wrong Data Types
**Problem**: Type mismatch errors
**Solution**: Understand data types, use type conversion
**Prevention**: Validate variable types

### Pitfall 3: Missing Variable Defaults
**Problem**: Required variables not provided
**Solution**: Provide defaults when appropriate
**Prevention**: Document required vs optional

---

## Best Practices

1. **Format Code**: Use terraform fmt
2. **Use Comments**: Document complex logic
3. **Type Variables**: Specify types explicitly
4. **Validate Inputs**: Use variable validation
5. **Organize Files**: Logical file structure
6. **Use Locals**: For computed values
7. **Document Variables**: Clear descriptions
8. **Test Expressions**: Verify logic works
9. **Use Functions**: Leverage built-in functions
10. **Keep It Simple**: Avoid unnecessary complexity

---

## Further Reading

### Official Documentation
- [HCL Language](https://www.terraform.io/docs/language/syntax/index.html)
- [Variables](https://www.terraform.io/docs/language/values/variables.html)
- [Functions](https://www.terraform.io/docs/language/functions/index.html)

### Related Topics
- Resources (Module 4)
- Functions and Expressions (Module 9)

---

*This module covers Terraform's HCL configuration language in detail. Understanding HCL syntax, data types, variables, outputs, and functions is essential for writing effective Terraform configurations.*

