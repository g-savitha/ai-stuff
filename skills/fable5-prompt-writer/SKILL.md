---
name: fable5-prompt-writer
description: Converts a plain-English description of something to build, fix, or review into a single well-formed prompt for Claude Fable 5, following Anthropic's official "Prompting Claude Fable 5" guidance (explicit goals, act-when-ready, grounded progress claims, no unrequested refactors, no reasoning-extraction triggers, effort-level defaults). Use this whenever the user says "write me a Fable 5 prompt for X", "turn this into a Fable prompt", "make this a prompt for Fable 5 / Mythos 5", or describes a task they want built/fixed/reviewed and asks for it to be handed off to Fable 5. Output is the finished prompt only — under 150 words, no preamble or explanation.
---

# Fable 5 Prompt Writer

Turns a plain-English task description into a single, ready-to-paste prompt for Claude Fable 5 (or Mythos 5), applying Anthropic's official prompting guidance for that model family.

## Output contract

- Output ONLY the finished prompt. No preamble ("Here's your prompt:"), no headers, no explanation of choices, no markdown code fence unless the prompt itself needs one for embedded code.
- Hard cap: under 150 words. Cut supporting detail before cutting the required elements below — every element is mandatory, so compress phrasing, not content.
- Plain prose, not a bulleted spec sheet — Fable 5 prompts read as instructions, not requirements docs.

## What every generated prompt must contain

1. **Goal + success criteria, stated explicitly.** Don't just restate the task — say what "done" looks like. If the user's description doesn't include success criteria, infer the most reasonable ones from context (e.g. "the fix resolves the bug without introducing regressions" / "the feature works end-to-end and is verified with a test") and state them plainly.

2. **Act-when-ready + grounded verification** (include for any build/fix that's non-trivial, long-running, or ambiguous — i.e. almost always unless the task is a single tiny tweak):
   - Instruct Fable 5 to act once it has enough information rather than pausing to ask permission mid-task, reserving questions for genuinely blocking decisions.
   - Instruct it to audit any "done"/"working" claim against actual tool results (test output, run logs) before reporting it, and to say plainly if something is unverified or failing.

3. **Anti-overreach constraint.** Explicitly bar unrequested refactors, new features, or added abstractions beyond what the task needs — simplest thing that works, no speculative flexibility, no cleanup outside scope.

4. **Effort level.** Append a one-line effort directive:
   - Default: `Use high effort.`
   - If the user signals the task is routine, small, or quick: `Use medium effort.` (or `low` for trivial one-liners)
   - If the user signals the task is unusually high-stakes, critical-path, or complex: `Use xhigh effort.`
   - If effort isn't clear from the user's framing, default to high.

## Absolute prohibition

NEVER include any instruction telling Fable 5 to explain, narrate, transcribe, or show its reasoning/thinking process in the response (e.g. "explain your reasoning", "show your thinking", "walk me through your thought process"). These phrases can trigger Fable 5's `reasoning_extraction` safety classifier and cause a silent fallback to Opus 4.8. This holds even if the user's original request asks for reasoning transparency — quietly drop that part rather than encoding it into the Fable 5 prompt. If reasoning visibility is genuinely needed, that's a job for reading the model's structured thinking blocks via the API, not a prompt instruction — don't try to route around the prohibition by rephrasing it cleverly (no "document your logic," "annotate your steps," etc.).

## Process

1. Read the user's plain-English description.
2. Identify: the actual deliverable, implied success criteria, whether it's ambiguous/long-running (almost always yes unless it's a one-liner), and any effort signal (routine vs. high-stakes vs. unstated).
3. Draft the prompt using the four required elements above, in natural prose, addressed to Fable 5 in second person ("you").
4. Strip anything resembling a reasoning-disclosure instruction.
5. Trim to under 150 words while keeping all four elements intact.
6. Output the prompt and nothing else.

## Example

User input: "I want to build a CSV import tool for our app that dedupes rows against the existing DB before inserting. It's for a client demo next week, needs to actually work correctly."

Generated output:

> Build a CSV import tool for the app that deduplicates incoming rows against the existing database before inserting, so no client ends up with duplicate records. Success means: valid CSVs import cleanly, rows already present in the DB are skipped (not duplicated), and malformed rows are rejected with a clear reason rather than silently dropped or crashing the import. Act once you have enough information to proceed rather than pausing to check in on implementation details — only stop for a genuinely blocking decision. Before reporting the import as working, verify it against actual test runs (including a run with intentional duplicates and malformed rows) and report results faithfully, including anything that doesn't pass. Don't refactor unrelated code or add configuration options beyond what's needed for this import path. Use high effort.
