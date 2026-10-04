# Accepting an interactive demo

Read this when building or revising an interactive slice. It addresses a specific
failure: a landing page passed contrast, overflow and a few scripted clicks while
its Files/Workflows navigation and composer were decoration, and its primary
action ended with a success message instead of a usable result.

## Start with a visitor contract

Choose one useful journey grounded in the real product. Record:

- The visitor's starting state and goal.
- The visible entry point and actions.
- The observable result of each action.
- State that survives navigation, state requiring explicit save, and what resets it.
- Which parts are simulated, which are screenshots and which are actually connected.
- Whether the sample content gives a visitor a reason to care about the action. A
  development smoke fixture can pass every click assertion while still hiding the
  product's value. If the fixture is not representative, create a coherent sample task
  and make the guide respond to the visitor's visible result.

An illustrative contract for a coding workspace:

| Visitor action | Observable result | Failure to catch |
|---|---|---|
| Resume a sample project | Approval request appears; keyboard focus can reach it | Button only changes a caption |
| Deny startup | Nothing starts; a clear recovery path remains | Success state appears despite denial |
| Approve startup | A usable sample app appears | Text claims an app opened, but none is visible |
| Change sample data | Output changes and survives view switches | Navigation silently resets state |
| Edit notes, leave without saving | Draft survives; downstream action reads the saved version | Unsaved edits leak into a run |
| Save notes, run the sample workflow | The saved content appears in its output | Fixed output ignores visitor input |
| Reset | Conversation, output and sample data return to the initial state | Only the active scene resets |

Use only rows relevant to the product. Do not turn this example into a requirement
that every demo needs files, workflows or approvals.

## Establish product fidelity first

Inspect the product's screenshots as images and the source for the states being shown.
Keep reference captures beside the demo captures and compare like states at comparable
viewport sizes: navigation, panel proportions, typography, density, controls and the
actual sample/result. Adding real screenshots below an invented demo does not establish
fidelity. Prefer reuse of production components/styles with local adapters when practical.

Document intended differences such as synthetic data and larger reading sizes. A mobile
demo may show one pane at a time rather than miniaturizing a desktop UI; the action's
result must remain visible and the return path obvious. Test the actual embed width and
any expanded view, including closing it with the keyboard.

When reusing application code, isolate its storage and replace API/native bridges at
build time. Ensure no real command, provider, filesystem or account request can escape
the simulation. Unsupported operations should explain their limit when invoked. Test
forms inside the final iframe sandbox: a component can render and accept clicks while
the sandbox prevents its submit event, leaving Save or Run inert.

## Audit the apparent controls

Inspect the rendered page, not just its button selectors. Account for tabs, chips,
plus icons, search fields, message composers, menus, project switchers and links.
A styled span can promise an action as strongly as a button.

For each, implement a meaningful in-scope behavior, make unavailability explicit,
or remove the control. Do not simulate a freeform AI composer unless its supported
behavior is clear. Label a screenshot as a screenshot and allow inspection/zoom
where useful.

Keep the first action discoverable without an explanation from the builder.
After an action, make the payoff visible at both desktop and mobile widths.
When replacing a focused control, move focus somewhere useful; closing a dialog
should return focus to its trigger. Preserve reduced-motion behavior.

## Test the contract rather than the implementation

Extend the project's existing browser checks. Enter through the same CTA a visitor
uses. Click visible controls and assert results in the rendered page. Avoid forcing
clicks, calling internal handlers or mutating page state to make the journey pass.

Check the happy path and relevant alternatives: deny/cancel, unsaved changes,
switch-away-and-return, reset, keyboard access and repeat use. For user-entered
sample text, verify it remains text rather than becoming HTML. If a feature is
intentionally a fixed simulation, label it so the result cannot imply a live run.

Inspect screenshots of meaningful states, including the result, on narrow and wide
screens. No overflow does not prove that text is readable, controls are reachable,
or the payoff is on screen.

## Separate the evidence

| Gate | What it establishes |
|---|---|
| Render checks | The measured contrast, specificity and overflow properties |
| Journey checks | The named behaviors in the tested environment |
| Visual review | Legibility, hierarchy, discoverability and visible payoff |
| Product fidelity | Rendered states match inspected product references, with deliberate deviations recorded |
| Delivery check | The same journey works at the URL or embed the visitor receives |

An HTTP 200, screenshot, completed click, local test, push or deployment command
alone does not establish all four. Use the same journey for offline/file, local
HTTP and hosted checks where applicable. After publishing is authorized, verify the
actual advertised URL, release/download links and hosting/iframe restrictions.
Do not claim production verification from localhost.

Capture the final source revision, environments, observed outcomes and remaining
limits alongside durable screenshots. Keep release claims current when other work
advances during a paused redesign. Describe actual passing checks in the handoff
instead of an unqualified “the interactive test passes.”
