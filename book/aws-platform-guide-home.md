## **Introduction**

thoughtbot has developed a platform for deploying applications to AWS.
This guide documents our approach and provides help for both operators
deploying and administrating the platform and for developers deploying
applications to the platform.

-----

  - [Conventions and
    Expectations](../../conventions-and-expectations.md)
      - [Account
        Conventions](../../conventions-and-expectations/account-conventions.md)
      - [Repository
        Conventions](../../conventions-and-expectations/repository-conventions.md)
          - [Landing Zone
            Repository](../../conventions-and-expectations/repository-conventions/landing-zone-repository.md)
          - [Infrastructure
            Repository](../../conventions-and-expectations/repository-conventions/infrastructure-repository.md)
          - [Application
            Repository](../../conventions-and-expectations/repository-conventions/application-repository.md)
          - [Manifest
            Repository](../../conventions-and-expectations/repository-conventions/manifest-repository.md)
      - [Terraform
        Conventions](../../conventions-and-expectations/terraform-conventions.md)
  - [Landing Zone](../../landing-zone.md)
      - [Prerequisites](../../landing-zone/prerequisites.md)
      - [Launching Control
        Tower](../../landing-zone/launching-control-tower.md)
      - [Launch Customizations for Control
        Tower](../../landing-zone/launch-customizations-for-control-tower.md)
          - [Enroll Existing (Legacy)
            Accounts](../../landing-zone/launch-customizations-for-control-tower/enroll-existing--legacy--accounts.md)
      - [Set up accounts](../../landing-zone/set-up-accounts.md)
      - [Configure Single Sign
        On](../../landing-zone/configure-single-sign-on.md)
          - [Google Sign
            In](../../landing-zone/configure-single-sign-on/google-sign-in.md)
          - [Microsoft
            SSO](../../landing-zone/configure-single-sign-on/microsoft-sso.md)
      - [Access and
        Permissions](../../landing-zone/access-and-permissions.md)
  - [Provision Platform
    Resources](../../provision-platform-resources.md)
      - [Provision
        Networks](../../provision-platform-resources/provision-networks.md)
      - [Deploy Ingress
        stack](../../provision-platform-resources/deploy-ingress-stack.md)
      - [Deploy
        Flightdeck](../../provision-platform-resources/deploy-flightdeck.md)
      - [Deploy
        Grafana](../../provision-platform-resources/deploy-grafana.md)
  - [Maintain](../../maintain.md)
      - [Upgrade Flightdeck](../../maintain/upgrade-flightdeck.md)
          - [Add new clusters to your network
            module](../../maintain/upgrade-flightdeck/add-new-clusters-to-your-network-module.md)
          - [Create a new sandbox
            cluster](../../maintain/upgrade-flightdeck/create-a-new-sandbox-cluster.md)
          - [Set up ingress for the new
            cluster](../../maintain/upgrade-flightdeck/set-up-ingress-for-the-new-cluster.md)
          - [Deploy the Flightdeck
            platform](../../maintain/upgrade-flightdeck/deploy-the-flightdeck-platform.md)
          - [Deploy workloads to the new
            cluster](../../maintain/upgrade-flightdeck/deploy-workloads-to-the-new-cluster.md)
          - [RDS Postgres Database
            Upgrade](../../maintain/upgrade-flightdeck/rds-postgres-database-upgrade.md)
      - [Update SCIM tokens](../../maintain/update-scim-tokens.md)
  - [Provision](../../provision.md)
      - [Application
        Resources](../../provision/application-resources.md)
          - [Application Terraform
            Modules](../../provision/application-resources/application-terraform-modules.md)
          - [Application
            Roles](../../provision/application-resources/application-roles.md)
          - [Application
            Secrets](../../provision/application-resources/application-secrets.md)
      - [DNS](../../provision/dns.md)
  - [Deploy](../../deploy.md)
      - [Building CI/CD
        Pipelines](../../deploy/building-ci-cd-pipelines.md)
          - [GitHub
            Actions](../../deploy/building-ci-cd-pipelines/github-actions.md)
              - [Accessing another GitHub
                repository](../../deploy/building-ci-cd-pipelines/github-actions/accessing-another-github-repository.md)
          - [AWS Code
            Pipeline](../../deploy/building-ci-cd-pipelines/aws-code-pipeline.md)
      - [Managing Secrets](../../deploy/managing-secrets.md)
          - [AWS Secrets
            Manager](../../deploy/managing-secrets/aws-secrets-manager.md)
          - [Mounting
            Secrets](../../deploy/managing-secrets/mounting-secrets.md)
      - [Deploying to
        Kubernetes](../../deploy/deploying-to-kubernetes.md)
          - [Introduction to
            Kubernetes](../../deploy/deploying-to-kubernetes/introduction-to-kubernetes.md)
          - [Authoring Kubernetes
            Manifests](../../deploy/deploying-to-kubernetes/authoring-kubernetes-manifests.md)
          - [Deploying
            Services](../../deploy/deploying-to-kubernetes/deploying-services.md)
          - [Environment Variables and Configuration
            Files](../../deploy/deploying-to-kubernetes/environment-variables-and-configuration-files.md)
          - [Composing with
            Kustomize](../../deploy/deploying-to-kubernetes/composing-with-kustomize.md)
  - [Scaling and Capacity](../../scaling-and-capacity.md)
      - [Cluster
        Capacity](../../scaling-and-capacity/cluster-capacity.md)
      - [Container Size](../../scaling-and-capacity/container-size.md)
      - [Horizontal
        Scaling](../../scaling-and-capacity/horizontal-scaling.md)
  - [Troubleshooting](../../troubleshooting.md)
      - [Application
        Errors](../../troubleshooting/application-errors.md)
      - [Cluster Errors](../../troubleshooting/cluster-errors.md)
  - [Reference](../../reference.md)
      - [Modules](../../reference/modules.md)
          - [Flightdeck:
            Network](../../reference/modules/flightdeck--network.md)
          - [Flightdeck:
            Ingress](../../reference/modules/flightdeck--ingress.md)
          - [Flightdeck:
            Cluster](../../reference/modules/flightdeck--cluster.md)
          - [Flightdeck:
            Platform](../../reference/modules/flightdeck--platform.md)
      - [Templates](../../reference/templates.md)
          - [Flightdeck
            Template](../../reference/templates/flightdeck-template.md)
          - [Landing Zone
            Template](../../reference/templates/landing-zone-template.md)
  - [Security and Compliance](../../security-and-compliance.md)
      - [Security Incident Response Plan
        Template](../../security-and-compliance/security-incident-response-plan-template.md)
  - [Recently Updated](../../recently-updated.md)
  - [flightdeck-addons](../../flightdeck-addons.md)
      - [Setup PostgreSQL Slow Query
        Alert](../../flightdeck-addons/setup-postgresql-slow-query-alert.md)
