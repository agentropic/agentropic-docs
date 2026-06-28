# Installation

## Prerequisites

- [Rust](https://rustup.rs/) 1.70 or later
- Cargo (included with Rust)

## Adding ZeroicAI to Your Project

Create a new project:
```bash
cargo new my-agent-system
cd my-agent-system
```

### Option 1: Use the facade crate (all-in-one)

Add to your `Cargo.toml`:
```toml
[dependencies]
zeroicai = { git = "https://github.com/zeroicai/zeroicai", branch = "main" }
async-trait = "0.1"
tokio = { version = "1.0", features = ["full"] }
```

### Option 2: Pick individual crates

Use only what you need:
```toml
[dependencies]
# Required — agent trait, identity, lifecycle
z-core = { git = "https://github.com/zeroicai/z-core", branch = "main" }

# Optional — message passing between agents
z-messaging = { git = "https://github.com/zeroicai/z-messaging", branch = "main" }

# Optional — BDI reasoning, planning, utility functions
z-cognition = { git = "https://github.com/zeroicai/z-cognition", branch = "main" }

# Optional — organizational patterns (hierarchy, swarm, market, etc.)
z-patterns = { git = "https://github.com/zeroicai/z-patterns", branch = "main" }

# Optional — scheduling, supervision, metrics
z-runtime = { git = "https://github.com/zeroicai/z-runtime", branch = "main" }

async-trait = "0.1"
tokio = { version = "1.0", features = ["full"] }
```

## Verify Installation
```bash
cargo build
```

If it compiles, you're ready to go. Head to [Quick Start](./quickstart.md) to build your first agent.
