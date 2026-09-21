# ReliefBridge

**A Sales Cloud–first Salesforce portfolio project** — a disaster-relief nonprofit CRM built from scratch to learn, document, and showcase real-world Salesforce architecture across declarative configuration, Apex, LWC, integrations, Experience Cloud, and Agentforce.

**Status: In Progress** — this is a 25-week build, documented as it happens. Currently on **Week 1: Lead Intake & Assignment**. See [Current Progress](#current-progress) below for what's live.

---

## What is ReliefBridge?

ReliefBridge simulates the CRM backbone of a disaster-relief nonprofit — one that needs to onboard and manage partner organizations, corporate donors, and government agencies; track donation/grant pipelines; coordinate volunteers; and eventually route real-time relief requests. Rather than building another generic e-commerce demo, this project uses a domain with genuinely complex, defensible business logic: skill/value-based routing, multi-stakeholder deals, forecasting, and (later) AI-assisted intake.

The project is intentionally **Sales Cloud–first**: Leads, Accounts, Contacts, Opportunities, Products, and Campaigns are the backbone, with custom objects layered in only where standard objects don't fit. Service Cloud is deliberately out of scope for now.

## Why This Project Exists

Built as a structured, portfolio-grade learning path — to go deep on Salesforce configuration and development in a single cohesive system (rather than disconnected tutorials), produce something demonstrable for interviews and LinkedIn, and document real debugging/design decisions along the way, not just finished features.

## Tech & Features Covered

| Area | Status |
|---|---|
| Sales Cloud data model (Leads, Accounts, Contacts, Opportunities, Products) | 🟡 In Progress |
| Flow Automation | ⬜ Planned |
| Apex (triggers, batch, queueable, tests) | ⬜ Planned |
| Lightning Web Components | ⬜ Planned |
| External Integrations (REST callouts, Named Credentials) | ⬜ Planned |
| Experience Cloud (Partner Portal) | ⬜ Planned |
| Agentforce | ⬜ Planned |

## Repository Structure

```
reliefbridge-salesforce/
├── force-app/main/default/     # Salesforce metadata (retrieved via Salesforce CLI)
│   ├── objects/                 # Custom fields, record types, validation rules
│   ├── email/                   # Email templates
│   ├── letterhead/               # Branded letterheads
│   ├── staticresources/          # Logos and branding assets used in templates
│   ├── queues/
│   ├── profiles/
│   └── assignmentRules/
├── docs/
│   ├── ReliefBridge_WeekWise_Plan.md         # Full 25-week build plan
│   ├── ReliefBridge_Week1_Documentation.md   # Weekly build logs
│   ├── ReliefBridge_COMPONENTS.md            # Running component inventory
│   ├── ReliefBridge_UseCase_LeadIntake.md    # Standalone case studies
│   ├── *.docx                                # Polished, shareable write-ups
│   └── assets/branding/                      # Source design assets (non-metadata)
└── README.md
```

`force-app/` is a clean mirror of what's deployed in the org — nothing else lives there. `docs/` holds everything that explains, documents, or showcases the build.

## Current Progress

**Week 1 — Lead Intake & Assignment** ✅
- Custom Lead fields for org type, interest area, and contribution range
- Web-to-Lead capture form
- Criteria-based Lead Assignment Rule routing to two Queues
- Duplicate Rule (Allow + Report — see [case study](docs/ReliefBridge_UseCase_LeadIntake.md) for why)
- Branded email notifications (internal assignment alert + external auto-response)
- Two custom profiles (Partnership Rep, Partnership Manager) with tailored FLS, CRUD, and permissions
- Custom `ReliefBridge` Lightning App

📄 Full write-up: [Lead Intake Case Study](docs/ReliefBridge_UseCase_LeadIntake.md)
📋 Full plan: [Week-Wise Build Plan](docs/ReliefBridge_WeekWise_Plan.md)
📦 Component inventory: [COMPONENTS.md](docs/ReliefBridge_COMPONENTS.md)

**Up Next:** Week 2 — Accounts, Contacts, and record-type-based layouts for Nonprofit Partners, Corporate Donors, and Government Agencies.

## Notable Platform Findings

Along the way, this project surfaced a documented Salesforce platform limitation — Web-to-Lead-sourced records not reliably triggering native Assignment Rule email notifications to Queue members (Salesforce KB [000390823](https://help.salesforce.com/s/articleView?id=000390823&type=1)). Diagnosis process and workaround are documented in [Week 1 Documentation](docs/ReliefBridge_Week1_Documentation.md).

## Following Along
This build is being documented in a LinkedIn series as it progresses — each phase gets its own case study post covering not just what was built, but the design decisions and trade-offs behind it.

This build is being documented in a LinkedIn series as it progresses — each phase gets its own case study post covering not just what was built, but the design decisions and trade-offs behind it.

---

*Built by Manjari Deshpande — Sr. Salesforce Developer & Administrator.*
