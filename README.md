# terraform-demo

Welcome to the **Terraform Demo Playground**, a playful little lab for learning how Terraform builds an AWS environment from code.

## 🧩 What this project does

This repo creates a tiny AWS setup containing:

- an AWS provider configured for `us-west-2`
- a canonical Ubuntu AMI lookup
- a reusable VPC created from the `terraform-aws-modules/vpc/aws` module
- a single EC2 instance launched inside a private subnet

It is designed as a beginner-friendly study project: one little infrastructure script, one instance, and one chance to see Terraform make AWS happen.

## 🛠️ Files in the project

- `terraform.tf`
  - Configures Terraform Cloud settings for the workspace
  - Declares the AWS provider and minimum Terraform version
- `main.tf`
  - Defines the AWS provider region
  - Looks up the latest Ubuntu 24.04 AMI using `aws_ami`
  - Creates a VPC with public and private subnets using the official VPC module
  - Launches an EC2 instance into the first private subnet
- `variables.tf`
  - Defines the `instance_name` and `instance_type` variables
  - Provides sensible defaults for playing around quickly

## 🎯 What you learn here

This project is an interactive primer for:

- provider configuration
- using data sources to look up AMIs
- consuming a community Terraform module
- tagging resources with `Name`
- parameterizing your infrastructure with variables
- splitting config into reusable files

## 🚀 How to use it

1. Clone the repository
2. Set up your AWS credentials in your environment
3. Run `terraform init`
4. Run `terraform plan`
5. Run `terraform apply`

After apply, Terraform will create the VPC and EC2 instance in AWS.

## 🔐 AWS provider secrets

The AWS provider needs valid AWS credentials so Terraform can authenticate to your account. The most common secret values are:

- `AWS_ACCESS_KEY_ID`
- `AWS_SECRET_ACCESS_KEY`
- `AWS_SESSION_TOKEN` (optional, only for temporary credentials)

You can provide these in one of several safe ways:

- environment variables (recommended for local use)
- the shared credentials file at `~/.aws/credentials`
- an AWS profile name using `AWS_PROFILE`

Example environment setup:

```bash
export AWS_ACCESS_KEY_ID="YOUR_ACCESS_KEY_ID"
export AWS_SECRET_ACCESS_KEY="YOUR_SECRET_ACCESS_KEY"
export AWS_DEFAULT_REGION="us-west-2"
```

Never commit your AWS credentials or secret keys into Git. Keep them out of source control and use vaults, profiles, or secure credential stores instead.

## 🧪 Local testing vs Terraform Cloud

This repo includes a Terraform Cloud configuration block in `terraform.tf`. If you want to test locally, you can comment out the `cloud { ... }` block and use local state instead.

If you prefer to use Terraform Cloud, create an HCP account and run:

```bash
terraform login
```

That allows Terraform to use your local credentials for authentication and manage state in Terraform Cloud.

## 🧪 Customization options

You can change these values in `variables.tf` or override them from the CLI:

- `instance_name` — the EC2 instance `Name` tag
- `instance_type` — the EC2 instance size, default is `t2.micro`

Example:

```bash
terraform apply -var='instance_name=playground-server' -var='instance_type=t3.micro'
```

## ⚠️ Notes

- The VPC module uses three AZs in `us-west-2` but only creates one public subnet and two private subnets.
- The EC2 instance is placed in the first private subnet and uses the VPC module's default security group.

## 🎲 Final word

This project is a fun, small-scale Terraform playground: start the experiment, watch AWS infrastructure appear, and then destroy it when you are done.

```bash
terraform destroy
```

Happy Terraforming! 🌱
