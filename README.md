# tf-pipeline
Collection of terraform pipelines.

## Notes
- AWS Credentials: Make sure to store your AWS credentials securely in GitHub Secrets.
- Toggle Destroy: Use a secret (e.g., DESTROY_STAGING) to control whether the destroy step should run.
- Terraform Version: Adjust the Terraform version according to your needs.

## Workflows:
On pull requests to the main branch:

The terraform_pr job runs, which checks out the code, sets up Terraform, initializes the working directory, checks formatting, validates the configuration, and creates an execution plan.


When merging to the main branch:

The terraform_apply_staging job runs after the terraform_pr job. It applies the changes to the staging AWS account and then destroys the changes.
This job uses an environment called "staging", which allows you to add approvals or other restrictions.
To make this step optional, you can use GitHub environments to control when this job runs.


If step 2 passes without error:

The terraform_apply_production job runs, which applies the changes to your target AWS account (production).
This job also uses an environment called "production" for added control.



## To use this workflow:

Save this file as .github/workflows/terraform.yml in your repository.
Set up the following secrets in your GitHub repository:

`AWS_ACCESS_KEY_ID: Your AWS access key ID`
`AWS_SECRET_ACCESS_KEY: Your AWS secret access key`


Configure your Terraform backend and provider settings in your Terraform configuration files.
To make the staging step optional, you can use GitHub environments:

Go to your repository settings
Create environments for "staging" and "production"
For the "staging" environment, you can add a required reviewer or other deployment rules
