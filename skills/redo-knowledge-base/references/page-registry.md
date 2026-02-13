# Redo Knowledge Base — Page Registry

This file maps every page in the knowledge base to its Notion page ID. Use these IDs with the Notion MCP tools.

## Root

| Page | Notion Page ID |
|------|---------------|
| 📚 Redo Knowledge Base (Root) | `2e71a6608425809781e4d193f77eb170` |

## 🏢 Company & Products

| Page | Notion Page ID | Parent ID |
|------|---------------|-----------|
| 🏢 Company & Products (section) | `3051a6608425814484caf33b2c81337a` | Root |
| Company Overview | `3051a660842581639c35e1e5bf16c94f` | Company & Products |
| Reverse Logistics Cloud | `3051a660842581ba90aee415fecf89a3` | Company & Products |
| Shipping Cloud | `3051a6608425818ca60af2803f6e3b52` | Company & Products |
| Conversion Cloud | `3051a660842581898474f941f73f38ba` | Company & Products |
| Marketing Cloud | `3051a660842581ee8f13c306fe19b9fc` | Company & Products |
| Finance Cloud | `3051a660842581778ed4c240d6264733` | Company & Products |

## 🎯 GTM (Go-To-Market)

| Page | Notion Page ID | Parent ID |
|------|---------------|-----------|
| 🎯 GTM (section) | `3051a6608425818ead57c25aa9377117` | Root |
| Lead Scoring Methodology | (fetch from section page) | GTM |
| Prospecting Playbooks | (fetch from section page) | GTM |
| Objection Handling | (fetch from section page) | GTM |
| Discovery Questions | (fetch from section page) | GTM |
| Case Studies & Proof Points | (fetch from section page) | GTM |
| Territory Management | (fetch from section page) | GTM |

## 🤝 CX (Customer Experience)

| Page | Notion Page ID | Parent ID |
|------|---------------|-----------|
| 🤝 CX (section) | `3051a660842581b898fcc55b40ef163b` | Root |
| Customer Onboarding | (fetch from section page) | CX |
| NDR (Net Dollar Retention) | (fetch from section page) | CX |
| Customer Support | (fetch from section page) | CX |

## ⚙️ Systems & Data Architecture

| Page | Notion Page ID | Parent ID |
|------|---------------|-----------|
| ⚙️ Systems & Data Architecture (section) | `3051a660842581689341cdcea61dcf8e` | Root |
| System Map | (fetch from section page) | Systems |
| HubSpot Architecture | (fetch from section page) | Systems |
| Snowflake Schema | (fetch from section page) | Systems |
| Data Definitions | (fetch from section page) | Systems |

## 💰 Finance & Operations

| Page | Notion Page ID | Parent ID |
|------|---------------|-----------|
| 💰 Finance & Operations (section) | `3051a6608425810e8868d7799c49112d` | Root |
| Financial Model Architecture | (fetch from section page) | Finance |
| Quota & Compensation | (fetch from section page) | Finance |
| Revenue Recognition & Attribution | (fetch from section page) | Finance |
| Month-End Close Process | (fetch from section page) | Finance |
| Internal Products | (fetch from section page) | Finance |

## 📖 Reference

| Page | Notion Page ID | Parent ID |
|------|---------------|-----------|
| 📖 Reference (section) | `3051a660842581d08162c599f530f8c5` | Root |
| Glossary | (fetch from section page) | Reference |
| Team Directory | (fetch from section page) | Reference |
| Changelog | (fetch from section page) | Reference |

## Other (Legacy / Adjacent)

| Page | Notion Page ID |
|------|---------------|
| Commission Dashboard | `2e71a6608425802b9aaff1dbafb84d88` |

---

## Notes

- Page IDs marked `(fetch from section page)` need to be resolved by fetching the parent section page and reading the child `<page>` URLs. This is because they were created during the initial build and the IDs were not captured in the registry.
- When Claude fetches a section page, it should extract and cache child page IDs for that session.
- All page IDs work with Notion MCP tools: `notion-fetch`, `notion-update-page`, `notion-create-pages`, `notion-move-pages`.
