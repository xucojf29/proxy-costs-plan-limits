# proxy service for scraping: Real Credit Costs, Hidden Plan Limits, and How to Pick the Right Setup Without Overpaying

Searching for a proxy service for scraping usually means one of two things: your scraper just started getting blocked by a site that worked fine last month, or you're starting a new project and want to avoid burning money on the wrong kind of proxy. Either way, you face the same fork — raw proxy lists you manage yourself, or a managed scraping API that handles rotation, retries, and CAPTCHAs for you.

The short answer: if your targets are standard e-commerce, search, or content sites and you'd rather not build your own proxy infrastructure, a managed API like ScraperAPI gets you from signup to a working scraper in minutes. Plans start at **$49/month** for 100,000 API credits, there's a permanent free plan with 1,000 credits, and a 7-day no-questions-asked refund policy on paid subscriptions. If you need raw IP control — specific subnets, long-lived sessions for account work — a raw proxy provider fits better, and the numbers below will tell you that too.

> **Quick verdict:** For most developers scraping 20,000–200,000 pages a month against normal websites, test on the free trial (5,000 credits, 7 days, no credit card), then start on Hobby at $49/month. Scraping Google or LinkedIn specifically? Read the credit-cost section first — those pages cost far more than 1 credit each. 👉 [Check ScraperAPI's full plan list and current prices](https://www.scraperapi.com/pricing?fp_ref=coupons)

## Two kinds of proxy services for scraping — and which one fits your project

A raw proxy provider sells you IPs. Datacenter, residential, ISP, or mobile, billed per GB or per IP, with you writing the rotation logic, retry handling, header management, and CAPTCHA solving. A scraping API like ScraperAPI sells you successfully scraped pages: you send a URL to one endpoint, and the service picks the proxy, rotates it, retries on failure, solves CAPTCHAs, and optionally renders JavaScript before returning the HTML.

A practical rule from operators who run scrapers at scale: start with datacenter proxies, which cost roughly a tenth of residential bandwidth, and only move a specific domain to residential once its block rate passes about 5% — measured per domain, not across your whole operation. Residential bandwidth spent on sites that never needed it is the most common way scraping budgets evaporate.

On pool sizes: ScraperAPI advertises 40M+ proxies across residential, datacenter, and mobile ranges. Treat that the same way you'd treat a competitor's "400M+ IPs" — as a marketing number. No provider, ScraperAPI included, publishes how many of those IPs are actually clean for your target site. What matters is success rate on your targets, which you can only establish by testing, which is exactly what free trials are for.

The honest trade-off with the API model: you give up IP-level control. ScraperAPI lets you target a country with the `country_code` parameter, but there's no dashboard for picking specific IPs or ASNs. If your project depends on pinning exact IPs — account management, ad verification — you want a raw provider with ISP proxies instead. Everyone else usually prefers never thinking about IPs at all.

## ScraperAPI plans: every tier and what actually differs between them

ScraperAPI currently shows eight tiers on its pricing page. All paid plans include the same feature set — JavaScript rendering, premium residential and mobile pools, CAPTCHA handling, structured-data endpoints, DataPipeline, unlimited bandwidth, and a 99.9% uptime guarantee. Tiers differ in credits, concurrency, and geo-targeting depth.

| Plan | Monthly price | Credits/mo | Threads | Geo targeting | Get it |
| --- | --- | --- | --- | --- | --- |
| Free | $0 | 1,000 | 5 | US & EU regions | [ Sign up free](https://www.scraperapi.com/?fp_ref=coupons) |
| Hobby | $49 | 100,000 | 20 | US & EU regions | [ Get Hobby](https://www.scraperapi.com/pricing?fp_ref=coupons) |
| Startup | $149 | 1,000,000 | 50 | US & EU regions | [ Get Startup](https://www.scraperapi.com/pricing?fp_ref=coupons) |
| Business | $299 | 3,000,000 | 100 | Country-level | [ Get Business](https://www.scraperapi.com/pricing?fp_ref=coupons) |
| Scaling | $475 | 5,000,000 | 200 | Country-level | [ Get Scaling](https://www.scraperapi.com/pricing?fp_ref=coupons) |
| Professional | $975 | 10,500,000 | 300 | Country-level | [ Get Professional](https://www.scraperapi.com/pricing?fp_ref=coupons) |
| Advanced | $1,975 | 21,500,000 | 500 | Country-level | [ Get Advanced](https://www.scraperapi.com/pricing?fp_ref=coupons) |
| Enterprise | Custom | 22,000,000+ | 500+ | Country-level | [ Talk to sales](https://www.scraperapi.com/pricing?fp_ref=coupons) |

Details the table can't show:

- **Hobby and Startup are locked to US and EU geotargeting.** The full country-level list only unlocks at Business ($299/month) and up. If your project needs Japanese or Brazilian IPs, the cheaper plans won't get you there at any credit count.
- **Annual billing takes 10% off**, dropping Hobby to $44.10/month and Scaling to $427.50/month — about $58 and $570 saved per year respectively. Worth it only if you're sure you'll still be scraping in a year.
- **Credits reset each month and don't roll over.** Unused credits from a light month are gone at renewal — though annual-plan subscribers can renew early to pull in a fresh batch of credits instead of waiting out the year.
- **Running dry mid-month behaves differently by plan.** Monthly plans from Hobby through Professional can auto-upgrade to the next tier (which usually means a better per-credit rate). Scaling, Professional, Advanced, and Enterprise can instead use Pay-As-You-Go at a fixed per-credit rate, capped by a monthly spending limit you set — and per the official docs, that cap is a hard limit, not a pre-charge.

If none of the standard tiers fits, support can build custom plans, and you can cancel anytime from the dashboard without a cancellation fee.

## The credit system: where the real cost of this proxy service lives

This is the part most reviews skip, and it's the difference between the right plan and an expensive surprise.

A standard page costs 1 credit. Harder targets cost more, and the multipliers add up fast:

| Request type | Credits per request |
| --- | --- |
| Standard GET (default pool) | 1 |
| JavaScript rendering (`render=true`) | 10 |
| Premium pool (`premium=true`) | 10 |
| Premium + rendering | 25 |
| Ultra-premium (`ultra_premium=true`) | 30 |
| Ultra-premium + rendering | 75 |

Some domains carry fixed costs on top of that. Per ScraperAPI's pricing page: **Amazon pages cost 5 credits, Google and Bing (including subdomains) cost 25, and LinkedIn costs 30** — before rendering. Sites protected by Cloudflare, Datadome, or PerimeterX add another 10 credits each time their protection gets bypassed.

Run the math on a real workload and the headline numbers change meaning. Startup's 1,000,000 credits is a million basic pages — but only about 40,000 Google result pages at 25 credits each, or roughly 33,000 LinkedIn pages at 30. Add rendering on top and the count drops further. A job that needs `render=true` plus `premium=true` on a protected target can cost 20–30x the headline per-page rate. That's how the underlying proxy economics work; the hard stuff genuinely is harder. Just budget against your real per-page cost, not the credit count on the pricing page.

Three tools keep this under control, and all are free on every plan:

1. The **Domain Cost Estimator** in your dashboard shows the exact credit cost of any URL before you scrape it.
2. The `max_cost` parameter caps credits per request, so one unexpectedly stubborn page can't drain your balance.
3. Failed requests don't burn credits. Only successful responses are charged, and internal retries are free — ScraperAPI's docs put the expected failure rate at 1–3%, with support available to tune setups that exceed 10%.

## Policies and limits buried in the docs

A few things that affect real usage but never make it onto pricing pages:

- **Refunds:** 7 days, no questions asked, handled through support. Cancel anytime from the dashboard; you won't be charged for cancelling.
- **No bandwidth billing, no single-IP purchases.** Everything is billed per request. You can't buy "just 10 GB" or one dedicated IP.
- **Latency runs 4–12 seconds on average**, depending on the target site, with individual requests allowed up to 60 seconds before failing. Factor this into concurrency planning: throughput is threads × requests per minute, and slow targets eat both.
- **JavaScript rendering has its own burst limit** — 10 render requests per second by default, separate from your plan's thread count. Higher render rates are Enterprise-only. If your workload is render-heavy, this wall often arrives before the credit wall.
- **Bulk jobs go through the Async API, not the standard endpoint.** The standard API handles one URL per request; for larger jobs, the Async Jobs API accepts batch submissions of up to 50,000 URLs, retries in the background for up to 24 hours, and can deliver results via webhook or polling, with completed results kept for up to 72 hours.
- **Page interaction is limited to what the rendering instruction set supports** — clicking, typing, scrolling, and waiting for events before the rendered HTML comes back. That covers a lot of "click to load more" cases, but you're not driving a full browser session the way Selenium or Puppeteer would.

None of these are dealbreakers for typical scraping jobs. They become relevant fast if your project is actually browser automation wearing a scraping costume.

## How the pricing compares to raw proxy providers

For context, current published rates from the raw-proxy side: Bright Data lists residential pay-as-you-go at $4.00/GB (list price $8, with promotional periods at half price), Oxylabs starts around $6/GB with volume tiers down to $5/GB, IPRoyal charges $7/GB pay-as-you-go but drops to $1.75/GB at the 10 TB+ custom tier with no traffic expiry, and Decodo (formerly Smartproxy) sits at $4.00/GB. Webshare sells datacenter proxies from $2.99/month for 100 IPs and runs a permanent free tier with 10 proxies and 1GB.

On the scraping-API side, ScrapingBee starts at $19/month for 75,000 credits, and WebScraping.AI at $29/month for 250,000 credits — both cheaper per credit than ScraperAPI's $49 for 100,000, if raw credit price is your only criterion.

These numbers don't translate directly. A GB of residential bandwidth is raw traffic; a ScraperAPI credit is a successfully scraped page including rotation, retries, and CAPTCHA handling. At large scale against simple targets, raw datacenter proxies will almost always win on the invoice. The managed model wins on engineering hours — you're paying someone else to maintain the rotation, unblocking, and parsing infrastructure.

## Who shouldn't buy ScraperAPI

Saying this plainly, because it saves some readers the subscription entirely:

- **Account management and ad verification.** These need a stable IP identity over long periods and control over which IP you're on. Sticky sessions here are relatively short-lived, and there's no IP/ASN selection. Raw residential or ISP providers are the right tool.
- **Very low volumes.** Under ~1,000 basic pages a month, the free plan covers you. Don't subscribe.
- **Industrial-scale scraping against heavy anti-bot targets** like Cloudflare bot management or Akamai. At 30–75 credits per page, price out a dedicated enterprise solution against your actual numbers before committing.

For price monitoring, SERP tracking, lead generation, and catalog syncing against standard e-commerce sites, the default pool clears most requests without touching the premium tiers — that's the sweet spot.

## Getting started without spending anything

The trial path costs nothing and needs no card:

1. 👉 [Create a free ScraperAPI account](https://www.scraperapi.com/?fp_ref=coupons) — 5,000 credits for your first 7 days, no credit card required.
2. Send a single GET request with your API key and confirm your target returns clean HTML.
3. Add the harder flags one at a time: `render=true` for JavaScript-heavy pages, `premium=true` if you're getting blocked, `country_code` if location matters.
4. Check the Domain Cost Estimator for your target URL, multiply by monthly volume, and pick the matching tier.

When the trial ends, the account drops to the permanent free plan — 1,000 credits and 5 concurrent connections, still enough for small ongoing jobs.

## Quick answers

**Does ScraperAPI charge for failed requests?** No. Only successful responses consume credits, including internal retries. If your failure rate exceeds roughly 10%, support can help tune the setup.

**Is there a money-back guarantee?** Yes — a 7-day no-questions-asked refund on subscriptions, processed through support. Cancellation itself is always free.

**Is there a discount or coupon code?** Annual billing gives 10% off every paid plan. ScraperAPI doesn't publish coupon codes on its pricing page; if a third-party site claims a working code, verify it at checkout before building your budget around it.

**Can I scrape Google or LinkedIn with it?** Yes — those domains have dedicated credit costs (25 and 30 respectively), and the pricing model explicitly prices for them. On a million-credit plan, that works out to roughly 40,000 Google pages or 33,000 LinkedIn pages, less if you enable rendering.

**How slow is it?** Expect 4–12 seconds per request on average, depending on the site. Rendering requests take longer still. Plan your concurrency accordingly.

## Which plan to actually pick

Working backward from real situations:

- Evaluating, or under 1,000 basic pages a month → stay on the **free plan**.
- Standard sites, 50K–100K pages a month, US/EU targets → **Hobby at $49/month**.
- Around a million pages monthly on standard targets → **Startup at $149/month**.
- Country-level geotargeting needed, or 100+ threads → **Business at $299/month**; there's no cheaper path to full geo.
- Google, LinkedIn, or protected targets → run the credit math first; you'll likely land on **Business or Scaling**, where Pay-As-You-Go also kicks in as a safety net.
- Past ~20 million credits a month → **Enterprise** with custom pricing.

One last check before you pay: run your actual target URL through the Domain Cost Estimator and confirm the credits-per-page number matches what you assumed. It takes two minutes, and it's the difference between picking the right plan and explaining a surprise invoice to whoever approves the budget. 👉 [See all ScraperAPI plans and start the free trial](https://www.scraperapi.com/pricing?fp_ref=coupons)
