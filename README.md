# Ejobs.ro Job Listings Scraper Romania

Extract structured data from [ejobs.ro](https://ejobs.ro) — ejobs.ro — records. Every record carries extracted emails, salary and locations the source publishes. Address and city is included where the source lists it. Structured rows, ready to import into a CRM or a spreadsheet.

**[Ejobs.ro Job Listings Scraper Romania on Apify →](https://apify.com/blackfalcondata/ejobs-ro-scraper?fpr=1h3gvi)**

---

## 🚀 How to use this actor

> ### 💚 $5 free Apify credits — every month
> No credit card required. No commitment. Cancel anytime.

### 👉 [Sign up free on Apify →](https://console.apify.com/sign-up?fpr=1h3gvi)

1. **Click sign up** — pick GitHub, Google, or email; takes ~30 seconds
2. **Open this actor** — input is pre-filled with a working example
3. **Click Start** — export results as JSON, CSV, or Excel

Your **$5 monthly platform credit** is enough to run this actor right away — and again every month — scraping typically several hundred to several thousand results per run, depending on your input.

## Key features

**Search with filters** — Search by keyword and location. Filter by 📝 description format, and more.

**Detail enrichment** — Fetch full descriptions, salary data, employer profiles for each job.

**Incremental mode** — Only get new or changed jobs since your last run. Content hash per job — no duplicates, no re-processing.

**Change classification** — Track unchanged, expired, cross-run duplicate detection across runs. Build audit trails of how jobs evolve over time.

**Compact output** — Emit core fields only (AI-agent / MCP-friendly). Keeps response size small for LLM workflows.

**Description truncation** — Cap description length per ${NOUN} to control output size and cost.

**Result cap** — Stop after N jobs. Set to 0 for the full catalog.

**Export anywhere** — Download as JSON, CSV, or Excel. Stream via Apify API, webhooks, or integrations with Make, Zapier, Airbyte, Keboola.

**Structured data** — Every job returns the same schema with consistent field naming. All fields always present — `null` when unavailable, never omitted.

---

## Use cases

**Data pipeline automation**
Integrate with your ETL pipeline to collect structured jobs from ejobs.ro on a schedule. Export to CSV, JSON, or directly to your database. Use compact mode to control output size.

**Market research**
Monitor jobs, track trends, and analyze market dynamics with structured, deduplicated data from ejobs.ro.

**Change monitoring**
Run daily or hourly in incremental mode to capture only new, updated, or expired ${NOUNS}. Perfect for price-tracking, churn analysis, and alerting pipelines.

**Compensation benchmarking**
Aggregate salary ranges across roles, industries, and locations on ejobs.ro to inform pricing decisions, hiring plans, or candidate negotiations.

**AI / LLM training data**
Structured JSON per job is ready for RAG pipelines, embeddings, and agent workflows. Compact mode trims tokens for LLM context windows.

---

## Quick start

```json
{
  "query": "developer",
  "maxResults": 100,
  "includeDetails": true,
  "descriptionMaxLength": 0,
  "compact": false,
  "descriptionFormat": "all",
  "excludeEmptyFields": false,
  "incrementalMode": false,
  "emitUnchanged": false,
  "emitExpired": false,
  "skipReposts": false,
  "notifyOnlyChanges": false,
  "notificationLimit": 5
}
```

---

## Input parameters

| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|
| `query` | string | — | Job search keywords (Romanian or English, e.g. "programator", "developer", "vanzari"). To browse every open job, enter * on its own, send query as an empty string, or leave it empty and pick a city or a filter below. |
| `location` | string | — | Romanian city to filter on — city name (e.g. "Bucuresti", "Cluj-Napoca", "Timisoara") or the site's numeric city id. Leave empty to search the whole country. |
| `maxResults` | integer | `100` | Maximum total results (0 = every matching job; the whole site has more than 14,000 open jobs). |
| `startUrls` | array | `[]` | Paste ejobs.ro URLs: a job ad (ejobs.ro/user/locuri-de-munca/<title>/<number>), a city page (ejobs.ro/locuri-de-munca/<city>) or a job search (ejobs.ro/locuri-de-munca?q=<keywords>). Each URL becomes its own search; results are merged and deduped by job ID. When provided, startUrls REPLACE the query/location fields; unsupported URLs are skipped, and if none is supported the run stops with an input error. |
| `cities` | array | — | Only jobs in these cities — names as the site writes them (e.g. "Bucuresti", "Cluj-Napoca") or numeric city ids. Combined with 📍 City. |
| `departments` | array | — | Only jobs in these departments (e.g. "IT Software", "Marketing", "Vanzari" — the labels the site shows; accents optional) or numeric department ids. |
| `industries` | array | — | Only jobs in these industries (e.g. "IT / Telecom", "Comert / Retail" — the labels the site shows; accents optional) or numeric industry ids. |
| `careerLevels` | array | — | Only jobs at these career levels (the labels the site shows: "Fara experienta", "Entry-Level (< 2 ani)", "Mid-Level (2-5 ani)", "Senior-Level (> 5 ani)", "Manager / Executiv") or numeric ids. |
| `contractTypes` | array | — | Only jobs with these contract types ("Full time", "Part time", "Internship / Voluntariat", "Proiect / Sezonier") or numeric ids. |
| `maxAgeDays` | integer | — | Only jobs published or republished within this many days. Leave empty for any age. |
| `verifiedOnly` | boolean | `false` | Only jobs from employers carrying the site's verified badge. |
| `includeDetails` | boolean | `true` | Fetch full job details. |
| `descriptionMaxLength` | integer | `0` | Truncate description to N chars. 0 = no truncation. |
| `compact` | boolean | `false` | Core fields only (for AI-agent/MCP workflows). |
| `descriptionFormat` | enum | `"all"` | Which description representations to emit. `all` keeps description + descriptionText + descriptionHtml + descriptionMarkdown (default, back-compat). `text` / `html` / `markdown` keep only one variant — typically halves output size. |
| `excludeEmptyFields` | boolean | `false` | Drop null / empty-string / empty-array fields (and fully-null nested objects like socialProfiles) from each output record. Off by default to preserve dataset schema compatibility; turn on to slim output for LLM / agent consumption. |
| `incrementalMode` | boolean | `false` | Compare against previous run state. stateKey is optional — defaults to a value derived from search inputs (queries, locations, startUrls) so different filter sets never share state. |
| `stateKey` | string | — | Optional. Stable identifier for the tracked search universe. Leave empty to auto-generate from search inputs. |
| `emitUnchanged` | boolean | `false` | Also emit jobs that have not changed since the last run (incrementalMode only). |
| `emitExpired` | boolean | `false` | Also emit jobs that have disappeared since the last run (incrementalMode only). |
| `skipReposts` | boolean | `false` | When incremental, skip jobs whose content matches an expired job from a prior run (cross-run repost detection). |
| `notifyOnlyChanges` | boolean | `false` | When Incremental Mode is on, only send notifications for NEW and UPDATED jobs. Has no effect outside incremental mode. |
| `notificationLimit` | integer | `5` | Maximum number of jobs included in each notification message (1–20). |
| `telegramToken` | string | — | Telegram bot token (from @BotFather). Required for Telegram notifications. |
| `telegramChatId` | string | — | Telegram chat or channel ID (e.g. "-100123456789"). Required when telegramToken is set. |
| `discordWebhookUrl` | string | — | Discord incoming webhook URL. Server Settings → Integrations → Webhooks → New Webhook. |
| `slackWebhookUrl` | string | — | Slack incoming webhook URL. api.slack.com/messaging/webhooks. |
| `whatsappPhoneNumberId` | string | — | Your WhatsApp Business phone-number ID (numeric, from Meta dashboard). Required when whatsappAccessToken is set. |
| `whatsappAccessToken` | string | — | WhatsApp Cloud API permanent access token (System User token from Meta Business). Recipient must have messaged the business number within the last 24h (service-conversation window — free since Nov 2024). |
| `whatsappTo` | string | — | Recipient phone in E.164 format without + (e.g. "436641234567"). Recipient must have messaged your business number within last 24h. |
| `webhookUrl` | string | — | Receives a JSON POST with {metadata, items} after each run. Universal escape hatch for n8n / Make / Zapier / custom backends. |
| `webhookHeaders` | object | — | Optional JSON object of custom headers (e.g. {"Authorization":"Bearer ..."}). |
| `appConnector` | string | — | Optional. Pick a connected app under Settings → API & Integrations to receive your results (including any contact details). Best-effort across MCP connectors as Apify expands its catalog. |
| `mcpIssueTeam` | string | — | Only when the connected app is an issue tracker: the team (name or ID) the summary issue is created under, if that app requires one. |

---

## Output fields

Every job returns the same 70-field schema. Missing values are `null` — never omitted.

- `id`
- `url`
- `portalUrl`
- `slug`
- `title`
- `company`
- `companyId`
- `companySlug`
- `companyVerified`
- `companyLogoUrl`
- `companyUrl`
- `locations`
- `location`
- `cities`
- `counties`
- `latitude`
- `longitude`
- `geoPrecision`
- `positions`
- `workModel`
- `contractTypesIds`
- `contractTypes`
- `careerLevelsIds`
- `careerLevels`
- `departmentsIds`
- `departments`
- `industriesIds`
- `industries`
- `educationLevelsIds`
- `educationLevels`
- `languagesIds`
- `languages`
- `creationDate`
- `publishDate`
- `publishDateISO`
- `expirationDate`
- `lastModified`
- `republishDate`
- `originalPublishDate`
- `jobType`
- `isNew`
- `externalUrl`
- `descriptionHtml`
- `descriptionText`
- `descriptionMarkdown`
- `idealCandidateHtml`
- `idealCandidateText`
- `companyDescriptionHtml`
- `companyDescriptionText`
- `summary`
- `salaryText`
- `salaryMin`
- `salaryMax`
- `salaryCurrency`
- `salaryType`
- `extractedEmails`
- `extractedPhones`
- `extractedUrls`
- `socialProfiles`
- `isRepost`
- `repostOfId`
- `repostDetectedAt`
- `listingId`
- `searchQuery`
- `contentQuality`
- `detailFetched`
- `scrapedAt`
- `source`
- `contentHash`
- `changeType`

---

## Sample output

One object per job. Here is a real example from a production run:

```json
{
  "id": "99252220",
  "url": "https://www.ejobs.ro/user/locuri-de-munca/java-developer/99252220",
  "portalUrl": "https://www.ejobs.ro/user/locuri-de-munca/java-developer/99252220",
  "slug": "java-developer",
  "title": "Java developer",
  "company": "HCL TECHNOLOGIES ROMANIA SRL",
  "companyId": 333144,
  "companySlug": "hcltech",
  "companyVerified": true,
  "companyLogoUrl": "https://www.ejobs.ro/img/logos/3/333144.png",
  "companyUrl": "https://www.ejobs.ro/company/hcltech/333144",
  "locations": [
    {
      "cityId": 1,
      "cityName": "București",
      "countyId": 10,
      "countyName": "Bucuresti",
      "address": null,
      "latitude": 44.432,
      "longitude": 26.106,
      "geoPrecision": "city"
    }
  ]
}
```

*Truncated — full records contain 70 fields. See Output fields for the complete schema.*

**[Try Ejobs.ro Job Listings Scraper Romania now — $5 free credit, no credit card →](https://apify.com/blackfalcondata/ejobs-ro-scraper?fpr=1h3gvi)**

---

## Pricing

Pay only for what you extract. No subscription required — Apify's free $5 credit covers thousands of results.

| Event | Price (USD) |
| --- | --- |
| Actor Start | $0.000043 |
| Result | $0.00096 |

See the [actor on Apify](https://apify.com/blackfalcondata/ejobs-ro-scraper?fpr=1h3gvi) for current pricing.

---

## FAQ

**How do I scrape ejobs.ro?**
Use this actor on Apify to extract structured data from ejobs.ro. Configure your search query and filters in the input, then click Start — no coding required.

**How do I get ejobs.ro data as JSON, CSV, or Excel?**
The actor writes each job to Apify's dataset. Download as JSON, CSV, or Excel from the Console, stream via the API, or push to Make, Zapier, Airbyte, or Keboola.

**Is it legal to scrape ejobs.ro?**
Web scraping of publicly available data is generally legal. This actor only accesses publicly visible information. Always check ejobs.ro's terms of service for your specific use case.

**How much does it cost?**
Pay-per-event pricing — you only pay for ${NOUNS} extracted. Apify's free $5 credit is enough to run thousands of results before you pay anything.

**How does incremental mode work?**
Each job gets a content hash. On subsequent runs, only new or changed jobs are emitted — saving time, compute, and storage. Expired jobs can be tracked separately.

**Do I need an API key or credentials?**
No. Just sign up for Apify, paste your input, and click Start. No credit card required.

---

## Related products by Black Falcon Data

[Browse all Black Falcon Data actors →](https://apify.com/blackfalcondata?fpr=1h3gvi)

---

## Getting started with Apify

New to Apify? [Create a free account with $5 credit](https://console.apify.com/sign-up?fpr=1h3gvi) — no credit card required.

1. Sign up — $5 platform credit included
2. Open [Ejobs.ro Job Listings Scraper Romania](https://apify.com/blackfalcondata/ejobs-ro-scraper?fpr=1h3gvi) and configure your input
3. Click **Start** — export results as JSON, CSV, or Excel

Need more later? [See Apify pricing](https://apify.com/pricing?fpr=1h3gvi).

---

## About Black Falcon Data

Black Falcon Data builds production-grade web scrapers for job boards and marketplace data. Browse our full actor catalog at [www.blackfalcondata.com](https://www.blackfalcondata.com).

---

*Last updated: 2026 10*
