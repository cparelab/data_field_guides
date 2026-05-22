# tidy-data-companion

A field guide for cleaning messy, real-world datasets. Upload a file, get back something analysis-ready — with a data dictionary and full audit trail included.

Built for practitioners who regularly receive data in poor shape and need to clean it properly, document what changed, and hand it to someone else without losing trust in the numbers.

---

## Before and after

**Before** — what typically arrives:

| Health Programme — Beneficiary Data 2023 (CONFIDENTIAL) | | | | | | |
|---|---|---|---|---|---|---|
| **Country Name** | **Region** | **Programme Code** | **# Beneficiaries (Total)** | **% Female** | **Funding USD** | **Status** |
| Kenya | AFR | WHP-001 | 12400 | 0.54 | 850000 | Active |
| Ivory Coast | AFR | WHP-003 | 5400 | 0.48 | 410000 | active |
| côte d'ivoire | AFR | WHP-003B | 3100 | 0.49 | 290000 | Active |
| Nigeria | AFR | WHP-007 | ~31,000 | 0.44 | 1.1M | ACTIVE |
| Palestine | EMR | WHP-008 | 4200 | 0.53 | 310000 | Active |
| Uganda | AFR | WHP-005 | | 0.57 | 530000 | Active |
| 🟡 Taiwan | WPR | WHP-010 | 2900 | 0.47 | 220000 | Active |
| TOTAL | | | =SUM(D3:D14) | | =SUM(F3:F14) | |
| *Data extracted from DHIS2 on 15 Jan 2024. Not for external distribution.* | | | | | | |

**After** — what the skill produces:

| country_original | country_iso3 | region | programme_code | beneficiaries_total | pct_female | funding_usd | status | data_quality_flag |
|---|---|---|---|---|---|---|---|---|
| Kenya | KEN | AFR | WHP-001 | 12400 | 0.54 | 850000 | Active | |
| Ivory Coast | CIV | AFR | WHP-003 | 5400 | 0.48 | 410000 | Active | |
| côte d'ivoire | CIV | AFR | WHP-003B | 3100 | 0.49 | 290000 | Active | |
| Nigeria | NGA | AFR | WHP-007 | 31000 | 0.44 | 1100000 | Active | Approximate — source used ~31,000 and 1.1M |
| Palestine | PSE | EMR | WHP-008 | 4200 | 0.53 | 310000 | Active | Contested entity — ISO3 PSE used per analyst instruction |
| Uganda | UGA | AFR | WHP-005 | | 0.57 | 530000 | Active | |
| Taiwan | TWN | WPR | WHP-010 | 2900 | 0.47 | 220000 | Active | Contested entity — To be verified |

*Title row, totals row, notes row, and colour-encoded meaning removed. Country names mapped to ISO 3166-1 alpha-3. Status casing normalised. Approximate values converted to integers. All changes logged in the `change_log` sheet.*

---

## What it does

You upload a file. The skill reads it, tells you what it found, asks only what it can't infer, then produces:

- **`cleaned_data.xlsx`** and/or **CSV files** — your choice at the start. The xlsx has three sheets: cleaned data, data dictionary, and change log. CSVs give you the same content in a format that travels better long-term.
- **`diagnostic_narrative.md`** — plain English summary of what was found and fixed, written for a non-technical reader. Paste into Word, Notion, or Google Docs.

It infers where it can. It flags where it can't. You stay in control of every structural decision.

---

## What it catches

- Multi-row headers and merged title cells
- Column names with special characters, spaces, or symbols
- Inconsistent country names — full names, abbreviations, ISO2 codes, French or local variants all in the same column
- Country standardisation to ISO 3166-1 alpha-3, with explicit handling of contested entities (Kosovo, Palestine, Taiwan, Western Sahara)
- Colour-encoded meaning converted to explicit columns
- Mixed data types — numbers stored as text, approximate values like `~31,000` or `1.1M`
- Inconsistent categorical casing (`Active`, `active`, `ACTIVE`)
- Embedded totals rows and notes rows sitting inside the data
- Date columns not in YYYY-MM-DD format
- Missing values — distinguishing between meaningful absence and data gaps
- Datasets that need reshaping rather than cleaning — flagged and handed back before anything is touched

---

## The foundations

Two papers underpin this skill:

**Wickham, H. (2014). Tidy Data.** *Journal of Statistical Software, 59*(10).
The structural framework — one observation per row, one variable per column, one value per cell. [Read it](https://www.jstatsoft.org/article/view/v059i10)

**Broman, K. W., & Woo, K. H. (2018). Data Organization in Spreadsheets.** *The American Statistician, 72*(1), 2–10.
The practical rules — consistent formatting, YYYY-MM-DD dates, no colour-encoded meaning, no calculations in raw data, always ship a data dictionary. [Read it](https://doi.org/10.1080/00031305.2017.1375989)

Worth reading both. They're short.

---

## How to use

### Option 1 — Claude skill

Add the `SKILL.md` file to your Claude skills directory:

```bash
mkdir -p ~/.claude/skills
git clone https://github.com/cparelab/data_field_guides ~/.claude/skills/data_field_guides
```

The skill activates when you upload a file and ask for help cleaning it.

### Option 2 — Paste into any AI assistant

The prompt works anywhere. If your organisation runs Microsoft 365 Copilot, ChatGPT, Gemini, or another assistant, open `SKILL.md`, copy everything below the second `---`, and paste it at the start of your conversation before uploading your file.

Useful if your environment restricts external AI tools or Claude isn't your default assistant.

---

## Scope

- Works best with single-sheet or simple multi-sheet Excel files and CSVs
- Very large files (50,000+ rows) may hit context limits depending on your assistant
- Cleans and documents — does not impute missing values or make analytical decisions
- ISO3 mapping covers standard UN member states; contested entities always flagged for analyst input

---

Built by [cparelab](https://github.com/cparelab) — field guides for data practitioners who want to do the work properly.
