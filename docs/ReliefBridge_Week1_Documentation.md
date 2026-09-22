# ReliefBridge — Week 1 Documentation: Lead Object & Web-to-Lead

## Objective
Build the Lead object foundation for ReliefBridge: custom fields, Web-to-Lead capture form, Lead Assignment Rule routing to queues, and the ReliefBridge Lightning App shell.

## What Was Built

### Custom Fields (Lead)
| Field Label | API Name | Type |
|---|---|---|
| Organization Type | `Organization_Type__c` | Picklist |
| Area of Interest | `Area_of_Interest__c` | Multi-Select Picklist |
| Estimated Contribution Range | `Estimated_Contribution_Range__c` | Picklist |
| Lead Source Detail | `Lead_Source_Detail__c` | Text (255) |
| Referred By Partner | `Referred_By_Partner__c` | Lookup(Account) |

### Access & Visibility
- Field-Level Security set for all 5 fields on active profile(s)
- Fields added to Lead Page Layout in a dedicated "ReliefBridge Details" section
- Two Queues created: `Senior_Coordinators`, `General_Partnership_Pool` — both with "Send Email to Members" enabled
- Confirmed "Convert Leads" permission on working profile

### Profiles Created

Both cloned from **Standard User** (not System Administrator) to keep access realistic and demonstrable.

**Partnership Rep** — individual seller; owns and progresses their own Leads/Opportunities.
**Partnership Manager** — team oversight; broader visibility, approval authority, strategic field edit rights.

**Object-Level (CRUD) Permissions:**

| Object | Partnership Rep | Partnership Manager |
|---|---|---|
| Lead | Create, Read, Edit (no Delete) | Create, Read, Edit, Delete |
| Account | Create, Read, Edit (no Delete) | Create, Read, Edit, Delete |
| Contact | Create, Read, Edit (no Delete) | Create, Read, Edit, Delete |
| Opportunity | Create, Read, Edit (no Delete) | Create, Read, Edit, Delete |
| Campaign | Read only | Create, Read, Edit |

**Field-Level Security — Lead fields:**

| Field | Partnership Rep | Partnership Manager |
|---|---|---|
| Organization_Type__c | Visible, Editable | Visible, Editable |
| Area_of_Interest__c | Visible, Editable | Visible, Editable |
| Estimated_Contribution_Range__c | Visible, Editable | Visible, Editable |
| Lead_Source_Detail__c | Visible (read-only) | Visible, Editable |
| Referred_By_Partner__c | Visible, Editable | Visible, Editable |

*Rationale: Lead Source Detail reflects marketing/web-form attribution — Reps shouldn't edit attribution data; Managers may need to correct it.*

**Special Permissions:**

| Permission | Partnership Rep | Partnership Manager |
|---|---|---|
| Convert Leads | Enabled | Enabled |
| Transfer Record | Disabled | Enabled |
| Approve/Reject in Approval Process | Disabled | Enabled (Week 6) |
| View All Forecasts | Disabled | Enabled via Forecast Manager permission set (Week 4) |

**Design note:** Manager-only rights (delete, transfer, approve, and strategic Account fields like Partner_Tier__c) live on the **profile** since they're core to the role. Forecast access is layered on via a **permission set** instead, to keep the profile baseline lean — a deliberate profile-vs-permission-set separation, and a good interview talking point on access-model design.

**Not yet assigned:** ReliefBridge Lightning App still needs to be assigned to both profiles once Week 4 security work is finalized (currently assigned to Admin only).

### Automation
- **Lead Assignment Rule** (`ReliefBridge_Lead_Assignment`), active, 2 entries:
  1. Organization Type = Government Agency OR Contribution Range = $100K+ → `Senior_Coordinators`
  2. All other Leads → `General_Partnership_Pool`
- **Web-to-Lead form** generated with "Use Lead Assignment Rule" checked
- **Duplicate/Matching Rule** on Lead (Email exact match, Company fuzzy match) — configured as **Allow + Report** on Create, not Block. Rationale: a public-facing form fails silently on Block (no record, no error, no notification to anyone); Allow + Report preserves every submission and surfaces duplicates as a reviewable Duplicate Record Set instead. Confirmed working: a second submission with matching email auto-generated a Duplicate Record Set and Duplicate Record Item, flagged as created by "Automated Process."

