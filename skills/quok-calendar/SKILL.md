---
name: quok-calendar
description: Manage the shared Quok family calendar through Quok MCP tools. Use when the user asks to find, list, inspect, create, update, confirm, propose, reschedule, assign, or delete Quok family events, or refers to the family, household, or our calendar.
---

# Quok Calendar

Use Quok as the source of truth for the family's shared household calendar. Do not route Quok, family, household, or "our calendar" requests to Google Calendar or another personal calendar connector.

## Connect

1. Confirm that the Quok MCP tools are available.
2. Call `server_info` before the first calendar operation when the connected family, member, or environment is unclear.
3. If the tools are unavailable, tell the user to connect `https://api.getquok.com/mcp` and stop. Never substitute another calendar.

## Choose the tool

- Use `list_family_events` for events within a known time window.
- Use `search_family` with the event type filter when the user gives a keyword but no event id.
- Use `get_family_event` to inspect one known event and obtain its current version.
- Use `create_family_event` to add an event.
- Use `update_family_event` to change an event or transition its status.
- Use `delete_family_event` only for permanent deletion.

Treat the tool's live input schema as authoritative. Do not invent fields or rely on a copied schema.

## Read safely

- Translate relative dates using the current date and the user's locale and timezone.
- Send explicit ISO 8601 offsets in event-list windows. Treat `from` as inclusive and `to` as exclusive.
- Search first when a title could match multiple events. Ask the user to choose only when the results remain ambiguous.
- Fetch the event before updating or deleting it unless the current result already contains its id, full details, and version.
- Respect the visibility and permissions enforced by Quok. Do not infer or expose inaccessible private events.

## Write safely

- Preserve every relevant detail the user supplied, including event type, all-day intent, timezone, recurrence, visibility, and assignees.
- Ask a concise question when a missing date, time, timezone, recurrence boundary, or matching event would materially change the result.
- Default normal user-requested events to confirmed. Use proposed only when the user asks to stage something for review.
- Pass the latest version as `expectedVersion` for updates and deletes when available.
- Treat an explicit request to delete one identified event as confirmation. Otherwise identify the exact event and obtain confirmation before calling `delete_family_event`.
- Use the entity returned by a successful write. Do not repeat a write or perform a lookup solely to rediscover its id.
- Report the resulting title and time briefly. Surface tool errors instead of claiming success.

## Combine with lists

When a request also creates tasks, shopping items, or a trip list, complete the calendar part with this skill and use `quok-lists` for the list part. Keep the two writes independently understandable if either one fails.
