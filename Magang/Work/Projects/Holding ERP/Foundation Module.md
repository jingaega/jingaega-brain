---
title: Foundation Module
type: module-spec
project: "[[Holding ERP]]"
status: draft
created: 2026-10-13
tags: [internship, erp, module, foundation]
---

# Foundation Module

Part of [[Holding ERP]]. Company context is in [[Company Roles and Modules]]. This is Phase 1 in [[Pandu Roadmap]]: everything the other modules need before they can start. Even a quote needs a user, a company, a customer and an item.

## Overview and scope
- **In the ERP:** login, the 6 companies with their own color theme, the 2 super users, users, roles and permissions, which user can work in which company, and master data (items, units, customers, vendors)
- **Handed to other modules:** nothing. Every other module reads from this one
- **Which companies use it:** all 6. Pandu first, the others are seeded but switched on in Phase 9
- **UI language:** Indonesian

## Key terms
| Term | Meaning | Who makes it |
|---|---|---|
| Company | One of the 6 PTs. Every record belongs to one | Seeded at setup |
| Theme | A company's colors (and logo). The whole screen switches when you switch company | Super user |
| Super user | One of the 2 accounts that can see and manage everything in every company | Seeded at setup |
| User | A person who logs in | Super user |
| Role | A named set of permissions, like what a sales admin can do | Super user |
| Permission | One action on one record type, like "create item" or "view customer" | Fixed in the system |
| Access | A user gets a role inside a company. One user can have access to several companies | Super user |
| Item | Something we sell, buy or make | Company admin |
| UoM | Unit of measure (pcs, kg, liter, box) | Company admin |
| Customer | Who we sell to. Can be an outside company or another group company | Company admin |
| Vendor | Who we buy from. Can be an outside company or another group company | Company admin |

## Records the module owns
| Record | Main fields | Statuses | Links to |
|---|---|---|---|
| Company | Code, name, short name, is holding, theme colors, logo | Active, Inactive | |
| User | Name, email, password, is super user | Active, Inactive | |
| Role | Name, list of permissions | Active, Inactive | Permissions |
| Access | User, company, role | | User, Company, Role |
| Session | User, current company, expiry | | User |
| UoM | Code, name | Active, Inactive | Company |
| Item | Code, name, UoM, buy or make flag | Active, Inactive | Company, UoM |
| Customer | Code, name, address, contact, group company link (empty for outside customers), order type (PO first or work first), rework report needed | Active, Inactive | Company, linked group company |
| Vendor | Code, name, address, contact, group company link (empty for outside vendors) | Active, Inactive | Company, linked group company |

Notes:
- Buy or make, order type and rework report come from [[Sales and Marketing Module]]. They are stored now so S&M doesn't have to change these records later
- Floor price and credit limit are not added yet. They wait for the S&M open questions
- Nothing is ever hard deleted *(assumed)*. Records are set to Inactive so old documents still point to them

## Main flow
```mermaid
flowchart TD
  Login["Login"] --> Pick["Pick company<br/>only ones you have access to"]
  Pick --> Theme["Screen switches to<br/>that company's theme"]
  Theme --> Work["Work inside that company<br/>only what your role allows"]
  Super["Super user"] --> Users["Create users,<br/>give access per company and role"]
  Super --> Roles["Create roles,<br/>tick permissions"]
  Work --> Master["Master data<br/>items, UoM, customers, vendors"]
```

## Business rules
| Rule | How the system enforces it |
|---|---|
| Every record belongs to one company | `company_id` on every record. The API only returns records from the company picked at login |
| You only see companies you have access to | The company picker and the API both check the user's access. Super users see all |
| Your role decides what you can do | Every API action checks the permission. Hiding a button is not enough |
| Super users can do everything | Super user flag skips the permission check, in every company |
| There are exactly 2 super users | Seeded at setup. The screen does not offer "make super user" *(assumed)* |
| Each company has its own theme | The theme is loaded from the company record when you switch company |
| Nothing is hard deleted *(assumed)* | Master data and users go Inactive instead. Inactive items, customers and vendors can't be picked on new documents |
| Codes are unique inside a company *(assumed)* | Two items in Pandu can't share a code. Pandu and Anggoan can |

