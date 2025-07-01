# 🧭 Global Instructions for OpenAI Codex Agents

Codex agents working in this repository **must inherit all rules and hand-off
protocols defined in `AGENTS.md`.**  
These instructions add a few universal guard-rails and defaults.

---

## 1 · Tone & Output
* **Direct, concise, actionable.** Prefer numbered or bulleted lists.
* No apologies, no disclaimers, no “As an AI…”.
* Default language: U.S. English (mirror the user’s language if different).
* Use fenced code blocks for code; plain Markdown for everything else.
* When asked for JSON or raw text, output **only that**—no Markdown wrapper.

## 2 · Coding Behaviour
* Follow any language-specific rules given in the relevant task brief *or*
  agent checklist inside **`AGENTS.md`**.
* Include complete, executable snippets—imports, constants, error handling.
* Show **only the final state** of the file(s) unless explicitly asked for diffs.

## 3 · Commit & PR Etiquette  (see `AGENTS.md` for the full workflow)
* Commit header format:  
  `agent(scope): one-line summary`&nbsp;&nbsp;→ `coder(auth): add JWT refresh`
* Keep PR descriptions ≤ 200 words:
  1. **Why** (motivation)  
  2. **What** (key changes)  
  3. **How** (tests / verification)
* Tag reviewers according to `.github/CODEOWNERS`.

## 4 · Review Severity Threshold
Use the `comment_severity_threshold` value from `.ai/openai/config.yaml`
(default `MEDIUM`). Only post review comments at or above that level.

## 5 · Secrets & PII
Never output, log, or commit any credential, token, password, or customer data.
Mask examples: `sk-********`.

## 6 · Uncertainty Policy
If confidence < 80 % **or** requirements are ambiguous, ask one clear
follow-up question instead of guessing.

## 7 · Testing Requirements
* New or modified code **must** come with unit tests in `tests/`.
* Use `pytest` + `pytest-cov`; target ≥ 90 % coverage for touched files.

## 8 · Fun Flag
Check `.ai/openai/config.yaml → have_fun`.  
If `false`, omit poems, jokes, emojis, or other playful content.

---

These rules apply to every Codex invocation in this repository unless the
**user prompt** explicitly overrides them.
