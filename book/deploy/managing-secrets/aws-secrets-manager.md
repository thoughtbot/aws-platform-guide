[AWS Secrets
Manager](https://docs.aws.amazon.com/secretsmanager/latest/userguide/intro.html)
is a service for storing, securing, updating, and accessing secrets like
passwords, API keys, and cryptographic certificates. A secret can
consist of either a single blob of data, or a JSON object consisting of
key value string pairs.

You can use Terraform to provision a Secrets Manager secret to store
your secret data.

  - For things like a database URL, we use a Lambda rotation function
    which creates a new password in the database each month and then
    updates the secret. (For example, see
    [thoughtbot/terraform-aws-databases/rds-postgres/admin-login](https://github.com/thoughtbot/terraform-aws-databases/tree/main/rds-postgres/admin-login))

  - <span class="inline-comment-marker" data-ref="eca7a0a0-b83d-49ab-b1a5-05ea4964344a">For
    things like the Rails secret base</span>, we generate a random one
    in Terraform and populate it there.
    
    <div class="code panel pdl" style="border-width: 1px;">
    
    <div class="codeContent panelContent pdl">
    
    ``` syntaxhighlighter-pre
    resource "random_password" "secret_key_base" {
      length  = 32
      special = false
    }
    
    module "rails_secret" {
      source = "github.com/thoughtbot/terraform-aws-secrets//secret?ref=v0.4.0"
    
      description = "Secrets for the Rails application"
      name        = "example-app-secret"
    
      initial_value = jsonencode({
        SECRET_KEY_BASE = random_password.secret_key_base.result
      })
    }
    ```
    
    </div>
    
    </div>

  - For external tokens that we can't control, we create an empty secret
    in Terraform (using
    [thoughtbot/terraform-aws-secrets/secret](https://github.com/thoughtbot/terraform-aws-secrets/tree/main/secret)
    as source) and populate it by hand in AWS Management Console.
    
    <div class="code panel pdl" style="border-width: 1px;">
    
    <div class="codeContent panelContent pdl">
    
    ``` syntaxhighlighter-pre
    module "prismic_secret" {
      source = "github.com/thoughtbot/terraform-aws-secrets//secret?ref=v0.4.0"
    
      description = "Secrets for accessing the Prismic API"
      name        = "example-prismic"
    
      initial_value = jsonencode({
        # Fill this in using the SecretsManager UI or CLI
        PRISMIC_ACCESS_TOKEN = ""
      })
    }
    ```
    
    </div>
    
    </div>

Once you’ve safely stored your secret using Secrets Manager, you can
[add secrets to pods as environment variables or
files](../../../deploy/managing-secrets/mounting-secrets.md).
