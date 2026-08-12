# Organizational Patterns

This guide helps you choose the right pattern for your multi-agent system.

## Choosing a Pattern

| Scenario | Pattern | Why |
|----------|---------|-----|
| Clear authority chain | **Hierarchy** | Tasks flow top-down, authority levels control delegation |
| Decentralized swarm | **Swarm** | No central leader, agents coordinate through local rules |
| Temporary alliance | **Coalition** | Agents join forces for a specific goal, then dissolve |
| Resource allocation | **Market** | Competitive bidding determines who gets what |
| Shared knowledge | **Blackboard** | Multiple experts contribute to a common knowledge base |
| Democratic governance | **Federation** | Weighted voting on policies and decisions |
| Nested autonomy | **Holarchy** | Units are both autonomous wholes and parts of larger wholes |
| Role-based teamwork | **Team** | Members have defined roles and responsibilities |

## Combining Patterns

Real systems often combine multiple patterns:

### Coalition + Market

Agents form coalitions to bid together in a market:
```rust
// Form coalition
let mut coalition = Coalition::new("buyers");
coalition.add_member(agent_a);
coalition.add_member(agent_b);
coalition.set_value(combined_budget);

// Bid as a group
let mut auction = Auction::new(AuctionType::English, "resource");
auction.add_bid(Bid::new(agent_a, coalition.value(), "resource"));
```

### Hierarchy + Team

Org hierarchy delegates to teams that self-organize:
```rust
// Hierarchy delegates
let task = Delegation::new(ceo, team_lead, "Build feature X", 3);
org.delegate(task);

// Team self-organizes
let mut team = Team::new("feature_x");
team.assign_role(lead_id, Role::new("Lead", RoleType::Leader));
team.assign_role(dev_id, Role::new("Dev", RoleType::Executor));
```

### Federation + Swarm

Federation sets policies, swarm executes:
```rust
// Federation decides strategy
let policy = Policy::new("rules", PolicyType::WeightedVote)
    .with_threshold(0.7)
    .with_rule("Stay within boundary");
federation.add_policy(policy);

// Swarm follows rules autonomously
let behavior = Behavior::new(BehaviorType::Exploration)
    .with_parameter("boundary", 1000.0);
swarm.set_behavior(behavior);
```

## Pattern Lifecycle

Most patterns follow a similar lifecycle:

1. **Create** — `Pattern::new("name")`
2. **Add members** — `pattern.add_member(agent_id)`
3. **Configure** — set strategies, policies, behaviors
4. **Operate** — agents interact through the pattern
5. **Query** — check state, results, membership

## When Not to Use Patterns

If you have a single agent doing a single task, you don't need organizational patterns. Start with `agent-core` alone and add patterns when you have multiple agents that need coordination.
