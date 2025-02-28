### Terraform Conventions

We use Terraform to maintain infrastructure as code.

- [Terraform Conventions](#terraform-conventions)
  - [State Backends](#state-backends)
  - [Filenames](#filenames)

#### State Backends

On AWS, we use the [S3
backend](https://www.terraform.io/docs/language/settings/backends/s3.html)
for Terraform state. We create a separate backend bucket for each AWS
account. Terraform state should be encrypted using unique,
customer-managed KMS keys. The key for each module's state should
reflect that module's path within the [infrastructure
repository](#infrastructure-repository).

You can use the [Terraform state backend Cloudformation
template](https://github.com/thoughtbot/cloudformation-terraform-state-backend)
to create a secure Terraform state backend for each AWS account. When
using Control Tower, you can use [Customizations for Control Tower](#launch-customizations-for-control-tower) to
create a Terraform state backend as part of the baseline for each
account.

::: info
The [landing zone template
repository](https://github.com/thoughtbot/aws-landing-zone-template) comes with
configuration for Terraform state backends.
:::

#### Filenames

|                |                                              |
| -------------- | -------------------------------------------- |
| `backend.tf`   | Configuration for the state backend          |
| `main.tf`      | Resources, data sources, modules, and locals |
| `outputs.tf`   | Outputs                                      |
| `providers.tf` | Configuration for Terraform providers        |
| `variables.tf` | Variables                                    |
| `versions.tf`  | Required Terraform and provider versions     |

**Formatting**

All files should be formatted using `terraform fmt`.
