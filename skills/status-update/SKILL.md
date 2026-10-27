---
name: status-update
description: Write a status update for a client, project or team from their Atwork sheets. Use when the user asks for a weekly status, a progress report, a client update, "where are we on X", or what changed this week.
---

# Status update from Atwork

Build the update from what is in the user's Atwork workspace, never from memory. The result is a short
update the user can send as is.

## 1. Find the work

1. Resolve the client, project or team the user names with `workspace.search`. If they name a space,
   `workspace.structure` lists the sheets inside it.
2. Read each candidate sheet once with `sheet.read` and `limit: 5` to learn its field names: the status,
   the due date, the assignee, and any field that ties a row to the client or project.
3. If several sheets could hold the work and the request doesn't say which, ask one short question
   instead of guessing.

## 2. Collect the facts

Use the period the user gives. Without one, cover the last 7 days and the next 7, counted in the user's
time zone.

- **Done:** rows in a done status changed in the period. `sheet.read` with a status filter and
  `sort: [{ "field": "ModifiedAt", "direction": "desc" }]`.
- **In progress and coming up:** open rows, with their owner and due date.
- **Overdue:** open rows past their due date. On a task sheet, `task.read` with `overdue: true` and the
  `sheetId` returns them directly.
- **Totals:** `sheet.aggregate` with COUNT grouped by status, plus SUM of an amount field when the sheet
  has one. Use it instead of paging through every row.
- **Blocked:** rows whose status says blocked or waiting, and rows with a `Blocked By` link.

Filter with the values you see in `sheet.read` results (for example Status EQ "Done"). An unknown value
returns an error that lists the valid choices; correct it and retry.

## 3. Write the update

```
<Client or project>: status, <date range>

Done
- <row title> (<owner>)

In progress
- <row title>, <owner>, due <date>

Coming up
- <row title>, due <date>

Overdue or at risk
- <row title>, was due <date> (<what is known about it>)

Needs a decision
- <only items that wait on the reader>
```

- Lead with what changed. One fact per line, with owners and dates.
- Keep a section and say "Nothing overdue" rather than dropping it.
- Don't invent progress, dates or reasons that aren't in the rows or their comments.
- Match the user's language and keep it under about 20 lines unless they ask for more.

## 4. Save it only when asked

Ask before writing anything to Atwork. If the user wants the update kept, `page.create` saves it as a page
in the project's space (find the `spaceId` with `workspace.structure`).
