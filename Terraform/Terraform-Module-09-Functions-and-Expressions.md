# Module 9: Terraform Functions and Expressions

## 9.1 Built-in Functions

### String Functions

String functions manipulate text values: **lower()**, **upper()**, **title()**, **trim()**, **substr()**, **replace()**, **split()**, **join()**, **format()**, **regex()**, **regexall()**, **startswith()**, **endswith()**, **strrev()**, **chomp()**, and more.

String function examples:
```hcl
# Case conversion
lower_name = lower("MY-INSTANCE")           # "my-instance"
upper_name = upper("my-instance")           # "MY-INSTANCE"
title_name = title("my instance")           # "My Instance"

# Trimming
trimmed = trim("  hello  ", " ")           # "hello"
chomped = chomp("text\n")                   # "text"

# Substring
short = substr("long-string", 0, 4)         # "long"

# Replacement
replaced = replace("hello world", "world", "terraform")  # "hello terraform"

# Splitting and joining
parts = split("-", "a-b-c")                 # ["a", "b", "c"]
joined = join("-", ["a", "b", "c"])        # "a-b-c"

# Formatting
formatted = format("Hello, %s!", "Terraform")  # "Hello, Terraform!"
formatted_num = format("%.2f", 3.14159)        # "3.14"

# Regular expressions
matched = regex("^[a-z]+$", "hello")        # "hello" (if match)
all_matches = regexall("[0-9]+", "a1b2c3")  # ["1", "2", "3"]

# Prefix/suffix
starts = startswith("hello", "he")          # true
ends = endswith("hello", "lo")              # true
```

### Numeric Functions

Numeric functions perform mathematical operations: **abs()**, **ceil()**, **floor()**, **max()**, **min()**, **pow()**, **signum()**, **parseint()**, **parsefloat()**, and more.

Numeric function examples:
```hcl
# Absolute value
abs_value = abs(-5)                         # 5

# Rounding
ceiling = ceil(4.3)                         # 5
flooring = floor(4.7)                      # 4

# Min/Max
maximum = max(1, 5, 3, 9)                  # 9
minimum = min(1, 5, 3, 9)                  # 1

# Power
power = pow(2, 3)                           # 8

# Sign
sign = signum(-5)                           # -1
sign_zero = signum(0)                       # 0
sign_pos = signum(5)                        # 1

# Parsing
parsed_int = parseint("42", 10)             # 42
parsed_float = parsefloat("3.14", 10)      # 3.14
```

### Collection Functions

Collection functions work with lists, maps, and sets: **length()**, **keys()**, **values()**, **element()**, **lookup()**, **contains()**, **merge()**, **flatten()**, **distinct()**, **slice()**, **sort()**, **reverse()**, **zipmap()**, and more.

Collection function examples:
```hcl
# Length
list_length = length([1, 2, 3])             # 3
map_length = length({a=1, b=2})             # 2

# Keys and values
map_keys = keys({a=1, b=2, c=3})           # ["a", "b", "c"]
map_values = values({a=1, b=2, c=3})       # [1, 2, 3]

# Element access
first = element(["a", "b", "c"], 0)         # "a"
last = element(["a", "b", "c"], 2)          # "c"

# Lookup
value = lookup({a=1, b=2}, "a", "default") # 1
not_found = lookup({a=1, b=2}, "c", "default")  # "default"

# Contains
has_item = contains(["a", "b", "c"], "b")   # true

# Merge
merged = merge({a=1}, {b=2}, {c=3})        # {a=1, b=2, c=3}

# Flatten
flat = flatten([[1, 2], [3, 4]])           # [1, 2, 3, 4]

# Distinct
unique = distinct([1, 2, 2, 3, 3, 3])      # [1, 2, 3]

# Slice
slice = slice(["a", "b", "c", "d"], 1, 3)  # ["b", "c"]

# Sort
sorted = sort(["c", "a", "b"])             # ["a", "b", "c"]

# Reverse
reversed = reverse([1, 2, 3])               # [3, 2, 1]

# Zipmap (create map from two lists)
mapped = zipmap(["a", "b"], [1, 2])        # {a=1, b=2}
```

### Type Conversion Functions

Type conversion functions change data types: **tostring()**, **tonumber()**, **tobool()**, **tolist()**, **tomap()**, **toset()**.

Type conversion examples:
```hcl
# To string
str = tostring(42)                          # "42"
str_bool = tostring(true)                   # "true"

# To number
num = tonumber("42")                        # 42
float_str = tonumber("3.14")               # 3.14

# To bool
bool_val = tobool("true")                   # true
bool_false = tobool("false")                # false

# To list
list_val = tolist(toset(["a", "b", "c"]))  # ["a", "b", "c"]

# To map
map_val = tomap({a=1, b=2})                # {a=1, b=2}

# To set
set_val = toset(["a", "b", "a", "c"])      # ["a", "b", "c"] (duplicates removed)
```

