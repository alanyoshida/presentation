---
title: "What does service mesh have to do with security"
author: "Alan Yoshida"
format: revealjs
---

# Istio
![Istio|200](https://www.vectorlogo.zone/logos/istioio/istioio-icon.svg)
What does service mesh have to do with security

---

![Security Overview](https://istio.io/latest/docs/concepts/security/overview.svg)

---

The Istio security features provide strong identity, powerful policy, transparent TLS encryption, and authentication, authorization and audit (AAA) tools to protect your services and data. The goals of Istio security are:

-   Security by default: no changes needed to application code and infrastructure
-   Defense in depth: integrate with existing security systems to provide multiple layers of defense
-   Zero-trust network: build security solutions on distrusted networks

---

![[arch-sec.jpg]]

---

## Identity And Certificate Management
![Identity Provisioning Workflow | 500](https://istio.io/latest/docs/concepts/security/id-prov.svg)

---

## Authentication Architecture
![Policy|800](https://istio.io/latest/docs/concepts/security/authn.svg)

---

## PeerAuthentication
```yaml
apiVersion: security.istio.io/v1beta1
kind: PeerAuthentication
metadata:
  name: default
  namespace: foo
spec:
  mtls:
    mode: PERMISSIVE

---

apiVersion: security.istio.io/v1beta1
kind: PeerAuthentication
metadata:
  name: default
  namespace: foo
spec:
  selector:
    matchLabels:
      app: finance
  mtls:
    mode: STRICT

```

---

## Authorization Architecture
![Policy](https://istio.io/latest/docs/concepts/security/authz.svg)

---

## AuthorizationPolicy
```yaml
apiVersion: security.istio.io/v1beta1
kind: AuthorizationPolicy
metadata:
 name: httpbin
 namespace: foo
spec:
 action: ALLOW
 rules:
 - from:
   - source:
       principals: ["cluster.local/ns/default/sa/sleep"]
   - source:
       namespaces: ["test"]
   to:
   - operation:
       methods: ["GET"]
       paths: ["/info*"]
   - operation:
       methods: ["POST"]
       paths: ["/data"]
   when:
   - key: request.auth.claims[iss]
     values: ["https://accounts.google.com"]

```

---

## Controle de rotas via VirtualService
Rotas Publicas e  Rotas Privadas

```yaml
apiVersion: networking.istio.io/v1alpha3
kind: VirtualService
metadata:
  name: reviews-route
spec:
  hosts:
  - reviews.prod.svc.cluster.local
  http:
  - name: "reviews-v2-routes"
    match:
    - uri:
        prefix: "/wpcatalog"
    - uri:
        prefix: "/consumercatalog"
    rewrite:
      uri: "/newcatalog"
    route:
    - destination:
        host: reviews.prod.svc.cluster.local
        subset: v2
  - name: "reviews-v1-route"
    route:
    - destination:
        host: reviews.prod.svc.cluster.local
        subset: v1

```

---

Cors:
```yaml
apiVersion: networking.istio.io/v1alpha3
kind: VirtualService
metadata:
  name: ratings-route
spec:
  hosts:
  - ratings.prod.svc.cluster.local
  http:
  - route:
    - destination:
        host: ratings.prod.svc.cluster.local
        subset: v1
    corsPolicy:
      allowOrigins:
      - exact: https://example.com
      allowMethods:
      - POST
      - GET
      allowCredentials: false
      allowHeaders:
      - X-Foo-Bar
      maxAge: "24h"

```

---

## SPIFEE
![[Pasted image 20220701122718.png|200]]
A universal identity control plane for distributed systems

SPIFFE, the Secure Production Identity Framework For Everyone, provides a secure identity, in the form of a specially crafted X.509 certificate, to every workload in a modern production environment. SPIFFE removes the need for application-level authentication and complex network-level ACL configuration.

---

## What is SPIRE?
**SPIRE**, the SPIFFE Runtime Environment, is an extensible system that implements the principles embodied in the SPIFFE standards. SPIRE manages platform and workload attestation, provides an API for controlling attestation policies, and coordinates certificate issuance and rotation.

---

## MTLS
![[Pasted image 20220701121014.jpg | 300]]

---

## Envoy com curiefense

Curiefense is the open source cloud native application security platform that protects all forms of web traffic, services, and APIs. It includes bot management, WAF, application-layer DDoS protection, session profiling, advanced rate limiting, and much more.

![[Pasted image 20220919161921.png | 500]]

---

## References
- https://istio.io/latest/docs/concepts/security

---

## The end is near