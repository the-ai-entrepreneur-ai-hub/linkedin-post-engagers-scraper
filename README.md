# LinkedIn Post Engagers Scraper — LinkedIn Leads from Comments & Reactions

Scrape **LinkedIn post engagers** — every commenter and reaction on any LinkedIn post — and
turn them into an ICP-filtered lead list with the engagement signal attached. This is
**LinkedIn lead generation from real intent**: people who just engaged with a post in your
niche are raising their hand about that topic, and their comment text is your outreach
opening line.

[![Run on Apify](https://img.shields.io/badge/Run%20on-Apify-00A950)](https://apify.com/george.the.developer/linkedin-post-engagers-scraper)

**Hosted version:** [LinkedIn Leads Scraper: Post Engagers, Comments & Reactions](https://apify.com/george.the.developer/linkedin-post-engagers-scraper) — no login, no cookies, no browser extension, pay only for leads that match your filters.

## What it does

```
LinkedIn post URLs
   → collect commenters + reactions (like, celebrate, insightful, ...)
   → dedupe people across posts + your suppression list
   → filter by job-title keywords (your ICP)
   → one lead row per match, with the engagement signal block
```

Each lead row: `name`, `headline`, `profileUrl`, `engagementType` (comment/reaction),
`commentText` (their words — the personalization hook), `reactionType`, `postUrl`.

## Quick start (API)

```bash
curl -X POST "https://api.apify.com/v2/acts/george.the.developer~linkedin-post-engagers-scraper/run-sync-get-dataset-items?token=$APIFY_TOKEN" \
  -H "Content-Type: application/json" \
  -d '{
    "postUrls": ["https://www.linkedin.com/posts/USERNAME_slug-activity-1234567890-abcd"],
    "titleIncludeKeywords": ["founder", "marketing", "growth"],
    "titleExcludeKeywords": ["student", "intern"]
  }'
```

Node.js:

```js
import { ApifyClient } from 'apify-client';

const client = new ApifyClient({ token: process.env.APIFY_TOKEN });
const run = await client.actor('george.the.developer/linkedin-post-engagers-scraper').call({
    postUrls: ['https://www.linkedin.com/posts/USERNAME_slug-activity-1234567890-abcd'],
    titleIncludeKeywords: ['founder', 'marketing', 'growth'],
});
const { items } = await client.dataset(run.defaultDatasetId).listItems();
console.log(items.filter((r) => r.recordType === 'lead'));
```

## Example output

```json
{
  "recordType": "lead",
  "name": "Jane Doe",
  "headline": "Head of Growth Marketing at Acme",
  "profileUrl": "https://www.linkedin.com/in/jane-doe",
  "engagementType": "comment",
  "commentText": "We tried exactly this last quarter — the attribution part is the hard bit.",
  "postUrl": "https://www.linkedin.com/posts/...",
  "billable": true
}
```

## Why engagement leads beat cold lists

- Manual way: a VA filters a 5,000-engager post by hand at ~2 min/profile ≈ **160 hours ≈ $2,400**.
- This scraper: the same post filtered to your ICP for **$0.005 per matched lead** — a few dollars.
- Cold lists tell you who someone is. Engagement tells you what they care about **right now**.

## Use cases

1. **Signal-based outbound** — mail people who just engaged with your topic; open with their own comment.
2. **Competitor audience capture** — run it on a competitor's launch posts.
3. **Influencer audience mining** — schedule it on the 5–10 creators your buyers follow.
4. **Event & launch follow-up** — collect everyone engaging with a conference hashtag post.

## What it does NOT do (honest limits)

- **No emails.** You get clean profile URLs + signal; pipe them into Clay, Apollo or your
  own enrichment stack. Email enrichment with bring-your-own verification is on the roadmap.
- **No private data.** Public post engagement only. No login-walled profiles, no InMail, no
  connection requests, no message sending.
- The underlying engagement scrape runs under your Apify account (~$2 per 1,000 engagers,
  billed by the scraper it orchestrates) — the ICP filter saves money on leads, not on the scrape.

## FAQ

**How do I scrape LinkedIn post commenters?** Paste the post URL, keep comments enabled,
run — every commenter returns with name, headline, profile URL and comment text.

**How do I extract people who liked a LinkedIn post?** Reactions are collected in the same
run with the reaction type (like, celebrate, insightful, love) on each row.

**Do I need Sales Navigator or a LinkedIn account?** No. Public engagement data only.

**Can I export to CSV / Excel / JSON?** Yes — every run's dataset exports in all formats
from the Apify Console or API.

## Related tools

- [LinkedIn Company Employees Scraper](https://github.com/the-ai-entrepreneur-ai-hub/linkedin-company-employees-email-scraper)
- [Google Maps Email Extractor](https://github.com/the-ai-entrepreneur-ai-hub/google-maps-email-extractor)
- [Job Posts to Buyer Emails](https://github.com/the-ai-entrepreneur-ai-hub/job-posts-to-buyer-emails)

## License

MIT