### Encoding Functions

Encoding functions handle data encoding: **base64encode()**, **base64decode()**, **base64gzip()**, **base64gunzip()**, **urlencode()**, **urldecode()**, **jsonencode()**, **jsondecode()**, **yamlencode()**, **yamldecode()**, **csvdecode()**.

Encoding function examples:
```hcl
# Base64
encoded = base64encode("hello")             # "aGVsbG8="
decoded = base64decode("aGVsbG8=")          # "hello"

# URL encoding
url_encoded = urlencode("hello world")       # "hello%20world"
url_decoded = urldecode("hello%20world")     # "hello world"

# JSON
json_encoded = jsonencode({a=1, b="two"})   # "{\"a\":1,\"b\":\"two\"}"
json_decoded = jsondecode("{\"a\":1}")      # {a=1}

# YAML
yaml_encoded = yamlencode({a=1, b="two"})   # YAML string
yaml_decoded = yamldecode("a: 1\nb: two")   # {a=1, b="two"}
```

---

## 9.2 Advanced Expressions

### Conditional Expressions

Conditional expressions choose values based on conditions using the ternary operator: `condition ? true_value : false_value`. Conditionals enable dynamic configuration based on variables or other conditions.

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

# Multiple conditions
storage_type = var.environment == "prod" && var.high_performance ? "io1" : "gp3"
```

### For Expressions

For expressions transform collections: **for lists** (`[for item in list : transformation]`), **for maps** (`{for key, value in map : key => transformation}`), **for with conditions** (`[for item in list : transformation if condition]`).

For expression examples:
```hcl
# Transform list
numbers = [1, 2, 3, 4, 5]
squared = [for n in numbers : n * n]        # [1, 4, 9, 16, 25]

# Transform map
original = {a=1, b=2, c=3}
doubled = {for k, v in original : k => v * 2}  # {a=2, b=4, c=6}

# With conditions
even_squared = [for n in numbers : n * n if n % 2 == 0]  # [4, 16]

# Complex transformation
instances = [for i in range(3) : {
  name = "instance-${i}"
  type = i == 0 ? "t3.large" : "t3.micro"
}]
```

### Splat Expressions

Splat expressions simplify accessing attributes from collections of resources: **list splat** (`resource.*.attribute`), **single splat** (`resource[*].attribute`). Splat expressions reduce repetition when working with resource collections.

Splat expression examples:
```hcl
resource "aws_instance" "web" {
  count = 3
  # ...
}

# Splat to get all IDs
all_ids = aws_instance.web[*].id           # List of all IDs

# Splat to get all private IPs
all_ips = aws_instance.web[*].private_ip    # List of all IPs

# With for_each
resource "aws_instance" "app" {
  for_each = toset(["app1", "app2"])
  # ...
}

