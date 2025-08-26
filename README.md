# Terraform GitHub Actions Workflows

This repository contains reusable GitHub Actions workflow templates for running Terraform `plan` and `apply` operations. These workflows are designed to be triggered using `workflow_call` from other workflows and support AWS IAM role assumption for secure infrastructure deployments.

---

## Workflows

### 1. `terraform-plan-template.yaml`

**Purpose:**  
Executes the `terraform plan` command to generate and display an execution plan for Terraform-managed infrastructure.

#### Inputs:
- `working_directory` (string, required): Path to the Terraform configuration directory.
- `terraform_version` (string, required, default: `1.5.7`): Version of Terraform to use.
- `aws_iam_role_arn` (string, required): ARN of the AWS IAM Role to assume.
- `aws_iam_role_session_name` (string, required): Name for the AWS IAM Role session.
- `aws_region` (string, required, default: `eu-central-1`): AWS region for deployment.

#### Outputs:
- `terraform_plan_output`: The output result from the Terraform plan operation.

#### Key Steps:
- Check out the repository.
- Configure AWS credentials using IAM role assumption.
- Set up the specified Terraform version.
- Run Terraform init, fmt check, validation, and generate a plan.
- Save plan output to `plan.out`.

---

### 2. `terraform-apply-template.yaml`

**Purpose:**  
Executes the `terraform apply` command to apply the changes required to reach the desired state of the configuration.

#### Inputs:
- `working_directory` (string, required): Path to the Terraform configuration directory.
- `terraform_version` (string, required, default: `1.5.7`): Version of Terraform to use.
- `aws_iam_role_arn` (string, required): ARN of the AWS IAM Role to assume.
- `aws_iam_role_session_name` (string, required): Name for the AWS IAM Role session.
- `aws_region` (string, required, default: `eu-central-1`): AWS region for deployment.

#### Outputs:
- `terraform_apply_output`: The output result from the Terraform apply operation.

#### Key Steps:
- Check out the repository.
- Configure AWS credentials using IAM role assumption.
- Set up the specified Terraform version.
- Run Terraform init, fmt check, validation, and apply the plan.
- Store apply output in `apply.out`.

---

## Usage

You can call these workflows from your own workflow like this:

```yaml
jobs:
  terraform-plan:
    uses: devops-engineer-associate-1/shared-github-workflows/.github/workflows/terraform-plan-template.yaml@main
    with:
      working_directory: ./terraform
      terraform_version: 1.5.7
      aws_iam_role_arn: arn:aws:iam::123456789012:role/your-role
      aws_iam_role_session_name: github-action-session
      aws_region: eu-central-1
```

```yaml
jobs:
  terraform-apply:
    needs: terraform-plan
    uses: devops-engineer-associate-1/shared-github-workflows/.github/workflows/terraform-apply-template.yaml@main
    with:
      working_directory: ./terraform
      terraform_version: 1.5.7
      aws_iam_role_arn: arn:aws:iam::123456789012:role/your-role
      aws_iam_role_session_name: github-action-session
      aws_region: eu-central-1
```
