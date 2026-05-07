# @appfigures/cli

[![npm](https://img.shields.io/npm/v/@appfigures/cli.svg)](https://www.npmjs.com/package/@appfigures/cli)

The Appfigures CLI — query app metrics, reviews, and store data from your terminal.

Try it now without installing:

```sh
npx @appfigures/cli auth login
npx @appfigures/cli apps search "youtube"
```

## Install

```sh
npm install -g @appfigures/cli
```

Requires Node.js 22+. Works with pnpm and yarn too.

## Quick start

```sh
appfigures auth login
appfigures --help
```

Also available as `af` alias.

## Authentication

Run `af auth login` in an interactive terminal to sign in. Two ways:

- **Browser** (default): opens your browser, approve, paste the code shown back into the CLI. The resulting token is stored in your OS credential manager (macOS Keychain, Windows Credential Manager, Linux Secret Service).
- **Personal access token**: pick "Personal access token" at the prompt for instructions. Create a token at [appfigures.com/developers/keys](https://appfigures.com/developers/keys), then `export APPFIGURES_API_KEY=<your-token>`.

For CI or any non-interactive context, `af auth login` will not run — see [Environment](#environment) below.

## Built for agents

Designed to be driven by LLM agents as much as by humans.

- **JSON on stdout, everything else on stderr.** Every command emits a single JSON value on stdout — `jq`-ready, no interleaved logs, hints, or update notices.
- **Auto-detected agent mode.** When stdout isn't a TTY (piped, redirected, or captured by an agent harness) the CLI switches to non-interactive: no prompts, no spinners, and errors include an agent-directed `NOTE:` hint on stderr to steer the next action.
- **Self-describing, offline.** `af docs get catalog_playbook` and `af docs get numeric_metrics` print the full query grammar and dataset catalog without hitting the API — plan queries without round-trips or stale training data.
- **Unattended auth.** Set `APPFIGURES_API_KEY` in the environment; every command runs without a browser or prompt.
- **Raw API escape hatch.** `af api <path>` proxies any endpoint the first-class commands don't cover.

<!-- prettier-ignore-start -->
<!-- BEGIN auto-generated COMMANDS — do not edit; run `pnpm generate-docs` to update -->
## Commands

### Apps

Find and identify apps.

| Command | Description |
| ------- | ----------- |
| <a href="#command-apps-search"><code>af&nbsp;apps&nbsp;search</code></a> | Find apps by name or publisher. Returns one row per unified app. Default returns Apple and Google listings — pass `--all-stores` to include other storefronts. To filter apps by estimate values (e.g. apps with >100k downloads last month) use [`explorer query`](#command-explorer-query). For estimates broken down by time, country, or storefront, use [`metrics query`](#command-metrics-query) with datasets estimates.sales or estimates.revenue. |
| <a href="#command-apps-tracked"><code>af&nbsp;apps&nbsp;tracked</code></a> | List the apps your Appfigures account tracks. |
| <a href="#command-apps-get"><code>af&nbsp;apps&nbsp;get</code></a> | Get an app's record — basic metadata (name, developer, etc) and, if the user tracks it, what data they can access. Pass a product ID for one storefront; unified app ID for all storefronts together. |

### Explorer

Search and analyze the app catalog — 3M+ apps, 120+ queryable fields.

| Command | Description |
| ------- | ----------- |
| <a href="#command-explorer-query"><code>af&nbsp;explorer&nbsp;query</code></a> | Advanced search across the 3M-app catalog — filter and sort by any of 120+ fields (download/revenue estimates, SDK presence, demographics, ratings, ranks, pricing, release dates, ad activity). Uses a simple array query grammar. The full field list and query syntax are documented in [`docs get catalog_playbook`](#command-docs-get). |
| <a href="#command-explorer-aggregate"><code>af&nbsp;explorer&nbsp;aggregate</code></a> | Advanced aggregation across the 3M-app catalog — counts, averages, min/max, and histograms over any set of matching apps. Uses the same bespoke JSON query grammar as [`explorer query`](#command-explorer-query); returns aggregates, not app records. For market sizing, benchmarking, and segment analysis. |
| <a href="#command-explorer-fields"><code>af&nbsp;explorer&nbsp;fields</code></a> | List the catalog fields and the current user's access level for each. Same field set [`explorer query`](#command-explorer-query) and [`explorer aggregate`](#command-explorer-aggregate) accept. |

### Metrics

Query numeric datasets across dimensions.

| Command | Description |
| ------- | ----------- |
| <a href="#command-metrics-query"><code>af&nbsp;metrics&nbsp;query</code></a> | Query any numeric dataset for one or more apps. Optionally grouped by up to two dimensions, returned as a nested partition tree, not app records. Independently filterable by country, device type, and date range. `filterAppsBy*` options narrow the app set (by ID, storefront, source, or type). Some datasets require app ownership, some work for any app. |

### Store

App store presence — listing content, category ranks, top charts, and featured placements.

| Command | Description |
| ------- | ----------- |
| <a href="#command-store-app-ranks"><code>af&nbsp;store&nbsp;app&#8209;ranks</code></a> | Rank history for one or more apps across countries, device types, subtypes, and categories. Returns time-series positions and day-over-day deltas. |
| <a href="#command-store-top-charts"><code>af&nbsp;store&nbsp;top&#8209;charts</code></a> | Top apps in a category chart — for a given country and category. Returns ranked entries with current positions and day-over-day deltas. |
| <a href="#command-store-categories"><code>af&nbsp;store&nbsp;categories</code></a> | Lists every store category with its ID. Numeric category IDs required by [`store app-ranks --category-ids`](#command-store-app-ranks) and [`store top-charts --category-id`](#command-store-top-charts) are available here. |
| <a href="#command-store-featured"><code>af&nbsp;store&nbsp;featured</code></a> | Featured and editorial placement history for an app. |
| <a href="#command-store-app-listing"><code>af&nbsp;store&nbsp;app&#8209;listing</code></a> | Full store listing for one storefront — localized text (name, subtitle, description, release notes) plus screenshots, video, categories, monetization, supported devices, country availability, price, file size, and age rating. Takes a numeric product ID (one storefront at a time; a unified app has one product per storefront). One locale per request. |

### Reviews

Search, summarize, and reply to iOS and Google Play app store reviews.

| Command | Description |
| ------- | ----------- |
| <a href="#command-reviews-list"><code>af&nbsp;reviews&nbsp;list</code></a> | Search and filter reviews for one or more apps by star rating, date range, country, language, and app version. |
| <a href="#command-reviews-breakdown"><code>af&nbsp;reviews&nbsp;breakdown</code></a> | Review volume breakdown for one or more apps, by star rating, date, or country. |
| <a href="#command-reviews-reply"><code>af&nbsp;reviews&nbsp;reply</code></a> | Reply to a specific review. |

### Keywords

Organic keyword visibility and tracking.

| Command | Description |
| ------- | ----------- |
| <a href="#command-keywords-list"><code>af&nbsp;keywords&nbsp;list</code></a> | List tracked keywords with their opaque IDs. |
| <a href="#command-keywords-rankings"><code>af&nbsp;keywords&nbsp;rankings</code></a> | Current organic keyword rankings for one or more apps. Returns which keywords each app ranks for, with position, popularity, and competitiveness. Works for any app. |
| <a href="#command-keywords-results"><code>af&nbsp;keywords&nbsp;results</code></a> | Apps ranking for a specific keyword in organic search results. Returns the ranked list of apps appearing for that keyword, with keyword popularity and competitiveness scores. |
| <a href="#command-keywords-related"><code>af&nbsp;keywords&nbsp;related</code></a> | Find related and suggested keywords for ASO research. |

### Apple Ads

Apple Search Ads intelligence.

| Command | Description |
| ------- | ----------- |
| <a href="#command-apple-ads-keywords"><code>af&nbsp;apple&#8209;ads&nbsp;keywords</code></a> | Paid keyword data for one or more apps — which keywords each app has ad impressions for, with impression share and organic rank. |
| <a href="#command-apple-ads-advertisers"><code>af&nbsp;apple&#8209;ads&nbsp;advertisers</code></a> | Apps advertising on a specific keyword, with impression share, organic rank, first/last seen dates, and lifetime data. |

### Docs

Reference docs for composing requests and interpreting responses.

| Command | Description |
| ------- | ----------- |
| <a href="#command-docs-get"><code>af&nbsp;docs&nbsp;get</code></a> | Returns a reference doc by slug. |

### API

| Command | Description |
| ------- | ----------- |
| <a href="#command-api"><code>af&nbsp;api</code></a> | Make a raw API request. See the API reference at https://docs.appfigures.com. |

### Auth

| Command | Description |
| ------- | ----------- |
| <a href="#command-auth-login"><code>af&nbsp;auth&nbsp;login</code></a> | Sign in to Appfigures |
| <a href="#command-auth-logout"><code>af&nbsp;auth&nbsp;logout</code></a> | Remove stored credentials |
| <a href="#command-auth-status"><code>af&nbsp;auth&nbsp;status</code></a> | Show authentication and account status |

Run `af <command> --help` for arguments, flags, and examples.
<!-- END auto-generated COMMANDS -->
<!-- prettier-ignore-end -->

## Environment

| Variable             | Purpose                                     |
| -------------------- | ------------------------------------------- |
| `APPFIGURES_API_KEY` | API key; skips interactive auth             |
| `AF_VERBOSE`         | Log HTTP to stderr (same as `-v`)           |
| `NO_COLOR`           | Disable ANSI color                          |
| `NO_UPDATE_NOTIFIER` | Skip the npm-registry update check          |
| `CI`                 | Also skips the update check (any CI system) |

## API Reference

Every command with its full argument and flag list. For the one-line overview, see [Commands](#commands) above.

**Global flags.** All commands accept:

- `-v, --verbose` — Log HTTP requests to stderr. Also set via `AF_VERBOSE=1`.
- `-V, --version` — Print the CLI version and exit.
- `-h, --help` — Show usage for the current command.

**Output format.** Every command emits a single JSON value on stdout — pipe to `jq` for filtering. Informational messages, hints, and update notices go to stderr so pipelines stay clean.

<!-- prettier-ignore-start -->
<!-- BEGIN auto-generated API REFERENCE ENTRIES — do not edit; run `pnpm generate-docs` to update -->
<a id="apps-search"></a>
<a id="command-apps-search"></a>
### af apps search

`af apps search <query> [flags]`

Find apps by name or publisher. Returns one row per unified app. Default returns Apple and Google listings — pass `--all-stores` to include other storefronts. To filter apps by estimate values (e.g. apps with >100k downloads last month) use [`explorer query`](#command-explorer-query). For estimates broken down by time, country, or storefront, use [`metrics query`](#command-metrics-query) with datasets estimates.sales or estimates.revenue.

**Options**

- `<query>` required string. Search query — app name or publisher
- `--all-stores` boolean, default `false`. Include storefronts beyond Apple and Google — Amazon, Windows, Steam, Roku, LG TV, Samsung TV, and others.
- `--count` integer, default `10`. Number of results to return
- `--page` integer, default `1`. Page number (1-indexed)

---

<a id="apps-tracked"></a>
<a id="command-apps-tracked"></a>
### af apps tracked

`af apps tracked [flags]`

List the apps your Appfigures account tracks.

**Options**

- `--count` integer, default `10`. Number of results to return
- `--page` integer, default `1`. Page number (1-indexed)
- `--q` string. Substring match on app name.
- `--filter-apps-by-id` (integer or string)[]. Only include data about specific apps, by product ID or unified app ID. Takes precedence over the other `filterAppsBy*` keys when set. Storefront, source, or type filters are better for app sets that can be described by those criteria.
- `--filter-apps-by-storefront` string[]. Narrow the account's tracked apps to those on these storefronts (e.g. apple:ios, google_play).
- `--filter-apps-by-source` string[]. Narrow the account's tracked apps by tracking relationship.
- `--filter-apps-by-type` string[]. Narrow the account's tracked apps to products of these types.

---

<a id="apps-get"></a>
<a id="command-apps-get"></a>
### af apps get

`af apps get <app-id> [flags]`

Get an app's record — basic metadata (name, developer, etc) and, if the user tracks it, what data they can access. Pass a product ID for one storefront; unified app ID for all storefronts together.

**Options**

- `<app-id>` required integer or string. App identifier — unified app ID or product ID
- `--all-stores` boolean, default `false`. For a unified app ID: include member products across all storefronts (Amazon, Steam, Windows, Roku, etc.). When false, member_products is restricted to app-intelligence storefronts (iOS + Google Play). Ignored for product IDs.

---

<a id="explorer-query"></a>
<a id="command-explorer-query"></a>
### af explorer query

`af explorer query [query] [flags]`

Advanced search across the 3M-app catalog — filter and sort by any of 120+ fields (download/revenue estimates, SDK presence, demographics, ratings, ranks, pricing, release dates, ad activity). Uses a simple array query grammar. The full field list and query syntax are documented in [`docs get catalog_playbook`](#command-docs-get).

**Options**

- `[query]` array. Explorer query in JSON array format to select matching catalog Products. Missing values and `[]` match every Product across every storefront. The full field list and query syntax are documented in [`docs get catalog_playbook`](#command-docs-get).
- `--fields` string[]. Explorer field names. The full field list is documented in [`docs get catalog_playbook`](#command-docs-get).
- `--sort` string. Explorer field name. The full field list is documented in [`docs get catalog_playbook`](#command-docs-get).
- `--order` string. Sort direction
- `--count` integer, default `10`. Number of results to return
- `--page` integer, default `1`. Page number (1-indexed)
- `--allow-unscoped-nested` boolean, default `false`. Escape hatch: only pass true after a query error says the unscoped nested semantics are intentional.

---

<a id="explorer-aggregate"></a>
<a id="command-explorer-aggregate"></a>
### af explorer aggregate

`af explorer aggregate <fields> [flags]`

Advanced aggregation across the 3M-app catalog — counts, averages, min/max, and histograms over any set of matching apps. Uses the same bespoke JSON query grammar as [`explorer query`](#command-explorer-query); returns aggregates, not app records. For market sizing, benchmarking, and segment analysis.

**Options**

- `--query` array. Explorer query in JSON array format to select matching catalog Products. Missing values and `[]` match every Product across every storefront. The full field list and query syntax are documented in [`docs get catalog_playbook`](#command-docs-get).
- `<fields>` required string[]. Field+aggregation pairs (e.g. `all_rating/stats`, `storefronts/terms`). Aggregations: `stats`, `terms`, `histogram`, `date_histogram`, `cardinality`. The full field list is documented in [`docs get catalog_playbook`](#command-docs-get).
- `--allow-unscoped-nested` boolean, default `false`. Escape hatch: only pass true after a query error says the unscoped nested semantics are intentional.

---

<a id="explorer-fields"></a>
<a id="command-explorer-fields"></a>
### af explorer fields

`af explorer fields`

List the catalog fields and the current user's access level for each. Same field set [`explorer query`](#command-explorer-query) and [`explorer aggregate`](#command-explorer-aggregate) accept.

---

<a id="metrics-query"></a>
<a id="command-metrics-query"></a>
### af metrics query

`af metrics query <dataset> [flags]`

Query any numeric dataset for one or more apps. Optionally grouped by up to two dimensions, returned as a nested partition tree, not app records. Independently filterable by country, device type, and date range. `filterAppsBy*` options narrow the app set (by ID, storefront, source, or type). Some datasets require app ownership, some work for any app.

**Options**

- `<dataset>` required string. Dataset to query (e.g. sales.combined_downloads). The full list is documented in [`docs get numeric_metrics`](#command-docs-get).
- `--group-by` string[]. Dimensions to group by. Max 2: the first slot becomes the outer entity type, the second the inner series.
- `--granularity` string. Time granularity when grouping by date
- `--include-total` boolean, default `false`. Return a single total alongside the breakdowns. Skips client-side summation for dashboard-style asks.
- `--count` integer. Cap the number of rows returned. Without this option, paginated datasets auto-paginate every page; a small count returns a preview instead of the full series.
- `--countries` string[]. Filter to one or more ISO country codes (e.g. US, JP, GB)
- `--device-type` string. Device type
- `--all-time` boolean, default `false`. Opt in to the entire history. Without this flag (and without `start`/`end`), the query defaults to the last 30 days. Mutually exclusive with `start` and `end`.
- `--filter-apps-by-id` (integer or string)[]. Only include data about specific apps, by product ID or unified app ID. Takes precedence over the other `filterAppsBy*` keys when set. Storefront, source, or type filters are better for app sets that can be described by those criteria.
- `--filter-apps-by-storefront` string[]. Narrow the account's tracked apps to those on these storefronts (e.g. apple:ios, google_play).
- `--filter-apps-by-source` string[]. Narrow the account's tracked apps by tracking relationship.
- `--filter-apps-by-type` string[]. Narrow the account's tracked apps to products of these types.
- `--start` string. Start date (YYYY-MM-DD)
- `--end` string. End date (YYYY-MM-DD, defaults to today)

---

<a id="store-app-ranks"></a>
<a id="command-store-app-ranks"></a>
### af store app-ranks

`af store app-ranks <app-ids> [flags]`

Rank history for one or more apps across countries, device types, subtypes, and categories. Returns time-series positions and day-over-day deltas.

**Options**

- `<app-ids>` required (integer or string)[]. App identifiers (unified app IDs or product IDs)
- `--countries` string[]. Country codes to query. Defaults to every country with rank coverage.
- `--granularity` string, default `hourly`. Sampling rate. Hourly gives the freshest data; pass `--granularity=daily` for compact multi-day history.
- `--device-types` string[], default `["handheld"]`. Filter response rows by device type.
- `--subtypes` string[], default `["free"]`. Filter response rows by subtype.
- `--category-ids` integer[]. Filter response rows by category ID.
- `--start` string. Start date (YYYY-MM-DD)
- `--end` string. End date (YYYY-MM-DD, defaults to today)
- `--count` integer, default `10`. Number of results to return
- `--page` integer, default `1`. Page number (1-indexed)

---

<a id="store-top-charts"></a>
<a id="command-store-top-charts"></a>
### af store top-charts

`af store top-charts [flags]`

Top apps in a category chart — for a given country and category. Returns ranked entries with current positions and day-over-day deltas.

**Options**

- `--country` required string. ISO country code (e.g. US, JP, GB)
- `--category-id` required integer. Store category ID
- `--subtype` string, default `free`. Which list within the category
- `--date` string. Snapshot date (YYYY-MM-DD, defaults to current).
- `--count` integer, default `10`. Number of results to return
- `--page` integer, default `1`. Page number (1-indexed)

---

<a id="store-categories"></a>
<a id="command-store-categories"></a>
### af store categories

`af store categories [flags]`

Lists every store category with its ID. Numeric category IDs required by [`store app-ranks --category-ids`](#command-store-app-ranks) and [`store top-charts --category-id`](#command-store-top-charts) are available here.

**Options**

- `--all` boolean, default `false`. Include non-rank stores (roku, vizio, etc.) — they have categories but no rank data
- `--ids` integer[]. Only return these category IDs.
- `--q` string. Substring match on category name.

---

<a id="store-featured"></a>
<a id="command-store-featured"></a>
### af store featured

`af store featured <app-id> [flags]`

Featured and editorial placement history for an app.

**Options**

- `<app-id>` required integer or string. App identifier — unified app ID or product ID
- `--start` string. Start date (YYYY-MM-DD)
- `--end` string. End date (YYYY-MM-DD, defaults to today)
- `--count` integer, default `10`. Number of results to return
- `--page` integer, default `1`. Page number (1-indexed)

---

<a id="store-app-listing"></a>
<a id="command-store-app-listing"></a>
### af store app-listing

`af store app-listing <product-id> [flags]`

Full store listing for one storefront — localized text (name, subtitle, description, release notes) plus screenshots, video, categories, monetization, supported devices, country availability, price, file size, and age rating. Takes a numeric product ID (one storefront at a time; a unified app has one product per storefront). One locale per request.

**Options**

- `<product-id>` required integer. Numeric product ID for one storefront. Not a unified app ID. Member product_id values are available from [`apps get '<unified-app-id>'`](#command-apps-get).
- `--language` string. Locale (e.g. en, ja, zh-Hans) for name, subtitle, description, release notes, and screenshots. Defaults to en; falls back to the first available locale when the requested one has no metadata. The response echoes the resolved language.
- `--device-type` string, default `handheld`. Relevant to Apple apps. Pick handheld for iPhone-specific metadata, tablet for iPad, desktop for Mac, etc.

---

<a id="reviews-list"></a>
<a id="command-reviews-list"></a>
### af reviews list

`af reviews list [flags]`

Search and filter reviews for one or more apps by star rating, date range, country, language, and app version.

**Options**

- `--stars` number[]. Filter by star rating
- `--language` string. Filter by language code (e.g. en, ja)
- `--version` string. Filter by app version
- `--country` string. ISO country code (e.g. US, JP, GB)
- `--filter-apps-by-id` (integer or string)[]. Only include data about specific apps, by product ID or unified app ID. Takes precedence over the other `filterAppsBy*` keys when set. Storefront, source, or type filters are better for app sets that can be described by those criteria.
- `--filter-apps-by-storefront` string[]. Narrow the account's tracked apps to those on these storefronts (e.g. apple:ios, google_play).
- `--filter-apps-by-source` string[]. Narrow the account's tracked apps by tracking relationship.
- `--filter-apps-by-type` string[]. Narrow the account's tracked apps to products of these types.
- `--count` integer, default `10`. Number of results to return
- `--page` integer, default `1`. Page number (1-indexed)

---

<a id="reviews-breakdown"></a>
<a id="command-reviews-breakdown"></a>
### af reviews breakdown

`af reviews breakdown [flags]`

Review volume breakdown for one or more apps, by star rating, date, or country.

**Options**

- `--country` string. ISO country code (e.g. US, JP, GB)
- `--filter-apps-by-id` (integer or string)[]. Only include data about specific apps, by product ID or unified app ID. Takes precedence over the other `filterAppsBy*` keys when set. Storefront, source, or type filters are better for app sets that can be described by those criteria.
- `--filter-apps-by-storefront` string[]. Narrow the account's tracked apps to those on these storefronts (e.g. apple:ios, google_play).
- `--filter-apps-by-source` string[]. Narrow the account's tracked apps by tracking relationship.
- `--filter-apps-by-type` string[]. Narrow the account's tracked apps to products of these types.
- `--start` string. Start date (YYYY-MM-DD)
- `--end` string. End date (YYYY-MM-DD, defaults to today)

---

<a id="reviews-reply"></a>
<a id="command-reviews-reply"></a>
### af reviews reply

`af reviews reply <review-id> [flags]`

Reply to a specific review.

**Options**

- `<review-id>` required string. Review ID to reply to
- `--body` required string. Reply text

---

<a id="keywords-list"></a>
<a id="command-keywords-list"></a>
### af keywords list

`af keywords list`

List tracked keywords with their opaque IDs.

---

<a id="keywords-rankings"></a>
<a id="command-keywords-rankings"></a>
### af keywords rankings

`af keywords rankings [flags]`

Current organic keyword rankings for one or more apps. Returns which keywords each app ranks for, with position, popularity, and competitiveness. Works for any app.

**Options**

- `--product-ids` integer[]. Product identifiers (numeric, single-store products).
- `--country` required string. ISO country code (e.g. US, JP, GB)
- `--device-type` string. Device type
- `--count` integer, default `10`. Number of results to return
- `--page` integer, default `1`. Page number (1-indexed)

---

<a id="keywords-results"></a>
<a id="command-keywords-results"></a>
### af keywords results

`af keywords results <keyword-name> [flags]`

Apps ranking for a specific keyword in organic search results. Returns the ranked list of apps appearing for that keyword, with keyword popularity and competitiveness scores.

**Options**

- `<keyword-name>` required string. Keyword to look up
- `--country` string. ISO country code (e.g. US, JP, GB)
- `--storefront` string. App store platform — e.g. apple:ios, google_play, amazon_appstore, steam, windows10, apple:mac, apple:tv, apple:imessage (and others the API may add)
- `--device-type` string. Device type
- `--count` integer, default `10`. Number of results to return
- `--page` integer, default `1`. Page number (1-indexed)

---

<a id="keywords-related"></a>
<a id="command-keywords-related"></a>
### af keywords related

`af keywords related <keyword-name> [flags]`

Find related and suggested keywords for ASO research.

**Options**

- `<keyword-name>` required string. Seed keyword to find related terms for
- `--country` string. ISO country code (e.g. US, JP, GB)
- `--storefront` string. App store platform — e.g. apple:ios, google_play, amazon_appstore, steam, windows10, apple:mac, apple:tv, apple:imessage (and others the API may add)
- `--device-type` string. Device type

---

<a id="apple-ads-keywords"></a>
<a id="command-apple-ads-keywords"></a>
### af apple-ads keywords

`af apple-ads keywords <product-ids> [flags]`

Paid keyword data for one or more apps — which keywords each app has ad impressions for, with impression share and organic rank.

**Options**

- `<product-ids>` required integer[]. Product identifiers (numeric, single-store products).
- `--days` integer, default `180`. Lookback period in days. Common values: 7, 14, 30, 90, 180, 365.
- `--country` string. ISO country code (e.g. US, JP, GB)
- `--device-type` string. Device type
- `--count` integer, default `10`. Number of results to return
- `--page` integer, default `1`. Page number (1-indexed)

---

<a id="apple-ads-advertisers"></a>
<a id="command-apple-ads-advertisers"></a>
### af apple-ads advertisers

`af apple-ads advertisers <keyword-name> [flags]`

Apps advertising on a specific keyword, with impression share, organic rank, first/last seen dates, and lifetime data.

**Options**

- `<keyword-name>` required string. Keyword to look up advertisers for
- `--days` integer, default `180`. Lookback period in days. Common values: 7, 14, 30, 90, 180, 365.
- `--country` string. ISO country code (e.g. US, JP, GB)
- `--device-type` string. Device type
- `--count` integer, default `10`. Number of results to return
- `--page` integer, default `1`. Page number (1-indexed)

---

<a id="docs-get"></a>
<a id="command-docs-get"></a>
### af docs get

`af docs get <slug>`

Returns a reference doc by slug.

**Options**

- `<slug>` required string. Which reference to return

---

<a id="command-api"></a>
### af api

`af api <path> [flags]`

Make a raw API request. See the API reference at https://docs.appfigures.com.

**Options**

- `<path>` required string. API path (e.g. /users, /products)
- `--method` string, default `GET`. HTTP method (one of: GET, POST, PUT, PATCH, DELETE, HEAD, OPTIONS)
- `--body` string. Request body (JSON string)

---

<a id="auth-login"></a>
<a id="command-auth-login"></a>
### af auth login

`af auth login [flags]`

Sign in to Appfigures

Prints a URL to authorize this CLI. Open it, approve access, then re-run
with `--code <code>` using the code shown after approval.

For unattended use (CI, scripts), set APPFIGURES_API_KEY in the
environment instead — no browser flow needed.

**Options**

- `--code` string. Authorization code from the OAuth consent page. Second step of `af auth login` — exchanges the code, saves the token, exits.

---

<a id="auth-logout"></a>
<a id="command-auth-logout"></a>
### af auth logout

`af auth logout`

Remove stored credentials

---

<a id="auth-status"></a>
<a id="command-auth-status"></a>
### af auth status

`af auth status`

Show authentication and account status
<!-- END auto-generated API REFERENCE ENTRIES -->
<!-- prettier-ignore-end -->

## Support

- Issues: [github.com/appfigures/cli/issues](https://github.com/appfigures/cli/issues)
- API docs: [docs.appfigures.com](https://docs.appfigures.com)

## Contributing

This package is developed in a private monorepo and published here as a mirror. To report bugs or request features, [open an issue](https://github.com/appfigures/cli/issues).

We're also hiring AI-native devs. If you want to help build the future of AI App Intelligence apply at [appfigures.com/careers](https://appfigures.com/careers).

## License

Apache 2.0
