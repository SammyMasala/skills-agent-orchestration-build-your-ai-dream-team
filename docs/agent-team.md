# Agent team

We are using GitHub Copilot CLI in a Codespace to orchestrate the work for Mona's Project Pulse dashboard. The custom agents live under `.github/agents` and are configured as repo-level agent definitions for the project.

## Team members

- Orchestrator
  - Model: Claude Opus 4.7 (copilot)
  - Responsibility: Breaks the work into phases, coordinates specialist agents, assigns file scopes, and verifies that the final result fits together.
  - Definition: `.github/agents/orchestrator.agent.md`

- Planner
  - Model: Claude Opus 4.7 (copilot)
  - Responsibility: Researches the repository, identifies dependencies and edge cases, and produces an ordered implementation plan the orchestrator can execute.
  - Definition: `.github/agents/planner.agent.md`

- Coder
  - Model: GPT-5.5 (copilot)
  - Responsibility: Implements the code changes, fixes bugs, and validates behavior within the assigned file scope.
  - Definition: `.github/agents/coder.agent.md`

- Designer
  - Model: Gemini 3.1 Pro (copilot)
  - Responsibility: Focuses on user experience, accessibility, visual design, layout, and interaction flow for the dashboard UI.
  - Definition: `.github/agents/designer.agent.md`

## How we will use them

The Orchestrator will delegate work to the Planner, Coder, and Designer based on task scope. The Planner creates the implementation strategy, the Designer shapes the dashboard experience, and the Coder performs the code work and validation. GitHub Copilot CLI is the coordination layer running in the Codespace, allowing the team to work together while keeping each agent focused on a well-defined responsibility.
