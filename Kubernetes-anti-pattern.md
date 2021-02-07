One of the benefits of Kubernetes is the elasticity of resources

## 1. Using containers with the latest tag in Kubernetes deployments

Using the “latest” tag for Docker images is a bad practice on its own as “latest” is just a name of the tag and it doesn’t actually mean “most recent” or “lastly built”. Latest is also the default tag if you don’t specify one when talking about a container image.

....
spec:
      containers:
      - name: dont-do-this
        image: docker.io/myusername/my-app:latest
...
Container tags are mutable, so the “latest” tag does not really mean anything to anyone.The “latest” tag is even more dangerous if you couple it with an always pull policy. Let’s say that your pod is dead, and Kubernetes decides to restart it in order to make it healthy
![misusing latest](https://codefresh.io/wp-content/uploads/2021/01/Flows-20.png)

The correct deployment format in Kubernetes should follow a proper tagging strategy. The specific strategy is not that important as long as you have one.

Some suggestions are:

Using tags with Git hashes (e.g. docker.io/myusername/my-app:acef3e). This is straightforward to implement but may be overkill since a Git hash is not easily readable by non-technical people.
Using tags with the application version following semantic versions (e.g. docker.io/myusername/my-app:v1.0.1). This method has many advantages for both developers and non-developers and is our personal recommendation.
Using tags that signify a consecutive number such as a build number or build date/time. This format is very hard to work with but can be easy to adopt with legacy applications.

The important thing is that you should agree that container tags should be treated as immutable. A Docker image that is marked as v2.0.5 should be created only once and should be promoted from one environment to another.

## 2.Baking the configuration inside container images

Your images should be “generic” in the sense that they should be able to run in any environment.Your container images should be built once and then promoted from one environment to another. No configuration should be present in the container itself.

If your container image:

  has hardcoded IP addresses
  contains passwords and secrets
  mentions specific URLs to other services
  Is tagged with strings such as “dev”, “qa”, “production”
  
 ..then you have fallen into the trap of building environment-dependent container images.The solution to this problem is very simple. Create “generic” container images that know nothing about the environment they are running on. For configuration, you can use any external method such as Kubernetes configmaps, Hashicorp Consul, Apache Zookeeper, etc.
 ![Getting configuration from env.](https://codefresh.io/wp-content/uploads/2021/01/Flows-17.png)
 
 A secondary advantage is that if you do need to change the configuration on your cluster, you can simply change the external configuration system instead of rebuilding the full container image from scratch. Depending on the programming language and framework that you use, you can even update the live configuration without any restarts or redeployments.

## 3.Coupling applications with Kubernetes features/services for no reason

![Getting information directly from the cluster](https://codefresh.io/wp-content/uploads/2021/01/Flows-08-1536x633.png)
Now of course, if your application is Kubernetes specific (let’s say that you are creating an autoscaler or operator) then it indeed needs to access Kubernetes services directly.If your application is correctly designed you shouldn’t need Kubernetes for running integration tests locally. Just launch the application on its own (with Docker or Docker-compose) and hit it directly with the tests.

![Inject configuration externally](https://codefresh.io/wp-content/uploads/2021/01/Flows-07.png)

Alternatively, you can also use any of the dedicated solutions for local Kubernetes development such as Okteto, garden.io, and tilt.dev.

## 4.Mixing application deployment with infrastructure deployment

A pipeline that deploys everything (infra/app) might take 30 minutes, while a pipeline that deploys only the application might take only 5 minutes. You are spending 25 extra minutes on each deployment for no reason at all when the infrastructure has not changed.

## Anti-pattern 5 – Performing ad-hoc deployments with kubectl edit/patch by hand

The kubectl command is very powerful and comes with built-in apply/edit/patch commands that can change resources in place on a live cluster. One of the most frequent reasons for failed deployments is environment configuration.
If all your deployments happen via a Git commit:

 * You have a complete history of what happened in your cluster in the form of Git commit history
 * You know exactly what is contained on each cluster at any point in time and how environments differ among themselves
 * You can easily recreate or clone an environment from scratch by reading the Git configuration.
 * Rolling back configuration is trivial as you can simply point your cluster to a previous commit.
 * Most importantly, if a deployment fails, you can pinpoint really fast what was the last change that affected it and how it changed its configuration
 
## Antipattern 6 – Using Kubectl as a debugging tool

### Kubectl is not a debugging tool and should not be used as such

If you need to run kubectl to inspect something it means that you have a gap in your observability tools and the thing that you need to inspect should be added to your monitoring tools.
Kubevious for example is a comprehensive Kubernetes dashboard with a built-in rule engine that allows you to search and mark Kubernetes resources according to custom rules.

## Anti-pattern 7 – Misunderstanding Kubernetes network concepts

ClusterIP services are internal to the cluster, NodePorts are both internal and external and Load balancers are external to the cluster
![Kubernetes traffic types](https://codefresh.io/wp-content/uploads/2021/01/Flows-02.png)

DNS, security certificates, virtual services are all aspects that should be handled in detail for a production Kubernetes cluster.

 ### Ingress
 
 ```An API object that manages external access to the services in a cluster, typically HTTP.Ingress may provide load balancing, SSL termination and name-based virtual hosting.```
  ##### Terminology 
  
  * Node: A worker machine in Kubernetes, part of a cluster.
  * Cluster: A set of Nodes that run containerized applications managed by Kubernetes. For this example, and in most common Kubernetes deployments, nodes in the cluster are not part of the public internet.
  * Edge router: A router that enforces the firewall policy for your cluster. This could be a gateway managed by a cloud provider or a physical piece of hardware.
  * Cluster network: A set of links, logical or physical, that facilitate communication within a cluster according to the Kubernetes networking model.
  * Service: A Kubernetes Service that identifies a set of Pods using label selectors. Unless mentioned otherwise, Services are assumed to have virtual IPs only routable within the cluster network
  
  #### What is Ingress? 
  
   ```Ingress exposes HTTP and HTTPS routes from outside the cluster to services within the cluster. Traffic routing is controlled by rules defined on the Ingress resource.```
 
  An Ingress may be configured to give Services externally-reachable URLs, load balance traffic, terminate SSL / TLS, and offer name-based virtual hosting. An Ingress controller is responsible for fulfilling the Ingress, usually with a load balancer, though it may also configure your edge router or additional frontends to help handle the traffic.
 
  ![More Read](https://kubernetes.io/docs/concepts/services-networking/ingress/)
  
  #### Use an HTTP Proxy to Access the Kubernetes API
  
  
  ![Read More](https://kubernetes.io/docs/tasks/extend-kubernetes/http-proxy-access-api/)
  
## Anti-pattern 8 – Using permanent staging environments instead of dynamic environments

“I am a developer and want to see my feature running and run integration tests against it”, dynamic environments is the way to go."

Abandon the manual maintenance of static environments and move to dynamic environments that are created and destroyed on demand. With Kubernetes this is very easy to accomplish:

![Dynamic preview environments](https://codefresh.io/wp-content/uploads/2021/01/Flows-06.png)

There are many ways to achieve this scenario, but at the very least every Open Pull Request should create a dynamic environment that contains only that Pull Request and nothing else. The environment is automatically destroyed when the pull request is merged/closed or after a specified amount of time.

And then magically a dynamic environment should be present at: feature-a-b-together.staging.company.com or staging.company.com/feature-a-b-together.

Behind the scenes you can use Kubernetes namespaces and ingress rules for this isolation.

## Anti-pattern 9 – Mixing production and non-production clusters
you shouldn’t fall into the trap of creating a single big cluster for all your needs.
mixing production and non-production is an obvious bad idea for resource starvation.

Unless you take active steps against it, all communication inside a Kubernetes cluster is allowed by default

### Depending on the size of your company you might have more clusters than two such as:

  * Production
  * Shadow/clone of production but with less resources
  * Developer clusters for feature testing (see the previous section)
  * Specialized cluster for load testing/security (see previous section)
  * Cluster for internal tools

## Anti-pattern 10 – Deploying without memory and cpu limits

By default an application that is deployed to Kubernetes has no specified resource limits. This means that your application can potentially take over the whole cluster. This would be problematic in a staging cluster and catastrophic in a production cluster as the slightest memory leak or CPU hiccup will wreak havoc in the rest of the applications.

Note that once you gain confidence with your application and how it uses Kubernetes resources you can also automate the whole resource game with a ![vertical cluster auto-scaler](https://github.com/kubernetes/autoscaler/tree/master/vertical-pod-autoscaler).

## Anti-pattern 11 – Misusing Health probes

By default, a Kubernetes application has no health probes unless you explicitly set them up.This means that all your applications should have resource limits AND health probes when deployed in any cluster of any type.

In summary:

  * Startup probe => Checks the initial boot of your applications. It runs only once
  * Readiness probe => Checks if your application can respond to traffic. Runs all the time. If it fails Kubernetes will stop routing traffic to your app (and will try later)
  * Liveness probe => Checks if your application is in a proper working state. Runs all the time. If it fails Kubernetes will assume that your app is stuck and will restart it.
  
  Creating cascading failures by mistake is a very common problem that is destructive even with Virtual machines and Load balancers (i.e., it is not specific to Kubernetes).
  
  ![Cascading Failures](https://codefresh.io/wp-content/uploads/2021/01/Flows-19.png)
  
  Become familiar with circuit breakers as they allow you to decouple the “liveness” of an application with its ability to respond.
  
  ## Anti-pattern 12 – Not using the Helm package manager
  
  There is only one package manager right now for Kubernetes and that is Helm. You can think of Helm as the apt/rpm utility for your Kubernetes clusters.Helm is a package manager that also happens to include templating capabilities.
  
 All templating solutions for Kubernetes suffer from the same issue. They can do their magic during application deployment, but after the deployment has finished Kubernetes only knows a set of manifests and nothing more. Any concept of “application” is lost and can only be recreated by having the original source files at hand.

Helm, on the other hand, knows about the whole application and stores application-specific information in the cluster itself. Helm tracks the application resources AFTER the deployment. The difference is subtle but important.
  
  ![A Helm application](https://codefresh.io/wp-content/uploads/2021/01/Flows-04.png)
  
Let’s say that you get kubectl access on a cluster that has 4 applications (where each one is composed of multiple resources) in the same namespace. Your task is to delete one of these applications.

If you have used a simple templating solution (such as Kustomize), you cannot easily understand the application structure by looking at the running resources. If you don’t have the original templates (i.e. kustomizations and original patches) you need to manually inspect resources with kubectl and make the correlations between the application components.

Helm on the other hand tracks the application inside the cluster. You can simply do:

 * helm ls (to look at the list of applications in the namespace)
 * helm uninstall my-release (to remove the application)
 
That’s it! No access to the original templates is needed or to a CI pipeline.

One of the killer features of Helm is also the rollback function.

With Helm it is trivial to do:

 * helm ls (to see releases)
 * helm history my-release (to see previous versions of deployments)
 * helm rollback my-release my-previous-version

Deploying a Helm chart to a cluster should happen from a Helm repository and not a Git repo.

## Anti-pattern 13 – Not having deployment metrics

By “metrics” we actually mean the whole trilogy of:

 * logging – to examine the events and details of requests (usually post-incident)
 * tracing – to dive deep in the journey of a single request (usually post-incident)
 * metrics – to detect an incident (or even better to predict it)
 

The exact solution that you choose for your metrics is not that important, as long as you have sufficient metrics for all your use cases such as:

  * Getting critical information in a timely manner instead of using kubectl (see antipattern 6)
  * Understanding how traffic enters your cluster and what is the current bottleneck (see antipattern 7)
  * Verifying/adapting your resource limits (see antipattern 10)
 
 ![Automated rollbacks](https://codefresh.io/wp-content/uploads/2021/01/Flows-16.png)
 
 1. A deployment takes place
 2. Metrics are examined (and compared to base case)
 3. Either the deployment is marked as finished or it is rolled back
 
Note that there is no manual step involved in these actions.
 
 ## Anti-pattern 14 – Not having a strategy for secrets
 
 Secrets should be passed during runtime to containers. There are many approaches to secret handling from simple storage to git (in an encrypted form) to a full secret solution like Hashicorp vault.
 
 Some common pitfalls here are:

  * Using multiple ways for secret handling
  * Confusing runtime secrets with build secrets
  * Using complex secret injection mechanisms that make local development and testing difficult or impossible.
  
 You should also pass to each application only the secrets it actually needs.
 
 Runtime secrets are the secrets that an application needs AFTER it is deployed. Examples would be database passwords, SSL certificates, and private keys.
