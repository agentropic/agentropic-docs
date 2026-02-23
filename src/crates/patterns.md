# agentropic-patterns

Eight organizational patterns for structuring multi-agent systems.

## Hierarchy

Command-and-control with levels and delegation:
```rust
let mut org = Hierarchy::new("my_corp");

let exec = Level::new("Executive", LevelType::Strategic, 3);
let mgmt = Level::new("Management", LevelType::Tactical, 2);
let ops = Level::new("Operations", LevelType::Operational, 1);

org.add_level(exec.clone());
org.add_level(mgmt.clone());
org.add_level(ops.clone());

// Assign agents to levels
org.assign_agent(ceo_id, exec);
org.assign_agent(manager_id, mgmt);

// Level comparison
assert!(exec.is_above(&mgmt));

// Delegate tasks down the chain
let task = Delegation::new(ceo_id, manager_id, "Ship v2.0", 3);
org.delegate(task);

org.delegations().len();  // 1
```

## Swarm

Decentralized coordination inspired by nature:
```rust
let mut swarm = Swarm::new("drones");
swarm.add_member(drone_id);
swarm.size();  // 1

// Flocking (Reynolds' Boids)
let flocking = Flocking::new()
    .with_separation(2.5)
    .with_alignment(1.0)
    .with_cohesion(0.8);

// Foraging (ant colony optimization)
let foraging = Foraging::new()
    .with_pheromone_strength(1.5)
    .with_evaporation_rate(0.15)
    .with_exploration_rate(0.3);

// Behavior
let behavior = Behavior::new(BehaviorType::Exploration)
    .with_parameter("search_radius", 500.0);
swarm.set_behavior(behavior);

// Consensus voting
let mut consensus = Consensus::new(0.6);  // 60% threshold
consensus.vote(agent_a, "option_1");
consensus.vote(agent_b, "option_1");
consensus.vote(agent_c, "option_2");
consensus.is_reached();  // true if threshold met
consensus.winner();       // Some("option_1")
```

## Coalition

Temporary alliances with shared strategy:
```rust
let mut coalition = Coalition::new("alliance");
coalition.add_member(agent_a);
coalition.add_member(agent_b);

let strategy = Strategy::new(StrategyType::MaximizeUtility)
    .with_parameter("risk_tolerance", 0.7);
coalition.set_strategy(strategy);
coalition.set_value(10000.0);

coalition.size();   // 2
coalition.value();  // 10000.0
```

## Market

Resource allocation through auctions:
```rust
let mut market = Market::new("exchange");

let mut auction = Auction::new(AuctionType::English, "gpu_hours")
    .with_reserve_price(50.0);

auction.add_bid(Bid::new(agent_a, 75.0, "gpu_hours"));
auction.add_bid(Bid::new(agent_b, 120.0, "gpu_hours"));

auction.winner();  // Some(Bid { agent_b, 120.0 })

market.add_auction(auction);
market.allocation_mut().allocate(agent_b, "gpu_hours");
```

Auction types: `English`, `Dutch`, `SealedBid`, `Vickrey`.

## Federation

Governance with weighted voting:
```rust
let mut fed = Federation::new("council");
fed.add_member(agent_a);
fed.add_member(agent_b);

fed.set_weight(agent_a, 2.0);
fed.set_weight(agent_b, 1.0);

let policy = Policy::new("approval", PolicyType::WeightedVote)
    .with_threshold(0.6)
    .with_rule("Requires majority approval");
fed.add_policy(policy);

fed.get_policy("approval");  // Some(&Policy)
```

## Team

Role-based coordination:
```rust
let mut team = Team::new("dev_team");

team.assign_role(
    lead_id,
    Role::new("Tech Lead", RoleType::Leader)
        .with_responsibility("Architecture")
        .with_responsibility("Code review"),
);

team.assign_role(
    dev_id,
    Role::new("Developer", RoleType::Executor)
        .with_responsibility("Implementation"),
);

team.set_leader(lead_id);
team.members().len();  // 2
```

Role types: `Leader`, `Coordinator`, `Executor`.

## Holarchy

Nested autonomous units:
```rust
let mut holarchy = Holarchy::new("system");
// Holons are agents that are both wholes and parts
holarchy.add_holon(parent_id, Holon::new("department"));
holarchy.add_child(parent_id, child_id);
```

## Blackboard

Shared knowledge space:
```rust
let mut board = Blackboard::new("shared_knowledge");
board.write("temperature", "25.0");
board.read("temperature");  // Some("25.0")
board.add_source(KnowledgeSource::new("sensor_agent"));
```
