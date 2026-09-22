# ReliefBridge — Week-Wise Build Plan
### A Sales Cloud–first Salesforce portfolio project (Apex, LWC, Flows, Integrations, Experience Cloud, Agentforce)

**Org setup note:** Build everything in one Developer Edition org. For the security/role-hierarchy phase, create 2–3 users (you + 1–2 fictional coworkers) — that's enough to demo sharing rules and forecast rollups convincingly.

---

## PHASE 1 — Data Model & Sales Cloud Foundations (Weeks 1–4)

### Week 1: Lead Management
- Add custom Lead fields: Organization Type, Area of Interest, Estimated Contribution Range, Lead Source Detail, Referred By Partner
- Build Web-to-Lead form
- Create Lead Assignment Rules (2–3 entries: high-value → senior queue, rest → general pool)
- Set up Duplicate Rule + Matching Rule on Lead (Email + Company)
- Configure Lead Conversion field mapping to Account/Contact
- Create the **ReliefBridge Lightning App** (Setup → App Manager → New Lightning App). Add only the **Leads** tab for now (plus Home, Reports, Dashboards); assign to your Admin profile. Expand the navigation items each subsequent week as new objects come online (Accounts/Contacts in Week 2, Opportunities in Week 3), and assign the App to Partnership Rep/Manager profiles once those exist (Week 4).
- **Deliverable:** Submit a test lead through the web form and convert it manually.

### Week 2: Accounts & Contacts
- Create Account Record Types: Nonprofit Partner, Corporate Donor, Government Agency
- Build distinct page layouts per record type
- Add custom Account fields: Service Region, Partner Tier, Active Partnership Since
- Set up Account hierarchy (parent org → regional chapters)
- Enable Contact Roles + add custom role picklist values (Decision Maker, Grant Administrator, Logistics Coordinator, Field Contact)
- **Deliverable:** 5–6 sample Accounts (mix of record types) with hierarchy and Contacts attached.

### Week 3: Opportunities, Products & Price Books
- Build custom Opportunity Stage picklist + Sales Path (Prospecting → Needs Analysis → Proposal Sent → Committed → Fulfilled/Declined)
- Add custom Opportunity fields: Fulfillment Deadline, Funding Type
- Create Products (Water Pallet, Shelter Kit, Medical Supply Kit, Cash Grant) + Standard Price Book + custom "2026 Relief Catalog" Price Book
- Add Opportunity Line Items so Amount rolls up from products
- Build validation rule: block stage change to "Committed" without a Contact Role and Amount > 0
- **Deliverable:** 8–10 Opportunities across stages, with products attached, moving through the Path.

### Week 4: Campaigns, Security & Forecasting
- Build parent + child Campaign structure; enable Campaign Influence
- Create 2–3 test users; build 3-tier role hierarchy (VP → Coordinator → Rep)
- Clone profiles (Partnership Rep, Partnership Manager); build a Forecast Manager permission set
- Set OWD to Private on Account/Contact/Opportunity; add a criteria-based sharing rule (Service Region match)
- Enable Collaborative Forecasting
- **Deliverable:** Full Phase 1 walkthrough — record a 5-minute Loom demoing Lead → Convert → Opportunity → Forecast, with sharing working across your test users. Post it on LinkedIn.

---

## PHASE 2 — Flow Automation (Weeks 5–7)

### Week 5: Screen Flows
- Build a Screen Flow for Volunteer/Partner self-registration with skill/availability capture (feeds Contact)
- Add a Screen Flow for Coordinators to log a new Resource Request quickly

### Week 6: Record-Triggered & Approval Flows
- Record-Triggered Flow: notify Coordinators (email/Chatter) when a large Opportunity moves to "Proposal Sent"
- Approval Process: Opportunities above a $ threshold require Coordinator sign-off before "Committed"

### Week 7: Scheduled Flows & Cleanup
- Scheduled Flow: nightly job flags Opportunities with no activity in 14+ days as "At Risk"
- Review all Flows for bulkification and error handling (fault paths, not just happy path)
- **Deliverable:** Document each Flow with a short "what it does and why" — this becomes interview talking points.

---

## PHASE 3 — Apex (Weeks 8–12)

### Week 8: Trigger Framework
- Build a trigger handler framework (one trigger per object, logic in handler classes — not directly in trigger body)
- Apply it to Opportunity (e.g., auto-update Partner Tier on Account based on cumulative closed-won Amount)

### Week 9: Core Apex Logic
- Build the signature feature: an Apex class that scores/prioritizes open Opportunities by urgency + amount + partner tier
- Write it as a callable, testable service class (not just trigger logic)

### Week 10: Async Apex
- Batch Apex: nightly re-scoring of all open Opportunities
- Queueable/Future class: prep for an external callout (used in Phase 5)

### Week 11: Apex Testing
- Write test classes for all of the above — 90%+ coverage, bulk-safe (test with 200 records), assert both success and failure paths
- Mock callouts using `HttpCalloutMock` in preparation for Phase 5

### Week 12: Buffer & Review
- Catch-up week — finish any lagging test coverage, refactor for readability
- **Deliverable:** Push all Apex to a GitHub repo via SFDX; write a README explaining the scoring algorithm's logic — this is your strongest interview story.

---

## PHASE 4 — LWC (Weeks 13–16)

### Week 13: Foundational Components
- Build a "My Opportunities" LWC for reps (list view + inline stage updates) using Lightning Data Service

### Week 14: Data Visualization Component
- Build a pipeline/forecast dashboard LWC (wire adapter pulling aggregated Opportunity data, rendered as a chart)

