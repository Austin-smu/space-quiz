# Copilot Instructions for Space Quiz

## Architecture & Technology Stack

- **Single-File Application**: Self-contained client-side web application in `index.html`. No build pipelines, bundlers, external frameworks, CDNs, or runtime server dependencies.
- **Styling Architecture**: Pure CSS custom properties for dual theme support (`:root` for dark theme, `@media (prefers-color-scheme: light)` for light theme). Monospaced HUD elements use `var(--font-mono)`. Motion effects respect `@media (prefers-reduced-motion: reduce)`.
- **Component Layout**: Centered card layout constrained to a maximum width of `480px`.
- **View Transitions**: Two primary views toggled via DOM styling (`#quiz-view` and `#results-view`).

## Build, Test, and Execution

- **Running the Application**: Open `index.html` directly in any web browser (`file:///.../index.html`) or preview it using the integrated browser canvas.
- **Testing**:
  - Manual and browser-level smoke verification can be executed via the integrated browser canvas using JavaScript evaluation against the DOM state.
  - Automated browser-level end-to-end interactions can be run using the Playwright MCP server configured in `.vscode/mcp.json`.
  - Useful DOM functions for targeted verification: `renderQuestion()`, `updateProgress()`, `handleSelect(index)`, and `showResults()`.

## Key Conventions & Domain Rules

- **Mission Control Theme**: UI elements follow aerospace mission-control styling:
  - Channels labelled as `CH-A //` through `CH-D //`.
  - Telemetry status pills (`STANDBY`, `NOMINAL`, `TRAJ_ERR`, `TRUE VECTOR`).
  - Stage indicator tracking with calibrated progress ticks and midpoint markers.
- **Passing Threshold**: A passing score requires at least **7 out of 10** correct answers (70%). Scores below 7 trigger the failed mission status banner (`MISSION STATUS: FAILED // RETRY REQUIRED`).
- **Accessibility & Keyboard Navigation**:
  - Options list uses `role="radiogroup"` with child buttons having `role="radio"` and roving `tabindex` (active option has `tabindex="0"`, others `-1`).
  - Standard arrow key navigation (<kbd>ArrowUp</kbd>, <kbd>ArrowDown</kbd>, <kbd>ArrowLeft</kbd>, <kbd>ArrowRight</kbd>, <kbd>Home</kbd>, <kbd>End</kbd>) is supported for option cycling.
  - Live feedback is announced via `aria-live="polite"` and alert roles.
- **State Management**:
  - `questions`: Array of objects containing `question`, `options`, zero-based `answer` index, and `fact`.
  - Tracked via global module state: `currentIndex`, `score`, and `answered`.
