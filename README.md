# linkedin company scraper api: What 30 Credits per Request Really Buys, Current Plan Pricing, and How the Main Options Compare in Independent Tests

Short answer first: a LinkedIn company scraper API takes a company's public /company/ page and returns structured data — industry, company size, employee count, headquarters, website, follower count, specialties — in one call. Because LinkedIn's official API is gated behind Partner Program approval, third-party scraper APIs are the practical route for arbitrary public company data. The part most listings hide from you: on general-purpose APIs, LinkedIn requests cost far more than normal pages, and success rates on LinkedIn differ enormously between providers. This article walks through the real per-request credit math, ScraperAPI's full current plan table, independent benchmark results, and when a general API is the right pick versus a LinkedIn-dedicated alternative.

> **Quick take:** ScraperAPI is a general web scraping API that bills LinkedIn at **30 credits per request** — its cheapest paid plan ($49/mo) covers roughly **3,300 company lookups per month**. New accounts get 5,000 free credits for 7 days (no credit card) plus a permanent 1,000-credit free plan, and official docs state only successful requests are billed. It fits teams already using one API for many targets. If LinkedIn is your *only* target, a dedicated provider or the benchmark's top performer will serve you better — details below. 👉 [Check current plans and start the 5,000-credit trial](https://www.scraperapi.com/pricing?fp_ref=coupons)

## What data can a LinkedIn company scraper API actually return?

A company scraper API reads the public, logged-out /company/ page and pulls whatever LinkedIn exposes there. Working examples across current tools return the same core field set: company name, industry, company size range (e.g. "5,001–10,000 employees"), a precise employee count where available, headquarters location, external website URL, follower count, company type, founding year, specialties, and the logo. One public API response for Stripe illustrates the shape: 16,979 employees, South San Francisco HQ, founded 2010, over 1.5 million followers — all from a single public page.

Two limits to settle before building anything:

- **Public-only.** Everything behind the login wall — full employee profiles, connections, internal analytics — is out of scope. A company scraper gets firmographics, not your competitor's staff list.
- **Real-time, not a database.** Most providers fetch the live page on each call rather than serving cached records, which is good for accuracy and bad for bulk historical pulls.

One quirk matters before you write code: LinkedIn serves company data through its internal (Voyager) API rather than plain HTML, and its authwall page returns **without an error status code**. An authwall can look like a successful 200 response. Validate the body, never the status alone.

## Why the official LinkedIn API doesn't solve this

The official API can't hand you arbitrary company data, which is the entire reason this product category exists. Company and profile data require Partner Program approval, and member data additionally requires that member's own OAuth consent. There is no path to "give me any company slug's public page" through the first-party API — it's the compliant route only for data your app is already authorized to access, and it's free within that approved scope.

That leaves three third-party families:

1. **General scraping APIs** (ScraperAPI, Scrapfly, WebScrapingAPI, Scrapingdog) — one API for any URL, with proxy rotation and anti-bot handling as the core product.
2. **LinkedIn-dedicated APIs** — specialized endpoints for companies, profiles, jobs and posts, typically billed per successful request.
3. **Browser automation and dataset platforms** — Apify actors for one-off exports, Bright Data for enterprise-scale record purchases, PhantomBuster for running automation through your own logged-in session, with the ban risk landing on your account.

## ScraperAPI plans and pricing — the full table

ScraperAPI prices by credits, and every plan includes JS rendering, premium residential and mobile IPs, auto-parsing, CAPTCHA handling, automatic retries, unlimited bandwidth and a 99.9% uptime guarantee. Annual billing takes 10% off each tier. New users get a 7-day trial with 5,000 credits (no credit card required) and a permanent free plan of 1,000 credits capped at 5 concurrent connections.

