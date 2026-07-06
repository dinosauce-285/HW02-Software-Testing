---
name: bug-reporter
description: Record a discovered bug in BOTH the bug report (reports/bug-report.tex) and GitHub Issues (with a screenshot), using the same Bug ID and template. Use when the user says "report bug", "file bug", or "create issue".
---

# Bug Reporter (GitHub Issues + Markdown)

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
