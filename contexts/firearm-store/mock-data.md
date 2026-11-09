# Firearm Store Mock Data

Everything the firearm-store context needs to be mocked. Nothing here is real data, and nothing connects to a live service. **Owner** is the only role allowed to change the data (an orchestrator, or an external party for its own data). Files are JSON under `data/firearm-store/`. External party folders are named after the party's role ID. **Created** marks files that exist; the others are still to be written (purchasing and technical-services data aren't needed until those scenarios are tested).

## Shared Records (owned by units)

| Data | Contents | Owner | Used by | Location |
|---|---|---|---|---|
| Catalog | Products for sale: category (handgun, rifle, shotgun...), caliber, fire modes (semi-auto, burst, full-auto), model, list price | Sales orchestrator | Sales consultant, sales orchestrator | `records/sales/catalog.json` (created) |
| Inventory | Item records: item ID, catalog reference, serial number (fake), new or used, status (available, reserved, sold, on hold, rejected), location, `release_ref` (the release disposition that made it sellable) | Inventory orchestrator | Stock manager, receiving, sales (through contract) | `records/inventory/items.json` (created) |
| Reservations | Item, case, expiry | Inventory orchestrator | Stock manager | `records/inventory/reservations.json` (created) |
| Intake and movement records | What arrived, moved, or was delivered to a customer, when, and for which purchase or case | Inventory orchestrator | Receiving operations | `records/inventory/movements.json` (created) |
| Compliance requirements | Documents and customer details required per item. Known requirement: any gun with a full-auto mode requires the customer's address | Compliance orchestrator | Documentation specialist | `records/compliance/requirements.json` (created) |
| Form definitions | Fields of each mock form | Compliance orchestrator | Documentation specialist | `records/compliance/forms.json` (created) |
| Customer financial profiles | Mock credit score, income, existing debt, used for credit analysis | Finance orchestrator | Payments and credit specialist | `records/finance/credit-profiles.json` (created) |
| Installment terms | Available plans, interest rates, number of installments (the only source of plan terms) | Finance orchestrator | Payments and credit specialist | `records/finance/installment-terms.json` (created) |
| Payment methods | Accepted methods and their status before payment | Finance orchestrator | Payments and credit specialist | `records/finance/payment-methods.json` (created) |
| Ledger | Every sale and purchase, with amounts | Finance orchestrator | Accounting specialist, financial analyst | `records/finance/ledger.json` (created) |
| Price history | Past purchase prices per product and supplier | Purchasing orchestrator | Procurement analyst | `records/purchasing/price-history` |
| Purchase records | Supplier orders and used-item acquisitions | Purchasing orchestrator | Purchasing specialists | `records/purchasing/` |
| Provenance reference data | Historical and collectible reference information | Technical services orchestrator | Collectibles research specialist | `records/technical-services/provenance` |
| Inspection records | Condition assessments, inspection results, dispositions | Technical services orchestrator | QA manager, inventory (through contract) | `records/technical-services/inspections.json` (created: pass results and release dispositions for the starting stock) |

## External Party Data

| Data | Contents | Owner | Location |
|---|---|---|---|
| Criminal records | Fictional records with fake ID, name, and date of birth (all three must match); drive `no-record`, `record-found`, `delayed` outcomes | Government background-check service | `external/government-background-check/criminal-records.json` (created) |
| Market comparables | Past sale prices of rare and collectible items | Independent appraiser | `external/independent-appraiser/comparables` |
| Supplier catalogs | Products, wholesale prices, lead times, stock | Supplier | `external/supplier/catalog` |
| Supplier return policy | When a return is accepted or refused | Supplier | `external/supplier/return-policy` |
| Technical reference data | Data the external technical specialist uses for independent assessments | External technical specialist | `external/external-technical-specialist/` |

## Case Input (provided by the operator)

Not stored as shared records; it arrives with the case request. Fake values only.

- **Customer information**: name, fake ID document, date of birth, address (required for any gun with a full-auto mode), payment method.
- **Seller information** (used-item sales): name, fake ID, item description, ownership and provenance documents, maintenance history.
- **The request itself**: e.g., a quotation for a handgun paid in cash.

## Human Responses (mocked or entered by hand)

- **Inspection results** from the inspection technician: pass, needs work, reject, or inconclusive, with notes.
- **Operator answers** to information requests and escalations.

## Mock Limits and Thresholds

Arbitrary mock values; change them freely. Each value lives in **one** place, its rule (or data file), so change it there.

| Limit | Defined in |
|---|---|
| Installment credit limit ($5,000) and minimum credit score (650) | Rule `installment-limits` |
| Installment plans | `records/finance/installment-terms.json` |
| Purchasing budget per order ($20,000) | Rule `purchasing-budget` |
| Used-item offer ceiling (70% of the valuation midpoint) | Rule `used-item-offer-ceiling` |
| Valuation confidence threshold (0.75) | Rule `valuation-confidence` |
| Reservation expiry (72 hours) | Rule `reservation-expiry` |

## Test Identities

Fake customers the mock data knows about. Use them in case requests; each one triggers a known outcome.

| Customer | Fake ID | Date of birth | Background check (fictional) | Credit score |
|---|---|---|---|---|
| Jordan Test | `FAKE-ID-1001` | 1985-04-12 | `no-record` | 720 |
| Casey Sample | `FAKE-ID-1002` | 1990-09-30 | `record-found` | 690 |
| Morgan Mock | `FAKE-ID-1003` | 1978-01-05 | `no-record` | 580 (below 650) |
| Riley Demo | `FAKE-ID-1004` | 1995-07-21 | `delayed` | 760 |

Any other fake ID, or a listed ID with a different name or date of birth, returns `cannot-determine` from the background check. Unlisted IDs have no credit profile.

Stock worth knowing: `CAT-HG-002` (P45) is out of stock (its only item is sold), `CAT-HG-003` (MX9) and `CAT-RF-002` (AR Select, $7,800) have a full-auto mode, and `CAT-SG-001`'s only item is on hold awaiting inspection.