| Plan | Price (monthly / annual) | API credits | Concurrent threads | LinkedIn lookups included* | Get started |
| --- | --- | --- | --- | --- | --- |
| Free | $0 | 1,000 | 5 | ~33 | [Sign up free](https://www.scraperapi.com/pricing?fp_ref=coupons) |
| Hobby | $49 / $44.10 | 100,000 | 20 | ~3,333 | [Start Hobby](https://www.scraperapi.com/pricing?fp_ref=coupons) |
| Startup | $149 / $134.10 | 1,000,000 | 50 | ~33,333 | [Start Startup](https://www.scraperapi.com/pricing?fp_ref=coupons) |
| Business | $299 / $269.10 | 3,000,000 | 100 | ~100,000 | [Start Business](https://www.scraperapi.com/pricing?fp_ref=coupons) |
| Scaling | $475 / $427.50 | 5,000,000 | 200 | ~166,666 | [Start Scaling](https://www.scraperapi.com/pricing?fp_ref=coupons) |
| Professional | $975 / $877.50 | 10,500,000 | 300 | ~350,000 | [Start Professional](https://www.scraperapi.com/pricing?fp_ref=coupons) |
| Advanced | $1,975 / $1,777.50 | 21,500,000 | 500 | ~716,666 | [Start Advanced](https://www.scraperapi.com/pricing?fp_ref=coupons) |
| Enterprise | Custom | 22,000,000+ | 500+ | Custom | [Contact sales](https://www.scraperapi.com/pricing?fp_ref=coupons) |

*LinkedIn lookups = credits ÷ 30, per the official per-domain pricing. If the free tier runs out mid-evaluation, support can issue extra testing credits on request.

## The real cost of LinkedIn company lookups: 30 credits per request

This is the number comparison articles usually skip, and it decides which plan makes sense. ScraperAPI's official pricing FAQ is explicit: a standard page costs 1 credit, Amazon 5, Google and Bing (including subdomains) 25 — and **LinkedIn 30**. So the tempting mental math of "$49 buys 100,000 requests" collapses the moment your target is a company page. Divide by 30 and the Hobby plan covers about **3,333 LinkedIn company fetches a month**, which works out to roughly $14.70 per thousand LinkedIn requests before any parameter surcharges.

The same 100,000 Hobby credits would alternatively buy 100,000 ordinary pages, 20,000 Amazon pages, or 4,000 SERP queries. If your workload is mixed — heavy Google and Amazon scraping with a few thousand company lookups on the side — the credit system is friendlier than the LinkedIn-only math suggests.

Before scaling up, use the cost controls that are actually built in:

- The **Domain Cost Estimator** in the dashboard (or the `urlcost` API endpoint) returns the exact credit cost of any URL before you commit a batch.
- `max_cost` caps what a single request can consume.
- Every response carries a `sa-credit-cost` header showing actual spend for that call.
- Anti-bot bypasses cost extra when triggered: Cloudflare, Datadome, PerimeterX and Akamai add 10 credits per scrape. Parameter surcharges stack too — `render=true` adds 10, `premium=true` adds 10, and `ultra_premium=true` adds 30 (paid plans only). For most public company pages, a plain request without rendering is the sensible default; escalate only when a target refuses to respond.

The trial math is worth knowing too: 5,000 trial credits equal about **166 LinkedIn company requests**. Enough to validate field coverage on a real target list before spending anything. 👉 [Test the cost estimator against your own company URLs before paying](https://www.scraperapi.com/pricing?fp_ref=coupons)

## How well does it actually work on LinkedIn?

An independent monthly benchmark that runs LinkedIn as a dedicated test target (data from September, refreshed monthly, no sponsors or affiliate links) put ScraperAPI at a **57.7% success rate on LinkedIn** — well behind Scrapfly's 97.4% and WebScrapingAPI's 82%, ahead of Scrapingdog's 20%. Overall across all targets ScraperAPI averaged 52.3%, but the per-target spread is more revealing: 96.2% on Etsy and 90.2% on Amazon, yet 1.1% on Booking and 24.2% on Realtor. LinkedIn sits in the middle of a very uneven distribution.

Three findings from that benchmark affect real costs:

- **Blocked requests are often still billed.** The benchmark's own critique of ScraperAPI reads "Often charges for blocked requests." Combined with the official billing rule (only 200 and 404 responses are charged) and the authwall behavior described above, the mechanism is clear: LinkedIn's authwall returns a 200 status without data, so it bills as a success. At 57.7% success, your effective cost per *retrieved company record* runs meaningfully above the headline $14.70 per thousand — treat that figure as a floor, not a quote.
- **A forced cache on hard targets.** Difficult domains get a 10-minute result cache, which the benchmark flags as producing stale data. For company pages that change slowly this is rarely a problem; for anything where freshness matters within minutes, it's a real limitation.
- **Speed is the genuine strength.** When a request clears, ScraperAPI tends to return quickly (7.5 seconds average across targets versus Scrapfly's 17.8), which suits latency-sensitive lookups where you can absorb retries.

One data-quality note that applies to every provider in this space, including the benchmark itself: results are per-run snapshots. A 58% month can be a 70% month later — the direction to plan around is that general APIs pay for LinkedIn access in retries and effective cost, by design.

## Fine print that changes decisions

Three plan-level restrictions only surface when you read the pricing page closely:

- **Geotargeting is capped below Business.** Hobby and Startup target only the US and EU. Country-level targeting starts at Business ($299/mo), and even the top tier's geo pool is limited (13 countries) — far smaller than some competitors' 50+. If requests must originate from specific countries, the two cheapest plans don't qualify.
- **Concurrency is a hard wall.** Exceed your thread limit and you get a 429; the request is turned away rather than queued, with no extra charge. At 20 threads on Hobby, a parallelized company-list crawl hits this fast — build retry logic with backoff from day one.
- **Analytics history is 30 days on the cheap tiers.** Hobby and Startup dashboards keep only the last 30 days of stats; Business and up retain everything.

Operational details round this out: when credits run out, scraping pauses until renewal by default. Auto-upgrade (opt-in, Hobby through Professional) bumps you to the next plan automatically; pay-as-you-go at a fixed per-credit rate with a monthly cap exists only from Scaling upward. And **credits don't roll over month to month** — unused balance vanishes at renewal, so size your plan to what you'll actually consume.

## How the alternatives compare — and when they beat ScraperAPI

Honest positioning: if LinkedIn company data is your only target and volume is meaningful, dedicated LinkedIn APIs generally deliver higher success rates at lower per-record cost, because they're built around LinkedIn's logged-out pages instead of treating it as one hard domain among thousands. One such provider publishes pricing of $0.60 per 1,000 requests with a 1,000-request free tier and reports a ~2.6 second median response; that figure comes from the provider's own site, so verify against your own trial before committing.

The field, information only — no links, since none of these are recommendations with a checkout path from this article:

| Option | Pricing model | Free tier | Notable trait |
| --- | --- | --- | --- |
| Official LinkedIn API | Free within Partner-approved scope | Approval required | Can't pull arbitrary public company pages |
| Scrapfly | From $30/mo | 1,000 credits (one-time) | 97%+ LinkedIn success in the Sept benchmark; slower responses; no annual plans |
| WebScrapingAPI | From $19/mo | — | 82% LinkedIn success; broad language SDK support |
| Scrapingdog | From $40/mo | — | Markets a dedicated LinkedIn endpoint; 20% success in the same test |
| LinkedIn-dedicated APIs | ~$0.60–0.90 per 1k requests | 1,000 requests | One key covers companies, profiles, jobs, posts; success-only billing |
| Apify actors | Pay per result/usage | Monthly free credits | One actor per job type; convenient for one-offs, clunky to stitch together at scale |
| Bright Data | Per record / subscription | Trial on signup | Enterprise volume; often returns datasets or raw pages you parse yourself |
| PhantomBuster | Subscription, per slot | Limited trial | Runs through your own logged-in session; ban risk sits on your account |

Reading this fairly: Scrapfly's 97.4% LinkedIn success at a lower entry price makes it the stronger pure-LinkedIn performer in that independent test, and if company scraping is your sole use case, it deserves the top of your shortlist. WebScrapingAPI undercuts nearly everyone on cost per thousand. ScraperAPI's case is the unified one — one key and one billing system across every domain in your stack, with published per-domain pricing and fast responses when requests clear. Capterra rates it 4.6 from 62 reviews, against Scrapfly's 4.9 from 237. If you also scrape Amazon, Google and e-commerce daily, one general provider beats juggling a LinkedIn specialist plus a generalist; if LinkedIn is the whole job, the benchmark numbers point elsewhere.

## FAQ

**How many LinkedIn company lookups does the cheapest ScraperAPI plan include?**
About 3,333 per month on Hobby ($49), calculated from the official 30-credit cost per LinkedIn request against 100,000 monthly credits. The free plan covers roughly 33 lookups.

**Do failed LinkedIn requests cost credits on ScraperAPI?**
Officially, only responses with 200 or 404 status (plus requests you cancel within 70 seconds) are billed. But LinkedIn's authwall returns 200 status without real data, and an independent benchmark specifically flags ScraperAPI for often charging on blocked requests — so log success by response content, and assume failed-looking 200s are billed.

**Can ScraperAPI return structured company JSON?**
Yes. Auto-parsing (`autoparse=true`) costs no extra credits and all plans include structured data parsing APIs. Expect the public-page fields: name, industry, size, employee count, HQ, website, followers, specialties.

**Is scraping public LinkedIn company data legal?**
LinkedIn's terms restrict automated access, and the risk profile differs by method: general APIs and dedicated scraper APIs use no personal account, while session-based tools like PhantomBuster run through your own logged-in profile and carry account-ban risk. The official API is the compliant first-party route but requires partner approval. This isn't legal advice — check your own jurisdiction and use case.

**What happens when I run out of credits mid-project?**
Scraping pauses until renewal unless you've enabled auto-upgrade (Hobby through Professional) or pay-as-you-go (Scaling and up, capped at a monthly spend you set).

## Before you pick a plan

Match the tool to the workload. If LinkedIn company data is one output among many, ScraperAPI's single-key setup, published credit costs and 5,000-credit trial make it an easy default — start on Hobby, watch your `sa-credit-cost` headers, and upgrade when the ~3,300-request ceiling gets tight. If it's your entire workload, the independent numbers favor a dedicated LinkedIn API or Scrapfly on both success rate and per-record cost, and no comparison table changes that. If you need country-level targeting below Business, or you're burning through 20 threads on a large company list, size up a level before the 429s start. One last check before checkout: confirm your monthly volume against the credits table above, since unused credits expire at each renewal. 👉 [Start the 5,000-credit trial and benchmark it on your own company list](https://www.scraperapi.com/pricing?fp_ref=coupons)
