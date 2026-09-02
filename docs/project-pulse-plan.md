# Project Pulse Dashboard Implementation Plan

## Summary

Build a lightweight static Project Pulse dashboard for contributors. The dashboard will present active projects, owners, statuses, recent activity, and priorities or risks in a polished, accessible, responsive card layout.

The implementation uses plain HTML, CSS, and JSON with no new framework or package dependency. The VS Code launch configuration serves the `app/` directory with `python3 -m http.server 5500` and opens `index.html`.

## Work plan

### 1. Confirm requirements and ownership

**Owner:** Orchestrator, with Planner input

Read the Project Pulse brief, agent definitions, and existing workspace configuration. Confirm required data fields, visual hooks, launch behavior, accessibility expectations, and validation gates before implementation begins.

### 2. Define the dashboard experience

**Owner:** Designer  
**Assigned file:** `app/styles.css`

The Designer defines the information hierarchy, responsive grid, card layout, spacing, typography, contrast, borders, shadows, rounded corners, status badges, priority or risk treatment, and keyboard-visible focus states. Status and priority must remain understandable without relying on color alone. The Designer also provides markup guidance for semantic landmarks, heading order, labels, and readable contrast.

### 3. Prepare representative project data

**Owner:** Coder  
**Assigned file:** `app/project-data.json`

Create valid JSON with a top-level `projects` array. Each project includes `name`, `owner`, `status`, `recentActivity`, and `priority`. Include varied statuses and priorities so active work, risks, and recent activity are represented.

### 4. Implement the dashboard page

**Owner:** Coder  
**Assigned file:** `app/index.html`

Create the accessible dashboard structure with the visible heading `Project Pulse`, a stylesheet reference, and data loading from `project-data.json`. Render every project into a visible card using the `project-card` class, displaying its name, owner, status, recent activity, and priority. Include useful visible states for an empty project list and data-loading failure.

### 5. Implement visual styling

**Owner:** Designer, or Coder after design approval  
**Assigned file:** `app/styles.css`

Implement the approved visual system and responsive behavior. Include the `.dashboard` and `.project-card` hooks, `border-radius`, and `box-shadow`. Ensure long values wrap without horizontal overflow and that status and priority remain legible across viewport sizes and in grayscale.

### 6. Configure local preview

**Owner:** Coder  
**Assigned file:** `.vscode/launch.json`

Create strict JSON with a configuration named exactly `Run Project Pulse Dashboard`. Run `python3 -m http.server 5500`, use `${workspaceFolder}/app` as `cwd`, and use `serverReadyAction` to open `http://localhost:%s/index.html`. Do not modify `.vscode/tasks.json`.

### 7. Integrate and review

**Owner:** Orchestrator  
**Files:** `app/index.html`, `app/styles.css`, `app/project-data.json`, `.vscode/launch.json`

Review that selectors, data names, paths, and launch behavior agree across files. Resolve integration issues sequentially before validation and confirm unrelated files remain unchanged.

## File assignments

| File | Owner | Responsibility |
| --- | --- | --- |
| `docs/project-pulse-plan.md` | Planner / Orchestrator | Record scope, ownership, dependencies, parallelization, risks, and validation gates. |
| `app/index.html` | Coder | Implement accessible dashboard structure and render project data into visible cards. |
| `app/styles.css` | Designer | Define responsive visual presentation, card layout, badges, spacing, and accessibility states. |
| `app/project-data.json` | Coder | Provide deterministic Project Pulse data using the required `projects` schema. |
| `.vscode/launch.json` | Coder | Provide the local preview configuration rooted at `app/`. |

## Responsibilities

### Designer

Own information hierarchy, visual direction, responsive layout, accessibility, status and priority affordances, and styling implementation in `app/styles.css`. Provide markup guidance without changing the data schema or launch configuration.

### Coder

Own HTML/data integration, the JSON data model, and VS Code launch configuration. Implement `app/index.html`, `app/project-data.json`, and `.vscode/launch.json`, incorporating the Designer's decisions without changing unrelated files.

### Orchestrator

Assign non-overlapping scopes, sequence dependent work, review the integrated result, and coordinate fixes. Only one agent should modify a given file at a time.

## Dependencies

- Requirements and agent definitions must be read before implementation.
- The data schema must be agreed before HTML rendering is finalized.
- Designer decisions must be available before final CSS and markup polish.
- `app/index.html` depends on the JSON field names and CSS selectors.
- `.vscode/launch.json` depends on the final location of `app/index.html`.
- Browser validation depends on all app files existing and the launch configuration using the correct `cwd` and URL.
- No new package, framework, build tool, or external service is required.

## Parallel work decisions

These activities can run in parallel because their file scopes do not overlap:

- The Designer can define the visual and accessibility direction in `app/styles.css`.
- The Coder can prepare representative `app/project-data.json`.
- The Planner or Orchestrator can document requirements and validation criteria.
- The Coder can draft `.vscode/launch.json` while HTML and CSS are being developed.

HTML/data integration should wait for the data schema, and final markup polish should incorporate the core design decisions. Cross-file integration review and final validation remain sequential. Agents must not overwrite one another's files.

## Edge cases and risks

- Invalid JSON must produce a visible, actionable error instead of a blank dashboard.
- An empty `projects` array should show a useful empty state.
- Missing or blank field values must not break card layout or semantic context.
- Long project names, owners, and activity text must wrap without horizontal overflow.
- Status and priority must not rely only on color.
- JSON loading should be validated through HTTP rather than opening the HTML directly from the filesystem.
- The launch URL must include `/index.html` so users do not see a directory listing.
- `launch.json` must remain strict JSON with no comments.

## Validation expectations

Before handoff, the Orchestrator verifies:

1. `app/index.html`, `app/styles.css`, `app/project-data.json`, and `.vscode/launch.json` exist.
2. `app/index.html` contains the visible title `Project Pulse`, references `styles.css` and `project-data.json`, and renders cards with `project-card`.
3. Every rendered card displays name, owner, status, recent activity, and priority.
4. `app/styles.css` contains `.dashboard`, `.project-card`, `border-radius`, and `box-shadow`, plus responsive and accessible styling.
5. `app/project-data.json` parses successfully, has a top-level `projects` array, and every project has all required fields.
6. `.vscode/launch.json` parses as strict JSON, contains `Run Project Pulse Dashboard`, uses `python3 -m http.server 5500`, sets `cwd` to `${workspaceFolder}/app`, and opens `http://localhost:%s/index.html`.
7. The launch configuration opens the dashboard UI from `index.html`, not a directory listing.
8. Desktop and narrow viewport checks show no overflow, readable spacing, visible status and priority treatment, and keyboard focus.
9. Only the assigned implementation files and this plan are changed.

## Open questions

No product questions block implementation. Filtering, sorting, persistence, authentication, or a backend data source should be planned separately if later required.
