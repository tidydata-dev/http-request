# HTTP Request & Bulk API Calls — Python, Node.js & cURL examples

Send **HTTP requests to any API**: **one call** (handy for **Clay**, n8n, Make and Zapier through a callback URL and lookup key) or **thousands in one run**, filling one request template with the **`{{columns}}`** of a CSV, Excel file, Google Sheet or Apify dataset. **Retries, per-host rate limits, JSON field extraction and automatic pagination** included. **$1 per 1,000 requests**; requests that get no response are free.

Hosted on Apify: **[HTTP Request & Bulk API Calls](https://apify.com/tidydata/http-request?fpr=wtg6qt)**. These examples show how to call it from your code; the Actor runs in the cloud, no API key for the source site, no proxies to manage.

## Python

```bash
pip install apify-client
export APIFY_TOKEN=...   # free account at apify.com → Settings → API & Integrations
```

```python
import os
from apify_client import ApifyClient

client = ApifyClient(os.environ["APIFY_TOKEN"])
run = client.actor("tidydata/http-request").call(run_input={
    "url": "https://api.github.com/users/{{user}}",
    "method": "GET",
    "rows": [
        {
            "user": "octocat"
        },
        {
            "user": "torvalds"
        }
    ],
    "extract": {
        "name": "name",
        "followers": "followers"
    },
    "dropResponse": True
})
for item in client.dataset(run["defaultDatasetId"]).iterate_items():
    print({k: item.get(k) for k in ['requestUrl', 'status', 'ok', 'name', 'followers']})
```

## Node.js

```javascript
import { ApifyClient } from 'apify-client';

const client = new ApifyClient({ token: process.env.APIFY_TOKEN });
const run = await client.actor('tidydata/http-request').call({"url": "https://httpbin.org/get", "method": "GET"});
const { items } = await client.dataset(run.defaultDatasetId).listItems();
console.log(items.length, items[0]);
```

## cURL (one call, results as JSON)

```bash
curl -s -X POST "https://api.apify.com/v2/acts/tidydata~http-request/run-sync-get-dataset-items?token=$APIFY_TOKEN" \
  -H "Content-Type: application/json" \
  -d '{"url": "https://httpbin.org/get", "method": "GET"}'
```

## More TidyData Actors

[YouTube Transcript](https://apify.com/tidydata/youtube-transcript-scraper?fpr=wtg6qt) · [YouTube Comments](https://apify.com/tidydata/youtube-comments-scraper?fpr=wtg6qt) · [YouTube Scraper](https://apify.com/tidydata/youtube-channel-scraper?fpr=wtg6qt) · [Website Contacts](https://apify.com/tidydata/website-contact-extractor?fpr=wtg6qt) · [Google News](https://apify.com/tidydata/google-news-scraper?fpr=wtg6qt) · [App Reviews](https://apify.com/tidydata/app-reviews-scraper?fpr=wtg6qt) · [Merge & Dedupe](https://apify.com/tidydata/merge-dedupe-datasets?fpr=wtg6qt) · [all of them](https://github.com/tidydata-dev)

Examples are MIT-licensed.

---
<sub>Links to apify.com include our affiliate ID: if you start a paid Apify plan through them, Apify pays TidyData a commission at no extra cost to you.</sub>
