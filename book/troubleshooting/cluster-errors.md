## Pod Stuck Terminating

This may be caused by a pod being scheduled on an unreachable node.

## Unreachable Nodes

If you see pods failing to schedule or terminate, you may have one or
more unreachable nodes. You can find unreachable nodes by running:

<div class="code panel pdl" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
flightctl kubectl get node | grep NotReady
ip-1-2-3-4.us-east-1.compute.internal NotReady <none> 1d0h v1.18.9-eks-d1db3c
```

</div>

</div>

Note the name of the node and check its status:

<div class="code panel pdl" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
flightctl kubectl describe node ip-1-2-3-4.us-east-1.compute.internal
```

</div>

</div>

### Kubelet stopped posting node status

Find the underlying EC2 instance:

<div class="code panel pdl" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
aws ec2 describe-instances \
  --filters Name=private-dns-name,Values=ip-1-2-3-4.us-east-1.compute.internal \
  --query "Reservations[*].Instances[*].InstanceId"
```

</div>

</div>

Terminate the instance:

<div class="code panel pdl" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
aws ec2 terminate-instances --instance-ids i-01234567890abcedf
```

</div>

</div>

Watch your node list to verify the node is removed:

<div class="code panel pdl" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
flightctl kubectl get node --watch
```

</div>

</div>

Once the node is removed, the pod will terminate successfully.
