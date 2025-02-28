### Create a new sandbox cluster

::: caution
This is an advanced topic for platform engineers.
:::

Create a new root module in the `cluster` directory of your
infrastructure repository.

```
infra/
  cluster/
    sandbox-v2/
```

You can copy the configuration from the previous version of your
cluster, updating the versions of Flightdeck and Kubernetes. We
recommend running the latest version of Kubenetes [supported by EKS](https://docs.aws.amazon.com/eks/latest/userguide/kubernetes-versions.html).

```
module "cluster" {
  # Update this to the latest version of Flightdeck
  source = "github.com/thoughtbot/flightdeck//aws/cluster?ref=v0.12.1"

  # Use the name of your previous cluster and bump the version
  name        = "mycompany-sandbox-v2"

  # Use the latest version of Kubernetes supported by EKS.
  k8s_version = "UPDATE"

  # Copy existing configuration for node groups
  node_groups = {}
}
```

Apply this module to provision a cluster and node groups running the
latest supported versions.
