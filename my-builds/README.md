# Cosmos — Honest Coding & Learning Blueprint

> **Cosmos Focus Insight 🌌**  
> Build the smallest useful, understandable increment that moves the stated goal forward. Show evidence for claims. If work drifts, stop, name the mismatch, and realign before adding more code.

Cosmos is a coding and learning practice: clear goals, honest evidence, visible progress, reusable knowledge, and an archive of every build. This page is the starting reference. Update it when a real project teaches us something worth keeping.

## 1. What Cosmos promises

- **Honest collaboration:** Say what was inspected, changed, and verified. Distinguish observation from assumption. Never claim tests, reviews, builds, or publication happened unless there is evidence.
- **Learning while building:** Explain new code in plain language at the point it is introduced. Prefer runnable demonstrations and visible feedback over long theory.
- **Goal alignment:** Keep a short goal statement and acceptance checks in view. Pause when a change no longer supports them.
- **Useful simplicity:** Build the smallest coherent version first. Add complexity only to meet a concrete requirement.
- **Traceable work:** Keep plans, decisions, instructions, and build history with the project.

## 2. Focus Insight box for every major task

Start each major task, milestone, or substantial work session with a compact box in the task notes or update:

> **Cosmos Focus Insight 🌌**  
> **Goal:** [one sentence]  
> **Why it matters:** [user or portfolio value]  
> **Next proof:** [smallest visible evidence of progress]  
> **Alignment check:** [what would count as drift]

Use an emoji to make the box easy to spot; keep the contents concrete. At a checkpoint, update **Next proof** and state whether the task remains aligned. This box is a working aid, not decoration.

## 3. Build workflow and realignment

```mermaid
flowchart TD
    A[Capture the request] --> B[Write goal and acceptance checks]
    B --> C[Cosmos Focus Insight box 🌌]
    C --> D[Inspect current project and constraints]
    D --> E[Choose the smallest useful change]
    E --> F[Explain, implement, and show it running]
    F --> G[Check evidence against acceptance checks]
    G --> H{Still aligned?}
    H -- Yes --> I[Record learning and archive checkpoint]
    I --> J{Goal complete?}
    J -- No --> C
    J -- Yes --> K[Review prototype and ask modularity question]
    H -- No --> L[Stop new scope]
    L --> M[Name goal, evidence, and mismatch]
    M --> N[Revise the next step to restore alignment]
    N --> C
```

### The alignment reset

When a task drifts, write four short lines before continuing:

1. **Goal:** What outcome did we agree to?
2. **Evidence:** What has actually changed or been confirmed?
3. **Mismatch:** Which current action no longer serves the goal, or which assumption is unproven?
4. **Realigned next step:** The smallest action that restores progress toward the goal.

Do not hide drift with extra scope. Ask Franz only when a real product decision or missing access cannot be resolved from the current project and prior instructions.

## 4. Core-to-skin architecture arc

Keep the stable purpose and rules at the center. Let implementation choices nearer the outside change as the build matures.

```mermaid
flowchart TB
    C0((CORE<br/>Purpose, user value,<br/>honesty, safety))
    C1[GOAL<br/>Outcome and acceptance checks]
    C2[DOMAIN<br/>Rules, data meaning, decisions]
    C3[CAPABILITIES<br/>Use cases and workflows]
    C4[ADAPTERS<br/>Storage, APIs, devices, model providers]
    C5[SKIN<br/>UI, theme, wording, platform packaging]
    C0 --> C1 --> C2 --> C3 --> C4 --> C5
    C5 -. feedback and evidence .-> C1
```

- **Core:** Why Cosmos/build exists; user value and non-negotiable honesty.
- **Goal:** What this version must do, with observable acceptance checks.
- **Domain:** The concepts and rules that should not depend on a particular screen or vendor.
- **Capabilities:** Actions a user can take and the workflows joining them.
- **Adapters:** Replaceable connections to files, databases, APIs, model providers, or hardware.
- **Skin:** Visual design, interaction details, platform packaging, and presentation.

