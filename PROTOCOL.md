# TCM Diagnosis Protocol · Quick Reference

## Core Rule

**Phases 1-3: diagnosis only, no code. Phase 4: present plan first, execute after approval.**

---

## The Four Phases

```
Observe → Listen → Inquire → Diagnose → Follow-up ⇢ Done
                                  ↓               ↑
                              Rejected ──── Back to Inquire
```

### 1. Observe (望)
Scan context: open files, project structure, conversation history, CLAUDE.md
→ Output: structured observation report (task type / files involved / known info / missing info)

### 2. Listen (闻)
Restate the request in your own words, surface hidden assumptions
→ Wait for confirmation. If corrected, update — don't argue

### 3. Inquire (问)
Ask 1-4 targeted questions to fill gaps
→ Ask about constraints and boundaries, not implementation details
→ Self-check: "Will the answer change my approach?" If no, drop it

### 4. Diagnose & Treat (切)
Diagnosis → Plan (each step with verification) → Get approval → Write code

---

## Follow-up

Leave feedback loop after execution. Max 2 iterations per task.
Still off after 3 attempts → pause, suggest manual intervention.

---

## User Override

User says "just do it, skip diagnosis":
1. Accept the override
2. Max 2 must-know items to confirm
3. Mark with `[Diagnosis skipped]`
4. No guarantees on first try

---

## When to Skip

- Typo fixes, simple renames, one-line changes
- Pure information lookup
- One-off shell commands
- Diagnosis done in last 2 turns
