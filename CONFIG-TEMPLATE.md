# Team config template

A living artefact, not a one-time form. Resolve it in this order at the start of every
run (see [SKILL.md](SKILL.md) step 1):

1. **Look first.** Search near the team's spec template for a section matching the shape
   below: the playbook page that holds the template, a `.claude/` file in the product
   repo, wherever this team's config was last stored. If found, use it as is.
2. **Ask only for gaps.** Anything missing, ask the user. Do not make them restate what
   is already documented.
3. **Persist it.** If none existed, create one before finishing this run, stored where
   step 1 will find it next time, and say where.

```md
# spec-to-storybook config: <product name>

## Repo
- **Repository**: <owner/name>
- **Default branch**: <main>
- **Node**: <how to select the pinned version, e.g. `nvm use`>
- **Install**: <e.g. `pnpm install --frozen-lockfile`>
- **Branch pattern**: <default `docs/storybook-<piece>-spec`>
- **PR title pattern**: <default `docs(storybook): <piece> design spec`>

## Storybook
- **Main config**: <path to .storybook/main.ts>
- **Components prefix**: <the titlePrefix shared components should appear under>
- **Component folders discovered**: <which folders are already in the stories globs>
- **Story conventions doc**: <path, if any>
- **Lint / format**: <commands to run on a story file>
- **Build**: <command for the static build> and <path to storybook-static>
- **Visual check**: <Chromatic on PR, or other>

## Spec
- **Spec template**: <link to the team's copy of SPEC-TEMPLATE.md, Notion or repo>
- **Status words**: <default Draft, In-review, Agreed, Built, Archived>

## Linear
- **Team key**: <e.g. ABC>
- **Project**: <project the piece's issue belongs to, or "from the piece's issue">
- **Parent issue rule**: <e.g. the piece's implementation issue>
- **Milestone**: <optional>
- **Labels**: <labels that exist and fit a review ticket>
- **Assignee rule**: <default: the person running the skill>

## Slack
- **Channel**: <#name> (<channel id>)
- **Review group**: <@handle> (<subteam id>)
- **Route**: <OGP gateway, or the connector the team uses>

## Known environment quirks
- <facts discovered during past runs that would otherwise need rediscovering: a mock
  with no default, a local install that fails type checks while CI passes, a dev server
  that does not serve a file. Date them if they might go stale.>
```

Keep entries short. If a field does not apply, say so rather than leaving it blank, so it
reads as confirmed absent rather than not yet filled in.

## Write back what you learn

Every product-specific fact belongs in the config, never in this skill's own files.
SKILL.md and REFERENCE.md stay usable for any product. When a run turns up something new,
append it to **Known environment quirks** rather than letting it evaporate with the
session.
