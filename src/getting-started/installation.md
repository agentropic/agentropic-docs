# Installation

## Prerequisites

- [Rust](https://rustup.rs/) 1.70 or later
- Cargo (included with Rust)

## Adding Agentropic to Your Project

Create a new project:
```bash
cargo new my-agent-system
cd my-agent-system
```

### Option 1: Use the facade crate (all-in-one)

Add to your `Cargo.toml`:
```toml
[dependencies]
agentropic = { git = "https://github.com/agentropic/agentropic", branch = "main" }
async-trait = "0.1"
tokio = { version = "1.0", features = ["full"] }
```

### Option 2: Pick individual crates

Use only what you need:
```toml
[dependencies]
# Required — agent trait, identity, lifecycle
agentropic-core = { git = "https://github.com/agentropic/agentropic-core", branch = "main" }

# Optional — message passing between agents
agentropic-messaging = { git = "https://github.com/agentropic/agentropic-messaging", branch = "main" }

# Optional — BDI reasoning, planning, utility functions
agentropic-cognition = { git = "https://github.com/agentropic/agentropic-cognition", branch = "main" }

# Optional — organizational patterns (hierarchy, swarm, market, etc.)
agentropic-patterns = { git = "https://github.com/agentropic/agentropic-patterns", branch = "main" }

# Optional — scheduling, supervision, metrics
agentropic-runtime = { git = "https://github.com/agentropic/agentropic-runtime", branch = "main" }

async-trait = "0.1"
tokio = { version = "1.0", features = ["full"] }
```

## Verify Installation
```bash
cargo build
```

If it compiles, you're ready to go. Head to [Quick Start](./quickstart.md) to build your first agent.
