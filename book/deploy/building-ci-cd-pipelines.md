## Building CI/CD Pipelines

There are many continuous integration and deployment tools available.
The process for using these tools is fairly similar:

- When a change to the application source code is merged, a new
  container image is built for the application and pushed to an image
  repository.
- When changes are pushed to a deploy branch, the latest container
  image is substituted into the manifests for the application and the
  manifests are applied to the appropriate cluster.

We prefer to use GitHub Actions to deploy our applications to AWS.

Some of our clients also use AWS Code Pipeline to deploy.
