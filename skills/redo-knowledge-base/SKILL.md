---
name: redo-knowledge-base
description: >
  Read, update, and create pages in Redo's Notion knowledge base. Use this skill ANY time a user asks to:
  update documentation, add content to the knowledge base, edit a KB page, look up Redo product info,
  find competitive intel, check ICP scoring, update the glossary, document a process, add a changelog entry,
  or anything involving the Redo Knowledge Base in Notion. Also trigger when a user says things like
  "add this to the KB", "update the docs", "document this", "put this in Notion", "update the knowledge base",
  or references any section by name (Company & Products, GTM, CX, Systems, Finance, Reference).
  This skill knows the full page hierarchy, templates, and conventions so Claude can make consistent updates.
  This skill enforces role-based access control — always verify permissions before write operations.
---

# Redo Knowledge Base Skill

You manage Redo's Notion knowledge base. Your job is to read, update, and create pages while maintaining the architecture, templates, and conventions defined below.

## Quick Reference

- **Root page:** `2e71a6608425809781e4d193f77eb170`
- **Full page registry:** Read `references/page-registry.md` for all page IDs
- **Templates:** Read `references/templates.md` for standard page formats
- **Access control config:** Read `references/access-control.md` for user roles and permissions

---

## Access Control

**Every write operation (create, update, delete) MUST be gated by a permissions check.** Read operations are available to all authenticated users.

### Roles

| Role | Read | Create Pages | Edit Pages | Edit Structure / Move Pages | Manage Access |
|------|------|-------------|------------|----------------------------|---------------|
| **viewer** | ✅ | ❌ | ❌ | ❌ | ❌ |
| **editor** | ✅ | ✅ | ✅ | ❌ | ❌ |
| **admin** | ✅ | ✅ | ✅ | ✅ | ✅ |

### How to Identify the Current User

1. Use the Notion MCP tool `notion-get-users` with `user_id: "self"` to retrieve the current user's Notion identity (name, email, user ID).
2. Look up the user in `references/access-control.md` by matching their **Notion user ID** or **email address**.
3. If the user is **not listed** in the access control config, they default to **viewer** (read-only).

### Enforcement Rules

1. **Before any write operation**, identify the user and confirm their role permits the action.
2. **If the user lacks permission**, respond with a clear, friendly message:
   > "You have **viewer** access to the Redo Knowledge Base, which allows you to read and search content. Editing requires **editor** or **admin** access. If you need write access, please contact a KB admin."
3. **Never bypass access control.** Even if the user insists, do not perform write operations for unauthorized users. Suggest they contact an admin listed in the access control config.
4. **Log the acting user** in changelog entries (use their name from the Notion profile, not a hardcoded name).
5. **Section-level overrides:** The access control config supports optional per-section role overrides. If a user has a section-specific role, it takes precedence over their global role for pages within that section.

### Admin Responsibilities

Admins can:
- Add, remove, or change user roles by instructing Claude to update `references/access-control.md`
- Modify KB architecture (move pages between sections, rename sections)
- Perform bulk operations (mass status updates, restructuring)

---

## Architecture

```
📚 Redo Knowledge Base (Root)
├── 🏢 Company & Products ─── 3051a6608425814484caf33b2c81337a
│   ├── Company Overview
│   ├── Reverse Logistics Cloud
│   ├── Shipping Cloud
│   ├── Conversion Cloud
│   ├── Marketing Cloud
│   └── Finance Cloud
├── 🎯 GTM (Go-To-Market) ─── 3051a6608425818ead57c25aa9377117
│   ├── Lead Scoring Methodology
│   ├── Prospecting Playbooks
│   ├── Objection Handling
│   ├── Discovery Questions
│   ├── Case Studies & Proof Points
│   └── Territory Management
├── 🤝 CX (Customer Experience) ─── 3051a660842581b898fcc55b40ef163b
│   ├── Customer Onboarding
│   ├── NDR (Net Dollar Retention)
│   │   ├── Retention
│   │   └── Expansion (Cross-Sell & Upsell)
│   └── Customer Support
├── ⚙️ Systems & Data Architecture ─── 3051a660842581689341cdcea61dcf8e
│   ├── System Map
│   ├── HubSpot Architecture
│   │   ├── Pipeline Definitions
│   │   ├── Property Reference
│   │   ├── Automation & Workflows
│   │   └── Formula Library
│   ├── Snowflake Schema
│   └── Data Definitions
├── 💰 Finance & Operations ─── 3051a6608425810e8868d7799c49112d
│   ├── Financial Model Architecture
│   ├── Quota & Compensation
│   ├── Revenue Recognition & Attribution
│   ├── Month-End Close Process
│   └── Internal Products
└── 📖 Reference ─── 3051a660842581d08162c599f530f8c5
    ├── Glossary
    ├── Team Directory
    └── Changelog
```

## Workflow: Identifying the User

**Run this at the start of every conversation that may involve write operations.**

1. Call `notion-get-users` with `user_id: "self"` to get the current Notion user.
2. Match against `references/access-control.md` by Notion user ID or email.
3. Cache the user's identity and role for the session.
4. If the user is not in the config, inform them they have read-only access.

