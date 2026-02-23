# Multi-Agent Communication

This guide covers connecting agents through the messaging system.

## Basic Setup

Every messaging scenario needs three things: agents, a router, and messages.
```rust
use agentropic_core::prelude::*;
use agentropic_messaging::prelude::*;

// 1. Create agents (anything with an AgentId)
let alice_id = AgentId::new();
let bob_id = AgentId::new();

// 2. Create router and register agents
let router = Router::new();
let mut alice_rx = router.register(alice_id).unwrap();
let mut bob_rx = router.register(bob_id).unwrap();

// 3. Send messages
let msg = Message::new(alice_id, bob_id, Performative::Inform, "Hello Bob");
router.send(msg).unwrap();

// 4. Receive
let received = bob_rx.recv().await.unwrap();
assert_eq!(received.content(), "Hello Bob");
```

## Request-Reply Pattern

A common pattern: one agent requests something, the other replies.
```rust
// Alice requests
let request = Message::new(
    alice_id, bob_id,
    Performative::Request,
    "Compute fibonacci(10)",
);
router.send(request).unwrap();

// Bob receives and processes
let req = bob_rx.recv().await.unwrap();

// Bob replies
let reply = MessageBuilder::new()
    .sender(bob_id)
    .receiver(alice_id)
    .performative(Performative::Inform)
    .content("Result: 55")
    .in_reply_to(req.id())
    .build()
    .unwrap();
router.send(reply).unwrap();

// Alice gets the result
let result = alice_rx.recv().await.unwrap();
```

## Proposal Protocol

Negotiation between agents:
```rust
// Step 1: Alice proposes
let proposal = Message::new(alice_id, bob_id, Performative::Propose, "Buy 100 units at $50");
router.send(proposal).unwrap();

// Step 2: Bob accepts or rejects
let prop = bob_rx.recv().await.unwrap();
let response = Message::new(bob_id, alice_id, Performative::Accept, "Deal accepted");
// or: Performative::Reject, "Price too high"
router.send(response).unwrap();

// Step 3: Alice confirms
let resp = alice_rx.recv().await.unwrap();
let confirm = Message::new(alice_id, bob_id, Performative::Confirm, "Order confirmed");
router.send(confirm).unwrap();
```

## Conversation Tracking

Group related messages with conversation IDs:
```rust
let conv_id = "order-42".to_string();

let msg1 = MessageBuilder::new()
    .sender(alice_id)
    .receiver(bob_id)
    .performative(Performative::Request)
    .content("Start order")
    .conversation_id(conv_id.clone())
    .build()
    .unwrap();

// All subsequent messages in this exchange use the same conversation_id
```

## Broadcasting

Send to multiple agents by iterating:
```rust
let agents = vec![bob_id, charlie_id, dave_id];

for &receiver in &agents {
    let msg = Message::new(alice_id, receiver, Performative::Inform, "Meeting at 3pm");
    router.send(msg).unwrap();
}
```

## Choosing Performatives

| You want to... | Use |
|----------------|-----|
| Share information | `Inform` |
| Ask for action | `Request` |
| Ask for information | `Query` |
| Suggest a deal | `Propose` |
| Agree to a proposal | `Accept` |
| Decline a proposal | `Reject` |
| Verify receipt | `Confirm` |
| Deny something | `Disconfirm` |
| Listen for updates | `Subscribe` |
| Solicit bids | `CFP` |
| Decline a request | `Refuse` |
