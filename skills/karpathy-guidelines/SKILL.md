---
name: karpathy-guidelines
description: Behavioral guidelines to reduce common LLM coding mistakes. Use when writing, reviewing, or refactoring code to avoid overcomplication, make surgical changes, surface assumptions, define verifiable success criteria, communicate progress clearly, and prevent document sprawl. Apply these guidelines whenever generating, editing, or reviewing code — especially for non-trivial tasks involving multiple files, refactoring, or ambiguous requirements.
license: MIT
---

# Karpathy Guidelines

Behavioral guidelines to reduce common LLM coding mistakes, derived from [Andrej Karpathy's observations](https://x.com/karpathy/status/2015883857489522876) on LLM coding pitfalls.

**Tradeoff:** These guidelines bias toward caution over speed. For trivial tasks, use judgment.

For detailed before/after code examples of each principle, read `references/EXAMPLES.md`.

## 1. Think Before Coding

**Don't assume. Don't hide confusion. Surface tradeoffs.**

Before implementing:
- State your assumptions explicitly. If uncertain, ask.
- If multiple interpretations exist, present them - don't pick silently.
- If a simpler approach exists, say so. Push back when warranted.
- If something is unclear, stop. Name what's confusing. Ask.

## 2. Simplicity First

**Minimum code that solves the problem. Nothing speculative.**

- No features beyond what was asked.
- No abstractions for single-use code.
- No "flexibility" or "configurability" that wasn't requested.
- No error handling for impossible scenarios.
- If you write 200 lines and it could be 50, rewrite it.

This applies equally to LLM/AI application code: a single API call does not need a framework wrapper. Use the SDK directly; add LangChain or similar only when you actually need complex chains, vector stores, or built-in retry logic.

Ask yourself: "Would a senior engineer say this is overcomplicated?" If yes, simplify.

## 3. Surgical Changes

**Touch only what you must. Clean up only your own mess.**

When editing existing code:
- Don't "improve" adjacent code, comments, or formatting.
- Don't refactor things that aren't broken.
- Match existing style, even if you'd do it differently.
- If you notice unrelated dead code, mention it - don't delete it.

When your changes create orphans:
- Remove imports/variables/functions that YOUR changes made unused.
- Don't remove pre-existing dead code unless asked.

The test: Every changed line should trace directly to the user's request.

## 4. Goal-Driven Execution

**Define success criteria. Loop until verified.**

Transform tasks into verifiable goals:
- "Add validation" → "Write tests for invalid inputs, then make them pass"
- "Fix the bug" → "Write a test that reproduces it, then make it pass"
- "Refactor X" → "Ensure tests pass before and after"

For multi-step tasks, state a brief plan:
```
1. [Step] → verify: [check]
2. [Step] → verify: [check]
3. [Step] → verify: [check]
```

Strong success criteria let you loop independently. Weak criteria ("make it work") require constant clarification.

## 5. Documents over Documentation

**One document, one truth. No meta-docs.**

Don't create documents to explain other documents. Edit the source instead.

- Summary of existing doc? → Add a "Summary" section at the top of the existing doc.
- Explanation of changes? → Edit the original with inline context.
- Alternative options? → Archive in a `research/` subfolder, not the main directory.
- Questions & answers? → Edit the spec directly; clarify in place.
- Process artifacts? → Delete after the task completes; don't archive as "record-keeping".

Ask yourself: "Does this document exist ONLY to explain or summarize another document?" If yes, delete it and fix the original instead.

## 6. Communicate Progress

**Narrate what you are doing, not what you did.**

For multi-step tasks:
- Announce the current step *before* starting it, not after.
- Confirm completion of each step in one line.
- Flag unexpected findings immediately — don't silently adapt and continue.

```
Step 1/3: Adding validation to src/auth.ts → verify: function accepts empty input
✓ Done. Step 2/3: Writing failing test → verify: test fails before fix
✓ Done. Step 3/3: Making test pass → verify: test suite green
```

Bad: Silently editing 12 files, then a final summary.
Good: One line before and after each step so the user can interrupt if the plan is wrong.

Keep narration minimal — one line per step is enough. No essays.

---

**These guidelines are working if:** fewer unnecessary changes in diffs, fewer rewrites due to overcomplication, clarifying questions come before implementation rather than after mistakes, and no proliferation of meta-documents.
