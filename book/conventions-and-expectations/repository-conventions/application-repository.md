### Application Repository

Source code for applications should also be managed in Git. We recommend
that each application contain a Dockerfile for building Docker images
and a deploy folder to hold [Helm Charts'](https://thoughtbot.github.io/helm-charts) files.

```
Dockerfile
deploy/
  production/
    values.yaml
  staging/
    values.yaml
  values.yaml
```

::: info
You can quickly create new applications using
[Suspenders](https://github.com/thoughtbot/suspenders).
:::
