# ReliefBridge — Component Inventory

## Week 1 — Lead Object & Web-to-Lead

| Component Type | API Name / Identifier | Purpose | Week |
|---|---|---|---|
| Custom Field (Lead) | `Organization_Type__c` | Categorizes lead's organization type (Nonprofit, Corporate Donor, Government Agency, Individual Major Donor) | 1 |
| Custom Field (Lead) | `Area_of_Interest__c` | Multi-select of what the lead is interested in (funding, supplies, partnership) | 1 |
| Custom Field (Lead) | `Estimated_Contribution_Range__c` | Bucketed estimate of potential contribution size | 1 |
| Custom Field (Lead) | `Lead_Source_Detail__c` | Free-text granularity beyond standard Lead Source | 1 |
| Custom Field (Lead) | `Referred_By_Partner__c` | Lookup(Account) — partner org that referred this lead (distinct from `Company`) | 1 |
| Page Layout | Lead Layout — "ReliefBridge Details" section | Surfaces the 5 custom fields above | 1 |
| Queue | `Senior_Coordinators` | Routes high-value leads (Gov Agency or $100K+) | 1 |
| Queue | `General_Partnership_Pool` | Routes all other leads | 1 |
| Assignment Rule | `ReliefBridge_Lead_Assignment` | Routes new Leads to the two queues above based on org type/contribution range | 1 |
| Duplicate/Matching Rule | `Lead_Duplicate_Rule` (Email exact match, Company fuzzy match) | Configured as **Allow + Report** (not Block) on Create/Edit — preserves every Web-to-Lead submission while auto-flagging matches via a Duplicate Record Set/Item for manual review. See Week 1 doc for rationale. | 1 |
| Letterhead | ReliefBridge branded letterhead | Header banner (logo + tagline) and footer (contact info, social icons) used by both Lead email templates below | 1 |
| Email Template (HTML, using Letterhead) | Lead Assignment Notification | Sent to Queue members on assignment; includes Lead Name, Organization Type, Area of Interest, Estimated Contribution Range | 1 |
| Email Template (HTML, using Letterhead) | Lead Auto-Response | Sent to the Lead on Web-to-Lead submission; confirms receipt, sets 24-business-hour follow-up expectation | 1 |
| Static Resource / Document | ReliefBridge logo, header banner, footer banner | Branding images referenced by the Letterhead; retrieved separately from object metadata (`StaticResource`/`Document` metadata types) | 1 |
| Web-to-Lead Form | Public lead capture form | External-facing intake, feeds the Assignment Rule | 1 |
| Lightning App | `ReliefBridge` | Custom navigation app; Lead tab added (Home, Reports, Dashboards); to expand weekly | 1 |
| Profile | `Partnership Rep` | Cloned from Standard User; individual seller access — CRUD, FLS, and special permissions as documented in Week 1 doc | 1 |
| Profile | `Partnership Manager` | Cloned from Standard User; team oversight access — broader CRUD, FLS, and special permissions as documented in Week 1 doc | 1 |
| Known Issue (documented, resolved) | Web-to-Lead queue notification delay | Salesforce documented platform limitation (KB 000390823); investigated and confirmed now delivering correctly — see Week 1 doc | 1 |
| Documentation | `ReliefBridge_Week1_Documentation.md`, `ReliefBridge_Week1_Evidence.docx`, `ReliefBridge_UseCase_LeadIntake.md` | Build log, screenshot evidence, and standalone case study for Week 1 | 1 |

---

## Week 2 — Accounts & Contacts
*(append here once built)*

## Week 3 — Opportunities, Products & Price Books
*(append here once built)*

## Week 4 — Campaigns, Security & Forecasting
*(append here once built)*

## Weeks 5–7 — Flow Automation
*(append here once built)*

## Weeks 8–12 — Apex
*(append here once built)*

## Weeks 13–16 — LWC
*(append here once built)*

## Weeks 17–19 — Integrations
*(append here once built)*

## Weeks 20–22 — Experience Cloud
*(append here once built)*

## Weeks 23–25 — Agentforce
*(append here once built)*
