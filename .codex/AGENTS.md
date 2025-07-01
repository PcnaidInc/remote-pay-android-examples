# 🤖 AGENTS.md — OpenAI Codex Workflow

This repository is maintained by a **team of AI agents** that automate routine
development tasks—planning, coding, refactoring, testing, and reviewing.  
All agents run on OpenAI Codex via GitHub Actions and communicate through
issues, pull-request comments, and commit messages.

---

## 1. Agent Roster & Responsibilities

| Handle            | Role / Scope                           | Triggers (labels / paths / events)                | Output format                    |
|-------------------|----------------------------------------|---------------------------------------------------|----------------------------------|
| **@planner-bot**  | Break high-level feature requests into bite-sized tasks & create tracking issues | PR or issue labeled `needs-plan`                  | - Task list under `## Plan`<br>- Links to sub-issues |
| **@arch-bot**     | Propose file / folder layouts, interfaces, data models  | Commit touching `/**/architecture/**` or label `arch` | Markdown diagram + bullets |
| **@coder-bot**    | Write or modify code per `planner-bot` checklists | Draft PR with label `generated`                   | Code diff + inline comments |
| **@test-bot**     | Generate / update unit & integration tests | File change in `src/**` or `coder-bot` PR         | Adds/updates files in `tests/`   |
| **@lint-bot**     | Run Black, Ruff, Mypy, and auto-fix style | Any new commit on open PR                         | Pushes fix-up commit, sets status |
| **@review-bot**   | Code-review open PRs, flagging issues severity ≥ `MEDIUM` | PR ready for review (`review-bot` label)          | GitHub Review with severity tags |
| **@sec-bot**      | Scan for secrets / vulnerable deps      | Push to `main` OR Dependabot PR                   | Security alert + patch PR        |
| **@doc-bot**      | Update docstrings & `docs/` when public APIs change | Merge to `main` touching `src/**`                 | Commit to `docs/` + PR summary   |

> **Note:** Any human maintainer can override an agent by commenting  
> `@<agent> skip` on the issue/PR.

---

## 2. General Operating Rules

1. **Token Budget**  
   * Soft limit 8 k tokens/response; chunk large diffs.

2. **Coding Standards**  
   * Follow [`STYLEGUIDE.md`](./STYLEGUIDE.md) for Python.  
   * New files must include type hints & Google-style docstrings.

3. **Commit Conventions**  
   * `agent(scope): one-line summary`  
     e.g. `coder(auth): add JWT refresh endpoint`

4. **Branch Etiquette**  
   * Agents create branches under `agent/<handle>/<topic>`.  
   * Squash-merge only; no force-push to `main`.

5. **Cross-Agent Handoffs**  
   * Planner → Coder → Test → Lint → Review chain enforced by required-status checks.
   * Each agent posts a checkbox list; the next agent begins when all boxes
     are ticked.

6. **Safety Rails**  
   * Never commit API keys, secrets, or PII.  
   * `sec-bot` blocks merge if secrets detected.

---

## 3. Trigger Matrix (Quick Ref)

| Event                            | Labels / Path                             | Fired Agent(s)       |
|----------------------------------|-------------------------------------------|----------------------|
| New feature request issue        | `needs-plan`                              | `planner-bot`        |
| Commit in `/architecture/`       | n/a                                       | `arch-bot`           |
| Draft PR opened by `planner-bot` | `generated`                               | `coder-bot`          |
| Push affecting `src/**`          | n/a                                       | `test-bot`, `lint-bot`|
| PR status checks complete        | `Ready for Review` comment added          | `review-bot`         |
| Merge to `main`                  | Any                                       | `doc-bot`, `sec-bot` |

---

## 4. Local Invocation (VS Studio)

Developers can query agents locally via the **OpenAI Codex CLI**:

```bash
# Ask coder-bot to refactor a file
codex agent coder-bot \
      --file src/payment/processor.py \
      --prompt "Refactor into smaller functions, keep logic identical."

