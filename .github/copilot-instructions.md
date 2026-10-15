# Copilot instructions

## Project shape and commands

This is a standalone, dependency-free browser quiz. The entire app lives in `index.html`: markup, CSS, quiz data, and JavaScript are inline. There is no package manifest, build step, automated test runner, or lint command.

- Run it by opening `index.html` directly in a browser; no server is required.
- Check changes in the browser by answering questions, confirming score/progress and feedback, finishing the quiz, and using **Play again**.
- There is no automated single-test command; validate the relevant interaction in the browser.

## Application structure

The HTML defines one mission-control screen with two mutually exclusive views: the quiz (`#quiz`) and results (`#result`). The inline `questions` array is the source of truth for question text, answer options, correct option indexes, categories, and feedback facts. `renderQuestion`, `selectAnswer`, and `showResults` drive the quiz state and transition between those views. The result reaction and celebration threshold are derived from the final score.

CSS in the document’s `<style>` block owns layout, interaction states, animation, and visual theme. JavaScript updates existing elements and creates answer buttons from the question data; there is no framework, module boundary, or external asset pipeline.

## Codebase-specific conventions

- Keep the app self-contained in `index.html`; do not add runtime dependencies, external assets, or a server requirement unless the project’s scope changes.
- Treat element IDs used by the inline script as DOM contracts. Update the markup and its JavaScript references together when changing them.
- Keep question content in the `questions` data array rather than duplicating it in markup. `answer` is a zero-based index into `options`.
- Use the existing CSS custom properties for colors so light and `prefers-color-scheme: dark` themes stay aligned. Keep responsive adjustments in the existing mobile media query.
- Preserve the native-button interaction model and visible `:focus-visible` treatment. Keep question feedback in its live region, maintain the progress bar’s ARIA value as questions advance, and move focus when the quiz changes screen or question.
- Honor `prefers-reduced-motion` for decorative and interaction animations; keep decorative layers non-interactive and hidden from assistive technology.
- Each new run uses a Fisher–Yates shuffle of the question records and avoids repeating the immediately previous order. Keep each question’s options, correct answer, and fact attached to that record; track selected options in the shuffled run order for the results review.
- The personal-best score is stored under `orbit-space-quiz-personal-best` in local storage. Storage access may fail, so preserve quiz behavior with the in-memory fallback.
- The celebratory results treatment starts at 8 correct answers. Keep its copy, reaction, badge, and styling consistent if quiz length or scoring changes.
