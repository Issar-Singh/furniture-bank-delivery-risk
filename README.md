# Furniture Bank — Delivery Risk Dashboard
### Stage 2 Submission | Analytics & Data Associate

---

## Overview

This repository contains my Stage 2 submission for the Analytics & Data Associate role at Furniture Bank.

The brief asked for one thing that answers a question a Furniture Bank team member would actually need answered on a Tuesday morning. This is that thing.

**What I built:** A Delivery Risk Dashboard in Power BI, connected to a synthetic five-source data model designed to reflect how Furniture Bank's data actually lives across systems — not how it looks in a clean export.

---

## The Tuesday Question

> *"Which deliveries going out in the next 72 hours still have an open blocker — and what needs to happen before they leave the building?"*

Without a tool like this, answering that means manually cross-referencing Salesforce, a dispatch log, and an inventory sheet. This dashboard makes it visible in under a minute.

---

## Repository Structure

```
furniture-bank-delivery-risk/
│
├── README.md                             # This file
├── SUBMISSION.md                         # Five question answers
│
├── dashboard/
│   └── screenshot.png                    # Power BI dashboard screenshot
│   └── Furniture_Bank_Delivery_Risk.pbix # Power BI working file
│
├── data/
│   └── furniture_bank_synthetic.xlsx     # Five synthetic data sheets
│
└── docs/
    └── data-dictionary.md                # Column definitions and assumptions
```

---

## Data Model

The synthetic dataset simulates five operational data sources:

| Sheet | Simulates | Key Fields |
|---|---|---|
| `raw_referrals_typeform` | Typeform intake forms | referral_id, agency_name, urgent_flag, requested_items |
| `raw_clients_salesforce` | Salesforce client cases | client_id, intake_complete, client_confirmed, target_delivery_date |
| `raw_inventory_match` | Inventory matching system | match_status, essential_items_complete, missing_item_count |
| `raw_delivery_ops` | Delivery scheduling | delivery_id, assigned_truck_id, route_zone, delivery_status |
| `raw_routes_trucks` | Truck routing and capacity | truck_id, stops_assigned, max_stops, route_capacity_flag |

The data is intentionally messy — inconsistent boolean formats, mixed capitalisation, missing values — because that is what real multi-system data looks like before it has been standardised. Power Query and M code were used to clean and join these into a master risk table inside Power BI.

---

## Dashboard Summary

**KPI Cards**
- 30 deliveries scheduled in the next 7 days
- 6 outside the 72-hour target
- 9 unassigned truck deliveries
- 19 high-risk | 10 medium-risk

**Visuals**
- Risk reason breakdown by count (bar chart)
- Zone vs. risk level distribution (stacked bar)
- Row-level operations table: delivery ID, date, zone, risk level, reason, recommended action, truck assignment

**Filters**
- Route zone slicer
- Risk level slicer

---

## Important Notes

- All data is **synthetic**. No real client, donor, or operational records were used.
- This is a **working prototype**, not a polished production tool.
- The five answers to the brief questions are in [`SUBMISSION.md`](./SUBMISSION.md).

---

*Furniture Bank Stage 2 — April 2026*
