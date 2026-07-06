---
name: ai-audit-logger
description: Append an entry to the HW02 AI Audit Report (reports/ai-audit-report.tex) after a meaningful AI interaction, following the mandatory Section-9 template (tool, date/time, prompt, AI output, human review). Use when the user says "log audit", "log AI session", or "update audit report".
---

# AI Audit Logger

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
