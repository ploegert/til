# Mediator

## Mediator Pattern

#### (“Coordinate complex behavior across services”)

The Mediator Pattern defines:

> An object that encapsulates how a set of components interact.

Instead of components communicating directly:

* Each communicates only with the mediator
* The mediator controls interaction logic

This:

* Reduces coupling
* Centralizes workflow
* Allows interaction logic to change independently&#x20;

Mediator Topology Characteristics:

* Contains business logic
* Coordinates workflows
* Decides execution order

Example:

Mediator receives event:

```
Order → Payment → Invoice
```

If payment fails:

* Invoice never runs

The mediator:

> Orchestrates the workflow by determining whether to proceed or stop processing across services.&#x20;

Think:

```
Event → Mediator decides → Execute step-by-step flow
```

***

## Broker vs Mediator (Critical Difference)

| Broker                       | Mediator                       |
| ---------------------------- | ------------------------------ |
| Routes events                | Orchestrates workflows         |
| No execution control         | Has execution control          |
| Stateless routing            | Stateful coordination          |
| Services react independently | Services coordinated centrally |
| No business logic            | Contains business logic        |
| Event distribution           | Process orchestration          |

As explicitly stated:

> The mediator topology coordinates multiple event processors and the workflow within it — while a broker simply sends events out and does not control workflow.
