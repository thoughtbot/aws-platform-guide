
## Provision Networks

<div class="panel" style="background-color: #FFFAE6;border-width: 1px;">

<div class="panelContent" style="background-color: #FFFAE6;">

This is an advanced topic for platform engineers.

</div>

</div>

In the infrastructure
[repository](#repository-conventions)
for the organization, you can create Terraform root modules for managing
VPCs and related networking resources:

```
infra/
  network/
    sandbox/
    production/
```

These networks can be provisioned using the [network Terraform
module](https://github.com/thoughtbot/flightdeck/tree/main/aws/network) from Flightdeck.

<div class="confluence-information-macro confluence-information-macro-information">

<span class="aui-icon aui-icon-small aui-iconfont-info confluence-information-macro-icon"></span>

<div class="confluence-information-macro-body">

The [Flightdeck template
repository](https://github.com/thoughtbot/flightdeck-template) comes with
configuration for necessary networks.

</div>

</div>

![A Digram of the Network setup](./images/networks.svg)
