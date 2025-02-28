### Deploy workloads to the new cluster

Update your continuous deployment workflow to deploy to your new sandbox
cluster.

- [Deploy workloads to the new cluster](#deploy-workloads-to-the-new-cluster)
  - [GitHub Actions](#github-actions)
  - [AWS CodePipeline](#aws-codepipeline)

#### GitHub Actions

Update your workflow to include a job that deploys to the new cluster:

```
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

Push your updated workflow and trigger a deployment. Ensure the
deployment is successful and verify that your workloads are running
successfully on the new cluster.

#### AWS CodePipeline

Add your new cluster to your deploy project:

```
module "deploy_project" {
  source = "github.com/thoughtbot/terraform-eks-cicd//modules/deploy-project?ref=v0.3.0"

  cluster_names = [
    # Existing clusters
    "mycompany-sandbox-v1",
    "mycompany-production-v1",

    # Add your new cluster
    "mycompany-sandbox-v2"
  ]
}
```

Update your staging (or other pre-production stages) module to deploy to
the new cluster.

```
module "ordering_staging" {
  source = "github.com/thoughtbot/terraform-eks-cicd//modules/cicd-pipeline?ref=v0.3.0"

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

Apply this module and trigger a deployment. Ensure the deployment is
successful and verify that your workloads are running successfully on
the new cluster.
