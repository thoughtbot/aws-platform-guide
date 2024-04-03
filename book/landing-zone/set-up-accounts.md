
## Set up accounts

<div class="panel" style="background-color: #FFFAE6;border-width: 1px;">

<div class="panelContent" style="background-color: #FFFAE6;">

This is an advanced topic for platform engineers.

</div>

</div>

In order to fully deploy the thoughtbot platform, it's recommended to
set up the [conventional
accounts](https://thoughtbot.atlassian.net/wiki/spaces/APG/pages/10649900):

  - Operations: CI/CD pipelines and centralized monitoring

  - Network: centralized DNS and networking

  - Sandbox: staging and development workloads

  - Production: production workloads

  - Identity: permission sets and identity provider

  - Backup: backup policies and isolated vaults

<div class="confluence-information-macro confluence-information-macro-information">

<span class="aui-icon aui-icon-small aui-iconfont-info confluence-information-macro-icon"></span>

<div class="confluence-information-macro-body">

You can provision the standard accounts using the [landing zone
repository template](https://github.com/thoughtbot/aws-landing-zone-template).

</div>

</div>

It may take some time for all the required accounts to be provisioned.
Once all the accounts are fully enrolled, you are ready to create VPC
[networks.](#provision-networks)

![Diagram of recommended accounts with resources that belong in each
account](./images/accounts--1-.png)
