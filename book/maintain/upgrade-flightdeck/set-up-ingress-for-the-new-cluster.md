
### Set up ingress for the new cluster

<div class="panel" style="background-color: #FFFAE6;border-width: 1px;">

<div class="panelContent" style="background-color: #FFFAE6;">

This is an advanced topic for platform engineers.

</div>

</div>

![AWS Ingress Migration](./images/aws-ingress-migration.png)

Edit the ingress stack for your sandbox clusters.

```
infra/
  ingress/
    sandbox/
```

Add your new cluster to the list of clusters and target groups.

```
module "ingress" {
  cluster_names = ["mycompany-sandbox-v1", "mycompany-sandbox-v2"]

  target_group_weights = {
    mycompany-sandbox-v1 = 100
    mycompany-sandbox-v2 = 0
  }
}
```

Apply this module to create the new target group. Using the above
weights, the target group will be created but will receive no traffic.
We will wait until workloads are fully deployed to shift traffic.
