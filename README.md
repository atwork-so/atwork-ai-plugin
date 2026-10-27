# Atwork

Work with your [Atwork](https://atwork.so) sheets, tasks and pages from Claude. The plugin connects
Claude to your Atwork workspace and teaches it a few workflows that teams run every week: writing a
status update, planning a project, reviewing a pipeline, and building a new workspace from a short
description.

## What's inside

**Connector.** The plugin points at Atwork's MCP server, `https://mcp.atwork.so/mcp`. Connect it from the
plugin's Connectors tab and sign in with your Atwork account. Claude then acts as you and sees only what
you can see in Atwork. If you already added the Atwork connector from the directory, it is the same
server, so you get one set of tools.

**Skills**

| Skill | Ask for example |
| --- | --- |
| `status-update` | "Write this week's status update for the Harbor Dental project." |
| `plan-project` | "Plan the website launch for 15 December and add the tasks to the Launch sheet." |
| `pipeline-review` | "How does the deals pipeline look? What's stuck?" |
| `build-workspace` | "Set up a hiring tracker with candidates, roles and interview stages." |

**Commands** (Cowork and Claude Code)

- `/atwork:status <client or project>`: a status update, ready to send.
- `/atwork:today`: your day planned from your Atwork tasks and calendar.

Claude drafts plans and designs in the conversation first and creates things in Atwork only after you
agree. Rows created in one step can be removed in one step.

## Install

- **Claude (web, desktop, Cowork):** Customize > Plugins > Discover, search for Atwork, then connect the
  Atwork connector from the plugin's Connectors tab.
- **Claude Code:** run `/plugin directory` and choose Atwork. A plugin added on claude.ai also appears in
  Claude Code.

## Requirements

An Atwork account. Atwork has a free plan at [atwork.so](https://atwork.so). The setup guide for the
connector is at [atwork.so/mcp](https://atwork.so/mcp).

## Data

The plugin itself is instructions only: it runs no code and stores nothing. When you use it, Claude
calls the Atwork MCP server at `mcp.atwork.so` with your OAuth sign-in to read and change data in your
own Atwork workspace. Nothing is sent anywhere else. Atwork's
[privacy policy](https://atwork.so/privacy) covers the data in your workspace.

## Other AI tools

The skills follow the open [Agent Skills](https://agentskills.io) format, and the connector is a standard
MCP server, so both work with other assistants that support them.

## Support

Email [support@atwork.so](mailto:support@atwork.so) or visit [atwork.so/contact](https://atwork.so/contact).
