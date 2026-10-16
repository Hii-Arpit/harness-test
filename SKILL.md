---
name: verify-docs-ui
description: Verify Harness Developer Hub instructions and UI claims against a live Harness environment with a reusable Playwright workflow, and capture publication-quality screenshots for the pages you edit. Use when reviewing or rewriting docs that describe Harness screens, buttons, fields, wizards, pipelines, templates, tables, or end-to-end UI flows, when feedback requires checking the product before editing, or when a page needs screenshots or its existing screenshots refreshed.
---

# Verify docs against the Harness UI

Use product evidence before changing documentation. Treat the current UI as evidence, not as automatic proof that a draft is wrong. Record environment, account scope, feature flags, and incomplete loads that can explain differences.

The bundled runtime controls a persistent Chrome profile over CDP and stores all session data and evidence outside the repository by default. The same session also captures the screenshots the edited pages ship with, so the images and the claims come from one verified view of the product.

## Workflow

### 1. Turn the docs into verifiable claims

Read the target page and relevant review feedback. Create a short claim inventory:

- Exact UI labels and element types.
- Navigation sequence.
- Required and optional fields.
- Defaults, generated values, and validation.
- Branches, tabs, menus, and alternate flows.
- Result state after save, run, import, or delete.
- Product behavior described as a limitation or guarantee.

Assign stable IDs such as `C01`, `C02`, and `C03`. Do not start with exploratory clicking and reconstruct claims afterward.

Classify each claim:

- **Observable:** Verify directly in the UI.
- **Environment-dependent:** Verify only with the environment and feature state recorded.
- **Backend-dependent:** UI evidence is insufficient. Use an API, execution result, or authoritative source too.
- **Destructive:** Requires explicit user approval before execution.

### 2. Prepare the isolated runtime

Run:

```bash
node .claude/skills/verify-docs-ui/scripts/setup.mjs
node .claude/skills/verify-docs-ui/scripts/session.mjs start https://qa.harness.io/
```

The setup creates `${HARNESS_DOC_VERIFY_HOME:-~/.cache/harness-doc-ui-verifier}` and installs `playwright-core` there. The persistent Chrome profile and evidence never enter Git.

Check readiness:

```bash
node .claude/skills/verify-docs-ui/scripts/session.mjs status
```

Use `HARNESS_DOC_VERIFY_CDP_PORT` to override port `9222`.

The profile keeps its authentication between sessions and across days, so a returning session is usually already signed in. Confirm that with one cheap navigation before you interrupt the user. Only if Chrome lands on sign-in, ask them to authenticate in that browser.

Never copy cookies, browser profiles, tokens, or local storage from another profile. Never print secrets or authentication storage. Do not delete the profile as cleanup: signing in is the only step here that cannot be automated, so keeping it is what makes the next session cheap.

### 3. Read the screen before writing the scenario

Do not guess selectors. A wrong guess costs a full replay, and replaying the
flow to fix one step is the largest single cost in this workflow.

Navigate the managed browser to the screen you are about to verify, then run:

```bash
node .claude/skills/verify-docs-ui/scripts/snapshot.mjs
```

This prints the accessibility tree and every addressable control on the current
screen, each rendered as the runtime helper call that would reach it. Write the
scenario against that output so the first run is the only run.

Options:

- `--selector="nav"` scopes the tree to one region instead of the whole page.
- `--out=name` also saves `name.aria.yml` and `name.controls.json` under the runtime home.
- `--interactive` drops label elements from the control list.
- `--max=N` caps the printed tree length. The default is 24000 characters.

A text or accessibility dump is evidence of what is present, never proof of what is absent. It does not include the placeholder of an empty input, so a search box is invisible to it. Before you document that a control is missing, or delete a line that describes one, check the inputs directly or look at a screenshot, and check more than one instance of the screen.

Go to [harness-ui-patterns.md](references/harness-ui-patterns.md) for the selector catalog, the waiting rules, the Pipeline Studio sequence, and the traps that turn a fast run into a wrong edit. Read it before your first scenario against an unfamiliar Harness surface.

### 4. Create a scenario

Generate a scenario from the claim inventory and save it in the runtime scenarios directory shown by setup. Do not ask the user to copy or edit the example unless they specifically want to author the scenario themselves. Use `scripts/example-scenario.mjs` as the starting structure.

Declare the interaction scope in `meta`:

- **`observe`:** Navigate, open panels and menus, filter, search, and inspect without persistent changes.
- **`mutate`:** Create or edit dedicated `docs-verify-*` resources after the user approves the listed side effects.
- **`execute`:** Run tests, pipelines, experiments, scans, or other workloads after the user approves cost and operational impact.
- **`destructive`:** Delete resources or change shared configuration only after explicit approval for those exact actions.

List expected side effects and cleanup in the scenario metadata. If exploration reveals an unlisted side effect, stop before the action and ask.

A scenario exports:

