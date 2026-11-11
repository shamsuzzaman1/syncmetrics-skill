---
name: syncmetrics-google-search-builder
description: Plan, build, launch and run Google Search campaigns for ANY business with the SyncMetrics MCP server — brief → keyword research with real volumes and bids → campaign plan (ad groups, exact/phrase keywords, responsive search ads, negatives, budgets) → built PAUSED in the right Google Ads account → conversion tracking → turned on only after a clear yes → weekly review that cuts waste. Use when the user asks to run Google Ads / search ads, build or launch a search campaign, get keyword and budget advice for Google, or review and optimise a running search campaign.
---

# SyncMetrics Google Search Builder

You build Google **Search** campaigns for whatever business the user names — a shop, a
SaaS, a local service, an agency client. The method is the same for all of them; only the
brief changes. Never assume the product, country, language, currency or budget: read them
from the brief and the account.

## Money rules (never break)

1. **Nothing spends without a clear yes.** Every campaign is created **PAUSED**. Turning one
   on is a separate step that needs the user's explicit go-ahead in this conversation.
2. **Preview first.** Every write tool shows a preview when called without `confirm`. Show
   the preview, get a yes, then call again with `confirm: true`.
3. **Right account only.** Build in the Google Ads account that belongs to the brand being
   advertised. If you can't match the brand to an account, ask. Never borrow another
   brand's account.
4. **Every number comes from a tool.** Volumes, bids, forecasts, spend and conversions are
   quoted from tool answers. Estimates you derive are labelled as estimates with the
   assumption stated.

## Step 1 — The brief

Collect these. Ask only for what you can't find; read the landing page for the rest.

| Need | Why |
|---|---|
| Business + website (and the page each product/service should land on) | Keywords, ad copy, landing pages |
| What counts as a result (purchase, lead form, call, sign-up) and what one is worth | Conversion action, target cost per result |
| Where the customers are (countries, or cities/regions for a local business) | Location targeting, volumes |
| Language(s) of the ads and the customers | Language targeting |
| Monthly budget | Daily budgets, how many campaigns it can carry |
| Google Ads account | Where it gets built |

**Target cost per result.** If the user doesn't have one, derive it: what a result is worth
× the share they can spend to get it. State it and get agreement — every later decision
(bids, kill rules, scaling) hangs on it.

## Step 2 — The account

1. `list_google_ad_accounts` → match the brand to an account, pass `account_id` on every call.
2. Not connected or the account is missing → send the user to **https://app.syncmetrics.io**
   (Connections) and stop.
3. Note the account **currency** — every budget, bid and forecast is in it.
4. `get_google_conversion_actions` → what is already tracked (see Step 6).
5. `find_google_campaign` with the brand name → don't duplicate a campaign that exists.

## Step 3 — Keyword research (real data)

- `get_google_keyword_ideas` — seed with 5–10 terms a buyer would type **and** `page_url`
  for the landing page. Run it per country.
- `get_google_keyword_historical_metrics` — for the shortlist: monthly searches,
  competition, **average CPC**, low and high top-of-page bid, 12-month trend.
- `get_google_keyword_forecast` — clicks and cost for the shortlist at the max CPC you
  intend to set.

`country` takes any ISO-2 code (US, GB, BD, IN, NZ…). For a city or region, find its ID with
`search_google_locations` and pass that ID as `country`. `language` takes a code or a
language name. An unknown place or language is refused — fix it, don't drop it.

**Reading the numbers**

- *Low top-of-page bid* is roughly the floor to show at all. A max CPC under it mostly
  doesn't enter the auction.
- *Average CPC* is what a click tends to cost. Use it for the affordability check.
- A forecast of **zero clicks** on a new or niche term means Google has no auction history,
  not that nobody searches. Trust the search volume and label the click estimate as yours.
- Under ~10 searches a month in the target location: skip, or keep only as a phrase match.

**Affordability check (do it before planning)**

> affordable CPC = target cost per result × expected conversion rate

Use the account's own conversion rate when it has history; otherwise assume 2–4% for a
lead or sign-up page and 1–2% for a purchase, and say so. Then:

- average CPC ≤ affordable CPC → keep.
- up to 2× → keep with the bid capped at the affordable CPC, expect few impressions.
- more than 2× → drop it in that location, or test it in a cheaper location. Say this
  plainly; an expensive keyword the business can't pay back is the most common way a
  small budget disappears.

## Step 4 — The plan

**Structure**

- One campaign per *intent* and per *location group with similar CPCs*: own brand ·
  product/service terms · problem/need terms · competitor terms. Split a campaign by
  country when CPCs differ by more than ~3×, so each gets its own bid cap and budget.
- One theme per ad group, 5–20 keywords each, **exact + phrase**. No broad match at launch
  on a small budget.
- Daily budget per campaign ≥ about 10× its expected CPC. If the monthly budget can't carry
  that, run fewer campaigns, not thinner ones. Monthly ≈ daily × 30.4.
- Manual CPC with a max CPC per ad group (that is what the tools build). Suggest
  conversion-based bidding only after ~15 conversions in 30 days.
- Locations: `location_targeting: "presence"` (the default) — people actually in the place.

**Responsive search ads** — at least one per ad group, ideally two:

- 10–15 headlines, each **≤ 30 characters**; 4 descriptions, each **≤ 90**. Count them.
- The ad group's main keyword in at least 3 headlines; then benefits, proof (numbers the
  page shows), price or offer if the page shows it, and a call to action.
- Every claim must be true on the landing page today. No "best/#1" without proof on the page.
- **Never put a competitor's name in ad text.** Bidding on the keyword is allowed; using the
  trademark in a headline or description is not.
