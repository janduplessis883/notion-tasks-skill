---
name: notion-context-skill
description: Search Notion or query a task database for unfinished items, then pull page content into the conversation for summarization and prioritization.
metadata:
  require-secret: true
  require-secret-description: Paste either a Notion token, or a JSON secret with token and database_id so the database stays private.
---

# Notion Context Skill

## Purpose

Use this skill when the user wants to search, read, summarize, prioritize, or answer questions using content from Notion.

## Instructions

Call the `run_js` tool using `index.html` and a JSON string for `data` with the following fields:

- `query`: Optional string. A short page title or keyword search for Notion content.
- `page_id`: Optional string. A Notion page ID or a full Notion page URL. Use this when the user gives a specific page.
- `database_id`: Optional string. A Notion database ID or database URL. Use this for task lists and other structured tables.
- `data_source_id`: Optional string. Use this instead of `database_id` if the user explicitly gives a data source ID.
- `max_results`: Optional integer from 1 to 5. Default to 3.
- `max_pages`: Optional integer from 1 to 100. Default to 50 when querying a task database.
- `include_content`: Optional boolean. Default to `true`. Set to `false` only if the user wants a quick list of matches.
- `include_properties`: Optional boolean. Default to `true`.
- `status_property`: Optional string. Default to `Status`.
- `done_value`: Optional string. Default to `Done`.
- `due_property`: Optional string. Default to `Date`.
- `priority_property`: Optional string. Default to `Priority`.

## Secret Format

The skill accepts either of these secret formats:

- Raw token only: `secret_xxx`
- JSON for private setup:

```json
{
  "token": "secret_xxx",
  "database_id": "your-private-database-id"
}
```

You can also provide `data_source_id` in the JSON secret if you prefer.

## Selection Rules

- If the user asks for unfinished tasks from a Notion database, pass `database_id` or `data_source_id`.
- For task lists, set `status_property` to the exact property name if the user provides it.
- For task lists, set `done_value` to the exact completed status name if the user provides it.
- For task lists, set `due_property` and `priority_property` to the exact column names if the user provides them.
- If the user provides a specific Notion page URL or page ID, pass it in `page_id`.
- Otherwise, pass the most important search phrase in `query`.
- Keep `query` short and literal. Do not include conversational filler.
- If the user asks for one page, prefer `max_results: 1`.
- For task summaries, prefer `include_properties: true` and use `include_content: true` when page notes might help explain the task.

## Response Rules

- Use only the information returned by the tool.
- Mention when no matching page was found.
- If multiple pages are returned, briefly identify the best match before summarizing.
- For task databases, start with overdue and due-soon items.
- For task databases, use `Priority` together with due date to rank urgency.
- For task databases, always highlight exactly 3 quick tasks the user can finish next, based on title scope, due date, priority, and any available notes or properties.
- Keep the answer concise unless the user asks for a deeper summary.
