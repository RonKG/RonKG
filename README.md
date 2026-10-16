# Ronald Wahome

Nashville, TN · [wahomeron@gmail.com](mailto:wahomeron@gmail.com) · [linkedin.com/in/ronaldwahome](https://www.linkedin.com/in/ronaldwahome/)

Most of this work is in Kenya. Markets like it have not had the digitization Western economies already run on, so the value still sits in a scan, a register, or a listing nobody can trust. An economy can only use what it can read. The goal is to unlock that value in a way that belongs to the market itself. Kenya is the case study, carried far enough to show what opens.

These are the five projects from the last year I want to highlight, all my own. There is more in my repositories, much of it private. Where the source is private, I can walk through the design on a call.

## BRIA — business risk checks for Kenyan companies

Banks, fintechs, and law firms need a faster read on whether a Kenyan company is risky to onboard, short of a full manual due-diligence file. BRIA answers that from public records: court decisions, gazette notices, regulatory registers, procurement debarments, and news. Every flag cites the record it came from.

Pipelines land each source in Postgres and can be rerun without duplicating rows. Raw pages and PDFs stay in object storage, so a parse can be redone from the original. Where the source is a scan or a block of prose, a model does the reading: OCR on gazette pages, parties and outcomes from court text, severity on news. News classification runs under a spend cap.

Models fill fields. They do not decide the score. The score comes from a rules engine with versioned weights, and each flag's plain-English sentence is written from the same structured result. When a company's score moves, the history records why: new information, a weights change, time decay, or a change in which records were matched to the company. The API serves that result. The PDF report and the dashboard are built from the same response, so the three cannot disagree.

I ran it in gated phases: data, then matching and scoring, then product. A phase had to pass a written check before the next one started, and each feature began as a written spec.

What is live:

- A REST API for company risk lookups
- PDF risk reports
- An internal lookup dashboard
- A site and live demo at [bria.co.ke](https://bria.co.ke)

**Stack:** Python, FastAPI, PostgreSQL, Cloudflare R2, Streamlit, Railway, OpenAI and Mistral models

**Results I will talk through:**

- About 247,000 entities scored in one full batch run
- 98% accuracy on a labeled entity-matching sample (49 of 50)
- 20 of 20 known companies scored as expected end to end, with no false positives or false negatives on that set

The demo on the site is the public view.

## Kenya Gazette parsing

The Kenya Gazette is a scanned public record. Useful notices (winding-up, director changes, procurement) are locked in PDFs. These parsers are BRIA's ingestion step: an issue goes in, and structured notices and tables come out that a later product can store and cite.

I built them as two standalone Python packages first, then brought them into BRIA as dependencies. Mistral OCR is the default reader. A Docling layout parser is the fallback, and it scores its own confidence on each document. The raw OCR response is kept as the audit copy, so a parser fix can be replayed on the stored response without paying to read the page again. The load into Postgres is idempotent, so running an issue twice leaves the same notices.

The first live issue produced 298 notices and 32 tables, at an estimated OCR cost of about six cents. Further live runs went in approved batches of ten or fewer.

**Stack:** Python, Mistral OCR, Docling, Pydantic, JSON Schema, Cloudflare R2

## Kenya real-estate market signals

A neighborhood-level read on listing prices in Kenya. The question came first: is the data stable enough to say whether prices are rising or falling in a given area.

About 312,000 listings from April through December 2025, pulled from Postgres and profiled before any trend was drawn: nulls, types, coverage, and the price distribution by percentile. The cleaning rule came out of that profile. Prices outside KES 100,000 to 500 million were dropped, about 1.4% of rows, and the band is written into the analysis so a reload makes the same cut. Medians, not means, because a few large listings would move an average. A neighborhood needed 500 listings before its weekly median was read as a trend. About 50 cleared that bar.

The finding is the deliverable. Over that window, typical prices across the market rose about 38%. Eight of the ten most active neighborhoods rose. Kilimani, Westlands, and Kileleshwa were the deepest markets. The chart sits behind that sentence.

**Stack:** PostgreSQL, Python, pandas, Jupyter, Plotly

## The Registry

A product site for a curated Kenya property listing, aimed at buyers who have to judge a home from abroad. The engineering here is the front end: a Next.js app with a listing view, an interactive density map of Nairobi, and charts for a short market note. The pages have to be readable for someone who will not open a notebook.

The figures on the site are sample data. The measured price work is the study above.

**Stack:** Next.js, React, Leaflet, Recharts, Vercel

Live site: [verity-site-iota.vercel.app](https://verity-site-iota.vercel.app)

## Porsche market notes

I was shopping for a Porsche with $60,000 all-in for the winning bid and the auction fee, and I wanted a 911 Carrera S or better, or a six-cylinder Cayman. Auction search makes that misleading. A base Carrera, a flat-4 Cayman, and a GT car all show up in the same list, and a live bid is not a sale.

So I wrote the rules down first, then priced only the cars that passed them. The budget is the hammer plus the Cars & Bids buyer fee (5%, capped), which puts the real bid ceiling near $57,100. Tax, title, and shipping sit outside that. The snapshot is a JSON file with fixed fields, and sold results and live bids are kept apart, so a bid cannot land in a median. Each pass is dated and the older files stay, so the next pull is a comparison against the same fields.

The 4 October 2026 Cars & Bids snapshot is the one in the repo. The car that fits is a 2014–2016 Cayman S: median hammer $44,000, and 19 of 21 recent sales were inside the cap. A manual costs more (median $54,750) and a PDK less (median $38,250). A sorted 991 Carrera S is a different purchase. Coupes in that set median $72,250.

**Stack:** JSON snapshots, Markdown reports

Repo: [p-car-research](https://github.com/RonKG/p-car-research)
