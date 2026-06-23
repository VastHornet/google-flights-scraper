[Google Flights Scraper](https://apify.com/crawlio/google-flights-scraper?fpr=data)

## Overview

Google Flights Scraper searches Google Flights and stores one dataset item per returned itinerary. It supports one-way, round-trip, and multi-city searches.

## Supported searches

- **One-way**: one origin, one destination, one departure date.
- **Round-trip**: one outbound leg and one return leg.
- **Multi-city**: two or more ordered flight legs.

## Quick start

1. Open the Actor on Apify.
2. Select `oneWay`, `roundTrip`, or `multiCity`.
3. Enter airport IATA codes, dates, passengers, currency, and language.
4. Use Apify Proxy when Google blocks direct requests.
5. Run the Actor and download results from the Dataset tab.

## Configuration

| Field | Description | Default |
| --- | --- | --- |
| `operation` | Search mode: `oneWay`, `roundTrip`, or `multiCity`. | `oneWay` |
| `origin`, `dest` | Three-letter IATA airport codes, for example `JFK` and `LHR`. | `JFK`, `LHR` |
| `date`, `returnDate` | Travel dates in `YYYY-MM-DD` format. Leave empty to use valid future dates automatically. | Empty |
| `seatClass` | `economy`, `premium-economy`, `business`, or `first`. | `economy` |
| `stops` | `0` any stops, `1` nonstop, `2` max one stop. | `0` |
| `sort` | `best`, `price`, or `duration`. | `best` |
| `currency`, `language` | Google Flights currency and locale, for example `USD` and `en-US`. | `USD`, `en-US` |
| `fetchAllLegs` | Multi-city only: `true` to fetch complete two-leg itineraries, `false` to only fetch the first leg. | `false` |
| `proxyConfiguration` | Optional Apify Proxy settings. | `{"useApifyProxy": false}` |

## Usage examples

### One-way input

```
{
  "operation": "oneWay",
  "oneWay": {
    "origin": "JFK",
    "dest": "LHR",
    "date": "",
    "adults": 1,
    "seatClass": "economy",
    "stops": 0,
    "sort": "best",
    "currency": "USD",
    "language": "en-US"
  },
  "proxyConfiguration": {
    "useApifyProxy": true
  }
}
```

### Round-trip input

```
{
  "operation": "roundTrip",
  "roundTrip": {
    "origin": "JFK",
    "dest": "LHR",
    "date": "",
    "returnDate": "",
    "adults": 1,
    "seatClass": "economy",
    "currency": "USD",
    "language": "en-US"
  },
  "proxyConfiguration": {
    "useApifyProxy": true
  }
}
```

### Multi-city input

```
{
  "operation": "multiCity",
  "multiCity": {
    "legs": "[{\"origin\":\"JFK\",\"dest\":\"LHR\",\"date\":\"2026-06-01\"},{\"origin\":\"LHR\",\"dest\":\"CDG\",\"date\":\"2026-06-05\"}]",
    "adults": 1,
    "seatClass": "economy",
    "stops": 0,
    "sort": "best",
    "currency": "USD",
    "language": "en-US",
    "fetchAllLegs": false
  },
  "proxyConfiguration": {
    "useApifyProxy": true
  }
}
```

## Output Examples

Results are written to the default dataset. The key-value store record `OUTPUT` contains a compact run summary.

### One-way output

```
{
  "price": 77236,
  "airlines": ["Qatar Airways"],
  "departure": "2026-06-01T19:05",
  "arrival": "2026-06-02T14:05",
  "stops": 1,
  "duration_min": 850,
  "segments": [
    {
      "from": "DAC",
      "from_name": "Hazrat Shahjalal International Airport",
      "to": "DOH",
      "to_name": "Hamad International Airport",
      "departure": "2026-06-01T19:05",
      "arrival": "2026-06-01T21:55",
      "duration_min": 350,
      "plane": "Boeing 777"
    },
    {
      "from": "DOH",
      "from_name": "Hamad International Airport",
      "to": "LHR",
      "to_name": "Heathrow Airport",
      "departure": "2026-06-02T07:45",
      "arrival": "2026-06-02T14:05",
      "duration_min": 500,
      "plane": "Airbus A350"
    }
  ]
}
```

### Round-trip output

```
{
  "price": 862,
  "airlines": ["American"],
  "departure": "2026-06-01T19:20",
  "arrival": "2026-06-02T07:20",
  "stops": 0,
  "duration_min": 420,
  "segments": [
    {
      "from": "JFK",
      "from_name": "John F. Kennedy International Airport",
      "to": "LHR",
      "to_name": "Heathrow Airport",
      "departure": "2026-06-01T19:20",
      "arrival": "2026-06-02T07:20",
      "duration_min": 420,
      "plane": "Boeing 787"
    }
  ]
}
```

### Multi-city output

```
{
  "price": 1186,
  "airlines": ["British Airways"],
  "legs": [
    {
      "departure": "2026-06-01T19:10",
      "arrival": "2026-06-02T07:20",
      "stops": 0,
      "duration_min": 430,
      "airlines": ["British Airways"],
      "segments": [
        {
          "from": "JFK",
          "from_name": "John F. Kennedy International Airport",
          "to": "LHR",
          "to_name": "Heathrow Airport",
          "departure": "2026-06-01T19:10",
          "arrival": "2026-06-02T07:20",
          "duration_min": 430,
          "plane": "Boeing 777"
        }
      ]
    },
    {
      "departure": "2026-06-05T12:50",
      "arrival": "2026-06-05T15:10",
      "stops": 0,
      "duration_min": 80,
      "airlines": ["British Airways"],
      "segments": [
        {
          "from": "LHR",
          "from_name": "Heathrow Airport",
          "to": "CDG",
          "to_name": "Paris Charles de Gaulle Airport",
          "departure": "2026-06-05T12:50",
          "arrival": "2026-06-05T15:10",
          "duration_min": 80,
          "plane": "Airbus A320neo"
        }
      ]
    }
  ],
  "price_scope": "entire_trip",
  "search_scope": "multi_city_complete",
  "requested_legs": [
    {
      "date": "2026-06-01",
      "origin": "JFK",
      "dest": "LHR"
    },
    {
      "date": "2026-06-05",
      "origin": "LHR",
      "dest": "CDG"
    }
  ]
}
```

## Reliability notes

- Google Flights can return consent, verification, or incomplete pages from shared IPs.

## Legal note

This Actor collects publicly available flight-search results. Make sure your use case complies with applicable laws, regulations, and website terms.