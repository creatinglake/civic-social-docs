---
status: draft
last-reviewed: 2026-04-19
owners: [adam]
version: 0.1
---

# Civic Hub AI Moderation Strategy

---

## 1. Overview

AI moderation in Civic.Social is designed as a **pluggable, assistive layer** within Civic Hubs and civic processes.

It is not a centralized censorship system. Instead, it enables:

- higher-quality civic discourse
- scalable moderation
- local governance control

Each Civic Hub can:
- define its own moderation policies
- select or swap AI moderation agents
- escalate to human moderators when needed

---

## 2. Core Principle

> AI moderation assists and enforces local rules transparently, but does not act as an invisible authority.

This ensures:
- transparency
- user trust
- democratic legitimacy

---

## 3. Architectural Position

AI moderation lives in the:

- Civic Hub layer
- Civic Process layer (e.g., comments, deliberation, submissions)

It operates as:

> A pluggable moderation agent integrated into participation workflows

---

## 4. Moderation Capabilities

### 4.1 Content Quality Assistance

AI can:
- evaluate sources submitted by users
- suggest more credible or relevant sources
- flag low-quality or unsupported claims

Example:
> "This source appears to be opinion-based and lacks citations. Would you like to include a more authoritative reference?"

---

### 4.2 Civility & Behavior Moderation

AI can detect:
- personal attacks
- profanity
- harassment

And respond with:

- warnings
- rewrite suggestions
- optional blocking

Example:
> "This comment may violate community guidelines on respectful discourse. Would you like to revise it?"

---

### 4.3 Deliberation Support

AI can:
- identify off-topic comments
- encourage constructive framing
- suggest policy-focused language

Example:
> "Would you like help reframing this comment to focus on the policy rather than individuals?"

---

### 4.4 Structured Feedback Loop

AI moderation is interactive, not silent:

- user submits content
- AI provides feedback
- user can revise or proceed (depending on rules)

---

## 5. Moderation Modes

### 5.1 Advisory Mode (Default)
- AI provides suggestions
- no enforcement

### 5.2 Assisted Enforcement
- warnings or soft blocks
- explanation provided
- user can revise

### 5.3 Strict Enforcement
- hard blocking based on rules
- clear explanation required

Each hub or process can configure its own mode.

---

## 6. Escalation Model

AI moderation serves as a **first-layer filter**, not the final authority.

Escalation paths:

- user override (where allowed)
- appeal process
- human moderator review

This ensures:
- fairness
- accountability
- adaptability

---

## 7. Pluggable Agent Model

Civic.Social supports:

- multiple moderation agents
- interchangeable implementations
- local customization

Hub admins can:
- choose a moderation agent
- configure rules and thresholds
- swap agents over time

This avoids:
- centralized control
- vendor lock-in

---

## 8. Technical Approach (MVP)

### 8.1 System Components

1. **Classifier Layer**
   - detects toxicity, harassment, etc.

2. **LLM Layer**
   - generates explanations
   - suggests rewrites

3. **Policy Layer**
   - determines:
     - allow
     - warn
     - block

---

### 8.2 Example Flow

User submits:
> "You're an idiot, this policy is garbage."

System:

1. Classifier flags toxicity
2. LLM generates feedback
3. Policy layer applies rule

Response:
> "This comment may violate community guidelines. Would you like to revise it?"

---

## 9. Use of Open Models (Hugging Face)

Open model ecosystems (e.g., Hugging Face) can be used to:

- source moderation models
- run inference (hosted or local)
- experiment with civic-specific behavior

### Strategy

- Start with existing models (no training required)
- Use prompting + rules for behavior
- Optionally fine-tune later

### Constraint

> AI providers must remain optional and replaceable

---

## 10. Governance Safeguards

AI moderation must:

- provide explanations for decisions
- avoid ideological enforcement
- allow appeals or overrides
- defer to human moderation when needed

AI evaluates:
- civility
- structure
- relevance

NOT:
- political correctness
- ideological truth

---

## 11. Strategic Value

AI moderation enables:

- higher-quality discourse
- scalable moderation across hubs
- improved user experience
- resilience against misinformation and toxicity

It functions as a:

> Civic Quality Layer

---

## 12. Summary

AI moderation in Civic.Social is:

- pluggable
- transparent
- locally governed
- assistive first, enforcement second

It enhances civic participation while preserving autonomy, trust, and democratic legitimacy.
