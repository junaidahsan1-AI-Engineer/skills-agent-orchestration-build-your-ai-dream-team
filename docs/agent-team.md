# Agent team

I will use GitHub Copilot CLI in a Codespace to orchestrate a four-agent team for building Mona's Project Pulse dashboard. Each agent is defined under `.github/agents/`:

| Agent | Target model | Responsibility | Definition |
| --- | --- | --- | --- |
| **Orchestrator** | Claude Opus 4.7 (copilot) | Coordinates the project, delegates work to specialists, manages phases and dependencies, and verifies the integrated result. It does not implement the dashboard directly. | `.github/agents/orchestrator.agent.md` |
| **Planner** | Claude Opus 4.7 (copilot) | Researches the repository, documentation, dependencies, risks, edge cases, and validation needs, then produces an actionable implementation plan. It does not write code. | `.github/agents/planner.agent.md` |
| **Designer** | Gemini 3.1 Pro (copilot) | Defines the dashboard's UI/UX, information hierarchy, accessibility, responsive behavior, visual styling, project cards, status badges, and priority treatment. | `.github/agents/designer.agent.md` |
| **Coder** | GPT-5.5 (copilot) | Implements assigned application logic and support configuration, follows repository patterns, handles errors explicitly, and validates the resulting behavior. | `.github/agents/coder.agent.md` |

The Orchestrator will first obtain the Planner's implementation strategy, then coordinate the Designer and Coder in phases with explicit file ownership. Independent work can run in parallel, while dependent or overlapping work runs sequentially. Git staging, commits, and pushes remain under the learner's control.
