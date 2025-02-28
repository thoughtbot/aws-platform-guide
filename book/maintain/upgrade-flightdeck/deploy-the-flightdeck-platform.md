### Deploy the Flightdeck platform

::: caution
This is an advanced topic for platform engineers.
:::

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
  source = "github.com/thoughtbot/flightdeck//aws/platform?ref=v0.12.1"

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
