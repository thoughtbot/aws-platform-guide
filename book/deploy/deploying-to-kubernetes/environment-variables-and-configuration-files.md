
### Environment Variables and Configuration Files

Configuration for pods can provided as environment variables or files by
creating config maps and secrets. Both are documents containing
key/value data. Config maps are for non-sensitive data. For storing
sensitive data like passwords and API tokens, see [managing
secrets](../../deploy/managing-secrets.md).

#### Config Maps

Config maps are key/value pairs of string data which can be mounted in a
pod as environment variables or as a file.

##### Environment Variables

You can define environment variables as key/value pairs in a config map:

<div class="code panel pdl" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
apiVersion: v1
kind: ConfigMap
metadata:
  name: example
  namespace: default
data:
  APPLICATION_HOST: example.com
  LANG: en_US.UTF-8
  PIDFILE: /tmp/server.pid
  PORT: "3000"
  RACK_ENV: production
  RAILS_ENV: production
  RAILS_LOG_TO_STDOUT: "true"
  RAILS_SERVE_STATIC_FILES: "true"
```

</div>

</div>

You can then mount them in pods by modifying your deployment manifest:

<div class="code panel pdl" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
apiVersion: apps/v1
kind: Deployment
metadata:
  name: example-web
  namespace: default
spec:
  selector:
    matchLabels:
      app.kubernetes.io/name: example
  template:
    metadata:
      labels:
        app.kubernetes.io/name: example
    spec:
      containers:
      - name: main
        envFrom:
        - configMapRef:
            name: example
```

</div>

</div>

##### Files

You can also store a file in a config map and mount it:

<div class="code panel pdl" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
apiVersion: v1
kind: ConfigMap
metadata:
  name: sidekiq
  namespace: default
data:
  sidekiq.yml: |
    :verbose: false
    :concurrency: 10
    :timeout: 25
```

</div>

</div>

You can then mount them in pods by modifying your deployment manifest:

<div class="code panel pdl" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
apiVersion: apps/v1
kind: Deployment
metadata:
  name: example-web
  namespace: default
spec:
  selector:
    matchLabels:
      app.kubernetes.io/name: example
  template:
    metadata:
      labels:
        app.kubernetes.io/name: example
    spec:

      # Define your config map as a volume
      volumes:
      - name: sidekiq
        configMap:
          name: sidekiq

      # Mount the volume in your container
      containers:
      - name: main
        volumeMounts:
        - name: sidekiq
          mountPath: /app/config/sidekiq.yml
          subPath: sidekiq.yml
```

</div>

</div>