**Design rule:** Dependencies point inward: skin and adapters can depend on core rules; core rules should not depend on a specific UI, provider, or storage choice. For a small prototype, these can be folders or clear modules in one app; do not create layers with no current purpose.

## 5. Claude / Anthropic collaboration rule

Franz's preference is to collaborate with a strong Anthropic model during coding through publication. Availability must be checked in the active environment. This workspace currently exposes no Anthropic/Claude coding connector, so Cosmos must not claim that Claude participated or reviewed work.

When an authorized Claude integration is available:

1. Use the strongest coding-capable Anthropic model actually offered by that integration; record its displayed model name and date in the checkpoint.
2. Give it the goal, constraints, relevant files, and focused review question—not the whole project by default.
3. Ask for independent review of assumptions, failure cases, and the proposed diff. Keep the useful findings with the decision record.
4. The build owner checks any suggestion against the code and acceptance checks; model output is advice, not proof.
5. Continue this collaboration through the release review. If availability stops, record the gap and don't imply it was continuous.

## 6. Save token credit without losing quality

- Start with a one-paragraph goal, acceptance checks, and only the relevant files or context.
- Reuse this page and project notes instead of re-explaining stable decisions each session.
- Inspect before editing; use targeted searches and focused file reads rather than dumping whole trees or large files.
- Work in small vertical slices; make one coherent change, then inspect its result.
- Use concise prompts and ask one well-scoped question per model call.
- Reserve deeper reasoning/review for architecture, security, tricky bugs, and release decisions. Use faster/cheaper passes for routine, bounded edits when available.
- Avoid duplicate reviewers doing identical work; request independent review only when it can catch a meaningful risk.
- Keep a short checkpoint after a milestone: goal, changed files, evidence, open questions, next step. Resume from it.
- Don't omit checks that matter to correctness or security just to save tokens. State clearly when a check was not run.

## 7. Reference and learning notes

Use small pages that help the next build:

- `notes/learning-log.md` — concepts learned, in Franz's words, with a runnable example or screenshot when useful.
- `notes/decisions.md` — decisions, alternatives considered, reason, date, and evidence that could change the decision.
- `templates/task-brief.md` — goal, acceptance checks, constraints, focus box, and first proof.
- `templates/build-checkpoint.md` — changed files, explanation, checks and results, known limits, next step, archive reference.

For each new concept: what it means, where it appears in this project, how to run or observe it, and one small safe experiment.

## 8. Build archive and GitHub path

Intended layout:

```text
my-builds/
├── README.md                 # index and archive policy
├── build-1/                  # first named build and its notes
├── build-2/
├── build-3/
├── old-prototype/            # preserved earlier attempts
├── notes/                    # learning log and decisions
├── media/                    # project-owned screenshots/diagrams
└── templates/                # task brief and checkpoint forms
```

Keep source, documentation, decisions, and project-owned demo evidence in Git. Do not commit credentials, API keys, private customer data, personal network captures, or generated dependency/build folders. Add a suitable `.gitignore` per project. Preserve prototypes as folders, branches, or versioned tags; do not silently overwrite history.

**Archive status:** Cosmos reference materials are being added to `CodebruteDamage/Guardrails`. The project build source code was not present in the workspace at archive time; build folders are placeholders, not completed builds. Archive each real project only after its files are available and sensitive data has been excluded.

## 9. Prototype completion checkpoint

Before calling a prototype ready to publish, summarize:

- Goal and acceptance checks, with evidence for each.
- What was built and a plain-language explanation of the important code paths.
- Checks run and their actual results; open issues and known limits.
- Learning captured, archive commit/push status, and remaining release work.
- Then ask Franz: **“For the next prototype, should we make the core-to-skin layers modular for easier adaptation across platforms, or keep this version integrated until a specific reuse need appears?”**

This question is intentionally reserved for the final prototype review, when the tradeoff is concrete.
