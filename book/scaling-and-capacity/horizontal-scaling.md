
## Horizontal Scaling

You can scale your application horizontally by adding more pods to your
deployment. The simplest way is to specify the number of replicas in
your deployment manifest. This example will run three pods for a
deployment, balancing requests between them:

<div class="code panel pdl" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
apiVersion: apps/v1
kind: Deployment
metadata:
  name: example
spec:
  replicas: 3
```

</div>

</div>

### Autoscaling

Most applications experience fluctuations in traffic both seasonally and
throughout the day. You can use the horizontal pod autoscaler to
automatically add and remove replicas based on metrics observed in your
cluster. Here as an example which automatically scales deployment based
on the number of incoming requests per second:

<div class="code panel pdl" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
apiVersion: autoscaling/v2beta2
kind: HorizontalPodAutoscaler
metadata:
  name: example
spec:
  scaleTargetRef:
    apiVersion: apps/v1
    kind: Deployment
    name: example
  minReplicas: 2
  maxReplicas: 10
  metrics:
  - type: Pods
    pods:
      metric:
        name: http_requests_per_second
      target:
        type: AverageValue
        averageValue: 10
```

</div>

</div>
