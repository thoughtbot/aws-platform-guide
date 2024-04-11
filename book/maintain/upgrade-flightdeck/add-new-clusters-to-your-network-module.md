
### Add new clusters to your network module

In order to provision a new cluster, you will need to update the tags
for your VPC and other network resources so that the new Kubernetes
cluster will find them.

```
infra/
  network/
    sandbox/
```

Update your network module to include the name of the cluster you will
be creating.

```
module "network" {
  # Use the latest version of Flightdeck
  source = "github.com/thoughtbot/flightdeck//aws/network?ref=UPDATE"

  cluster_names          = [
    # Existing cluster
    "mycompany-sandbox-v1",

    # Add your new cluster
    "mycompany-sandbox-v2"
  ]

  # Other configuration
  name                   = "..."
}
```

Apply this module to update the tags for your network resources.
