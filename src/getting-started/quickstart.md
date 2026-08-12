# Quick Start

Build and run your first agent in under 5 minutes.

## Step 1: Create a Project
```bash
cargo new hello-agent
cd hello-agent
```

## Step 2: Add Dependencies

Edit `Cargo.toml`:
```toml
[dependencies]
agent-core = { git = "https://github.com/rustyai/agent-core", branch = "main" }
async-trait = "0.1"
tokio = { version = "1.0", features = ["full"] }
```

## Step 3: Write Your Agent

Replace `src/main.rs`:
```rust
use agent_core::prelude::*;

struct CounterAgent {
    id: AgentId,
    count: u32,
}

impl CounterAgent {
    fn new() -> Self {
        Self {
            id: AgentId::new(),
            count: 0,
        }
    }
}

#[async_trait]
impl Agent for CounterAgent {
    fn id(&self) -> &AgentId {
        &self.id
    }

    async fn initialize(&mut self, ctx: &AgentContext) -> AgentResult<()> {
        ctx.log_info("Counter agent initialized");
        Ok(())
    }

    async fn execute(&mut self, ctx: &AgentContext) -> AgentResult<()> {
        self.count += 1;
        ctx.log_info(&format!("Count: {}", self.count));
        Ok(())
    }

    async fn shutdown(&mut self, ctx: &AgentContext) -> AgentResult<()> {
        ctx.log_info(&format!("Final count: {}", self.count));
        Ok(())
    }
}

#[tokio::main]
async fn main() {
    let mut agent = CounterAgent::new();
    let ctx = AgentContext::new(*agent.id());

    agent.initialize(&ctx).await.unwrap();

    for _ in 0..5 {
        agent.execute(&ctx).await.unwrap();
    }

    agent.shutdown(&ctx).await.unwrap();
}
```

## Step 4: Run
```bash
cargo run
```

Output:
```
[INFO] [a1b2c3d4-...] Counter agent initialized
[INFO] [a1b2c3d4-...] Count: 1
[INFO] [a1b2c3d4-...] Count: 2
[INFO] [a1b2c3d4-...] Count: 3
[INFO] [a1b2c3d4-...] Count: 4
[INFO] [a1b2c3d4-...] Count: 5
[INFO] [a1b2c3d4-...] Final count: 5
```

## What's Next?

- [Architecture](./architecture.md) — understand how the crates fit together
- [Building Your First Agent](../guides/first-agent.md) — deeper dive into the Agent trait
- [Multi-Agent Communication](../guides/communication.md) — agents talking to each other
