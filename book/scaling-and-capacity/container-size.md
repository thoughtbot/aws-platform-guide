
## Container Size

Unlike some other platforms, containers are not constrained to a
predetermined size. When you deploy your application, you decide how
much RAM and CPU to allocate to each container. This is done using
[resource
requests](https://kubernetes.io/docs/concepts/configuration/manage-resources-containers/)
in your pod manifests. Here is an example container which requests
512MiB of RAM and expects to use half a CPU core:

<div class="code panel pdl" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
spec:
  containers:
  - name: main
    image: example:latest
    resources:
      requests:
        memory: 512Mi
        cpu: "500m"
```

</div>

</div>

We strongly recommend adding resource requests to all your containers.
This will tell the cluster how much space it should expect to allocate
to run your container.

### Vertical Autoscaling

Many applications experience fluctuation in memory or CPU requirements
as the application scales. Rather than manually tracking the resources
your containers require and updating your resource requests as the
application grows, you can tell the cluster to automatically adjust the
requirements using the vertical pod autoscaler, which is deployed as
part of the Flightdeck platform. Here is an example which will
automatically adjust the size of the pods for a deployment, but will
never scale it beyond 2 CPU cores or 4GiB of memory:

<div class="code panel pdl" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
apiVersion: autoscaling.k8s.io/v1
kind: VerticalPodAutoscaler
metadata:
  name: example
spec:
  targetRef:
    apiVersion: apps/v1
    kind: Deployment
    name: example
  updatePolicy:
    updateMode: Auto
  resourcePolicy:
    containerPolicies:
    - containerName: "main"
      maxAllowed:
        cpu: 2000m
        memory: 4Gi
```

</div>

</div>
