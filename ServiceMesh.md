Service Mesh

https://kublr.com/blog/implementing-a-service-mesh-with-istio-to-simplify-microservices-communication/

https://blog.codeship.com/an-overview-of-the-service-mesh-and-its-tooling-options/

A service mesh at it’s most fundamental level is a network overlay. Network traffic that crosses the service mesh still rides on traditional switches and routers (or their cloud analogues), but that traffic is encapsulated within another layer that the lower-level devices do not see into. You might compare it to putting a letter into an envelope and mailing it. The sender and recipient can read the letter, but the mail system just passes the envelope around.

![Service Mesh Flow](https://www.singlestoneconsulting.com/wp-content/uploads/2020/04/Service-Mesh-Flow.png)

![Control Plane Implementation](https://www.singlestoneconsulting.com/wp-content/uploads/2020/04/Screen-Shot-2020-04-09-at-12.48.00-PM-1300x624.png)

This has three important implications for the network and security teams:

All traffic in the mesh will be encrypted by default.
Some traditional security technologies (layer 2 IDS/IPS I’m looking at you here) will not function as desired on service mesh traffic due to that encryption.
Endpoints in a service mesh can be placed anywhere that has layer 3 connectivity with the other mesh endpoints (on premise, in the cloud, etc).

The two main goals of a service mesh are to allow insight into previously invisible service communications layers and to gain full control of all microservices communication logic, like dynamic service discovery, load balancing, timeouts, fallbacks, retries, circuit breaking, distributed tracing, and security policy enforcement between services. 

 implementing one of the fully featured service mesh systems in your cluster (Linkerd, Istio or Conduit), will provide you with the following possibilities;

Allow services to talk plain HTTP and not bother about HTTPS on the application layer: The service mesh proxies will manage HTTPS encapsulation on the sender side and TLS termination on the receiver side, allowing the application components to use plain HTTP or gRPC and any other protocol without bothering with encryption in transit. The encryption is taken care of by the proxies.

Security policies enforcement: the proxy knows which services are allowed to access some other services and endpoints and will deny unauthorized traffic.

Circuit breaking: automatic back off in case of accessing an overloaded service or endpoint, that has a high latency already, to avoid hitting it with more and more requests which may cause that endpoint to fail completely under an excessive load.

Latency aware load balancing: instead of using a round-robin style of balancing (which ignores the latency of each target), use smarter balancing according to response times of each backend target. This is an incredibly important feature of a modern service mesh

Queue depth load balancing: route new requests based on the least busy target by current request processing amount. The service mesh knows exactly where it has sent all previous requests, and which of them are still processing or completed, so it will send new incoming requests based on that logic to a target with the lowest queue for processing

Per request routing: route particular requests marked by selected HTTP header to specific targets behind the load balancer, allowing easy canary deployment testing and other creative use cases. One of the most powerful features a service mesh provides.

Health checks, retry budgets, and eviction of misbehaving targets.
Metrics and tracing: reporting of requests volume per target, latency metrics, success and error rates.

There are two main patterns for deploying a service mesh:

As a host shared proxy, a DaemonSet in Kubernetes terms. This type of deployment will use fewer resources if many containers exist on the same host and might also utilize connection pooling to improve throughput. But a failure in one proxy terminates the whole fleet of containers on that host, instead of breaking a single service (if it was used as a sidecar proxy).

As a sidecar container, where the proxy is injected into each pod definition to run alongside the main service. Usually a “sidecar container per pod” is a good choice, limiting possible proxy failure to a single pod, and will not affect other pods on the same host.

a service mesh will save development time, and allow a unified way to control the communication in a central fashion

At the moment there are several major options to use as service mesh: Linkerd, Conduit, Istio, and Envoy. Istio is a component built on top of Envoy, it’s a control plane that can be used with both Envoy and Linkerd as its data plane proxies. The control plane allows a cluster operator to set particular settings in a centralized fashion, which will then be distributed across the data plane proxies and reconfigure them.

Istio is a perfect example of a full feature service mesh, it has several “master components” that manage all “data plane” proxies (those proxies can be Envoy or Linkerd but by default, it is Envoy so that’s what we’ll use in our tutorial while Linkerd integration is still a work in progress).

Control plane:
* Pilot: provides routing rules and service discovery information to the Envoy proxies.
* Mixer: collects telemetry from each Envoy proxy and enforces access control policies.
* Istio-Auth: provides “service to service” and “user to service” authentication and can convert unencrypted traffic to TLS based between services. Soon will be able to provide access audit information (work in progress).
Data plane:
* Envoy: feature rich proxy, that is being managed by control plane components. Intercepts traffic to and from the service and applies the needed routing and access policies following the rules set in the control plane.


