# Agent team

I will use the following custom agent team to build Mona's Project Pulse dashboard:

| Agent | Target model | Responsibility | Definition |
| --- | --- | --- | --- |
| **Orchestrator** | Claude Opus 4.7 (copilot) | Coordinates the Planner, Coder, and Designer; breaks the work into phases, assigns explicit file scopes, manages dependencies, and verifies the integrated result. | [`.github/agents/orchestrator.agent.md`](../.github/agents/orchestrator.agent.md) |
| **Planner** | Claude Opus 4.7 (copilot) | Researches the repository and relevant documentation, identifies requirements, edge cases, dependencies, and risks, and produces an implementation plan without writing code. | [`.github/agents/planner.agent.md`](../.github/agents/planner.agent.md) |
| **Coder** | GPT-5.5 (copilot) | Implements the dashboard code and supporting runnable-app configuration within the assigned scope, using clear, testable logic and validating the result. | [`.github/agents/coder.agent.md`](../.github/agents/coder.agent.md) |
| **Designer** | Gemini 3.1 Pro (copilot) | Defines and implements the Project Pulse user experience, including information hierarchy, accessibility, responsive behavior, visual styling, project cards, status badges, and priority treatment. | [`.github/agents/designer.agent.md`](../.github/agents/designer.agent.md) |

I am using **GitHub Copilot CLI in a Codespace** to orchestrate this team and build Mona's Project Pulse dashboard.

## How the team will work together

1. The **Orchestrator** receives the Project Pulse brief, asks the **Planner** for an implementation plan, and assigns explicit file ownership so agents do not overwrite one another's work.
2. The **Planner** researches the repository and records the phases, dependencies, edge cases, validation expectations, and assignments in `docs/project-pulse-plan.md`.
3. After the plan is agreed, the **Designer** defines the information hierarchy, responsive layout, accessibility details, and visual direction for the dashboard. Design work can run in parallel with implementation planning when the file scopes do not overlap.
4. The **Coder** uses the plan and design direction to create `app/index.html`, `app/styles.css`, `app/project-data.json`, and `.vscode/launch.json`. The launch configuration serves the `app/` directory and opens `index.html`.
5. The **Orchestrator** reviews the integrated result, checks that the dashboard shows active projects, owners, statuses, recent activity, priorities, and contributor-friendly summaries, then coordinates fixes and reports the final handoff.

The team is coordinated through GitHub Copilot CLI: planning happens before dependent implementation, independent design and planning tasks may run in parallel, and integration and validation happen sequentially after the Coder's files are available. Each agent reports its decisions and remaining risks back to the Orchestrator, who keeps the work focused on the Project Pulse brief.
