# Synapse Sentinel: The Central Nervous System for Jordan's Operation

## Overview

**Synapse Sentinel** is not just logging or compliance - it's the **central nervous system and decision authority** for an entire development operation. It receives all signals (events), makes autonomous decisions, and only escalates to humans by exception.

### The Name

- **Synapse** = nervous system (receives all signals from every part of the operation)
- **Sentinel** = watchdog (guards against drift, enforces patterns, makes decisions)

## Core Philosophy

> "Agents propose. Sentinel disposes."

**Three-Layer Authority:**
1. **Jordan** - Sets specifications and strategy
2. **Sentinel** - Enforces specs, makes tactical decisions, closes issues, approves PRs
3. **Agents** - Execute tasks, propose solutions, iterate on feedback

**Jordan is the escalation point, not the decision maker.** The system runs itself.

---

## Architecture

### System-Wide View

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
                                  ↓                    ↑
                          ┌──────────────────────────────┐
                          │    AI AGENTS (Workers)       │
                          │  • Test Generator            │
                          │  • Issue Creator             │
                          │  • PR Builder                │
                          │  • Pattern Matcher           │
                          └──────────────────────────────┘
```

---

## The Three Parts

### 1. CORE (The Nervous System)

**Role:** Dumb pipe that receives, logs, and routes everything.

```
synapse-sentinel/core/
├── listeners/          # Subscribe to all Laravel Verbs events
├── webhooks/           # Receive GitHub, CI/CD, external events
├── logging/            # Audit trail (ground truth)
├── indexing/           # Organize by project, domain, tool, date
└── rollback/           # Undo/revert orchestration
```

**What it captures:**
- Every Laravel Verbs event
- Every tool call from Claude Code
- Every GitHub webhook (PR, issue, commit, deployment)
- Every CI/CD pipeline run
- Every error, state change, decision

**It's the source of truth.** Everything flows through Core.

---

### 2. PREFRONTAL CORTEX (The Brain)

**Role:** Executive function - makes decisions based on what Core knows.

```
synapse-sentinel/prefrontal-cortex/
├── judges/             # Decision-making logic
│   ├── PRJudge.php    # Should this PR merge?
│   ├── IssueJudge.php # Is this issue done?
│   ├── PatternJudge.php # Is this pattern drift?
│   └── BrandJudge.php # Does this follow conventions?
│
├── compliance/         # Pre-push gates
│   ├── PatternGate.php
│   ├── TestGate.php
│   └── DependencyGate.php
│
├── decisions/          # Decision execution
│   ├── ApproveAndMerge.php
│   ├── CloseIssue.php
│   └── EscalateToHuman.php
│
└── alerts/             # Notification routing
    ├── SlackAlert.php
    └── GitHubNotification.php
```

**Decision Matrix:**

| Decision | Who Decides |
|----------|-------------|
| Is this PR mergeable? | Sentinel |
| Is this issue done? | Sentinel |
| Should this deploy? | Sentinel |
| Is this in-brand? | Sentinel |
| Should agent retry or escalate? | Sentinel |
| Is this pattern drift? | Sentinel |
| Should human be notified? | Sentinel |

**Escalates to Jordan only when:**
- Agent failed 3+ times
- Pattern break it can't resolve
- Spec itself seems wrong
- Something totally novel

---

### 3. KNOWLEDGE (The Memory)

**Role:** Queryable memory - agents ask Sentinel what they need to know.

```
synapse-sentinel/knowledge/
├── events/             # Raw logs from Core (rarely accessed)
│   ├── 2025-12-13/    # By date
│   │   └── nodes/     # Individual tool calls
│   └── index/         # By project, domain, tool
│
├── synthesis/          # Auto-generated insights
│   ├── chat/          # Per-project summaries
│   │   └── 2025-12-13-insights.md
│   ├── patterns/      # Cross-project patterns
│   └── decisions/     # Historical decisions
│
├── context/            # Curated knowledge
│   ├── meta/          # Architecture, ADRs
│   ├── projects/      # Project-specific docs
│   ├── areas/         # Domain knowledge
│   └── resources/     # Reusable patterns
│
└── query/              # Agent query interface
    └── SentinelQuery.php
