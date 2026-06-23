[Google Flights Scraper](https://apify.com/kaix/google-flights-scraper?fpr=data)

# Google Flights Scraper

Scrape Google Flights for flight search results, calendar prices, and booking details. Supports batch searches with full filter control.

## Why use this scraper?

- Fast: lightweight requests without browser overhead
- Batch searches: run multiple route queries in a single actor run
- Full filter control: stops, price, duration, airlines/alliances, departure/arrival time windows
- Calendar prices: get cheapest-price-per-day across ~60 days via three calendar modes
- Booking details: fare info and booking links from multiple providers
- Structured output: typed FlightResult records with segments, layovers, CO2 emissions

## Use cases

- Monitor flight prices across routes and dates
- Build fare databases for travel analytics
- Compare prices across airlines and alliances
- Feed calendar price data into alerting pipelines
- Track cheapest nonstop options on specific corridors

## How to use

### Basic one-way search

```
{
  "searches": [
    { "origin": "SFO", "destination": "LAX", "departureDate": "2026-06-15" }
  ]
}
```

### Round-trip

```
{
  "searches": [
    { "origin": "SFO", "destination": "NRT", "departureDate": "2026-07-01", "returnDate": "2026-07-15" }
  ]
}
```

### Batch searches

```
{
  "searches": [
    { "origin": "SFO", "destination": "LAX", "departureDate": "2026-06-15" },
    { "origin": "JFK", "destination": "MIA", "departureDate": "2026-06-20" },
    { "origin": "ORD", "destination": "DEN", "departureDate": "2026-06-25" }
  ]
}
```

### With filters

```
{
  "searches": [
    { "origin": "SFO", "destination": "LAX", "departureDate": "2026-06-15" }
  ],
  "maxStops": "0",
  "maxPrice": 200,
  "airlines": ["UA", "AA"],
  "departureTimeEarliest": "08:00",
  "departureTimeLatest": "18:00",
  "sortBy": "cheapest"
}
```

### With calendar prices

```
{
  "searches": [
    { "origin": "SFO", "destination": "LAX", "departureDate": "2026-06-15" }
  ],
  "includeCalendarPrices": true,
  "calendarMode": "graph"
}
```

### Explore cheapest destinations (flexible dates)

```
{
  "exploreOrigin": "DAD",
  "tripDuration": "1-week"
}
```

### Business class with multiple passengers

```
{
  "searches": [
    { "origin": "SFO", "destination": "NRT", "departureDate": "2026-07-01", "returnDate": "2026-07-15" }
  ],
  "cabinClass": "business",
  "adults": 2,
  "children": 1
}
```

## Input

| Field | Type | Default | Description |
| --- | --- | --- | --- |
| `exploreOrigin` | string |  | IATA airport code to explore cheapest destinations (e.g. `SFO`) |
| `tripDuration` | enum | `1-week` | Trip duration for explore: `weekend`, `1-week`, `2-weeks` |
| `searches` | array |  | List of `{ origin, destination, departureDate, returnDate? }` |
| `tripType` | enum | inferred | `one-way`, `round-trip`, `multi-city` |
| `cabinClass` | enum | `economy` | `economy`, `premium-economy`, `business`, `first` |
| `adults` | integer | `1` | Adult passengers (1-9) |
| `children` | integer | `0` | Child passengers (0-9) |
| `infantsOnLap` | integer | `0` | Infants on lap (0-9) |
| `infantsInSeat` | integer | `0` | Infants in seat (0-9) |
| `maxStops` | enum | any | `0` (nonstop), `1` (1 or fewer), `2` (2 or fewer) |
| `maxPrice` | integer |  | Maximum price in your currency |
| `maxDuration` | integer |  | Maximum total duration in minutes |
| `airlines` | string[] |  | Airline IATA codes or alliance names (e.g. `UA`, `ONEWORLD`) |
| `departureTimeEarliest` | string |  | Earliest departure (HH:MM, e.g. `08:00`) |
| `departureTimeLatest` | string |  | Latest departure (HH:MM, e.g. `18:00`) |
| `arrivalTimeEarliest` | string |  | Earliest arrival (HH:MM) |
| `arrivalTimeLatest` | string |  | Latest arrival (HH:MM) |
| `sortBy` | enum | `best` | `best`, `cheapest`, `fastest`, `departure-time`, `arrival-time` |
| `showAllResults` | boolean | `false` | Return all results instead of top ~30 |
| `includeCalendarPrices` | boolean | `false` | Fetch cheapest price per day |
| `calendarMode` | enum | `graph` | `graph` (~60 days), `picker` (~6 weeks), `grid` (±3 days) |
| `includeBookingDetails` | boolean | `false` | Fetch booking links per flight |
| `proxyConfiguration` | object |  | Proxy settings. Residential proxies recommended |

## Output

### Explore output (live data — Da Nang, 1-week trips)

When using `exploreOrigin`, one record per destination:

```
{
  "city": "Hạ Long Bay",
  "country": "Vietnam",
  "airport": "HAN",
  "departureDate": "2026-10-02",
  "returnDate": "2026-10-18",
  "price": 52,
  "cheapestPrice": 27,
  "airline": "Vietjet",
  "airlineCode": "VJ",
  "stops": 0,
  "duration": 80
}
```

### Flight search output (live data — Da Nang to Tokyo)

```
{
  "origin": "DAD",
  "destination": "NRT",
  "departureDate": "2026-05-15",
  "returnDate": null,
  "tripType": "one-way",
  "cabinClass": "economy",
  "price": 291,
  "currency": "USD",
  "pricePerPassenger": null,
  "totalDuration": 855,
  "stops": 1,
  "airlines": ["T'Way Air"],
  "airlineCodes": ["TW"],
  "outbound": {
    "duration": 855,
    "stops": 1,
    "segments": [
      {
        "airline": "T'Way Air",
        "airlineCode": "TW",
        "flightNumber": "TW14",
        "aircraft": "Boeing 737",
        "departureAirport": "DAD",
        "arrivalAirport": "ICN",
        "departureTime": "2026-05-15T01:25:00",
        "arrivalTime": "2026-05-15T08:15:00",
        "duration": 290,
        "layover": {
          "airport": "ICN",
          "duration": 405
        }
      },
      {
        "airline": "T'Way Air",
        "airlineCode": "TW",
        "flightNumber": "TW245",
        "aircraft": "Boeing 737MAX 8 Passenger",
        "departureAirport": "ICN",
        "arrivalAirport": "NRT",
        "departureTime": "2026-05-15T15:00:00",
        "arrivalTime": "2026-05-15T17:40:00",
        "duration": 160,
        "layover": null
      }
    ]
  },
  "return": null,
  "co2Emissions": 355138,
  "bookingToken": "CjRI...",
  "calendarPrices": [
    { "date": "2026-05-15", "price": 291 },
    { "date": "2026-05-16", "price": 256 },
    { "date": "2026-05-17", "price": 237 },
    { "date": "2026-05-18", "price": 266 },
    { "date": "2026-05-19", "price": 291 }
  ]
}
```

### Output fields

**Explore destination** (when using `exploreOrigin`): `city`, `country`, `airport`, `departureDate`, `returnDate`, `price`, `cheapestPrice`, `airline`, `airlineCode`, `stops`, `duration`

**Flight** (when using `searches`): `origin`, `destination`, `departureDate`, `returnDate`, `tripType`, `cabinClass`, `price`, `currency`, `pricePerPassenger`, `totalDuration`, `stops`, `airlines`, `airlineCodes`, `outbound`, `return`, `co2Emissions`, `co2EmissionsLabel`, `bookingToken`, `fareClass`

**Leg**: `duration`, `stops`, `segments[]`

**Segment**: `airline`, `airlineCode`, `flightNumber`, `aircraft`, `departureAirport`, `arrivalAirport`, `departureTime`, `arrivalTime`, `duration`, `layover` (`airport`, `duration`)

**Calendar prices** (when `includeCalendarPrices` is true, attached to first result): `date`, `price`

**Booking details** (when `includeBookingDetails` is true): `baggageAllowance`, `fareRules`, `bookingLinks[]` (`airline`, `url`)