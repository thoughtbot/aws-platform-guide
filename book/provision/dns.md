## DNS

![A diagram of our standard DNS setup](./images/dns.svg)

In the [infrastructure repository](#repository-conventions)
for the organization, you can create Terraform root modules for managing
hosted zones for root domains and subdomains:

```
infra/
  hosted-zones/
    example.com/
    production.example.com/
    staging.example.com/
```

You can use `aws_route53_zone` to manage root domains. These hosted
zones should be placed in the [Network account](#aws-accounts).

### Account Subdomains

In order to control updates to public DNS while still allowing workloads
to publish updates to their endpoint addresses, it is recommended that
you create a hosted zone for each stage of the software development life
cycle, such as staging and production.

You can use the
[terraform-route-53-delegated-subdomain](https://github.com/thoughtbot/terraform-route-53-delegated-subdomain)
Terraform module to provision these subdomain zones. The subdomain
hosted zones should be placed in [Workload accounts](#aws-accounts).

You can then use `aws_route53_record` from the Network account to alias
public addresses to the proper subdomain address.
