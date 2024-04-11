
## Deploy Flightdeck

<div class="panel" style="background-color: #FFFAE6;border-width: 1px;">

<div class="panelContent" style="background-color: #FFFAE6;">

This is an advanced topic for platform engineers.

</div>

</div>

[Flightdeck](https://github.com/thoughtbot/flightdeck) is a platform for
deploying and managing applications on Kubernetes. Flightdeck consists
of Terraform modules for deploying a curated set of preconfigured open
source projects and AWS products.

<div class="confluence-information-macro confluence-information-macro-information">

<span class="aui-icon aui-icon-small aui-iconfont-info confluence-information-macro-icon"></span>

<div class="confluence-information-macro-body">

The [Flightdeck template
repository](https://github.com/thoughtbot/flightdeck-template) comes with
configuration for clusters and the platform.

</div>

</div>

### Clusters

In order to deploy Flightdeck, you'll first need Kubernetes clusters. On
AWS, Flightdeck is designed to deploy to AWS's EKS platform. Flightdeck
contains a [cluster Terraform
module](https://github.com/thoughtbot/flightdeck/tree/main/aws/cluster) capable of setting
up compatible EKS clusters.

Create a root module for each phase of the software development
lifecycle to deploy an [EKS
cluster](https://docs.aws.amazon.com/eks/latest/userguide/clusters.html)
and [managed node
groups](https://docs.aws.amazon.com/eks/latest/userguide/managed-node-groups.html).

```
infra/
  cluster/
    sandbox-v1/
    production-v1/
```

### Platform

You are now ready to deploy Flightdeck for the sandbox and production
clusters using the [workload platform
module](https://github.com/thoughtbot/flightdeck/tree/main/aws/platform).

```
infra/
  platform/
    sandbox-v1/
    production-v1/
```

Once these modules are applied, you can deploy
[Grafana](#deploy-grafana) for
monitoring.
