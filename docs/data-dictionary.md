# Data Dictionary
### Furniture Bank Delivery Risk — Synthetic Dataset

---

> All data in this workbook is synthetic. It was generated to simulate realistic operational records across Furniture Bank's inferred data systems. No real client, donor, or staff data was used.

---

## Sheet 1: `raw_referrals_typeform`
*Simulates intake referral forms submitted via Typeform by referring agencies*

| Column | Type | Description |
|---|---|---|
| referral_id | String | Unique referral identifier (e.g. R-1001) |
| referral_date | DateTime | Date and time the referral was submitted |
| client_id | String | Links to client record in Salesforce |
| agency_name | String | Referring agency (e.g. WoodGreen, COSTI, Fred Victor) |
| household_size | Integer | Number of people in the household |
| address_text | String | Delivery address — intentionally unstructured |
| postal_code | String | Intentionally inconsistent formatting to simulate real form data |
| phone_number | String | Intentionally inconsistent formatting (dashes, dots, brackets) |
| urgent_flag | String | Urgency flag — inconsistent values (Urgent / FALSE / null) to simulate real form output |
| requested_items | String | Free-text list of requested furniture items |
| notes | String | Additional intake notes from the referring agency |

**Intentional data quality issues:** postal codes missing or malformatted, phone numbers in multiple formats, urgent_flag not standardised — reflects real Typeform export behaviour.

---

## Sheet 2: `raw_clients_salesforce`
*Simulates client case records exported from Salesforce*

| Column | Type | Description |
|---|---|---|
| client_id | String | Unique client identifier (e.g. C-2001) |
| case_id | String | Salesforce case number (e.g. CASE-3001) |
| client_name | String | Client name (synthetic) |
| referral_status | String | Current referral status — inconsistent capitalisation (New / new / Approved) |
| intake_complete | String | Whether intake is complete — mixed booleans (TRUE / FALSE / null) |
| client_confirmed | String | Whether client has confirmed delivery — mixed values (Confirmed / Yes / FALSE / null) |
| service_priority | String | Priority level — inconsistent casing (standard / Standard / High / priority) |
| target_delivery_date | DateTime | Target delivery date based on 72-hour service cycle |
| current_case_stage | String | Stage in the case workflow |
| last_updated | DateTime | Timestamp of last record update |

**Intentional data quality issues:** referral_status, intake_complete, client_confirmed, and service_priority all contain inconsistent values — reflects real Salesforce export behaviour where fields are free-text or have evolved over time without enforced picklists.

---

## Sheet 3: `raw_inventory_match`
*Simulates inventory matching records — likely maintained in a Google Sheet or internal system*

| Column | Type | Description |
|---|---|---|
| case_id | String | Links to Salesforce case record |
| match_status | String | Overall match status (Complete / Partial / Partial Match / null) |
| essential_items_complete | String | Whether all essential items are matched — mixed values (TRUE / FALSE / Yes / No / null) |
| missing_item_count | Integer | Number of items not yet matched |
| pending_dependency | String | Whether match is blocked by a dependency — mixed booleans |
| last_match_update | DateTime | Timestamp of last inventory update |
| blocker_reason | String | Free-text description of what is blocking the match |

**Intentional data quality issues:** essential_items_complete uses four different value formats across rows — reflects the reality that inventory tracking often lives in manually maintained sheets with no enforced data validation.

---

## Sheet 4: `raw_delivery_ops`
*Simulates delivery scheduling and operations records*

| Column | Type | Description |
|---|---|---|
| delivery_id | String | Unique delivery identifier (e.g. D-4001) |
| case_id | String | Links to Salesforce case record |
| scheduled_delivery_date | DateTime | Planned delivery date |
| assigned_truck_id | String | Truck assigned to delivery — null if unassigned |
| route_zone | String | Delivery zone (Central / East / West / North / South) |
| delivery_status | String | Current status (Scheduled / Pending / Reschedule Review) |
| assigned_team | String | Crew assigned to the delivery |
| delivery_window | String | Time window for delivery (e.g. 9-12 / AM / PM / 3-6) |
| special_handling_flag | String | Whether delivery requires special handling |

---

## Sheet 5: `raw_routes_trucks`
*Simulates truck routing and capacity data — likely maintained in a dispatch sheet*

| Column | Type | Description |
|---|---|---|
| truck_id | String | Truck identifier (e.g. T01, T07) |
| scheduled_date | DateTime | Date the truck is scheduled to operate |
| route_zone | String | Zone the truck is assigned to on that date |
| stops_assigned | Integer | Number of stops currently assigned |
| max_stops | Integer | Maximum stops the truck can handle |
| route_capacity_flag | String | Capacity status (Normal / Near Capacity / Over Capacity) |
| truck_capacity_score | Integer | Capacity score out of 100 |
| join_key | String | Composite key used to join truck data to delivery records (truck_id + date) |

---

## Risk Scoring Logic (applied in Power BI master table)

| Risk Level | Condition |
|---|---|
| High | Delivery within 72 hours with any critical blocker (missing items, no truck, unconfirmed client) |
| Medium | Blocker exists but delivery is 3–7 days out — time to resolve |
| Low | No significant blockers, delivery feasible with current setup |

**Risk reason priority (when multiple blockers exist):**
1. Missing essential items
2. No truck assigned
3. Client not confirmed
4. Intake incomplete
5. Partial furniture match

---

*Furniture Bank Stage 2 — April 2026*
