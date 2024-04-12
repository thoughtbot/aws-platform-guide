### Composing with Kustomize

Writing Kubernetes by hand can introduce significant data duplication.
You may have several deployments using the same image and configuration
with slight variations, such as container arguments. You may also use
the same configuration in several environments with variations for
staging and production. You can use Kustomize to abstract common
configuration into bases and apply changes in overlays.

Read more on the [Kustomize web site](https://kustomize.io/).
