
### Application Repository

Source code for applications should also be managed in Git. We recommend
that each application contain a Dockerfile for building Docker images
and a buildspec for pushing them to an ECR repository.

<div class="code panel pdl" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
Dockerfile
buildspec.yaml
```

</div>

</div>

<div class="confluence-information-macro confluence-information-macro-information">

<span class="aui-icon aui-icon-small aui-iconfont-info confluence-information-macro-icon"></span>

<div class="confluence-information-macro-body">

You can quickly create new applications using
[Suspenders](https://github.com/thoughtbot/suspenders).

</div>

</div>
