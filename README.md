[Google Flights Scraper](https://apify.com/solidcode/google-flights-scraper?fpr=data)

Extract real-time flight prices, airlines, schedules, and routes from Google Flights at scale. Search one-way, round-trip, or complex multi-city itineraries with full control over passengers, filters, currency, and locale — and get clean, structured JSON every time.

## Why This Scraper?

- **Full trip-type coverage** — One-way, round-trip, and multi-city searches with 2+ legs all in a single actor
- **Multi-airport support** — Search across several origin or destination airports at once (e.g. `CDG,ORY` or `JFK,LGA,EWR`)
- **Rich flight details** — Price, airline, flight number, aircraft type, departure and arrival times, layovers, total duration, and stop count for every leg
- **Powerful filters** — Cap maximum price, restrict to specific airlines, limit stops (nonstop only, up to 1 stop, up to 2), and hide basic economy fares
- **29 languages × 39 countries × 20 currencies** — Pull results the way your target traveller sees them
- **Price insights included** — First-page results tell you whether the current price is low, typical, or high versus the historical average, plus the lowest price available
- **Airport directory** — Every referenced airport comes back with full name, city, and country — no extra lookups needed
- **Non-technical input** — Friendly labels and dropdowns for languages, countries, currencies, and stop limits; no need to remember Google's internal codes

## Use Cases

**Travel & Fare Monitoring**

- Track prices on specific routes over time to identify the best booking windows
- Monitor competitor fares for OTA and metasearch pricing strategies
- Build fare alert systems that notify users when prices drop

**Travel Agencies & OTAs**

- Compare airline offers across multiple routes and dates in seconds
- Populate search results for travel booking sites and mobile apps
- Feed live fare data into CRM systems for personalised customer offers

**Market Research & Analytics**

- Analyse price trends between city pairs, seasons, or airlines
- Study how fares shift across currencies, booking country, or trip type
- Benchmark basic economy vs. main cabin pricing across carriers

**Corporate Travel & Expense Management**

- Audit booked fares against live market prices for policy compliance
- Surface cheaper alternatives for frequent business routes
- Build internal dashboards comparing travel spend to real-time market rates

**Product Development & Data Enrichment**

- Enrich itinerary databases with live pricing, durations, and layover details
- Power route-planning tools with accurate multi-city trip data
- Feed ML models with structured historical fare and availability signals

## Getting Started

### Simple One-Way Search

The minimum input — origin, destination, and a date:

```
{
    "departureAirports": "LAX",
    "arrivalAirports": "JFK",
    "outboundDate": "2026-08-15"
}
```

### Round-Trip with Passengers

Add a return date to run a round-trip search:

```
{
    "departureAirports": "SFO",
    "arrivalAirports": "SEA",
    "outboundDate": "2026-09-10",
    "returnDate": "2026-09-17",
    "adults": 2,
    "children": 1
}
```

### Filtered Search

Cap price, limit stops, and restrict to specific airlines:

```
{
    "departureAirports": "LHR",
    "arrivalAirports": "JFK,EWR",
    "outboundDate": "2026-10-05",
    "returnDate": "2026-10-19",
    "maxPrice": 800,
    "maxStops": "0",
    "airlines": "BA,AA,VS",
    "excludeBasicEconomy": true,
    "currency": "GBP",
    "country": "uk",
    "language": "en"
}
```

### Multi-City Itinerary

Build a custom multi-leg trip by listing each leg:

```
{
    "multiCityLegs": [
        { "departureAirports": "LAX", "arrivalAirports": "JFK", "date": "2026-07-10" },
        { "departureAirports": "JFK", "arrivalAirports": "MIA", "date": "2026-07-15" },
        { "departureAirports": "MIA", "arrivalAirports": "LAX", "date": "2026-07-22" }
    ],
    "adults": 1,
    "currency": "USD"
}
```

When `multiCityLegs` is provided, the simple route fields above are ignored.

## Input Reference

### Route

| Parameter | Type | Default | Description |
| --- | --- | --- | --- |
| `departureAirports` | string |  | IATA airport codes to depart from, comma-separated (e.g. `LAX` or `CDG,ORY`) |
| `arrivalAirports` | string |  | IATA airport codes to arrive at, comma-separated (e.g. `JFK` or `LAX,SEA`) |
| `outboundDate` | string |  | Departure date in `YYYY-MM-DD` format. Required for one-way and round-trip |
| `returnDate` | string |  | Return date in `YYYY-MM-DD`. Set this for a round-trip; leave empty for one-way |
| `multiCityLegs` | array | `[]` | Multi-city itinerary legs. Each leg has `departureAirports`, `arrivalAirports`, and `date`. Overrides the simple route fields when set |

### Passengers

| Parameter | Type | Default | Description |
| --- | --- | --- | --- |
| `adults` | integer | `1` | Adult passengers (age 12+). Minimum 1 |
| `children` | integer | `0` | Child passengers (ages 2–11) |
| `infants` | integer | `0` | Infant passengers (under 2) |

### Filters

| Parameter | Type | Default | Description |
| --- | --- | --- | --- |
| `maxPrice` | integer |  | Only return flights at or below this price, in the chosen currency |
| `maxStops` | string |  | `"0"` = direct only, `"1"` = up to 1 stop, `"2"` = up to 2 stops |
| `airlines` | string |  | Preferred airline IATA codes, comma-separated (e.g. `UA,AA,DL`) |
| `excludeBasicEconomy` | boolean | `false` | Hide basic economy fares (no free carry-on or seat selection) |

### Localization

| Parameter | Type | Default | Description |
| --- | --- | --- | --- |
| `currency` | string | `"USD"` | Currency for prices — 20 ISO 4217 codes supported (USD, EUR, GBP, CAD, AUD, JPY, CHF, INR, CNY, and more) |
| `language` | string | `"en"` | Result language — 29 options (English, Spanish, French, German, Chinese, Japanese, Arabic, Hindi, and more) |
| `country` | string | `"us"` | Country market Google searches from — 39 options. Affects available fares and airlines |

### Options

| Parameter | Type | Default | Description |
| --- | --- | --- | --- |
| `fetchBookingOptions` | boolean | `false` | Include direct booking links with fare family and baggage details. One-way searches only. Each resolved link is billed separately |
| `maxPages` | integer | `1` | How many times to run the same search. Google returns all matches in one call, so leave at 1 unless you specifically want duplicate pages |

## Output

Each dataset item represents one page of results. Here's a trimmed example:

```
{
    "search_parameters": {
        "tripType": "one-way",
        "legs": [
            { "departureAirports": ["LAX"], "arrivalAirports": ["JFK"], "date": "2026-08-15" }
        ],
        "passengers": { "adults": 1, "children": 0, "infants": 0 },
        "language": "en",
        "country": "us",
        "currency": "USD"
    },
    "search_metadata": {
        "total_flights_found": 25,
        "best_flights_count": 3,
        "other_flights_count": 22,
        "pages_processed": 1,
        "booking_options_count": 0
    },
    "search_timestamp": "2026-04-20T22:15:00+00:00",
    "page_number": 1,
    "best_flights": [
        {
            "price": 174,
            "airline": "B6",
            "airlineName": "JetBlue",
            "departureAirport": "LAX",
            "arrivalAirport": "JFK",
            "departureDate": "2026-08-15",
            "departureTime": "20:50",
            "arrivalDate": "2026-08-16",
            "arrivalTime": "05:25",
            "durationMinutes": 335,
            "stops": 0,
            "flightId": "OriFxc",
            "legs": [
                {
                    "airline": "B6",
                    "airlineName": "JetBlue",
                    "flightNumber": "424",
                    "aircraft": "Airbus A321",
                    "departureAirport": "LAX",
                    "arrivalAirport": "JFK",
                    "departureDate": "2026-08-15",
                    "departureTime": "20:50",
                    "arrivalDate": "2026-08-16",
                    "arrivalTime": "05:25",
                    "durationMinutes": 335
                }
            ],
            "bookingToken": "CjRIMXV1M21..."
        }
    ],
    "other_flights": [ "..." ],
    "price_insights": {
        "priceLevel": "high",
        "lowestPrice": 174,
        "typicalPrice": 139,
        "typicalPriceRange": [125, 190]
    },
    "airports": [
        { "code": "LAX", "name": "Los Angeles International Airport", "city": "Los Angeles", "country": "US" },
        { "code": "JFK", "name": "John F. Kennedy International Airport", "city": "New York", "country": "US" }
    ]
}
```

### All Available Fields

**Top-level**

| Field | Type | Description |
| --- | --- | --- |
| `search_parameters` | object | Echo of the resolved search — trip type, routes, dates, passengers, language, country, currency |
| `search_metadata` | object | Aggregate stats — total flights found, best/other counts, pages processed, booking options count |
| `search_timestamp` | string | ISO-8601 timestamp of when this page was fetched |
| `page_number` | integer | 1-indexed page number |
| `best_flights` | object[] | Google's top-recommended flights for this search |
| `other_flights` | object[] | Additional flight options beyond the best picks |
| `price_insights` | object | First page only — tells you whether the current price is low, typical, or high versus history |
| `airports` | object[] | First page only — full details for every airport referenced in the results |
| `booking_options` | object[] | Populated only when `fetchBookingOptions=true` on a one-way search |

**Flight object**

| Field | Type | Description |
| --- | --- | --- |
| `price` | number | Total price in the chosen currency |
| `airline` | string | Primary airline IATA code |
| `airlineName` | string | Full airline name |
| `departureAirport` | string | IATA code of the origin airport |
| `arrivalAirport` | string | IATA code of the final destination |
| `departureDate` | string | `YYYY-MM-DD` |
| `departureTime` | string | 24-hour local time (`HH:MM`) |
| `arrivalDate` | string | `YYYY-MM-DD` |
| `arrivalTime` | string | 24-hour local time (`HH:MM`) |
| `durationMinutes` | number | Total trip duration in minutes (including layovers) |
| `stops` | number | Number of stops (0 = nonstop) |
| `flightId` | string | Unique identifier for this flight option |
| `legs` | object[] | Per-segment details (airline, flight number, aircraft, times, airports) |
| `bookingToken` | string | Token you can feed back into `fetchBookingOptions` to resolve direct booking links |

**Price insights**

| Field | Type | Description |
| --- | --- | --- |
| `priceLevel` | string | `"low"`, `"typical"`, or `"high"` compared to historical fares |
| `lowestPrice` | number | Lowest price currently available |
| `typicalPrice` | number | Typical price for this route at this time of year |
| `typicalPriceRange` | number[] | `[min, max]` typical price band |

**Airport**

| Field | Type | Description |
| --- | --- | --- |
| `code` | string | IATA code (e.g. `LAX`) |
| `name` | string | Full airport name |
| `city` | string | City name |
| `country` | string | Country code |

## Tips for Best Results

- **Use future dates within the next 12 months** — Google Flights rarely returns results for dates more than a year out
- **Match currency and country to your use case** — a search with `country="uk"` and `currency="GBP"` shows fares and airlines as a UK traveller would see them, which can differ significantly from the US view
- **Leverage multi-airport search** — `JFK,LGA,EWR` as a single origin often surfaces cheaper or more convenient options than any single airport alone
- **Start with `maxStops="0"` for premium routes** — nonstop filters cut noise on busy business routes and reveal only the most attractive fares
- **Leave `maxPages` at 1** — Google returns every matching flight in a single response; setting it higher just re-runs the same query
- **Combine `airlines` with `excludeBasicEconomy`** — perfect for corporate travel auditing or loyalty-program-aware fare monitoring

## Pricing

**$0.01 per 1,000 results** — one of the lowest prices on Apify for live flight data.

| Results | Cost |
| --- | --- |
| 1,000 | $0.01 |
| 10,000 | $0.10 |
| 100,000 | $1.00 |
| 1,000,000 | $10.00 |

One page typically contains 10–60 flight options across the `best_flights` and `other_flights` arrays, so a single page is usually a complete search for a given route and date. Platform fees (compute, proxy, storage) depend on your Apify plan.

When `fetchBookingOptions=true` is enabled on a one-way search, each resolved booking link is billed separately — a typical search produces 80–120 options.

## Integrations

Export results in JSON, CSV, Excel, XML, or RSS. Connect to 1,500+ apps via:

- **Zapier** / **Make** / **n8n** — Workflow automation
- **Google Sheets** — Direct spreadsheet export
- **Slack** / **Email** — Notifications on new results
- **Webhooks** — Custom API integrations
- **Apify API** — Full programmatic access to runs and datasets

## Legal & Ethical Use

This actor is designed for legitimate fare monitoring, travel research, pricing analysis, and data enrichment. Users are responsible for complying with applicable laws and Google's Terms of Service. The data returned is publicly available flight pricing — do not use it for spam, resale-as-inventory, or any deceptive purpose.