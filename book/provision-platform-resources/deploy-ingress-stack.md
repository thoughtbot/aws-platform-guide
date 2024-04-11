
## Deploy Ingress stack

::: caution
This is an advanced topic for platform engineers.
:::

In the infrastructure
[repository](#infrastructure-repository)
for the organization, you can create Terraform root modules for managing
ingress resources, including hosted zones, SSL certificates, load
balancers, target groups, and DNS aliases.

### Hosted Zones

In order to provision the ingress stack, you'll need at least one hosted
zone. For more information on configuring hosted zones, see [DNS
administration](#dns).

### Ingress Stack

![A diagram of the Ingress Stack](./images/ingress.png)

Create a root module for the ingress stack for each stage of the
software development lifecycle.

::: info
The [Flightdeck repository template](https://github.com/thoughtbot/flightdeck-template)
comes with configuration for the ingress stack.

</div>

</div>

```
infra/
  ingress/
    sandbox/
    production/
```

Flightdeck includes a [Terraform
module](https://github.com/thoughtbot/flightdeck/tree/main/aws/ingress) for provisioning an
entire ingress stack, including:

  - An [application load
    balancer](https://docs.aws.amazon.com/elasticloadbalancing/latest/application/introduction.html)
    for handling incoming requests.

  - An [ACM
    certificate](https://docs.aws.amazon.com/acm/latest/userguide/acm-overview.html)
    for encrypting requests using TLS.

  - A [Route 53
    alias](https://docs.aws.amazon.com/Route53/latest/DeveloperGuide/resource-record-sets-choosing-alias-non-alias.html)
    to publish a DNS address for the load balancer.

  - [Target
    groups](https://docs.aws.amazon.com/elasticloadbalancing/latest/application/load-balancer-target-groups.html)
    for the clusters in this ingress stack.

Apply the root module for each stage of the software development
lifecycle to provision the ingress stack.

Once the ingress stack is fully provisioned, you are ready to proceed
with [deploying the Flightdeck
platform](#deploy-flightdeck).
