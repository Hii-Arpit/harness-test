# Harness UI automation patterns

Everything here was paid for with a failed run. Read it before writing a scenario
or a capture command against a Harness screen.

## Traps that produce wrong documentation

These are worse than a timeout, because they end in a confident, incorrect edit.

### A text dump does not show input placeholders

`document.body.innerText` and the accessibility tree do not include the
placeholder text of an empty input. A search box reading `Search` is invisible to
both.

This has caused a real regression: a **Search load test template** field was
deleted from a page because it did not appear in a text dump, and had to be
restored after a screenshot showed it plainly on screen.

**Never conclude a field is absent from a text dump.** To check for a field:

```js
for (const i of await page.locator('input:visible, textarea:visible').all()) {
  console.log(await i.getAttribute('name'), await i.getAttribute('placeholder'));
}
```

Or take a screenshot and look at it. Prefer both when you are about to delete
documentation.

### Absence needs corroboration more than presence does

A control that is missing can mean the product removed it, or that the page had
not finished loading, or that this account lacks the feature flag, or that the
data needed to render it does not exist, or that you are looking at a resource
whose age predates it.

Before documenting a removal:

1. Confirm the page fully loaded, including the Harness spinner.
2. Check more than one instance. The Environment tab was declared gone only after
   three separate DR pipelines all showed three tabs.
3. Consider feature flags and account scope, and record them.
4. Prefer "the UI shows X" over "the product does not support Y" when you only
   have UI evidence.

Conversely, do not treat a control that never populates as proof of a bug. Three
Resilience metrics cards stayed empty for every reachable service, but every one
of those services had zero test runs, so conditional-versus-obsolete stayed
undecided and was reported as open rather than written up as either.

### Verify against the drawer, not only the library card

Harness is mid-rename in several places, and the two ends disagree. The Step
Library card reads **Chaos Probe** while the drawer it opens is titled
**Resilience Test Probe**. In Project Settings, hubs are listed as **Resilience
Test Hubs** with a **New RT Hub** button, but the docs and API call them
ChaosHubs.

Document the label on the surface the reader is looking at, and say so when two
surfaces disagree. Do not silently pick one.

## Waiting

Harness reports `networkidle` while its own spinner is still on screen, so
`waitForLoadState` alone captures half-built views. Wait for the spinner to
disappear:

```js
await page.getByText('Loading, please wait', { exact: false })
  .first().waitFor({ state: 'hidden', timeout: 30_000 }).catch(() => {});
```

`capture.mjs` does this on every capture. In a scenario, use `ui.settle`.

A panel also renders its frame before its fields, so waiting for the container
is not enough. Wait for a specific field you expect inside it, which is what
`--wait` is for.

Budget roughly 12 to 16 seconds for a first module load and 5 to 8 seconds for an
in-app tab switch. These are slow screens; a 30 second default timeout is not
generous.

## Selector catalog

Harness uses Blueprint components with CSS-module class hashes. Hashed names such
as `.BqPfve` change between builds, so use them only inside a single session, and
prefer the stable forms below.

| Target | Selector |
|---|---|
| Content area right of the left nav | `[class*="DefaultLayout-module_content"]` |
| Modal dialog | `.bp3-dialog` or `[role=dialog]` |
| Step or entity configuration drawer | `.bp3-drawer` |
| Open dropdown or menu | `.bp3-popover-content` |
| Tab in a tab strip | `[role=tab]` |
| Pipeline canvas stage or step node | `[data-nodeid]` |
| Summary and heatmap cards | `[class*="summaryCards"]`, `[class*="heatmapCard"]`, `[class*="topCards"]` |

To disambiguate a repeated component, add `:has-text()` rather than an index:

```js
page.locator('.bp3-dialog:has-text("Create new DR Test")')
```

Scope a tab click, because tab labels collide with navigation items. A bare
`getByText('Execution')` hits the left nav:

```js
await page.locator('[role=tab]', { hasText: 'Execution' }).first().click();
```

## Pipeline Studio

This is the slowest surface to automate. Expect it to cost several times what a
list page costs.

- **Click stage and step nodes through `[data-nodeid]`.** Node labels are not
  reliably clickable text, and `getByText('<stage name>')` times out.
- **The Add Step menu nests.** Clicking **Add Step** on the canvas opens a menu
  that itself contains **Add Step**, **Add Step Group**, **Use template**, and
  **Create with AI**. Click the canvas button with `.first()` and the menu item
  with `.last()`, then the step name in the Step Library.
- **Discarding a step drawer loses the stage selection.** Adding a second step
  after a discard fails. Reload the pipeline and redo node click, then tab click,
  for each step you want to inspect. Do not try to loop over step types in one
  pass.
- **Pickers cascade.** A probe, fault, or action list is empty until an
  infrastructure is selected, and an infrastructure with no onboarded services
  blocks the wizard's **Next**. Fill the upstream field first.
- **The URL leaves the module.** Opening a DR or load pipeline lands on
  `/ng/account/<acct>/all/orgs/.../pipelines/<id>/pipeline-studio`, outside the
  chaos module path. Match on `pipeline-studio` rather than the module.

You can add and configure steps to inspect their fields without persisting
anything, as long as you never click **Save**. The studio will show unsaved
state; navigating away discards it. **Apply Changes** on a drawer only updates
the in-memory pipeline.

## Routes are not guessable

Module routes do not follow the documentation's URL structure, and a wrong guess
returns a 404 page that looks like a missing feature. Both `/chaos-hubs` and
`/dr-tests` 404; the real routes are `/settings/chaos/hubs` and `/dr-testing`.

Navigate by clicking the left nav item once, then read `page.url()` and reuse
that URL for the rest of the session.

## Wizards and pagination

A picker with its own pager puts `Prev`, `(1-1) of 1`, and `Next` inside the
list, so a footer `Next` click can hit pagination instead of advancing the
wizard. Match the wizard control by its exact composed text, which includes the
icon name:

```js
await page.getByText('Nextchevron-right', { exact: true }).click();
```

## Shell quoting

Run `capture.mjs` as its own command. Nesting a capture inside a
`node --input-type=module -e "..."` block breaks on quote escaping and fails with
a bare `Node.js v24.x` error that says nothing about the cause.

Keep inline `node -e` blocks for inspection, and use the script for capture.

## Session persistence

The persistent profile in `${HARNESS_DOC_VERIFY_HOME}/profile` keeps its
authentication across sessions and across days, so a returning session is
normally already signed in. Check with `session.mjs status` and a cheap
navigation before asking the user to authenticate again.

Do not delete the profile as cleanup. Losing it costs a manual sign-in, which is
the one step in this workflow that cannot be automated.
