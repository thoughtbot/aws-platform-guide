
## Provision Networks

::: caution
This is an advanced topic for platform engineers.
:::

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

::: info
The [Flightdeck template
repository](https://github.com/thoughtbot/flightdeck-template) comes with
configuration for necessary networks.
:::

![A Digram of the Network setup](./images/networks.svg)