```js
export const meta = {
  name: 'short-kebab-case-name',
  environment: 'Harness QA',
  docs: ['docs/path/to/page.md'],
  interaction: 'observe',
  sideEffects: [],
  pageUrlPattern: '^https://qa\\.harness\\.io/',
  fullPageScreenshots: false,
};

export default async function verify(ui) {
  await ui.claim('C01', 'The page displays a Create button', async () => {
    await ui.goto('https://qa.harness.io/...');
    await ui.expectRole('button', 'Create');
  });
}
```

Prefer the runtime helpers:

| Helper | Use for |
|---|---|
| `ui.ensureUrl(url, { match? })` | Enter a screen without reloading when already there. |
| `ui.goto(url)` | A deliberate full page load. |
| `ui.clickRole(role, name, options?)` | Click or select a semantic element. |
| `ui.clickText(text, options?)` | Click visible text when no role fits. |
| `ui.clickFirst([locators], options?)` | Click the first locator that becomes visible. |
| `ui.firstVisible([locators])` | Resolve a locator without clicking. |
| `ui.fillLabel(label, value)` | Enter text in a labeled field. |
| `ui.fillPlaceholder(placeholder, value)` | Enter text when only a placeholder exists. |
| `ui.selectOption(label, option)` | Pick from a native select. |
| `ui.expectRole(role, name)` / `ui.expectText(text)` | Assert a documented label. |
| `ui.aria(selector?)` | Accessibility tree of the page or a region, as YAML. |
| `ui.text()` | Visible page text as one string. |
| `ui.present([strings])` | Which documented labels are actually on screen. |
| `ui.settle(options?)` | Wait for the view to stop changing. |
| `ui.waitUntil(description, predicate, options?)` | Poll for a result state such as a finished execution. |
| `ui.checkpoint(name, details?)` | Full evidence including a screenshot. |
| `ui.probe(name, details?)` | Text and control evidence without a screenshot. |
| `ui.note(text)` / `ui.uniqueName(prefix)` | Record a decision, generate a `docs-verify-*` name. |

Use `ui.page` only when no semantic helper can express the interaction. If you use it, prefer `getByRole`, `getByLabel`, `getByPlaceholder`, or a stable `data-testid`. CSS selectors and forced clicks are last resorts and must include a note explaining why.

Scenarios are dynamic. They may branch based on visible controls, available test data, permissions, or feature state. Capture the branch decision as a note. Do not hard-code coordinates when a semantic locator or stable test ID exists.

### 5. Run and resume

Run:

```bash
node .claude/skills/verify-docs-ui/scripts/run-scenario.mjs /absolute/path/to/scenario.mjs
```

Options:

- `--claims=C05,C06` reruns only those claims and marks the rest `skipped`. Use it to iterate on one broken step instead of replaying the whole flow. State-dependent claims must still run in order, so include the claims that set up the state.
- `--resume` skips claims that passed in the previous run and navigates to the screen that run reached, so a rerun does not replay work that already succeeded.
- `--screenshots=all` captures every state, `--screenshots=off` captures none. The default `auto` captures screenshots at `ui.checkpoint` calls and on failed or blocked claims.
- `--no-cache` ignores remembered locators and probes every candidate list from scratch. Use it when you suspect the cache is masking a real UI change.

The runner:

- Reuses the authenticated browser.
- Waits for actionable states instead of fixed sleeps.
- Remembers which candidate in a `ui.clickFirst` or `ui.firstVisible` list resolved, and tries it first next time. A candidate that stops resolving is re-probed and re-recorded, so a UI change costs one slow run rather than every run.
- Records the status and screen of each claim, which is what `--resume` reads.
- Captures one result artifact per claim, plus whatever the scenario captures explicitly.
- Saves screenshots, visible text, visible controls, URL, console errors, failed HTTP responses, and structured results.
- Redacts common credentials and tokens from text artifacts, and strips credential-bearing query parameters from stored URLs.
- Marks claims `passed`, `failed`, `blocked`, `needs-corroboration`, or `skipped`.
- Reports per-claim duration and locator cache hits so you can find the slow steps.

If a flow stops for user input or an environment prerequisite, record it as blocked. Do not improvise a passing result.

### Keep runs fast

Browser actions are not the bottleneck in this workflow. Replaying a whole flow
to fix one step is. Optimise the number of runs first, then the runs themselves:

