<div class="panel" style="background-color: #FFFAE6;border-width: 1px;">

<div class="panelContent" style="background-color: #FFFAE6;">

This is an advanced topic for platform engineers.

</div>

</div>

In the infrastructure
[repository](../conventions-and-expectations/repository-conventions/infrastructure-repository.md)
for the organization, you can create Terraform root modules for managing
ingress resources, including hosted zones, SSL certificates, load
balancers, target groups, and DNS aliases.

## Hosted Zones

In order to provision the ingress stack, you'll need at least one hosted
zone. For more information on configuring hosted zones, see [DNS
administration](../provision/dns.md).

## Ingress Stack

![ingress](./ingress.png)

Create a root module for the ingress stack for each stage of the
software development lifecycle.

<div class="confluence-information-macro confluence-information-macro-information">

<span class="aui-icon aui-icon-small aui-iconfont-info confluence-information-macro-icon"></span>

<div class="confluence-information-macro-body">

The [Flightdeck repository
template](../reference/templates/flightdeck-template.md) comes with
configuration for the ingress stack.

</div>

</div>

<div class="code panel pdl" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
infra/
  ingress/
    sandbox/
    production/
```

</div>

</div>

Flightdeck includes a [Terraform
module](../reference/modules/flightdeck--ingress.md) for provisioning an
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
platform](../provision-platform-resources/deploy-flightdeck.md).
