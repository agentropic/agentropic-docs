# agentropic-runtime

Production execution environment — scheduling, supervision, fault tolerance, and observability.

## Supervisor

Monitors agents and applies restart policies:
```rust
let mut supervisor = Supervisor::new("prod_supervisor");

let policy = RestartPolicy::new(RestartStrategy::OnFailure)
    .with_max_retries(3)
    .with_backoff_seconds(5);

supervisor.supervise(agent_id, policy);
supervisor.supervised_count();  // 1

// Access policy
supervisor.get_policy(&agent_id);  // Some(&RestartPolicy)
```

### Restart Strategies

| Strategy | Behavior |
|----------|----------|
| `Never` | Agent stays down |
| `Always` | Restart regardless of exit reason |
| `OnFailure` | Restart only on error |
| `ExponentialBackoff` | Restart with increasing delays |

### Health Checks
```rust
// Record health status
supervisor.get_health_check_mut(&agent_id).unwrap().record_healthy();
supervisor.get_health_check_mut(&agent_id).unwrap().record_unhealthy();

// Query status
let hc = supervisor.get_health_check(&agent_id).unwrap();
hc.status();      // HealthStatus::Healthy | Unhealthy | Unknown
hc.is_healthy();  // bool
hc.failures();    // u32
```

## Circuit Breaker

Prevents cascading failures:
```rust
let mut breaker = CircuitBreaker::new(
    3,                          // failure threshold
    Duration::from_secs(30),    // timeout
);

breaker.state();  // CircuitState::Closed

// Record failures
breaker.record_failure();
breaker.record_failure();
breaker.record_failure();  // hits threshold
breaker.state();           // CircuitState::Open
breaker.is_allowed();      // false

// After timeout, transitions to HalfOpen
// On success, returns to Closed
breaker.record_success();
breaker.state();  // CircuitState::Closed
```

State transitions:
```
Closed → Open (on threshold failures)
Open → HalfOpen (after timeout)
HalfOpen → Closed (on success) | Open (on failure)
```

## Exponential Backoff

Retry with increasing delays:
```rust
let mut backoff = ExponentialBackoff::new(
    Duration::from_millis(100),   // initial delay
    Duration::from_secs(10),      // max delay
);

backoff.next_delay();  // 100ms
backoff.next_delay();  // 200ms
backoff.next_delay();  // 400ms
backoff.next_delay();  // 800ms
backoff.retries();     // 4

backoff.reset();       // back to 100ms
```

## Scheduler

Task scheduling with multiple policies:
```rust
let scheduler = Scheduler::new(
    SchedulingPolicy::new(PolicyType::Priority)
);

// Task queue
let mut queue = TaskQueue::new();
queue.push(Task::new(agent_id, 3));   // priority 3
queue.push(Task::new(agent_id2, 1));  // priority 1

queue.len();       // 2
queue.pop();       // Task { priority: 3 } (FIFO order)
queue.is_empty();  // false
```

Policy types: `FairShare`, `Priority`, `RoundRobin`, `FCFS`.

## Metrics

Collect and export agent metrics:
```rust
let mut registry = MetricsRegistry::new();

let mut collector = Collector::new();
collector.record(
    Metric::new("requests", MetricType::Counter, 142.0)
        .with_label("agent", "worker_a")
);
collector.record(
    Metric::new("cpu", MetricType::Gauge, 45.2)
        .with_label("agent", "worker_a")
);

registry.register("agent_metrics", collector);

// Export as JSON
let exporter = MetricsExporter::new(registry);
let json = exporter.export_json().unwrap();
```

Metric types: `Counter` (monotonic), `Gauge` (up/down), `Histogram`.

## Sandbox

Resource isolation for agents:
```rust
let config = IsolationConfig::new()
    .with_cpu_quota(50.0)
    .with_memory_limit(512 * 1024 * 1024)
    .with_network_isolation(true);

let limits = ResourceLimits::new()
    .with_max_cpu(50.0)
    .with_max_memory(512 * 1024 * 1024)
    .with_max_threads(4);

let sandbox = Sandbox::new(agent_id, config, limits);
sandbox.is_enabled();  // true
```
