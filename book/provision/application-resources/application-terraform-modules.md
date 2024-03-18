Application should be provisioned using Terraform. For each application
that requires resources in AWS, create a Terraform root module for each
account in which it will be deployed.

<div class="code panel pdl" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
infra/
  applications/
    example/
      sandbox/
      production/
```

</div>

</div>

To ensure that staging and production match, you can encapsulate
stateful resources into a module:

<div class="code panel pdl" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
infra/
  applications/
    example/
      modules/
        state/
```

</div>

</div>

You can then this module to provision resources in each account. You can
also use the Flightdeck application-config module to set up the
namespace, service account, secrets, and role bindings in your cluster:

<div class="code panel pdl" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
# applications/example/sandbox/main.tf

module "staging" {
  source = "../modules/state"

  cluster_names = [data.aws_eks_cluster.sandbox_v1.name]
  environment   = "staging"

  s3_bucket = "example-staging-activestorage"

  redis_sidekiq_name      = "example-staging-sidekiq-redis-orange"
  redis_sidekiq_node_type = "cache.t4g.micro"

  postgres_identifier     = "example-staging-strawberry"
  postgres_instance_class = "db.t4g.small"
}

module "staging_sandbox_v1" {
  providers = { kubernetes = kubernetes.sandbox_v1 }
  source    = "github.com/thoughtbot/flightdeck//aws/application-config"

  namespace               = module.staging.namespace
  secrets_manager_secrets = module.staging.secrets_manager_secrets
  pod_service_account     = module.staging.service_account_name
  pod_iam_role            = module.staging.pod_role_arn
}

data "aws_eks_cluster" "sandbox_v1" {
  name = "example-sandbox-v1"
}
```

</div>

</div>
