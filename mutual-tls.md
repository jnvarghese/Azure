
*Mutual TLS: Securing Microservices in Service Mesh

![what is mtls](https://cdn.thenewstack.io/media/2021/01/17958866-screen-shot-2021-01-29-at-16.24.28.png)


Mutual TLS extends the client-server TLS model to include authentication of both parties. Where the bank relies on other, application-specific mechanisms to confirm a client’s identity — such as a user name and password (often accompanied by two-factor authentication) — mTLS uses x.509 certificates to identify and authenticate each microservice. Each certificate contains a public encryption key and an identity, and is signed by a trusted certificate authority who proves that the certificate represents the entity presenting it.

In mTLS, each microservice in a service mesh verifies the other’s certificate and uses the public keys to create encryption keys unique to each conversation. This enables the communications between pairs of microservices to be authenticated and encrypted.

-How mTLS Works in a Service Mesh

  Microservice A sends a request for the certificate of microservice B.
  Microservice B replies with its certificate and requests the certificate of Microservice A.
  Microservice A checks with the certificate authority that the certificate belongs to Microservice B.
  Microservice A sends its certificate to microservice B and also shares a session encryption key (encrypted with the public key of microservice B).
  Microservice B checks with the certificate authority that the certificate it received belongs to microservice A.
  With both microservices mutually authenticated and a session key created, communication between them can be encrypted and sent via the secure link
  
  
From an mTLS perspective, Istio and all service mesh control planes must offer:
  
  A certificate authority that handles certificate signing and management.
  A configuration API server that distributes communication policies (such as authentication policies, authorization policies and secure naming information) to the proxies.
  The control plane distributes the certificates and authorization policies to the sidecars. When two microservices need to communicate, the sidecars establish a secure proxy-       proxy link and are responsible for encrypting the traffic through it.
  
The Role of Sidecars for mTLS

sidecars implments authentication mechanisms, defining authorization policies, and traffic encryption in the code of each microservice. When two microservices need to communicate, it is the sidecars that establish the mTLS connection through which encrypted traffic will flow. 

Why Non-mTLS Communication Is Still Important

Sometimes it is important for microservices to communicate with external sources or microservices that may not have mTLS enable

Implementing mTLS in a Service Mesh

All service meshes work on the same principles to secure communications between microservices.Istio, for example, is advanced and flexible with its mTLS implementation. It offers granular levels to define the extent of your mTLS deployment. Mutual TLS can be set specific to a service, across a namespace, or over the entire service mesh — obviously, Istio selects the narrowest matching policy for each service.

This granularity enables you to assign namespace ownership to different organizational groups and lets them define their own mTLS settings. That said, each group needs to be mindful of the level of mTLS restriction they deploy — especially for microservices that communicate externally.

Istio supports three mTLS modes that enable you to control how microservices communicate in a service mesh:

Permissive: Proxies will accept both mTLS and plain text traffic.
Strict: Proxies accept only mTLS traffic.
Disable: Mutual TLS is disabled.

![mTLS Deployment Modes](https://cdn.thenewstack.io/media/2021/01/95861c98-screen-shot-2021-01-29-at-16.28.48.png)

Istio configures each proxy to use mTLS in permissive mode by default, which allows a service to accept both plain text and mutual TLS traffic.

There are many things that can break applications when you move to strict mode. For example:

Microservices without sidecars will not complete an mTLS handshake; you may have to add a sidecar to those microservices without one.
Incorrect naming of service ports will cause sidecars to reject mTLS requests; pay extra attention to Istio’s precise naming convention of $protocol-$service.

-Be Aware of mTLS Differences in Various Service Mesh Control Planes

Red Hat OpenShift is based on the Istio control plane and has similar mTLS features, including granular implementation and Permissive mode by default, but replaces the underlying BoringSSL with OpenSSL.
