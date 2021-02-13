# Ingress for Kubernetes

 * Ingress: A Kubernetes Ingress exposes HTTP and HTTPS traffic from outside the Cluster to Services within the cluster.
 * Ingress Controller: An application that is responsible for fulfilling Ingress requests.

An external client of the Cluster, can access a Pod running within the Cluster via an Ingress Controller. Once traffic is directed to the Pod, traffic will be directed to the correct container within that Pod.

![Figure](https://github.com/jnvarghese/Azure/blob/master/image/1_StSV9Y39W526t9VbXo7Pmw.png?raw=true)

The preceding diagram describes the following components. A Client would like to send traffic to a Pod. The Pod is deployed to a K3s Cluster and exposed within the Cluster by creating a ClusterIP Service. This Service is not accessible by the client, but the service is accessible by the Ingress Controller. The Ingress Controller fulfills routing rules defined by Ingress. The Ingress Controller is exposed to the client by a NodePort Service.
![Figure](https://github.com/jnvarghese/Azure/blob/master/image/1_5oo8LVrF2byMiXmq7vGUFw.png?raw=true)

The diagram above shows that K3s elects to deploy Traefik as an implementation for the Ingress Controller. Therefore, Traefik will take on the responsibility of fulfilling Ingress requests. Ingress requests are submitted by K3s users to instantiate routing rules for incoming traffic based off various HTTP attributes.
The Ingress described in the diagram above creates a routing rule on Traefik such that incoming traffic with a path matching anything after / will be redirected to Service nginx-svc at port 80.
