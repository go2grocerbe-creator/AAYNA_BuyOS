# AAYNA Product Scout Lite

A 6-tab Google Sheets / Excel workbook for tracking and scoring product sourcing
candidates from SkyBuyBD, AliExpress, or other China sourcing platforms against
AAYNA's specific brand criteria.

## Files

- `AAYNA_Product_Scout_Lite.xlsx` — the workbook. Open in Google Sheets
  (Drive → Upload → Open with Google Sheets, or File → Save as Google Sheets)
  or Excel/LibreOffice.
- `build_sheet.py` — the generator script (Python + openpyxl). Re-run it to
  regenerate the workbook from scratch after editing column definitions,
  formulas, or styling in code.

## Tabs

1. **Product Tracker** — one row per candidate product. White columns are
   manual input; colored columns calculate automatically. Scores 10 criteria
   (Feminine, Trend, AAYNA Aesthetic Fit, Easy to Style, Lightweight,
   Reels/Photo Potential, Giftability, Price Fit, Demand, Quality/Risk) into a
   Total Product Score out of 100, then applies hard rejection rules: price
   over BDT 700 forces "Price Review" (or "Reject" if it's far over), and a
   Quality/Risk or Aesthetic Fit score of 1-2 caps the decision at "Maybe"
   even if the total score is high. Includes 5 demonstration rows, one for
   each decision branch.
2. **Dashboard** — read-only live overview: totals by decision, Top 10
   highest scoring products, best products under BDT 700, best for
   reels/photos, best giftable products, and everything still awaiting
   partner review.
3. **Claude Scoring Prompt** — a ready-to-copy prompt for getting an AI first
   opinion on a product's 10 scores, recommended decision, and a content/reel
   idea, formatted to paste straight back into the Product Tracker.
4. **Settings** — exchange rates, default fees/markup, price ceilings,
   scoring weights, and Buy/Maybe/Reject thresholds. Change a number here
   once and every formula on the Product Tracker tab updates.
5. **Dropdown Lists** — the option lists backing the dropdowns (platform,
   category, currency, decision, approval status, 1-5 scores).
6. **Instructions** — beginner-friendly, step-by-step usage guide for a
   3-partner team.

## Regenerating

```bash
pip install openpyxl
cd sheets && python3 build_sheet.py
```
