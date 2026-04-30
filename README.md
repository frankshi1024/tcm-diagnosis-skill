# TCM Diagnosis Protocol (中医四诊协议)

> A diagnosis-first protocol for LLMs, inspired by Traditional Chinese Medicine's four diagnostic methods: Observation (望), Listening (闻), Inquiry (问), and Pulse-taking (切).
> Enforces structured requirement clarification and root-cause analysis before any implementation.

## Origins

This protocol emerged from three converging lines of thought:

**Line one: long-term practice.** Using LLMs since the GPT-3 / early Stable Diffusion era. Back then, you had to write exhaustive positive prompts and negative prompts to get usable results. As models improved, verbose prompts gradually shifted from being helpful to being constraining — the more detail you gave, the less room the model had to perform. The key question shifted from "how to write better prompts" to **how to let the model decide what to do at the right time, with the right amount of guidance**. This observation condensed into an analogy: the old Chinese medicine doctor — not someone who memorizes more prescriptions, but someone who knows which prescription fits the moment and in what dosage.

**Line two: rule writing.** Over years of daily use, a personal set of behavioral rules crystallized (Beauty in Concrete, Clarity First, the User Isn't Always Right, Expert Above Experts, Simplicity Above All, etc.). These rules cover writing quality, information presentation, critical thinking, and language constraints — essentially an early practice of "LLM behavioral guidelines."

**Line three: the Karpathy format.** The [andrej-karpathy-skills](https://github.com/forrestchang/andrej-karpathy-skills) repo provides a concrete implementation pattern: SKILL.md + frontmatter + structured principles. Its four principles (Think Before Coding / Simplicity First / Surgical Changes / Goal-Driven Execution) solve **execution quality** — how to write good code. But they miss an earlier step: **whether to do it at all, and what exactly to do.**

**Where tcm-diagnosis fits:** Karpathy governs "prescription quality"; tcm-diagnosis governs "diagnosis first." They complement each other, not replace each other.

## Core Problem

> Most quality issues in LLM output stem not from "how to write the code," but from "writing before understanding."

Current LLMs are capable enough, but when the user's goal is unclear, the model's发散 thinking can easily go off track. The traditional fix is more detailed prompts — but that path narrows: detailed prompts themselves become a constraint.

tcm-diagnosis takes a different approach: **instead of more rules to constrain the LLM, use a diagnostic process to align the user's and the model's understanding of the problem.**

## Protocol Overview

```
Observe (望) → Listen (闻) → Inquire (问) → Diagnose & Treat (切)
                                                    ↓
                                             ← Follow-up (复诊) ──
```

- **Phase 1 · Observe**: Read all context (files, project structure, conversation history), output a structured observation report
- **Phase 2 · Listen**: Restate the user's request, surface implicit assumptions, wait for confirmation
- **Phase 3 · Inquire**: Ask 1-4 targeted questions to fill information gaps
- **Phase 4 · Diagnose & Treat**: Present diagnosis and plan, get approval, then write code
- **Follow-up**: Leave a feedback loop for iteration

**Core constraint:** Phases 1-3 are diagnosis only — no code. Phase 4 presents the plan first, executes after approval.

---

## Preview

Actual output from a real project — the LLM performs structured diagnosis before writing any code:

| Phase | Screenshot |
|-------|-----------|
| **Listen** — Restate requirements, surface assumptions | ![Listen phase - confirming version relationship](images/tcm-diagnosis-output.png) |
| **Inquire** — Ask targeted questions | ![Inquire phase - metadata granularity](images/tcm-inquiry-phase.png) |
| **Diagnose** — Dig deeper into constraints | ![Diagnose phase - automation scenarios](images/tcm-plan-and-code.png) |

See [skills/SKILL.md](skills/SKILL.md) (English) and [skills/SKILL_ZH.md](skills/SKILL_ZH.md) (Chinese).  
Quick reference: [PROTOCOL.md](PROTOCOL.md) | [PROTOCOL_ZH.md](PROTOCOL_ZH.md)

## Comparison with Karpathy

| Dimension | Karpathy | TCM Diagnosis |
|-----------|----------|---------------|
| Core focus | Execution quality (how to code) | Diagnosis first (what & why) |
| Constraint style | Soft guidelines | Hard protocol (no code before diagnosis) |
| Phase relationship | 4 parallel principles | 4 sequential phases, ordered |
| User interaction | More concise output | Confirm + inquire before output |
| Best suited for | All coding tasks | Vague requirements, cross-module changes, high-risk ops |
| Design philosophy | Reduce common LLM mistakes | Align user-LLM problem understanding |

## Installation

**As CLAUDE.md:**

Append the content of `skills/SKILL.md` (English) or `skills/SKILL_ZH.md` (Chinese) to your project's `CLAUDE.md`.

**As Claude Code Skill:**

```bash
cp skills/SKILL.md .claude/skills/tcm-diagnosis/SKILL.md
```

**As Cursor Rule:**

Copy to `.cursor/rules/` directory.

## When to Use / When to Skip

**Use when:**
- Vague or open-ended requests ("optimize performance", "refactor this module")
- Cross-file changes
- High-risk operations (data safety, production environment)
- You're unsure whether the LLM understands correctly

**Skip when:**
- Typo fixes, simple renames, one-line changes
- Pure information lookup
- One-off shell commands
- Diagnosis was already done in the last 2 turns

## License

MIT. See [LICENSE](LICENSE) for details.
