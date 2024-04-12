## Cluster Capacity

Compute capacity - that is, the ability to provision memory and CPU for
the containers running in your pods - is provided by [EKS managed node
groups](https://docs.aws.amazon.com/eks/latest/userguide/managed-node-groups.html).
These node groups are made up of EC2 instances which run in the VPC in
your AWS account.

When you schedule a pod to run on the Flightdeck platform, the
autoscaler will find an existing node with enough remaining CPU and
memory to run your pod’s containers. If you don’t have any remaining
nodes with enough space, an additional EC2 instance will be provisioned
and join the cluster. If the autoscaler detects unused space in your
cluster, it will shut down EC2 instances which aren’t necessary.

![Node Groups](./images/node-groups.png)
