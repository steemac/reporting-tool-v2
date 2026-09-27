# Ledger Line Reports — Version 2 (in development)

Started 27 September 2026 as a copy of Version 1. Version 1 stays untouched in its own folder.

Hosting: to be uploaded to a separate domain (not claude.ai). Links between the pages are relative (`index.html`, `guide.html`, `faq.html`), so the folder works on any host. `index.html` is now a complete HTML document (doctype, head, body).

## What changed in v2 (27 Sept 2026)
- **New layout, same branding.** The report dropdown is gone. There's a search box ("Find a report") and a row of heading chips on the card that overlaps the hero. Below that sits a sticky **Report library** sidebar grouped by heading. The home view is a catalogue of report cards, each showing where the same report lives in QuickFile. Opening a report shows a breadcrumb, a back link, the "In QuickFile: …" pointer, the filters, summary figures and the table. Reports can be linked directly with `#r=<report id>`.
- **23 new reports**, all rebuilt entirely from the backup. Reports the backup only partly supports are deliberately left out (cash-based P&L, ageing, cash-basis project reports, VAT, statement of cash flows).
  - Financial statements: Profit and loss; Segmented profit and loss (monthly/quarterly/yearly); Balance sheet; Trial balance; Income and expenditure breakdown.
  - Nominal ledger: Chart of accounts; Nominal ledger detail.
  - Sales and clients: Client list; Sales invoices; Outstanding invoices; Payments received; Client statement; Client monthly balance; Debtors at a date.
  - Purchases and suppliers: Supplier list; Purchase invoices; Outstanding purchases; Payments made; Supplier statement; Supplier monthly balance; Creditors at a date; Supplier payment run (batch payment report).
  - Products: Sales inventory.
- **Advanced** heading = all 16 Version 1 reports, unchanged, in 5 subheadings. They're marked "not available in QuickFile".
- New backup files read: `Sales_Receipt.csv`, `Purchase_Receipt.csv`, `Inventory_Items.csv` (in addition to the v1 set).
- Client and supplier names in the lists and monthly-balance reports link straight to that contact's statement.

## v2 build 2 (27 Sept 2026): Advanced split into areas, 17 more reports (56 in total)
Advanced now has 33 reports in 8 areas. Items marked (new) were added in this build.
- **Sales analysis:** sales invoice items, sales by client/item/nominal, quantity by item, income by year (calendar and custom year-end).
- **Purchase analysis:** purchase invoice items, purchases by supplier/nominal, expenditure by year (calendar and custom).
- **Customer and supplier insights (new):** Top clients, Top suppliers (ranking, share, first/last activity, months since), How quickly clients pay (average days from issue to final payment, % within terms).
- **Projects:** Project summary (new: income, costs, profit and margin per tag, all time), Sales by project, Profit by project.
- **Banking:** Bank account summary (new), Bank transactions export (new: all accounts together), Untagged bank transactions (new), Money in and out by month (new), plus the two existing bank balance reports.
- **Contacts and addresses (new):** Client address list, Supplier address list (address split into columns plus one-line version, for mail merge), Contact directory (clients and suppliers together), Full client export, Full supplier export (every column in the backup).
- **Team (new):** Team members (access level, added, last login), Team login history (IP address, browser and device).
- **Data checks (new):** Possible duplicate purchases (same supplier and amount within 0/3/7/14 days), Gaps in numbering (sales, credit note and purchase sequences, flagging numbers marked deleted).
- New backup files read: `Team_Members.csv`, `Team_Member_Logins.csv`. Bank statement lines are now kept in full for the export reports.
- Checked on the real backup: 24,427 bank lines and 911 untagged. Bank summary closing total £174,797.20 matches the Bank account balances report. 185 missing numbers and 12 same-day duplicate groups, both matching an independent pandas check. All 56 reports run with no errors.
- Data notes for this account: suppliers have no addresses, contacts or bank details saved; 6 of 12 clients have an address and email.

