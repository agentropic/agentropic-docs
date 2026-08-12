# Architecture

RustyAI is built as five composable crates, each handling a distinct concern.

## Dependency Graph
```
runtime
    ├── patterns
    │       ├── cognition
    │       │       └── agent-core
    │       ├── messaging
    │       │       └── agent-core
    │       └── agent-core
    └── agent-core
```

## Layer Model

### Layer 1: Core (`agent-core`)

The foundation. Defines what an agent *is*:

- **`Agent` trait** — `initialize()`, `execute()`, `shutdown()`
- **`AgentId`** — UUID-based unique identity
- **`AgentContext`** — execution context with logging
- **`AgentState`** — lifecycle state machine (Created → Initialized → Running → Paused → Stopped)
- **`AgentError`** — typed error handling

Every other crate depends on agent-core. If you're building anything with RustyAI, you need this.

### Layer 2: Communication (`messaging`)

How agents talk to each other:

- **`Message`** — sender, receiver, performative, content, conversation tracking
- **`Router`** — register agents, route messages by AgentId
- **`Performative`** — FIPA speech acts (Inform, Request, Propose, Accept, Reject, etc.)
- **`MessageBuilder`** — fluent API for constructing messages

### Layer 3: Intelligence (`cognition`)

How agents think:

- **`BDIAgent`** — Belief-Desire-Intention architecture
- **`BeliefBase`** — queryable knowledge store
- **`Planner`** — action planning with state transitions
- **`ReasoningEngine`** — rule-based inference
- **`UtilityFunction`** — numerical strategy evaluation

### Layer 4: Organization (`patterns`)

How agents organize:

| Pattern | Use Case |
|---------|----------|
| **Hierarchy** | Command chains, delegation, authority levels |
| **Swarm** | Decentralized coordination, flocking, foraging, consensus |
| **Coalition** | Temporary alliances with shared strategy |
| **Market** | Resource allocation via auctions (English, Dutch, Vickrey, sealed-bid) |
| **Federation** | Governance with weighted voting and policies |
| **Team** | Role-based coordination with leaders and executors |
| **Holarchy** | Nested autonomous units (holons) |
| **Blackboard** | Shared knowledge space with expert knowledge sources |

### Layer 5: Execution (`runtime`)

How agents run in production:

- **`Supervisor`** — restart policies, health monitoring
- **`Scheduler`** — task queues with fair-share, priority, round-robin policies
- **`CircuitBreaker`** — fault isolation (closed → open → half-open)
- **`ExponentialBackoff`** — retry with increasing delays
- **`MetricsRegistry`** — counters, gauges, histograms with JSON export
- **`Sandbox`** — resource limits and isolation

## Design Principles

1. **Composition over inheritance** — agents compose behaviors from traits, not class hierarchies
2. **Async-first** — all agent methods are async, built on Tokio
3. **Zero-cost patterns** — organizational patterns are data structures, not runtime overhead
4. **Fail gracefully** — supervisors, circuit breakers, and health checks are first-class concepts
