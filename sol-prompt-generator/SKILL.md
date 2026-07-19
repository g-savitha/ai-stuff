---
name: sol-prompt-generator
description: Generate, review, migrate, and refine prompts for GPT-5.6 Sol and the GPT-5.6 family. Use when designing system prompts, developer instructions, agent prompts, tool descriptions, prompt stacks, or prompt-evaluation plans; when simplifying an existing prompt; or when tuning autonomy, grounding, citations, reasoning effort, verbosity, programmatic tool calling, or validation behavior.
---

# Sol Prompt Generator

## Purpose

Design prompts that state the desired outcome, success criteria, evidence, constraints, available tools, output shape, and stopping conditions while leaving GPT-5.6 room to choose an efficient path.

Read [references/prompting-guide.md](references/prompting-guide.md) when creating or substantially revising a prompt. Treat it as guidance, not a rigid checklist: preserve requirements that affect behavior and remove repetition that does not.

## Workflow

1. Identify the prompt's job, target user, model/runtime context, and user-visible outcome.
2. Inspect the existing prompt stack, tools, examples, and representative failures when available. Preserve explicit user requirements and measured invariants.
3. Draft or revise the prompt using only the sections that change behavior:
   - Role and context
   - Personality and collaboration style
   - Goal
   - Success criteria
   - Constraints and approval boundaries
   - Tools and routing rules
   - Evidence/citation requirements
   - Output format
   - Stop, retry, fallback, and abstention rules
4. Remove duplicated instructions, stale scaffolding, irrelevant tools, unnecessary examples, and contradictions. Use absolute language only for true invariants.
5. Make autonomy explicit: distinguish read-only investigation, local implementation, external writes, destructive actions, purchases, and scope expansion. Require confirmation only for actions that genuinely need it.
6. Add task-specific controls for grounded research, long-running work, programmatic tool calling, frontend/visual work, or reasoning effort only when relevant.
7. Include a completion bar and a verification loop. For migrations, recommend preserving the baseline model/reasoning setting and changing one variable at a time.

## Output

Lead with the result. For a generated prompt, provide the prompt in a copyable code block, followed by brief implementation notes only when they affect use. For a review, report the highest-impact issues first and provide a revised prompt. For an evaluation plan, define representative cases, pass/fail criteria, and measurable comparisons such as correctness, completeness, citations, tokens, latency, cost, calls, retries, and user-visible output.

If key context is missing, make a low-risk assumption and label it. Ask only for information that materially changes the prompt; otherwise produce a useful draft with clearly marked placeholders.

## Quality bar

Before finishing, check that the prompt:

- defines an observable outcome and completion bar;
- preserves safety, permission, privacy, business, and evidence constraints;
- avoids conflicting or redundant instructions;
- routes tools by prerequisites, dependencies, and result quality;
- handles missing, partial, conflicting, or unsupported evidence without guessing;
- specifies what to return and when to stop;
- does not silently expand scope or turn a review/diagnosis into implementation.

