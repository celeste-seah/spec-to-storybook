---
name: spec-to-storybook
description: Turns a design spec written in the [Storybook] format into Storybook stories with Autodocs descriptions, opens a PR, files a Linear review ticket, writes the story pairings back to the spec and asks the team for review in Slack. Use when the user gives a design spec URL and asks to move it into Storybook, document a component from its spec, or says "spec to storybook".
---

# Spec to Storybook

Documented in the Native Design Workflow & Experiments Library:
https://app.notion.com/p/3ea77dbba78881f9803ec33aafd0686f

One shot per spec, run when the component is built. Reruns detect what already exists and
skip to the first undone step. Writes stories and descriptions only. Never invents
behaviour that is not in the spec.

Product particulars (repo, Storybook paths, Linear team, Slack channel) come from the
team's config, see [CONFIG-TEMPLATE.md](CONFIG-TEMPLATE.md). Never hard-code them here.

## Quick start

`/spec-to-storybook <spec url>` inside the product repo.

## Workflow

1. **Config.** Look for the team's config where CONFIG-TEMPLATE.md says to. Ask only for
   gaps. Persist a new one before finishing.
2. **Preflight.** Hard stops: spec connector missing, `gh` not logged in, dirty tree,
   local default branch not fast-forwarded. Soft gates, report then ask to continue:
   status not Built, unticked Open questions, Stories items with a blank **Story** field.
   Use the repo's pinned Node. See [REFERENCE.md](REFERENCE.md#preflight).
3. **Read the spec.** Only `[Storybook]` sections move. `[Storybook] Description` becomes
   the meta's component description, verbatim. Each `[Storybook] Stories` item becomes
   one story in spec order, its one-line description verbatim, its copy from the
   callouts. No item, no story; no description line, no story description. Everything
   else stays in the spec.
4. **Find the component and its stories.** Locate the component file. Check Storybook
   discovers its folder; if not, add a discovery entry under the config's prefix.
   Propose a pairing per Stories item against existing exports.
5. **Missing stories.** Ask per item: write it, or not. Write it: add the story in this
   PR, following [REFERENCE.md](REFERENCE.md#story-file) and the repo's conventions.
   Not: ask how the Docs page should handle the gap.
6. **Write the story file.** Placeholders such as `{{VARIABLE}}` need a synthetic
   literal: ask, or reuse fixtures. Components that read app state need a per-story
   provider, never a shared singleton. No screenshots; story canvases are the pictures.
7. **Verify.** Lint and format changed files, run the Storybook static build, confirm
   the story ids appear in the build index. Chromatic or the team's visual check runs on
   the PR; do not try to view Storybook locally.
8. **Branch, commit, PR.** Branch and title from config defaults
   (`docs/storybook-<piece>-spec`, `docs(storybook): <piece> design spec`). Real PR,
   not draft. Body: problem, solution, stories list, tests. Stop after opening.
9. **Linear ticket.** One ticket: review and merge the PR. No description. PR and spec as
   links. Assign to the runner by Linear user id. Team, project, parent, milestone and
   labels from config. Show it before creating. Linear down: list it in the PR body.
10. **Write back to the spec.** Fill each **Story** field with the confirmed export name.
    Append one Decisions log line with the PR and ticket links. Offer to set status to
    Built. Never archive.
11. **Ask for review on Slack.** One message in the config's channel, tagging the
    config's review group, with PR, spec, story list and ticket links. Show it before
    sending. Template in [REFERENCE.md](REFERENCE.md#slack).

## Rules

- Spec order is Storybook order. The first Stories item is the Docs hero.
- Copy prose verbatim. Tighten nothing, add nothing. If the spec is wrong, say so and
  stop; do not fix it in code.
- Never change component source to make a story render.
- One run touches one component.
- Paste URLs from the run. Never retype them.
