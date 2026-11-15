# ShopLedger

ShopLedger is a small Express application for a grocery shop's point of sale, order history, inventory, customer credit (Khata), and GST reporting. The browser interface is served from `public/`; the API and business logic are in `routes/` and `lib/`.

## Run Locally

Requirements: Node.js 18 or newer.

```sh
npm install
npm start
```

Open `http://localhost:3000`. Set `PORT` to use another port. Run the in-memory characterization suite with:

```sh
npm test
```

## Features

- Create cash, UPI, and Khata orders with coupon discounts and HSN-based GST.
- Browse and filter orders, cancel an order, and open a printable HTML invoice.
- Review sellable stock, customer credit balances, and record Khata settlements.
- View dashboard figures and invoice-to-ledger GST reconciliation.

## Structure

- `server.js` — Express setup, static files, API routers, and invoice route.
- `routes/` — product, order, customer, and report/pricing endpoints.
- `lib/` — order, inventory, pricing, tax, coupon, ledger, invoice, report, date, and storage logic.
- `public/` — browser UI, JavaScript, and styles.
- `config.js` — shop details, tax slabs, coupons, and defaults.
- `data/db.json` — local JSON data store for products, orders, customers, settlements, and audit entries.
- `test/` — Node's built-in `node:test` characterization tests.
- `docs/architecture.svg` — application and order-flow diagram.

## API Overview

All JSON endpoints are mounted under `/api`. Main routes include `GET /products`, `GET /coupons`, `GET /orders` (supports `status`, `search`, `from`, and `to`), `POST /orders`, `GET /orders/:id`, `POST /orders/:id/cancel`, `GET /customers`, and `POST /customers/:id/settle`. Reporting endpoints are under `/reports`; cart preview is `POST /pricing/preview`. Printable invoices use `GET /orders/:id/invoice` outside the `/api` prefix.

## Data and Limitations

The app reads and writes `data/db.json` synchronously; it has no database migrations or multi-user transaction layer. Back up this file before running against important data. The current cancellation flow does not restore the stock deducted at order placement, and invoice figures can differ from stored order totals; the reconciliation report surfaces invoice differences. Configure secrets outside source control before exposing the app beyond a trusted local environment.
