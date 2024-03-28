## Provision Networks

<div class="panel" style="background-color: #FFFAE6;border-width: 1px;">

<div class="panelContent" style="background-color: #FFFAE6;">

This is an advanced topic for platform engineers.

</div>

</div>

In the infrastructure
[repository](../conventions-and-expectations/repository-conventions.md)
for the organization, you can create Terraform root modules for managing
VPCs and related networking resources:

<div class="code panel pdl" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
infra/
  network/
    sandbox/
    production/
```

</div>

</div>

These networks can be provisioned using the [network Terraform
module](../reference/modules/flightdeck--network.md) from Flightdeck.

<div class="confluence-information-macro confluence-information-macro-information">

<span class="aui-icon aui-icon-small aui-iconfont-info confluence-information-macro-icon"></span>

<div class="confluence-information-macro-body">

The [Flightdeck template
repository](../reference/templates/flightdeck-template.md) comes with
configuration for necessary networks.

</div>

</div>

![networks](./images/networks.svg)