## Workflow: Updating an Existing Page

1. **Check permissions.** Confirm the user has `editor` or `admin` role (globally or for the target section).
2. **Identify the target page.** Match the user's request to a section and page in the architecture above.
3. **Fetch the section page** using `notion-fetch` with the section's page ID to discover child page IDs (they appear as `<page url="...">` in the content).
4. **Fetch the target page** to read its current content.
5. **Make the edit** using `notion-update-page`:
   - `replace_content_range` — to swap out a specific section. Use `selection_with_ellipsis` with first ~10 chars + `...` + last ~10 chars of the block to replace.
   - `insert_content_after` — to add new content after an existing section.
   - `replace_content` — only for full page rewrites (rare).
   - `update_properties` — to change the page title.
6. **Update metadata.** Always set `**Last Updated:** [today's date YYYY-MM-DD]` and update `**Status:**` if appropriate.
7. **Log significant changes** by adding an entry to the Changelog page in Reference. Use the acting user's name (from Notion profile), not a hardcoded name.

## Workflow: Creating a New Page

1. **Check permissions.** Confirm the user has `editor` or `admin` role.
2. **Determine the correct parent section** from the architecture.
3. **Read `references/templates.md`** and use the appropriate template:
   - Product cloud pages → Product Cloud Template
   - Everything else → Generic Page Template
4. **Create the page** using `notion-create-pages` with:
   - `parent.page_id` set to the section page ID (or a sub-section page ID for nested pages like HubSpot Architecture children)
   - `properties.title` set to the page name
   - `content` using the template with metadata header filled in
5. **Set the Owner** in the metadata header to the acting user's name.
6. **Update the section index page** if the section page has a list or description of its children — add a mention of the new page.
7. **Log the creation** in the Changelog with the acting user's name.

## Workflow: Reading / Answering Questions

1. **No permission check needed** — all users can read.
2. **Identify which page(s) contain the answer.** Use the architecture above.
3. **Fetch the page(s)** using `notion-fetch`.
4. **Answer from the content.** If the page has TODO placeholders or is marked Draft/Stale, tell the user and offer to help populate it (if they have write access).

## Workflow: Managing Access (Admin Only)

1. **Confirm the requesting user is an admin.**
2. To add a user: Look up their Notion user ID via `notion-get-users` (search by name/email), then add them to `references/access-control.md`.
3. To change a role: Update the user's entry in the config.
4. To remove a user: Remove their entry (they'll revert to default viewer access).

## Rules

1. **Always check permissions before write operations.** This is non-negotiable.
2. **Always use metadata headers.** Every page starts with Owner, Last Updated, Status, Related Pages.
3. **Always update Last Updated** when editing a page. Use today's date in YYYY-MM-DD format.
4. **Use the acting user's name** in Owner fields and changelog entries — never hardcode a specific person's name.
5. **Product cloud pages use the Product Cloud Template** with five sections: Features, Competitive Positioning, ICP Definitions & Scoring, Target Segments, Disqualification Criteria.
6. **All other pages use the Generic Page Template** with: Overview, Content, Key Takeaways.
7. **Never create pages at root level.** Always nest under the appropriate section.
8. **Resolve page IDs dynamically.** The page registry has known IDs for section pages. For child pages, fetch the section page and extract `<page url="...">` tags to find child IDs.
9. **Preserve existing content.** When adding to a page, use `insert_content_after` or surgical `replace_content_range`. Never blow away content unintentionally.
10. **Use Notion Markdown.** Fetch the spec at `notion://docs/enhanced-markdown-spec` if unsure about formatting syntax.
11. **Status transitions:** Draft → Active (when content is complete and verified). Active → Stale (when content may be outdated). Any → Draft (when being reworked).
12. **Cross-reference related pages** in the `Related Pages` metadata field and inline where helpful.

## Product Clouds Reference

Redo's products are organized into five clouds:

| Cloud | Products | Section |
|-------|----------|---------|
| Reverse Logistics | Returns, Warranties | Company & Products |
| Shipping | Order Tracking, Package Protection | Company & Products |
| Conversion | Checkout Optimization, OMS | Company & Products |
| Marketing | Email/SMS | Company & Products |
| Finance | (internal financial tools) | Company & Products |

## Edge Cases

- **If the user asks to update something not in the KB:** Create the page in the most logical section, using the appropriate template (if they have write access).
- **If a page has TODO placeholders:** Replace them with real content. Update status from Draft to Active if the page is now complete.
- **If the user provides info conversationally** ("our main competitor for Returns is Loop"): Extract the structured info and update the relevant page (e.g., Reverse Logistics Cloud > Competitive Positioning section) — after confirming write access.
- **If the page registry doesn't have an ID:** Fetch the parent section page to discover child page IDs dynamically. This is expected — not all child IDs are pre-registered.
- **If a user without write access asks to edit:** Politely explain their access level and suggest they contact a KB admin. Offer to help them read or search instead.
- **If you cannot determine the user's identity:** Default to viewer (read-only) and inform the user that their identity could not be verified for write access.
