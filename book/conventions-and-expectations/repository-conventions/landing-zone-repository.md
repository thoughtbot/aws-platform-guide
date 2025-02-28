### Landing Zone Repository

The landing zone repository contains foundational [Cloudformation
templates](https://aws.amazon.com/cloudformation/) and [service control
policies](https://docs.aws.amazon.com/organizations/latest/userguide/orgs_manage_policies_scps.html)
deployed as a customized package for [Customizations for Control
Tower](https://aws.amazon.com/solutions/implementations/customizations-for-aws-control-tower/).
We typically name this repository "*organization*-landing-zone."

```
landing-zone/
  manifest.yaml
  policies/
  templates/
```

::: info
You can create this repository using the
[aws-landing-zone-template](https://github.com/thoughtbot/aws-landing-zone-template),
by selecting it in the Repository template dropdown during repo
creation.
:::

GitHub docs for [Creating a repository from a
template](https://docs.github.com/en/repositories/creating-and-managing-repositories/creating-a-repository-from-a-template).

![Creating a GitHub repository from a template](./images/landing-zone-template.png)
