# 📚 Accounting Concepts

A quick reference for the accounting principles used by this agent.

---

## Double-Entry Bookkeeping

Every transaction affects **at least two accounts**. The total of all debits must always equal the total of all credits.

```
DR (Debit)          CR (Credit)
──────────          ──────────
Assets ↑            Assets ↓
Expenses ↑          Liabilities ↑
                    Equity ↑
                    Revenue ↑
```

### Example: Vendor bill for office supplies ฿10,700 (incl. 7% VAT)

| Account | Code | Debit (DR) | Credit (CR) |
|---------|------|-----------|------------|
| Office Supplies Expense | 5200 | ฿10,000 | |
| Input VAT | 1170 | ฿700 | |
| Accounts Payable | 2000 | | ฿10,700 |
| **Total** | | **฿10,700** | **฿10,700** ✓ |

---

## VAT (Value Added Tax)

In Thailand, VAT is 7%. When you receive a vendor invoice:

- **Input VAT** (account 1170) = VAT you paid = Asset (you can claim this back)
- **VAT Payable** (account 2100) = VAT you collected from customers = Liability

```
Invoice total = Net amount + (Net × 7%)
Input VAT = Invoice total - Net amount
```

---

## Accounts Payable

When you receive a bill but haven't paid it yet, it goes to **Accounts Payable** (account 2000). This is a liability — you owe the vendor money.

When you pay:
```
DR Accounts Payable  ฿10,700
  CR Cash            ฿10,700
```

---

## Profit & Loss Statement

The P&L shows revenue minus expenses over a period:

```
Revenue
  Sales Revenue        ฿120,000
  Service Revenue      ฿ 36,000
  ─────────────────────────────
  Total Revenue        ฿156,000

Expenses
  Cost of Goods Sold   ฿ 38,200
  Utilities            ฿  8,750
  Professional Svc     ฿ 15,800
  ─────────────────────────────
  Total Expenses       ฿ 62,750

  ═════════════════════════════
  Net Profit           ฿ 93,250  (59.8% margin)
```

---

## Document Types

| Document | Accounting treatment |
|----------|---------------------|
| **Vendor Invoice / Bill** | DR Expense + Input VAT, CR Accounts Payable |
| **Purchase Order** | DR Inventory / Expense, CR Accounts Payable |
| **Receipt** | DR Expense, CR Cash (already paid) |
| **Credit Note** | Reversal of original entry |
| **Sales Invoice** | DR Accounts Receivable, CR Revenue + VAT Payable |
