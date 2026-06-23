[Google Flights Scraper](https://apify.com/openclawai/google-flights-scraper?fpr=data)

# ✈️ Google Flights Scraper — Real-Time Prices, Itineraries & Carbon Data

Scrape **live Google Flights data** without an API key. Get prices, airlines, durations, layovers, plane types, departure & arrival times, and carbon emissions for any route, any date, any cabin class.

> Built for travel sites, fare aggregators, corporate travel platforms, and anyone tired of paying $1,000/mo for a flight API.

## ⚡ Why this actor

- **Real-time data** — every run hits Google Flights live, not a cached snapshot
- **No API key** — zero setup, zero quota anxiety
- **Full itinerary detail** — multi-segment legs, plane types, exact times, carbon emissions
- **All cabins** — economy, premium economy, business, first
- **All trip types** — one-way, round-trip, multi-city
- **Pay-per-itinerary** pricing — only pay for results you get

## 💰 Use cases

| Buyer | Use case |
| --- | --- |
| 🏢 Corporate travel | Daily fare monitoring on key routes for policy compliance |
| 📰 Travel media | Build "cheapest day to fly" content & price comparison tables |
| 🤖 AI travel agents | Real-time fare lookup for chatbot/assistant pipelines |
| 📊 Pricing analytics | Track airline pricing strategy & seasonality |
| 💡 Indie SaaS | Power your travel-deal alerts product without a Skyscanner contract |

## 🚀 Quick example

**Input:**

```
{
    "from_airport": "JFK",
    "to_airport": "LAX",
    "date": "2026-06-15",
    "trip": "one-way",
    "seat": "economy",
    "adults": 1,
    "max_results": 50
}
```

**Output (one itinerary):**

```
{
    "from_airport": "JFK",
    "to_airport": "LAX",
    "date": "2026-06-15",
    "trip": "one-way",
    "airline_codes": ["B6"],
    "airline_names": ["JetBlue"],
    "price": 149,
    "stops": 0,
    "duration_min_total": 353,
    "departure": "2026-06-15T06:00:00",
    "arrival": "2026-06-15T08:53:00",
    "segments": [
        {
            "from_airport": {"code": "JFK", "name": "John F. Kennedy International Airport"},
            "to_airport": {"code": "LAX", "name": "Los Angeles International Airport"},
            "departure": "2026-06-15T06:00:00",
            "arrival": "2026-06-15T08:53:00",
            "duration_min": 353,
            "plane_type": "Airbus A320"
        }
    ],
    "carbon_emission_g": 407000,
    "carbon_typical_on_route_g": 363000,
    "scraped_at": "2026-04-30T10:18:06.919Z"
}
```

## 🌍 Supported features

| Feature | Supported |
| --- | --- |
| One-way searches | ✅ |
| Round-trip searches | ✅ |
| Multi-city itineraries | ✅ |
| Economy, Premium, Business, First | ✅ |
| Adults, children, infants (seat & lap) | ✅ |
| 30+ result languages | ✅ |
| Carbon emissions data | ✅ |
| Plane type per segment | ✅ |
| Layover detection | ✅ (via `stops` field) |

## 🛠️ Output fields

| Field | Type | Description |
| --- | --- | --- |
| `airline_codes` | array | IATA airline codes (e.g. `B6`, `DL`, `UA`) |
| `airline_names` | array | Human-readable airline names |
| `price` | number | Total price in displayed currency (typically USD) |
| `stops` | integer | Number of layovers (0 = direct) |
| `duration_min_total` | integer | Total trip time in minutes |
| `departure` / `arrival` | ISO datetime | First-segment departure / last-segment arrival |
| `segments` | array | Per-leg detail with airports, times, plane type |
| `carbon_emission_g` | number | This itinerary's CO₂ emissions (grams) |
| `carbon_typical_on_route_g` | number | Typical CO₂ for the route (compare for greener picks) |

## 💸 Pricing

Pay per scraped itinerary — set the price tier in Apify Console after the actor is published.

## 🔍 SEO keywords

google flights scraper, flight price scraper, airfare scraper, flight data api, scrape flights without api, real-time flight prices, fare aggregator data, multi-city flight scraper, google flights api alternative, airline price tracker, flight scraping tool, carbon emissions flight data, jfk lax flights scraper, business class scraper, premium economy scraper

## ⚠️ Compliance

This actor extracts publicly available data from Google Flights search results. No login or paid API is involved. Use the data in compliance with applicable laws and Google's terms of service. Intended for research, monitoring, and analytics workflows.