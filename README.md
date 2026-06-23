[Google Flights Scraper](https://apify.com/kaizu/google-flights-scraper?fpr=data)

# Google Flights Scraper - Real-Time Flight Prices & Calendar

**The most reliable Google Flights API on Apify. Get real-time prices, schedules, and booking links for any route worldwide.**

[![Price per run](https://img.shields.io/badge/price-%240.015%2Frun-success)](https://apify.com/google-flights-scraper)
[![Response time](https://img.shields.io/badge/response-2--5%20seconds-blue)](https://apify.com/google-flights-scraper)
[![Currencies](https://img.shields.io/badge/currencies-40%2B-orange)](https://apify.com/google-flights-scraper)

## Why Choose This Scraper?

**⚡ Blazing Fast** — Results in 2-5 seconds (5x faster than competitors)

**🌍 Global Coverage** — 9,000+ airports worldwide, all major airlines

**💰 Best Value** — Only $0.015 per run (60% cheaper than SerpApi)

**🔗 Direct Booking Links** — Get clickable Google Flights URLs to book instantly

**🌱 Carbon Footprint** — Track CO2 emissions for eco-conscious travel

**📊 Rich Data** — Layover details, aircraft types, price insights, airport info

---

## What You Get

### For Every Flight Search:

- ✅ **Real-time prices** from Google Flights (not cached)
- ✅ **Direct booking URLs** — Click to book on Google Flights
- ✅ **Complete itineraries** — All legs, layovers, aircraft types
- ✅ **Carbon emissions** — Environmental impact data
- ✅ **Price insights** — "Typical", "Low", or "High" price indicator
- ✅ **Airport details** — Full airport names, cities, countries
- ✅ **37 currencies** — Local pricing for global markets

### Price Calendar Mode:

Find the cheapest dates across any date range. Perfect for flexible travelers.

---

## Perfect For

| Use Case | How It Helps |
| --- | --- |
| **Travel Startups** | Power your flight comparison app with real Google data |
| **Price Tracking** | Monitor routes and get alerts when prices drop |
| **Travel Agencies** | Quote accurate prices to customers instantly |
| **Data Science** | Analyze airline pricing patterns & trends |
| **Trip Planning** | Find the cheapest dates to fly with calendar view |
| **Affiliate Sites** | Monetize with direct booking links |

---

## Live Example

**Input:**

```
{
  "departure_id": "JFK",
  "arrival_id": "LAX",
  "outbound_date": "2026-06-15",
  "adults": 1,
  "currency": "USD"
}
```

**Output (Real Flight):**

```
{
  "airline": "JetBlue",
  "flightNumber": "B6 1523",
  "departureAirport": "JFK",
  "arrivalAirport": "LAX",
  "departureTime": "15:15",
  "arrivalTime": "18:17",
  "duration": "6 hr 2 min",
  "durationMinutes": 362,
  "price": 169,
  "priceFormatted": "$169",
  "booking_url": "https://www.google.com/travel/flights/booking?tfs=...",
  "carbonEmissions": {
    "totalKg": 145,
    "differencePercent": -12
  },
  "layovers": [],
  "legs": [
    {
      "airline": "JetBlue",
      "flightNumber": "B6 1523",
      "departure": "JFK",
      "arrival": "LAX",
      "departureTime": "15:15",
      "arrivalTime": "18:17",
      "duration": "6 hr 2 min",
      "durationMinutes": 362,
      "aircraft": "Airbus A320"
    }
  ]
}
```

---

## Output Fields Reference

| Field | Description | Example |
| --- | --- | --- |
| `airline` | Airline name | "JetBlue" |
| `flightNumber` | Full flight number | "B6 1523" |
| `price` | Price in selected currency | 169 |
| `priceFormatted` | Human-readable price | "$169" |
| `booking_url` | Direct booking link | "[https://www.google.com/travel/flights/booking](https://www.google.com/travel/flights/booking)?..." |
| `durationMinutes` | Total flight time | 362 |
| `stops` | Number of stops | 0 |
| `layovers` | Stop details with duration | [{"airport": "AUH", "duration": "2 hr 20 min"}] |
| `carbonEmissions` | CO2 data | {"totalKg": 145, "differencePercent": -12} |
| `legs` | Individual flight segments | See above |
| `isBest` | Google "best" flag | true |

**Bonus:** OUTPUT key-value includes `airports`, `priceInsights`, and `flightsUrl`

---

## How to Use

### 1. Quick Start (30 seconds)

1. Click **Try for free**
2. Enter airport codes (e.g., JFK → LAX)
3. Set your travel date
4. Click **Start**
5. Download JSON/CSV/Excel

### 2. Advanced Filters

- **Max stops:** 0 (nonstop) to 3
- **Max price:** Set your budget limit
- **Airlines:** Filter by specific carriers (AA, DL, UA)
- **Cabin class:** Economy to First
- **Currency:** 40+ currencies supported

### 3. One-Way vs Round-Trip

**One-way flight:** Leave `return_date` empty

```
{
  "departure_id": "JFK",
  "arrival_id": "LAX",
  "outbound_date": "2026-06-15",
  "return_date": null
}
```

**Round-trip flight:** Add `return_date`

```
{
  "departure_id": "JFK",
  "arrival_id": "LAX",
  "outbound_date": "2026-06-15",
  "return_date": "2026-06-22"
}
```

### 4. Price Calendar

Set `type: "calendar"` to find cheapest dates across a range.

---

## Pricing

**Pay only for what you use:**

- $0.015 per actor run
- No monthly fees
- No setup costs

**Example costs:**

- 100 searches = $1.50
- 1,000 searches = $15.00
- 10,000 searches = $150.00

*Compare to: SerpApi ($0.01-0.05 per request), other scrapers ($0.02+ per run)*

---

## Supported Currencies (40+)

USD, EUR, GBP, JPY, CNY, KRW, INR, BRL, CAD, AUD, CHF, SEK, NOK, DKK, PLN, CZK, HUF, TRY, THB, MXN, SGD, HKD, NZD, ZAR, RUB, ILS, AED, SAR, PHP, MYR, IDR, TWD, ARS, CLP, COP, PEN, VND, EGP, NGN, KES

---

## Integrations

**Export to:**

- 📊 Google Sheets
- 📧 Email notifications
- 💬 Slack alerts
- 🔄 Webhooks
- ☁️ Amazon S3
- 🗄️ Any REST API

**Scheduling:**

- Run hourly, daily, or custom intervals
- Get alerts when prices drop
- Monitor competitor routes

---

## Data Reliability

✅ **Real-time data** — Direct from Google Flights (not cached)

✅ **99%+ uptime** — Enterprise-grade infrastructure

✅ **Automatic retries** — Handles transient failures

✅ **Input validation** — Clear error messages

✅ **Structured output** — Clean JSON, no HTML parsing

---

## Limits

- Max 9 adult passengers per search
- Valid IATA airport codes only (3 letters)
- Future dates only (YYYY-MM-DD format)
- 30-second timeout per request

---

## For Actor Owners (Setting Up This Actor)

**To configure pricing:**

1. Push to Apify: `npx apify push`
2. Go to [Apify Console](https://console.apify.com) → Your Actor
3. Click **"Monetization"** in left sidebar
4. Set **Pay-per-event** pricing:

- Price: $0.015 per run
- Event name: "Actor run"
5. Click **Save**

**To set environment variables:**

1. Actor Settings → Environment variables
2. Add secrets:

- `API_URL` = `https://flights-api.wonderbook.co.il`
- `API_KEY` = your production API key
3. Mark both as **Secret** (encrypted)

See `DEPLOYMENT.md` for full deployment guide.

---

## Need Help?

- 📖 **Documentation:** Check the OUTPUT tab for each run
- 💬 **Support:** Message via Apify platform
- 🐛 **Issues:** Include your input JSON and error message

---

**Start extracting flight data today. Click "Try for free" →**