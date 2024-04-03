
#### Google Sign In

You will need to be an administrator for the Google domain to follow
these instructions. This may require setting up a screen sharing session
with somebody else who has access.

<div class="confluence-information-macro confluence-information-macro-note">

<span class="aui-icon aui-icon-small aui-iconfont-warning confluence-information-macro-icon"></span>

<div class="confluence-information-macro-body">

There is a difference between an admin for Google Cloud and Google
Workspace. Make sure the owner of the service account is an admin for
Google Workspace and are able to access the Google Admin Console.

</div>

</div>

If you’re using Google as a sign-in provider, you’ll also want to deploy
the sso-sync Lambda to automatically provision user accounts in Identity
Center. Otherwise, users will need to be manually added in both Google
and AWS.

1.  Set Google as an external identity provider using the above guide.

2.  You should have a Identity account for managing SSO identities. This
    is created if you’re using the `accounts.yaml` file from the
    template.

3.  Deploy the sso-sync Lambda to the Identity account using the
    instructions below.

### SSOSync Lambda

From the Google cloud console, [create a new
project](https://console.cloud.google.com/projectcreate) for the
Lambda’s credentials. Give it a name that makes it clear why the
project exists, such as “aws-google-sso-sync.”

#### To be performed by Google Admin:

Follow the [Google
tutorial](https://developers.google.com/workspace/guides/create-credentials#service-account)
to create a service account:

1.  In the Google Cloud console, go to [IAM & Admin \> Service
    Accounts](https://console.cloud.google.com/iam-admin/serviceaccounts).

2.  Click **Create service account**.

3.  Give your service account a name and description, then click
    **Create and continue**.

4.  Click **Continue**.

5.  Click **Done**.

Create credentials for your service account:

1.  Click **Keys** \> **Add key** \> **Create new key**.

2.  Select **JSON**, then click **Create**.

3.  Make a note of the downloaded credentials file.

4.  Click **Close**.

5.  Expand **Advanced Settings**.

6.  Save the **Client ID** under **Domain-wide Delegation.**

Now enable the Admin SDK:

1.  In the Google Cloud console, go to [APIs & Services \> Enabled APIs
    & services](https://console.cloud.google.com/apis/dashboard).

2.  Click **Enable APIs and Services**.

3.  Search for **Admin SDK API**.

4.  Click the **Enable** button.

Now enable domain-wide delegation for your service account:

1.  From the Google admin console, go to [Security \> API Controls \>
    Domain-wide
    Delegation](https://admin.google.com/ac/owl/domainwidedelegation).

2.  Click **Add new**.

3.  Fill in the Client ID for the service account you saved earlier.

4.  Under OAuth scopes, specify the following:  
    <https://www.googleapis.com/auth/admin.directory.group.readonly>  
    <https://www.googleapis.com/auth/admin.directory.group.member.readonly>  
    <https://www.googleapis.com/auth/admin.directory.user.readonly>

5.  Click **Authorize**.

#### To be performed by Infra Dev:

Enable SCIM for your Identity Center directory:

1.  Sign into the Identity account from your AWS landing zone.

2.  From [AWS IAM Identity Center
    settings](https://us-east-1.console.aws.amazon.com/singlesignon/home?region=us-east-1#!/instances/7223617cd32e4b1d/settings),
    within the **Automatic provisioning** information box, choose
    **Enable**.

3.  Save the displayed **SCIM endpoint** and **Access token**.

4.  Create a new Terraform module in the infrastructure repository under
    `sso-sync/secrets` to store the credentials:
    
    <div class="code panel pdl" style="border-width: 1px;">
    
    <div class="codeContent panelContent pdl">
    
    ``` syntaxhighlighter-pre
    module "secret" {
      source = "github.com/thoughtbot/terraform-aws-secrets//secret?ref=v0.4.0"
    
      description = "Secrets for deploying the AWS/Google SSO Sync Lambda"
      name        = "aws-google-sso-sync"
    
      initial_value = jsonencode({
        GoogleCredentials       = ""
        SCIMEndpointAccessToken = ""
        SCIMEndpointUrl         = ""
      })
    }
    ```
    
    </div>
    
    </div>

5.  Apply the Terraform module.

6.  From [AWS Secrets
    Manager](https://us-east-1.console.aws.amazon.com/secretsmanager/listsecrets),
    find the created secret.

7.  Click **Retrieve Secret Value** and then click **Edit**.

8.  Copy the contents of the JSON credentials file you downloaded into
    the **GoogleCredentials field.**

9.  Fill in the **SCIMEndpointAccessToken and SCIMEndpointUrl** values
    you saved earlier.

10. Click **Save**.

11. Create a new Terraform module in the infrastructure repository under
    `sso-sync/lambda` to deploy the SSOSync Lambda:
    
    <div class="code panel pdl" style="border-width: 1px;">
    
    <div class="codeContent panelContent pdl">
    
    ``` syntaxhighlighter-pre
    module "lambda" {
      source = "github.com/thoughtbot/terraform-aws-google-sso?ref=v0.1.0"
    
      google_admin_email         = "google-admin@example.com"
      google_credentials         = local.secrets.GoogleCredentials
      google_group_match         = "email:aws-*"
      name                       = "aws-google-sso-sync"
      scim_endpoint_access_token = local.secrets.SCIMEndpointAccessToken
      scim_endpoint_url          = local.secrets.SCIMEndpointUrl
      semantic_version           = "2.0.2"
    }
    
    locals {
      secrets = jsondecode(
        data.aws_secretsmanager_secret_version.sso_sync.secret_string
      )
    }
    
    data "aws_secretsmanager_secret_version" "sso_sync" {
      secret_id = "aws-google-sso-sync"
    }
    ```
    
    </div>
    
    </div>

12. Apply the module.

IAM Identity Center will now automatically synchronize matched groups
and users from your Google domain.

## Updating the SCIM Token

You will need to regularly update the SCIM token as it expires to
continually provision users and groups: [Update SCIM
tokens](#update-scim-tokens)