- **Snapshot before authoring.** Run `snapshot.mjs` and write against the real tree. This is the single largest saving, because it removes the guess, run, fail, guess cycle.
- **Iterate with `--claims` and `--resume`.** Fix one step and rerun that step, not the flow.
- **Navigate once.** Enter the module with `ui.ensureUrl` or a single `ui.goto`, then move with in-app clicks. Every extra `ui.goto` is a full application reload.
- **Verify a screen once.** Group all claims about one screen together, before moving on. Do not leave and re-enter a screen to check a second label.
- **Screenshot deliberately.** Use `ui.probe` for label and control checks, and reserve `ui.checkpoint` for states where layout, icons, or visual hierarchy matter. Set `fullPageScreenshots: true` in `meta` only when you need content below the fold, since full-page capture is slow on long lists.
- **Never sleep for a fixed time.** Use `ui.settle` for view changes and `ui.waitUntil` for result states. Both return as soon as the condition holds.
- **Reach a screen once, then take every shot it owes you.** Getting into a deep surface such as Pipeline Studio is most of the cost; capturing a second region once you are there is nearly free. Decide the full image list for a screen before you leave it.
- **Run `capture.mjs` as its own command.** Nesting it inside a `node --input-type=module -e "..."` block breaks on quote escaping and fails with a bare `Node.js v24.x` error that names no cause.

### 6. Inspect evidence before editing

Read `summary.json`, `report.md`, and relevant text artifacts. Read screenshots when layout, icon identity, modal structure, or visual hierarchy matters.

For each claim, decide:

- **Confirmed:** Evidence directly supports the wording.
- **Contradicted:** Evidence directly disproves the wording.
- **Partially confirmed:** The wording is broader than the evidence.
- **Blocked:** Authentication, permissions, data, feature flags, or product errors prevented verification.
- **Needs corroboration:** The claim cannot be established from the UI alone.

Do not treat absent UI as a product limitation until the page is fully loaded, the expected permissions, scope, and feature flags are confirmed, and you have seen the same absence on more than one instance of the screen. A single resource can be old enough to predate the control you are looking for.

Prefer "the UI shows X" to "the product does not support Y" when all you have is UI evidence. When a control is present but never populates, and the data it needs does not exist either, report it as open rather than writing it up as conditional or as obsolete.

### 7. Capture documentation screenshots

Run artifacts are evidence and must not ship. When a page needs an image, capture it deliberately with:

```bash
node .claude/skills/verify-docs-ui/scripts/capture.mjs \
  --selector='.bp3-dialog' \
  --out="$PWD/docs/<space>/.gitbook/assets/name.png"
```

The script crops to a region, emulates a 2x display, hides the support chat, toasts, tour callouts and the Ask AI widget, and can click a menu or tab open first. Run `--help`-style flags from its header comment.

Always read the captured file back and judge it as a reader would, then re-frame and re-capture. The first capture is routinely wrong in a way the flags fix: a cut-off page header, a band of empty space, a half-rendered form.

Be deliberate about how many images a page gets. An image per heading buries the reader and rots on the next UI change; almost none leaves prose describing layout that one picture would settle. When you do add images, delete the prose they replace, so the two do not have to be kept in sync.

Go to [screenshots.md](references/screenshots.md) for when an image earns its place, the framing and naming rules, the masking checklist, and the GitBook asset conventions.

### 8. Update docs and verify the diff

Edit only claims supported by evidence or clearly label uncertainty for the user. Follow the repository documentation rules, which the root `CLAUDE.md` and `.cursor/rules/` define.

Two of those rules are easy to get wrong while transcribing a UI you have just been driving. Use **click** for buttons and **select** for menu items, options, tabs, and cards, so a menu becomes "Click **+ New Load Test**, then select **Create from scratch**". Write links as "Go to X to Y", never "see" or "refer to".

After edits:

1. Re-run affected claims if wording changed materially.
2. Check that every relative link resolves to a file on disk and every image path resolves to an asset in the space. There is no build step or local link checker in this repository, so an unresolved path surfaces only in the GitBook change-request preview.
3. Ensure no runtime profile, evidence artifacts, generated scenarios, or the runtime home itself were added to Git. Only the documentation pages and the assets you captured belong in the change.
4. Report confirmed changes separately from blocked or inferred items, and say which environment and account the evidence came from.

Use [report-format.md](references/report-format.md) for the final verification summary.

## Safety rules

- Default to read-only exploration.
- Ask before actions that run pipelines, experiments, scans, load tests, deletes, imports, provisioning, or changes shared resources.
- Use unique `docs-verify-*` names for approved test resources.
- Record created resources and clean them up only when deletion was also approved.
- Never clone a real Chrome profile or extract cookies, tokens, passwords, local storage, or session storage.
- Never capture password fields, access keys, secret values, request authorization headers, or full API payloads that can contain credentials.
- Restrict screenshots to relevant product pages. Close unrelated tabs before capture.
- Inspect every image you intend to publish for real customer names, email addresses, user avatars, account identifiers, and secret values. Mask them with `--mask` or pick a different row. Never capture a password field, access key, token, or authorization header, masked or otherwise; re-frame instead.
- Stop if the active account, organization, project, or environment differs from the approved target.

## Finish the session

Keep the session when more verification is expected. Otherwise run:

```bash
node .claude/skills/verify-docs-ui/scripts/session.mjs stop
```

Do not delete the persistent profile unless the user asks. It avoids repeated authentication while remaining isolated from the user's normal browser.
