# Mona's Project Pulse dashboard implementation plan

## Goal and scope

This plan implements the Project Pulse dashboard described in `.github/project-pulse-brief.md` and aligns with the custom agent responsibilities in `.github/agents/` and `docs/agent-team.md`.

The dashboard is a small static app with:

- `app/index.html`
- `app/styles.css`
- `app/project-data.json`
- `.vscode/launch.json`

The implementation must keep the app lightweight, deterministic, and easy to preview in VS Code. The final experience is a contributor-friendly dashboard showing active projects, owners, statuses, recent activity, priority/risk, and a polished visual layout.

## Assumptions

- This is a static frontend with no framework or build step.
- Data will live in `app/project-data.json` as a top-level `projects` array.
- The UI will be rendered from `app/index.html` and styled via `app/styles.css`.
- `.vscode/launch.json` will launch a local static file server from the `app/` directory and open `index.html`, not the folder listing.
- The project is intentionally small enough for one dashboard page and a handful of data records.
- The app must remain readable on desktop and narrow screens without requiring additional libraries.

## Agent responsibilities

### Orchestrator
- Coordinates the work across Designer and Coder.
- Confirms scope, sequencing, and readiness to move between phases.
- Verifies that the dashboard matches the brief and that no file ownership conflicts occur.

### Planner
- Maintains the plan and clarifies dependencies and edge cases.
- Ensures work proceeds in an ordered sequence that minimizes rework.
- Keeps the plan aligned with repository instructions and the brief.

### Designer
- Owns the UX and information hierarchy for the dashboard.
- Defines visual treatment: spacing, typography, color signals, status badges, cards, and responsive behavior.
- Reviews the semantics of the HTML structure to ensure accessibility and usability.
- Gives implementation guidance for `.dashboard`, `.project-card`, and related CSS hooks.

### Coder
- Owns the static file implementation and validation.
- Builds the HTML structure, data source, and launch config.
- Ensures JSON is valid and that the app loads correctly from the expected folder path.
- Performs final smoke tests for layout and runtime behavior.

## File ownership

- `app/index.html`
  - Primary owner: Coder
  - Supporting review: Designer
  - Responsibility: structure, semantic sections, project card template, dataset load hook, and accessibility

- `app/styles.css`
  - Primary owner: Designer
  - Supporting implementation: Coder
  - Responsibility: dashboard shell, card layout, badges, spacing, typography, contrast, responsive states

- `app/project-data.json`
  - Primary owner: Coder
  - Supporting review: Designer
  - Responsibility: valid `projects` array containing `name`, `owner`, `status`, `recentActivity`, and `priority`

- `.vscode/launch.json`
  - Primary owner: Coder
  - Supporting review: Orchestrator
  - Responsibility: deterministic VS Code launch config serving `app/` and opening `index.html`

## Dependencies and sequencing

### Required dependencies
- The HTML markup must be based on the data model in `app/project-data.json`.
- The CSS classes must match the HTML structure for reliable layout and theming.
- The launch config depends on the app folder structure being correct and the app root being `app/`.
- The Designer and Coder must agree on the dashboard semantics before final CSS polish.

### Sequential work
1. Review the repository brief and confirm the static app requirements.
2. Define the dashboard content model and data contract.
3. Create the HTML structure around the agreed card and status patterns.
4. Add the styling to match the design specification.
5. Validate the launch configuration and preview path.
6. Run final smoke tests for accessibility and rendering.

### Parallelizable work
- The Designer can draft layout and color decisions while the Coder assembles the data structure in parallel.
- The Coder can prepare `project-data.json` and the required HTML skeleton concurrently once the data contract is confirmed.
- The final CSS tuning can proceed after the HTML structure is stable, even while the Coder continues validating the launch configuration.

The key rule is that content-model decisions must be final before the last round of CSS tuning so the view is not redesigned around incomplete data.

## Ordered implementation phases

### Phase 1: Align on requirements and dashboard pattern
- Confirm the project brief, static app constraints, and file set.
- Define the dashboard narrative: active projects, ownership, status, activity, and priority.
- Ensure the app remains contributor-friendly and not overloaded with technical detail.
- Output: clear design direction and a shared understanding of the final user experience.

### Phase 2: Define the data contract
- Create `app/project-data.json` with a top-level `projects` array.
- Ensure each project includes:
  - `name`
  - `owner`
  - `status`
  - `recentActivity`
  - `priority`
- Decide on a small, realistic set of statuses and priority labels to keep the UI consistent.
- Keep values deterministic and human-readable so the frontend can render them without additional logic.
- Output: valid JSON ready for the UI to consume.

### Phase 3: Build the dashboard structure in HTML
- Build `app/index.html` with a clear top-level dashboard layout.
- Include a page header, summary area, and project card list.
- Use accessible semantics: headings, landmarks, list structure, readable labels, and descriptive text.
- Ensure the HTML includes stable hooks for CSS classes like `.dashboard`, `.project-card`, `.status-badge`, and `.priority-pill`.
- Keep the page robust even if some project values are empty or longer than expected.
- Output: a semantic static structure ready for styling.

