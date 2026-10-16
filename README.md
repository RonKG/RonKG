# Ronald Wahome

Nashville, TN · [wahomeron@gmail.com](mailto:wahomeron@gmail.com) · [linkedin.com/in/ronaldwahome](https://www.linkedin.com/in/ronaldwahome/)

I build data products from public records and market data. The four projects below are my own work from the last year. The source is private. I can walk through the design on a call.

## BRIA — business risk checks for Kenyan companies

Banks, fintechs, and law firms need a faster read on whether a Kenyan company is risky to onboard, short of a full manual due-diligence file. BRIA answers that from public records: court decisions, gazette notices, regulatory registers, and news. Every flag cites the record it came from.

What is live:

- A REST API for company risk lookups
- PDF risk reports
- An internal lookup dashboard
- A marketing site and live demo at [bria.co.ke](https://bria.co.ke)

Public sources are collected, resolved into companies and people, then turned into a cited risk result. The API, reports, and site sit on top of that.

**Stack:** Python, FastAPI, PostgreSQL, Streamlit, Cloudflare, Railway

**Results I will talk through:**

- About 100,000 companies and people resolved from those public sources
- 98% agreement with a labeled entity-resolution sample (49 of 50)
- A 20-company end-to-end check that matched the expected result in every case, with no false positives or false negatives on that set

Source is private. The demo on the site is the public view.

## Kenya Gazette parsing

The Kenya Gazette is a scanned public record. Useful notices (winding-up, director changes, procurement) are locked in PDFs. I built parsers that turn an issue into structured notices a downstream product can store and cite.

Two paths, used for different scan qualities:

- A layout parser that reads a PDF into a validated set of notices, with a confidence score on the document
- An OCR pipeline for scans, which keeps the raw OCR output as the audit copy and writes a cleaned notice bundle from it

BRIA consumes this output. The parsers also stand on their own.

**Stack:** Python, Docling, Mistral OCR, JSON Schema

Source is private. I can walk through the document contract and a sample issue on a call.

## Kenya real-estate market signals

A neighborhood-level read on listing prices in Kenya, built to answer whether the data is stable enough to say if prices are rising or falling in a given area.

About 312,000 listings from April through December 2025. Almost all of them had a price. About 1.4% were dropped because the price was implausible. That left 50 neighborhoods with 500 or more listings, which is enough to track a typical price, and another set of neighborhoods with a thinner but usable count.

Over that window, typical prices across the market rose about 38%. Eight of the ten most active neighborhoods rose. Kilimani, Westlands, and Kileleshwa were the deepest markets.

**Stack:** PostgreSQL, Python, Jupyter, Plotly

Source is private. I can walk through the cleaning rules and the neighborhood cuts on a call.

## The Registry

A product site for a curated Kenya property listing, aimed at buyers who have to judge a home from abroad. The promise is verification plus presentation: listings, a market heatmap, and short market notes, in one Next.js app deployed on Vercel.

The pages are built and deployed. Listings on the site are samples, used to show the presentation, not a live inventory.

**Stack:** Next.js, React, Vercel

Live site: [verity-site-iota.vercel.app](https://verity-site-iota.vercel.app)

## Porsche market notes

I was shopping for a Porsche with $60,000 all-in for the winning bid and the auction fee, and I wanted a 911 Carrera S or better, or a six-cylinder Cayman. The auction sites make that search misleading. A base Carrera, a flat-4 Cayman, and a GT car all show up in the same list, and a live bid is not a sale.

So I wrote the rules down first, then priced only the cars that passed them. The budget is the hammer plus the Cars & Bids buyer fee (5%, capped), which puts the real bid ceiling near $57,100. Tax, title, and shipping sit outside that. Sold prices and live bids stay in separate files, because a car that has not sold is not a comp. Each pass is a dated snapshot: the same fields, the older files left in place, so the next pull can say what changed.

The 4 October 2026 Cars & Bids snapshot is the one in the repo. The car that fits is a 2014–2016 Cayman S: median hammer $44,000, and 19 of 21 recent sales were inside the cap. A manual costs more (median $54,750) and a PDK less (median $38,250). A sorted 991 Carrera S is a different purchase. Coupes in that set median $72,250.

**Stack:** Markdown reports, JSON snapshots, Cars & Bids sold results

Repo: [p-car-research](https://github.com/RonKG/p-car-research)
