# Agent team

I will build Mona's Project Pulse dashboard with the following custom agent team, orchestrated through GitHub Copilot CLI in a Codespace.

## Orchestrator

- **Agent:** Orchestrator
- **Target model:** Claude Opus 5.5 (copilot)
- **Responsibility:** Coordinates the Planner, Coder, and Designer. The Orchestrator turns the implementation plan into scoped phases, delegates work, manages dependencies and parallelism, integrates results, and reports the final outcome.
- **Definition:** `.github/agents/orchestrator.agent.md`

## Planner

- **Agent:** Planner
- **Target model:** Claude Opus 5.5 (copilot)
- **Responsibility:** Researches the repository and relevant dependencies, identifies requirements and edge cases, and produces a practical implementation plan with file assignments and validation expectations.
- **Definition:** `.github/agents/planner.agent.md`

## Designer

- **Agent:** Designer
- **Target model:** Gemini 3.7 Flash (copilot)
- **Responsibility:** Designs the dashboard UI/UX, including accessibility, responsive behavior, project cards, status badges, priority treatment, visual hierarchy, and deterministic CSS hooks.
- **Definition:** `.github/agents/designer.agent.md`

## Coder

- **Agent:** Coder
- **Target model:** GPT-5.5 (copilot)
- **Responsibility:** Implements the planned application code and support configuration within assigned file scopes. The Coder keeps behavior clear and testable and validates the implementation, including the runnable app setup where required.
- **Definition:** `.github/agents/coder.agent.md`
