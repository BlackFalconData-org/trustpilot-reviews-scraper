# Trustpilot Reviews Scraper - Full Review Coverage

Extract structured data from [trustpilot.com](https://trustpilot.com) — trustpilot.com - full review coverage beyond Trustpilot’s 200-review display limit. TrustScores, star ratings, reviewer profiles, company contact data, and transparency reports. Incremental mode detects new reviews. Compact output for AI agents.

**[Trustpilot Reviews Scraper - Full Review Coverage on Apify →](https://apify.com/blackfalcondata/trustpilot-reviews-scraper?fpr=1h3gvi)**

---

## Key features


**Multiple input modes** — reviews (default) or search companies or browse category or company info only. Switch modes without re-scraping.

**Incremental mode** — Only get new or changed listings since your last run. Content hash per listing — no duplicates, no re-processing.

**Change classification** — Track cross-run repost detection across runs. Build audit trails of how listings evolve over time.

**Compact output** — Emit core fields only (AI-agent / MCP-friendly). Keeps response size small for LLM workflows.

**Description truncation** — Cap description length per listing to control output size and cost.

**Result cap** — Stop after N listings. Set to 0 for the full catalog.

**Export anywhere** — Download as JSON, CSV, or Excel. Stream via Apify API, webhooks, or integrations with Make, Zapier, Airbyte, Keboola.

**Structured data** — Every listing returns the same schema with consistent field naming. All fields always present — `null` when unavailable, never omitted.

---

## Use cases


**Data pipeline automation**
Integrate with your ETL pipeline to collect structured listings from trustpilot.com on a schedule. Export to CSV, JSON, or directly to your database. Use compact mode to control output size.

**Market research**
Monitor listings, track trends, and analyze market dynamics with structured, deduplicated data from trustpilot.com.

**Change monitoring**
Run daily or hourly in incremental mode to capture only new, updated, or expired listings. Perfect for price-tracking, churn analysis, and alerting pipelines.

**AI / LLM training data**
Structured JSON per listing is ready for RAG pipelines, embeddings, and agent workflows. Compact mode trims tokens for LLM context windows.

---

## Quick start

```json
{
  "companyDomain": "booking.com",
  "maxResults": 50,
  "includeCompanyInfo": true
}
```

---

## Input parameters

| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|
| `mode` | enum | `"reviews"` | What to scrape. reviews = review data (default). searchCompanies = find companies by keyword. categories = browse a Trustpilot category. companyInfo = company profiles only (no reviews). |
| `companyDomain` | string | — | Company domain as used on Trustpilot, e.g. 'booking.com' or 'www.amazon.com'. |
| `companyName` | string | — | Company name to search for. Resolved automatically to the Trustpilot domain slug. |
| `startUrls` | array | — | List of Trustpilot review page URLs, e.g. https://www.trustpilot.com/review/booking.com |
| `searchQuery` | string | — | Company search keyword (for searchCompanies mode). E.g. 'car insurance', 'web hosting'. |
| `searchCountry` | string | `"US"` | ISO country code for company search results. Default: US. |
| `searchMaxPages` | integer | `5` | Max pages of search/category results to fetch (5 results per page). Default: 5. |
| `categoryUrl` | string | — | Trustpilot category URL for categories mode. E.g. 'https://www.trustpilot.com/categories/electronics_technology'. |
| `maxResults` | integer | `200` | Maximum reviews to return per company (0 = unlimited). Default: 200. |
| `stars` | array | — | Only return reviews with these star ratings. Leave empty for all ratings. |
| `languages` | array | — | Filter by review language ISO codes, e.g. ['en', 'de', 'fr']. Leave empty for all languages. |
| `date` | enum | `""` | Only return reviews from this time period. |
| `lookbackDays` | integer | — | Rolling window: only return reviews published within the last N days. Overrides date range when set. Use with sort=recency for efficient early termination. |
| `topics` | array | — | Filter by Trustpilot topic categories. Leave empty for all topics. E.g. ["customer_service", "delivery_service"] |
| `verified` | boolean | `false` | Return only verified reviews. |
| `withReplies` | boolean | `false` | Return only reviews that have a company reply. |
| `sort` | enum | `"recency"` | Review sort order. |
| `countryOfReviewer` | string | — | Only return reviews from reviewers in this country. ISO Alpha-2 code, e.g. 'US', 'GB', 'DE'. |
| `search` | string | — | Keyword search within review text. |
| `includeCompanyInfo` | boolean | `true` | Include company metadata: trust score, total reviews, star breakdown, categories, website, contact, and AI summary. |
| `includeProductReviews` | boolean | `false` | Include associated product review data when available. Adds a productReviews array to each review. |
| `includeTransparency` | boolean | `false` | Fetch the Trustpilot transparency report per company (+1 request). Adds a separate record with reply rate, organic share, flagged review counts, monthly distributions, and verification status. Auto-included in companyInfo mode. |
| `descriptionMaxLength` | integer | `0` | Truncate review text to N characters. 0 = no truncation. |
| `compact` | boolean | `false` | Return core review fields only: reviewId, reviewUrl, rating, title, text, dates, reviewer, companyTrustScore. Ideal for AI pipelines and MCP integrations. Overridden by 'fields' if set. |
| `fields` | array | — | Return only these fields. Overrides compact mode. E.g. ["reviewId", "rating", "title", "text", "publishedDate"]. Leave empty for all fields (or use compact for the default subset). |
| `incrementalMode` | boolean | `false` | Only return new and changed reviews compared to the previous run. Requires stateKey. |
| `stateKey` | string | — | Unique identifier for this tracking job, e.g. 'booking-com-monitor'. Required when incrementalMode is enabled. |
| `skipReposts` | boolean | `false` | Exclude reviews detected as reposts of previously seen reviews. |
| `authCookiesJson` | string | — | Optional Trustpilot consumer auth cookies as JSON. Accepts either the exported envelope with all_cookies or a plain cookie object. When provided, the actor verifies page 11 access and switches to full linear pagination. Leave empty for cookieless mode. |
| `requestDelayMs` | integer | `0` | Delay between HTTP requests in milliseconds. Use to reduce rate-limit risk on large jobs. Default: 0. |

---

## Output fields

Every listing returns the same 52-field schema. Missing values are `null` — never omitted.

- `type`
- `reviewId`
- `reviewUrl`
- `companyDomain`
- `companyName`
- `rating`
- `title`
- `text`
- `language`
- `reviewSource`
- `likes`
- `publishedDate`
- `experiencedDate`
- `updatedDate`
- `isVerified`
- `verificationLevel`
- `authorId`
- `reviewerName`
- `reviewerCountry`
- `reviewerTotalReviews`
- `reviewerIsVerified`
- `reviewerImageUrl`
- `reviewsOnSameDomain`
- `replyText`
- `replyDate`
- `replyUpdatedDate`
- `location`
- `companyTrustScore`
- `companyStars`
- `companyTotalReviews`
- `companyWebsite`
- `companyIsClaimed`
- `companyCategories`
- `companyCity`
- `companyCountry`
- `companyContactEmail`
- `companyContactPhone`
- `companyResponseRate`
- `companyResponseTime`
- `companyRating1Star`
- `companyRating2Star`
- `companyRating3Star`
- `companyRating4Star`
- `companyRating5Star`
- `languageDistribution`
- `similarCompanies`
- `productReviews`
- `aiSummary`
- `portalUrl`
- `scrapedAt`
- `source`
- `changeType`

---

## Sample output

One object per listing. Here is a real example from a production run:

```json
{
  "type": "review",
  "reviewId": "68362b480787ca52990267fc",
  "reviewUrl": "https://www.trustpilot.com/reviews/68362b480787ca52990267fc",
  "companyDomain": "booking.com",
  "companyName": "Booking.com",
  "rating": 1,
  "title": "Opgehangen omdat de geen vervangend…",
  "text": "Opgehangen omdat de geen vervangend hotel wilde zoeken na gecanceld accomodaties. ",
  "language": "af",
  "reviewSource": "Organic",
  "likes": 0,
  "publishedDate": "2025-05-27T23:14:48.000Z"
}
```

*Truncated — full records contain 52 fields. See Output fields for the complete schema.*

**[Try Trustpilot Reviews Scraper - Full Review Coverage now — $5 free credit, no credit card →](https://apify.com/blackfalcondata/trustpilot-reviews-scraper?fpr=1h3gvi)**

---

## FAQ

**How do I scrape trustpilot.com?**
Use this actor on Apify to extract structured data from trustpilot.com. Configure your search query and filters in the input, then click Start — no coding required.

**How do I get trustpilot.com data as JSON, CSV, or Excel?**
The actor writes each listing to Apify's dataset. Download as JSON, CSV, or Excel from the Console, stream via the API, or push to Make, Zapier, Airbyte, or Keboola.

**Is it legal to scrape trustpilot.com?**
Web scraping of publicly available data is generally legal. This actor only accesses publicly visible information. Always check trustpilot.com's terms of service for your specific use case.

**How much does it cost?**
Pay-per-event pricing — you only pay for listings extracted. Apify's free $5 credit is enough to run thousands of results before you pay anything.

**How does incremental mode work?**
Each listing gets a content hash. On subsequent runs, only new or changed listings are emitted — saving time, compute, and storage. Expired listings can be tracked separately.

**Do I need an API key or credentials?**
No. Just sign up for Apify, paste your input, and click Start. No credit card required.

---

## Related products by Black Falcon Data


- [StepStone Scraper](https://apify.com/blackfalcondata/stepstone-scraper?fpr=1h3gvi) — Job listings from 18 European portals
- [Indeed Job Scraper](https://apify.com/blackfalcondata/indeed-job-scraper?fpr=1h3gvi) — Indeed job listings with salary data
- [Glassdoor Job Scraper](https://apify.com/blackfalcondata/glassdoor-job-scraper?fpr=1h3gvi) — Glassdoor listings with company ratings
- [Arbeitsagentur Scraper](https://apify.com/blackfalcondata/arbeitsagentur-scraper?fpr=1h3gvi) — Germany's official job portal (1M+ listings)
- [SEEK Scraper](https://apify.com/blackfalcondata/seek-scraper?fpr=1h3gvi) — Australia & NZ's largest job board
- [Naukri Scraper](https://apify.com/blackfalcondata/naukri-scraper?fpr=1h3gvi) — India's largest job portal

---

## Getting started with Apify

New to Apify? [Create a free account with $5 credit](https://console.apify.com/sign-up?fpr=1h3gvi) — no credit card required.

1. Sign up — $5 platform credit included
2. Open [Trustpilot Reviews Scraper - Full Review Coverage](https://apify.com/blackfalcondata/trustpilot-reviews-scraper?fpr=1h3gvi) and configure your input
3. Click **Start** — export results as JSON, CSV, or Excel

Need more later? [See Apify pricing](https://apify.com/pricing?fpr=1h3gvi).

---

## About Black Falcon Data

Black Falcon Data builds production-grade web scrapers for job boards and marketplace data. Browse our full actor catalog at [www.blackfalcondata.com](https://www.blackfalcondata.com).

---

*Last updated: 2026 04*
