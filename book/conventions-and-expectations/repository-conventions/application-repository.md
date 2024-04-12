### Application Repository

Source code for applications should also be managed in Git. We recommend
that each application contain a Dockerfile for building Docker images
and a buildspec for pushing them to an ECR repository.

```
Dockerfile
buildspec.yaml
```

::: info
You can quickly create new applications using
[Suspenders](https://github.com/thoughtbot/suspenders).
:::
