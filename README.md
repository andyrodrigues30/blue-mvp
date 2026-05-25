# The B.L.U.E. System - MVP

## Overview
The B.L.U.E. System is a decentralized learning architecture that transforms distributed user-generated content into structured knowledge.

This MVP is built around three core principles:
- Data is owned at the edge (PDS)
- Structure is derived, not stored centrally (Indexer)
- Presentation is independent of data ownership (App layer)

## MVP Goal
This MVP exists to prove two core ideas:
- Content can be transformed into structured understanding
- Structured understanding can be derived across independent PDS nodes without central control

## System Components
### 1. Personal Data Server (PDS)
Each PDS:
- stores only user-owned data
- uses immutable ATProto-style records
- emits append-only record streams
- contains no global state or awareness of other nodes

Stores:
- `blue.article`
- `blue.method`
- `blue.vote`

### 2. Indexer
A read-only computation layer that aggregates distributed PDS data.

Responsibilities:
- consume records from multiple PDS nodes
- group content by `subjectArea` and `difficultyLevel`
- compute derived learning hierarchies
- link methods to articles
- aggregate votes
- validate structural consistency

#### MVP Constraint
- Only one Indexer instance exists
- It is not authoritative
- It can be replaced or recomputed at any time

### 3. App / UI Layer
The App layer:
- queries Indexer outputs
- displays structured learning views
- renders subject => level => article navigation
- shows methods and vote signals

It contains no structural logic.

## Achitecture Diagrams
### Core MVP Architecture
![MVP Architecture](docs/diagrams/MVP-Architecture.png)

### Architecture at Scale
![Architecture at Scale](docs/diagrams/Architecture-at-Scale.png)

## Data Model
The system defines three core record types:
- `blue.article` - procedural learning guide (how-to format)
- `blue.method` - alternative explanation of an article
- `blue.vote` - independent signal for aggregation

All records are:
- immutable
- CID-addressed
- stored in PDS nodes
- globally referenceable via URI

## Key System Properties
### Decentralised
- User-owned data (PDS)
- Content creation
- Identity (DID-based)

### Centralised (MVP only)
- Indexer instance
- Derived hierarchy computation

### Stateless Layers
- App layer
- Indexer (regenerable from PDS streams)

## What This System Demonstrates
The MVP validates that:
- structured learning systems do not require central ownership
- hierarchical knowledge can be derived from distributed data
- multiple independent data sources can form a unified learning graph
- ATProto-style record streams are sufficient for educational structure systems

## What is NOT in MVP
- multi-indexer federation
- relay / firehose infrastructure
- trust or reputation systems
- blockchain anchoring
- cross-PDS storage syncing

These are defined in `/docs` as future extensions.

## Documentation
Full system design is split into:
- `/docs/architecture.md`
- `/docs/data-model.md`
- `/docs/indexing.md`
- `/docs/trust-and-federation.md`
- `/docs/vision.md`

## Design Philosophy
B.L.U.E. is built on a separation of concerns:
- PDS - owns data
- Indexer - interprets structure
- App - presents structure

And critically:
> No single layer controls data, meaning, and presentation simultaneously.

## MVP Success Criteria
The system is valid if:
- Articles from multiple PDS nodes appear in a unified view
- Content is correctly grouped by subject and difficulty level
- Methods correctly attach across PDS boundaries
- PDS nodes function independently of Indexer
- The hierarchy can be fully recomputed from raw PDS streams

## Summary
B.L.U.E. is not a traditional learning platform.
It is an experiment in whether structured knowledge can emerge from distributed, user-owned data without central authority.