### Email Branding
- Built a **Letterhead** (Classic Email Template infrastructure) with a branded header banner (ReliefBridge logo + tagline) and footer (contact info, social icons)
- Built two **HTML Email Templates using the Letterhead**:
  1. Internal assignment notification — sent to Queue members, includes Lead Name, Organization Type, Area of Interest, Estimated Contribution Range
  2. External auto-response — sent to the Lead, confirms receipt and sets a 24-business-hour follow-up expectation
- Logo/header/footer images stored as Salesforce Documents/Static Resources (retrieved separately from object metadata — see Component Inventory)

### Navigation
- **ReliefBridge Lightning App** created with Leads tab (Home, Reports, Dashboards included); assigned to Admin profile. To be expanded with Accounts/Contacts (Week 2) and Opportunities (Week 3).

## Testing Performed
- Submitted test Lead via Web-to-Lead form → confirmed correct routing to queue per Assignment Rule criteria
- Created test Lead manually via Salesforce UI (with "Assign using active assignment rule" checked) → confirmed correct routing
- Verified Lead Conversion produces expected Account/Contact records
- Submitted a duplicate test Lead (same email, different name/company) → confirmed record still created (not blocked) and a Duplicate Record Set/Item auto-generated for review
- Confirmed branded assignment and auto-response emails render and deliver correctly end-to-end (see screenshot evidence below)

## Evidence
All screenshots documenting the working pipeline are consolidated into a single companion document: **[ReliefBridge_Week1_Evidence.docx](ReliefBridge_Week1_Evidence.docx)**. It walks through, in order: the branded internal assignment email, the Lead record confirming correct queue ownership, the branded auto-response, the duplicate submission test, the duplicate Lead's independent routing, and the auto-generated Duplicate Record Item.

Full narrative write-up using the same screenshots: [Lead Intake Case Study](ReliefBridge_UseCase_LeadIntake.md)


## Known Issue (Investigated & Resolved): Web-to-Lead Assignment Notification Delay

**Symptom encountered during initial testing:** When a Lead was created via Web-to-Lead and routed to a Queue via the Assignment Rule, Queue Members initially did not receive the assignment notification email — despite:
- Queue "Send Email to Members" enabled and confirmed saved
- Deliverability set to "All Email"
- Org-Wide Email Address verified
- Queue membership confirmed (user correctly listed under Queue Members)
- Email Log Files showing no send error

**Contrast at the time:** The identical Assignment Rule and Queue correctly triggered the notification email when a Lead was created manually through the Salesforce UI (with "Assign using active assignment rule" checked). The failure was specific to the Web-to-Lead creation channel.

**Root cause investigated (Salesforce Known Issue):** Documented in Salesforce Help Article [000390823 — "Salesforce Users Are Not Receiving Web to Lead Assignment Notifications"](https://help.salesforce.com/s/articleView?id=000390823&type=1). Salesforce's own guidance indicates automated field updates on a record in quick succession (as happens when Assignment Rule + Auto-Response Rule both fire near-simultaneously on Web-to-Lead-sourced records) can suppress subsequent email notifications tied to that record — a known platform limitation, not a configuration error.

**Attempted fix:** Enabled Setup → Process Automation Settings → "Send an email each time automation updates the same record" (per Salesforce's Resolution Option 1).

**Current status:** Branded assignment and auto-response emails are now confirmed delivering correctly end-to-end (see Evidence section below). Built and retained the diagnostic trail below as documentation, since it's a legitimate example of systematic platform troubleshooting.

**Interview talking point:** This is a good example to have ready — it demonstrates real diagnostic process (systematically ruling out Deliverability, membership, OWEA, FLS) and shows awareness that not every issue is a config error; some are documented platform limitations with a known workaround pattern.

## Carried Forward to Later Weeks
- Week 6: Build custom Record-Triggered Flow for Queue email notification, as a more visible/testable/maintainable alternative to relying on native Assignment Rule email behavior for Web-to-Lead
- Week 20: Replace placeholder Web-to-Lead Return URL with real Experience Cloud "Thank You" page
