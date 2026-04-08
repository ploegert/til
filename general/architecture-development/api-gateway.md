# API Gateway

## API Gateway

#### (“Control who gets into the system”)

An API Gateway:

> Acts as a single entry point for external clients to access backend microservices.&#x20;

It:

* Receives inbound client requests
* Routes them to appropriate backend services
* Enforces authentication
* Manages traffic flow
* May translate protocols&#x20;

Gateway characteristics:

* Edge‑facing
* Client‑to‑service communication
* Request routing
* Security enforcement

Think:

```
Internet → Gateway → Internal Services
```

Gateways are **not brokers** because:

* They typically operate synchronously
* Do not coordinate service workflows
* Do not distribute asynchronous events
