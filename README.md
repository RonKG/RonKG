# Ronald Wahome

Nashville, TN · [wahomeron@gmail.com](mailto:wahomeron@gmail.com) · [linkedin.com/in/ronaldwahome](https://www.linkedin.com/in/ronaldwahome/)

Most of this work is in Kenya. Markets like it have not had the digitization Western economies already run on, so the value still sits in a scan, a register, or a listing nobody can trust. An economy can only use what it can read. The goal is to unlock that value in a way that belongs to the market itself. Kenya is the case study, carried far enough to show what opens.

Four projects from the last year, all my own. The source is private. I can walk through the design on a call.

## BRIA — business risk checks for Kenyan companies

Banks, fintechs, and law firms need a faster read on whether a Kenyan company is risky to onboard, short of a full manual due-diligence file. BRIA answers that from public records: court decisions, gazette notices, regulatory registers, and news. Every flag cites the record it came from.

I ran it as a sequence of deliveries. Pipelines land regulatory files, gazette extracts, court records, and news in Postgres, in a form that can be rerun. Where the source is a scan or a block of prose, a model does the reading: OCR on gazette pages, parties and outcomes pulled from court text, news marked adverse or not. The extract stays tied to the source row. Names are then resolved into companies and people, and only then does someone get a result: an API call, a PDF that can sit in an onboarding file, or a lookup on the dashboard. A stage had to check out before the next one started.

What is live:

- A REST API for company risk lookups
- PDF risk reports
- An internal lookup dashboard
- A marketing site and live demo at [bria.co.ke](https://bria.co.ke)

**Stack:** Python, FastAPI, PostgreSQL, Streamlit, Cloudflare, Railway

**Results I will talk through:**

- About 100,000 companies and people resolved from those public sources
- 98% agreement with a labeled entity-resolution sample (49 of 50)
- A 20-company end-to-end check that matched the expected result in every case, with no false positives or false negatives on that set

Source is private. The demo on the site is the public view.

## Kenya Gazette parsing

The Kenya Gazette is a scanned public record. Useful notices (winding-up, director changes, procurement) are locked in PDFs. This is the ingestion step: an issue goes in, and structured notices come out that a later product can store and cite.

Two paths, chosen by the scan. A layout model reads a cleaner PDF into a validated set of notices and scores its own confidence. A heavier OCR path handles the rest, keeps the raw model output as the audit copy, and writes a cleaned notice bundle from it. Both leave through the same JSON contract, so the load downstream does not care which model read the page. BRIA is the main consumer. The parsers also stand on their own.

**Stack:** Python, Docling, Mistral OCR, JSON Schema

Source is private. I can walk through the document contract and a sample issue on a call.

## Kenya real-estate market signals

A neighborhood-level read on listing prices in Kenya. The question came first: is the series stable enough to say whether prices are rising or falling in a given area.

About 312,000 listings from April through December 2025 went into Postgres. Almost all of them had a price. About 1.4% were dropped because the price was implausible, and that rule was fixed before any trend was drawn. Neighborhoods with 500 or more listings, about 50 of them, are the ones thick enough to track a typical price. Thinner markets stay in the set, with a wider band of doubt.

The finding is the deliverable. Over that window, typical prices across the market rose about 38%. Eight of the ten most active neighborhoods rose. Kilimani, Westlands, and Kileleshwa were the deepest markets. The chart sits behind that sentence.

**Stack:** PostgreSQL, Python, Jupyter, Plotly

Source is private. I can walk through the cleaning rules and the neighborhood cuts on a call.

## The Registry

A product site for a curated Kenya property listing, aimed at buyers who have to judge a home from abroad. This one is about how an insight gets shown. A listing, a density map of Nairobi, and a short market note have to be readable by someone who will not open the notebook.

The pages are built and deployed. The figures on them are samples, used to test whether the insight is readable. The measured price work is the study above.

**Stack:** Next.js, React, Vercel

Live site: [verity-site-iota.vercel.app](https://verity-site-iota.vercel.app)

## Porsche market notes

I was shopping for a Porsche with $60,000 all-in for the winning bid and the auction fee, and I wanted a 911 Carrera S or better, or a six-cylinder Cayman. The auction sites make that search misleading. A base Carrera, a flat-4 Cayman, and a GT car all show up in the same list, and a live bid is not a sale.

So I wrote the rules down first, then priced only the cars that passed them. The budget is the hammer plus the Cars & Bids buyer fee (5%, capped), which puts the real bid ceiling near $57,100. Tax, title, and shipping sit outside that. Sold hammers went into a fixed schema. Live bids stayed out of the medians, because a car that has not sold is not a comp. Each pass is a dated snapshot with the older files left in place, so the next pull is a comparison.

The 4 October 2026 Cars & Bids snapshot is the one in the repo. The car that fits is a 2014–2016 Cayman S: median hammer $44,000, and 19 of 21 recent sales were inside the cap. A manual costs more (median $54,750) and a PDK less (median $38,250). A sorted 991 Carrera S is a different purchase. Coupes in that set median $72,250.

**Stack:** Markdown reports, JSON snapshots, Cars & Bids sold results

Repo: [p-car-research](https://github.com/RonKG/p-car-research)
