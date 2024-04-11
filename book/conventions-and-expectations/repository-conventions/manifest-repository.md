
### Manifest Repository

For each application repository being deployed, we will create an
additional repository containing deployment manifests and configuration
values and makes it easy to deploy both using a CI/CD pipeline. This
allows us to separate code from configuration. These repositories should
be named "*application*-manifests."

```
buildspec.yaml
bases/
charts/
overlays/
```
