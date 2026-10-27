# Rules for Atwork Steps documents

## Sheet type

- `"TASK"` when each row is work someone does: tasks, tickets, requests, deliverables, checklist items.
  Task rows appear in the assignee's My Tasks and agenda.
- `"GENERAL"` for things that are tracked rather than done: contacts, companies, deals, assets, vendors,
  notes.

## Fields every sheet already has

Never add a field whose meaning matches one of these; a second, duplicate column appears next to the
built-in one.

- Every sheet: `Title` (the row's name), `Description` (long text), `Tags`, and an `Owner`.
- Task sheets also: `Assignee`, `Reporter`, `DueDate`, `Status` (To do, In progress, Done), `Priority`
  (Urgent, High, Medium, Low), `Type`, `Timer` and `Goal`.

The default Status choices on a task sheet can't be changed while creating it. For custom stages, use a
GENERAL sheet with its own Status field (for example `Stage`), or change the choices after it exists.

Don't use these as field codes: `owner`, `assignee`, `createdBy`, `tags`, `index`, `title`, `status`,
`description`. Prefer specific codes such as `lead`, `contact`, `stage`, `notes_internal`.

## Field definitions

Each field is `{ "ref", "code", "name", "dataType", "icon" }` plus `options` or `description` when needed.

- `dataType` must be one of these exact names: Short Text, Long Text, Number, Date, DateRange, Email,
  URL, PhoneNumber, CheckBox, Status, Select, Multi-Select, Employee, Employee List, Team, Team List,
  Job, Job List, Actor, File List, Document, Location, Address, Duration, Link, Vote, Goal, Code,
  Relation, Formula, Mirror, Rollup, Rate, Timer, Pipeline, Summary, DocX, Button.
- Money is a Number; put the currency in the field's `description`.
- Give every field an `icon`, a Bootstrap Icons class such as `bi-calendar-event`, `bi-globe` or
  `bi-flag`. A field without one shows a blank header icon.
- Set `isUnique: true` on a field that identifies a row, such as an email address or an external id.

## Links between sheets

- Use `dataType: "Relation"` with `options.relatedSheet: { "$ref": "<sheet ref>" }`.
- To link to a sheet that already exists, use `{ "$existing": { "sheetId": <id> } }`, with the id from
  `workspace.structure`.
- `Link` is a plain web address field, not a link between sheets.

## Stages

A Status field lists its choices in `options.items`. Give every choice a `bucket`: `"Todo"`,
`"InProgress"` or `"Complete"`, so finished choices count as finished. Select and Multi-Select choices
take no `bucket`.

## Placement

- A sheet without a `folder` goes to the top of the space.
- Set a sheet's `folder` only to a ref declared in `folders`, never to a space ref.
- Don't add `version` or `capabilities` keys; any top-level key other than `spaces`, `folders`, `sheets`,
  `forms`, `dashboards`, `pages`, `spaceHome`, `seedRows` and `workflows` is an error.

## Example rows

- `seedRows` maps a sheet ref to 3 to 5 realistic rows, keyed by field code. Set the row name with
  `Title`.
- Status and Select cells hold the choice's title.
- On a task sheet, don't seed the built-in Status, Priority or Type fields.
- Realistic names and dates, never "Task 1" or "Sample item".

## A short guide page

Add `pages: [{ "ref": "p.guide", "name": "How to use this", "markdown": "..." }]` with a few lines on what
each sheet is for and the first thing to do.

## Complete example

```json
{
  "spaces": [{ "ref": "space", "name": "Sales", "icon": "bi-graph-up" }],
  "sheets": [
    {
      "ref": "companies",
      "name": "Companies",
      "type": "GENERAL",
      "attributes": [
        { "ref": "website", "code": "website", "name": "Website", "dataType": "URL", "icon": "bi-globe" },
        {
          "ref": "industry", "code": "industry", "name": "Industry", "dataType": "Select", "icon": "bi-tag",
          "options": { "items": [{ "title": "Software" }, { "title": "Retail" }, { "title": "Health" }] }
        }
      ]
    },
    {
      "ref": "deals",
      "name": "Deals",
      "type": "GENERAL",
      "attributes": [
        {
          "ref": "company", "code": "company", "name": "Company", "dataType": "Relation", "icon": "bi-building",
          "options": { "relatedSheet": { "$ref": "companies" } }
        },
        {
          "ref": "value", "code": "value", "name": "Value", "dataType": "Number", "icon": "bi-cash-coin",
          "description": "Deal value in USD"
        },
        {
          "ref": "stage", "code": "stage", "name": "Stage", "dataType": "Status", "icon": "bi-flag",
          "options": {
            "items": [
              { "title": "Lead", "bucket": "Todo" },
              { "title": "Proposal", "bucket": "InProgress" },
              { "title": "Won", "bucket": "Complete" },
              { "title": "Lost", "bucket": "Complete" }
            ]
          }
        },
        { "ref": "close", "code": "close_date", "name": "Close date", "dataType": "Date", "icon": "bi-calendar-event" }
      ]
    }
  ],
  "pages": [
    {
      "ref": "p.guide",
      "name": "How to use this",
      "markdown": "# Sales\n\nAdd each company once in **Companies**, then log every opportunity in **Deals** and move it through the stages."
    }
  ],
  "seedRows": {
    "companies": [
      { "Title": "Northwind Traders", "website": "https://northwind.example", "industry": "Retail" },
      { "Title": "Brightline Health", "website": "https://brightline.example", "industry": "Health" }
    ],
    "deals": [
      { "Title": "Northwind annual renewal", "value": 12000, "stage": "Proposal", "close_date": "2026-12-15" },
      { "Title": "Brightline pilot", "value": 4500, "stage": "Lead", "close_date": "2027-01-20" }
    ]
  }
}
```
