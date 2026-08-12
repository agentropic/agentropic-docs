# API Reference

Full API documentation is generated from source code using `cargo doc`.

## Generating Docs Locally

From the workspace root:
```bash
cd ~/Desktop/rustyai
cargo doc --no-deps --open
```

This opens HTML documentation for all crates in your browser.

## Crate Documentation

| Crate | Docs |
|-------|------|
| `agent-core` | `cargo doc -p agent-core --open` |
| `messaging` | `cargo doc -p messaging --open` |
| `cognition` | `cargo doc -p cognition --open` |
| `patterns` | `cargo doc -p patterns --open` |
| `runtime` | `cargo doc -p runtime --open` |

## Key Entry Points

### agent-core

- `agent_core::prelude::*` — Agent, AgentId, AgentContext, AgentState, AgentResult

### messaging

- `messaging::prelude::*` — Message, MessageBuilder, Router, Performative

### cognition

- `cognition::prelude::*` — BDIAgent, BeliefBase, ReasoningEngine, UtilityFunction, State
- `cognition::UtilityFunction` — direct import

### patterns

- `patterns::prelude::*` — all pattern types
- Individual modules: `hierarchy`, `swarm`, `coalition`, `market`, `federation`, `team`, `holarchy`, `blackboard`

### runtime

- `runtime::prelude::*` — Supervisor, Scheduler, CircuitBreaker, MetricsRegistry
