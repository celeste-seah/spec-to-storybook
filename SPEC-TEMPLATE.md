# [Status] <Piece> / Design spec

> Written for one behaviour-heavy design piece: a banner, a modal, a dialog, a switching
> flow. Sections marked **[Storybook]** are copied verbatim into the component's Storybook
> Autodocs page when the piece is built, so write them in final phrasing. Everything else
> stays here. Prefix the title with the current status and bold the same word in the
> Status row.

| Metadata | Details |
|---|---|
| **Piece** | name |
| **Objective** | one line |
| **Person in charge** | name |
| **Status** | **Draft** → In-review → Agreed → Built → Archived |
| **Ticket** | link |
| **Design file** | link, or Not applicable |
| **Related docs** | links |
| **Storybook** | add once Built |

---

## [Storybook] Description

Two or three sentences of prose: what the piece is, what it is for, where its content
comes from. This becomes the opening of the Storybook Docs page. Follow with a **Use it**
line, and a **Do not use it** line if there is a real anti-pattern.

### How it works

Numbered rules. Each states the trigger, what the system does and what the user sees.
Add a History toggle under a rule only if it was debated, one line per change like a git
log. Toggles stay here and are not copied.

1. When X, the system does Y, the user sees Z.
2. Rule that was debated
   - History: YYYY-MM-DD, what changed, who, link

### Copy rules

1. Length limit and what happens when exceeded.
2. Links and calls to action.
3. Variables and fallbacks.

---

## [Storybook] Stories

One item per story, in the order they should appear. Autodocs renders the first item as
the page hero, so lead with the states or the most common configuration. Instances, edge
cases and states all live here; the one-line description says which it is. All copy for
the piece lives in the callouts; other docs link here rather than repeating it.

#### <Story name>

One line: what this story shows and the rule it demonstrates. Copied under the canvas.
**Story:** export name, filled in once the story exists

**<Version, if the story shows more than one>**
> exact copy, with template variables in {{BRACES}}
- **Trigger:** what turns it on and off

---

## Open questions

Tick a question when decided and write the decision after the arrow. Empty or all ticked
means ready for build.

- [ ] Question
- [x] Answered question → **decision**

---

## Decisions log

One line per decision. Date, decision, link. Reasoning stays in the linked page or thread.

- YYYY-MM-DD: decision. link
