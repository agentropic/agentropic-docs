# RustyAI

**A modular multi-agent framework for Rust.**

RustyAI provides everything you need to build autonomous agent systems — from individual agent lifecycles to complex organizational patterns, cognitive reasoning, message-passing, and supervised runtime execution.

## Why RustyAI?

- **Modular** — Use only the crates you need. A simple agent needs only `agent-core`. Complex systems compose all five crates.
- **Type-safe** — Rust's type system catches agent communication errors, invalid state transitions, and resource misuse at compile time.
- **Pattern-rich** — Eight organizational patterns out of the box: hierarchy, swarm, coalition, market, blackboard, federation, holarchy, and team.
- **Observable** — Built-in metrics, health checks, circuit breakers, and distributed tracing.
- **Standards-based** — FIPA-compliant performatives for agent communication.

## The Five Crates

| Crate | Purpose |
|-------|---------|
| `agent-core` | Agent trait, identity, lifecycle, state machine |
| `messaging` | Router, messages, FIPA performatives, protocols |
| `cognition` | BDI architecture, planning, reasoning, utility functions |
| `patterns` | Hierarchy, swarm, coalition, market, federation, team, holarchy, blackboard |
| `runtime` | Scheduler, supervisor, circuit breaker, metrics, isolation |

The `rustyai` facade crate re-exports all five for convenience.

## Quick Example
```rust
use agent_core::prelude::*;

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

- [GitHub Organization](https://github.com/rustyai)
- [Examples](https://github.com/rustyai/examples)
- [Website](https://rustyai.org)
