# FEC Campaign Finance Scraper: Candidates & Donors

Scrape US federal campaign finance from the official OpenFEC API as clean JSON. Search candidates and committees by name, or pull itemized Schedule A contributions for a committee. No login, no browser needed.

**Run it on Apify:** [apify.com/themineworks/fec-campaign-finance](https://apify.com/themineworks/fec-campaign-finance)
**Docs, FAQ and pricing:** [themineworks.com/actors/fec-campaign-finance](https://themineworks.com/actors/fec-campaign-finance/)

**Price:** From $0.60 per 1,000 records on Apify's higher plans ($1.00 on the free plan), plus a $0.005 start fee per run. Failed and empty results are never charged.

## What it returns

* Candidate search: party, office, state, election years
* Committee/PAC search by name
* Itemized Schedule A contributions by committee
* Official OpenFEC API, DEMO_KEY works for small tests
* Empty results are never charged

## Quick start

You need a free [Apify account](https://console.apify.com/sign-up) and its API token (Settings, API & Integrations).

### Python

```bash
pip install apify-client
```

```python
from apify_client import ApifyClient

client = ApifyClient("YOUR_APIFY_TOKEN")
run = client.actor("themineworks/fec-campaign-finance").call(run_input={
    "query": "Warren",
    "committeeId": "C00401224",
    "maxResults": 25
})

for item in client.dataset(run["defaultDatasetId"]).iterate_items():
    print(item)
```

### Node.js

```bash
npm install apify-client
```

```javascript
import { ApifyClient } from 'apify-client';

const client = new ApifyClient({ token: 'YOUR_APIFY_TOKEN' });
const run = await client.actor('themineworks/fec-campaign-finance').call({
    "query": "Warren",
    "committeeId": "C00401224",
    "maxResults": 25
});
const { items } = await client.dataset(run.defaultDatasetId).listItems();
console.log(items);
```

### cURL

One request that runs the actor and returns the results in the response (for runs under 5 minutes):

```bash
curl -X POST "https://api.apify.com/v2/acts/themineworks~fec-campaign-finance/run-sync-get-dataset-items?token=YOUR_APIFY_TOKEN" \
  -H "Content-Type: application/json" \
  -d '{"query": "Warren", "committeeId": "C00401224", "maxResults": 25}'
```

### Command line

This repo includes ready-made clients that save results to JSON and CSV:

```bash
python3 fec_campaign_finance_scraper.py --token YOUR_APIFY_TOKEN --query "Warren" --committee-id "C00401224" --max-results "25"
node fec_campaign_finance_scraper.mjs --token YOUR_APIFY_TOKEN --query "Warren" --committee-id "C00401224" --max-results "25"
```

## Input

| Field | Type | Default | Description |
|---|---|---|---|
| `mode` | string | `"candidates"` | What to fetch |
| `query` | string |  | Name to search, for candidates or committees modes (for example Warren, ActBlue) |
| `committeeId` | string |  | FEC committee ID (for example C00401224), required for contributions mode |
| `maxResults` | integer | `25` | Maximum number of rows to return |

## Output

One row per result, as JSON, CSV, Excel or through the API.

| Field | Type | Description |
|---|---|---|
| `_mode` | string | Row type: candidate, committee or contribution |
| `candidate_id` | string | FEC candidate ID |
| `committee_id` | string | FEC committee ID |
| `name` | string | Candidate or committee name |
| `party` | string | Party (full name) |
| `office` | string | Office sought (candidates) |
| `state` | string | State |
| `district` | string | District (candidates) |
| `incumbent_challenge` | string | Incumbent / challenger / open seat (candidates) |
| `election_years` | array | Election years (candidates) |
| `committee_type` | string | Committee type (committees) |
| `treasurer_name` | string | Committee treasurer (committees) |
| `contributor_name` | string | Contributor name (contributions) |
| `contributor_employer` | string | Contributor employer (contributions) |
| `contributor_occupation` | string | Contributor occupation (contributions) |
| `contributor_state` | string | Contributor state (contributions) |
| `amount` | string | Contribution receipt amount in USD (contributions) |
| `date` | string | Contribution receipt date (contributions) |
| `recipient_committee` | string | Recipient committee name (contributions) |
| `scraped_at` | string | ISO timestamp when this record was scraped |

## Use it from an AI agent

The actor works as a tool in Claude, Cursor or any MCP client through Apify's MCP server:

```
https://mcp.apify.com/?tools=themineworks/fec-campaign-finance
```

## FAQ

### Where does the data come from?

The official OpenFEC API (api.open.fec.gov), the Federal Election Commission’s own public data source, the same one fec.gov itself uses.

### Do I need an API key?

No for small tests. FEC’s public DEMO_KEY works but is rate-limited. Get a free api.data.gov key in seconds for real volume.

### Can I look up a specific committee’s donors?

Yes. Search committees mode to find the committee ID, then contributions mode with that ID to get itemized donations.

### What does it cost?

You pay per result actually delivered: a candidate, committee, or contribution row. Failed and empty results are never charged.

### What can I pull besides candidates?

Committee and PAC records by name, and itemized Schedule A contributions for a given committee.

### Can I filter candidates?

Yes, by party, office, state and election year.

### Is the DEMO_KEY enough?

For small tests, yes. For sustained pulls, register a free api.data.gov key, which lifts the rate limit well above the shared demo allowance.

### Can I export the results to CSV or Excel?

Yes. Every run saves to an Apify dataset you can download as JSON, CSV, Excel or XML, or read through the API. The Python and Node clients in this repo also write the results to local files.

### Can I run it on a schedule?

Yes. Save your input as a task on Apify and attach a schedule, or call the API from your own cron job. Scheduled runs are billed the same way as manual ones.

## Related scrapers

* [India Gov Data](https://themineworks.com/actors/india-data-gov-scraper/): Trade, prices, and thousands of datasets from data.gov.in
* [CourtListener Scraper](https://themineworks.com/actors/courtlistener-court-records/): US court opinions, dockets, and case law as structured JSON
* [Socrata Open Data](https://themineworks.com/actors/socrata-open-data/): Any government data portal: CDC, HHS, NYC, Texas, and hundreds more

Part of [The Mine Works](https://themineworks.com/): 151 pay-per-result scrapers with no login and no browser setup on your side.

## License

MIT © The Mine Works
