# Examples Overview

The [examples](https://github.com/rustyai/examples) crate contains 8 runnable examples that demonstrate every part of the framework.

## Running Examples
```bash
git clone https://github.com/rustyai/examples
cd examples
cargo run --example hello_agent
```

## Example Index

### hello_agent

**Crates:** agent-core

Simplest possible agent. Creates an agent, runs it through the full lifecycle (initialize → execute → shutdown), and demonstrates state transitions.

### messaging

**Crates:** agent-core, messaging

Two agents communicate through a router. Alice sends a request to Bob, Bob replies with results, Alice confirms. Shows `Message`, `MessageBuilder`, `Router`, and `Performative`.

### bdi_reasoning

**Crates:** cognition

Four utility functions (aggressive, conservative, balanced, fallback) evaluate different market scenarios. Demonstrates strategy comparison and selection.

### market_auction

**Crates:** agent-core, patterns

Agents bid on compute resources through English and sealed-bid auctions. Shows `Market`, `Auction`, `Bid`, reserve prices, winner selection, and resource allocation.

### swarm_consensus

**Crates:** agent-core, patterns

Ten drone agents form a swarm with flocking and foraging behaviors, then vote on a target using consensus. Demonstrates `Swarm`, `Flocking`, `Foraging`, `Behavior`, and `Consensus`.

### hierarchy_delegation

**Crates:** agent-core, patterns

Builds a corporate hierarchy (Executive → Management → Operations), assigns agents to levels, delegates tasks down the chain, and organizes a team with roles and responsibilities.

### supervised_agents

**Crates:** agent-core, runtime

Three workers run under a supervisor with different restart policies. Demonstrates health checks, circuit breaker state transitions, exponential backoff, metrics collection with JSON export, and task queue processing.

### full_system

**Crates:** all five

Complete trading system. Three agents with different strategies form a coalition, communicate via router, evaluate market conditions using utility functions, govern themselves through a federation with policies, and operate under supervision with health monitoring and metrics.
