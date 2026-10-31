# My Monies

A personal cash-flow tracker for planning income, bills, transfers, and future account balances.

**[Open My Monies](https://my-monies-tool.netlify.app/)**

My Monies runs in your browser without an account or bank connection. Enter your balances and expected transactions to see how your money could change over time, spot potential shortfalls, and explore hypothetical purchases or income.

## Features

| Area | What it does |
| --- | --- |
| Dashboard | Shows account balances, upcoming transactions, and cash-flow projections. |
| Accounts | Tracks checking, savings, and credit cards separately, with a combined net-worth total. |
| Income and expenses | Supports recurring schedules and one-time transactions assigned to specific accounts. |
| Transfers | Moves money between accounts, including credit card payments and optional transfer fees. |
| Categories | Organizes income and expenses with sorting, filtering, and category archiving. |
| History | Shows projected monthly or yearly totals with expandable transaction details and CSV exports. |
| What if | Tests hypothetical income and expenses, checks for projected account shortfalls, and estimates when a target amount becomes available. |
| Backups | Exports and imports JSON data, with an optional password-encrypted export. |
| Display | Provides a responsive dark interface and adjustable text size. |

### Flexible recurring transactions

Income, expenses, and recurring transfers can repeat:

- Monthly on a selected day.
- Twice a month on two selected days.
- Every two weeks.
- Every 30 days.

Recurring items can end after a chosen number of occurrences or by a date. You can set a different first payment, adjust an individual occurrence's amount, move it to another date, or skip it. Optional business-day adjustments move scheduled dates earlier or later around weekends and the app's built-in US federal holiday calendar.

### Credit cards and interest

Enter a credit card's amount owed as a positive number in the Accounts screen. Internally, the app treats it as debt and subtracts it from the combined total.

Accounts support an annual interest rate and a monthly posting day. Interest is estimated using the annual rate divided by 12 and applied to the projected balance. This is a simplified model; actual bank and credit card calculations can differ.

## Getting started

1. Open the [live app](https://my-monies-tool.netlify.app/).
2. In **Accounts**, add the accounts you want to track and enter their balances.
3. Set the starting date in **Dashboard** so it matches the date represented by those balances.
4. Add paychecks or other expected income in **Income**.
5. Add recurring bills and one-time purchases in **Expenses**.
6. Use **Transfers** for movements between your own accounts.
7. Review **Dashboard**, **History**, and **What if** to explore your plan.
8. In **Accounts**, export a backup after entering your data.

Changes are saved automatically in the current browser.

## How projections work

The app builds a timeline from the starting date, account balances, scheduled transactions, transfers, and estimated interest.

- Income increases the assigned account's balance; expenses decrease it.
- A transfer removes the entered amount from the source account. The destination receives that amount minus any fee, with a minimum received amount of zero.
- Monthly dates beyond a month's last day are clamped to that month's last day.
- The History view summarizes generated transactions and projected balances. It is not a bank-imported record of confirmed transactions.
- History's category filter narrows the displayed transactions and totals; the ending balance still reflects the full account balance.
- The What if shortfall check excludes credit card accounts and compares baseline projections with hypothetical transactions.
- The affordability calculator searches up to two years ahead for the first date the selected balance meets the requested amount. It does not guarantee that spending that amount will leave enough money for later bills.

Projections depend on the information you enter. Update your plan when balances, bills, or income change. The dashboard's monthly-net figure is an approximate recurring-income-minus-expenses measure, rather than a complete cash-flow total.

## Data storage and privacy

Financial data is stored in the browser's `localStorage` under `ledger-budget-data`. The text-size preference uses `ledger-zoom`.

- There is no application login, backend database, automatic bank import, or built-in cross-device synchronization.
- Data is specific to the browser profile and website origin. A different browser, device, or site address has separate storage.
- Clearing site data can remove your entries. Private browsing is unsuitable for keeping a durable record.
- The stored browser data is not encrypted by the app. Password protection applies only to encrypted export files.
- Uploading the source code to GitHub or deploying it to Netlify does **not** back up your financial entries.

The application code has no external JavaScript dependencies or financial-data upload calls. Hosting providers still handle normal website requests.

## Back up and restore

In **Accounts**, choose:

- **Export data (.json)** for a readable, unencrypted backup.
- **Export encrypted (.json)** for a password-protected backup.
- **Import data (.json)** to restore either format.

**Import replaces the current dataset; it does not merge records. Export your current data before importing another file.**

Encrypted exports use the browser's Web Crypto API with AES-256-GCM and a password-derived key using PBKDF2-SHA-256, 150,000 iterations, a random salt, and a random initialization vector. Keep the password somewhere safe: the app has no password recovery.

Store backups outside browser storage. Do not commit financial backup files to this public repository.

### CSV reports

In **History**, use **Export summary (CSV)** or **Export all transactions (CSV)**. Exports follow the current monthly/yearly view and account/category filters. Select all accounts and all categories for an unfiltered report.

CSV reports are for analysis in spreadsheet software; use JSON exports to restore the app's data.

## Run locally

The entire application is in [`index.html`](index.html): HTML, CSS, and plain JavaScript. There is no package installation or build step.

Download the repository and open `index.html` in a modern browser for a quick look. For consistent browser storage and access to encryption features, serve it from localhost. For example, with Python installed:

```sh
git clone https://github.com/justinward108/my-monies.git
cd my-monies
python -m http.server 8000
```

Then open **http://localhost:8000**. Stop the server with **Ctrl+C**. Use the same address and port when returning to that dataset. Encrypted backups require a browser context that supports Web Crypto, such as HTTPS or localhost.

## Project structure

```text
my-monies/
├── index.html   # Complete browser application
└── README.md    # Usage and implementation guide
```

The current repository was recovered from the deployed Netlify application. It can be hosted as a static site without a build step. The live deployment and repository are separate unless automatic deployment is configured.

## Current limitations

- Transactions and balances are entered manually.
- Currency display uses a dollar sign; there is no currency conversion.
- Holiday adjustments use US federal holidays rather than a bank-specific calendar.
- Interest estimates do not model daily accrual or a lender's exact statement rules.
- Backward projections before the starting date reverse transactions without reversing compounded interest.
- Browser storage and manual exports are the only built-in persistence mechanisms.
