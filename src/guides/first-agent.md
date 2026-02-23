# Building Your First Agent

This guide walks through building an agent from scratch, covering every part of the `Agent` trait.

## The Agent Trait

Every agent must implement four things:

1. **`id()`** — return a unique `AgentId`
2. **`initialize()`** — set up the agent
3. **`execute()`** — do work
4. **`shutdown()`** — clean up
```rust
use agentropic_core::prelude::*;

struct TemperatureMonitor {
    id: AgentId,
    readings: Vec<f64>,
    threshold: f64,
}

impl TemperatureMonitor {
    fn new(threshold: f64) -> Self {
        Self {
            id: AgentId::new(),
            readings: Vec::new(),
            threshold,
        }
    }

    fn average(&self) -> f64 {
        if self.readings.is_empty() {
            return 0.0;
        }
        self.readings.iter().sum::<f64>() / self.readings.len() as f64
    }
}

#[async_trait]
impl Agent for TemperatureMonitor {
    fn id(&self) -> &AgentId {
        &self.id
    }

    async fn initialize(&mut self, ctx: &AgentContext) -> AgentResult<()> {
        ctx.log_info(&format!(
            "Temperature monitor online. Threshold: {}°C",
            self.threshold
        ));
        Ok(())
    }

    async fn execute(&mut self, ctx: &AgentContext) -> AgentResult<()> {
        // Simulate reading a sensor
        let reading = 20.0 + (self.readings.len() as f64 * 1.5);
        self.readings.push(reading);

        let avg = self.average();
        ctx.log_info(&format!("Reading: {:.1}°C (avg: {:.1}°C)", reading, avg));

        if avg > self.threshold {
            ctx.log_info("ALERT: Average temperature exceeds threshold!");
        }

        Ok(())
    }

    async fn shutdown(&mut self, ctx: &AgentContext) -> AgentResult<()> {
        ctx.log_info(&format!(
            "Monitor offline. {} readings taken, final avg: {:.1}°C",
            self.readings.len(),
            self.average()
        ));
        Ok(())
    }
}
```

## Running the Agent
```rust
#[tokio::main]
async fn main() {
    let mut monitor = TemperatureMonitor::new(25.0);
    let ctx = AgentContext::new(*monitor.id());

    monitor.initialize(&ctx).await.unwrap();

    for _ in 0..10 {
        monitor.execute(&ctx).await.unwrap();
    }

    monitor.shutdown(&ctx).await.unwrap();
}
```

## Key Points

- **Store an `AgentId`** as a field. Create it with `AgentId::new()`.
- **`AgentContext` is borrowed** — you don't own it, just use it for logging and metadata.
- **Return `AgentResult<()>`** — use `Ok(())` for success, or return an `AgentError` variant on failure.
- **`execute()` is called repeatedly** — think of it as one tick or cycle. Keep state between calls in your struct fields.
- **`async_trait` is required** — the `Agent` trait uses async methods, which require the `#[async_trait]` attribute.

## Error Handling

Return typed errors when things go wrong:
```rust
async fn execute(&mut self, ctx: &AgentContext) -> AgentResult<()> {
    if self.sensor_disconnected() {
        return Err(AgentError::ExecutionFailed(
            "Sensor disconnected".to_string()
        ));
    }
    Ok(())
}
```

## Next Steps

- [Multi-Agent Communication](./communication.md) — connect agents with messaging
- [Organizational Patterns](./patterns.md) — structure agents into teams, hierarchies, swarms
