
### Introduction to Kubernetes

Kubernetes is a container orchestration platform. Kubernetes consists of
several components:

  - An object store for declaring Kubernetes resources

  - The Kubernetes API, which allows clients to read and modify
    resources

  - Controllers, which are containers that use the Kubernetes API to
    watch and update resources and make changes to your infrastructure
    to reflect resources

  - The Kubelet, which runs containers on nodes

#### Controllers and Resources

Developers create Kubernetes resources to describe how their application
should run in the cloud. Kubernetes controllers watch these resources to
make infrastructure reflect these resources.

Controllers can create or modify other resources based on the resources
they watch or they can make actual changes to your cloud infrastructure
to bring your declarations to life.

![Kubernetes Controllers](./images/kubernetes-controllers.png)

This the above diagram, one controller watches deployments and creates a
new replica set for each revision to the deployment. Another controller
watches replica sets and creates pods to for each replica set.

#### Core Resources

There are a few core Kubernetes resources that you'll interact with as a
developer. Kubernetes includes controllers to manage all these resources
for you, so all you need to do is declare which resources you need to
run your services.

![Kubernetes Core Resources](./images/kubernetes-core-resources.png)

##### Pods

Pods are the core compute resource in a Kubernetes cluster. A pod is a
tightly coupled group of containers working together to provide one unit
of work. Some pods may consist of only one container. One example of a
pod would be a single instance of a web worker responding to user web
requests. In order to fulfill production traffic, you would run several
web pods. A pod is similar to a dyno from Heroku.

Kubernetes resources have labels, which are strings that describe the
role of a resource. For example, your web pods might have a `component`
label with a value of `web`.

Pods declare a list of containers that will run to complete their work
alongside configuration necessary to run those containers. A minimal
container definition includes a container image used to create
containers in the cluster.

Pods can also declare ports that will be exposed from their containers.
For example, your web pod might declare an `http` port listening on port
`8080`.

##### Services

A service groups pods together to fulfill a common purpose.

Services find pods based on their labels and optionally describe service
ports which will be routed to the appropriate port on each pod.

A web service could select pods with a `component` label of `web` and
declare the port `80` for the service will route to port `8080` on its
pods.

##### Deployments

While a service describe which pods will be used to fulfill a common
need, a deployment describes how those pods will run.

A deployment includes labels that will be used to determine which pods
it will control as well as a pod template which will be used to create
pods for the deployment. A deployment can also declare how many pods
will run in order to fulfill requests for the service.

A web deployment would select pods using the `component` label of `web`
and include a pod template with the container image for your application
as well as any necessary configuration, such as environment variables or
volumes.

##### Replica Sets

Each time you modify a deployment, a replica set will be created based
on that revision of the deployment. Each time a new replica set is
created, it will gradually create pods based on the deployment's revised
pod template until the desired number of pods is created.

Each replica set becomes obsolete when a new replica set is created for
its deployment. Once a replica set is obsolete, it will slowly terminate
its pods as the new replica set becomes ready until no pods are running
for the old replica set.

##### Config Maps

Config maps can be used to declare environment variables or files that
can be mounted in a container. You can create a config map and then use
it from several deployments. Config maps are used for non-sensitive
information.

##### Secrets

Secrets work just like config maps except that they're designed to store
sensitive information like passwords. Secrets are mounted as environment
variables or files but can have different permissions based on their
sensitive nature.

#### Manifests

To create these resources in a Kubernetes cluster, developers usually
author [Kubernetes
manifests](#authoring-kubernetes-manifests)
in a Git repository which is automatically applied to the cluster by a
CI/CD pipeline.
