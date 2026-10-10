# Lab 3

Instructions for this section will be provided in class and on Blackboard when we reach it.

Put your work for Lab 3 in this folder.

"Why a real account would use OIDC rather than stored keys?"

- Real account would use OIDC to obtain temporary AWS credentials instead of storing
long lived access keys in the repo - this reduces risk of credential exposure that
would occur from those long lived keys

"why this course uses session-scoped secrets and what limits the damage if they leak."
- Course uses session-scoped secrets because AWS Academy was said to restrict
IAM changes required for OIDC, the damage is limited even if leaked because the
session-scoped secrets is only 4 hours from the start of pressing Start Lab in
AWS Academy

ExpiredToken on a deploy: your lab session ended. 
Start a new one, re-run refresh-gha-creds.sh, re-run the job. 
Nothing in the repository changes.

"Input required and not supplied: aws-region": the AWS_REGION variable does not exist 
not a credential problem. Run the refresh script, 
or gh variable set AWS_REGION --body us-east-1.


Terraform version used: 1.10.3

## Experiments


End Lab should revoke access using the temporary AWS credentials provided/stored in GitHub Actions. The workflow should then fail when aws sts get-caller-identity attempts to authenticate with those expired credentials. I expect AWS to return an ExpiredToken error or a similar authentication failure. After starting a new lab session and refreshing the GitHub Actions credentials, the workflow should succeed again without requiring any repository file changes.  
Observed: The workflow instead reached terraform init and failed while trying to access lab3/terraform.tfstate in the S3 backend, returning HTTP 403 Forbidden. After starting a new lab session and rerunning refresh-gha-creds.sh, the workflow succeeded again with no repository file changes, confirming that the issue was expired temporary credentials rather than the Terraform code.

Remove the backend
Commenting out the backend "s3" block and running terraform init -migrate-state moved Terraform away from the shared S3 backend and resulted in local state being used. When I ran terraform plan, Terraform proposed creating the existing SSM parameter again with 1 to add, 0 to change, 0 to destroy, showing that without the shared backend it no longer had the same state information. After the experiment, I restored the S3 backend, reinitialized Terraform, and removed the local terraform.tfstate file.

