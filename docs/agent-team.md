# Agent team for Mona's Project Pulse dashboard

I am using GitHub Copilot CLI in a Codespace to orchestrate the work for Mona's Project Pulse dashboard. The custom team lives under `.github/agents/` and is designed to split planning, design, implementation, and coordination into focused specialist roles.

- Orchestrator — Model: Claude Opus 5.5 (copilot). Responsible for coordinating the overall build, breaking work into phases, delegating to specialists, and verifying the integrated result. Definition: `.github/agents/orchestrator.agent.md`.
- Planner — Model: Claude Opus 5.5 (copilot). Responsible for researching the codebase, identifying dependencies and edge cases, and producing a practical implementation plan and file ownership map. Definition: `.github/agents/planner.agent.md`.
- Designer — Model: Gemini 3.7 Flash (copilot). Responsible for dashboard UX, visual hierarchy, accessibility, responsiveness, and styling direction for the Project Pulse interface. Definition: `.github/agents/designer.agent.md`.
- Coder — Model: GPT-5.5 (copilot). Responsible for implementing the application logic, fixing issues, and creating the code needed for the dashboard in the file scope assigned by the Orchestrator. Definition: `.github/agents/coder.agent.md`.

This team gives the Project Pulse build a clear workflow: the Planner researches and plans, the Designer shapes the dashboard experience, the Coder implements the code, and the Orchestrator keeps the work aligned and validated across the project.