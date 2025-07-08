
MOC : [[Jira structure to implement observation and pattern detection]]
Source : 
Author : 
Tags : 
Date : 2025-07-08
***
## 🧩 EPIC 5 — **Conversational Dashboard Agent with Graph Visualization**

**Type**: Epic  
**Title**: Create an Interactive Dashboard Agent for POs with Profile Setup, Alert Dialogue, and Graph Visualization

### **Goal**

Develop a smart, web-based interface for Product Owners to interact with Agent Smith. This dashboard acts as a conversational agent that supports natural language dialogue for configuring personal preferences, reviewing past alerts, and requesting insights into team dynamics. It will also provide live visualizations of interaction graphs and alert timelines, giving POs both data and context.

---

### **Acceptance Criteria**

- The PO can log in securely and configure their profile (teams, project scope, alert preferences)
- The agent supports basic conversation flows via chat UI (e.g., “show me recent alerts”, “why was this flagged?”)
- Alert metadata and recommendations can be requested contextually (follow-up Q&A)
- The dashboard displays a timeline of alerts
- A graph visualization pane shows recent collaboration patterns, isolated users, and active stories

---

### **Open Questions**

- Should we base the agent on an LLM (open source / hosted)?
- Do we persist chat sessions for traceability or start fresh each time?
- Do we restrict interactions to predefined intents or allow open-ended dialogue?    

---

## 🧱 Tasks Breakdown

---

### 🔧 TASK 5.1 — Define PO Profile Schema and Preferences

**Type**: Task  
**Title**: Define and store PO preferences and team/project configuration  
**Goal**: Allow each PO to declare which teams, projects, and alert types are relevant.

**Acceptance Criteria**:

- Schema includes: PO ID, teams, projects, channel preferences, severity filters
- Stored in PostgreSQL or user config store
- Editable from frontend
- Linked to alerts and visualization scopes    

---

### 🔧 TASK 5.2 — Set Up Frontend Framework (React + Tailwind or similar)

**Type**: Task  
**Title**: Scaffold the web interface with authentication and theming  
**Goal**: Provide the base for the conversational and visualization components.

**Acceptance Criteria**:

- React app bootstrapped (Next.js or Vite)
- PO can log in (stubbed or real auth)
- App includes sections: Profile, Chat, Graphs, Alerts
- Theming reflects internal tool aesthetics    

---

### 🔧 TASK 5.3 — Integrate Chat UI and Agent Backend

**Type**: Task  
**Title**: Enable natural language interaction with backend agent  
**Goal**: Create an interface where the PO can chat with Agent Smith.

**Acceptance Criteria**:

- Chat input and display implemented (scroll, persist session locally)
- Backend responds via API with message and optional actions
- Supports basic intents: “list alerts”, “show profile”, “explain this alert”
- Can route to more advanced logic later (LLM, retrieval, etc.)    

---

### 🔧 TASK 5.4 — Connect Chat Agent to Alert Data

**Type**: Task  
**Title**: Allow PO to discuss alerts contextually with Agent Smith  
**Goal**: Provide alert metadata and history via conversation.

**Acceptance Criteria**:

- Agent understands references like “last alert”, “the one from Tuesday”
- Agent can explain why an alert was sent (link to pattern/rule/graph query)
- Agent can summarize trends (e.g., “3 alerts this week, all due to isolation”)
- Answers tailored to current PO profile    

---

### 🔧 TASK 5.5 — Embed Graph Visualization in Dashboard

**Type**: Task  
**Title**: Display live graph of team interactions and agile patterns  
**Goal**: Allow PO to explore team dynamics visually.

**Acceptance Criteria**:

- Uses D3.js or vis.js to display Neo4j graph
- Shows nodes (users, tickets) and edges (comments, mentions, assignments)
- Interactive filtering: show last 7 days, by team, by pattern
- Tooltips or side panel show alert links (e.g., “⚠️ this node is isolated”)    

---

### 🔧 TASK 5.6 — Show Alert Timeline and Quick Filters

**Type**: Task  
**Title**: Display historical alerts with filters and navigation  
**Goal**: Let the PO review past alerts and compare time periods.

**Acceptance Criteria**:

- Alerts shown chronologically with icons, type, timestamp
- Filters: by team, pattern type, severity
- Clicking on an alert shows metadata and links to chat or graph
- Layout responsive and user-friendly    

---

### 🔧 TASK 5.7 — Design Conversational Flows and UX Copy

**Type**: Task  
**Title**: Write conversational UX content for the Agent  
**Goal**: Give Agent Smith a tone and structure helpful to POs.

**Acceptance Criteria**:

- Predefined responses for key actions (e.g., welcome message, alert summary, error)
- Escalation phrases (e.g., “this looks critical, you might want to check in with the team”)
- Friendly, concise, and clear    
- Documented in a dialogue map or YAML/JSON structure
***
#### References

- [[epic 4 - ML-Based Enhancement and Adaptive Pattern Discovery]]

#### Linked To

#### Contradicted By