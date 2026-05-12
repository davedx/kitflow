# European Hardware Procurement Platform — Product Spec

## Grounding

This spec is derived from primary research: job postings at Helsing, Neura Robotics, and 1X Technologies; Cofactr customer testimonials; Luminovo case studies; and European distributor API coverage. Features are included only where a documented pain point or validated customer behaviour supports them. Assumptions are flagged explicitly.

---

## MVP

**Goal:** Solve exactly one pain completely — BOM in, purchase orders out, without manual distributor site visits.

**Target customer:** 20–50 person drone or robotics startup with an engineer doing procurement in spreadsheets, building two to five times per year, no dedicated procurement hire.

---

### BOM Ingestion

Accept a BOM in whatever format the customer actually has it: CSV export from Altium, spreadsheet, Onshape BOM export, Arena PLM export. Parse without requiring a specific template.

AI earns its keep here immediately: interpreting inconsistent column names, resolving partial part numbers, handling multi-level BOMs, flagging rows it's uncertain about for human review rather than silently failing.

---

### Multi-Distributor Price and Availability Query

Against the ingested BOM, query Farnell, RS Components, TME, and Mouser APIs simultaneously for each line item. Return a consolidated view: who has stock, at what price, with what lead time.

No more opening four browser tabs per component.

---

### Intelligent Sourcing Recommendation

For each line item, recommend a source. Default optimisation: availability first, then price. Customer can override per line item or set global rules ("always prefer TME for passives", "never use RS for this project").

---

### Alternative Part Suggestions

Where a part is unavailable, out of stock, or on long lead time, suggest parametric alternatives from the same distributor catalogues. Pull equivalents based on specs, not just manufacturer cross-references. Flag alternatives for engineer sign-off — no auto-substitution without approval.

---

### Purchase Order Generation

Consolidate sourcing recommendations into purchase orders per distributor. Export as PDF or CSV, or place orders directly via API where supported. Farnell ordering API first; others where available; flag manual placement required where not.

This is the step that eliminates the one-to-two day pre-build ordering process documented in customer evidence.

---

### Order Tracking

Once orders are placed, track status from distributor APIs. Single dashboard: what's shipped, what's delayed, what's at risk. Alerts when a lead time slips or an item goes on backorder after ordering.

---

## Full Public Launch Product

**Goal:** Cover the full procurement lifecycle from BOM to delivery confirmation, with AI handling the operational work that currently falls on engineers. Expand to the 100–200 person company with a dedicated buyer who needs intelligence and agent layers to scale without adding headcount.

---

### Everything in MVP, plus:

---

### Supplier Management Layer

Persistent record of every supplier the customer has used — distributor accounts, direct manufacturer relationships, approved vendor lists. Track performance over time: on-time delivery rate, quality issues, lead time accuracy.

This is the institutional knowledge a senior procurement engineer currently carries in their head. The platform starts capturing it systematically from day one.

---

### Component Intelligence

Per component, surface:
- Lifecycle status (active, last time buy, obsolete)
- Known supply risk flags
- Single-source warnings
- Lead time trends over time

Integrated into the procurement workflow rather than a separate research tool. The engineer shouldn't have to leave the platform to discover that a component they just specified is going EOL in eight months.

---

### AI Procurement Agent

Beyond generating purchase orders: an agent that handles the full ordering workflow autonomously for routine builds.

Customer defines rules and approval thresholds — "for orders under €500 per line item with an approved distributor, place automatically; above that, surface for approval." Agent places orders, monitors them, handles distributor communications for simple issues (split shipments, substitution offers), and escalates exceptions to a human.

This is the Cofactr "Order Agent" model but transparently AI-driven and configurable, not opaque.

---

### BOM Versioning and Change Management

When a BOM revision comes in — hardware rev 4 to rev 5 — diff it against the previous version, identify changed and added line items, highlight which changes affect in-flight orders, and flag parts ordered for rev 4 that are now obsolete.

This is the workflow that falls apart in spreadsheets and causes excess inventory and production delays.

---

### Multi-Project Inventory Visibility

Across projects, show what's in stock, what's on order, and where there's overlap. If two projects both need the same capacitor, consolidate orders for better pricing. Surface excess inventory from one project that could be consumed by another before ordering new stock.

MRP-lite — not full Odoo MRP, but enough visibility to avoid obvious waste.

---

### ERP Integration

Odoo integration first, confirmed usage at 1X and prevalent in European manufacturing SMEs. Push purchase orders from the platform into Odoo, pull confirmed receipts back.

Arena PLM integration second, for companies using it as their BOM source of truth.

Once integrated, the platform becomes operational infrastructure rather than a standalone tool. This is the primary stickiness mechanism.

---

### Direct Manufacturer Sourcing and RFQ

For components where distributor pricing is uncompetitive at volume, or where a part isn't stocked by distributors, surface direct manufacturer options and support sending RFQs. AI drafts the RFQ from the BOM line item spec.

This is where "negotiation" starts to mean something real — not negotiating with Farnell, but helping a customer get a volume quote from TE Connectivity or Würth directly.

---

### Spend Analytics

Across all orders: spend by distributor, by component category, by project, over time. Identify where volume consolidation would unlock better pricing. Flag distributor relationships where the customer has enough spend to negotiate a framework agreement.

Turns procurement data into commercial insight.

---

### REACH/RoHS Compliance Flagging

*(Requires further customer research before speccing in detail.)*

Flag components with REACH SVHC substances or RoHS non-compliance as a byproduct of component data already pulled for procurement purposes.

**Not validated by customer evidence gathered so far.** Before building: establish whether customers experience this as a procurement-layer problem or handle it separately, whether it affects purchasing decisions or only documentation, and which customer segments (consumer vs. defence vs. industrial) actually require it. May be a meaningful differentiator for European customers; may be a solution looking for a problem at this layer.

---

## What This Deliberately Does Not Do At Launch

- Physical warehousing, inspection, or kitting — explicitly out of scope; revisit on demand signal
- Full compliance reporting or audit trails
- Supplier auditing or on-site verification
- Commodity price forecasting
- ITAR compliance — US market problem, not the European target
- Full MRP/ERP replacement