```

**How agents use it:**

```php
// Agent needs to know how tests are written
$patterns = Sentinel::query()
    ->repo('github-zero')
    ->type('test_files')
    ->recent(30)
    ->get();

// Agent needs authentication patterns
$auth = Sentinel::search('OAuth flow')
    ->technologies(['Laravel', 'Saloon'])
    ->examples();
```

---

## Decision Flow Example

### Scenario: Agent Opens PR

```
1. Agent creates PR
   ↓
2. GitHub webhook → Core receives event
   ↓
3. Core logs event, indexes by project/type
   ↓
4. Prefrontal Cortex judges PR:
   ├── PRJudge runs rules:
   │   ├── Tests pass? ✓
   │   ├── Pattern compliant? ✓
   │   ├── Dependencies approved? ✗ (guzzle not approved)
   │   └── Brand compliant? ✓
   │
   └── Verdict: REJECT
   ↓
5. Sentinel comments on PR:
   "Rejected: Dependency guzzlehttp/guzzle requires approval.
    Agent: Remove unapproved dependency and retry."
   ↓
6. Agent fixes, pushes again
   ↓
7. Pre-push gate validates locally BEFORE push
   ↓
8. All checks pass → Sentinel approves, merges, deploys
   ↓
9. Sentinel closes issue: "Completed. Deployed to production."
```

**Jordan never involved unless something novel happens.**

---

## Pre-Push Gates

Before code ever reaches GitHub, Sentinel validates:

```
git push
    ↓
synapse-sentinel pre-push hook
    ↓
├── Pattern check: Does this belong in this repo?
├── Brand check: Follows naming conventions?
├── Test coverage: All tests pass?
├── Dependency check: Only approved packages?
└── Verdict: PASS → push | REJECT → abort
```

**Example rejection:**

```
✗ Pattern validation failed
  - Service logic found in Controller (should be in app/Services)
  
✗ Dependency validation failed
  - guzzlehttp/guzzle added without approval
  
REJECTED - Fix issues before pushing
```

---

## Knowledge Synthesis

**The Problem:** Raw event logs are too granular (1000+ individual tool calls).

**The Solution:** Auto-synthesize insights.

```
Raw Events (nodes/)
    ↓
Synthesis Engine (Grok via Prism?)
    ↓
Summaries (synthesis/)
```

**Command:**
```bash
php artisan sentinel:synthesize --project=chat --date=2025-12-13
```

**Output:** `knowledge/synthesis/chat/2025-12-13-insights.md`

```markdown
# Synthesis: chat project (2025-12-13)

## Activity Summary
- 358 tool calls across 8 hours
- Primary domains: shell (40%), testing (25%), code (20%)

## Key Patterns
1. Test-First Development - All features started with tests
2. Trait Composition - Preferred over inheritance
3. Laravel Prompts - Used for all interactive commands

## Decisions Made
- Component architecture approved
- Namespace registry pattern chosen
```

---

## Domain Organization

### GitHub Organizations

```
jordanpartridge/           # Personal brand, hobby projects
partridgerocks/            # Life strategy, business planning
the-shit/                  # AI automation, core tools
  ├── chat                 # Universal intake, BYOI interface
  ├── brain                # Claude knowledge, philosophy
  ├── agent                # GitHub Action runtime
  └── worker               # Worker execution layer
  
synapse-sentinel/          # Compliance, watchdog, authority
  ├── core                 # Event intake, logging
  ├── prefrontal-cortex    # Decision engine
  └── knowledge            # Memory, patterns
  
