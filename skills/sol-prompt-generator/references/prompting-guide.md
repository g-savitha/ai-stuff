# Prompting guidance for GPT-5.6 Sol

Use this guide when adapting prompts, tool descriptions, agent instructions, or prompt stacks to GPT-5.6 Sol or the GPT-5.6 family. Pair it with the current [GPT-5.6 model guide](https://developers.openai.com/api/docs/guides/latest-model?model=gpt-5.6) for API details, limits, pricing, and feature availability.

## Simplify first

Start with a prompt and tool set that works. Remove one group of instructions, examples, or tools at a time and rerun the same representative evaluations.

Trim repeated rules, redundant examples, obsolete process instructions, and unrelated tools. Keep the user-visible outcome, success criteria, stopping conditions, safety/business/evidence/permission constraints, context-dependent tool routing, required output, and validation requirements. Resolve contradictions explicitly; conflicting prompt contracts often create more instability than missing detail.

## Outcome and stopping conditions

Describe the destination rather than prescribing every step. Prefer:

```text
Resolve the request end to end.

Success means:
- make the decision from the available policy and evidence;
- complete allowed in-scope actions;
- return completed_actions, result, and blockers;
- if required evidence is missing, ask for the smallest missing field.
```

Use ALWAYS, NEVER, must, and only for true invariants. For judgment calls, use decision rules. Preserve explicit user values and provide criteria when the correct value must be inferred.

Use a stopping rule such as:

```text
Resolve the request in the fewest useful tool loops, but do not let loop minimization outrank correctness, required evidence, calculations, or citations. After each result, check whether the core request can now be answered. If yes, answer; if required evidence is missing, name it and use the smallest useful fallback.
```

## Personality and collaboration

Define personality (tone, warmth, directness, formality, empathy, polish) separately from collaboration style (when to ask, assume, act, explain tradeoffs, verify, and handle uncertainty). Keep both short. Use `text.verbosity` for the default detail level and prompt for task-specific structure or length.

When shortening an answer, preserve required facts, decisions, caveats, and next steps; remove introductions, repetition, generic reassurance, and optional background first. For editing or rewriting, preserve the requested artifact, length, structure, genre, and factual claims before improving clarity or flow. Do not add claims, sections, or promotional tone unless requested.

## Autonomy and approval boundaries

Use one compact policy:

```text
For requests to answer, explain, review, diagnose, or plan, inspect relevant materials and report the result; do not implement changes unless asked. For requests to change, build, or fix, make requested in-scope local changes and run relevant non-destructive validation. Require confirmation for external writes, destructive actions, purchases, or material scope expansion.
```

Name safe local actions when useful, such as reading files, inspecting logs, editing in-scope code, and running tests. For long-running work, distinguish research, design, implementation, review, and external coordination so the model does not silently move between layers.

## Tool routing

Expose only task-relevant tools. Each tool description should state what it does, when to use it, important return fields, and error behavior. Resolve required discovery, retrieval, and validation prerequisites before taking an action. Parallelize independent reads; keep dependent decisions sequential; synthesize after parallel retrieval. If results are empty, partial, or suspiciously narrow, try one or two meaningful fallbacks before concluding that nothing exists.

### Programmatic Tool Calling

Use PTC for bounded, deterministic reduction such as filtering, joining, sorting, ranking, deduplication, aggregation, batching, or repeated validation. Prefer direct calls when one call is sufficient, intermediate results are small, each result changes the next decision, approval is required, citations/native artifacts must be preserved, or semantic judgment is needed.

If using PTC, specify the bounded stage, eligible read-only tools, output schema, retry limit, stop condition, and handoff back to direct model judgment. Test both the `program_output` and final assistant message. Compare direct and programmatic routes on the same representative tasks for correctness, completeness, evidence, tokens, latency, cost, calls, and retries.

## Grounding, citations, and retrieval budgets

Make citation behavior explicit. For ordinary Q&A, start with one broad search using short, discriminative keywords. Retrieve again only when a required fact, owner, date, ID, or source is missing; exhaustive coverage/comparison is requested; a named artifact must be read; or an important claim would otherwise be unsupported. Do not search again merely to improve phrasing or add nonessential examples.

For research, cite only retrieved sources, attach citations to supported claims, label inference separately, state source conflicts, and narrow the answer or report missing evidence instead of guessing. For creative drafts, separate source-backed facts from creative language and never invent names, metrics, dates, roadmap status, outcomes, or capabilities.

## Long-running state

For multi-step tasks, give a short visible preamble before the first tool call and sparse updates at major phase changes. Do not narrate routine tool calls. Preserve assistant phase values when replaying history. Compact after milestones rather than every turn, and keep the prompt behaviorally consistent after compaction. Use persisted reasoning only while the objective, assumptions, and priorities remain stable; stale reasoning can add tokens, latency, and anchoring.

Keep reusable prompt prefixes stable when prompt caching matters. Add cache breakpoints only when measured behavior and cost improve.

## Reasoning effort

Establish a baseline before changing reasoning effort. Preserve the current GPT-5.5/GPT-5.4 setting, then compare that setting with one level lower on representative tasks. Use low for latency-sensitive work when quality holds, medium as a balanced default, and high/xhigh or max only when evaluations show a meaningful gain. Before increasing effort, check for missing success criteria, dependency rules, tool routing, or verification loops.

## Frontend and visual tasks

Provide product context, preserve the existing design system, and name important states and constraints. For incremental changes, inspect and preserve tokens/components/patterns, responsive behavior, and expected states; do not add features or decoration unless requested. Render and inspect before finalizing. Choose original image detail for large, dense, or coordinate-sensitive vision/OCR/computer-use tasks when the extra cost is justified.

## Validation

Give the model access to relevant validation tools and state what matters. For code, run targeted tests, type/lint checks, builds, and a minimal smoke test as applicable. For visual artifacts, render and inspect clipping, spacing, missing content, and consistency. If validation cannot run, explain why and name the next-best check.

## Suggested structure

```text
Role: [function and context]
Personality: [tone and collaboration style]
Goal: [user-visible outcome]
Success criteria: [what must be true before responding]
Constraints: [policy, safety, business, evidence, and side-effect limits]
Tools: [which tools to use, when, and what not to use]
Output: [sections, length, format, and tone]
Stop rules: [when to retry, fallback, abstain, ask, or stop]
```

## Migration workflow

When migrating an existing application to GPT-5.6:

1. Switch the model and preserve current reasoning effort.
2. Run representative evaluations before changing the prompt.
3. Remove obsolete scaffolding, repeated instructions, and irrelevant tools.
4. Add only the smallest targeted instruction that fixes a measured regression.
5. Rerun the same cases after each prompt or reasoning change.

Do not rewrite a working prompt stack all at once; otherwise model, reasoning, prompt, tools, and runtime effects cannot be separated.

