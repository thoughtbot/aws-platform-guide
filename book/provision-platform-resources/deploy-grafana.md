
## Deploy Grafana

<div class="panel" style="background-color: #FFFAE6;border-width: 1px;">

<div class="panelContent" style="background-color: #FFFAE6;">

This is an advanced topic for platform engineers.

</div>

</div>

We use [Grafana](https://grafana.com/) to monitor infrastructure and
applications. You can use AWS's managed services to deploy centralized
Prometheus and Grafana instances.

*Note for single account deployments: AWS Managed Grafana requires
either AWS SSO or a SAML provider to sign in. If you're not using an AWS
Organization with SSO enabled, you'll need a SAML provider to continue.*

### AWS Managed Prometheus

Flightdeck can forward time series data from its federated Prometheus
instance to an AWS Managed Prometheus instance for long-term storage.
This instance can also be used as a data source for AWS Managed Grafana.

Create a root module for your Prometheus workspace:

<div class="code panel pdl" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
infra/
  monitoring/
    prometheus-workspace/
```

</div>

</div>

Apply [prometheus-workspace
module](https://github.com/thoughtbot/flightdeck/tree/main/aws/prometheus-workspace)
from Flightdeck in the [Operations
account](../conventions-and-expectations/account-conventions.md).

Update your production workload platform configuration to use the
Prometheus workspace:

<div class="code panel pdl" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
module "workload_platform" {
  # Match the value from the monitoring/prmetheus-workspace module
  prometheus_workspace_name = "flightdeck"

  # Use the account ID for the Operations account
  monitoring_account_id = "123456789012"
}
```

</div>

</div>

Apply the workload platform module to start writing to the Prometheus
workspace.

### AWS Managed Grafana

AWS provides managed Grafana instances, but there is currently no
support for deploying Grafana using Terraform or Cloudformation, so you
need to create the workspace through the AWS Console.

#### Creating the Workspace

In the Operations account:

  - TODO: TF module for the following configs
    
      - Create a workspace named "Flightdeck'.
    
      - Enable SSO or SAML.
    
      - Use service-managed permissions.
    
      - Enable Managed Service for Prometheus and CloudWatch.
    
      - Enable Amazon SNS.
    
      - TODO: document about organization access type + OUs

  - Manual steps:
    
      - Set yourself as an admin of the workspace.
    
      - Add an SSO group to the workspace.

TODO: Order of operation for prometheus workspace, grafana workspace,
auth and data sources

#### Setting up Dashboards

  - From the Grafana workspace in the AWS Management Console, sign into
    the managed Grafana instance.

  - Under Settings, select API Keys.

  - Create a new Admin API key named "Terraform" that expires after 30
    days.

  - Copy the API key.

  - TODO: What to do with the copied API key

Once these modules are applied, the platform is fully deployed and you
can proceed to build [CI/CD
pipelines](https://mission-control.thoughtbot.com/branch/main/aws/book/admin/cicd.html).
