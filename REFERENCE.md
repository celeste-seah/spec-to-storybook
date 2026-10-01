# Spec to Storybook: reference

Generic mechanics. Anything that names a team, channel, repo path or ticket lives in the
team's config ([CONFIG-TEMPLATE.md](CONFIG-TEMPLATE.md)), not here.

## Preflight

```bash
git status --porcelain            # must be empty
gh auth status                    # must be logged in
git checkout <default> && git pull --ff-only
nvm use                           # or the repo's Node manager; wrong Node breaks tooling
pnpm install --frozen-lockfile    # or the repo's install command
```

Read the spec with the Notion connector (`notion-fetch`), or open the file if the team
keeps specs in the repo. Status is the bold word in the Metadata Status row and the
title prefix, e.g. `[In-review]`.

## Spec structure

The spec is written to mirror the Autodocs page, so conversion is a copy.
Full template: [SPEC-TEMPLATE.md](SPEC-TEMPLATE.md).

```
Metadata table                       spec only
## [Storybook] Description           -> parameters.docs.description.component
   prose intro, **Use it** line
   ### How it works (numbered)
   ### Copy rules (numbered)
## [Storybook] Stories               -> one story per item, in order
   #### <Story name>
      one-line description           -> parameters.docs.description.story
      **Story:** <ExportName>        -> filled by this skill
      **<Version>** + callout copy   -> fixture data
      - **Trigger:** ...             -> comment or ignored
## Instances                         spec only: product-specific configurations
## Open questions                    spec only
## Decisions log                     spec only, append one line
```

History toggles inside the Description stay in the spec and are not copied. So does the
Instances section: Storybook documents rules and states that hold for any product, not
one tenant's copy.

## Storybook discovery

Check the Storybook main config lists a discovery entry covering the component's folder.
If the component lives outside the design system package, add an entry under the same
prefix as the design system so it appears on the same shelf with Autodocs on:

```ts
{
  directory: '<relative path to the app>',
  files: 'src/components/**/*.stories.tsx',
  titlePrefix: '<config: storybook.componentsPrefix>',
},
```

Route-level or screen-level stories usually have Docs off. Do not put spec docs there.

## Story file

File beside the component, named per the repo's story conventions. Shape:

```tsx
const componentDescription = `
<verbatim [Storybook] Description as markdown; escape backticks as \`>
`

// Per-story provider. A shared singleton makes every Docs canvas show the same
// state because the Docs page renders all stories at once.
function Preview({ data }: { data: Data[] }): React.JSX.Element {
  const client = useMemo(() => makeClient(data), [data])
  useEffect(() => { return (): void => { client.destroy() } }, [client])
  return <Provider client={client}><Component /></Provider>
}

const meta = {
  component: Component,
  parameters: {
    controls: { disable: true },                                  // if no props
    docs: { description: { component: componentDescription } },
    layout: 'fullscreen',                                         // or per conventions
  },
  tags: ['autodocs'],
  title: '<Component>',
} satisfies Meta<typeof Component>

export const FirstItem = {
  parameters: { docs: { description: { story: '<verbatim one-liner>' } } },
  play: waitForVisible,                                          // visual check waits for play
  render: (_): React.JSX.Element => <Preview data={[...]} />,
} satisfies Story
```

Notes that held on the first run and will likely hold elsewhere:

- Lint rules often demand explicit return types on `beforeEach`, `render` and `play`.
- If the component reads auth or feature flags, the Storybook mock for that hook may have
  no default; set it in the meta `beforeEach`.
- Autodocs shows the first story twice, once as Primary and once in the Stories list.
  That is Storybook's default. Note it, do not fix it inside this skill.
- If the team's conventions require a `Default` story and the spec has none, ask. Do not
  add one on your own.

Verify:

```bash
<config: lint command> <story file>
<config: format command> <story file>
<config: storybook build command>
grep -o '"<prefix>-<component>--[a-z-]*"' <storybook static dir>/index.json
```

## PR

Branch and title from config defaults. Open with `gh pr create`, real not draft. If the
user later switches it to draft, leave it. Write the body for an engineer who has not
seen the spec template: no `[Storybook]` section names, no skill vocabulary. Keep the
change list and the implementation reasons apart. Link only PRs that are still open or
merged; check with `gh pr view` before citing one.

```markdown
## Problem