## Handoffs to other modules
| Module | What it does for this module | Where in the flow |
|---|---|---|
| All modules | Read users, access, companies and master data from here | Everywhere |
| S&M | Uses customers, items, and the group company link to spot intercompany orders | Quote, PO cust, SO |
| Procurement | Uses vendors and items | PR, PO |

## Placeholders until Jingga decides
These are built now so work isn't blocked, and are easy to change later. All are *(assumed)*.
- **Super users:** two anonymous accounts, "Super User 1" and "Super User 2". Real names and emails replace them later. Where they sit (Pandu or Serio) is still open
- **Colors:** one neutral default theme for all 6 companies. Colors are just fields on the company record, so changing them later is a data change, not a code change
- **Roles:** one test role ("Uji Coba", view only) for checking that permissions work. The real role list replaces it

## Pandu baseline
- What Pandu does here today: unknown. Item, customer and vendor lists probably live in Excel *(assumed)*, so an Excel import may be wanted later
- What Wening changes: items need batch and expiry later (Phase 6). Not in this module
- Records that cross into Anggoan: Anggoan is a customer of Pandu, and Pandu is a vendor of Anggoan. Both are linked with the group company link
- What the holding view excludes: nothing here. The group company link is what later lets the holding view drop intercompany sales

## Open questions
- [ ] Who are the 2 super users, and do they sit in Pandu or Serio?
- [ ] Colors (and logos) for each company
- [ ] The real list of roles, and what each one can do
- [ ] Master data per company or shared? Spec currently says per company (Anggoan has its own catalog). If per company, how does a Wening item at Pandu match the same item at Anggoan for the automatic PO to SO?
- [ ] Currency: IDR only?
- [ ] Does anyone besides the super users manage users and roles (one admin per company)?
- [ ] Do we need an Excel import for items, customers and vendors at the start?
- [ ] Where does it run, Pandu's server or cloud? (also in [[Holding ERP]])

## Acceptance checks
Written before the build. Each one is something Jingga can click through.
- [ ] Log in as Super User 1, see all 6 companies in the picker
- [ ] Switch company, the colors change, the data changes
- [ ] Create a user with the test role in Pandu only. Log in as them: only Pandu shows, and they can view but not create items
- [ ] That user can't open an Anggoan record, even by typing its URL
- [ ] Create an item, a customer and a vendor in Pandu. They don't show up in Anggoan
- [ ] Mark Anggoan as a group company customer of Pandu
- [ ] Set an item Inactive, it disappears from pick lists but stays on its detail page
- [ ] A second browser sees a new item appear without refreshing (real-time check)
- [ ] All labels and buttons are in Indonesian

## Build plan
Draft by Claude, waiting for Jingga's approval. Stack is in the decisions table in [[Holding ERP]].

- Data model: the records table above, one PostgreSQL database, `company_id` on every business record
- Screens: login, company picker, user list and form, role list and form (permission checkboxes), item, UoM, customer and vendor list and form
- API: one Go service. Login, logout, current user, switch company, then list, create, edit and deactivate for each record above
- Repo layout (`jingaega/PT-SERIO-ERP`):
  - `backend/` Go (Fiber): `cmd/api`, `migrations`, `queries`, `internal/platform` (server, auth, company scope, real-time), `internal/modules/{auth,company,access,masterdata}`
  - `frontend/` Next.js with TypeScript: `app/login`, `app/[company]/...`, shared components, API client, themes
  - `CLAUDE.md` from [[Claude Code Instructions]], `README.md`, `docker-compose.yml` for the database, `Makefile`, CI that runs tests and builds
  - Specs stay in this vault, not in the repo
- Slices (small, one record end to end):
  - [ ] 1. Scaffold: repo layout, database in Docker, health check, empty Next.js shell in Indonesian, CI, `CLAUDE.md`
  - [ ] 2. Companies: 6 seeded with the default theme, company picker switches the theme
  - [ ] 3. Login and logout with sessions, 2 placeholder super users seeded
  - [ ] 4. Roles and permissions, checked on every API call, with tests for the access rules above
  - [ ] 5. User admin: super user creates users and gives access per company and role
  - [ ] 6. Items and UoM
  - [ ] 7. Customers, with the group company link and the S&M settings
  - [ ] 8. Vendors, with the group company link
  - [ ] 9. Real-time: lists update live in a second browser
- Open questions only block their own slice. Slices 1 to 3 and 6 to 9 can start now. Slice 4 and 5 use the test role until the real list exists
