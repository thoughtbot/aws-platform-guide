### Application Roles

For applications, CI/CD pipelines, and developers to perform their tasks
in AWS you will need three IAM roles for an application:

- Pod role: your application will use a Kubernetes service account to
  assume this role using
  [IRSA](https://docs.aws.amazon.com/eks/latest/userguide/iam-roles-for-service-accounts.html)
  in order to access storage, databases, and secrets.
- Deploy role: your CI/CD pipelines will assume this role and will be
  mapped to a Kubernetes group using
  [aws-auth](https://docs.aws.amazon.com/eks/latest/userguide/add-user-role.html)
  in order to update the application during deployment.
- Developer role: developers will assume this role and will be mapped
  to a Kubernetes group using
  [aws-auth](https://docs.aws.amazon.com/eks/latest/userguide/add-user-role.html)
  in order to view application status.

![Application Roles](./images/application-roles.png)

#### Pod Role

You can use the service-account-role module from Flightdeck to the
service account and an IAM role with the proper trust policy:

```
module "pod_role" {
  source = "github.com/thoughtbot/flightdeck//aws/service-account-role?ref=v0.9.0"

  cluster_names    = ["example-sandbox-v1"]
  name             = "example-pods"

  # Your manifests must use a service account with the same name and namespace
  service_accounts = ["example:example-staging"]
}
```

You can then create IAM policies and attach them to the role:

```
resource "aws_iam_policy" "reports_bucket" {
  name   = "example-bucket"
  policy = module.example_bucket.policy_json
}

resource "aws_iam_role_policy_attachment" "reports_bucket" {
  policy_arn = aws_iam_policy.example_bucket.arn
  role       = module.service_role.name
}
```

You can pass this role to the Flightdeck application-config module to
set up the proper service account and annotations to map pods to the
role:

```
module "staging_sandbox_v1" {
  source    = "github.com/thoughtbot/flightdeck//aws/application-config"

  # This must match the service account and namespace declared above
  namespace               = "example-staging"
  pod_service_account     = "example"

  pod_iam_role            = module.pod_role.arn
}
```

#### Deploy Role

If you’re using GitHub Actions, you can use the EKS deploy role module
to create your deploy role:

```
module "deploy_role" {
  source = "github.com/thoughtbot/terraform-eks-cicd//modules/github-actions-eks-deploy-role?ref=v0.1.1"

  cluster_names         = ["example-sandbox-v1"]
  github_branches       = ["main"]
  github_organization   = "example"
  github_repository     = "example"
  iam_oidc_provider_arn = data.aws_ssm_parameter.iam_oidc_provider_arn.value
  name                  = "example-staging-deploy"
}
```

This role must be added to the aws-auth ConfigMap, which you can do in
the platform configuration:

```
module "platform" {
  source = "github.com/thoughtbot/flightdeck//aws/platform?ref=v0.9.0"

  # Other config

  custom_roles = {
    example-staging-deploy = data.aws_iam_role.deploy.arn
  }
}

data "aws_iam_role" "eks_auth" {
  name = "example-staging-deploy"
}
```

You can then use Kubernetes role bindings to assign permissions to the
role.

If you’re using the Flightdeck application-config module, you can
include the deploy group as part of your configuration:

```
module "staging_sandbox_v1" {
  providers = { kubernetes = kubernetes.sandbox_v1 }
  source    = "github.com/thoughtbot/flightdeck//aws/application-config"

  deploy_group = "example-staging-deploy"
}
```

Flightdeck also includes a module to provide write access to a single
namespace if you’re configuring your deploy role separately:

```
module "deploy_role_bindings" {
  source     = "github.com/thoughtbot/flightdeck//aws/deploy-role-bindings"

  group         = "example-deploy"
  name          = "deploy"
  namespace     = "example-staging"
}
```

#### Developer Role

If you’re using the Flightdeck application-config module, you can
include the developer group as part of your configuration:

```
module "staging_sandbox_v1" {
  providers = { kubernetes = kubernetes.sandbox_v1 }
  source    = "github.com/thoughtbot/flightdeck//aws/application-config"

  developer_group = "example-staging-developer"
}
```

Flightdeck also includes a module to provide read access to a single
namespace if you’re configuring your developer role separately:

```
module "developer_role_bindings" {
  source     = "github.com/thoughtbot/flightdeck//aws/developer-role-bindings"

  enable_exec = false
  group       = "example-developer"
  name        = "developer"
  namespace   = "example-staging"
}
```

This role must be added to the aws-auth ConfigMap, which you can do in
the platform configuration:

```
module "platform" {
  source = "github.com/thoughtbot/flightdeck//aws/platform?ref=v0.9.0"

  # Other config

  custom_roles = {
    example-developer = module.sso_roles.by_name_without_path.DeveloperAccess
  }
}

module "sso_roles" {
  source = "github.com/thoughtbot/terraform-aws-sso-permission-set-roles?ref=v0.2.0"
}
```
