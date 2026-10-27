---
name: plan-project
description: Turn a goal, brief or meeting notes into a planned set of tasks in Atwork, with owners, due dates and dependencies. Use when the user asks to plan a project, break work down, set up a launch or onboarding plan, or add a list of tasks to Atwork.
---

# Plan a project in Atwork

Draft the plan in the conversation first, create it in Atwork only after the user agrees, and create it
in one call.

## 1. Understand the work

Read the goal, brief or notes the user gives. You need a deadline and the people involved. If either is
missing, ask once, with at most two questions; otherwise go ahead with sensible defaults and say what you
assumed.

## 2. Choose where the tasks go

1. Find the project's task sheet with `workspace.search` or `workspace.structure`.
2. Read it with `sheet.schema` to learn the field names and the Status and Priority choices.
3. If there is no fitting sheet, offer two options: build one (see the build-workspace skill), or put the
   tasks in the user's default task sheet with `task.create`, which needs no `sheetId`.

## 3. Draft the plan

- Group the work into a few phases, then tasks. Each task title starts with a verb and names one result
  ("Draft the launch email", not "Email").
- Give every task an owner, a due date worked back from the deadline, and a priority when the sheet has
  one. Leave buffer before the deadline.
- Note dependencies: which task can't start until another is done.
- Keep it to what the user asked for, usually 5 to 30 tasks.

Show the plan as a table (phase, task, owner, due date, depends on) and ask whether to create it. Don't
create anything before the user agrees.

## 4. Check the owners

Owner names must match workspace members. Check them with `member.read` and `nameStartsWith`. If a name
matches nobody or several people, ask which person is meant.

## 5. Create the tasks

- Call `item.create_many` once with the `sheetId` and one row per task. Each row is a `fields` map using
  the names from `sheet.schema`, for example `Title`, `Assignee`, `DueDate`, `Status`, `Priority`. Up to
  200 rows per call.
- For dependencies, turn on the Blocking and Blocked By fields with `sheet.set_dependencies`, then set
  `Blocked By` on each dependent task with `item.update_humanized`, naming the blocking task by its title
  or `#Index`.

## 6. Report

Say how many tasks were created and list any that were skipped with the reason the tool gave. Give the
link to the sheet. Mention that `item.undo_batch` with the returned `batchId` removes the whole batch if
the plan needs to start over.