# Splat with for_each
all_app_ids = [for instance in aws_instance.app : instance.id]
```

### Dynamic Blocks

Dynamic blocks create nested blocks dynamically based on collections. Dynamic blocks enable creating variable numbers of nested blocks without repeating code.

Dynamic block example:
```hcl
variable "security_rules" {
  type = list(object({
    port        = number
    protocol    = string
    cidr_blocks = list(string)
  }))
  default = [
    {
      port        = 80
      protocol    = "tcp"
      cidr_blocks = ["0.0.0.0/0"]
    },
    {
      port        = 443
      protocol    = "tcp"
      cidr_blocks = ["0.0.0.0/0"]
    }
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

### Complex Expressions

Complex expressions combine multiple functions and operations: **nested functions** (functions calling functions), **chained operations** (multiple transformations), **conditional with functions** (functions in conditionals), **for with functions** (functions in for expressions).

Complex expression examples:
```hcl
# Nested functions
name = upper(replace(lower(var.instance_name), " ", "-"))

# Chained operations
tags = merge(
  {Environment = var.environment},
  {for k, v in var.additional_tags : k => v}
)

# Conditional with functions
bucket_name = var.environment == "prod" ? 
  lower("${var.project}-prod") : 
  lower("${var.project}-${var.environment}")

# For with functions
formatted_names = [for name in var.names : format("prefix-%s-suffix", name)]
```

---

## 9.3 Function Patterns

### String Manipulation

String manipulation patterns: **naming** (generate resource names), **formatting** (format strings), **validation** (regex patterns), **transformation** (case conversion, replacements).

String pattern examples:
```hcl
# Generate consistent names
resource_name = "${var.project}-${var.environment}-${var.resource_type}"

# Format with variables
connection_string = format("postgresql://%s:%s@%s/%s", 
  var.db_user, 
  var.db_password, 
  var.db_host, 
  var.db_name)

# Validation
is_valid = can(regex("^[a-z0-9-]+$", var.name))

# Transformation
clean_name = lower(replace(var.name, " ", "-"))
```

### List and Map Operations

List and map operation patterns: **filtering** (select items), **transformation** (map over items), **aggregation** (sum, max, min), **lookup** (find values), **merging** (combine maps).

Collection pattern examples:
```hcl
# Filtering
even_numbers = [for n in var.numbers : n if n % 2 == 0]

# Transformation
doubled = [for n in var.numbers : n * 2]

# Aggregation
max_value = max(var.numbers...)
sum_values = sum(var.numbers)

# Lookup with default
value = lookup(var.map, "key", "default")

# Merging
combined = merge(var.base_tags, var.env_tags, var.resource_tags)
```

### Type Conversions

Type conversion patterns: **string to number** (parse numbers), **list to set** (remove duplicates), **map to list** (extract values), **number to string** (format numbers).

Type conversion examples:
```hcl
# Parse numbers
port = tonumber(var.port_string)

# Remove duplicates
unique_items = toset(var.items_with_duplicates)

# Extract values
values_list = values(var.map)

# Format numbers
formatted = format("%.2f", var.price)
```

### Conditional Logic

Conditional logic patterns: **environment-based** (different values per environment), **feature flags** (enable/disable features), **resource selection** (choose resources), **default values** (provide defaults).

Conditional pattern examples:
```hcl
# Environment-based
instance_type = var.environment == "prod" ? "t3.large" : "t2.micro"

# Feature flags
count = var.enable_monitoring ? 1 : 0

# Resource selection
ami_id = var.use_custom_ami ? var.custom_ami_id : data.aws_ami.ubuntu.id

# Defaults
port = var.port != null ? var.port : 8080
```

### Data Transformation

Data transformation patterns: **flattening** (nested to flat), **nesting** (flat to nested), **grouping** (group by key), **pivoting** (transform structure).

Transformation examples:
```hcl
# Flattening
flat_list = flatten([[1, 2], [3, 4]])      # [1, 2, 3, 4]

# Grouping
grouped = {
  for k, v in var.items : 
  v.category => v...
}

# Pivoting
pivoted = {
  for item in var.items :
  item.id => {
    name = item.name
    type = item.type
  }
}
```

---

## Quick Reference

### Common Functions
```hcl
# String
lower("TEXT")           # "text"
upper("text")           # "TEXT"
substr("hello", 0, 3)   # "hel"

# Numeric
max(1, 5, 3)            # 5
min(1, 5, 3)            # 1
abs(-5)                 # 5

# Collection
length([1, 2, 3])       # 3
keys({a=1, b=2})        # ["a", "b"]
merge({a=1}, {b=2})     # {a=1, b=2}
```

### Conditional Expressions
```hcl
value = condition ? true_value : false_value
```

---

## Common Pitfalls

### Pitfall 1: Function Syntax Errors
**Problem**: Incorrect function usage causes errors
**Solution**: Check function documentation, test syntax
**Prevention**: Use terraform validate

### Pitfall 2: Type Mismatches
**Problem**: Functions expect specific types
**Solution**: Use type conversion functions
**Prevention**: Understand function requirements

### Pitfall 3: Complex Expressions
**Problem**: Hard to read and maintain
**Solution**: Use locals for complex expressions
**Prevention**: Keep expressions simple

---

## Best Practices

1. **Use Functions**: Leverage built-in functions
2. **Keep Simple**: Avoid overly complex expressions
3. **Use Locals**: For computed values
4. **Test Expressions**: Verify logic works
5. **Document Complex**: Explain complex expressions
6. **Type Conversion**: Use conversion functions
7. **Validate Inputs**: Before using in functions
8. **Read Documentation**: Understand function behavior
9. **Test Edge Cases**: Verify function behavior
10. **Use Conditionals**: For dynamic values

---

## Further Reading

### Official Documentation
- [Terraform Functions](https://www.terraform.io/docs/language/functions/index.html)
- [Expressions](https://www.terraform.io/docs/language/expressions/index.html)

### Related Topics
- HCL Configuration Language (Module 2)
- Advanced Features (Module 10)

---

*This module covers Terraform functions and expressions in detail. Understanding functions and expressions enables you to write dynamic, flexible Terraform configurations that adapt to different scenarios and requirements.*

