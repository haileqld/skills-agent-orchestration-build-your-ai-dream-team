# Mona's Project Pulse Dashboard Implementation Plan

## Summary

Build a small, polished, responsive static dashboard that helps Mona's contributors scan project health. The app will be implemented in `app/index.html`, `app/styles.css`, and `app/project-data.json`, with a VS Code launch setup in `.vscode/launch.json`.

The dashboard must present a top-level heading and a collection of project cards. Every card must clearly show project name, owner, status, recent activity, and priority. The initial view should read as an intentional dashboard rather than a bare document.

## Roles and file ownership

### Orchestrator

- Turns this plan into ordered phases and assigns work.
- Resolves dependencies and prevents overlapping edits.
- Integrates the HTML, CSS, data, and launch configuration.
- Runs final validation and reports a concise handoff.

### Designer

- Owns `app/styles.css`.
- Defines the visual hierarchy, type scale, spacing, card treatment, status badges, priority affordances, and responsive behavior.
- Uses the semantic hooks established for the Coder: at minimum `.dashboard` and `.project-card`.
- Delivers a polished layout with rounded corners and box shadows.
- Designs accessible contrast, visible focus states, and layouts that remain usable on narrow screens.
- Does not edit HTML, JSON, or launch configuration.

### Coder

- Owns `app/index.html`, `app/project-data.json`, and `.vscode/launch.json`.
- Builds deterministic semantic HTML for the dashboard and project cards.
- Loads `project-data.json` and renders the project fields without a framework or build step.
- Keeps JavaScript behavior simple and testable, including a clear loading or error state.
- Creates `.vscode/launch.json` as strict JSON with no comments.
- Configures the launch to serve `app/` as the working directory and open `index.html`.
- Does not change design decisions in `app/styles.css`; CSS ownership belongs to the Designer.

## Shared hook contract

The Coder and Designer must agree on these minimum hooks before independent work begins:

- `.dashboard` is the main dashboard container.
- `.project-card` is used by each project card.
- Status and priority values should have predictable CSS hooks or class prefixes derived from the data rather than free-form inline styles.
- The HTML must include visible text for project name, owner, status, recent activity, and priority.
- The JSON root must contain exactly a `projects` array in the implementation contract, though additional presentation metadata is not required.

## Ordered implementation steps

1. **Confirm requirements and establish the data shape**
   - Owner: Planner/Orchestrator.
   - Files: read-only review of the brief and existing repository structure.
   - Define a project object with `name`, `owner`, `status`, `recentActivity`, and `priority`.
   - Establish representative status and priority values for the initial dataset.
   - Dependency: none.

2. **Create the data contract and sample dataset**
   - Owner: Coder.
   - File: `app/project-data.json`.
   - Add the top-level `"projects"` array and several representative projects.
   - Ensure all required fields are populated and JSON syntax is valid.
   - Dependency: step 1.

3. **Build the dashboard HTML and rendering behavior**
   - Owner: Coder.
   - File: `app/index.html`.
   - Add a dashboard shell, `.dashboard` container, heading, and loading/error containers if needed.
   - Add JavaScript that fetches `./project-data.json`, validates the top-level array and required fields, and creates elements with the `.project-card` hook.
   - Keep script behavior robust if data is missing or malformed.
   - Dependency: step 1; expected DOM hooks from the shared contract. It can begin after the CSS assignment is understood.

4. **Create the visual design**
   - Owner: Designer.
   - File: `app/styles.css`.
   - Style the dashboard, cards, status badges, priority treatment, and readable project metadata.
   - Include rounded corners with `border-radius`, depth with `box-shadow`, clear spacing, and a responsive grid.
   - Add accessible contrast and keyboard-focus styles.
   - Dependency: step 1; can begin in parallel with steps 2 and 3 once hooks are fixed.

5. **Create the VS Code launch configuration**
   - Owner: Coder.
   - File: `.vscode/launch.json`.
   - Use strict JSON with no comments.
   - Name the configuration `Run Project Pulse Dashboard`.
   - Set the launch working directory to `${workspaceFolder}/app` and configure an HTTP server on a fixed local port, then open `http://localhost:<port>/index.html`.
   - Dependency: directory layout; independent of final HTML/CSS content.

6. **Integrate and polish**
   - Owner: Orchestrator.
   - Review HTML/CSS/data integration and ensure no file ownership overlap.
   - Make only final integration corrections to the assigned files.
   - Verify that project cards reflect the JSON and that the first screen is a polished dashboard.
   - Dependency: steps 2–5.