One paragraph. The component shipped in <PR or commit> but has no Storybook page. Its
behaviour is only written in the design spec, so engineers leave the repo to find it.

## Solution

Add a **<prefix> / <Piece>** page to Storybook with the spec's rules and <n> stories.
No production code changes.

- **Added:** `<story file path>`
  - The Docs text (what it is, how it works, copy rules) is copied word for word from
    the [<Piece> design spec](<spec url>).
  - **<Story name>** story: one line, from the spec.
  - **<Story name>** story: one line, from the spec.
- **Changed:** `<storybook config path>` now also picks up stories in `<folder>`.
  (Only if a discovery entry was added.)

Only behaviour that applies to any product is documented here. Product-specific
configurations, such as <instance name>, stay in the spec.

## Notes for reviewers

- One bullet per non-obvious implementation choice: per-story provider and why, any
  mock the story sets and when it can be dropped, known Storybook quirks left as is.

## Tests

Storybook static build passes and indexes the stories plus the Docs page. <Visual check>
on this PR is the visual check: the new <Piece> snapshots are the ones to review.

Ticket: [<KEY-n>](<ticket url>) (add after step 9, or write "filed next" and edit later)

Generated by the `spec-to-storybook` skill, run by <runner> with <model> in <harness>.
```

Append the team's attribution footer if their conventions ask for one.

## Linear

Route: OGP gateway connector, `portal_codemode_execute`. Teams on a different Linear
connector swap this call for their own; the fields are the same.

```js
async () => {
  const user = await codemode.linear_get_user({ query: "<runner name>" }); // take id
  return codemode.linear_save_issue({
    title: "Review Storybook docs for <piece> (PR #<n>)",
    team: "<config: linear.team>",
    project: "<config: linear.project>",
    parentId: "<config: linear.parentIssue>",
    milestone: "<config: linear.milestone, optional>",
    priority: 4,
    labels: ["<config: linear.labels, only ones that exist>"],
    links: [
      { url: "<pr url>", title: "PR #<n>" },
      { url: "<spec url>", title: "<spec title>" },
    ],
    assignee: "<runner user id>", // email silently does nothing
  });
};
```

No description. `links` must be objects. Look labels up at run time with
`linear_list_issue_labels`; skip any that do not exist. If the connector is down, put the
ticket text in the PR body and say so.

## Slack

Route: OGP gateway connector, `portal_codemode_execute`. Group handles render as
`<!subteam^ID>`.

```js
async () =>
  codemode.slack_slack_send_message({
    channel_id: "<config: slack.channelId>",
    message: [
      "hi <!subteam^<config: slack.reviewGroupId>> could someone review <PR_URL|PR #N: docs(storybook): PIECE design spec>?",
      "storybook docs for the PIECE, carried over from the design spec: <SPEC_URL|SPEC_TITLE>",
      "stories: STORY, STORY, STORY. no production code changes.",
      "tracked in <TICKET_URL|TICKET>",
    ].join("\n"),
  });
```

Keep "no production code changes" only when the diff touches nothing outside story files
and Storybook config. Drop the ticket line if Linear was down. Show the rendered text
before sending. If the gateway is down, fall back to the direct Slack connector and say
the message will post without the connector footer.

## Spec write-back (Notion)

Use `notion-update-page` with `update_content` and small exact `old_str` matches; long
matches fail on whitespace. Avoid `replace_content` on a spec with embedded blocks such as
a Linear embed, it drops them.

- Story field: replace the `**Story:** ` line under the item with the export name.
- Decisions log: append `- YYYY-MM-DD: stories and docs in [PR #n](url), eng review in [TICKET](url).`
- Status: change the bold word in the Status row and the title prefix together.
