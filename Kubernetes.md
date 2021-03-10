Kubernetes 

The Kubernetes Concepts documentation describes the distinction between the Kubernetes master and non-master nodes as follows:

The Kubernetes Master is a collection of three processes that run on a single node in your cluster, which is designated as the master node. Those processes are:    `kube-apiserver, kube-controller-manager and kube-scheduler.`
Each individual non-master node in your cluster runs two processes:
 -  kubelet, which communicates with the Kubernetes Master.
 -  kube-proxy, a network proxy which reflects Kubernetes networking services on each node.

East-west communication with KubeProxy¶

When you create a Kubernetes deployment for a microservice, Kubernetes deploys a set of pods based on the replica count. To access those pods, you create a Kubernetes service which provides an abstraction to access those pods. The abstraction is provided by assigning a Cluster IP address to the service.

Kubernetes DNS gets populated with an address record that maps the service name with the Cluster IP. So, when an application lets say tea wants to access a microservice (let’s say) coffee then DNS returns the Cluster IP of coffee service to tea application. The Tea application initiates a connection which is then intercepted by Kube Proxy to load balance it to a set of coffee pods.

What is etcd in Kubernetes?

Etcd is a distributed reliable key-value store which is simple, fast and secure. It acts like a backend service discovery and database, runs on different servers in Kubernetes clusters at the same time to monitor changes in clusters and to store state/configuration data that should to be accessed by a Kubernetes master or clusters
Etcd is also useful to set up the desired state for the system.
Etcd stores a critical data for Kubernetes. By means of distributed, it also maintains a copy of data stores across all clusters on distributed machines/servers.


Etcd acts as the brain of the Kubernetes cluster. Monitoring the sequence of changes is done using the ‘Watch’ function of etcd

etcd does not process the writes on each cluster node. Instead, only one of the instances gets the responsibility to process the writes internally. That node is called leader. The elect the leader among themselves and how do they ensure a write is propagated across all instances. This distributed consensus is implemented in etcd using raft protocol.

Kubernetes is the main consumer for etcd project, initiated by CoreOS

Kubernetes already has a very basic “service mesh” out-of-the-box; it’s the “service” resource. It provides service discovery by targeting the needed pods, and a round-robin balancing of requests. A “service” works by managing iptables on each host in the cluster, allowing only a round-robin load balancing approach, with no retries and back off logic, and no other features that we might expect a modern service mesh to handle


### Namespaces
Namespace are a logical way to break a cluster up into logical constructs and also allow you to define RBAC; pod security policies; network policies (to separate workloads); and quotas.

1) Choose the Most Appropriate Kubernetes Controller

Deployment, StatefulSet, and DaemonSet are the most often used controllers in Kubernetes.

When deploying stateless pods, always use the deployment controller. This brings PaaS-like capabilities to pods through scaling, deployment history, and rollback features.

For workloads such as database clusters, a StatefulSet controller will create a highly available set of pods that have a predictable naming convention. Stateful workloads such as Cassandra, Kafka, ZooKeeper, and SQL Server that need to be highly available are deployed as StatefulSets in Kubernetes.

When you need to run a pod on every node of the cluster, you should use the DaemonSet controller. Since Kubernetes automatically schedules a DaemonSet in newly provisioned worker nodes, it becomes an ideal candidate to configure and prepare the node for the workload. For example, if you want to mount an existing NFS or Gluster file share on the node before deploying the workload, package and deploy the pod as a DaemonSet.

2) Configure Health Checks for Pods

By default, all the running pods have the restart policy set to always which means the kubelet running within a node will automatically restart a pod when the container encounters an error.

Health checks extend this capability of kubelet through the concept of container probes. There are three probes that can be configured for each pod — Readiness, liveness, and startup.

A readiness probe ensures that the prerequisites are met before starting the pod. The readiness probe in a CMS pod will ensure that the datastore is mounted and accessible.

The liveness probe will periodically check the health of the container and report to the kubelet. When this health check fails, the pod will not receive the traffic. The service will ignore the pod until the liveness probe reports a positive state. For example, a MySQL pod may include a liveness probe that continuously checks the state of the database engine.

The startup probe which is still in alpha as of version 1.16, allows containers to wait for longer periods before handing over the health check to the liveness probe.

3) Make use of an Init Container to Prepare the Pod

There are scenarios where the container needs initialization before becoming ready. The initialization can be moved to another container to does the groundwork before the pod moves to a ready state.An init container can even be used to ensure that the pods are started in a specific sequence. A pod may contain multiple init containers with each container performing a specific initializing task.

4) Apply Node/Pod Affinity and Anti-Affinity Rules

Kubernetes scheduler does a good job of placing the pods on associated nodes based on the resource requirements of the pod and resource consumption within the cluster.However, there may be a need to control the way pods are scheduled on nodes. Kubernetes provides two mechanisms — Node Affinity/anti-affinity and pod affinity/anti-affinity.

Node affinity extends the already powerful nodeSelector rule to cover additional scenarios. Like the way Kubernetes annotations make labels/selectors more expressive and extensible, node affinity makes nodeSelector more expressive through additional rules. Node affinity will ensure that pods are scheduled on nodes that meet specific criteria. For example, a stateful database pod can be forced to be scheduled on a node that has an SSD attached. Similarly, node anti-affinity will help in avoiding scheduling the pods on the nodes that may cause issues.
While node affinity does matchmaking between pods and nodes, there may be scenarios where you need to co-locate pods for performance or compliance. Pod affinity will help us place pods that need to share the same node. For example, an Nginx web server pod must be scheduled on the same node that has a Redis pod. This will ensure low latency between the web app and the cache. In other scenarios, you may want to avoid running two pods on the same node. When deploying HA workloads, you may want to force that no two instances of the same pod run on the same node. Pod anti-affinity will enforce rules that prevent this possibility.

