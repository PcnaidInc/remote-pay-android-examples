# 🧭 Global Instructions for OpenAI Codex Agents

## 1. Tone & Style
* **Be direct, concise, and actionable** – bullet lists > walls of prose.
* **No apologies, no disclaimers, no “As an AI …”**.
* Default language is **U.S. English** unless the user’s prompt is in another tongue.

## 2. Coding Rules
* Follow [`STYLEGUIDE.md`](../../STYLEGUIDE.md) without exception.  
  * Python ≤ 3.12, use `from __future__ import annotations` where helpful.  
  * Max line length 100, 4-space indents, Google-style docstrings, full type hints.
* Always include **imports**, **constants**, and **error handling** in snippets—no “pseudo-code”.
* Show only the **final code** (no intermediate diffs) unless explicitly asked otherwise.

## 3. Commit & PR Etiquette
* Commit prefix: \
  `agent(scope): one-line summary`  e.g. `coder(auth): add JWT refresh`
* Keep PR descriptions ≤ 200 words; include:
  1. **Motivation** (why)
  2. **Key changes** (what)
  3. **Testing** (how verified)
* Tag reviewers using GitHub handles defined in `./CODEOWNERS`.

## 4. Comment / Review Severity
* Severity scale: `LOW` < `MEDIUM` < `HIGH` < `CRITICAL`
* Post review comments **≥ configured threshold** (see `config.yaml`).

## 5. Secrets & PII
* Never output, log, or commit API keys, access tokens, passwords, credit-card or user data.
* Mask example keys like `sk-xxxxxxxx`.

## 6. Asking for Clarification
* If **confidence < 80 %** or ambiguity blocks progress, ask a single blunt follow-up question.

## 7. Error Handling & Logging
* Catch **specific** exceptions, log at appropriate level (DEBUG/INFO/WARN/ERROR).
* Never swallow exceptions silently—log or re-raise.

## 8. Test Coverage
* New or changed code **must** include unit tests in `tests/` that cover the happy path + at least one failure path.
* Use `pytest` + `pytest-cov`; target ≥ 90 % coverage for touched files.

## 9. Output Formats
* **Default**: fenced code blocks for code, Markdown lists/tables for text.
* **When asked for JSON**: output *only* JSON—no markdown, no commentary.

## 10. Fun Flag
* The repo-level `have_fun` toggle (see `config.yaml`) controls any playful additions (e.g., limericks). Never add fun content when `have_fun: false`.

---

_These rules apply to every Codex invocation in this repository unless the **user prompt** explicitly overrides them._
