# Firearm Store Test Scenarios

Runs that exercise the firearm-store context, each with a known expected result. Test identities and stock are described in `mock-data.md`.

## How to Run

1. Start each run with a clear context: `/clear`.
2. Paste the scenario's command.
3. Answer as the operator when the dispatcher asks.
4. Compare the case response with **Expected**, then audit the case folder (`data/firearm-store/cases/<case-id>/`) with the checklist at the end.

Runs change the shared records (reservations, item status, ledger). Note the stock state before each run; see [Stock Notes](#stock-notes).

## Scenarios

### 1. Happy path

```text
/run-case firearm-store Jordan Test (FAKE-ID-1001, born 1985-04-12) wants a quotation for the Mockworks M9 Compact, paying cash
```

**Expected:** `deliverable`: a quote for $549, cash, valid 72 hours, reserving an available M9.
**Also checks:** finance and inventory delegate to their specialists (`delegation` checks pass), and the dispatcher keeps the bookkeeping without scripts.
**Optional:** answer "Continue, the customer paid" to test payment → delivery → transaction report → ledger entry and item `sold`.

### 2. Out of stock

```text
/run-case firearm-store Jordan Test (FAKE-ID-1001, born 1985-04-12) wants a quotation for the Mockworks P45 Full Size, paying cash
```

**Expected:** `blocking-findings`: out of stock (the only P45 is sold).

### 3. Background check finds a record

```text
/run-case firearm-store Casey Sample (FAKE-ID-1002, born 1990-09-30) wants a quotation for the Mockworks R15 Carbine, paying by card
```

**Expected:** `blocking-findings`: `check-failed`, labeled fictional, with no eligibility wording.

### 4. Full-auto gun without an address

```text
/run-case firearm-store Jordan Test (FAKE-ID-1001, born 1985-04-12) wants a quotation for the Mockworks MX9 Select-Fire, paying cash
```

**Expected:** `information-needed`: the customer's address (rule `full-auto-requires-address`). Answer with any fake address; the case resumes and ends in a `deliverable` quote for $2,450.

### 5. Credit score below the installment minimum

```text
/run-case firearm-store Morgan Mock (FAKE-ID-1003, born 1978-01-05) wants a quotation for the Mockworks R15 Carbine, paying in installments
```

**Expected:** no installment plans offered (score 580 < 650, rule `installment-limits`); the response is blocking findings or offers cash/card instead.

### 6. Installments above the credit limit

```text
/run-case firearm-store Jordan Test (FAKE-ID-1001, born 1985-04-12, address 1 Mock Street, Testville) wants a quotation for the Mockworks AR Select, paying in installments
```

**Expected:** $7,800 exceeds the $5,000 limit → escalation to the general manager → (no delegated exceptions) a human request to the operator for approval.

### 7. Background check delayed

```text
/run-case firearm-store Riley Demo (FAKE-ID-1004, born 1995-07-21) wants a quotation for the Mockworks M9 Compact, paying cash
```

**Expected:** `check-delayed`; likely a human request for judgment (guide `human-review-triggers`) or blocking findings.

### 8. Identity mismatch

```text
/run-case firearm-store Alex Wrong (FAKE-ID-1001, born 2000-01-01) wants a quotation for the Mockworks M9 Compact, paying cash
```

**Expected:** the background check returns `cannot-determine` (the ID exists but the name and date of birth don't match); compliance doesn't clear.

## Stock Notes

- The M9 Compact (`CAT-HG-001`) had two units; check `records/inventory/items.json` for how many are still `available`. Scenarios 1, 7, and 8 all request it, and each successful quote reserves one unit for 72 hours.
- Completing a sale (scenario 1, optional step) marks the unit `sold` and adds a ledger entry.

## Audit Checklist

After each run, in the case folder:

- [ ] The case response matches **Expected**.
- [ ] `events.jsonl` logs every task, check, state change, and decision, and ends with `case-closed` once you close the case.
- [ ] `tasks.json` respects dependencies, and independent tasks ran in parallel.
- [ ] Each file was written only by its owner: specialists wrote only their `output.json`; only orchestrators changed `state/` and records.
- [ ] Every record change cites a specialist or external-party output as its source (`delegation` checks pass).
- [ ] No invented data: every value traces back to the mock data or the operator's request.
- [ ] Background-check results are labeled fictional everywhere they appear.
