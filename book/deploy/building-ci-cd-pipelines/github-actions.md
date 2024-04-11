
### GitHub Actions

Some of our clients deploy applications using GitHub Actions.

#### Continuous Integration

For each [application repository](#), resources related to the CI Job
should be provisioned in the [Operations
Account](#aws-accounts).

```
infra/
  applications/
    APPLICATION/
      operations/
```

In order to build Docker images for an application, you'll need:

  - A [Dockerfile](https://docs.docker.com/engine/reference/builder/).
    This can be kept in the [application repository](#).

  - An [ECR
    repository](https://docs.aws.amazon.com/AmazonECR/latest/userguide/Repositories.html).
    You can use the [ECR repository Terraform
    module](https://github.com/thoughtbot/terraform-eks-cicd/tree/main/modules/ecr-repository)
    to set this up.

    ```
    module "ecr_repository" {
      source = "github.com/thoughtbot/terraform-eks-cicd//modules/ecr-repository?ref=v0.1.0"

      name = "example-org/example-app"

      # AWS accounts allowed to pull this image
      workload_account_ids = [
        "123456789010", # Sandbox account ID
        "123456789010", # Production account ID
      ]
    }
    ```

  - An IAM OIDC provider which trusts your GitHub Actions workflow. If
    you used the [landing zone
    template](https://github.com/thoughtbot/aws-landing-zone-template),
    one will already be created in the Operations account and you can
    locate its ARN using the SSM parameter
    /GitHubActions/OIDCProviderArn.

  - An IAM role that can be assumed by GitHub using OIDC and access ECR
    in order to push your built Docker image.

    ```
    module "ecr_role" {
      source = "github.com/thoughtbot/terraform-eks-cicd//modules/github-actions-ecr-role?ref=v0.1.1"

      allow_github_pull_requests = true
      ecr_repositories           = [module.ecr_repository.name]
      github_branches            = ["main", "production"]
      github_organization        = "example-org"
      github_repository          = "example-app"
      iam_oidc_provider_arn      = data.aws_ssm_parameter.iam_oidc_provider_arn.value
      name                       = "github-actions-ecr-example"

      depends_on = [module.ecr_repository]
    }

    data "aws_ssm_parameter" "iam_oidc_provider_arn" {
      name = "/GitHubActions/OIDCProviderArn"
    }
    ```

  - Once this root module is applied, create GitHub Actions job(s) to
    build Docker images and generate manifests. These Actions can be
    triggered when pull requests are opened by [defining
    triggers](https://docs.github.com/en/actions/using-workflows/events-that-trigger-workflows)
    in the Job itself.

GitHub Actions will start building and pushing Docker images for your
application to ECR whenever developers open pull requests.

#### Continuous Deployment

One of the [application
roles](#application-roles) for
each stage of the software development lifecycle is a deploy role, which
can be used by CI/CD pipelines to deploy new versions of the
application. You can assume this role from your deployment GitHub
workflow using the [configure-aws-credentials
action](https://github.com/aws-actions/configure-aws-credentials).

You can use the [k8s-bake action](https://github.com/Azure/k8s-bake) to
generate Kubernetes manifests for your application. There is a
[helm-rails](https://github.com/thoughtbot/helm-charts/tree/main/charts/helm-rails)
Helm chart you can use with this action to deploy a Rails application.

Finally, you can use the [k8s-deploy
action](https://github.com/Azure/k8s-deploy) to apply the updated
manifests to your cluster.

Once configured, deployments will kick off in GitHub Actions whenever
you push code to the appropriate branch in your source repository.
