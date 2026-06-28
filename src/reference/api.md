# API Reference

Full API documentation is generated from source code using `cargo doc`.

## Generating Docs Locally

From the workspace root:
```bash
cd ~/Desktop/zeroicai
cargo doc --no-deps --open
```

This opens HTML documentation for all crates in your browser.

## Crate Documentation

| Crate | Docs |
|-------|------|
| `z-core` | `cargo doc -p z-core --open` |
| `z-messaging` | `cargo doc -p z-messaging --open` |
| `z-cognition` | `cargo doc -p z-cognition --open` |
| `z-patterns` | `cargo doc -p z-patterns --open` |
| `z-runtime` | `cargo doc -p z-runtime --open` |

## Key Entry Points

### z-core

- `z_core::prelude::*` — Agent, AgentId, AgentContext, AgentState, AgentResult

### z-messaging

- `z_messaging::prelude::*` — Message, MessageBuilder, Router, Performative

### z-cognition

- `z_cognition::prelude::*` — BDIAgent, BeliefBase, ReasoningEngine, UtilityFunction, State
- `z_cognition::UtilityFunction` — direct import

### z-patterns

- `z_patterns::prelude::*` — all pattern types
- Individual modules: `hierarchy`, `swarm`, `coalition`, `market`, `federation`, `team`, `holarchy`, `blackboard`

### z-runtime

- `z_runtime::prelude::*` — Supervisor, Scheduler, CircuitBreaker, MetricsRegistry
