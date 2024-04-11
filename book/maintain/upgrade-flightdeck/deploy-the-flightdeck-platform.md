
### Deploy the Flightdeck platform

<div class="panel" style="background-color: #FFFAE6;border-width: 1px;">

<div class="panelContent" style="background-color: #FFFAE6;">

This is an advanced topic for platform engineers.

</div>

</div>

Create a new root module to deploy the Flightdeck platform for the
sandbox cluster using the <span>workload platform module</span>.

```
infra/
  platform/
    sandbox-v1
```

You can copy the configuration for the previous version of the platform,
updating the version of Flightdeck.

```
module "platform" {
  # Use the latest version of Flightdeck
  source = "github.com/thoughtbot/flightdeck//aws/platform?ref=UPDATE"

  # Copy configuration from the previous deployment
  cluster_name = data.aws_eks_cluster.this.name
  admin_roles = []
  domain_names = []
}

data "aws_eks_cluster" "this" {
  # Use the name of the cluster you created in the previous step
  name = "mycompany-sandbox-v2"
}
```

Apply this module to install the necessary components on your new
cluster.
