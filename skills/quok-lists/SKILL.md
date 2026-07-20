---
name: quok-lists
description: Manage Quok family lists, tasks, groceries, shopping, and packing through Quok MCP tools. Use when the user asks to find, list, create, rename, update, assign, move, complete, uncheck, or delete a Quok list or list item, or refers to the family or household tasks and shopping.
---

# Quok Lists

Use Quok as the source of truth for shared family lists. Keep household tasks, groceries, shopping, and packing in Quok rather than a personal notes or task connector.

## Connect

1. Confirm that the Quok MCP tools are available.
2. Call `server_info` before the first list operation when the connected family, member, or environment is unclear.
3. If the tools are unavailable, tell the user to connect `https://api.getquok.com/mcp` and stop. Never substitute another list service.

## Choose the tool

- Use `list_family_tasks` for the family's combined to-do view.
- Use `list_family_shopping` for combined shopping and groceries.
- Use `list_family_lists` to find list containers, optionally by type.
- Use `list_family_items` for the contents of one known list.
- Use `get_family_item` to inspect one known item and obtain its current version.
- Use `search_family` when the user gives a keyword, list name, item text, or member name but no id.
- Use `add_family_task` for to-do items and `add_family_shopping_item` for shopping or grocery items.
- Use `update_family_list_item` to edit, assign, or move an item.
- Use `complete_family_list_item` to check or uncheck an item.
- Use `delete_family_list_item` only for permanent item deletion.
- Use `create_family_list`, `rename_family_list`, and `delete_family_list` for list containers.

Treat the tool's live input schema as authoritative. Do not invent fields or rely on a copied schema.

## Read safely

- Prefer the aggregate task or shopping tools when the user asks what remains across the family.
- Resolve a named list before listing or adding to it. Omit `listId` only when the family default is clearly intended.
- Search first when item text could match multiple entries. Ask the user to choose only when the results remain ambiguous.
- Fetch the current item or list before changing or deleting it unless the current result already contains its id, details, and version.
- Use member search results to resolve assignees. Never guess member ids.

## Write safely

- Keep quantity and unit on shopping or grocery items, not tasks.
- Preserve item text, checked state, assignees, quantity, and unit unless the user asks to change them.
- Pass the latest version as `expectedVersion` for updates and deletes when available.
- Prefer `complete_family_list_item` for check and uncheck requests because it states the intended final state.
- Treat an explicit request to delete one identified item as confirmation. Otherwise identify the exact item and obtain confirmation before deleting it.
- Before deleting a list, resolve it and inspect its contents with `list_family_items`. Do not delete the list if its contents cannot be inspected.
- If the list contains items, warn that `delete_family_list` permanently removes them and obtain confirmation unless the user already explicitly acknowledged deleting the list and all its items. If the list is empty, treat an explicit request to delete the identified list as confirmation.
- Use the entity returned by a successful write. Do not repeat a write or perform a lookup solely to rediscover its id.
- Report the affected list or item briefly. Surface tool errors instead of claiming success.

## Combine with the calendar

When a request also creates or changes a family event, complete the list part with this skill and use `quok-calendar` for the calendar part. Keep the writes independently understandable if either one fails.
