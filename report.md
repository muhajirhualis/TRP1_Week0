# TRP 1 – MCP Setup Challenge Report

## 1. What I Did

I successfully completed all three tasks of the challenge:

- **Task 1 (Setup)**: Configured **VS Code** with the **Tenx MCP server** following the official documentation. Verified that the MCP connection is active and logging interactions.
  
- **Task 2 (Research & Configure)**: Studied **Boris Cherny’s full workflow thread** (provided as `Boris Cherny's workflow.docx`) and integrated his core best practices into my agent rules file at `.github/copilot-instructions.md`. Key improvements included:
  - Enforcing a **“plan-first” workflow** before any code or analysis.
  - Introducing **subagent thinking** (e.g., “Data Engineering Subagent,” “Research Proposal Editor”) to improve task-specific reasoning.
  - Adding **self-verification instructions** so the agent checks its own output for correctness—mirroring Boris’s emphasis that “giving Claude a way to verify its work will 2–3x the quality” [[9]].
  - Structuring the rules file as a **living document**, consistent with how Boris’s team updates `CLAUDE.md` “any time we see Claude do something incorrectly” [[4]].
  - Placing all behavioral rules **after the mandatory trigger framework** to preserve logging compliance while enhancing AI guidance.

- **Task 3 (Documentation)**: This report documents the entire process, including successes, challenges, and key insights.

## 2. What Worked

- The **MCP server connection in VS Code worked reliably**, enabling automatic logging of all agent interactions.
- The **trigger workflow** (calling `log_passage_time_trigger` on every message) functioned as required and ensured auditability.
- After updating the rules file, the AI agent began:
  - **Proposing clear, numbered plans** before executing tasks (directly reflecting Boris’s “Plan mode” practice [[2]]).
  - **Asking for confirmation** at critical steps, reducing hallucination.
  - **Adopting specialized roles**, which improved relevance for academic and data science tasks.
- The **modular structure** of the rules file made it easy to test and iterate—just like Boris’s team treats `CLAUDE.md` as shared, version-controlled knowledge [[4]].

## 3. What Didn’t Work & Troubleshooting

- **Initial confusion about rule placement**: At first, I wasn’t sure where to insert Boris-inspired rules without breaking the trigger system.  
  → **Solution**: I kept the **entire original trigger block intact** and appended new behavioral guidelines **after** the `***** mandatory workflow *************` marker, using clear comment separators.

- **Agent sometimes skipped planning**: Early versions of the rules used soft language like “consider planning,” which the model ignored.  
  → **Solution**: I changed the instruction to **“ALWAYS PLAN FIRST”** in bold, imperative terms—matching Boris’s explicit style [[2]].

- **No native slash commands or hooks in VS Code Copilot**: Unlike Claude Code, VS Code doesn’t support `.claude/commands/` or `PostToolUse` hooks [[5]].  
  → **Solution**: I **simulated hooks via instructions**, e.g., “Always format output in APA style and markdown code blocks,” and **simulated subagents via role prompts**.

## 4. Insights Gained

Beyond Boris Cherny’s workflow, I cross-referenced community best practices from GitHub repositories using CLAUDE.md patterns, Cursor AI documentation, and MCP integration guides to ensure robust rule design. I tested multiple phrasings (e.g., ‘should plan’ vs. ‘ALWAYS PLAN FIRST’) and observed that stronger imperatives significantly improved agent compliance.

Other insights I have gained are:

- **Rules directly shape agent reliability**. Small wording changes (e.g., “should” → “MUST”) create dramatic improvements in compliance.
- **Planning is non-negotiable for quality**. As Boris states, “Most sessions start in Plan mode… go back and forth until I like its plan” [[2]]. Enforcing this reduced errors and improved alignment.
- **Verification is transformative**. Building in self-checks (“Does this follow APA?”, “Is the code runnable?”) mirrors Boris’s #1 tip: “Give Claude a way to verify its work” [[9]].
- **The rules file is a team contract**. Even as a solo developer, treating `copilot-instructions.md` as a living, evolving guide—like `CLAUDE.md`—creates compounding improvements over time [[4]].

This exercise confirmed that **modern AI-assisted development isn’t just about tools—it’s about intentional workflows, clear contracts (rules), and feedback loops**.