# Cfn Sync Stacks

## environments.yaml

contains the AWS environment/account/region configuration. It is used to generate the cfn-sync deployment templates and deployment stacks.

An Example of an environments.yaml

```yaml
dev:
  accountId: 123456789012
  regions: 
  - ap-southeast-2

test:
  accountId: 223456789012
  regions: 
  - ap-southeast-2
  - us-west-2

prod:
  accountId: 323456789012
  regions: 
  - ap-southeast-2
  - us-west-2
  - us-east-1
```

To add a new environment or region to an existing environment you create a branch from `main` add the configuration to the `environments.yaml` commit and push the branch to github and create a pull-request for the change.

The creation of the PR will trigger the `.github/workflows/setup-deployments.yml` workflow which will generate the deployment templates and stacks and commit them to the PR

## Deploy a new stack to an environment

1. create a new branch from `main`

2. create a stack deployment file in the `.stacks/<environment>/<region>/my-service.stack.yaml` for example

```yaml
template-file-path: ./stacks/template/my-service.compiled.yaml

parameters:
  EnvironmentName: dev-main 
  EnvironmentType: development
  ...

```

3. commit, push and create a pull request for the new stack. This will trigger the `.github/workflows/new-deployments.yml` workflow which will add the stack deployment configuration to the corresponding `.stacks/<environment>/<region>/cfn-sync.stack.yaml` file and commit this to the PR

4. Merging the PR to main will trigger the creation of the stack in the corresponding aws account and region