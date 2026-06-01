# 🏗️ terraform-aws-security-group

[![vikas](https://img.shields.io/badge/Made%20by-vikas-blue?style=flat-square&logo=terraform)]
[![License: MIT](https://img.shields.io/badge/License-MIT-blue.svg)](LICENSE)
[![Terraform](https://img.shields.io/badge/Terraform-1.13%2B-purple.svg?logo=terraform)](#)
[![CI](https://github.com/chahalvikas2022/terraform-multicloud-labels/actions/workflows/ci.yml/badge.svg)]

[//]: # (> 🌩️ **A production-grade, reusable AWS Subnet module by []&#40;https://www.opsstation.com&#41;**)
> Designed for reliability, performance, and security — following AWS networking best practices.
---

## 🏢 About vikas

**vikas** delivers **Cloud & DevOps excellence** for modern teams:
- 🚀 **Infrastructure Automation** with Terraform, Ansible & Kubernetes
- 💰 **Cost Optimization** via scaling & right-sizing
- 🛡️ **Security & Compliance** baked into CI/CD pipelines
- ⚙️ **Fully Managed Operations** across AWS, Azure, and GCP



---

🌟 Features

- Creates and manages AWS Security Groups
- Supports new and existing Security Groups
- Flexible ingress and egress rule management
- Allows rules with CIDR blocks, prefix lists, and security group references
- Supports both IPv4 and IPv6 traffic rules
- Enables fine-grained control for TCP, UDP, and ICMP protocols
- Compatible with VPC and cross-VPC setups
- Automatically tags resources using the Labels module
- Fully compatible with other OpsStation Terraform modules


## ⚙️ Usage Example
## Example: Basic

```hcl
module "security_group" {
  source      = "git::https://github.com/opsstation/terraform-aws-security-group.git?ref=v1.0.0"
  name        = local.name
  environment = local.environment
  vpc_id      = module.vpc.vpc_id

  ## INGRESS Rules
  new_sg_ingress_rules_with_cidr_blocks = [{
    rule_count  = 1
    from_port   = 22
    to_port     = 22
    protocol    = "tcp"
    cidr_blocks = ["172.16.0.0/16"]
    description = "Allow ssh traffic."
  },
    {
      rule_count  = 2
      from_port   = 27017
      to_port     = 27017
      protocol    = "tcp"
      cidr_blocks = ["172.16.0.0/16"]
      description = "Allow Mongodb traffic."
    }
  ]

  ## EGRESS Rules
  new_sg_egress_rules_with_cidr_blocks = [{
    rule_count  = 1
    from_port   = 22
    to_port     = 22
    protocol    = "tcp"
    cidr_blocks = ["172.16.0.0/16"]
    description = "Allow ssh outbound traffic."
  },
    {
      rule_count  = 2
      from_port   = 27017
      to_port     = 27017
      protocol    = "tcp"
      cidr_blocks = ["172.16.0.0/16"]
      description = "Allow Mongodb outbound traffic."
    }
  ]
}
```

## Example: Complete
```hcl
module "security_group" {
  source      = "git::https://github.com/opsstation/terraform-aws-security-group.git?ref=v1.0.0"
  name        = local.name
  environment = local.environment
  vpc_id      = module.vpc.vpc_id

  ## INGRESS Rules
  new_sg_ingress_rules_with_cidr_blocks = [{
    rule_count  = 1
    from_port   = 22
    to_port     = 22
    protocol    = "tcp"
    cidr_blocks = ["172.16.0.0/16"]
    description = "Allow ssh traffic."
  },
    {
      rule_count  = 2
      from_port   = 27017
      to_port     = 27017
      protocol    = "tcp"
      cidr_blocks = ["172.16.0.0/16"]
      description = "Allow Mongodb traffic."
    }
  ]

  new_sg_ingress_rules_with_self = [{
    rule_count  = 1
    from_port   = 22
    to_port     = 22
    protocol    = "tcp"
    description = "Allow ssh traffic."
  },
    {
      rule_count  = 2
      from_port   = 443
      to_port     = 443
      protocol    = "tcp"
      description = "Allow Mongodbn traffic."
    }
  ]

  new_sg_ingress_rules_with_source_sg_id = [{
    rule_count               = 1
    from_port                = 22
    to_port                  = 22
    protocol                 = "tcp"
    source_security_group_id = "sg-0f251332b1474ba77"
    description              = "Allow ssh traffic."
  },
    {
      rule_count               = 2
      from_port                = 27017
      to_port                  = 27017
      protocol                 = "tcp"
      source_security_group_id = "sg-0f251332b1474ba77"
      description              = "Allow Mongodb traffic."
    }]

  ## EGRESS Rules
  new_sg_egress_rules_with_cidr_blocks = [{
    rule_count  = 1
    from_port   = 22
    to_port     = 22
    protocol    = "tcp"
    cidr_blocks = [module.vpc.vpc_cidr_block, "172.16.0.0/16"]
    description = "Allow ssh outbound traffic."
  },
    {
      rule_count  = 2
      from_port   = 27017
      to_port     = 27017
      protocol    = "tcp"
      cidr_blocks = ["172.16.0.0/16"]
      description = "Allow Mongodb outbound traffic."
    }
  ]

  new_sg_egress_rules_with_self = [{
    rule_count  = 1
    from_port   = 22
    to_port     = 22
    protocol    = "tcp"
    description = "Allow ssh outbound traffic."
  },
    {
      rule_count  = 2
      from_port   = 27017
      to_port     = 27017
      protocol    = "tcp"
      description = "Allow Mongodb traffic."
    }]

  new_sg_egress_rules_with_source_sg_id = [{
    rule_count               = 1
    from_port                = 22
    to_port                  = 22
    protocol                 = "tcp"
    source_security_group_id = "sg-0f251332b1474ba77"
    description              = "Allow ssh outbound traffic."
  },
    {
      rule_count               = 2
      from_port                = 27017
      to_port                  = 27017
      protocol                 = "tcp"
      source_security_group_id = "sg-0f251332b1474ba77"
      description              = "Allow Mongodb traffic."
    }]
}
```

## Example: Only_rules

```hcl
module "security_group_rules" {
  source         = "git::https://github.com/opsstation/terraform-aws-security-group.git?ref=v1.0.0"
  name           = local.name
  environment    = local.environment
  vpc_id         = module.vpc.vpc_id
  new_sg         = false
  existing_sg_id = "sg-0474592052307dbb2"

  ## INGRESS Rules
  existing_sg_ingress_rules_with_cidr_blocks = [{
    rule_count  = 1
    from_port   = 22
    to_port     = 22
    protocol    = "tcp"
    cidr_blocks = ["10.9.0.0/16"]
    description = "Allow ssh traffic."
  },
    {
      rule_count  = 2
      from_port   = 27017
      to_port     = 27017
      protocol    = "tcp"
      cidr_blocks = ["10.9.0.0/16"]
      description = "Allow Mongodb traffic."
    }
  ]

  ## EGRESS Rules
  existing_sg_egress_rules_with_cidr_blocks = [{
    rule_count  = 1
    from_port   = 22
    to_port     = 22
    protocol    = "tcp"
    cidr_blocks = ["10.9.0.0/16"]
    description = "Allow ssh outbound traffic."
  },
    {
      rule_count  = 2
      from_port   = 27017
      to_port     = 27017
      protocol    = "tcp"
      cidr_blocks = ["10.9.0.0/16"]
      description = "Allow Mongodb outbound traffic."
    }]
}
```

## Example: Prefix_list

```hcl
module "security_group" {
  source              = "git::https://github.com/opsstation/terraform-aws-security-group.git?ref=v1.0.0"
  name                = local.name
  environment         = local.environment
  vpc_id              = module.vpc.vpc_id
  prefix_list_enabled = true
  entry = [{
    cidr = "10.19.0.0/16"
  }]

  ## INGRESS Rules
  new_sg_ingress_rules_with_prefix_list = [{
    rule_count  = 1
    from_port   = 22
    to_port     = 22
    protocol    = "tcp"
    description = "Allow ssh traffic."
  }
  ]

  ## EGRESS Rules
  new_sg_egress_rules_with_prefix_list = [{
    rule_count  = 1
    from_port   = 3306
    to_port     = 3306
    protocol    = "tcp"
    description = "Allow mysql/aurora outbound traffic."
  }
  ]
}
```
## 📤 Outputs

| **Name** | **Description** |
|-----------|-----------------|
| `security_group_id` | The unique identifier (ID) of the created Security Group. |
| `security_group_name` | The name assigned to the Security Group. |
| `security_group_arn` | The Amazon Resource Name (ARN) of the created Security Group. |
| `vpc_id` | The ID of the VPC in which the Security Group is created. |
| `ingress_rules` | A list of all configured inbound (ingress) rules applied to the Security Group. |
| `egress_rules` | A list of all configured outbound (egress) rules applied to the Security Group. |
| `description` | The description provided for the Security Group. |
| `owner_id` | The AWS account ID of the Security Group’s owner. |
| `tags` | A mapping of all tags assigned to the Security Group. |
| `arn` | Alias output for `security_group_arn` (for compatibility with external references). |

---
### ☁️ Tag Normalization Rules (AWS)

| Cloud | Case      | Allowed Characters | Example                            |
|--------|-----------|------------------|------------------------------------|
| **AWS** | TitleCase | Any              | `Name`, `Environment`, `CostCenter` |

---

### 💙 Maintained by [vikas]
> vikash — Simplifying Cloud, Securing Scale.