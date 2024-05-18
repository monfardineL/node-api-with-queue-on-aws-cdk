# Local development env

This stack provides a development environment to test the code before spending money on AWS.  
For this to work, we use [LocalStack](https://github.com/localstack/localstack), relying on Docker and Docker Compose.

## Requirements

This guide assumes you already have Docker and Docker Compose installed, so the steps are not covered here.

In order to work with AWS CLI on LocalStack, the easier way is to use `awslocal` CLI tool. You can read more about it, and other ways to use LocalStack [here](https://docs.localstack.cloud/user-guide/integrations/aws-cli/). This guide assumes you are using `awslocal`.

Hint: if you are trying to install `awslocal` with `pipx` but is having some errors, the fix may be to run:

```bash
pipx install awscli-local --break-system-packages
```

Is it safe? I don't know. But my system didn't break.

## How to use

Assuming you are inside `local-env` folder in your terminal:

- Bring LocalStack up by running docker-compose:

```bash
docker-compose up -d
```

- Check if `awsloocal` is working properly:

```bash
awslocal sts get-caller-identity
```

You should see an output similar to this:

```json
{
    "UserId": "AKIAIOSFODNN7EXAMPLE",
    "Account": "000000000000",
    "Arn": "arn:aws:iam::000000000000:root"
}
```

- Then deploy the CloudFormation stack with necessary resources:

```bash
awslocal cloudformation deploy --template-file local-dev-stack.yaml --stack-name local-development-stack
```

- Last, check if the resources were deployed successfully, and get their ARN:

```bash
awslocal cloudformation describe-stacks --stack-name local-development-stack --query "Stacks[0].Outputs" --output table
```
