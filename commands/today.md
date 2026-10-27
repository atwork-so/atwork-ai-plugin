---
description: Plan your day from your Atwork tasks and calendar
---

Plan my day from Atwork:

1. Call `task.read` without `assigneeUserId` (it defaults to me), first with `overdue: true`, then with
   `state: "open"` and a due window covering today in my time zone.
2. Call `calendar.read` for today.
3. Reply with today's meetings and their times, then the overdue tasks, then today's tasks in a sensible
   order around the meetings. Flag anything that won't fit. Keep it under 15 lines.

Don't change anything in Atwork unless I ask.
