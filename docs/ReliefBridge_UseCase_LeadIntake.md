# Case Study: Smart Lead Intake for ReliefBridge — Assignment Rules, Branded Notifications & Duplicate Management

*A standalone deep-dive from my ReliefBridge Salesforce portfolio project (a disaster-relief nonprofit CRM built on Sales Cloud).*

## The Business Problem

When a nonprofit, corporate donor, or government agency expresses interest in partnering with ReliefBridge, that inquiry needs to:
1. Reach the right internal team automatically, based on the size and type of opportunity
2. Never sit unacknowledged — the prospect should get an immediate, professional response
3. Never get lost or duplicated when the same prospect submits more than once

This is a real operational problem for any org running inbound intake at volume, and it's a great showcase of core Sales Cloud configuration: Web-to-Lead, Assignment Rules, Queues, and Duplicate Management working together.

## What Was Built

**Capture:** A public Web-to-Lead form collects Organization Type, Area of Interest, and Estimated Contribution Range alongside standard contact details.

**Routing:** A Lead Assignment Rule evaluates each submission — Government Agencies and contributions over $100K route to a `Senior_Coordinators` queue; everything else goes to a `General_Partnership_Pool` queue. This means a high-value inquiry never waits in a general queue behind smaller ones.

**Notification:** Both the assigned queue and the prospect receive branded, templated emails — the queue gets a clear internal summary (Lead name, org type, interest area, contribution range) so the team can act immediately; the prospect gets a professional confirmation so they're not left wondering if their inquiry went through.

**Duplicate Protection:** A Duplicate Rule (configured as Allow + Report, not Block) means a second, near-identical submission still creates a record — nothing is silently dropped — but the system automatically generates a Duplicate Record Set and flags it for review. This is a deliberate design choice: on a public-facing form, silently blocking submissions risks losing a real prospect's second attempt; surfacing the duplicate for a human to review is safer.

## Why "Allow" Instead of "Block"

This is a small decision with real consequences. Blocking duplicate creation on a public web form fails silently from the visitor's side — there's no dialog box, no error, the submission simply doesn't create a record and nobody is notified. Choosing Allow + Alert means every submission is preserved, and duplicates are surfaced as a reviewable queue item instead of a lost lead.

## A Real Debugging Story: Diagnosing a Platform Limitation

While testing the assignment notification flow, queue members initially weren't receiving the internal alert email — despite Deliverability set to "All Email," confirmed queue membership, a verified Org-Wide Email Address, and correct Field-Level Security. The same Assignment Rule and Queue worked correctly when a Lead was created manually, isolating the issue specifically to the Web-to-Lead channel.

The root cause turned out to be a documented Salesforce platform limitation (Help Article 000390823): automated field updates firing in quick succession on a Web-to-Lead-sourced record can suppress subsequent email notifications tied to that record. This wasn't a misconfiguration — it was a known platform quirk with a documented (if imperfect) resolution path.

This is worth including here because it reflects the actual day-to-day of Salesforce work: not every broken thing is a config mistake, and knowing how to systematically rule out causes — Deliverability, membership, verification, FLS — before concluding "this is a platform issue, here's the citation" is its own skill.

## Evidence

Full screenshot walkthrough — branded assignment email, branded auto-response, correct Lead routing, the duplicate submission test, and the auto-flagged Duplicate Record Item — is available in [ReliefBridge_Week1_Evidence.docx](ReliefBridge_Week1_Evidence.docx).

## Result

End-to-end, a prospect fills out one form and, within moments:
- Their Lead is created and correctly routed by value/type
- The right internal queue is notified with the specifics needed to follow up
- The prospect receives a confirmation
- If they (or someone else) submits a near-duplicate, it's flagged for merge/review rather than silently dropped or silently duplicated

## Skills Demonstrated
Web-to-Lead configuration · Assignment Rules (criteria-based routing) · Queues · Duplicate & Matching Rules · Email template branding · Systematic troubleshooting of platform-level automation behavior

---

*This is one module from a larger 25-week Salesforce portfolio build (ReliefBridge) covering the full Sales Cloud stack plus Apex, LWC, Integrations, Experience Cloud, and Agentforce. Full build log: [ReliefBridge_Week1_Documentation.md](ReliefBridge_Week1_Documentation.md). Source and full write-up on GitHub: [link].*
