# Indexing
## Overview
The Indexer is the computation layer of the B.L.U.E. System.
It transforms distributed PDS record streams into a structured, queryable learning graph

This includes:
- subject grouping
- difficulty progression validation
- method linking
- vote aggregation
- derived hierarchy construction

The Indexer does NOT store canonical data and is NOT authoritative.

## Core Principle
> The Indexer does not define knowledge. It reconstructs structure from existing data.

It operates on:
- `blue.article`
- `blue.method`
- `blue.vote`

All computations are derived, not stored as source-of-truth.

## Inputs
The Indexer consumes append-only record streams from multiple PDS nodes.

### Input types
- `blue.article`
- `blue.method`
- `blue.vote`

Each input is:
- immutable
- independently created
- source-controlled by PDS owners

## Output
The Indexer produces a **Derived Hierarchy View**.
This is a computed structure used by the App layer.

It includes:
- subject groupings
- difficulty progression chains
- article-method relationships
- aggregated vote signals

## Phases
### Aggregation Phase
#### Purpose
Collect and normalize distributed data.

#### Process
The Indexer:
- subscribes to multiple PDS streams
- ingests all records
- normalises into a unified internal representation

No transformation of source data occurs.
The result is a unified dataset

### Grouping Phase

#### Subject grouping
Articles are grouped by:
- `subjectArea`

#### Difficulty grouping
Within each subject:
- articles are grouped by `difficultyLevel`

### Hierarchy Construction
#### Definition
The derived hierarchy is a reconstructed learning graph:
```
subjectArea --> difficultyLevel --> ordered learning units
```

It is not stored anywhere in PDS.

#### Construction rules
For each `subjectArea`:

1. Collect levels
	- Identify all difficulty levels present

2. Validate continuity
	- Ensure progression integrity:
	    - If Level 3 exists, Level 1 and 2 must exist somewhere in the ecosystem
	    - Missing levels are flagged

3. Build graph
	- Construct ordered progression chains
		- e.g. Level 1 > Level 2 > Level 3

#### Missing Level Rule
If a level is missing:
- The Indexer does NOT reject data
- The Indexer marks the graph as **incomplete**

Example:
```
Level 1 --> (missing Level 2) --> Level 3
```

This is a structural warning, not a hard block.

### Method Linking
##### Definition
Methods are alternative interpretations of the same article.

##### Linking process
For each method:
- read `baseArticle`
- attach method under that article node

##### Constraints
- Method MUST reference exactly one article
- Method MUST NOT introduce new difficulty levels
- Method MUST remain within same subjectArea implicitly via baseArticle

### Vote Aggregation
#### Purpose
Votes provide signal strength for content relevance.

#### Process
For each article:
- collect all votes referencing article URI
- compute aggregated score:

```
score = sum(value)
```

#### Output usage

Votes are used for:
- ranking within difficulty levels
- surface quality signals
- optional sorting in App layer

#### Constraints
- Votes never modify articles
- Votes never affect structure
- Votes are purely interpretive signals

### Derived Hierarchy View
#### Definition
A materialised, queryable representation of the system’s structure.

#### Structure

```
SubjectArea
	|__ DifficultyLevel
	    |__ Articles
            |__ Methods
            |__ VoteScore
```

#### Properties
- fully recomputable
- not persisted as source of truth
- regenerated from raw PDS streams
- may differ across indexer implementations (future multi-indexer system)

## Validation Layer
#### Purpose
Ensures structural coherence of the dataset.

#### Rules
1. Level consistency
	- If Level N exists → Levels < N must exist somewhere in ecosystem
2. Method validity
	- Method must reference valid article URI

3. Subject consistency
	- Article subjectArea must remain consistent across its methods
4. Immutability enforcement
	- No record mutation is allowed
	- Only new CID entries are accepted

#### Output
Validation produces:
- warnings (non-blocking)
- structural integrity flags
- missing hierarchy indicators

No data is rejected in MVP unless explicitly invalid.

## Indexer Properties
### Statelessness
The Indexer:
- does NOT store canonical data
- can be fully rebuilt at any time
- relies only on PDS streams

### Replaceability
Any Indexer instance can:
- recompute the full system state
- produce a different interpretation of structure

This enables future:
- multi-indexer ecosystems
- federated interpretations
- comparative ranking systems

### Non-authority principle
The Indexer:
- does NOT define truth
- does NOT define correctness
- only reconstructs structure from existing records

## MVP Constraints

In the MVP:
- Single Indexer instance
- No distributed indexing network
- No relay system required
- No trust layer active
- No cross-indexer comparison

## Output Interface (to App Layer)

The Indexer exposes:

- `getSubjects()`
- `getHierarchy(subjectArea)`
- `getArticle(articleURI)`
- `getMethods(articleURI)`
- `getVotes(articleURI)`

These are:
- read-only
- derived views
- recomputable at any time

# Summary

The Indexer is the “understanding engine” of B.L.U.E. which does the following:
- reconstructs structure from distributed data
- builds learning hierarchies
- connects methods to concepts
- aggregates community signals
- enforces structural validation

It never owns the knowledge - it only interprets it.