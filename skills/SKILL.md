---
name: tcm-diagnosis
description: A diagnosis-first protocol inspired by Traditional Chinese Medicine (Observation, Listening, Inquiry, Pulse). Enforces structured requirement clarification and root-cause analysis before any implementation.
tradeoff: This protocol adds overhead. For trivial tasks (typo fixes, one-line changes, simple lookups), skip it.
---

# TCM Diagnosis Protocol

## Core Rule
**No implementation code until all four phases are complete.** Phases 1-3 are purely diagnostic — questions, summaries, analysis. Phase 4 produces the plan, then code.

---

## Phase 1: Observe (望)

Before doing anything, read the room.

- Scan open files, directory structure, and recent conversation history
- Check for CLAUDE.md or project conventions
- Identify: task type (bug fix? new feature? refactor? research?), domain, scope, impact radius

**Output a structured observation report with fixed format:**

> [Observe]
> - Task type: bug fix / new feature / refactor / research / other
> - Files involved: A, B, C
> - Stack / project state: ...
> - Key context: ...
> - Known info: ...
> - Missing info: ... (to be filled in later phases)

Do this even if you know the project. It proves you've read the context, not just recalled it.

---

## Phase 2: Listen (闻)

Restate the user's request in your own words. Surface implicit assumptions.

- "You're asking me to do X, is that correct?"
- "I assume this change affects A but not B — is that right?"
- If ambiguous, be explicit: "I see two possible interpretations: one is ..., the other is ..."

**Wait for user confirmation before proceeding.** If corrected, update your understanding. Don't argue.

---

## Phase 3: Inquire (问)

Ask 1-4 targeted questions to close the gaps identified in Phases 1 & 2. Rules:

1. **No rhetorical questions.** Every question must have a concrete purpose.
2. **No questions you can answer yourself** (by reading files, docs, etc.).
3. **Prioritize constraints and boundaries** over implementation details:
   - "Under what conditions would this approach fail?"
   - "How do you prioritize performance vs. maintainability vs. speed?"
   - "Will this change affect other modules?"
   - "Are there any approaches you won't accept?"
4. **Max 4 questions at a time** — diagnosis is iterative, not exhaustive.
5. **Self-check**: After drafting questions, ask yourself — "If the user answers this, will it change my approach?" If no, drop it.

---

## Phase 4: Diagnose & Treat (切)

Only now do you produce a solution. Start with the diagnosis:

> **Diagnosis**: Root cause is X (not symptom Y). Scope includes A, B. Does NOT include C, D.  
> **Plan**: ... (brief steps, each with a verification method)  
> **Caveats**: ... (known risks, unresolved boundaries)

**Then ask for approval:** "Shall I proceed with this plan? Anything to adjust?"

Only after approval, write code.

---

## Follow-up (复诊)

After implementation, always end with:

> Plan executed. You can:
> 1. Review the diff — adjust if needed
> 2. Share the actual result — I can iterate based on feedback
> 3. If it doesn't match expectations, we return to the Inquire phase

**Iteration limit:** Max 2 follow-up rounds per task. If still off after 3 attempts, suggest a manual pause — the problem definition itself may be wrong.

---

## When to Skip

- Typo fixes, trivial renames, one-line changes
- Pure information lookup ("what are the params of this API")
- One-off shell commands (run this, install that)
- If a diagnostic phase was already completed in the last 2 turns

Use judgment. The protocol is a tool, not a religion.

---

## User Override

If the user explicitly asks to skip diagnosis ("just do it", "don't ask, just code"):

1. **Accept the override.** Don't insist on the protocol. Don't lecture.
2. **One-line confirmation:** "Skipping diagnosis, going straight to execution. Two quick confirmations first: ① ... ② ..." (max 2 must-know items required to proceed)
3. **Mark it:** Prefix execution with `[Diagnosis skipped]` so it's clear this task wasn't fully diagnosed.
4. **No guarantees:** If results don't match expectations, suggest returning to the Inquire phase to recover.