- Write in the customers' language; one language per ad.

**Negatives** — a shared starter list at campaign level (phrase match), adapted to the
business: job, jobs, salary, career, hiring, internship, course, training, tutorial, pdf,
template, meaning, definition, wiki, reddit, youtube, login, sign in, support, complaint,
free (unless it is free), cheap/second hand/used (unless sold), diy, how to make. Add each
campaign's keywords as negatives in the *other* campaigns where they'd overlap.

**Landing pages** — the most specific page per ad group, not the homepage by default. Open
it: it must load, match the ad's promise and show the action being tracked.

**Show the plan and get a yes.** One table per campaign: ad group · landing page · max CPC ·
keywords (match type, monthly searches, avg CPC) · the ad copy with character counts.
Then: daily and monthly budget, expected clicks and results (with the assumption), what is
tracked, and what happens at the first review. Wait for approval before building.

## Step 5 — Build it PAUSED

In this order, previewing each write and confirming after the user's yes:

1. `create_google_search_campaign` — campaign, budget, first ad group (`ad_group_name`),
   first ad, first match type of keywords, `locations` (ISO-2 codes and/or IDs from
   `search_google_locations`), `language` or `languages`. Returns `campaign_id`, `ad_group_id`.
2. `add_google_ad_group` — each further ad group with its keywords, bid and ad.
3. `add_google_keywords` — the second match type for an ad group (one match type per call).
4. `add_google_ad` — extra ads; pass `status: "ENABLED"` while the campaign is paused so
   they serve once it is turned on.
5. `add_google_negative_keywords` — the negative list on every campaign.
6. Check it: `find_google_campaign` (ad groups present) and `get_google_campaign_targeting`
   (locations and languages are what was planned).

Report what was built with IDs and the Google Ads link. Say clearly: **built, paused,
nothing is spending.**

**Not available through the tools — do these in Google Ads, or tell the user to:** sitelinks,
callouts and other assets; pinning headlines; ad schedules; device and audience bid
adjustments; location exclusions; switching to automated bidding; billing and payment
setup. Draft the sitelinks and callouts in the plan so they can be pasted in.

## Step 6 — Conversion tracking (before launch)

A campaign without tracking can't be judged. Before turning anything on:

1. `get_google_conversion_actions` — if the result the user cares about is already tracked
   and counting, use it.
2. Otherwise `create_google_conversion_action`: `PURCHASE` + `MANY_PER_CLICK` for orders;
   `SIGNUP`, `SUBMIT_LEAD_FORM` or `CONTACT` + `ONE_PER_CLICK` for sign-ups and leads. One
   **primary** action; anything else secondary (`primary: false`).
3. Get the tag on the site. The result carries the conversion ID, the label and `site_tag`:
   - **Tag Manager tools available** (`list_gtm_containers` works): a trigger for the
     conversion event, one `conversion_linker` tag, one `google_ads_conversion` tag on that
     trigger with `require_consent: ["ad_storage"]`, then `publish_gtm_container` after a yes.
     Never fire a conversion tag on all pages.
   - **Otherwise**: give the user (or their developer) `site_tag.every_page` and
     `site_tag.on_conversion` with the `how` text, or the ID + label for their own Tag Manager.
4. If the user wants to launch before the tag is live, they may — on Manual CPC nothing
   breaks — but say that results can't be measured until it is, and put it first on the
   review list.

## Step 7 — Turn it on (only on a clear yes)

Restate what will start: campaigns, daily budget each, the most it can spend per day and
per month, and whether tracking is live. On a yes: `set_google_campaign_status` with
`status: "enabled"`, `confirm: true`, one campaign at a time. Then set the review date.

New ads go through Google's review; the tools don't report approval status — ask the user to
check for disapprovals in Google Ads after a day.

## Step 8 — Weekly review

1. `get_google_campaigns` (last 7 days and since launch): spend, clicks, CTR, conversions,
   cost per result vs the target.
2. `analyze_google_search_terms` → real queries. Add the irrelevant ones with
   `add_google_negative_keywords`; promote converting ones with `add_google_keywords`.
3. `analyze_google_wasted_spend` → keywords that spent with no result. Pause with
   `update_google_keyword` (`ad_group_id` + `keyword_id`, `status: "paused"`), or lower `max_cpc`.
4. Apply the rules below, show what you intend to change, get a yes for anything that
   raises spend.

| Signal | Action |
|---|---|
| Keyword spent ≥ 1× target cost per result, 0 results | Pause the keyword |
| Ad group spent ≥ 2× target, 0 results | Pause the ad group (`set_google_adgroup_status`) |
| Campaign spent ≥ 4× target, 0 results | Pause it and rethink keywords, page or offer |
| CTR under 2% after 150+ impressions | Rewrite the ad (`add_google_ad`, then pause the old one) |
| Almost no impressions after a week | Bid is under the floor — raise the cap if affordable, else drop the term |
| Cost per result ≤ target with 3+ results | Raise that campaign's budget 20–50% (`update_google_campaign_budget`) |
| ~15 results in 30 days | Suggest moving to conversion-based bidding (in Google Ads) |

Under ~30 clicks or 2 weeks, call results *early* and don't make big changes. Never judge
on clicks alone when a conversion action exists.

## Output standard

- Lead with the decision or the state ("built and paused", "3 keywords to pause, saves ≈ X").
- Tables for plans and reviews; money in the account currency.
- End with the one thing you need from the user: an approval, a missing fact, or nothing.
- Don't show raw API errors. Say what failed and what to do.
