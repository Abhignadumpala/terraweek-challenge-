# 🧩 TerraWeek Day 2 – HCL Deep Dive: Variables, Types & Expressions

**Date:** Monday, 13th July 2026

## Learning Summary

Today I explored HCL in depth — blocks, arguments, expressions, variables, validation, locals, outputs, built-in functions, and variable precedence. Also built a Terraform project using the Docker provider and practiced passing variables using both `-var` and `terraform.tfvars`.

Deep-dive theory for each topic lives in my [terraform-notes](https://github.com/Abhignadumpala/terraform-notebook) repo — linked throughout. This file focuses on the hands-on proof of work for the challenge.

---

## Table of Contents

- [Project Structure](#project-structure)
- [Task 1: Master HCL Syntax](#task-1-master-hcl-syntax)
- [Task 2: Variables, Types & Validation](#task-2-variables-types--validation)
- [Task 3: Locals, Outputs & Functions](#task-3-locals-outputs--functions)
- [Task 4: Build a Terraform Project](#task-4-build-a-terraform-project)
- [Difference Between `-var` and `terraform.tfvars`](#difference-between--var-and-terraformtfvars)
- [Variable Precedence](#variable-precedence)
- [Bonus](#bonus)
- [Final Verification](#final-verification)
- [Learning Outcome](#learning-outcome)

---

## Project Structure

```text
day02/
├── notes.md
├── example/
│   ├── main.tf
│   ├── variables.tf
│   ├── outputs.tf
│   ├── terraform.tfvars
│   └── terraform.tfvars.example
└── images/
```

---

## Task 1: Master HCL Syntax

```hcl
block_type "label_one" "label_two" {
  argument = value
}
```

```hcl
resource "aws_s3_bucket" "my_bucket" {
  bucket = "abhigna-terraform-lab-2026"
}
```

| Component | Description |
|---|---|
| Block | Defines an infrastructure object |
| Argument | Configures the block using `key = value` |
| Expression | Calculates or references values |

> 📖 **Read More** → [HCL Blocks, Arguments & Expressions](https://github.com/Abhignadumpala/terraform-notebook/blob/main/hcl-blocks-arguments-expressions.md)

---

## Task 2: Variables, Types & Validation

Created `variables.tf` covering all major Terraform variable types with a default, a validation block, and a sensitive variable.

### Variable Types

| Category | Types |
|---|---|
| Primitive | `string`, `number`, `bool` |
| Collection | `list`, `map`, `set` |
| Structural | `object`, `tuple` |

> 📖 **Read More** → [Terraform Variables — Types, Defaults, Validation](https://github.com/Abhignadumpala/terraform-notebook/blob/main/terraform-variables-types-validation.md)

### Project Files

- [`variables.tf`](./example/variables.tf)
- [`terraform.tfvars`](./example/terraform.tfvars)
- [`terraform.tfvars.example`](./example/terraform.tfvars.example)

### Output

![Variables & Validation](./images/01-task-2-variables-validation.png)

---

## Task 3: Locals, Outputs & Functions

Implemented a `locals` block, exposed values with `outputs`, and practiced built-in functions live using `terraform console`.

### Functions Practiced

- `upper()`
- `merge()`
- `join()`

> 📖 **Read More**
> - [Locals & Outputs](https://github.com/Abhignadumpala/terraform-notebook/blob/main/terraform-locals-and-outputs.md)
> - [Built-In Functions](https://github.com/Abhignadumpala/terraform-notebook/blob/main/terraform-built-in-functions.md)

### Project Files

- [`main.tf`](./example/main.tf)
- [`outputs.tf`](./example/outputs.tf)

### Results

#### Locals
![](./images/02-task-3.1-locals.png)

#### Outputs
![](./images/03-task-3.2-output.png)

#### Terraform Console
![](./images/04-task-3.3-terraform-console.png)

---

## Architecture

```text
Terraform CLI
      │
      ▼
Terraform Configuration
      │
      ▼
Docker Provider
      │
      ▼
Docker Engine
      │
      ▼
Nginx Container
      │
      ▼
http://localhost:8080
```

---

## Task 4: Build a Terraform Project

Built an Nginx Docker container using the Terraform Docker provider and ran the full Terraform workflow.

> 📖 **Read More**
> - [HCL Blocks, Arguments & Expressions](https://github.com/Abhignadumpala/terraform-notebook/blob/main/hcl-blocks-arguments-expressions.md)

### Project Files

- [`main.tf`](./example/main.tf)
- [`variables.tf`](./example/variables.tf)
- [`outputs.tf`](./example/outputs.tf)
- [`terraform.tfvars`](./example/terraform.tfvars)

### Terraform Workflow

| Step | Description |
|---|---|
| `terraform init` | Initialize providers |
| `terraform plan` | Preview changes |
| `terraform apply` | Create resources |
| `terraform output` | View outputs |
| `terraform destroy` | Remove resources |

### Workflow Output

#### Initialize
![](./images/05-task-4.1-init.png)

#### Plan (`-var`)
```bash
terraform plan -var 'container_name=tws-web' -var 'external_port=8080'
```
![](./images/06-task-4.2-plan-var.png)

#### Apply (`-var`)
```bash
terraform apply -var 'container_name=tws-web' -var 'external_port=8080'
```
![](./images/07-task-4.3.0-apply-var.png)

#### Browser Output
Verified the container was running by visiting `http://localhost:8080`.
![](./images/18-browser-output.png)

#### Output
```bash
terraform output
```
![](./images/08-task-4.3.1-output-var.png)

#### Destroy
```bash
terraform destroy -var 'container_name=tws-web' -var 'external_port=8080'
```
![](./images/09-task-4.4-destroy-var.png)

#### Using `terraform.tfvars` instead of `-var`

##### Plan
![](./images/10-4.0-plan-terraformtfvar-file.png)

##### Apply
![](./images/11-4.1-apply-terraformtfvar-file.png)

##### Destroy
![](./images/12-4.2-destroy-terraformtfvar-file.png)

---

## Difference Between `-var` and `terraform.tfvars`

| `-var` | `terraform.tfvars` |
|---|---|
| Passes variables through the command line | Stores variables in a file |
| Good for testing or one-off runs | Best for reusable projects |
| Must be supplied every run | Loaded automatically |

> 📖 **Read More** → [Variable Precedence](https://github.com/Abhignadumpala/terraform-notebook/blob/main/terraform-variable-precedence.md)

---

## Variable Precedence

```text
-var / -var-file
      ↓
*.auto.tfvars
      ↓
terraform.tfvars
      ↓
TF_VAR_*
      ↓
default
```

> 📖 **Read More** → [Variable Precedence — full breakdown](https://github.com/Abhignadumpala/terraform-notebook/blob/main/terraform-variable-precedence.md)

---

## Bonus

| Feature | Status |
|---|:---:|
| `for` expression | ✅ |
| Conditional expression | ✅ |
| `optional()` attributes | ✅ |

> 📖 **Read More** → [Built-In Functions & Advanced Expressions](https://github.com/Abhignadumpala/terraform-notebook/blob/main/terraform-built-in-functions.md)

### Results
![](./images/13-5.1-bonus-for-expression.png)
![](./images/14-5.2-bonus-optional.png)

---

## Final Verification

```bash
terraform fmt
terraform validate
terraform plan
terraform apply
```

### Final Output
![](./images/15-final-changes-fmt.png)
![](./images/16-final-apply.png)

---

## Learning Outcome

By completing Day 2, I can now:

- Build reusable Terraform configurations using HCL
- Define variables with validation and correct data types
- Use locals to reduce duplicate configuration values
- Display resource information using outputs
- Evaluate expressions live with `terraform console`
- Explain Terraform's variable precedence order
- Provision and destroy Docker resources with Terraform
- Follow the complete workflow from `init` to `destroy`

---

## 🎯 Conclusion

Day 2 strengthened my understanding of HCL and Terraform fundamentals. Combining theory with hands-on practice helped me write reusable configurations, manage variables properly, and automate Docker infrastructure end to end.