### Phase 4: Style the dashboard for readability and polish
- Create `app/styles.css` with a polished, consistent theme.
- Apply clear spacing, cards, shadows, badges, and contrast to distinguish project states.
- Use the Designer’s guidance to create distinct visual treatment for priority and risk levels.
- Add responsive rules so the dashboard stacks or collapses cleanly on narrower screens.
- Confirm typography and color choices still meet accessibility expectations.
- Output: a polished dashboard UI that looks like a finished Project Pulse frontend.

### Phase 5: Add the VS Code launch configuration
- Create `.vscode/launch.json` to support the Project Pulse dashboard in VS Code.
- Set the launch configuration to serve from `${workspaceFolder}/app` and open `index.html`.
- Use a deterministic name such as `Run Project Pulse Dashboard`.
- Ensure the launch config does not expose the directory listing; it should open the actual dashboard page.
- Output: one-click app preview inside VS Code.

### Phase 6: Validation and QA
- Open the dashboard through the VS Code launch config.
- Confirm the page loads from `app/index.html` rather than a directory listing.
- Verify all project cards render with their owner, status, activity, and priority labels.
- Check responsiveness and content overflow behavior.
- Validate that the page remains accessible and legible with keyboard-friendly structure.

## Detailed responsibilities by file

### `app/index.html`
Designer responsibilities:
- Approve layout hierarchy and information emphasis.
- Ensure the first view communicates dashboard purpose immediately.
- Recommend accessible and readable heading order and card semantics.

Coder responsibilities:
- Build the markup according to the agreed hierarchy.
- Add semantic sections and stable hooks for styling.
- Keep the structure simple and free of unnecessary framework-specific patterns.

### `app/styles.css`
Designer responsibilities:
- Define the visual system and hierarchy.
- Decide on spacing, surfaces, contrast, and badge styling.
- Ensure the cards feel polished without becoming decorative overkill.

Coder responsibilities:
- Implement the CSS faithfully.
- Verify classes match the HTML structure.
- Adjust spacing and behavior to preserve the dashboard’s readability.

### `app/project-data.json`
Coder responsibilities:
- Produce a valid JSON file with the required structure.
- Keep the values realistic and representative of contributors’ work.
- Make sure the dataset is consistent enough for the UI to render without edge-case code.

Designer responsibilities:
- Review whether the vocabulary and status names match the dashboard’s intended display.
- Ensure the content reads naturally for contributors and not like raw technical metadata.

### `.vscode/launch.json`
Coder responsibilities:
- Ensure the launch configuration is valid JSON.
- Set the correct working directory to `${workspaceFolder}/app`.
- Open the app page instead of the directory listing.

Orchestrator responsibilities:
- Confirm the launch target matches the brief and user experience requirement.

## Key edge cases to handle

- Empty or missing `projects` entries should not crash the page.
- Long project names or owner names should not break card layout.
- Status labels should remain readable if they use different capitalization or wording.
- Priority values should be shown consistently with a clear visual priority signal.
- Very short or very long `recentActivity` strings should not distort the card layout.
- Mobile widths should stack cards without horizontal overflow.
- Poor contrast or low-visibility badge colors should be corrected before final delivery.
- File paths must remain correct when the app is opened from VS Code in a Codespace environment.
- JSON must be valid and must not include trailing commas or malformed objects.
- If the app is launched from the wrong directory, the preview must still be corrected by the final launch config.

## Concrete validation steps

### Static validation
- Open `app/project-data.json` and confirm it is valid JSON with a top-level `projects` array.
- Inspect `app/index.html` and confirm the markup includes the expected dashboard sections and project card structure.
- Review `app/styles.css` and confirm the selectors match actual HTML elements and classes.
- Check `.vscode/launch.json` for proper JSON syntax and correct directory target.

### Functional preview validation
1. Use the VS Code Run configuration named `Run Project Pulse Dashboard`.
2. Confirm the browser opens `index.html` in the `app/` directory.
3. Verify the dashboard displays the cards, status badges, priority indicators, and project summaries.
4. Resize the browser to a narrow layout to confirm cards stack cleanly and text remains readable.
5. Check that no page section appears broken or collapsed due to missing data.
6. Confirm there is no folder listing exposed instead of the dashboard UI.

### Accessibility and polish checks
- Ensure headings and landmarks are in a logical reading order.
- Confirm text contrast is strong enough for badges and status labels.
- Ensure interactive or non-interactive elements are not ambiguous.
- Confirm the dashboard reads clearly without requiring a mouse to interpret project states.

## Risks and mitigations

- Risk: UI polish and data structure get out of sync.
  - Mitigation: lock the data contract before final styling.

- Risk: CSS selectors do not match the final HTML structure.
  - Mitigation: keep naming deterministic and align on class names before styling.

- Risk: launch config opens a directory listing.
  - Mitigation: explicitly set the working directory to `app/` and open `index.html`.

- Risk: project cards become unreadable on narrow widths.
  - Mitigation: include responsive layout rules and overflow-safe content styling.

- Risk: inconsistent status labels reduce clarity.
  - Mitigation: standardize a small set of expected values and align design with those labels.

## Exit criteria

The team can consider the dashboard complete when:

- `app/index.html` loads and renders the Project Pulse dashboard.
- `app/styles.css` provides a polished, readable layout with visible cards and badges.
- `app/project-data.json` contains a valid `projects` array that drives the dashboard.
- `.vscode/launch.json` opens the app correctly from the `app/` folder.
- The UI satisfies the brief, the agent responsibilities, and the expected user experience.
