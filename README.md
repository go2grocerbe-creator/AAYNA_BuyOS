# AAYNABuyOS MVP

A lean Bangladesh-focused internal sourcing and launch-prep tool for AAYNA, built from the Product Scout workbook logic. AAYNABuyOS helps the team manually scout, score, approve, reject, order, QC, batch for launch, and prepare women's accessories products for website upload.

This app does not scrape SkyBuyBD, AliExpress, 1688, or any sourcing site. It does not bypass login, CAPTCHA, or anti-bot systems. It does not auto-order products. It is a human decision-support tool only.

## Tech Stack

- Frontend: HTML, CSS, and vanilla JavaScript
- Local development: Vite
- MVP storage: Browser `localStorage`
- Export: Client-side CSV and JSON downloads
- Future AI/API work: Add a backend before using API keys

## Current Storage

Product candidates are stored in browser `localStorage` under `aaynaProducts`.
Settings are stored in browser `localStorage` under `aaynaSettings`.

Saved data uses a simple schema marker:

```text
app: AAYNABuyOS
schemaVersion: 3
```

This is suitable for a single-device MVP only. Clearing browser data will remove saved products unless you export a backup first.

## BuyOS v0.3 Adds

- Launch Batch section for approved products
- Launch status workflow:
  - shortlisted
  - sample_order
  - ordered
  - received
  - photo_ready
  - website_ready
  - live
  - paused
- Launch readiness checklist:
  - Product name
  - Description
  - Price confirmed
  - Images
  - Stock confirmed
  - Website export
  - Instagram content
- Dashboard metrics for launch batch, website-ready/live products, and photo/content pending products
- Formula-safe CSV export helper
- Internal approved/rejected CSV export
- Website-safe CSV export
- Full BuyOS JSON backup and restore
- v0.3.1 website CSV stock hardening so Stock Quantity is never blank
- v0.3.2 website CSV duplicate hardening so duplicate SKU/slug/product rows are skipped at export time

## Launch Batch Workflow

The Launch Batch shows approved products only. A product enters the Launch Batch when it is approved, using the existing approval meaning in the app. Newly approved products default to `shortlisted`.

Use the Launch Batch to move products through:

```text
Approved product -> shortlisted -> sample order -> ordered -> received -> photo ready -> website ready -> live
```

Checklist changes and launch status changes save immediately to `localStorage`.

## Internal CSV vs Website CSV

The internal approved/rejected CSV may include sourcing and business fields such as supplier/source URL, unit cost, shipping, fees, landed cost, purchase cost, profit, margin, notes, override reason, launch status, and launch checklist.

The website CSV is intentionally safer. It does not export supplier/source URLs, unit cost, shipping, fees, customs, landed cost, purchase cost, profit, margin, internal notes, override reason, sourcing notes, or QC notes.

Website CSV only includes customer/product-upload-safe fields:

- SKU
- Slug
- Product name
- Category
- Selling price
- Stock quantity
- Image URL
- Public status
- Short description

Stock Quantity is always exported as a number. The app uses the best available public stock value and falls back to `0` when stock is missing. If any website-ready products would export with stock `0`, the app shows a warning listing the affected products/SKUs before downloading the CSV.

Website CSV export also deduplicates rows at download time. It keeps the first matching product and warns about skipped duplicates using SKU first, then slug, then product id, then normalized product name + category + selling price. This does not delete or merge products inside BuyOS.

All CSV exports quote fields, escape double quotes, preserve commas/newlines, and prefix formula-like values that start with `=`, `+`, `-`, or `@`.

## Backup and Restore

Use `Export BuyOS backup JSON` to download a full workspace backup containing products, settings, launch status, launch checklist, and schema metadata.

Use `Import BuyOS backup JSON` to restore a backup. The app validates the file first and asks for confirmation before replacing current local data.

Malformed JSON or invalid backup files should show an error and must not wipe current data.

## Core Workflow Features

- Product Tracker-style candidate form
- Workbook-style cost calculations
- Weighted AAYNA 10-criteria scoring
- Buy / Maybe / Price Review / Reject decision rules
- Clear computed decision reasons
- Validation for required fields, source URL, image URL, cost, price, quality scores, ratings, and quantities
- Approval override reason required before approving products rejected by status or decision
- Product actions: approve, reject, watchlist, edit, delete, mark ordered, mark arrived, mark website ready
- Website-ready approved product CSV export
- Approved/rejected internal CSV export
- Dashboard KPIs for product counts, budget, website readiness, high-risk products, partner review, and launch readiness
- Settings for exchange rates, markup, price limits, budget, low-profit threshold, MOQ warning threshold, customs, and misc fee

## Internal Data Note

This MVP is an internal AAYNABuyOS workspace. It shows supplier names, supplier/source URLs, landed cost, purchase cost, profit margin, QC notes, and internal decision notes inside the admin workflow.

There is no public customer storefront in this repo. If a public customer page is added later, do not expose supplier cost, supplier URL, sourcing links, internal notes, margin notes, or cost-price fields there.

## Local Setup

```bash
npm install
npm run dev
```

Then open the local URL printed by Vite, usually:

```text
http://127.0.0.1:5173
```

## Manual Test Checklist

1. Load workbook samples.
2. Approve at least one product.
3. Confirm approved products appear in Launch Batch.
4. Change launch status.
5. Tick launch checklist items.
6. Refresh the browser and confirm launch status/checklist persisted.
7. Export website CSV and confirm Stock Quantity is not blank.
8. Confirm website CSV warns when website-ready products have missing or zero stock.
9. Create or duplicate a website-ready product with the same SKU, export website CSV, and confirm only one row appears for that SKU.
10. Confirm website CSV warns about skipped duplicate products/SKUs.
11. Confirm website CSV does not include source URL, supplier URL, unit cost, landed cost, purchase cost, profit, margin, internal notes, override reason, or QC notes.
12. Export approved/rejected CSV and confirm internal fields are preserved there.
13. Export BuyOS backup JSON.
14. Import the backup JSON and confirm products, settings, launch status, and checklist restore.
15. Try importing invalid JSON and confirm current data remains unchanged.
16. Try deleting a product and confirm the delete prompt appears.

## Environment Variables

Copy `.env.example` to `.env` when you add a backend later:

```bash
cp .env.example .env
```

Do not put real AI API keys in frontend code. Browser users can inspect frontend bundles, so OpenAI, Anthropic, Gemini, and similar keys must stay on a server.

## Next After MVP

- Replace localStorage with a shared database
- Add authentication and role-based approval permissions
- Add image uploads and product photo management
- Add backend-only AI scoring
- Add audit history for score changes, approvals, orders, arrivals, launch checklist changes, and QC
- Add supplier comparison and purchase planning reports
- Add website CMS integration for pushing approved product data
