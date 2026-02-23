# Glossary

**Agent** — An autonomous entity that perceives its environment, reasons about it, and acts. In Agentropic, anything implementing the `Agent` trait.

**AgentId** — A UUID-based unique identifier assigned to every agent at creation.

**BDI** — Belief-Desire-Intention. A cognitive architecture where agents maintain beliefs about the world, desires they want to achieve, and intentions they're actively pursuing.

**Blackboard** — A shared knowledge space where multiple agents read and write information.

**Circuit Breaker** — A fault tolerance pattern that stops sending requests to a failing service. States: Closed (normal), Open (blocking), HalfOpen (testing recovery).

**Coalition** — A temporary group of agents working together toward a shared goal with a common strategy.

**Consensus** — A decision-making mechanism where agents vote and a decision is reached when votes exceed a threshold.

**Delegation** — Assigning a task from a higher-authority agent to a lower-authority agent in a hierarchy.

**Federation** — A governance pattern where agents have weighted votes and decisions follow defined policies.

**FIPA** — Foundation for Intelligent Physical Agents. An IEEE standard for agent communication. Agentropic implements FIPA performatives.

**Flocking** — Swarm behavior based on Reynolds' Boids: separation, alignment, and cohesion.

**Foraging** — Swarm behavior inspired by ant colony optimization: pheromone trails, evaporation, and exploration.

**Hierarchy** — An organizational pattern with authority levels (Strategic, Tactical, Operational) and delegation chains.

**Holarchy** — A hierarchy of holons — entities that are simultaneously wholes and parts of larger systems.

**Holon** — A unit in a holarchy that is self-contained but also part of a larger whole.

**Market** — A resource allocation pattern using auctions (English, Dutch, Vickrey, sealed-bid).

**Message** — A communication unit carrying a performative, content, sender, receiver, and optional conversation tracking.

**Performative** — A speech act label describing the intent of a message (Inform, Request, Propose, Accept, Reject, etc.).

**Router** — The central message dispatch system. Agents register with a router to send and receive messages.

**Sandbox** — An isolation environment that limits an agent's resource usage (CPU, memory, threads).

**Supervisor** — A runtime component that monitors agent health and applies restart policies on failure.

**Swarm** — A decentralized group of agents coordinating through local rules rather than central authority.

**Team** — A group of agents with defined roles (Leader, Coordinator, Executor) and responsibilities.

**Utility Function** — A function that maps a state to a numerical score, used for strategy evaluation and decision making.
