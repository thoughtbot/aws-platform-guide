
## Set up accounts

::: caution
This is an advanced topic for platform engineers.
:::

In order to fully deploy the thoughtbot platform, it's recommended to
set up the [conventional
accounts](https://thoughtbot.atlassian.net/wiki/spaces/APG/pages/10649900):

  - Operations: CI/CD pipelines and centralized monitoring

  - Network: centralized DNS and networking

  - Sandbox: staging and development workloads

  - Production: production workloads

  - Identity: permission sets and identity provider

  - Backup: backup policies and isolated vaults

::: info
You can provision the standard accounts using the [landing zone
repository template](https://github.com/thoughtbot/aws-landing-zone-template).
:::

It may take some time for all the required accounts to be provisioned.
Once all the accounts are fully enrolled, you are ready to create VPC
[networks.](#provision-networks)

![Diagram of recommended accounts with resources that belong in each
account](./images/accounts--1-.png)
