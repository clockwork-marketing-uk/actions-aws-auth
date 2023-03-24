# AWS Auth Action

Authorize using the GitHub OIDC to AWS

## Installation

---

The AWS account being used will need to have the following setup in AWS

## IAM - Identity provider

**Provider:** token.actions.githubusercontent.com

**Audience:** sts.amazonaws.com

## IAM - Roles

**plan** with ReadOnlyAccess

**apply** with AdministratorAccess

Any roles will need to have a trusted entity of the Identity Provider set

```json

{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Principal": {
                "Federated": "arn:aws:iam::{AWS_ACCOUNT_NUMBER}:oidc-provider/token.actions.githubusercontent.com"
            },
            "Action": "sts:AssumeRoleWithWebIdentity",
            "Condition": {
                "StringEquals": {
                    "token.actions.githubusercontent.com:aud": "sts.amazonaws.com"
                },
                "StringLike": {
                    "token.actions.githubusercontent.com:sub": "repo:{GITHUB_ORG_NAME}/*:*"
                }
            }
        }
    ]
}

```

## Usage
----

```yaml

    - name: Authorize
      uses: clockwork-marketing-uk/actions-aws-auth@2
      with:
        region: ${{env.AWS_DEFAULT_REGION}}
        account_id: ${{env.AWS_ACCOUNT_ID}}
        role: ${{env.ROLE_NAME}}

```
