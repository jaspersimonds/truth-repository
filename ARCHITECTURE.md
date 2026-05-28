# Architecture Overview
### *How the Truth Repository Actually Works*

> This document explains the technical design in plain language. It is intentionally non-prescriptive — the goal is to describe *what* needs to be built, not *exactly how* to build it. The community will refine the specifics.

---

## The Three Layers

Think of the system as three stacked layers, each doing one job.

```
┌─────────────────────────────────────┐
│         ACCESS LAYER                │  ← How apps and people interact with it
├─────────────────────────────────────┤
│         STRUCTURE LAYER             │  ← The knowledge graph itself
├─────────────────────────────────────┤
│         STORAGE LAYER               │  ← Where data lives (can't be controlled)
└─────────────────────────────────────┘
```

---

## Layer 1: Storage — The Mesh

**What it does:** Holds all the data in a way that no single entity controls.

**Technology:** [IPFS](https://ipfs.tech/) (InterPlanetary File System) or similar distributed content-addressed storage.

**How it works:**
Every piece of data — every node, every connection, every debate entry — gets a unique fingerprint based on its actual content. That fingerprint IS its address. The data lives on thousands of computers simultaneously. There is no central server.

If one computer goes offline, the data still exists on hundreds of others. To remove data permanently, you would need to simultaneously control every participating computer on earth.

**What's already built:** IPFS exists and is production-ready. We do not build this layer — we use it.

**What we contribute:** The protocol for how knowledge graph data is structured and addressed within IPFS.

---

## Layer 2: Structure — The Knowledge Graph

**What it does:** Organizes all knowledge into nodes and connections, and makes the connections as meaningful as the nodes.

### Nodes (Stars)

Every piece of knowledge is a node. A node contains:
- A claim or fact
- The domain it belongs to
- Its creation timestamp and author
- Its current verification status
- Its full history of changes

Nodes can be added by humans or AI systems. They are never deleted (see Constitution, Article I).

### Connections (Gravity)

Every relationship between two nodes is a connection. A connection is not a simple link — it is a rich object containing:

```
Connection {
  node_a: reference
  node_b: reference
  strength: 0.0 → 1.0       // How well-supported is this relationship?
  evidence: [source list]    // What supports this connection?
  debates: [thread list]     // Arguments for and against
  truth_vector: [history]    // How strength has changed over time
  uncertainty_type: enum     // "unknown" | "expert_disagreement" |
                             // "evidence_challenged" | "politically_contested"
  created: timestamp
  last_updated: timestamp
}
```

The connection is a first-class object. It has its own history, its own debate, its own trajectory. This is the core architectural innovation.

### Truth Vectors

Every connection tracks its own history of strength changes. A connection that was 0.9 in 2005 and is 0.4 today tells a story. That story is data.

### Uncertainty Types

Not all uncertainty looks the same. The system distinguishes:
- **Unknown** — we simply don't have enough data yet
- **Expert disagreement** — qualified people genuinely see it differently
- **Evidence challenged** — new data has weakened a previously strong connection
- **Politically contested** — the challenge is coming from interest rather than evidence

These types are visually distinct. Conflating them would itself be a form of dishonesty.

### Time-Locked Vaults

Some nodes exist but are not yet openly accessible. Time-locked nodes are:
- Visible as existing (you can see the vault)
- Labeled with their unlock conditions or date
- Never hidden — only deferred

The existence of a vault is itself data.

### Absence Detection

The system continuously scans for connections that should logically exist but don't. Flagged absences are public, challengeable, and treated as significant. A missing link is not a neutral state.

---

## Layer 3: Access — The API

**What it does:** Lets any application read from and contribute to the knowledge graph.

**Design:** Open, documented, permissionless for reading. Contribution requires identity verification (to support the immutable history requirement).

**Who uses it:**
- The web interface humans interact with directly
- AI systems that maintain completeness and flag anomalies
- External applications (the school game, research tools, news platforms, etc.)
- The gamification layer

---

## The Gamification Layer

Built on top of the access layer. This is the human participation engine.

**How humans contribute:**
- Add new nodes with evidence
- Challenge weak or unsupported connections
- Defend connections under challenge
- Propose new connections between existing nodes

**How the incentive system works:**
- Points awarded for successful challenges (finding real weaknesses)
- Points awarded for successful defenses (protecting accurate connections)
- Discovery bonuses for new nodes that pass verification
- Penalty for bad-faith challenges (challenges that fail repeatedly)

The system rewards finding truth, not arguing loudly. Volume of assertion carries no weight. Evidence does.

**Visual language:**
- High-verified nodes: larger, brighter
- Contested nodes: visually distinct, actively flagged
- Nodes needing more information: clearly marked
- Time-locked vaults: visible containers with unlock indicators
- Absence flags: visible gaps in the connection map

---

## The AI Maintenance Layer

AI systems serve the graph in specific, bounded roles:

| Role | Function |
|------|----------|
| **Completeness Scanner** | Identifies knowledge domains with sparse nodes and flags missing connections |
| **Anomaly Detector** | Watches for suppression patterns — coordinated challenge spikes, suspicious gaps around specific entities |
| **New Node Proposer** | Suggests nodes that should exist based on existing graph structure |
| **Debate Summarizer** | Synthesizes long debate threads into accessible summaries |
| **Translation Layer** | Makes knowledge accessible across languages and contexts |

AI systems are contributors, not controllers. They operate within the same constitutional protections as human contributors.

---

## What Already Exists vs. What We Build

| Component | Status |
|-----------|--------|
| IPFS distributed storage | ✅ Exists — use it |
| Content-addressed data | ✅ Exists — use it |
| Knowledge graph databases | ✅ Exist — adapt them |
| Connection-as-first-class-object | 🔨 New — build this |
| Truth vector tracking | 🔨 New — build this |
| Constitutional enforcement at protocol level | 🔨 New — build this |
| Absence detection and flagging | 🔨 New — build this |
| Uncertainty type system | 🔨 New — build this |
| Suppression pattern detection | 🔨 New — build this |
| Gamification layer | 🔨 New — build this |

---

## Build Order (Proposed)

1. **The data model** — define exactly how nodes and connections are structured
2. **The storage protocol** — how the data model maps to IPFS
3. **The core API** — read and write access to the graph
4. **The constitutional enforcement layer** — immutability, traceability, cap logic
5. **The web interface** — the cosmos visualization and human interaction
6. **The gamification layer** — participation incentives
7. **The AI maintenance layer** — completeness, anomaly detection
8. **The absence detection system** — making gaps visible

---

## Open Questions for the Community

- What is the correct maximum ownership percentage for Article II?
- How should the initial seed of knowledge be populated?
- What is the right identity model — persistent accounts, zero-knowledge proofs, or something else?
- How should the safety classification process work in practice?
- What existing knowledge graph projects (Wikidata, Wolfram, etc.) should we interoperate with?

---

*This document describes the vision. The implementation details are a conversation. Open an issue.*
