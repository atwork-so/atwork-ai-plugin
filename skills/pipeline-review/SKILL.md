---
name: pipeline-review
description: Review a pipeline or any stage-based sheet in Atwork, such as deals, candidates, requests or content, with totals by stage, what moved, what is stuck and what needs action. Use when the user asks how the pipeline looks, for a forecast, deals by stage, a funnel, or what is stuck.
---

# Pipeline review in Atwork

Work from aggregates, not from paging through every row, and end with concrete next actions.

## 1. Find the sheet and its fields

1. Resolve the sheet the user names with `workspace.search`.
2. Read it once with `sheet.read` and `limit: 5` to learn the field names: the stage or status, the value
   or amount, the owner, and the dates (close date, due date, created).
3. Note which stages are closed (won, lost, done, hired, rejected). "Open pipeline" means every other
   stage.

## 2. Get the numbers

Use `sheet.aggregate`:

- COUNT and SUM of the value field, grouped by stage.
- The same grouped by owner, for the open stages only.
- For a trend, group a date column with `timeBucket: "month"` (or `"week"`).

Filter values may be the names you see in `sheet.read`, for example a stage NIN ["Won", "Lost"].

## 3. Find what moved and what is stuck

With `sheet.read`, using `fields` to keep the rows small:

- **Moved:** rows sorted by `ModifiedAt` descending, changed in the period.
- **Stuck:** open rows not changed in 14 days or more.
- **Slipped:** open rows whose close or due date has passed.

## 4. Write the review

```
<Sheet> pipeline, <date>

Stage        Count   Value
Lead            12   48,000
Proposal         5   61,500
...
Open total      21   134,000

What moved: <2-3 lines>
Stuck: <row title> (<owner>, no change since <date>)
Slipped: <row title> (<owner>, was due <date>)
Next actions:
- <owner>: <one action on a named row>
```

- Show currency and units the way the sheet shows them. Don't compute a weighted forecast unless the
  sheet has a probability field.
- Name rows by their title so the user can find them.
- Don't guess why something is stuck. Say what the rows and comments show.

## 5. Follow up only when asked

If the user wants the next actions tracked, create them as tasks (`task.create`, or `item.create_many` for
several). If they want the review kept, save it with `page.create`. Ask first either way.
