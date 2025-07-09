
MOC : [[Jira structure to implement observation and pattern detection]], [[Graph Theory]]
Source : 
Author : 
Tags : 
Date : 2025-07-08
***
## 🧩 EPIC 2 — **Graph-Based Pattern Detection for Decision Support**

**Type**: Epic  
**Title**: Leverage Neo4j to Detect Agile Patterns and Team Dynamics

### **Goal**

Use the graph representation (built in Epic 1) to analyze collaboration structures and agile signals. Define and execute queries and algorithms that help the decision engine detect early signs of risk, inefficiency, or deviation from agile norms. This Epic turns the graph from a visualization tool into an actionable reasoning engine for Agent Smith.

---

### **Acceptance Criteria**

- Define a core graph model and shared vocabulary (nodes, edges, attributes)
- Implement reusable Cypher queries to detect patterns (centrality, isolation, disconnection)
- Implement an interface (Python module) to run and consume those queries programmatically
- Document which patterns will feed into the alert system
- Identify metrics that could later feed into ML models (e.g., graph embeddings, user roles)

---

### **Open Questions**

- Should graph queries run on schedule or on-demand (by request or when events occur)?
- Do we need to version pattern queries or keep historical snapshots?
- Should we allow PO-specific pattern definitions (custom thresholds)?    

---

## 🧱 Tasks Breakdown

---

### 🔍 SPIKE 2.1 — Explore Useful Graph Metrics for Team Dynamics

**Type**: Spike  
**Title**: Research graph algorithms for team analysis in Neo4j  
**Goal**: Identify the most relevant graph concepts to detect dysfunction or collaboration risk (e.g., betweenness, PageRank, clustering coefficient).

**Timebox**: 2–3 days

**Deliverables**:

- Confluence page or Markdown summary of useful graph metrics
- Example Cypher queries for each metric
- Mapping between metrics and agile smells (e.g., low density = isolation)

---

### 🔧 TASK 2.2 — Define Core Graph Vocabulary and Schema

**Type**: Task  
**Title**: Document the graph model used for decision-making  
**Goal**: Align the team on a clear, shared schema of node and edge types.

**Acceptance Criteria**:

- Graph documentation includes:
    - Node types (`User`, `Story`, `Channel`)
    - Relationship types (`COMMENTED_ON`, `ASSIGNED_TO`, `MENTIONED_IN`)
    - Node/edge properties used in queries
- Format: Confluence diagram or Mermaid Markdown    

---

### 🔧 TASK 2.3 — Implement Graph Query Layer (Python)

**Type**: Task  
**Title**: Build reusable Python interface for Cypher-based pattern detection  
**Goal**: Enable the system to run graph queries programmatically and interpret results.

**Acceptance Criteria**:

- Module `graph/queries.py` provides helper functions to run queries
- Query results returned as Python dicts or objects
- Includes at least 3 working queries (e.g., “find isolated users”, “list over-central nodes”)
- Errors and no-result cases are handled gracefully    

**Guidance**:  
Use `neo4j` Python driver. Start small with query templates. Add helper functions to prepare filters or thresholds.

---

### 🔧 TASK 2.4 — Detect Isolation and Lack of Interaction

**Type**: Task  
**Title**: Query graph to detect members with few/no active links  
**Goal**: Identify users who are at risk of being disconnected from collaboration.

**Acceptance Criteria**:

- Cypher query: find users with fewer than N active edges in last X days
- Results returned with user ID and story IDs (if any)
- Query documented and integrated in Python module
- Logs when such users are found, for downstream alerting

---

### 🔧 TASK 2.5 — Detect Single Points of Failure (High Centrality)

**Type**: Task  
**Title**: Find users with excessive load or centrality in the graph  
**Goal**: Identify individuals who may become bottlenecks or fragile points.

**Acceptance Criteria**:

- Compute degree or betweenness centrality for `User` nodes
- Threshold to flag top 5% as high risk
- Link back to tickets where they appear as only assignee/commenter
- Document the meaning and consequence of this pattern

---

### 🔧 TASK 2.6 — Track PO Presence in Story Graph

**Type**: Task  
**Title**: Check whether PO is linked to stories in current sprint  
**Goal**: Identify when the PO is not participating in active work discussions.

**Acceptance Criteria**:

- For each active sprint, list stories not linked to the PO via any edge
- Log stories where PO never commented/mentioned/assigned
- Return list of `Story → Missing PO engagement` for alerting system
- Implement test case in Python query layer    

---

### 🔧 TASK 2.7 — Export Graph Pattern Scores to PostgreSQL

**Type**: Task  
**Title**: Persist graph-based metrics for alert engine usage  
**Goal**: Allow downstream systems to consume the results of graph analysis.

**Acceptance Criteria**:

- Store key scores (e.g., isolation score, centrality, engagement) in PostgreSQL
- Each entry linked to `user_id`, `story_id`, `sprint_id` as applicable
- Clear schema documented and migration included
- Makes graph data available for alert engine (Epic 3)
***
#### References

#### Linked To

[[epic 1 - foundations backend architecture and data ingestion]]

#### Contradicted By