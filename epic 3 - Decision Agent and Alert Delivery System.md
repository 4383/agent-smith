
MOC : [[Jira structure to implement observation and pattern detection]], [[Python]]
Source : 
Author : 
Tags : 
Date : 2025-07-08
***
## 🧩 EPIC 3 — **Decision Agent and Alert Delivery System**

**Type**: Epic  
**Title**: Design and Implement the Decision-Making and Alerting System for Agent Smith

### **Goal**

This Epic defines the logic and infrastructure behind Agent Smith's decision-making. It takes structured signals (from graph queries or rule engines), determines whether they require human attention, and delivers minimal, contextual alerts to Product Owners through Slack or email. The system must balance signal value, avoid overload, and allow easy configuration of what should trigger an alert.

---

### **Acceptance Criteria**

- A Decision Agent module consumes structured pattern outputs and applies thresholding, logic, or scoring
- Alerts are sent only when conditions are met (e.g., user isolation score > threshold)
- Alerts are clear, actionable, and specific to a project/team/PO
- Delivery mechanism implemented via Slack or email API
- Alerts are logged and auditable (with metadata: date, recipient, reason)

---

### **Open Questions**

- Do we need a feedback loop (PO can rate alert relevance)?
- Do we group alerts or send them individually?
- Should alerts be stored in a timeline dashboard from day one?

---

## 🧱 Tasks Breakdown

---

### 🔍 SPIKE 3.1 — Alert Fatigue Prevention Strategies

**Type**: Spike  
**Title**: Research best practices to avoid alert overload in monitoring systems  
**Goal**: Define clear design rules to ensure Agent Smith is helpful, not annoying.

**Timebox**: 2 days

**Deliverables**:

- Summary of existing models (e.g., Nagios, Opsgenie, Honeycomb)
- Criteria to throttle, group, or prioritize alerts
- Design proposals for: deduplication, cooldowns, escalation

---

### 🔧 TASK 3.2 — Design Decision Agent Interface

**Type**: Task  
**Title**: Create core DecisionAgent class that consumes graph results  
**Goal**: Define how raw detection results become alerts or are ignored.

**Acceptance Criteria**:

- `DecisionAgent.run(inputs)` method accepts structured signals (user ID, pattern type, score, context)
- Applies filtering rules (e.g., severity, repetition, project scope)
- Returns decision: `[IGNORE]`, `[ALERT]`, `[ESCALATE]`
- Logs all decisions with reason and context    

---

### 🔧 TASK 3.3 — Configure Alert Rules and Thresholds

**Type**: Task  
**Title**: Implement and store configurable alerting logic  
**Goal**: Allow thresholds and sensitivity to be customized per pattern or team.

**Acceptance Criteria**:

- JSON/YAML-based rules file (e.g., “isolation_score > 0.75”)
- Reloadable without restart
- Pattern types are versioned and documented
- Each rule defines severity and recommended message template

---

### 🔧 TASK 3.4 — Slack Notification Integration

**Type**: Task  
**Title**: Send alerts via Slack to POs  
**Goal**: Deliver formatted messages to target channels or direct messages.

**Acceptance Criteria**:

- Uses [[Slack]] API to send messages
- Alerts include title, context, timestamp, call-to-action
- Target recipient is derived from project config (via PO ID)
- Supports channel or DM delivery
- Sends test alert from script

---

### 🔧 TASK 3.5 — Email Alert Fallback

**Type**: Task  
**Title**: Enable email-based alerting as alternative channel  
**Goal**: Provide a fallback when Slack is unavailable or disabled.

**Acceptance Criteria**:

- Email template defined (subject, body, signature)
- SMTP or SendGrid integration
- Delivery success logged
- Alerting backend supports channel preference (Slack or email)

---

### 🔧 TASK 3.6 — Store Alert Logs in PostgreSQL

**Type**: Task  
**Title**: Persist all alert decisions for traceability  
**Goal**: Keep track of what was alerted, to whom, and why.

**Acceptance Criteria**:

- `alerts` table created with: `id`, `timestamp`, `user_id`, `pattern_type`, `channel`, `status`, `message`
- Insertion from Decision Agent
- Queryable for audit or visualization
- Includes alert suppression logs (ignored, repeated, throttled)

---

### 🔧 TASK 3.7 — Minimal Web Dashboard (Optional)

**Type**: Task  
**Title**: Display alert timeline on internal web [[DASHBOARD]]  
**Goal**: Allow POs to see past alerts and their context.

**Acceptance Criteria**:

- Simple FastAPI or Flask app
- Authenticated access via internal login
- View per PO: past alerts, type, timestamp, status
- Deployed locally or in internal cluster
***
#### References

- [[epic 2 - Graph-Based Pattern Detection for Decision Support]]

#### Linked To

- [[epic 4 - ML-Based Enhancement and Adaptive Pattern Discovery]]

#### Contradicted By