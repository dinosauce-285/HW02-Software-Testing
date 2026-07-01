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
  ai-audit-report.tex      # AI usage log / audit report (Skill 1)
  bug-report.tex           # Bug report (Skill 2)
  git-commit-log.txt       # Per-step commit log (Skill 3, plain text)
  test-cases-<FR-XX>.tex   # Test-case reports (domain-bva-test skill)
screenshots/               # Bug screenshots, named <BUG-ID>.png
```

Feature IDs: `FR-01` … `FR-19`. Techniques: `Domain` or `BVA`.

## Skills

- **`domain-bva-test`** (invocable, `.claude/skills/`): the core Section-6 skill —
  Domain Testing + Boundary Value Analysis + AI gap analysis for one feature.
- Skills 1–3 below are recurring procedures Claude follows in this repo.

## Working principles (from the assignment)

- **Disciplined AI-First:** guide the AI through every step of a technique; never a
  single generic "generate test cases and find bugs" prompt.
- **Human review:** every AI output must be reviewed and corrected by the student;
  record what changed (see Skill 1).
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

---

## Skill 1 — AI Audit Logger

**When:** after every meaningful AI interaction in this project
(triggers: "log audit", "log AI session", "update audit report").

**Goal:** append an entry to `reports/ai-audit-report.tex` (LaTeX) following the
mandatory template from Section 9 of the assignment.

**Steps:**
1. If `reports/ai-audit-report.tex` does not exist, create it with this skeleton
   (new entries are inserted at the `% APPEND-BELOW` marker):
   ```latex
   \documentclass{article}
   \usepackage[margin=1in]{geometry}
   \usepackage{enumitem}
   \title{AI Audit Report --- HW02 Domain Testing}
   \begin{document}
   \maketitle
   Declaration: I use AI tools for the following tasks.
   % APPEND-BELOW
   \end{document}
   ```
2. Gather for the entry:
   - **AI tool**: name + model (e.g. `Claude Code (Opus 4.8)`).
   - **Date & time**: real value (ask the user or use the system date — never invent).
   - **Task**: one line (e.g. "Design Domain Testing cases for FR-05").
   - **Prompt**: the user's original prompt (verbatim; trim if very long but keep intent).
   - **AI output**: summary of what the AI returned.
   - **Human review**: what was corrected/kept, and who is responsible.
3. Insert this block immediately above `% APPEND-BELOW`:
   ```latex
   \section*{[<n>] <Task>}
   \begin{itemize}[leftmargin=*]
     \item \textbf{AI tool:} <tool + model>
     \item \textbf{Date \& time:} <YYYY-MM-DD HH:MM>
     \item \textbf{Prompt:} \begin{quote}<user prompt>\end{quote}
     \item \textbf{AI output (summary):} <summary>
     \item \textbf{Human review:} <edits made / kept as-is; responsibility>
   \end{itemize}
   ```
4. Increment `<n>` from the last existing entry. Escape LaTeX specials in
   user text (`& % $ # _ { }`).

**Note:** always fill the Human review field to satisfy the "Human review" principle.

---

## Skill 2 — Bug Reporter (GitHub Issues + Markdown)

**When:** a bug is found (triggers: "report bug", "file bug", "create issue").

**Goal:** record the bug in BOTH `reports/bug-report.tex` and GitHub Issues (with a
screenshot), using the same template and the same Bug ID.

**Collect (ask the user if missing):**
- Related feature (`FR-XX`) and detecting technique (`Domain`/`BVA`).
- Short title.
- Severity: `Critical` / `Major` / `Minor` / `Trivial`.
- Steps to reproduce (numbered).
- Expected result / Actual result.
- Screenshot path under `screenshots/`.

**Steps:**
1. Generate the next Bug ID: `BUG-<3 digits>` (max ID in `reports/bug-report.tex`, +1).
2. `reports/bug-report.tex` uses the standalone skeleton below with a
   `% APPEND-BELOW` marker; insert this block immediately above the marker
   (assumes `\usepackage{graphicx}` and `\usepackage{enumitem}` in the preamble):
   ```latex
   \section*{<BUG-ID> --- <Title>}
   \begin{itemize}[leftmargin=*]
     \item \textbf{Feature:} <FR-XX> \quad \textbf{Technique:} <Domain/BVA> \quad \textbf{Severity:} <...>
     \item \textbf{Steps to reproduce:} \begin{enumerate}\item ...\end{enumerate}
     \item \textbf{Expected:} ...
     \item \textbf{Actual:} ...
     \item \textbf{GitHub Issue:} \url{<fill URL after creating>}
   \end{itemize}
   \begin{figure}[h]\centering
     \includegraphics[width=0.8\textwidth]{../screenshots/<BUG-ID>.png}
     \caption{<BUG-ID>}
   \end{figure}
   ```
3. Create the GitHub issue with `gh` (repo must have the group's GitHub remote):
   ```bash
   gh issue create \
     --title "<BUG-ID>: <Title>" \
     --label "bug" \
     --body "$(cat <<'EOF'
   **Feature:** <FR-XX> | **Technique:** <Domain/BVA> | **Severity:** <...>

   **Steps to reproduce:**
   1. ...

   **Expected:** ...
   **Actual:** ...

   > Screenshot attached below (drag-and-drop the image into the issue on the web).
   EOF
   )"
   ```
4. `gh` cannot upload images — after creating the issue, ask the user to drag-and-drop
   `screenshots/<BUG-ID>.png` into the issue on the web, then paste the issue URL into
   the **GitHub Issue** field in `bug-report.tex`.
5. If there is no remote / `gh` is not authenticated: tell the user and still write the
   LaTeX part first.

**Note:** the assignment requires every bug to appear in BOTH the report and
GitHub Issues with a screenshot — do not skip either. Escape LaTeX specials in
user-provided text.

---

## Skill 3 — Commit-per-Step + Commit Log

**When:** after finishing each step of a feature's testing procedure
(triggers: "commit this step", "commit step", "export commit log").

**Goal:** one commit per step (Section 12), using the Conventional Commits convention
above, and be able to export `reports/git-commit-log.txt`.

**Steps:**
1. Stage only the files for this step (avoid a blanket `git add -A` if unrelated work
   is in progress):
   ```bash
   git add <files-for-this-step>
   ```
2. Commit with a Conventional Commit message (no co-author trailer):
   ```bash
   git commit -m "test(FR-05): partition domains for product search"
   ```
3. Create a branch first only if on the default branch and the user wants one; commit
   and push only when the user asks.
4. When preparing the submission, export the log:
   ```bash
   git log --pretty=format:"%h | %ad | %s" --date=format:"%Y-%m-%d %H:%M" > reports/git-commit-log.txt
   ```

**Note:** each step = one commit; the message must name the technique + step so the log
reads as the full testing procedure.
