
### Application Secrets

Application secrets can be stored in AWS Secrets Manager and injected as
environment variables or files into running applications.

Secrets like encryption keys or API keys should be kept separate from
configuration values like host name or time zone.

When storing application secrets:

  - Use a separate Secrets Manager secret for each service for which you
    need a secret value

  - Use a different secret value for each environment like staging and
    production

  - Don’t copy values between secrets - if secret values must be reused,
    store them centrally and allow access to the services which need
    them

Following these guidelines will limit the damage if secrets are partly
disclosed and will make recovery from secret disclosure easier.

For each secret you create, you will need to give your application’s
service role permission to access the secret. You can do using IAM
policies in Terraform:

<div class="code panel pdl" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
module "rails_secret" {
  source = "github.com/thoughtbot/terraform-aws-secrets//secret?ref=v0.4.0"

  description = "Secrets for the Rails application"
  name        = "example-app-secret"

  initial_value = jsonencode({
    SECRET_KEY_BASE = random_password.secret_key_base.result
  })
}

module "service_policy" {
  source = "github.com/thoughtbot/flightdeck//aws/service-account-policy?ref=v0.9.0"

  name       = "example-app-service"
  role_names = [module.service_role.name]

  policy_documents = concat(
    module.rails_secret.policy_json,
    # Other secrets or policies
  )
}
```

</div>

</div>

For more information on using Secrets Manager secrets, see [Managing
Secrets](#managing-secrets) in the Deploy section.
