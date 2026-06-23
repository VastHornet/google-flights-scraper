[Google Flights Scraper](https://apify.com/scrape.badger/google-flights-scraper?fpr=data)

## What does Google Flights Scraper do?

Scrape [Google Flights](https://www.google.com/travel/flights) at scale — one-way, round-trip, and multi-city fares across all airlines, with cabin class, stops, and price filters.

## Why use Google Flights Scraper?

- **Every cabin class.** Economy, Premium Economy, Business, First.
- **One-way, round-trip, multi-city.** All Google Flights trip types.
- **Stops + airline filters.** Non-stop only, one-stop, or all; whitelist / blacklist airlines.
- **Currency + country targeting.** Local prices for your customer base.
- **Price insights.** When Google returns them — typical price range, lowest-price recent history.

## What data can Google Flights Scraper extract?

| Field | Type | Description |
| --- | --- | --- |
| result_group | string | `best` or `other` |
| price / currency | number / string | Total fare |
| airline | string | Primary carrier |
| flights | array | Per-segment info (origin / dest / times / aircraft) |
| duration | string | Total journey time |
| stops | number | Layover count |
| layovers | array | Layover airport + duration |
| carbon_emissions | object | Google's CO2 estimate in kg |

## How to scrape Google Flights

1. Click **Try for free**.
2. Enter `departure_id` (IATA code, e.g. `JFK`) and `arrival_id` (e.g. `LAX`).
3. Set `outbound_date` (and `return_date` if round-trip).
4. Optional: passengers (`adults`, `children`, …), `travel_class`, `stops`, `max_price`, airline filters.
5. Click **Start** — each flight offer streams into the dataset.

## How much will it cost?

**$0.012 per search (≈ $12 per 1,000 searches).** One call per origin-destination-date combo. Multi-city is still one call.

### Competitor benchmark

| Actor | Author | Price | Notes |
| --- | --- | --- | --- |
| kaizu/google-flights-scraper | kaizu | ~$15 / 1k searches | Most popular |
| credible_sandal/google-flights-scraper | credible_sandal | ~$18 / 1k searches | Subscription |
| scrapier/google-flights-scraper | scrapier | ~$20 / 1k searches | Per-result |
| **scrape-badger/google-flights-scraper** | **ScrapeBadger** | **$12 / 1k searches** | **20% below cheapest major** |

## Input

Configure the run in the **Input** tab above, or pass a JSON object matching the fields below when calling the Actor via the Apify API.

| Field | Required | Description |
| --- | --- | --- |
| departure_id | ✅ | IATA airport code (e.g. `JFK`). |
| arrival_id | ✅ | IATA airport code. |
| outbound_date | ✅ | `YYYY-MM-DD`. |
| return_date | Round-trip only | `YYYY-MM-DD`. |
| trip_type | — | `round_trip` (default) / `one_way` / `multi_city`. |
| adults / children / infants_in_seat / infants_on_lap | — | Passenger counts. |
| travel_class | — | `economy` / `premium_economy` / `business` / `first`. |
| stops | — | `any` / `nonstop` / `one_stop` / `two_stops`. |
| max_price | — | Upper price cap in the target currency. |
| currency / gl / hl | — | Currency + country + language (defaults `USD` / `us` / `en`). |

## Output

Every successful run streams records into the run's dataset. Download as JSON, CSV, XML, Excel, or HTML from the **Dataset** tab; consume programmatically via the Apify API or webhooks.

Example record:

```
{
  "result_group": "best",
  "price": 412,
  "currency": "USD",
  "airline": "Delta",
  "flights": [
    {
      "origin": "JFK",
      "destination": "LAX",
      "departure": "2026-12-01T08:00",
      "arrival": "2026-12-01T11:15"
    }
  ],
  "duration": "6h 15m",
  "stops": 0,
  "layovers": [],
  "carbon_emissions": {
    "this_flight": 203,
    "typical_for_this_route": 215
  }
}
```

## Tips / Advanced options

- **Run daily for fare tracking.** Prices change hourly — a cron gets you a price history for free.
- **Nonstop = 2-3x the price of one-stop.** Filter on `stops: nonstop` only when your customer specifically needs it.
- **`best` vs. `other`.** `best` is Google's ranked top offers (by convenience + price). `other` is everything else — cheaper but often with worse timings.
- **Carbon emissions for ESG reporting.** Use the `carbon_emissions.this_flight` kg figure for scope-3 tracking.

## FAQ, Disclaimers, Support

### What IATA codes do you support?

Every airport Google Flights supports (~8,000+). Major hubs + regional airports.

### Can I search by city?

Google accepts city codes too — `NYC` instead of `JFK` returns all NYC airports. The actor passes through whatever you provide.

### Does this include award / mileage fares?

No — Google Flights is cash-only. Use your airline's website for award availability.

### Multi-city?

Yes — set `trip_type: multi_city` and pass an array of legs via the SDK. Apify UI doesn't expose the array input directly; use the API.

### Disclaimer

This Actor scrapes public Google data only. You're responsible for compliance with Google's Terms of Service and any applicable data-protection laws (GDPR, CCPA, etc.) in your jurisdiction. ScrapeBadger does not store the scraped results — they are delivered directly to your Apify dataset.

### Support

Something not working? Open a ticket in the **Issues** tab above — we triage within one business day. Full API reference: [docs.scrapebadger.com](https://docs.scrapebadger.com).

### Related Actors

- [`google-hotels-scraper`](https://apify.com/scrape-badger/google-hotels-scraper) — Hotel search + details

### Powered by

[ScrapeBadger](https://scrapebadger.com) — Google-optimised residential proxy pool + browser-farm fallback, 99.7% uptime, unmetered bandwidth. No CAPTCHAs reach you.