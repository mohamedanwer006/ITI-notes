# Terraform Advanced

`terraform plan` output =>  the plan will applied in reverse order 

`terraform plan -out=plan.tfplan` =>  save the plan to a **plan.tfplan** file

The  **plan.tfplan** is binary file you can read it with `terraform show plan.tfplan`

also you can show it as json with `terraform show -json plan.tfplan` this useful for integration with other tools like CI/CD tools


By default the `show` command will show the state




Terraform Variables
===

Terraform variables are used to pass values to Terraform configuration files. They are used to parameterize the configuration files so that they can be used in different environments without having to change the configuration files.

Terraform variables Values can be set in the following ways:
1. Default values
2. Environment variables 
3. From a file `terraform.tfvars` or any `*.auto.tfvars`
4. Command-line flags

Command-line flags have the **highest precedence**, followed by terraform.tfvars, and then environment variables. If a variable is set in multiple places, Terraform will use the last one it finds.

If a variable is not set in any of these ways, its value will be **null**.

Terraform Variables Types
===
Terraform supports the following variable types :
1. `string`
2. `boolean` (true or false)
3. `number` (integer or float)
4. `list` (zero based array) 
5. `set` (like a list but unordered)
6. `map` (key-value pairs) variable "my_map" { type = map(string) default = { "key1" = "value1", "key2" = "value2" } }
7. `object` (complex type like struct in other languages) variable "my_object" { type = object({ name = string, age = number }) default = { name = "John", age = 30 } }
8. `tuple` (similar to object but with stricter type conversion rules) variable "my_tuple" { type = tuple([string, number]) default = ["John", 30] }



```bash
# string variable
variable "my_string" {
  type        = string
  description = "A simple string variable"
  default     = "Hello, World!"
}

# boolean variable
variable "my_boolean" {
  type        = bool
  description = "A simple boolean variable"
  default     = true
}

# number variable
variable "my_number" {
  type        = number
  description = "A simple number variable"
  default     = 42
}

# list variable
variable "my_list" {
  type        = list(string)
  description = "A simple list variable"
  default     = ["DEV", "QA", "STAGE", "PROD"]
}



# map variable
variable "my_map" {
  type        = map(string)
  description = "A simple map variable"
  default     = { "DEV" = "Development", "QA" = "Quality Assurance", "STAGE" = "Staging", "PROD" = "Production" }
}

# object variable
variable "my_object" {
  type = object({
    name = string
    age  = number
  })
  description = "A simple object variable"
  default = {
    name = "John"
    age  = 30
  }
}

# map of objects

variable "environments_instance_settings" {
  type = map(object({instance_type = string, monitoring = bool}))
  default = {
    "DEV" = {
      instance_type = "t2.micro"
      monitoring    = false
    }
    "QA" = {
      instance_type = "t2.small"
      monitoring    = false
    }
    "STAGE" = {
      instance_type = "t2.medium"
      monitoring    = false
    }
    "PROD" = {
      instance_type = "t2.large"
      monitoring    = true
    }

  }   

}

# access the map of objects var.environments_instance_settings["DEV"].instance_type

```

Setting Variables Values :
===
1. Terraform variables can be set in the following ways:
from a file` terraform.tfvars `,`terraform.tfvars.json`  or any `*.auto.tfvars`  
2. Environment variables `export TF_VAR_my_variable=foo`
3. command-line flags `terraform apply -var 'my_variable=foo'`



Terraform Outputs variables
===
Terraform outputs are used to display values that are computed by Terraform. They are used to display information to the user after a Terraform run.


Terraform Expressions
===

Hashicorp Configuration language (HCL) is a declarative language that is used to write Terraform configuration files. It is a superset of JSON and supports all the features of JSON. It also supports comments.

It is a Domain Specific Language (DSL), This means it does not have all the features of a general-purpose programming language. It is designed to be easy to read and write.

HCL supports: 

1. Expressions: (5+1) or (5>2)
2. Predefined Functions: length(var.my_list), split()

Experments with expressions and functions using the `terraform console` command
this command start in REPL read-evaluate-print-loop mode

A REPL is a simple, interactive computer programming environment that takes single user inputs (i.e. single expressions), evaluates them, and returns the result to the user;


```bash
$ terraform console
> 5+1
6
> 5>2
true
> length(var.my_list)
4
> split(",", "a,b,c,d")
[
  "a",
  "b",
  "c",
  "d",
]
> var.my_map["DEV"]
"Development"
> 5!=5? "true" : "false"
false

```

Terraform Function:
===
Terraform Function are grouped by type: 

- Numeric 
- String
- Collection
- Encoding
- Filesystem
- Date and Time
- Hash and Crypto
- IP Network
- Type Conversion


String Functions examples:
===
```bash
> lower("HELLO, WORLD!")
"hello, world!"
> upper("hello, world!")
"HELLO, WORLD!"
> substr("hello, world!", 0, 5)
"hello"
> substr("hello, world!", 7, 5)
"world"
> replace("hello, world!", "world", "Terraform")
"hello, Terraform!"
> format("%s, %s!", "hello", "world")
"hello, world!"
> split(",", "a,b,c,d")
[
  "a",
  "b",
  "c",
  "d",
]
> # split returns a list 
> join(",", ["a", "b", "c", "d"])
"a,b,c,d"

```

Terraform Count and For_each
===
Terraform count and for_each are used to create multiple resources of the same type.

count is used to create multiple copies of a resource. It is used when the number of resources is known in advance.

```bash
resource "aws_instance" "my_instances" {
  count = 3
  ami           = "ami-0c55b159cbfafe1f0"
  instance_type = "t2.micro"
}
# will create 3 identical instances
```

Access count from output
----

```bash 
output "instance_ids" {
  value = aws_instance.my_instances.*.id
}
# will output a list of instance ids
```

count it useful when you want to create multiple resources of the same type.

----

for_each is used to create multiple copies of a resource. It is used ideal for resources that vary one from another.

```bash
variable "environments" {
  type = list(string)
  default = ["DEV", "QA", "STAGE", "PROD"]
}
resource "aws_instance" "my_instances" {
  for_each = var.environments
  ami           = "ami-0c55b159cbfafe1f0"
  instance_type = "t2.micro"
  tags = {
    Name = each.key
  }
}
```

Terraform Modules
===
- Self-contained sub-configurations with input and output variables
- Module library
- Modules simplify configuration
- Modules can be used multiple times

Module cannot be deployed independently, they are used to create reusable components.

```
rootModule|
          |-> child module1
          |-> child module2
          |-> child module3
```
by default terraform modules will inherit the settings of the same provider as the root module.

use module block to call a module

```bash
module "my_module" {
  source = "github.com/terraform-aws-modules/terraform-aws-vpc"
  name   = "my_vpc"
  cidr   = "
}
```

Module Storage (source)
---

- Local file system
- Git repository
- GitHub
- HTTP(S) url
- Terraform Registry free public modules registry from Hashicorp

```
module "localName" {
  source = "./path/to/module"
}
```
-----


# Terraform Advanced
https://github.com/LinkedInLearning/advanced-terraform-2823489
