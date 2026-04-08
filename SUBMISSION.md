# Stage 2 Submission — Five Questions
### Furniture Bank | Analytics & Data Associate

---

> **Time disclosure:** I'll be upfront — I went over the 4-hour mark. A significant chunk was research: I watched Furniture Bank's videos and went through the website to understand the operation before building, not just generate a generic logistics dashboard. That research directly shaped the data structure and risk logic. As I get more fluent with Power BI and AI-assisted workflows, the build time comes down fast.

---

## Q1 — Show us the output

I built a Furniture Bank Delivery Risk Dashboard in Power BI, connected to a synthetic five-source data model designed to reflect how Furniture Bank's data actually lives across systems — not how it looks in a clean export.

The five sheets simulate: referral intake from Typeform, client case records from Salesforce, inventory match status, delivery operations, and truck route capacity. The data is intentionally messy — inconsistent boolean formats, mixed capitalisation in status fields, missing values — because that is what real multi-system data looks like before standardisation. Power Query and M code were used to clean and join these into a master risk table.

The dashboard answers one question: which deliveries in the next 7 days are at risk, why, and what needs to happen today. It includes KPI cards, a risk reason breakdown, a zone-vs-risk stacked bar, and a row-level operations table with recommended actions. Zone and risk level slicers allow filtering by area or urgency.

Working prototype on synthetic data, labeled as such. The data structure is the real deliverable — the records are not.

📊 Screenshot: [`dashboard/screenshot.png`](./dashboard/screenshot.png)
📁 Power BI file: [`dashboard/Furniture_Bank_Delivery_Risk.pbix`](./dashboard/Furniture_Bank_Delivery_Risk.pbix)

---

## Q2 — Name the Tuesday question

The operations coordinator opens this Tuesday morning and asks: *"Which deliveries going out in the next 72 hours still have an open blocker — and what needs to happen before they leave the building?"*

Without a tool like this, answering that means manually cross-referencing Salesforce, a dispatch log, and an inventory sheet. This makes it visible in under a minute.

---

## Q3 — State your data assumptions

- Delivery records live in Salesforce; truck assignment tracked there or in a parallel Google Sheet
- Client confirmation is a status field on the delivery record
- Five data sources modelled: Typeform (referrals), Salesforce (client cases), inventory match, delivery ops, truck routing — inferred from Furniture Bank's website and public operational context
- The 72-hour threshold came directly from the brief — Furniture Bank's own standard, not invented

**Where I am likely wrong:**
- Inventory readiness probably is not a clean queryable field — it may live in a warehouse coordinator's head, not a database. If so, "missing essential items" cannot be auto-populated without a manual input step that may not currently exist
- Truck assignment may be more complex than binary — capacity constraints and route conflicts are not fully captured by a single truck ID, though I did model route capacity as a separate field to gesture at this

---

## Q4 — Tell us what you got wrong about our operations

This is the question I sat with longest.

Before building, I researched Furniture Bank's website and watched YouTube videos specifically to understand the donation-to-client flow — not just for background, but to find where the coordination tax actually hits hardest. That research pointed me toward delivery and dispatch as the right place to focus.

But public-facing content shows what Furniture Bank does, not how it operates internally. I had to guess at:

- How inventory readiness is actually tracked day-to-day
- Whether dispatch runs from one system or across multiple tools
- How client confirmation works in practice — who owns it, when it happens, what "confirmed" actually means in their workflow

**My biggest assumption that is likely wrong:** I modelled "missing essential items" as something the system knows. It is probably something a warehouse coordinator knows from a physical check that never makes it into a database field. If that is true, the dashboard's most critical risk reason depends on a manual input step that does not currently exist.

I also chose to focus on delivery risk rather than intake, donor management, or funder reporting. That choice could be wrong — the coordination tax might hit harder elsewhere. Week one, I would sit with the operations coordinator and ask: where do things actually fall apart on a Tuesday?

---

## Q5 — Show us where AI helped and where you overrode it

I used ChatGPT heavily throughout this brief. Here is where my judgment and the AI's diverged:

**Where AI helped:**
- Generated the initial dataset structure and risk reason categories quickly
- Produced M code for Power Query once I shifted away from asking it to write Power Query directly — it handled explicit code better than GUI-level instructions

**Where I overrode it:**
- ChatGPT's first output was a static Excel file — five sheets and a master table that did not update dynamically. That is useless as a decision-support tool. I switched to Power BI entirely. That decision was mine.
- I asked the AI to make the data intentionally messy — inconsistent booleans, mixed capitalisation, nulls. Left to itself, AI produces clean synthetic data that looks nothing like a real Salesforce export. The coordination tax lives in the cleaning step, not just the visualisation. That added time. I think it was the right call.
- I stripped back a multi-chart layout the AI suggested to a single focused page. The brief penalises over-production. AI tends toward completeness. This role requires restraint.

The framing of these answers is also mine. AI would have written more confident answers. I chose to write accurate ones instead.

---

*Furniture Bank Stage 2 — April 2026*