5) Take Advantage of Auto Scalers

Kubernetes has auto-scaling capabilities for the deployments in the form of horizontal pod autoscaler (HPA), vertical pod autoscaler (VPA), and cluster auto-scaling.

Horizontal pod autoscaler automatically scales the number of pods in a replication controller, deployment, replica set or stateful set based on observed CPU utilization. HPA is represented as an object within Kubernetes which means it can be declared through a YAML file controlled via the kubectl CLI. Similar to the IaaS auto-scaling engines, HPA supports defining the CPU threshold, min and max instances of a pod, cooldown period and even custom metrics.

Vertical pod autoscaling removes the guesswork involved in defining the CPU and memory configurations of a pod. This autoscaler has the ability to recommend appropriate values for CPU and memory requests and limits, or it can automatically update the values. The auto-update flag will decide if existing pods will be evicted or continue to run with the old configuration. Querying the VPA object will show the optimal CPU and memory requests through upper and lower bounds.

As a short definition, an Ingress is a rule that charts how a service, walled inside the cluster, can bridge the divide to the outside world where clients can use it. At the same time, a proxy, which is called an Ingress Controller, listens at the edge of the cluster’s network — watching for those rules to be added — and maps each service to a particular URL path or domain name for public consumption. While the Kubernetes maintainers develop the API, other open source projects implement the Ingress Controllers and add their own features unique to their proxy.

### Reset workflow
`kubeadm reset` is responsible for cleaning up a node local file system from files that were created using the `kubeadm init` or `kubeadm join` commands. For control-plane nodes reset also removes the local stacked etcd member of this node from the etcd cluster and also removes this node's information from the kubeadm ClusterStatus object. ClusterStatus is a kubeadm managed Kubernetes API object that holds a list of kube-apiserver endpoints.


 
JMP Webinar
Webinar: Data Scientists in a Box - Case Studies to turbo-chareg your model-building

Topics: AI | Data Science | Data Visualization | Deep Learning | Machine Learning | NLP | Python | R | Statistics

How to Deploy a Flask API in Kubernetes and Connect it with Other Micro-services
<= Previous postNext post =>
 
 



Share16

Tags: API, Containers, Flask, Kubernetes, MySQL, Python, SQL

A hands-on tutorial on how to implement your micro-service architecture using the powerful container orchestration tool Kubernetes.
KNIME Analytics Platform for Visual Workflow Users Webinar, Feb 10
KNIME Analytics Platform for Visual Workflow Users
Webinar, Feb 10


comments
By Rik Kraan, Vantage AI

Kubernetes is a powerful container orchestration tool that automates deployment and management of containers. If you have a simple lightweight application that exists of one service, don’t bother using Kubernetes. Kubernetes’ benefits emerge if your application has a micro-service architecture with several components working together. It is a ‘open-source system for automating deployment, scaling and management of containerized applications and comes with several benefits including:

Easy (auto-)scaling based on demand
Ways to make your application fault tolerant by distributing workloads in a way that it will remain functional in case of partial failure
Automated health-checks and self-healing processes
Taking care of the communication between your micro-services and balancing incoming traffic evenly over all your resources
Starting with Kubernetes may seem daunting at first, but if you grasp the main concepts of it and play around with the excellent tutorials on the official website, you can get started fairly easily.

In this blog I will:

Provide a quick overview of the main concepts of Kubernetes
Demonstrate how to start your own local cluster
Deploy a MySQL database on your cluster
Set up an Flask app that functions as REST API to communicate with the database
Figure
Network. Photo by Alina Grubnyak on Unsplash

 

Kubernetes basics
 
In this section I will cover the basics of Kubernetes without too many details; feel free to dive deeper by reading the official documentation.

A Kubernetes cluster consists of a master and one or more worker nodes. This architecture is one of the main features of Kubernetes. As you will see, your micro-services are distributed over different nodes so they will remain healthy if one of the worker nodes fails. The master is responsible for managing the cluster and exposes the API via which you can communicate with your cluster. By default, worker nodes come with a few components including some pre-installed software that enables running containers of popular container services as Docker and containerd.

Three main concepts are essential to deploy your own applications on a Kubernetes cluster: Deployments, Pods and Services.

A Deployment is a set of instructions provided to the master on how to create and update your application. With these instructions the master will schedule and run your application on individual worker nodes. The deployment is continuously monitored by the master. If one of the instances of your applications goes down (e.g. if a worker node goes down), it will be automatically replaced by a new instance.
![Figure](https://github.com/jnvarghese/Azure/blob/master/image/1_XwP9kOVopeyg_OC2jikMKg.png?raw=true)

 

A Pod is the atomic unit within the Kubernetes platform. It represents a group of one or more containers and some shared resources for those containers (shared storage, a unique cluster IP address etc.). If you create a deployment, this deployment will create pods with containers inside them. Each pod is bound to a worker node. It is essential to understand that a worker node can have multiple pods, and all these pods will be rebuild on a different available worker node if the current worker node fails.
![Figure](https://github.com/jnvarghese/Azure/blob/master/image/1_3phpgpd52nU86LLNS85VLQ.png?raw=true)

 

A service basically defines a logical set of pods and defines a policy on how to access them. This is necessary as pods can go down and be restarted (e.g. if a worker node is deleted or crashes). A service routes traffic across a set of pods and allow pods to die and replicate without impacting your application. When defining a service, you can specify the type of the service. By default Kubernetes creates a ClusterIP service, which makes your service only accessible from inside the cluster.

![Figure](https://github.com/jnvarghese/Azure/blob/master/image/1_wroUOpgrvj9WLbd3_F8fnA.png?raw=true)
