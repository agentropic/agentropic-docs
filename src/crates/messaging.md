# messaging

Agent communication through routed messages with FIPA performatives.

## Message

A message carries content between two agents:
```rust
let msg = Message::new(
    sender_id,
    receiver_id,
    Performative::Request,
    "Please process this data",
);

msg.id();              // MessageId (unique)
msg.sender();          // AgentId
msg.receiver();        // AgentId
msg.performative();    // Performative::Request
msg.content();         // "Please process this data"
msg.timestamp();       // SystemTime
```

### Conversation Tracking

Messages can reference conversations and replies:
```rust
let msg = Message::new(sender, receiver, Performative::Inform, "result")
    .with_conversation_id("task-42".to_string())
    .with_reply_to(original_msg.id());

msg.conversation_id();  // Some("task-42")
msg.in_reply_to();      // Some(MessageId)
```

## MessageBuilder

Fluent API for constructing messages:
```rust
let msg = MessageBuilder::new()
    .sender(alice_id)
    .receiver(bob_id)
    .performative(Performative::Propose)
    .content("Let's collaborate on this task")
    .conversation_id("collab-001".to_string())
    .build()
    .expect("missing required field");
```

All four fields (sender, receiver, performative, content) are required. `conversation_id` and `in_reply_to` are optional.

## Router

Central message routing. Agents register to get a receiver channel:
```rust
let router = Router::new();

// Register agents — returns an mpsc receiver
let mut alice_rx = router.register(alice_id).unwrap();
let mut bob_rx = router.register(bob_id).unwrap();

// Send a message (routed by receiver field)
let msg = Message::new(alice_id, bob_id, Performative::Inform, "hello");
router.send(msg).unwrap();

// Bob receives it
let received = bob_rx.recv().await.unwrap();
assert_eq!(received.content(), "hello");

// Check registration
router.is_registered(&alice_id);  // true

// Unregister
router.unregister(&alice_id).unwrap();
```

Sending to an unregistered agent returns `MessagingError::AgentNotFound`.

## Performative

FIPA-compliant speech acts describing message intent:

| Performative | Meaning |
|-------------|---------|
| `Inform` | Sharing information |
| `Request` | Asking for action |
| `Query` | Asking for information |
| `Propose` | Making a proposal |
| `Accept` | Accepting a proposal |
| `Reject` | Rejecting a proposal |
| `Confirm` | Confirming receipt or action |
| `Disconfirm` | Denying something |
| `Subscribe` | Subscribing to updates |
| `CFP` | Call for proposals |
| `Refuse` | Refusing a request |

## Prelude
```rust
use messaging::prelude::*;
// Gives you: Message, MessageId, MessageBuilder, Router, Performative, AgentId
```
