The infrastructure repository contains Terraform modules for managing
cloud resources and Kubernetes Helm charts required for the platform. We
name this repository "*organization* infra."

<div class="code panel pdl" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
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

</div>

</div>

<div class="confluence-information-macro confluence-information-macro-information">

<span class="aui-icon aui-icon-small aui-iconfont-info confluence-information-macro-icon"></span>

<div class="confluence-information-macro-body">

You can create this repository using the
[flightdeck-template](../../reference/templates/flightdeck-template.md),
by selecting it in the “Repository template” dropdown during repo
creation in GitHub.

</div>

</div>

GitHub docs for [Creating a repository from a
template](https://docs.github.com/en/repositories/creating-and-managing-repositories/creating-a-repository-from-a-template).

![image-20230807-212358.png](attachments/13599104/238551048.png?width=250)
