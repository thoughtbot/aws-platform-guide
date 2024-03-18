Some of our clients deploy applications using
[CodePipeline](https://docs.aws.amazon.com/codepipeline/latest/userguide/welcome.html)
and
[CodeBuild](https://docs.aws.amazon.com/codebuild/latest/userguide/welcome.html).

![](attachments/14287288/14745728.png)

## Common Resources

In the [infrastructure
repository](../../conventions-and-expectations/repository-conventions/infrastructure-repository.md),
create a root module for common CI/CD resources:

<div class="code panel pdl" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
infra/
  cicd-common/
```

</div>

</div>

In order to deploy CI/CD pipelines on AWS, you will need the following:

  - An artifact bucket for your organization's build artifacts. You can
    provision one using the [artifact bucket Terraform
    module](https://github.com/thoughtbot/terraform-eks-cicd/tree/main/modules/artifact-bucket).

  - A CodeBuild credential to trigger builds when pull requests are
    opened. You can set this up using the [CodeBuild credential
    Terraform
    module](https://github.com/thoughtbot/terraform-eks-cicd/tree/main/modules/codebuild-credential).

  - A CodeStar connection to trigger pipeline runs when pull requests
    are merged. You can set this up using the [CodeStar connection
    Terraform
    module](https://github.com/thoughtbot/terraform-eks-cicd/tree/main/modules/codestar-connection).

These resources should be provisioned in the [Operations
account](../../conventions-and-expectations/account-conventions.md).

After adding these modules to your root module, apply the module. Once
the module has completed successfully, you will need to sign into the
AWS Management Console. Find the pending GitHub connection and follow
the instructions in the [AWS Developer Tools Console User
Guide](https://docs.aws.amazon.com/dtconsole/latest/userguide/connections-update.html)
to complete the connection.

## Continuous Integration

For each [application
repository](../../conventions-and-expectations/repository-conventions/application-repository.md),
create CodeBuild projects to build Docker images and generate manifests.
These projects can be triggered when pull requests are opened using
webhooks. Build projects and related resources should be provisioned in
the [Operations
account](../../conventions-and-expectations/account-conventions.md).

<div class="code panel pdl" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
infra/
  ci/
    APPLICATION/  
```

</div>

</div>

In order to build Docker images for an application, you'll need:

  - A [Dockerfile](https://docs.docker.com/engine/reference/builder/).
    This can be kept in the [application
    repository](../../conventions-and-expectations/repository-conventions/application-repository.md).

  - A
    [buildspec](https://docs.aws.amazon.com/codebuild/latest/userguide/build-spec-ref.html)
    for building and pushing Docker images. This can also be kept in the
    application repository.

  - An [ECR
    repository](https://docs.aws.amazon.com/AmazonECR/latest/userguide/Repositories.html).
    You can use the [ECR repository Terraform
    module](https://github.com/thoughtbot/terraform-eks-cicd/tree/main/modules/ecr-repository)
    to set this up.

  - A [CodeBuild
    project](https://docs.aws.amazon.com/codebuild/latest/userguide/working-with-build-projects.html)
    for building Docker images. You can create one using the [ECR
    project Terraform
    module](https://github.com/thoughtbot/terraform-eks-cicd/tree/main/modules/ecr-project).

  - A [manifests
    repository](../../conventions-and-expectations/repository-conventions/manifest-repository.md)
    to store configuration and manifests for your application.

  - A buildspec for generating application manifests. This can be stored
    in the manifests repository and should contain
    [Kustomize](https://kustomize.io/) or [Helm](https://helm.sh/)
    commands for generating Kubernetes manifests. This buildspec must
    produce YAML files as artifacts that can later be applied to the
    cluster to deploy the application.

  - A [CodeBuild
    project](https://docs.aws.amazon.com/codebuild/latest/userguide/working-with-build-projects.html)
    for generating application manifests. You can create one using the
    [manifests project Terraform
    module](https://github.com/thoughtbot/terraform-eks-cicd/tree/main/modules/manifests-project).

Once this root module is applied, CodeBuild will start building and
storing Docker images for your application whenever developers open pull
requests.

## Continuous Deployment

For each stage of the software development lifecycle, create a
CodePipeline pipeline to deploy the latest images and manifests to the
cluster. This pipeline can be triggered when pull requests are merged
using webhooks. Pipelines should be provisioned in the [Operations
account](../../conventions-and-expectations/account-conventions.md).

<div class="code panel pdl" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
infra/
  cd/
    APPLICATION/
```

</div>

</div>

  - An IAM role for deploying to each cluster. You can create one using
    the [deploy role Terraform
    module](https://github.com/thoughtbot/terraform-eks-cicd/tree/main/modules/deploy-role).
    This role must be provisioned in the same [Workload
    account](../../conventions-and-expectations/account-conventions.md)
    as the cluster.

  - A buildspec for applying manifests to the cluster. You can use
    kubectl and other commands to apply the manifests generated by your
    manifests project.

  - A CodeBuild project for applying manifests to the cluster. You can
    set one up using the [deploy-project Terraform
    module](https://github.com/thoughtbot/terraform-eks-cicd/tree/main/modules/deploy-project).

  - A CodePipeline pipeline for deploying the latest Docker images and
    manifests to the cluster. You can create pipelines using the
    [cicd-pipeline Terraform
    module](https://github.com/thoughtbot/terraform-eks-cicd/tree/main/modules/cicd-pipeline).

Once a pipeline is provisioned, the CodeBuild projects for building
Docker images and manifests will be triggered whenever a pull request is
merged. The artifacts from the manifest project will be provided to the
deploy project, which can apply the manifests to the cluster.
