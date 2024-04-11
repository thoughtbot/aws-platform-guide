
### Infrastructure Repository

The infrastructure repository contains Terraform modules for managing
cloud resources and Kubernetes Helm charts required for the platform. We
name this repository "*organization* infra."

```
infra/
  cd/
  ci/
  cicd-common/
  cluster/
  hosted-zones/
  network/
  operations-platform/
  workload-platform/
```

::: info
You can create this repository using the
[flightdeck-template](https://github.com/thoughtbot/flightdeck-template),
by selecting it in the “Repository template” dropdown during repo
creation in GitHub.

</div>

</div>

GitHub docs for [Creating a repository from a
template](https://docs.github.com/en/repositories/creating-and-managing-repositories/creating-a-repository-from-a-template).

![Creating a GitHub repository from a template](./images/image-20230807-212358.png)