### Week 15: Real-Time Component
- Build a component using Platform Events — e.g., push a live notification to open dashboards when a high-value Opportunity closes

### Week 16: Polish
- Add error handling, loading states, and responsive styling to all LWCs
- **Deliverable:** Record a screen capture of the dashboard updating live — strong LinkedIn/portfolio content.

---

## PHASE 5 — Integrations (Weeks 17–19)

### Week 17: Named Credentials & External Services
- Set up Named Credentials properly (no hardcoded endpoints/keys)
- Configure at least one External Service definition from an OpenAPI spec

### Week 18: Real Callouts
- REST callout to a public API (e.g., geocoding an Account's address to lat/long for territory mapping)
- Wire the Queueable class from Week 10 to fire this callout on Opportunity creation

### Week 19: Notification Integration
- Integrate an email or SMS service (e.g., Twilio trial) to alert a Coordinator when a Committed Opportunity is created
- **Deliverable:** Diagram (even a simple one) showing your integration architecture — Named Credential → Apex → External API → back into Salesforce.

---

## PHASE 6 — Experience Cloud (Weeks 20–22)

### Week 20: Site Setup
- Stand up an Experience Cloud site (Partner Central template) for Nonprofit/Corporate partners
- **Follow-up from Week 1:** build a real "Thank You" page on this site and swap it into the Web-to-Lead form's Return URL (Week 1 used a placeholder URL since no public-facing page existed yet)

### Week 21: Partner Self-Service
- Let partners log in and view/track their own Opportunity status and submitted resource requests
- Configure sharing sets so partners see only their own org's records

### Week 22: Security Review
- Review guest user profile permissions carefully (a very common real-world misconfiguration and a favorite interview question)
- **Deliverable:** A working external login you can demo live in an interview.

---

## PHASE 7 — Agentforce (Weeks 23–25)

### Week 23: Intake Agent
- Build an Agentforce agent that lets a partner describe a request in plain language and creates a structured record from it

### Week 24: Agent Actions
- Give the agent an action to check current Price Book/Product availability and suggest options

### Week 25: Guardrails
- Add topics/instructions so the agent stays on-domain and hands off appropriately
- **Deliverable:** This is your closing "wow" demo — record it last, once everything else is stable.

---

## PHASE 8 — Engineering Polish (Ongoing, Weeks 1–25 in parallel)

- Week 1: Initialize Git repo + SFDX project structure from day one
- Week 4, 12, 19, 22: Tag a release/milestone in Git after each major phase
- Week 11 onward: Set up a basic GitHub Actions pipeline running Apex tests on push
- Week 25: Write the full README (architecture diagram, ERD, setup instructions, demo video links) and do a final LinkedIn recap post series (one post per phase, or a single capstone post with the full walkthrough video)

---

## Weekly Rhythm (suggested)
- **~4–6 hours/week** is realistic for this pace alongside other commitments
- End each week with a 2–3 sentence LinkedIn post or personal changelog — builds visibility *while* learning, not just at the end
- Don't skip test coverage or documentation to "save time" — those are exactly what interviewers probe on portfolio projects

---

*Total timeline: ~25 weeks (~6 months) at a sustainable pace. Compress by combining weeks if you have more hours available, but don't skip the testing/documentation steps — they're a large part of what makes this a credible portfolio piece rather than a toy demo.*

---

## Appendix — Access & Visibility Checklist (apply every week you add fields/objects)

Every time you create a new custom field, object, or record type, walk this checklist before moving on. Skipping it is the #1 cause of "I built it but it's not showing up" bugs.

1. **Field-Level Security (FLS)** — When creating a field, explicitly set it Visible (and Editable, if needed) for every profile you're actively testing with. New fields default to hidden on most profiles.
2. **Page Layout** — Adding a field to an object does NOT add it to the layout. Go to the object's Page Layouts and drag the field in — group related fields into a labeled section for clarity.
3. **Record Type → Page Layout Assignment** — If the object has Record Types (Account does, starting Week 2), each Record Type needs its own layout assignment per profile (Object Manager → [Object] → Page Layouts → Page Layout Assignment).
4. **Queues** — If a Flow or Assignment Rule routes to a queue, create the queue first (Setup → Queues) and add members. Routing to a non-existent queue fails silently or errors on activation.
5. **Object/Field Permissions on Profiles or Permission Sets** — Check Create/Read/Edit/Delete at the object level, not just FLS at the field level, for every profile involved (especially cloned profiles like Partnership Rep/Manager, which don't inherit System Administrator's full access).
6. **Special Permissions** — Some actions need an explicit permission beyond standard CRUD: "Convert Leads," "Transfer Record," "Modify All Data," etc. Check these on the profile if a feature "should work" but silently doesn't.
7. **Web-to-Lead / Web-to-Case Field Exposure** — A field only appears in the Web-to-Lead generator if it's already on the page layout AND visible per FLS to the profile that owns unassigned records. Build the form last, after 1–3 are done.
8. **Sharing Rules / OWD** — If a record isn't visible to a test user despite object/field permissions being correct, the issue is almost always Org-Wide Defaults + missing sharing rule, not FLS. Check OWD before assuming a permissions bug.

**Suggested build order to avoid backtracking (applies every week):**
Fields (with FLS set) → Page Layout → Record Type assignment (if applicable) → Queues (if needed) → Profile/Permission Set object access → Special permissions → Web-to-Lead/Case forms (if applicable) → Sharing Rules/OWD.
