We use Terraform to maintain infrastructure as code.

<div class="toc-macro rbtoc1710773636907">

  - [State Backends](#TerraformConventions-StateBackends)
  - [Filenames](#TerraformConventions-Filenames)

</div>

## **State Backends**

On AWS, we use the [S3
backend](https://www.terraform.io/docs/language/settings/backends/s3.html)
for Terraform state. We create a separate backend bucket for each AWS
account. Terraform state should be encrypted using unique,
customer-managed KMS keys. The key for each module's state should
reflect that module's path within the [infrastructure
repository](../conventions-and-expectations/repository-conventions/infrastructure-repository.md).

You can use the [Terraform state backend Cloudformation
template](https://github.com/thoughtbot/cloudformation-terraform-state-backend)
to create a secure Terraform state backend for each AWS account. When
using Control Tower, you can use customizations for Control Tower to
create a Terraform state backend as part of the baseline for each
account.

<div class="confluence-information-macro confluence-information-macro-information">

<span class="aui-icon aui-icon-small aui-iconfont-info confluence-information-macro-icon"></span>

<div class="confluence-information-macro-body">

The [landing zone template
repository](../reference/templates/landing-zone-template.md) comes with
configuration for Terraform state backends.

</div>

</div>

## **Filenames**

<div class="table-wrap">

|                |                                              |
| -------------- | -------------------------------------------- |
| `backend.tf`   | Configuration for the state backend          |
| `main.tf`      | Resources, data sources, modules, and locals |
| `outputs.tf`   | Outputs                                      |
| `providers.tf` | Configuration for Terraform providers        |
| `variables.tf` | Variables                                    |
| `versions.tf`  | Required Terraform and provider versions     |

</div>

**Formatting**

All files should be formatted using `terraform fmt`.
