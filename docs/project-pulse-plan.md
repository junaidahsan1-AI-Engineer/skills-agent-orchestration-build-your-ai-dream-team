# Project Pulse Dashboard Implementation Plan

## Goal

Build Mona's static Project Pulse dashboard as a responsive, accessible view of active projects, ownership, status, recent activity, priority, and contributor-friendly summaries.

## Agent responsibilities

### Orchestrator

- Coordinate the Planner, Designer, and Coder.
- Enforce file ownership and resolve cross-file contracts.
- Sequence dependent work, integrate the result, and verify the complete preview.

### Planner

- Confirm the repository conventions, brief requirements, data contract, launch behavior, risks, and validation criteria.
- Keep the implementation focused on the four assigned application/configuration files.
- Do not implement the dashboard.

### Designer

- Define the information hierarchy, responsive layout, typography, spacing, card treatment, status badges, priority/risk treatment, and accessibility expectations.
- Specify the markup hooks required by the visual system, including `.dashboard` and `.project-card`.
- Ensure status and priority remain understandable without color alone, with sufficient contrast and visible keyboard focus states.
- Own the design direction for `app/styles.css` and coordinate its selector contract with the Coder.

### Coder

- Implement the static dashboard and connect its HTML, stylesheet, data, and launch configuration.
- Render multiple project cards showing name, owner, status, recent activity, priority, and summary.
- Keep the data shape and rendering deterministic, handle loading/error/empty states clearly if data is loaded at runtime, and avoid unrelated scope.
- Validate syntax, file references, local preview behavior, and cross-file consistency.

## File assignments

| File | Owner | Assignment |
| --- | --- | --- |
| `app/index.html` | Coder, informed by Designer | Create semantic page structure, the `Project Pulse` title and heading, project-card markup, accessible labels, stylesheet reference, and data-loading/rendering behavior. |
| `app/styles.css` | Designer direction; Coder implementation | Define the responsive dashboard and card layout, typography, spacing, status/priority treatments, focus states, `border-radius`, and `box-shadow`; include `.dashboard` and `.project-card`. |
| `app/project-data.json` | Coder | Provide a top-level `projects` array with multiple representative records. Each record must include `name`, `owner`, `status`, `recentActivity`, `priority`, and a contributor-friendly `summary`. |
| `.vscode/launch.json` | Coder | Add a strict JSON launch configuration named `Run Project Pulse Dashboard` using `python3 -m http.server 5500`, `${workspaceFolder}/app` as `cwd`, and a browser URL ending in `/index.html`. |

## Dependencies

1. Requirements and repository conventions must be confirmed before implementation.
2. The Designer's selector and content contract is needed before finalizing `app/index.html` and `app/styles.css`.
3. The HTML rendering contract must match the JSON schema before integration is complete.
4. The launch configuration depends on the `app/` directory and `index.html` entry point existing.
5. End-to-end validation depends on all four files being present and internally consistent.

## Parallel work decisions

After requirements confirmation, these independent tasks may proceed in parallel:

- The Designer defines the visual, responsive, accessibility, and selector contract for `app/styles.css`.
- The Coder creates `app/project-data.json`.
- The Coder drafts `.vscode/launch.json`.

The Designer and Coder may coordinate their interfaces in parallel, but must not make conflicting edits to the same file. Final `app/index.html` and stylesheet integration should follow the agreed selector/content contract. The Orchestrator's integration review and runtime validation remain sequential after implementation.

## Implementation sequence

1. Confirm the brief, repository conventions, required data fields, and static-app constraints.
2. Designer defines the visual and accessibility contract.
3. Coder creates representative project data and launch configuration.
4. Coder implements semantic HTML and data rendering.
5. Coder applies the Designer's responsive styling contract.
6. Orchestrator reviews all four files for matching selectors, fields, references, and scope.
7. Run static and runtime validation, then resolve any cross-file failures.

## Risks and edge cases

- The preview must target `/index.html`; opening the server root can show a directory listing.
- JSON loading should be validated over HTTP rather than relying on a `file://` URL.
- Invalid JSON, trailing commas, or mismatched field names can prevent rendering.
- Missing or empty data should result in a readable state rather than a blank or broken page.
- Long names and activity text must not break cards at narrow widths.
- Status and priority must be legible without relying on color alone.
- Port `5500` conflicts should be reported explicitly during validation.

## Validation expectations

### Static checks

- Confirm `app/index.html`, `app/styles.css`, `app/project-data.json`, and `.vscode/launch.json` exist.
- Parse both JSON files with a JSON parser; no comments or trailing commas are allowed.
- Confirm `index.html` references `styles.css` and `project-data.json`, contains `Project Pulse`, and renders `.project-card` elements.
- Confirm the stylesheet contains `.dashboard`, `.project-card`, `border-radius`, `box-shadow`, responsive rules, and visible focus treatment.
- Confirm every project has `name`, `owner`, `status`, `recentActivity`, `priority`, and `summary`.
- Confirm the launch name, command, working directory, port, and explicit `/index.html` target exactly match the assignment.

### Runtime checks

- Start the dashboard through `Run Project Pulse Dashboard`.
- Verify the server responds and the browser opens the dashboard rather than a directory listing.
- Verify multiple project cards render with all required fields visible.
- Test desktop and narrow viewport layouts, keyboard focus, and readable status/priority presentation.
- Stop the preview server after validation.

The implementation is ready for handoff only when the static checks pass, the configured preview renders successfully, and the Orchestrator confirms there are no unresolved cross-file inconsistencies or out-of-scope changes.
