
# Provision Platform Resources

::: caution
This is an advanced topic for platform engineers.
:::

On AWS, thoughtbot uses [Control
Tower](https://aws.amazon.com/controltower/) to implement security best
practices and reliable workload isolation. This provides a quick
starting point for a multi-account setup while still allowing for
significant customization and expansion later.

Rather than managing individual IAM accounts, Control Tower makes it
easy to use [AWS SSO](https://aws.amazon.com/single-sign-on/) to manage
users centrally and integrate with existing identity stores like a
Google or Microsoft user directory.

We use [Customizations for Control
Tower](https://aws.amazon.com/solutions/implementations/customizations-for-aws-control-tower/)
to configure [account
baselines](https://docs.aws.amazon.com/controltower/latest/userguide/terminology.html)
and deploy [service control
policies](https://docs.aws.amazon.com/organizations/latest/userguide/orgs_manage_policies_scps.html).

We have a standardized account infrastructure we use to structure
organizations.

## Getting Started

To install the platform, you can follow these guides:

- [Provision Networks](#provision-networks)
- [Deploy Ingress stack](#deploy-ingress-stack)
- [Deploy Flightdeck](#deploy-flightdeck)
- [Deploy Grafana](#deploy-grafana)

## Installing without Control Tower

If you're using a single AWS account or not using Control Tower for
another reason, you'll need to create the baseline role and S3 backend
for Terraform by hand. Once these are in place, you can proceed with
[Provision Networks](#provision-networks).
