---
name: build-workspace
description: Design and create an Atwork workspace for a process, with linked sheets, fields, stages, views and example rows. Use when the user asks to set up a CRM, tracker, pipeline, content calendar, hiring process or request intake in Atwork, or to "build a system for" something they run.
---

# Build a workspace in Atwork

Propose the design in the conversation, build it only after the user agrees, and build the whole thing in
one `steps.apply` call.

## 1. Understand the process

Work out what is tracked (the things, such as deals, candidates or requests), how each one moves (its
stages), who works on it, and what they need to see. Ask one or two short questions only when a key part
is unclear; otherwise assume sensibly and say so.

## 2. Check what already exists

Call `workspace.structure`. Reuse or extend an existing space or sheet instead of creating a duplicate,
and decide where the new structure goes: an existing space (its `spaceId`) or a new one.

## 3. Propose the design

Lay it out compactly, one line per sheet, then ask whether to build it:

```
Space: Sales
Companies: Website (URL) · Industry (choice: Software, Retail, Health)
Deals: Company (link to Companies) · Value (number, USD) · Stage (Lead → Proposal → Won / Lost) · Close date (date)
Views: Deals board by Stage · Deals calendar by Close date
Example rows: 3 to 5 per sheet
```

Keep it to what the user asked for. Don't add business rules, automations or fields they didn't ask for.

## 4. Build it

Write one Steps document and call `steps.apply`. Pass `spaceId` (and `folderId`) to install into an
existing space; leave them out and include one entry in `spaces` to create a new space.

Follow `references/building-rules.md` for sheet types, the fields every sheet already has, data types,
links between sheets, stages and example rows. It ends with a complete example document.

## 5. Add views

After `steps.apply`, add views with `view.create`, using the new `sheetId` values from its result:

- a board: `type: "KANBAN"` with `groupBy` set to a Status or Select field;
- a calendar or timeline: `type: "CALENDAR"`, `"GANTT_CHART"` or `"TIME_LINE"` with `dateField` set to a
  Date field.

## 6. Report

List what was created (the space, each sheet, views, any form, dashboard or page) with links. Report each
workflow error and each inbound email address the result returns, in plain words. Suggest one next step,
such as adding the team or importing existing rows.

## Changing a workspace that already exists

Read the sheet with `sheet.schema` first. Add a field with `sheet.add_attribute` and change one with
`sheet.update_attribute`. Editing the choices of a Status or Select field merges: reuse an option's id to
change it in place, leave the id out to add a choice, and list ids in `removeOptionIds` to delete them.
Removing a field is reversible with `sheet.restore_attribute`.
