Kubernetes 

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

