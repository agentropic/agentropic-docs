# agentropic-cognition

Cognitive architecture for intelligent agents — BDI, planning, reasoning, and utility-based decision making.

## UtilityFunction

Evaluate strategies numerically:
```rust
// Custom evaluator
let aggressive = UtilityFunction::new("aggressive", |state: &[String]| {
    if state.iter().any(|s| s.contains("high_reward")) {
        0.9
    } else {
        0.3
    }
});

// Default evaluator (always returns 0.5)
let fallback = UtilityFunction::simple("fallback");

// Evaluate
let state = vec!["high_reward".to_string(), "volatile".to_string()];
let score = aggressive.evaluate(&state);  // 0.9
aggressive.name();                         // "aggressive"
```

### Comparing Strategies
```rust
let strategies = [&aggressive, &conservative, &balanced];
let best = strategies.iter().max_by(|a, b| {
    a.evaluate(&state).partial_cmp(&b.evaluate(&state)).unwrap()
}).unwrap();
println!("Best: {}", best.name());
```

## BDI Architecture

Belief-Desire-Intention model for cognitive agents:

### BeliefBase

Queryable knowledge store:
```rust
let mut beliefs = BeliefBase::new();
beliefs.add(Belief::new("temperature", "hot"));
beliefs.add(Belief::new("location", "office"));

beliefs.query("temperature");  // Some(&Belief)
beliefs.remove("location");
```

### Desires and Goals

What the agent wants to achieve:
```rust
let desire = Desire::new("maximize_profit", 0.9);  // name, priority
let goal = Goal::new("reach_target", "profit > 1000");
```

### IntentionStack

Active plans the agent is executing:
```rust
let mut intentions = IntentionStack::new();
intentions.push(Intention::new("execute_trade", steps));
intentions.current();     // top intention
intentions.is_empty();    // false
```

## Planning

State-action planning:
```rust
let mut planner = Planner::new();
planner.add_action(Action::new(
    "move_north",
    preconditions,
    effects,
));

let plan = planner.plan(initial_state, goal_state);
```

## Reasoning

Rule-based inference:
```rust
let mut engine = ReasoningEngine::new();
engine.add_rule(Rule::new(
    "if_hot_then_cool",
    condition,
    action,
));

engine.infer(&beliefs);
```

## Module Structure
```
agentropic-cognition
├── bdi        (BDIAgent, Belief, BeliefBase, Desire, Goal, Intention, IntentionStack)
├── planning   (Action, Plan, Planner, State)
├── reasoning  (ReasoningEngine, Rule)
└── decision   (UtilityFunction)
```
