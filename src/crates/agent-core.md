# agent-core

The foundation crate. Defines the `Agent` trait, identity, lifecycle states, context, and error types.

## Agent Trait

Every agent implements this trait:
```rust
#[async_trait]
pub trait Agent: Send + Sync {
    fn id(&self) -> &AgentId;
    async fn initialize(&mut self, ctx: &AgentContext) -> AgentResult<()>;
    async fn execute(&mut self, ctx: &AgentContext) -> AgentResult<()>;
    async fn shutdown(&mut self, ctx: &AgentContext) -> AgentResult<()>;
}
```

- `initialize()` — called once when the agent starts. Set up resources, connections, state.
- `execute()` — called each cycle. The agent's main work happens here.
- `shutdown()` — called once when the agent stops. Clean up resources.

## AgentId

UUID-based unique identifier:
```rust
let id = AgentId::new();          // random UUID
println!("{}", id);                // displays UUID string

let id2 = AgentId::new();
assert_ne!(id, id2);              // always unique
```

`AgentId` is `Copy`, `Clone`, `Hash`, `Eq`, and `Serialize`/`Deserialize`.

## AgentContext

Execution context passed to every lifecycle method:
```rust
let ctx = AgentContext::new(agent_id);

ctx.log_info("Something happened");    // [INFO] [uuid] Something happened
ctx.log_error("Something broke");      // [ERROR] [uuid] Something broke
```

## AgentState

Lifecycle state machine with validated transitions:
```rust
pub enum AgentState {
    Created,
    Initialized,
    Running,
    Paused,
    Stopped,
}
```

Valid transitions:
```
Created → Initialized
Initialized → Running | Stopped
Running → Paused | Stopped
Paused → Running | Stopped
```
```rust
let state = AgentState::Created;
assert!(state.can_transition_to(AgentState::Initialized));   // true
assert!(!state.can_transition_to(AgentState::Running));       // false
```

## AgentError

Typed errors for agent operations:
```rust
pub enum AgentError {
    InitializationFailed(String),
    ExecutionFailed(String),
    ShutdownFailed(String),
    CommunicationFailed(String),
    Timeout(String),
    Internal(Box<dyn std::error::Error + Send + Sync>),
}
```

`AgentResult<T>` is an alias for `Result<T, AgentError>`.

## Prelude

Import everything you need:
```rust
use agent_core::prelude::*;
// Gives you: Agent, AgentId, AgentContext, AgentState, AgentResult, async_trait
```
