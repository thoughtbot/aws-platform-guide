Update your continuous deployment workflow to deploy to your new sandbox
cluster.

<div class="toc-macro rbtoc1710773639879">

  - [GitHub Actions](#Deployworkloadstothenewcluster-GitHubActions)
  - [AWS CodePipeline](#Deployworkloadstothenewcluster-AWSCodePipeline)

</div>

# GitHub Actions

Update your workflow to include a job that deploys to the new cluster:

<div class="code panel pdl" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
- name: Generate kube-config
  run: |
    aws eks update-kubeconfig --name mycompany-sandbox-v2 \
                              --region us-east-1
    echo "KUBE_CONFIG<<EOF" >> $GITHUB_ENV
    cat ~/.kube/config >> $GITHUB_ENV
    echo 'EOF' >> $GITHUB_ENV

- uses: azure/k8s-set-context@v2
  with:
    method: kubeconfig
    kubeconfig: ${{ env.KUBE_CONFIG }}

# Other deploy steps
- uses: azure/k8s-deploy@v4.2
```

</div>

</div>

Push your updated workflow and trigger a deployment. Ensure the
deployment is successful and verify that your workloads are running
successfully on the new cluster.

# AWS CodePipeline

Add your new cluster to your deploy project:

<div class="code panel pdl" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
module "deploy_project" {
  source = "github.com/thoughtbot/terraform-eks-cicd//modules/deploy-project?ref=v0.2.0"

  cluster_names = [
    # Existing clusters
    "mycompany-sandbox-v1",
    "mycompany-production-v1",
    
    # Add your new cluster
    "mycompany-sandbox-v2"
  ]
}
```

</div>

</div>

Update your staging (or other pre-production stages) module to deploy to
the new cluster.

<div class="code panel pdl" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
module "ordering_staging" {
  source = "github.com/thoughtbot/terraform-eks-cicd//modules/cicd-pipeline?ref=v0.2.0"

  deployments = {
    # Deployment for existing cluster
    "sandbox-v1" = { ... }
    
    # Copy the deployment and update for the new cluster
    "sandbox-v2" = {
      cluster_name  = "mycompany-sandbox-v2"
      region        = "..."
      role_arn      = "..."
      manifest_path = "..."
    }
  }
}
```

</div>

</div>

Apply this module and trigger a deployment. Ensure the deployment is
successful and verify that your workloads are running successfully on
the new cluster.
