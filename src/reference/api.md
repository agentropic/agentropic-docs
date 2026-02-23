# API Reference

Full API documentation is generated from source code using `cargo doc`.

## Generating Docs Locally

From the workspace root:
```bash
cd ~/Desktop/agentropic
cargo doc --no-deps --open
```

This opens HTML documentation for all crates in your browser.

## Crate Documentation

| Crate | Docs |
|-------|------|
| `agentropic-core` | `cargo doc -p agentropic-core --open` |
| `agentropic-messaging` | `cargo doc -p agentropic-messaging --open` |
| `agentropic-cognition` | `cargo doc -p agentropic-cognition --open` |
| `agentropic-patterns` | `cargo doc -p agentropic-patterns --open` |
| `agentropic-runtime` | `cargo doc -p agentropic-runtime --open` |

## Key Entry Points

### agentropic-core

- `agentropic_core::prelude::*` — Agent, AgentId, AgentContext, AgentState, AgentResult

### agentropic-messaging

- `agentropic_messaging::prelude::*` — Message, MessageBuilder, Router, Performative

### agentropic-cognition

- `agentropic_cognition::prelude::*` — BDIAgent, BeliefBase, ReasoningEngine, UtilityFunction, State
- `agentropic_cognition::UtilityFunction` — direct import

### agentropic-patterns

- `agentropic_patterns::prelude::*` — all pattern types
- Individual modules: `hierarchy`, `swarm`, `coalition`, `market`, `federation`, `team`, `holarchy`, `blackboard`

### agentropic-runtime

- `agentropic_runtime::prelude::*` — Supervisor, Scheduler, CircuitBreaker, MetricsRegistry
