# ÉgFigyelő


Automated personal flight deal finder. Searches current flight prices across flexible date ranges for a given trip length (`min_nights` to `max_nights`) or pulls open flight deals, ranks the cheapest available dates, and delivers results via Discord and Email.

### Source systems:

Api

 1. https://serpapi.com/google-flights-api


or Web-scraping

 2. [Regular Flight Search](https://www.google.com/travel/flights)
 3. [AI-Powered Flight Deals](https://www.google.com/travel/flights/deals)


## Core Goals

- **Track specific routes & date windows:** Monitor prices over time for watched origin → destination pairs, flexible dates, or trip lengths.
- **Surface opportunistic deals:** Pull broad/AI flight deals and flag genuine price drops against historical baselines.
- **Stay low-ops and self-hosted:** Run scheduled checks cleanly, store normalized snapshots locally/in a lightweight DB, and notify only when a real threshold is hit.
- **Source-agnostic ingestion:** Support **SerpApi Google Flights**, **Google Travel Flights**, and **Google Travel AI Flight Deals** behind a common data model.


## How It Works

```text
[config/watches.yaml]
         │
         ▼
┌─────────────────┐
│ Date & Rule Gen │  (Expands date range + min/max nights into candidate trips)
└────────┬────────┘
         ▼
┌─────────────────┐
│   Collectors    │  (SerpApi Google Flights / Flights Scraper / AI Deals Scraper)
└────────┬────────┘
         ▼
┌─────────────────┐
│ Filter & Rank   │  (Sorts by lowest price, picks Top N best dates)
└────────┬────────┘
         ▼
┌─────────────────┐
│  Deduplicator   │  (SQLite check to avoid re-sending identical alerts)
└────────┬────────┘
         ▼
┌─────────────────┐
│   Notifiers     │  (Discord Webhook + SMTP Email)
└─────────────────┘
