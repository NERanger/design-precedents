# Precedent research reference guide

Read this file during Deep-dive mode or when you need cross-domain analogies, a comparison matrix, or a full design taste review.

## Structural design dimensions

Translate the problem into these dimensions to find cross-domain precedents:

- **Interaction**: novice vs expert, frequent vs infrequent, exploratory vs procedural, low-risk vs destructive, sync vs async, single-object vs bulk, local vs collaborative.
- **State**: ephemeral vs durable, local vs distributed, authoritative vs derived, desired vs observed, mutable vs append-only, strong vs eventual consistency.
- **Execution**: request/response, streaming, queued, event-driven, workflow, reconciliation loop, batch, realtime.
- **Failure**: retryable, partially retryable, irreversible, replay-recoverable, reconciliation-recoverable, rollback-required, human-intervention-required.
- **Topology**: monolith, service boundary, distributed ownership, control plane vs data plane, client/server, peer-to-peer, coordinator/workers.

## Cross-domain reference map

Choose references by structural match, not product label. For example, a mocap control system often has better precedents in observability, robotics, media pipelines, or industrial control than in SaaS dashboards; an API configuration problem may resemble Kubernetes resources, Terraform, Git, or compiler configs more than another REST API.

- **Product and dense UI**: Figma, Linear, VS Code, GitHub, Notion, Grafana, Datadog, Carbon, Spectrum. Study object hierarchy, navigation, information density, progressive disclosure, state visibility, dangerous actions, keyboard workflows, empty/error/loading states, and inspection tools.
- **APIs and DX**: Stripe, GitHub, Google AIPs, AWS APIs, Kubernetes API conventions, gRPC, Terraform providers, mature standard libraries. Study resource modeling, naming, idempotency, pagination, versioning, error semantics, retries, long-running operations, partial failure, and defaults.
- **Backend and distributed systems**: PostgreSQL, Redis, Kafka, NATS, Kubernetes, etcd, Temporal, Consul, Terraform, CockroachDB, Git. Study state ownership, persistence, coordination, event history, reconciliation, concurrency, failure recovery, control-plane vs data-plane boundaries, and observability.
- **Complex professional and realtime systems**: Blender, Unreal Editor, DaVinci Resolve, ROS, CAD tools, industrial control, network management, robotics tooling, media pipelines, scientific instrumentation, fleet management. Ask which complexity belongs to the domain (keep visible) versus which complexity the system can absorb.

## Precedent comparison matrix

Use this table in Deep-dive mode when two or more mature systems solve the same structural problem differently:

| Dimension | Precedent A | Precedent B | Current system |
| --- | --- | --- | --- |
| User model and frequency | | | |
| State ownership | | | |
| Failure and recovery model | | | |
| Consistency and latency | | | |
| Scale | | | |
| Automation vs explicit control | | | |
| Exposed complexity | | | |
| Major trade-off | | | |

## Design taste checklist

Review relevant items before finalizing a substantial proposal:

- **User complexity**: must the user understand this concept, or can the system safely infer it? Is automation predictable, inspectable, and overridable?
- **State**: is there one authoritative owner and source of truth? Are authoritative, observed, derived, and stale states clearly distinct?
- **Defaults**: does the common path work safely with minimal config while remaining inspectable and overridable?
- **Failure**: what happens on partial success? Are retries safe and idempotent where needed? Can the system reconcile or recover without manual DB surgery?
- **Evolution**: which choices become active external contracts with stable identifiers? Avoid adding backward-compatibility layers or dual migration paths for superseded internal code unless an active contract requires it.
- **Observability**: can an operator answer what is happening, why, what failed, what depends on it, what changed, and what to do next?
- **Abstraction**: does it remove real complexity or merely rename it? Does it have a clear owner and compose cleanly?

## Worked example: camera IP changes in a distributed capture system

**Weak reasoning**: store each camera's IP in config and make operators update it manually.

**Structural precedent lookup**: instead of looking at static config forms, study service discovery, Kubernetes object identity, network device management, mDNS/DNS-SD, and robotics device discovery.

**Principle extracted**: separate **durable object identity** from **mutable network location**. A network address must not be a device's durable identity.

**Applied design**:

- Stable device identity independent of IP.
- Dynamically discovered network address with explicit connection health.
- Reconciliation loop when an address changes.
- Manual override preserved as a recovery path.

The precedent did not dictate a specific protocol. It fixed the conceptual model.
