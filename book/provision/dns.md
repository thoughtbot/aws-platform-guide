![](attachments/11173932/11206829.svg?width=566)

In the [infrastructure
repository](../conventions-and-expectations/repository-conventions.md)
for the organization, you can create Terraform root modules for managing
hosted zones for root domains and subdomains:

<div class="code panel pdl" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
infra/
  hosted-zones/
    example.com/
    production.example.com/
    staging.example.com/
```

</div>

</div>

You can use `aws_route53_zone` to manage root domains. These hosted
zones should be placed in the [Network
account](../conventions-and-expectations/account-conventions.md).

## Account Subdomains

In order to control updates to public DNS while still allowing workloads
to publish updates to their endpoint addresses, it is recommended that
you create a hosted zone for each stage of the software development life
cycle, such as staging and production.

You can use the
[terraform-route-53-delegated-subdomain](https://github.com/thoughtbot/terraform-route-53-delegated-subdomain)
Terraform module to provision these subdomain zones. The subdomain
hosted zones should be placed in [Workload
accounts](../conventions-and-expectations/account-conventions.md).

You can then use `aws_route53_record` from the Network account to alias
public addresses to the proper subdomain address.
