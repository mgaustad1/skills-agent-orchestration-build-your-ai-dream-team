# Project Pulse dashboard implementation plan

## Summary

Build Mona's lightweight, static Project Pulse dashboard so contributors can quickly see project names, owners, status, recent activity, priority or risk, and a short contributor-friendly summary. The interface should feel like a polished, accessible dashboard with responsive project cards. Keep the implementation within the three requested `app/` files and the VS Code launch configuration; no framework or package dependency is needed.

The plan makes dependencies, parallel work, and validation expectations explicit.

## Ordered phases and file assignments

1. **Agree on the interface and data contract**
   - **Owner:** Orchestrator, with Designer and Coder.
   - Confirm the shared HTML/CSS hooks before implementation: `.dashboard` for the dashboard layout and `.project-card` for each project, plus clear status and priority hooks if needed.
   - Agree that `app/project-data.json` has a top-level `projects` array. Each object includes `name`, `owner`, `status`, `recentActivity`, and `priority`; include a concise `summary` to meet the contributor-friendly brief.

2. **Create the dashboard presentation and content**
   - **Designer — `app/styles.css`:** Set visual hierarchy, typography, spacing, color and status/priority treatments, responsive card layout, and accessible contrast/focus styling. Style the agreed `.dashboard` and `.project-card` hooks. Report design decisions and any markup requirements to Coder.
   - **Coder — `app/index.html`:** Create the semantic page with the exact title **Project Pulse**, link `styles.css`, and load `project-data.json` to render visible project cards. Display each required project field, including status, recent activity, priority, and summary. Include an understandable loading or data-load error state and accessible labels/structure.
   - **Coder — `app/project-data.json`:** Add representative, valid project data using the agreed schema and multiple entries so the card layout can be reviewed.

3. **Make the dashboard runnable**
   - **Coder — `.vscode/launch.json`:** Add strict JSON for the **Run Project Pulse Dashboard** configuration. Run `python3 -m http.server 5500` with `cwd` set to `${workspaceFolder}/app`, and configure `serverReadyAction` to open `http://localhost:%s/index.html`. This serves the app directory and opens the page rather than a directory listing.

4. **Integrate and validate**
   - **Orchestrator:** Review all four assigned files together, resolve any mismatch between the markup, CSS hooks, and data schema, and report results.

## Responsibilities

- **Designer:** Own the dashboard's information hierarchy, accessibility, responsive behavior, and polished visual direction; implement the assigned stylesheet and communicate required hooks to Coder. Do not edit the HTML, data, or launch configuration.
- **Coder:** Own the semantic HTML/data-loading experience, project JSON, and launch configuration; implement against Designer's agreed visual and selector contract. Do not edit the stylesheet unless the Orchestrator explicitly reassigns it.
- **Orchestrator:** Coordinate the contract and file ownership, integrate both contributions, and verify the completed runnable dashboard.

## Dependencies and parallel work

- The initial data schema and CSS/HTML selector contract are prerequisites for integration. Agree them in Phase 1 to avoid conflicting assumptions.
- After that contract is settled, Designer can work on `app/styles.css` in parallel with Coder working on `app/project-data.json` and `.vscode/launch.json`. Coder can also draft `app/index.html` in parallel if the agreed hooks and schema are followed; the final data-rendering integration depends on both the HTML and JSON being available.
- Keep file ownership separate: Designer edits only `app/styles.css`; Coder edits only `app/index.html`, `app/project-data.json`, and `.vscode/launch.json`. Do not run final integration checks until those contributions are ready.
- Validation and launch-preview verification are sequential after integration; the launch check depends on all app files existing.

## Edge cases and risks

- Fetching JSON may fail if the page is opened directly using a `file://` URL. Use the configured local HTTP server and show a useful error state if loading still fails.
- Treat missing or empty project data gracefully; avoid rendering undefined values or an empty, confusing dashboard.
- Keep status and priority understandable through text and not color alone, and ensure the cards remain readable at narrow viewport widths.
- Ensure launch configuration paths and `cwd` are relative to the workspace as specified, and the opened URL ends in `/index.html`.

## Validation expectations

- Confirm `app/index.html` uses the exact title **Project Pulse**, links `styles.css`, loads `project-data.json`, and renders cards with the `project-card` class and all required project fields.
- Confirm `app/styles.css` defines `.dashboard` and `.project-card` and provides polished, responsive styling with clear status/priority treatments, spacing, and accessible contrast.
- Parse `app/project-data.json` as JSON; verify the top-level `projects` array and that each entry has `name`, `owner`, `status`, `recentActivity`, and `priority` (and the planned summary).
- Parse `.vscode/launch.json` as strict JSON. Verify the launch name is **Run Project Pulse Dashboard**, the command is `python3 -m http.server 5500`, `cwd` is `${workspaceFolder}/app`, and `serverReadyAction` opens `http://localhost:%s/index.html`.
- Start the configuration from VS Code and confirm the browser shows the Project Pulse UI at `index.html`, not a directory listing; stop the preview server after checking.
- Review the integrated dashboard at a narrow and a wide viewport and check keyboard access, visible focus, readable text, and understandable status/priority information.

## Open questions

- No blocking questions. The implementation assumes representative sample project entries are acceptable and that the agreed static HTML page can load JSON with browser `fetch` while served by the configured local HTTP server.