conduit-ui/                # Frontend components, UI
bulletproof-connectors/    # API integrations
```

### Event Flow Across Orgs

```
┌──────────────────────────────────────────────────────────────────┐
│                    SYNAPSE SENTINEL (Center)                     │
│                  Watches everything, decides everything          │
└────────────┬─────────────────────────────────────┬───────────────┘
             │                                     │
    ┌────────▼────────┐                   ┌───────▼────────┐
    │  the-shit/*     │                   │ partridgerocks │
    │  (Builders)     │                   │ (Life Strategy)│
    │                 │                   │                │
    │  Events logged  │                   │ Events logged  │
    │  Patterns       │                   │ Goals tracked  │
    │  learned        │                   │                │
    └────────┬────────┘                   └───────┬────────┘
             │                                     │
             └────────────┬────────────────────────┘
                          │
                ┌─────────▼──────────┐
                │  jordanpartridge   │
                │  (Personal Brand)  │
                │                    │
                │  Events logged     │
                └────────────────────┘
```

---

## AI Integration Strategy

### Current Thinking: Grok via Prism

**Why Grok?**
- Real-time X/Twitter context (if needed)
- Fast inference
- Cost-effective for synthesis tasks
- xAI API available

**Why Prism?**
- Universal LLM adapter (already at PSTrax)
- Swap providers without code changes
- Route by task type:
  - Grok → Synthesis, pattern detection
  - Claude → Code generation, complex reasoning
  - GPT-4 → Specific use cases

**Integration Points:**

```php
// Synthesis Engine
Prism::route('synthesis')
    ->provider('grok')
    ->synthesize($events);

// Decision Making
Prism::route('judgment')
    ->provider('grok')
    ->judge($pullRequest);

// Pattern Detection
Prism::route('patterns')
    ->provider('grok')
    ->detectDrift($codeChange);
```

---

## Migration Path

### Phase 1: Bootstrap (Week 1)
- [ ] Create synapse-sentinel org (exists)
- [ ] Create synapse-sentinel/core repo
- [ ] Create synapse-sentinel/knowledge repo structure
- [ ] Verify prefrontal-cortex is ready

### Phase 2: Migrate Knowledge (Week 2)
- [ ] Move claude-history/nodes → core/events
- [ ] Move personal-knowledge-management → knowledge/context
- [ ] Build synthesis script
- [ ] Test with one project (chat)

### Phase 3: Decision Engine (Week 3)
- [ ] Implement Judge system
- [ ] Build pre-push gates
- [ ] Create agent query interface
- [ ] Test with real PR

### Phase 4: Production (Week 4)
- [ ] Deploy to production
- [ ] Archive old repos
- [ ] All agents use Sentinel
- [ ] Jordan only sees escalations

---

## Success Metrics

**For Jordan:**
- Notifications reduced by 80%
- Only sees novel/critical issues
- System runs autonomously

**For Agents:**
- Clear accept/reject feedback
- Historical context available
- Faster iteration (no waiting for human)

**For System:**
- All events captured
- Patterns learned automatically
- Drift detected early
- Rollback capability for any change

---

## Questions for Grok

1. **Synthesis Strategy:** What's the optimal way to synthesize 1000+ raw event logs into actionable insights?

2. **Pattern Detection:** How should Sentinel identify when code deviates from established patterns?

3. **Decision Logic:** What rules should judges use to approve/reject PRs autonomously?

4. **Escalation Criteria:** When should Sentinel escalate vs handle autonomously?

5. **Query Optimization:** How to make agent queries fast (<100ms) across large event corpus?

6. **Real-time vs Batch:** Should synthesis happen real-time or nightly batch?

7. **Context Window:** How much historical context do agents need for good decisions?

8. **Multi-Project Learning:** How to apply patterns learned in one project to others?

---

## The End State

```
Jordan writes spec
    ↓
Issue created in GitHub
    ↓
Sentinel assigns to appropriate agent
    ↓
Agent proposes solution (PR)
    ↓
Sentinel judges PR
    ├─ APPROVE → merge, deploy, close issue
    ├─ REJECT → comment, agent retries
    └─ ESCALATE → notify Jordan (rare)
```

**The system runs itself. Jordan focuses on strategy, not tactics.**
