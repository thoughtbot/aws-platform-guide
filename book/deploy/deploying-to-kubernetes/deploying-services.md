
### Deploying Services

Deploying a service for a pod consists of two resources: a service to
describe the service to be provided, and a workload to describe the pods
which will fulfill the service. The most common kind of workload is a
deployment. You can create services and deployments by authoring
Kubernetes manifests.

#### Services

Services must provide a selector to describe which pods will fulfill
this service's requests. If your service exposes any ports, they will
also be described here.

<div class="code panel pdl" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
apiVersion: v1
kind: Service
metadata:
  # Labels for a service do not need to match its pods, but can overlap.
  labels:
    app.kubernetes.io/name: example
    app.kubernetes.io/component: web
  name: example-web
  namespace: default
spec:

  # Ports describe how the port will be exposed within the cluster as well as
  # the port on a pod to which traffic will be routed. If your service doesn't
  # expose any ports, this section is not required.
  ports:
  - name: http
    port: 3000
    protocol: TCP
    targetPort: http

  # The selector contains labels which must match for a pod to be considered
  # part of this service. These labels will usually be part of the metadata
  # section of your pod template in a deployment spec.
  selector:
    app.kubernetes.io/name: example
    app.kubernetes.io/component: web

  # If your service doesn't expose ports, you can use None here instead.
  type: ClusterIP
```

</div>

</div>

#### Deployments

Deployments describe how to create the pods that will run to fulfill a
service.

<div class="code panel pdl" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
apiVersion: apps/v1
kind: Deployment
metadata:
  labels:
    # Labels for a deployment do not need to match its pods, but can overlap.
    app.kubernetes.io/name: example
    app.kubernetes.io/component: web
  name: example-web
  namespace: default
spec:


  # The selector contains labels which must match for a pod to controlled by
  # this deployment. These labels must contain the selector for a service in
  # order to receive traffic for that service.
  selector:
    matchLabels:
      app.kubernetes.io/name: example
      app.kubernetes.io/component: web

  template:
    metadata:
      labels:
        # The labels for the pod template must include all the selector labels.
        app.kubernetes.io/name: example
        app.kubernetes.io/component: web

        # However, the pod template can also contain extra labels.
        app.kubernetes.io/version: stable

    spec:

      # Your deployment must describe at least one container for its pods.
      containers:
        # Each container must have a name which is unique within the deployment.
      - name: main

        # Your containers must include an image. This image can be created as
        # part of a CI/CD pipeline, usually using Docker.
        image: docker.io/mycompany/myapplication:abcd123

        # Your pod templates can include literal environment variables.
        - env:
          - name: PORT
            value: "3000"

        # Your pod template can also variables from a config map or secret.
        envFrom:
        - configMapRef:
            name: example
        - secretRef:
            name: example

        # If your pod exposes ports, they must be declared.
        ports:
        - containerPort: 3000
          name: http
          protocol: TCP

        # You should include a readiness probe so that Kubernetes knows when
        # your pods are ready to receive traffic for a service.
        readinessProbe:
          httpGet:
            httpHeaders:
            - name: Host
              value: example.com
            path: /robots.txt
            port: 3000
          initialDelaySeconds: 10
          periodSeconds: 10
          timeoutSeconds: 2

      # If you need to wait for an external condition or perform some special
      # work on startup, you can use an init container. These containers use the
      # same format as the containers above, but run sequentially before other
      # containers start. If an init container fails, it will be retried after
      # an incremental backoff period.
      initContainers:

      # This example container will fail if migrations haven't run yet, which
      # means the pod will wait to start up until migrations are complete.
      - command:
        - rake
        - db:abort_if_pending_migrations
        envFrom:
        - configMapRef:
            name: example
        - secretRef:
            name: example
        image: docker.io/mycompany/myapplication:abcd123
        name: migrations
```

</div>

</div>
