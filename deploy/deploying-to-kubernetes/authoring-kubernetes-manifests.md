Kubernetes resources are described by manifests written in YAML. Every
object in Kubernetes has a `kind`, `apiVersion`, and `metadata`. Most
objects also have a `spec`.

## `metadata`

Metadata declares information used to identify and find resources.

<div class="code panel pdl" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
# Kubernetes objects are versioned and grouped. When objects evolve in ways that
# aren't backwards compatible, they are released as a new version.
apiVersion: apps/v1

# Every Kubernetes object has a kind which declares the type of the resource.
kind: Deployment

metadata:

  # Every Kubernetes object has a name, which must be unique within its
  # namespace for its kind.
  name: example-web

  # Labels are used to select relevant resources by deployments and services.
  labels:

    # Kubernetes has some recommended labels such as name and component.
    app.kubernetes.io/component: web
    app.kubernetes.io/instance: production
    app.kubernetes.io/name: example
    app.kubernetes.io/part-of: example

  # Kubernetes objects are grouped in namespaces.
  namespace: default

# Most objects have a spec that describe the resource being created.
spec:
  # ...
```

</div>

</div>

## `spec`

The spec will vary depending on the kind of object.

For information about deploying services, see [deploying
services](../../deploy/deploying-to-kubernetes/deploying-services.md).
