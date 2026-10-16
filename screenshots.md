# Documentation screenshots

Evidence screenshots and documentation screenshots are different jobs. The runner
captures evidence: whole viewport, for you to read while deciding whether a claim
holds. `capture.mjs` captures assets that ship on a page, so they are cropped,
high density, free of floating chrome, and reviewed before they land.

Do not publish a run artifact. Re-capture with `capture.mjs`.

## When a screenshot earns its place

A screenshot is worth adding when a reader cannot act on the prose alone, or when
the image replaces prose rather than repeating it. The test is whether removing
the image costs the reader something.

Add one for:

- **A screen whose layout carries the meaning.** A results page with cards,
  charts, and tables in a particular arrangement. One image beats four
  paragraphs describing where things sit.
- **A dense field reference.** A configuration panel where the reader needs to
  confirm they are looking at the right form, and where the field order matters.
- **A visual relationship.** Steps nested inside a parallel block, a graph
  topology, a heatmap. Prose describing a shape is always worse than the shape.
- **A named surface that is hard to find.** A tab buried three levels into
  settings, or a group inside a long picker list.
- **A non-obvious state.** An empty state with specific wording, a "Coming Soon"
  control, a warning that only appears under one configuration.

Do not add one for:

- **A single button or field** the prose already names in bold.
- **A navigation path.** Write the path; do not photograph the sidebar.
- **Something that repeats the sentence next to it.** If the caption and the
  prose say the same thing, delete one of them.
- **A long scrolling list** where only the first rows are meaningful. Crop to
  what matters.
- **Anything you cannot re-capture.** An image you cannot reproduce becomes
  permanently stale.

Two failure modes are equally bad, and both have happened on this skill. Adding
a screenshot per heading buries the reader and guarantees the page rots the next
time the UI shifts. Adding almost none leaves whole pages of prose describing
layout that one image would settle. Decide per page, not per project: ask which
pages a reader works *through* while looking at the product, and put the images
there.

When you add images, look for prose to delete. Screenshot plus the full original
description is the worst outcome, because the two now have to be kept in sync.
Keep the sentences that state a rule, a value, or a consequence; drop the ones
that narrate what the picture shows.

## Capture workflow

Never accept the first capture unattended. The loop is capture, read the file
back, adjust framing, re-capture. Reading the image is not optional; it is how
you catch a spinner, a clipped panel, a stray tooltip, or a tenant name.

```bash
node .claude/skills/verify-docs-ui/scripts/capture.mjs \
  --url='https://qa.harness.io/ng/account/<acct>/module/chaos/.../dr-testing' \
  --wait='input[placeholder="Search"]' \
  --selector='[class*="DefaultLayout-module_content"]' \
  --out="$PWD/docs/<space>/.gitbook/assets/dr-tests-list.png"
```

Then read the PNG back and judge it as a reader would. Common corrections:

| What you see | Fix |
|---|---|
| Header, primary button, or filters cut off | Drop `--selector` and capture the viewport, or select a container that includes the header |
| Large empty area below the content | `--trim=0,0,N,0` |
| Half-rendered form or a spinner | `--wait=CSS` for a field the shot must contain, then raise `--settle` |
| Element taller than the screen is cut | `--fullpage` alongside the selector, which lifts the viewport clamp |
| Wrong instance of a repeated component | `--selector='.card:has-text("Resilience Testing")'` |
| Tab or menu closed | `--click='text:Load Tests'` before capture |

`--trim` runs after the viewport clamp, so it works on elements taller than the
screen. If a trim appears to do nothing, you are on an older copy of the script.

## Framing

- **Include the surface the reader must recognise.** A list screenshot with no
  page header is disorienting. When the header carries the primary action, keep
  it, even at the cost of a wider image.
- **Crop to one idea.** A dialog, a panel, a card group, a graph. If you are
  describing two things, take two images.
- **Keep the left navigation only when location is the point.** It is useful on
  the first image of a page, where it shows which module the reader is in, and
  noise on every image after that.
- **Default to `1440x900`.** Go wider, up to about `1680`, only for a table whose
  columns would otherwise wrap. Keep `--dsf=2`.
- **Let GitBook do the zooming.** It makes images click-to-expand natively, so
  never add a "Click to view full size" caption.

## Content safety

`capture.mjs` always hides the support chat, toasts, tour callouts, the "New UI
experience" popover, and the Ask AI widget. That list handles floating chrome; it
does not handle data.

Before you save an image, check it for real customer names, email addresses,
avatars of real users, account identifiers, and secret values. Mask them with
`--mask`, or pick a different row. Test-project names, sample pipeline names, and
the demo account identifier in a URL bar are fine.

A mask is only as good as its selector, and a selector that matches nothing fails
silently: the capture succeeds and the data is still in the image. After masking,
read the file back and confirm the block actually covers what you meant. Masking
an avatar container frequently leaves the adjacent name text untouched, because
they are separate elements. When you cannot pin the selector down quickly,
re-frame or crop the column out instead; that is faster and cannot fail quietly.

Never capture a password field, an access key, a token, or an authorization
header, masked or otherwise. Re-frame the shot instead.

## Committing the image

- Save into the **space** assets directory: `<space>/.gitbook/assets/name.png`.
  The space is the directory containing `SUMMARY.md`, which is not the same as
  the module directory. Walk up from the page until you find it.
- Name by what the image shows, matching the feature vocabulary:
  `dr-chaos-probe-step.png`, `load-thresholds-fleet-suggestion.png`. Not
  `screenshot-3.png`.
- Reference it relatively: `![alt](../.gitbook/assets/name.png)`.
- Write alt text as a sentence describing what the image shows, because it is
  also the caption: `![The DR Tests list, with one row per pipeline that
  contains a Disaster Recovery stage](...)`. Not `![screenshot]`.
- Replace an outdated image in place, keeping the filename, so existing
  references keep working.
- Check every image path resolves before committing. A missing asset renders as
  broken alt text on the published page.
