# CLAUDE.md — HW02 Domain Testing (EShop)

Assignment **HW02 – Domain Testing** on the SUT **EShop**
(https://github.com/ttbhanh/eshop-sut). Each student picks 4 features (one per
pool A/B/C/D), applies **Domain Testing** + **Boundary Value Analysis**, reports
bugs on GitHub Issues, and keeps an **AI Audit Report**.

All project documentation is written in **English**. Report deliverables are
authored in **LaTeX** (`.tex`) and compiled to PDF; only the git commit log stays
plain text.

## Output layout

```
reports/
  ai-audit-report.tex      # AI usage log / audit report (ai-audit-logger skill)
  bug-report.tex           # Bug report (bug-reporter skill)
  git-commit-log.txt       # Per-step commit log (plain text; see Git commit convention)
  test-cases-<FR-XX>.tex   # Test-case reports (domain-bva-test skill)
screenshots/               # Bug screenshots, named <BUG-ID>.png
```

Feature IDs: `FR-01` … `FR-19`. Techniques: `Domain` or `BVA`.

## Skills

CLAUDE.md holds only the general project guidance below. Each recurring procedure is
its own invocable skill under `.claude/skills/`. Skills never call each other — the
user invokes each one by prompt:

- **`domain-bva-test`** — the core Section-6 skill: Domain Testing + Boundary Value
  Analysis + AI gap analysis for one feature.
- **`ai-audit-logger`** — append an entry to the AI Audit Report after an AI session
  (triggers: "log audit", "log AI session").
- **`bug-reporter`** — file a bug in both the bug report and GitHub Issues, with a
  screenshot (triggers: "report bug", "file bug").

Committing one commit per testing step follows the **Git commit convention** below (a
plain git action, not a skill).

## Working principles (from the assignment)

- **Disciplined AI-First:** guide the AI through every step of a technique; never a
  single generic "generate test cases and find bugs" prompt.
- **Human review:** every AI output must be reviewed and corrected by the student;
  record what changed (see the `ai-audit-logger` skill).
- **Never fabricate** dates, metrics, or test results — ask the user when unsure.

---

## Git commit convention

- **Conventional Commits:** `<type>(<scope>): <description>`
  - `type`: `test` | `docs` | `feat` | `fix` | `chore` | `refactor`
  - `scope`: the feature ID (`FR-XX`) or one of `bug`, `audit`, `report`
- **One commit per testing step** (assignment Section 12).
- **Do NOT add a `Co-Authored-By` trailer** or any AI attribution footer.
- Description in imperative mood, lower case, no trailing period.
- Examples:
  - `test(FR-05): partition domains for product search`
  - `test(FR-05): add 3-value BVA cases for search page size`
  - `docs(bug): report BUG-001 duplicate coupon accepted`
  - `docs(audit): log domain testing session for FR-05`
