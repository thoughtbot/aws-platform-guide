
## Cluster Errors

### Pod Stuck Terminating

This may be caused by a pod being scheduled on an unreachable node.

### Unreachable Nodes

If you see pods failing to schedule or terminate, you may have one or
more unreachable nodes. You can find unreachable nodes by running:

```
flightctl kubectl get node | grep NotReady
ip-1-2-3-4.us-east-1.compute.internal NotReady <none> 1d0h v1.18.9-eks-d1db3c
```

Note the name of the node and check its status:

```
flightctl kubectl describe node ip-1-2-3-4.us-east-1.compute.internal
```

#### Kubelet stopped posting node status

Find the underlying EC2 instance:

```
aws ec2 describe-instances \
  --filters Name=private-dns-name,Values=ip-1-2-3-4.us-east-1.compute.internal \
  --query "Reservations[*].Instances[*].InstanceId"
```

Terminate the instance:

```
aws ec2 terminate-instances --instance-ids i-01234567890abcedf
```

Watch your node list to verify the node is removed:

```
flightctl kubectl get node --watch
```

Once the node is removed, the pod will terminate successfully.
