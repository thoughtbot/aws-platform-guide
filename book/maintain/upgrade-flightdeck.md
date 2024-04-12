## Upgrade Flightdeck

We recommend using a blue/green strategy for upgrading your cluster.
Updating Kubernetes in-place can result in interruptions to running workloads.
Updating Kubernetes will also frequently require updates to the Helm charts
deployed to the cluster, which introduces further risk. By creating a new
cluster when updating cluster components, you can ensure that workloads run
properly on the new infrastructure before shifting production traffic and
workloads.
