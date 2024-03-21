
### Mounting Secrets

Secrets are functionally identical to [config
maps](../../deploy/deploying-to-kubernetes/environment-variables-and-configuration-files.md),
but they can be configured with stricter permissions due to their
sensitive nature. Secret manifests are not committed to Git.

The best way to manage secrets on AWS is to store the secret value using
AWS Secrets Manager and synchronize the secret to your cluster using the
Kubernetes Secret Storage provider.

On AWS, you can synchronize a secret to your cluster by creating a
secret provider class:

<div class="code panel pdl" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
apiVersion: secrets-store.csi.x-k8s.io/v1alpha1
kind: SecretProviderClass
metadata:
   name: example
spec:
  provider: aws
  secretObjects:
  - secretName: example
    type: Opaque
    data:
    - key: SECRET_KEY_BASE
      objectName: SECRET_KEY_BASE
  parameters:
    objects: |
      - objectName: my-secrets-manager-secret
        objectType: secretsmanager
        jmesPath:
        - path: SECRET_KEY_BASE
          objectAlias: SECRET_KEY_BASE
```

</div>

</div>

Once a secret provider class is created, you can mount them similarly to
config maps:

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

      # Define your secret as a volume using the secrets storage provider
      volumes:
      - name: example
        csi:
          driver: secrets-store.csi.k8s.io
          readOnly: true
          volumeAttributes:
            secretProviderClass: example

      containers:
      - name: main

        # Mount a secret as environment variables
        envFrom:
        - secretRef:
            name: example

        # Or mount the volume in your container
        volumeMounts:
        - name: application
          mountPath: /app/config/application.yml
          subPath: application.yml
```

</div>

</div>
