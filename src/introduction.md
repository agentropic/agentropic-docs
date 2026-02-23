# Agentropic

**A modular multi-agent framework for Rust.**

Agentropic provides everything you need to build autonomous agent systems — from individual agent lifecycles to complex organizational patterns, cognitive reasoning, message-passing, and supervised runtime execution.

## Why Agentropic?

- **Modular** — Use only the crates you need. A simple agent needs only `agentropic-core`. Complex systems compose all five crates.
- **Type-safe** — Rust's type system catches agent communication errors, invalid state transitions, and resource misuse at compile time.
- **Pattern-rich** — Eight organizational patterns out of the box: hierarchy, swarm, coalition, market, blackboard, federation, holarchy, and team.
- **Observable** — Built-in metrics, health checks, circuit breakers, and distributed tracing.
- **Standards-based** — FIPA-compliant performatives for agent communication.

## The Five Crates

| Crate | Purpose |
|-------|---------|
| `agentropic-core` | Agent trait, identity, lifecycle, state machine |
| `agentropic-messaging` | Router, messages, FIPA performatives, protocols |
| `agentropic-cognition` | BDI architecture, planning, reasoning, utility functions |
| `agentropic-patterns` | Hierarchy, swarm, coalition, market, federation, team, holarchy, blackboard |
| `agentropic-runtime` | Scheduler, supervisor, circuit breaker, metrics, isolation |

The `agentropic` facade crate re-exports all five for convenience.

## Quick Example
```rust
use agentropic_core::prelude::*;

struct MyAgent {
    id: AgentId,
}

#[async_trait]
impl Agent for MyAgent {
    fn id(&self) -> &AgentId { &self.id }

    async fn initialize(&mut self, ctx: &AgentContext) -> AgentResult<()> {
        ctx.log_info("Agent starting");
        Ok(())
    }

    async fn execute(&mut self, ctx: &AgentContext) -> AgentResult<()> {
        ctx.log_info("Agent running");
        Ok(())
    }

    async fn shutdown(&mut self, ctx: &AgentContext) -> AgentResult<()> {
        ctx.log_info("Agent stopped");
        Ok(())
    }
}
```

## Project Links

- [GitHub Organization](https://github.com/agentropic)
- [Examples](https://github.com/agentropic/agentropic-examples)
- [Website](https://agentropic.com)
