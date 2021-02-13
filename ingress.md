# Ingress for Kubernetes

 * Ingress: A Kubernetes Ingress exposes HTTP and HTTPS traffic from outside the Cluster to Services within the cluster.
 * Ingress Controller: An application that is responsible for fulfilling Ingress requests.

An external client of the Cluster, can access a Pod running within the Cluster via an Ingress Controller. Once traffic is directed to the Pod, traffic will be directed to the correct container within that Pod.

A Client would like to send traffic to a Pod. The Pod is deployed to a K3s Cluster and exposed within the Cluster by creating a ClusterIP Service. This Service is not accessible by the client, but the service is accessible by the Ingress Controller. The Ingress Controller fulfills routing rules defined by Ingress. The Ingress Controller is exposed to the client by a NodePort Service.
