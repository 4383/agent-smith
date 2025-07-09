
MOC : [[Jira structure to implement observation and pattern detection]]
Source : 
Author : 
Tags : 
Date : 2025-07-08
***
## 🧩 EPIC 1 — **Foundations: Backend Architecture & Data Ingestion**

**Type**: Epic  
**Title**: Build the Backend Architecture for Data Ingestion and Graph-Driven Analysis

### **Goal**

Establish the foundational backend of Agent Smith by collecting data from downstream tools (Jira, Slack), storing it in PostgreSQL, and transforming it into a graph structure in Neo4j for advanced decision-making.  
This Epic sets up the full data flow: from raw observation to a structured, queryable graph that future agents can analyze.

---

### **Acceptance Criteria**

- [[Python]] backend project initialized with clean modular structure
- [[PostgreSQL]] database deployed with schema for Jira and Slack data
- [[Jira]] and [[Slack]] collectors implemented and scheduled periodically
- Collected data ingested into PostgreSQL with deduplication and traceability
- Extract-transform-load (ETL) logic sends relevant data into Neo4j
- Neo4j instance populated with a graph of team members, tickets, and interactions
- Basic graph queries run successfully (ex: show PO-story links, isolated nodes)    

---

### **Open Questions**

- Will graph injection happen in real time, on schedule, or on-demand?
- Should graph entities be enriched with metrics now or later (e.g., edge weight, timestamps)?
- Do we pre-define graph projections, or let agents choose dynamic subgraphs?

---

## 🧱 Tasks Breakdown

---

### 🔧 TASK 1.1 — Initialize Python Backend Project

**Type**: Task  
**Title**: Scaffold the Python codebase for Agent Smith  
**Goal**: Create a modular backend codebase, with support for collectors, storage, and transformations.

**Acceptance Criteria**:

- Project created with folders: `collectors/`, `models/`, `db/`, `etl/`, `utils/`, `scripts/`    
- Python dependencies managed with Poetry or requirements.txt
- Dev tools configured: `.env` with `python-dotenv`, `black`, `ruff`, Makefile or run scripts
- `README.md` includes setup steps for new developers
- Dummy main script logs “Agent Smith backend ready”    

---

### 🔧 TASK 1.2 — Set Up PostgreSQL with Docker

**Type**: Task  
**Title**: Deploy local PostgreSQL instance using Docker Compose  
**Goal**: Provide an easy-to-run development DB environment.

**Acceptance Criteria**:

- PostgreSQL runs locally with a mounted volume
- Configuration via `.env`    
- `docker-compose.yml` documented
- Test table accessible from Python with test insert    

---

### 🔧 TASK 1.3 — Design PostgreSQL Schema (Jira & Slack)

**Type**: Task  
**Title**: Define and implement schema for observed data  
**Goal**: Enable structured storage of collected Jira and Slack data.

**Acceptance Criteria**:

- Tables defined: `jira_issues`, `jira_transitions`, `slack_channels`, `slack_messages`, `team_members`
- Foreign keys, timestamps, indexes in place
- Schema documented in SQL and diagram (ERD)
- Migration script provided (raw SQL or Alembic)

---

### 🔧 TASK 1.4 — Implement SQLAlchemy Connection Layer

**Type**: Task  
**Title**: Create database connection logic for PostgreSQL  
**Goal**: Abstract DB access using SQLAlchemy Core or ORM.

**Acceptance Criteria**:
- Module `db/connection.py` with session management
- Uses environment config for host/user/password
- Includes basic test script to insert/query dummy row
- Logs connection success/failure    

---

### 🔧 TASK 1.5 — Jira Collector (Basic Metadata)

**Type**: Task  
**Title**: Collect metadata from Jira tickets and store in PostgreSQL  
**Goal**: Retrieve issues and transitions, normalize and persist.

**Acceptance Criteria**:

- Uses Jira REST API and auth token from `.env`
- Collects issue key, summary, type, status, assignee, transitions
- Deduplicates on issue key and updated timestamp
- Logs run summary (e.g., 157 tickets updated)

---

### 🔧 TASK 1.6 — Slack Collector (Activity and Mentions)

**Type**: Task  
**Title**: Retrieve team activity metadata from Slack and store in PostgreSQL  
**Goal**: Track per-channel activity and user mentions.

**Acceptance Criteria**:

- Slack API token loaded securely
- Pulls message counts, replies, mentions per user
- Data linked to `team_members` table
- Collector logs activity per channel and user

---

### 🔧 TASK 1.7 — Implement Scheduler for Periodic Collection

**Type**: Task  
**Title**: Schedule recurring collector jobs  
**Goal**: Ensure Jira and Slack collectors run automatically.

**Acceptance Criteria**:

- Uses `schedule` or `APScheduler`
- Jobs defined in reusable way
- Logs execution and failures
- Can be started via a single command

---

### 🔧 TASK 1.8 — Neo4j Setup (Local Dev)

**Type**: Task  
**Title**: Deploy and connect to a local [[Neo4j]] instance  
**Goal**: Provide a working Neo4j instance with connection ready for graph injection.

**Acceptance Criteria**:

- Neo4j running via Docker
- Python backend can connect via `neo4j` or `py2neo`
- `.env` holds credentials
- Console or test script confirms successful write/read    

---

### 🔧 TASK 1.9 — Implement Graph Projection ETL from PostgreSQL

**Type**: Task  
**Title**: Extract data from PostgreSQL and create graph in Neo4j  
**Goal**: Transform relational data into a graph model for reasoning.

**Acceptance Criteria**:

- Graph includes nodes: `User`, `Issue`, `Channel`
- Edges: `ASSIGNED_TO`, `COMMENTED_ON`, `MENTIONED_IN`
- Cypher queries define node/edge creation
- Script can be run manually or via scheduler

---

### 🔧 TASK 1.10 — Validate Graph Model with Example Queries

**Type**: Task  
**Title**: Run sample Cypher queries to validate graph correctness  
**Goal**: Confirm Neo4j graph reflects expected relationships.

**Acceptance Criteria**:

- Run and document queries:
    - Who is most central in story reviews?
    - Which POs are not linked to current tickets?
    - Are there team members with no recent interactions?
- Document limitations and gaps (e.g., missing edges, noisy nodes)
***
#### References

#### Linked To

- [[epic 2 - Graph-Based Pattern Detection for Decision Support]]

#### Contradicted By