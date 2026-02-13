# Redo Knowledge Base — Access Control

This file defines who can read, edit, and administer the Redo Knowledge Base. Claude checks this file before performing any write operations.

## How It Works

1. Claude identifies the current user via `notion-get-users` with `user_id: "self"`.
2. Claude matches the user against this file by **Notion User ID** or **email**.
3. The user's role determines what operations they can perform.
4. Users not listed here default to **viewer** (read-only).

## Roles

| Role | Read | Create | Edit | Restructure | Manage Access |
|------|------|--------|------|-------------|---------------|
| viewer | ✅ | ❌ | ❌ | ❌ | ❌ |
| editor | ✅ | ✅ | ✅ | ❌ | ❌ |
| admin | ✅ | ✅ | ✅ | ✅ | ✅ |

## User Registry

<!-- 
  To add a user:
  1. Get their Notion user ID via notion-get-users (search by name or email)
  2. Add a row to the table below
  3. Only admins can modify this file
-->

| Name | Email | Notion User ID | Global Role | Section Overrides |
|------|-------|---------------|-------------|-------------------|
| Brad Wilcox | brad.wilcox@getredo.com | 20dd872b-594c-811e-b795-00024affb4da | admin | Finance:viewer |
| Eric Lepretre | eric@getredo.com | a68a6df9-c06a-4c5e-a517-55f0ac97f752 | admin | — |
| Ridge Durrant | ridge@getredo.com | 8c032c04-962e-4b3d-82f8-81dfd7fcf524 | admin | — |
| Kash Johnson | kash@getredo.com | 3fe860cb-bd5a-4d18-9e84-b3d1c2e9d743 | admin | Finance:editor |
| Bailey Burke | bailey@getredo.com | 268d872b-594c-81ca-bfa6-00021e226916 | admin | Finance:viewer |
| Gabe Miller | gabe.miller@getredo.com | 25cd872b-594c-8192-b917-0002bdb14a1a | admin | Finance:viewer |
| Ryan Merrill | ryan.merrill@getredo.com | 2d6d872b-594c-811e-a929-000230e70f32 | admin | Finance:editor |
| Jackie Sullivan | jackie@redo.com | 18789492-53aa-486c-8c2c-b92294891a1d | admin | — |
| Jason Stewart | jason.stewart@redo.com | 2e6d872b-594c-81f0-915d-0002f134d3f5 | admin | Finance:viewer |
| Ainsley McLaughlin | ainsley@redo.com | 2fdd872b-594c-817d-8dab-00024a330044 | admin | Finance:viewer |
| Josh Thayne | josh.thayne@redo.com | 138d872b-594c-8139-9f35-00021caf5708 | admin | Finance:viewer |
| Haden Loveridge | haden@redo.com | 2f6d872b-594c-819a-9922-0002bfe31193 | admin | Finance:viewer |
| Spencer Evans | spencer.evans@redo.com | 2fdd872b-594c-815f-a921-000294c09008 | admin | — |
| Luke Glissmeyer | luke@getredo.com | 211d872b-594c-8101-a875-0002333354d2 | admin | — |
| Max Miner | max@redo.com | 2e7d872b-594c-811f-90cd-00024bab130e | admin | Finance:editor |
| Jasmine Jess | jasmine@redo.com | 303d872b-594c-8191-84b7-000286b3be96 | admin | Finance:viewer |

## Section-Level Overrides

Section overrides let you give a user a different role for specific sections. This is useful for subject-matter experts who should be able to edit their own section but not others.

**Format:** In the "Section Overrides" column, use `section_name:role` separated by commas.

**Examples:**
- `GTM:editor` — gives a viewer edit access to the GTM section only
- `CX:editor, Systems:editor` — edit access to CX and Systems sections
- `—` or empty — no overrides, use global role everywhere

**Section names** (use these exact strings in overrides):
- `Company & Products`
- `GTM`
- `CX`
- `Systems`
- `Finance`
- `Reference`

## Default Behavior

- **Unlisted users** → viewer (read-only)
- **Unrecognized Notion identity** → viewer (read-only) with a message suggesting they contact an admin
- **Section override** takes precedence over global role for pages within that section
- **Explicit section overrides take precedence** over global role, even for admins. This allows restricting sensitive sections (e.g., `Finance:viewer` on an admin limits them to read-only for Finance pages)

## Modification History

| Date | Change | Changed By |
|------|--------|-----------|
| 2026-02-12 | Initial access control setup | Brad Wilcox |
| 2026-02-12 | Added full Finance team (16 members) as admins | Brad Wilcox |
| 2026-02-12 | Restricted Finance section edits to Ridge Durrant, Jackie Sullivan, Luke Glissmeyer, Spencer Evans only | Brad Wilcox |
| 2026-02-12 | Upgraded Max Miner, Kash Johnson, Ryan Merrill to Finance:editor | Brad Wilcox |
| 2026-02-12 | Restored Eric Lepretre to full admin (removed Finance:viewer restriction) | Brad Wilcox |