7. **Validate and hand off**
   - Owner: Orchestrator, with Coder/Designer verification.
   - Run file, syntax, visual, responsive, and launch checks described below.
   - Report any remaining risk or open question in the final handoff.
   - Dependency: step 6.

## Parallel work decisions

The following work may proceed in parallel after the shared hook contract and data shape are fixed:

- Coder creates `app/project-data.json`.
- Coder creates `app/index.html`.
- Designer creates `app/styles.css`.
- Coder creates `.vscode/launch.json`.

The main coordination requirement is that the Coder and Designer agree on `.dashboard`, `.project-card`, status/priority hooks, and the card markup before implementation. The Coder owns HTML structure and the Designer owns only CSS; an assigned agent must not edit outside these boundaries.

## Sequential dependencies

1. Requirements and data shape must be settled before rendering or styling.
2. HTML hook names must be settled before the Designer can finish deterministic CSS.
3. JSON field names must be settled before HTML rendering logic is finalized.
4. Launch configuration should be created only after the app files exist or as a parallel, layout-known task.
5. Integration validation must follow all implementation steps.

## Validation expectations

### Files and syntax

- `app/index.html`, `app/styles.css`, `app/project-data.json`, and `.vscode/launch.json` must exist.
- `app/project-data.json` must parse as valid JSON.
- The JSON root must contain a top-level `projects` array.
- Every project object must include `name`, `owner`, `status`, `recentActivity`, and `priority`.
- `.vscode/launch.json` must parse as strict JSON and must have no comments.
- The launch name must be `Run Project Pulse Dashboard`.
- The launch working directory must be `${workspaceFolder}/app`.
- The launch target must open `/index.html` from the local server rather than a directory listing.

### Dashboard behavior

- Opening the launch configuration shows the dashboard UI immediately.
- Each project is rendered as a visible `.project-card`.
- The page displays project name, owner, status, recent activity, and priority.
- `.dashboard` is present in CSS and HTML styling.
- Status and priority treatments are visibly distinguishable.
- Malformed or unavailable JSON produces an explicit, user-readable error state instead of blank output.

### Design and accessibility

- The first view clearly resembles a polished dashboard.
- Cards use visible spacing, rounded corners, and shadows.
- Text remains readable against its background.
- Layout responds cleanly at desktop, tablet, and narrow mobile widths.
- Project cards wrap or reflow rather than overflow.
- Interactive or focusable elements have visible keyboard focus.
- Status, priority, owner, and activity use clear text labels in addition to any color treatment.
- Decorative color is never the only indication of status or priority.

### Recommended smoke test

1. Start `Run Project Pulse Dashboard`.
2. Confirm the browser opens `http://localhost:<port>/index.html`.
3. Confirm project cards and all required fields are visible.
4. Reload once to verify loading and rendering are deterministic.
5. Resize the viewport from desktop to mobile and verify that the grid reflows without horizontal overflow.
6. Parse the JSON and launch JSON with available JSON validators.

## Edge cases

- **Empty projects array:** render a clear empty state and do not create blank cards.
- **Fetch failure:** show an explicit error message and retain the dashboard shell.
- **Missing required field:** skip or label malformed records rather than silently showing incomplete cards.
- **Unknown status or priority:** use neutral fallback styling and keep the original text visible.
- **Long names or activity text:** allow wrapping and preserve card alignment.
- **Special characters:** render text safely so project data cannot become executable markup.
- **Narrow viewport:** switch to a single or compact card grid and avoid fixed widths.
- **Slow or delayed fetch:** show a loading state until data has loaded.
- **Serving from the wrong directory:** launch must explicitly run from `app/` and target `index.html`.
- **Opening `file://` directly:** fetch behavior may be limited; the launch server is the supported run path.

## Open questions

- Should the representative data use one fixed set of status terms such as Active, At Risk, and Paired, or should status vocabulary remain project-controlled?
- What priority terms should the initial cards use: High, Medium, Low, or another small set?
- Is there a preferred local server port for the VS Code configuration, or may the Coder choose a deterministic unused port such as `4173`?
- Should the dashboard include a project count or summary header, or remain limited to heading plus project cards?
- Are filters or sorting interactions required, or should the static dashboard remain read-only?
- Does the `recentActivity` field need to support plain text only, or structured dates and descriptions?

## Completion criteria

The implementation is complete when all four files exist, the launch configuration serves the app from `app/` and opens `index.html`, the project data renders into visible polished cards, responsive and accessibility checks pass, and no file ownership rule has been violated.