## v2 build 3 (27 Sept 2026)
- Advanced area headings restyled: each area is its own panel with a blue header bar, icon, description and count, and a row of jump buttons under the Advanced title. The sidebar area headings are tinted blue labels with icons.
- Loading progress: choosing a backup shows a branded "Preparing your reports" overlay. It has a progress bar, a percentage and 4 steps, and runs for at least 5 seconds (`LOADER_MS = 5000`) before the data appears. If processing takes longer, the bar holds at 96% until the data is ready. On an unreadable file it closes straight away and shows the error. Tested: data appears after about 5.5 s.

## v2 build 4 (27 Sept 2026): payment allocations (58 reports)
- New Advanced area **Payment allocations** with **Client payment allocations** and **Supplier payment allocations**, modelled on a customer's mock-up. Two layouts:
  - **Timeline:** invoices and payments in date order, each payment's allocation (colour marker → invoice #), and a running balance shown as "£X credit" when negative.
  - **By invoice:** each invoice, followed by the payments allocated to it and what's left to pay.
- **Marker rules:**
  - Colour markers link a payment to its invoice.
  - Bank icon: the payment matches a line on that bank account's statement on the same date (the receipt's BANKID = the Bank_<id>.csv file).
  - Split icon: only when several same-day allocations add up to ONE bank line and don't each match a line of their own.
  - "Paid before invoice" / "£X prepayment already mapped": when the payment date is earlier than the invoice date.
- **Real backup:** 460 of 465 sales payment groups match a bank line. There are no genuine split payments; every same-day multi-invoice group was separate bank lines. There are 2 sales prepayments (21 Charleston Court #000089; 44 Whitmore Court #000143) and 3 purchase prepayments. The example data includes a split payment (Harbour Cafe) and a prepayment (Northgate Properties) so the markers can be seen.

## How the figures are worked out
- Statements use `Nominal_Ledger.csv` (opening balances are already posted in it). QuickFile's code ranges: 0001–0999 fixed assets, 1000–1999 current assets, 2000–2299 current liabilities, 2300–2999 long-term liabilities, 3000–3999 capital; 4000–4999 income, 5000–5999 cost of sales, 6000–8999 expenses, 9000–9999 suspense (in P&L).
- Payments link to invoices through the `#ref` in each receipt's DETAILS text. "At a date" reports use the invoice gross less payments dated on or before that day.
- Deleted purchases are excluded everywhere.

## Verified (24 Sept 2026 backup)
- Balance sheet at 31 Aug 2026: net assets £186,532.66 = capital and reserves £186,532.66.
- Trial balance debits = credits (£1,271,560.18 for Sep 2025–Aug 2026). Closing debit and credit columns agree.
- P&L Sep 2025–Aug 2026: income £5,348.23, gross profit £3,136.60, net loss £49,197.27. Matches an independent pandas calculation.
- Outstanding sales £10.00 and purchases £307.71, which match the 1100/2100 control accounts. Receipts matched to invoices agree with PAID TO DATE on every invoice (0 mismatches).
- All 39 reports run without errors on both the example data and the real backup (jsdom). Screenshots checked at desktop width and 390px (no sideways scroll).

## Guide and FAQ (v2 build 5, 27 Sept 2026)
- `guide.html` has been rewritten for v2 in the v1 style (same nav, hero, fonts and the two QuickFile backup screenshots). It covers what the tool is, where the numbers come from, a 6-step how-to (including the loading bar and Export CSV), finding your way around, one section per heading, and each Advanced area in its own panel with how-to-read notes. Every report is listed with its description and its QuickFile location. The descriptions are taken straight from index.html so they match the tool exactly.
- `faq.html` has 24 questions in 5 groups (Getting started; Your data and privacy; Finding and using reports; Understanding the figures; About QuickFile). The old "no file download" answer is fixed.
- Both are full HTML documents with relative links only (no claude.ai links).
