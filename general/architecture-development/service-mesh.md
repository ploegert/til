# Service Mesh

## Service Mesh

#### (“Control how services talk internally”)

A Service Mesh:

> Acts as a decentralized communication layer for microservices.

Instead of a central intermediary:

* Lightweight proxies are deployed alongside each service
* These intercept service‑to‑service communication&#x20;

Providing:

* Service discovery
* Load balancing
* Security between services
* Observability of service communicatio

Think:

```
Service ↔ Sidecar Proxy ↔ Service
```

Mesh characteristics:

* Internal only
* Decentralized
* Network‑layer mediation
