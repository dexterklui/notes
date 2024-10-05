---
date: 2024-09-29 (Sun)
---

# Hledger

## Common Commands

- `hledger add` adds a transaction
- `hledger print` prints the journal
- `hledger register` prints the register
- `hledger balance` prints the balance
- `hledger bs` - balance sheet
- `hledger is` - cash flow statement

Reports can filter by different criteria:

- `type:A` filters for account type A (assets)
  - `A` or `Asset`
  - `L` or `Liability`
  - `E` or `Equity`
  - `R` or `Revenue`
  - `X` or `Expense`
  - `C` or `Cash` - a subtype of Asset, indicating **liquid assets** (cash, bank
    accounts)
  - `V` or `Conversion` - subtype of Equity, for conversions
- `assets liabilities` filters for accounts "assets" and "liabilities"
- `desc:food` filters with description containing food
- `date:2024-09-29` filters with date
- `payee:amazon` filters with payee

## 🧭 Navigation

- [🔼 Back to top](#hledger)
- [◀️ Back](../index.md)
- [🔖 Parent index](../index.md)
- [📑 Notes Index](../index.md)
