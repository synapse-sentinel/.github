# Synapse Sentinel

> **The Central Nervous System and Decision Authority for Jordan's Operation**

Synapse Sentinel is not just logging or compliance—it's the **autonomous decision engine** that runs an entire development operation. It receives all signals, makes tactical decisions, and only escalates to humans by exception.

---

## 🧠 The Philosophy

**"Agents propose. Sentinel disposes."**

### Three-Layer Authority

1. **Jordan** - Sets specifications and strategy
2. **Sentinel** - Enforces specs, makes decisions, closes issues, approves PRs
3. **Agents** - Execute tasks, propose solutions, iterate on feedback

**Jordan is the escalation point, not the decision maker.** The system runs itself.

---

## 🏗️ Architecture

```
┌─────────────────────────────────────────────────────────────────┐
│                         JORDAN (Human)                          │
│                    Sets Specs • Receives Escalations            │
└────────────────────────────┬────────────────────────────────────┘
                             │ only when needed
                             ↓
┌─────────────────────────────────────────────────────────────────┐
│                      SYNAPSE SENTINEL                           │
│                   (The Decision Authority)                      │
├─────────────────────────────────────────────────────────────────┤
│                                                                 │
│  ┌──────────────────┐  ┌──────────────────┐  ┌──────────────┐ │
│  │  CORE            │  │  PREFRONTAL      │  │  KNOWLEDGE   │ │
│  │  (Dumb Pipe)     │  │  CORTEX          │  │  (Memory)    │ │
│  │                  │  │  (Smart Layer)   │  │              │ │
│  │  • Webhooks      │  │  • Judges        │  │  • Events    │ │
│  │  • Event Logs    │  │  • Compliance    │  │  • Synthesis │ │
│  │  • Indexing      │  │  • Decisions     │  │  • Patterns  │ │
│  │  • Rollback      │  │  • Escalation    │  │  • Context   │ │
│  └────────┬─────────┘  └────────┬─────────┘  └──────┬───────┘ │
│           │                     │                    │         │
└───────────┼─────────────────────┼────────────────────┼─────────┘
            │                     │                    │
            ↓                     ↓                    ↓
    ┌───────────────┐     ┌──────────────┐    ┌──────────────┐
    │  GitHub       │     │  Pre-Push    │    │  Agent       │
    │  Webhooks     │     │  Gates       │    │  Queries     │
    └───────────────┘     └──────────────┘    └──────────────┘
```

---

## 📦 Repositories

### [prefrontal-cortex](https://github.com/synapse-sentinel/prefrontal-cortex)
**The Brain** - Executive function and decision-making

- Judges (PR approval, issue closure, pattern validation)
- Compliance gates (pre-push validation)
- Escalation logic
- Alert routing

### [core](https://github.com/synapse-sentinel/core) _(coming soon)_
**The Nervous System** - Event intake and logging

- Webhook listeners
- Event sourcing (Laravel Verbs)
- Audit trails
- Rollback orchestration

### [knowledge](https://github.com/synapse-sentinel/knowledge) _(coming soon)_
**The Memory** - Queryable knowledge base

- Raw event storage
- Auto-synthesis engine
- Pattern library
- Agent query interface

---

## 🎯 What Sentinel Decides

| Decision | Authority |
|----------|-----------|
| Is this PR mergeable? | **Sentinel** |
| Is this issue done? | **Sentinel** |
| Should this deploy? | **Sentinel** |
| Is this in-brand? | **Sentinel** |
| Should agent retry or escalate? | **Sentinel** |
| Is this pattern drift? | **Sentinel** |
| Should human be notified? | **Sentinel** |

**Escalates to Jordan only when:**
- Agent failed 3+ times
- Pattern break it can't resolve
- Spec itself seems wrong
- Something totally novel

---

## 🔄 The Flow

```
1. Jordan writes spec → Issue created
                         ↓
2. Sentinel assigns agent
                         ↓
3. Agent proposes solution (PR)
                         ↓
4. Sentinel judges PR
   ├─ APPROVE → merge, deploy, close issue ✓
   ├─ REJECT → comment, agent retries ↻
   └─ ESCALATE → notify Jordan (rare) ⚠️
```

---

## 🚀 Current Status

**Phase 1: Bootstrap** _(In Progress)_
- [x] Org created
- [x] Prefrontal Cortex initialized
- [x] Judge architecture designed
- [ ] Core repo created
- [ ] Knowledge base structured

**Phase 2: Knowledge Migration** _(Planned)_
- [ ] Migrate claude-history events
- [ ] Migrate personal-knowledge-management
- [ ] Build synthesis engine
- [ ] Test with one project

**Phase 3: Decision Engine** _(Planned)_
- [ ] Implement judges
- [ ] Build pre-push gates
- [ ] Create agent query interface
- [ ] Production deployment

---

## 📚 Documentation

- **[Full Vision & Strategy](https://github.com/synapse-sentinel/.github/blob/main/profile/VISION.md)** - Complete architectural overview
- **[Grok AI Analysis](https://github.com/synapse-sentinel/.github/blob/main/profile/GROK-ANALYSIS.md)** - Strategic validation and recommendations
- **[Implementation Issues](https://github.com/synapse-sentinel/prefrontal-cortex/issues)** - Active development roadmap

---

## 💡 Philosophy

> "Specifications over vibes. Deterministic systems over delegation and hoping."

Sentinel embodies Jordan's philosophy of building systems that run autonomously. It enforces explicit contracts, learns from historical patterns, and makes consistent decisions—freeing humans to focus on strategy instead of tactics.

---

**Built by:** [Jordan Partridge](https://github.com/jordanpartridge)  
**Status:** 🚧 Active Development  
**License:** Private
