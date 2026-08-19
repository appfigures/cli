# @appfigures/cli

[![npm version](https://img.shields.io/npm/v/@appfigures/cli)](https://www.npmjs.com/package/@appfigures/cli)
[![MCP server](https://img.shields.io/badge/MCP-server-blue)](https://modelcontextprotocol.io)

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

- **Log in yourself.** `af auth login --interactive` opens your browser in a guided flow; approve and paste the code back.
- **Or let your agent guide you.** By default, `af auth login` prints an authorization URL — open it, approve, and finish with `af auth login --code <code>` using the code shown. An agent can drive this end to end. For unattended use, set `APPFIGURES_API_KEY` instead: create a token at [appfigures.com/developers/keys](https://appfigures.com/developers/keys) by clicking **Create a New Client**, then **Create Personal Access Token**.

Either login saves the token to your OS credential manager (macOS Keychain, Windows Credential Manager, Linux Secret Service). See [Environment](#environment) for `APPFIGURES_API_KEY` and other overrides.

## MCP server

`af mcp` runs a local [Model Context Protocol](https://modelcontextprotocol.io) server over stdio, exposing the CLI's app-intelligence commands as MCP tools. Point any MCP client (Claude Code, Claude Desktop, Cursor, and others) at it to let an agent query app metrics, reviews, and store data directly.

Add it to your client's MCP config:

```json
{
	"mcpServers": {
		"appfigures": {
			"command": "npx",
			"args": ["-y", "@appfigures/cli", "mcp"]
		}
	}
}
```

The server signs in with your stored credentials, so run `af auth login` once first. For a headless setup, pass a token through the client's `env` instead:

```json
{
	"mcpServers": {
		"appfigures": {
			"command": "npx",
			"args": ["-y", "@appfigures/cli", "mcp"],
			"env": { "APPFIGURES_API_KEY": "<your-token>" }
		}
	}
}
```

Installed the CLI globally instead of running it through npx? Use `"command": "af"` with `"args": ["mcp"]`.

**Claude Code.** Add it with one command:

```sh
claude mcp add appfigures -- npx -y @appfigures/cli mcp
```

Run `af auth login` first to sign in, or append `--env APPFIGURES_API_KEY=<your-token>` for a headless setup.

<!-- prettier-ignore-start -->
<!-- BEGIN auto-generated COMMANDS -->
## Commands

### Apps

Find apps and look up their identity. Other commands take the app IDs these return.

| Command | Description |
| ------- | ----------- |
| <a href="#command-apps-search"><code>af&nbsp;apps&nbsp;search</code></a> | Find apps by name or publisher. Returns one row per unified app. Default returns Apple and Google listings; pass `--all-stores` to include other storefronts. To filter apps by estimate values (e.g. apps with >100k downloads last month) use [`explorer list-products`](#command-explorer-list-products). For estimates broken down by time, country, or storefront, use [`metrics query`](#command-metrics-query) with datasets estimates.sales or estimates.revenue. |
| <a href="#command-apps-tracked"><code>af&nbsp;apps&nbsp;tracked</code></a> | List the apps your Appfigures account tracks. |
| <a href="#command-apps-get"><code>af&nbsp;apps&nbsp;get</code></a> | Get an app's record: basic metadata (name, developer, etc) and, if the user tracks it, what data they can access. Pass a product ID for one storefront; unified app ID for all storefronts together. |

### Explorer

Search and analyze the full app catalog: millions of products across Apple, Google Play, Amazon, and other major stores, with 120+ fields spanning identity, storefront and country availability, categories, ratings, release dates, chart ranks, download and revenue estimates, SDK presence, demographics, and related apps.

| Command | Description |
| ------- | ----------- |
| <a href="#command-explorer-list-products"><code>af&nbsp;explorer&nbsp;list&#8209;products</code></a> | Read catalog fields for one app or many. Fields referenced by `query` or `sort` come back automatically; pass `--extra-fields` for more. Use `["match","product_id",<id>]` for a single app, or combine filters for population queries (e.g. iOS apps using Firebase with $1M+ US revenue). The 120+ fields span ranks, ratings, download and revenue estimates, SDKs, demographics, and more; query grammar and field list in [`docs get catalog_playbook`](#command-docs-get). |
| <a href="#command-explorer-aggregate-products"><code>af&nbsp;explorer&nbsp;aggregate&#8209;products</code></a> | Aggregate across the full catalog of millions of products across Apple, Google Play, Amazon, and other major stores: counts, averages, min/max, and histograms over any set of matching products. Uses the same query grammar as [`explorer list-products`](#command-explorer-list-products); returns aggregates, not product records. For market sizing, benchmarking, and segment analysis. |
| <a href="#command-explorer-describe-fields"><code>af&nbsp;explorer&nbsp;describe&#8209;fields</code></a> | List the catalog fields and the current user's access level for each. Search by keyword to find fields. Same field set [`explorer list-products`](#command-explorer-list-products) and [`explorer aggregate-products`](#command-explorer-aggregate-products) accept. |

### Metrics

Query numeric datasets across dimensions.

| Command | Description |
| ------- | ----------- |
| <a href="#command-metrics-query"><code>af&nbsp;metrics&nbsp;query</code></a> | Query any numeric dataset for one or more apps. Optionally grouped by up to two dimensions, returned as a nested partition tree, not app records. Independently filterable by country, device type, and date range. `filterAppsBy*` options narrow the app set (by ID, storefront, source, or type); without one, a query covers every app the account tracks. |
| <a href="#command-metrics-describe-datasets"><code>af&nbsp;metrics&nbsp;describe&#8209;datasets</code></a> | List every numeric dataset [`metrics query`](#command-metrics-query) accepts, one row per dataset with its value type and whether it's limited to your own apps. |

### Store

App store presence: listing content, category ranks, top charts, and featured placements.

| Command | Description |
| ------- | ----------- |
| <a href="#command-store-app-ranks"><code>af&nbsp;store&nbsp;app&#8209;ranks</code></a> | Trace rank history for one or more apps across countries, device types, category subtypes, and categories, as time-series positions with day-over-day deltas. |
| <a href="#command-store-top-charts"><code>af&nbsp;store&nbsp;top&#8209;charts</code></a> | List the top apps in a category chart for a given country and category, with current positions and day-over-day deltas. |
| <a href="#command-store-categories"><code>af&nbsp;store&nbsp;categories</code></a> | List every store category with its ID. Numeric category IDs required by [`store app-ranks --category-ids`](#command-store-app-ranks) and [`store top-charts --category-id`](#command-store-top-charts) are available here. |
| <a href="#command-store-featured"><code>af&nbsp;store&nbsp;featured</code></a> | List featured and editorial placements for an app or storefront product. Request 0 rows for summary stats only. |
| <a href="#command-store-app-listing"><code>af&nbsp;store&nbsp;app&#8209;listing</code></a> | Read the full store listing for one storefront: localized text (name, subtitle, description, release notes) plus screenshots, video, categories, monetization, supported devices, country availability, price, file size, and age rating. Takes a numeric product ID (one storefront at a time; a unified app has one product per storefront). One locale per request. |

### Audience

Who an app's users are and what else they use. Covers estimated age and gender, plus audience overlap with other apps.

| Command | Description |
| ------- | ----------- |
| <a href="#command-audience-demographics"><code>af&nbsp;audience&nbsp;demographics</code></a> | Read an app's audience demographics: the estimated age and gender breakdown. |
| <a href="#command-audience-cross-usage"><code>af&nbsp;audience&nbsp;cross&#8209;usage</code></a> | Find the apps that an app's users also use. |

### Reviews

Search, summarize, and reply to iOS and Google Play app store reviews.

| Command | Description |
| ------- | ----------- |
| <a href="#command-reviews-list"><code>af&nbsp;reviews&nbsp;list</code></a> | Read individual reviews for one or more apps. Returns review text, star rating, country, and app version. Filterable by star rating, date range, country, version, and tracking relationship. |
| <a href="#command-reviews-breakdown"><code>af&nbsp;reviews&nbsp;breakdown</code></a> | Aggregate review counts for one or more apps, bucketed by dimension. Returns one count per dimension value, plus a global total across the matched set. |
| <a href="#command-reviews-reply"><code>af&nbsp;reviews&nbsp;reply</code></a> | Post or withdraw a developer response on a specific review. Pass `content` to post; pass `delete: true` to withdraw a previously-posted response. Returns the resulting state (`published`/`pending` for a post, `removed`/`removal_pending` for a withdrawal) along with the submitting account. |

### Keywords

Keyword visibility, rank tracking, and discovery across organic search (App Store, Google Play) and Apple Ads.

| Command | Description |
| ------- | ----------- |
| <a href="#command-keywords-organic"><code>af&nbsp;keywords&nbsp;organic</code></a> | Check the organic keywords one or more apps rank for, with position, popularity, and competitiveness. |
| <a href="#command-keywords-paid"><code>af&nbsp;keywords&nbsp;paid</code></a> | List the paid keywords one or more apps run ads on, with impression share and organic rank. |
| <a href="#command-keywords-tracked-ranks"><code>af&nbsp;keywords&nbsp;tracked&#8209;ranks</code></a> | View where all your tracked keywords rank for a single app+country combo, with each keyword's current position, movement since it last changed, starting position, popularity, and competitiveness. |
| <a href="#command-keywords-tracked-trend"><code>af&nbsp;keywords&nbsp;tracked&#8209;trend</code></a> | Trace how one tracked keyword's rank changes over time for a single app+country combo. Each point gives the rank and how many positions it moved since the one before. |
| <a href="#command-keywords-suggestions"><code>af&nbsp;keywords&nbsp;suggestions</code></a> | Discover keyword ideas to consider targeting for a single app+country combo, ranked by relevance to the app and including some drawn from apps you compete with. Each comes with its popularity, competitiveness, and the app's current rank. |
| <a href="#command-keywords-ranking-apps"><code>af&nbsp;keywords&nbsp;ranking&#8209;apps</code></a> | List the apps ranking for a specific keyword in organic search, plus the keyword's own popularity and competitiveness scores. |
| <a href="#command-keywords-advertisers"><code>af&nbsp;keywords&nbsp;advertisers</code></a> | List the apps advertising on a specific keyword, with each advertiser's impression share, organic rank, and how long they've been bidding. |
| <a href="#command-keywords-related"><code>af&nbsp;keywords&nbsp;related</code></a> | Find keywords related to a seed term for ASO research. Useful for finding alternatives with a similar audience that are more popular or less competitive. |
| <a href="#command-keywords-tracked"><code>af&nbsp;keywords&nbsp;tracked</code></a> | List tracked keywords with their opaque IDs. |
| <a href="#command-keywords-track"><code>af&nbsp;keywords&nbsp;track</code></a> | Track a keyword to monitor your app's hourly rank for it over time and get automatic alerts when its position moves. |
| <a href="#command-keywords-untrack"><code>af&nbsp;keywords&nbsp;untrack</code></a> | Stop tracking a keyword. |

### Apple Ads

Manage your Apple Ads campaigns, ad groups, keywords, and performance.

| Command | Description |
| ------- | ----------- |
| <a href="#command-apple-ads-organizations"><code>af&nbsp;apple&#8209;ads&nbsp;organizations</code></a> | List the Apple Ads organizations you manage campaigns in, with each one's currency and timezone. |
| <a href="#command-apple-ads-campaigns"><code>af&nbsp;apple&#8209;ads&nbsp;campaigns</code></a> | List your Apple Ads campaigns with each one's status, budget, targeted countries, and schedule. |
| <a href="#command-apple-ads-ad-groups"><code>af&nbsp;apple&#8209;ads&nbsp;ad&#8209;groups</code></a> | List Apple Ads ad groups with each one's default bid, CPA cap, pricing model, and schedule. |
| <a href="#command-apple-ads-keywords"><code>af&nbsp;apple&#8209;ads&nbsp;keywords</code></a> | List a campaign's bid keywords with each keyword's performance (impressions, taps, installs, spend, cost-per-install) over a date range, plus its match type, bid, and whether it's a targeting or negative term. |
| <a href="#command-apple-ads-search-terms"><code>af&nbsp;apple&#8209;ads&nbsp;search&#8209;terms</code></a> | List the actual user search terms that triggered a campaign's ads, each with its all-time performance (impressions, taps, installs, spend, cost-per-install). Use these to discover new keywords to bid on or exclude. |
| <a href="#command-apple-ads-report"><code>af&nbsp;apple&#8209;ads&nbsp;report</code></a> | Report Apple Ads performance per campaign (impressions, taps, installs, spend, cost-per-install), plus an account-wide total, over a date range. |
| <a href="#command-apple-ads-top-keywords"><code>af&nbsp;apple&#8209;ads&nbsp;top&#8209;keywords</code></a> | Rank a campaign's top-performing keywords by conversion rate, spend, and installs over a date range. Each list holds the top keywords on one metric. |

### Sdks

Look up the SDKs we track.

| Command | Description |
| ------- | ----------- |
| <a href="#command-sdks-list"><code>af&nbsp;sdks&nbsp;list</code></a> | List every known SDK with its id, or search to find a specific one. |

### Docs

Reference docs and guides for specific actions and common tasks.

| Command | Description |
| ------- | ----------- |
| <a href="#command-docs-get"><code>af&nbsp;docs&nbsp;get</code></a> | Return a reference doc or guide by slug. |

### API

| Command | Description |
| ------- | ----------- |
| <a href="#command-api"><code>af&nbsp;api</code></a> | Make a raw API request for endpoints without a dedicated command. Endpoints, parameters, and response shapes are documented at https://docs.appfigures.com. |

### MCP

| Command | Description |
| ------- | ----------- |
| <a href="#command-mcp"><code>af&nbsp;mcp</code></a> | Run an MCP server over stdio for MCP clients like Claude Desktop and Cursor to call Appfigures tools. |

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
<!-- BEGIN auto-generated API REFERENCE ENTRIES -->
<a id="apps-search"></a>
<a id="command-apps-search"></a>
### af apps search

`af apps search <q> [flags]`

Find apps by name or publisher. Returns one row per unified app. Default returns Apple and Google listings; pass `--all-stores` to include other storefronts. To filter apps by estimate values (e.g. apps with >100k downloads last month) use [`explorer list-products`](#command-explorer-list-products). For estimates broken down by time, country, or storefront, use [`metrics query`](#command-metrics-query) with datasets estimates.sales or estimates.revenue.

**Options**

- `<q>` required string. Search query (app name or publisher).
- `--all-stores` boolean, default `false`. Include storefronts beyond Apple and Google: Amazon, Windows, Steam, Roku, LG TV, Samsung TV, and others.
- `--count` integer, default `10`. Number of results to return.
- `--page` integer, default `1`. Page number.

**Examples**

```sh
# Find every Electronic Arts app.
af apps search 'electronic arts'

# Page through long results.
af apps search 'electronic arts' --count=25 --page=2

# Find Minecraft on every storefront (e.g. Amazon, Steam, Windows, Roku; not common).
af apps search minecraft --all-stores
```

---

<a id="apps-tracked"></a>
<a id="command-apps-tracked"></a>
### af apps tracked

`af apps tracked [flags]`

List the apps your Appfigures account tracks.

**Options**

- `--count` integer, default `10`. Number of results to return.
- `--page` integer, default `1`. Page number.
- `--q` string. App name to filter by.
- `--filter-apps-by-id` (integer or string)[]. Only include data about specific apps, by product ID or unified app ID. Takes precedence over the other `filterAppsBy*` keys when set. Storefront, source, or type filters are better for app sets that can be described by those criteria.
- `--filter-apps-by-storefront` string[]. Narrow the account's tracked apps to those on these storefronts (e.g. apple:ios, google_play).
- `--filter-apps-by-source` string[]. Narrow the account's tracked apps by tracking relationship.
- `--filter-apps-by-type` string[]. Narrow the account's tracked apps to products of these types.

**Examples**

```sh
# List your apps with private-data access.
af apps tracked --filter-apps-by-source=own,shared

# List your tracked fitness apps.
af apps tracked --q=fitness

# List just your iOS apps.
af apps tracked --filter-apps-by-storefront=apple:ios

# Page through long results.
af apps tracked --filter-apps-by-source=own,shared --count=50 --page=2

# List tracked competitors.
af apps tracked --filter-apps-by-source=manual

# Find individual IAPs or subscriptions (not common).
af apps tracked --filter-apps-by-type=inapp,subscription
```

---

<a id="apps-get"></a>
<a id="command-apps-get"></a>
### af apps get

`af apps get <app-id> [flags]`

Get an app's record: basic metadata (name, developer, etc) and, if the user tracks it, what data they can access. Pass a product ID for one storefront; unified app ID for all storefronts together.

**Options**

- `<app-id>` required integer or string. The app's unified app ID or product ID.
- `--all-stores` boolean, default `false`. For a unified app ID: include member products across all storefronts (Amazon, Steam, Windows, Roku, etc.). When false, `member_products` is restricted to storefronts with app-intelligence coverage (iOS + Google Play). Ignored for product IDs.

**Examples**

```sh
# Get Minecraft's unified-app record (iOS + Google Play by default).
af apps get ua_X7iNgb

# Get Minecraft's Google Play product record.
af apps get 6938219
```

---

<a id="explorer-list-products"></a>
<a id="command-explorer-list-products"></a>
### af explorer list-products

`af explorer list-products [flags]`

Read catalog fields for one app or many. Fields referenced by `query` or `sort` come back automatically; pass `--extra-fields` for more. Use `["match","product_id",<id>]` for a single app, or combine filters for population queries (e.g. iOS apps using Firebase with $1M+ US revenue). The 120+ fields span ranks, ratings, download and revenue estimates, SDKs, demographics, and more; query grammar and field list in [`docs get catalog_playbook`](#command-docs-get).

**Options**

- `--query` array, default `[]`. Explorer query in JSON array format to select matching catalog Products. Missing values and `[]` match every Product across every storefront. The full field list and query syntax are documented in [`docs get catalog_playbook`](#command-docs-get).
- `--extra-fields` string[]. Additional fields to include beyond those your `query` or `sort` already reference. Find field paths (and which you can read) with [`explorer describe-fields`](#command-explorer-describe-fields).
- `--sort` string. Explorer field name. The full field list is documented in [`docs get catalog_playbook`](#command-docs-get).
- `--order` string, default `desc`. Sort direction.
- `--count` integer, default `10`. Number of results to return.
- `--page` integer, default `1`. Page number.
- `--allow-unscoped-nested` boolean, default `false`. Escape hatch for intentionally broad queries. Bypasses the default block on unscoped nested predicates that usually inflate results.

**Examples**

```sh
# Find iOS apps that have Firebase installed.
af explorer list-products --query='["and",["match","storefronts","apple:ios"],["nested","all_sdks",["and",["match","all_sdks.id","firebase"],["match","all_sdks.active",true]]]]'

# Rank the biggest US iOS games by revenue.
af explorer list-products --query='["and",["match","storefronts","apple:ios"],["match","categories.all",6014]]' --sort='custom_meta[country=us].revenue_estimates_sum_30_days' --order=desc --count=25

# Find US iOS apps in the $100k–$1M/month revenue tier.
af explorer list-products --query='["and",["match","storefronts","apple:ios"],["nested","custom_meta",["and",["match","custom_meta.country","us"],["match","custom_meta.revenue_estimates_sum_30_days",["number_range",100000,1000000]]]]]'

# Page through results.
af explorer list-products --query='["and",["match","storefronts","apple:ios"],["match","categories.all",6014]]' --count=50 --page=2

# Pass `--extra-fields` for columns the query doesn't already reference. Common for single-app reads.
af explorer list-products --query='["match","product_id",304004187384]' --extra-fields='custom_meta[country=zz].revenue_estimates_sum_365_days,all_sdks[id=firebase].active'
```

---

<a id="explorer-aggregate-products"></a>
<a id="command-explorer-aggregate-products"></a>
### af explorer aggregate-products

`af explorer aggregate-products <fields> [flags]`

Aggregate across the full catalog of millions of products across Apple, Google Play, Amazon, and other major stores: counts, averages, min/max, and histograms over any set of matching products. Uses the same query grammar as [`explorer list-products`](#command-explorer-list-products); returns aggregates, not product records. For market sizing, benchmarking, and segment analysis.

**Options**

- `<fields>` required string[]. Field+aggregation pairs (e.g. `all_rating/stats`, `storefronts/terms`). Aggregations: `stats`, `terms`, `histogram`, `date_histogram`, `cardinality`. The full field list is documented in [`docs get catalog_playbook`](#command-docs-get).
- `--query` array, default `[]`. Explorer query in JSON array format to select matching catalog Products. Missing values and `[]` match every Product across every storefront. The full field list and query syntax are documented in [`docs get catalog_playbook`](#command-docs-get).
- `--allow-unscoped-nested` boolean, default `false`. Escape hatch for intentionally broad queries. Bypasses the default block on unscoped nested predicates that usually inflate results.
- `--terms-count` integer, default `20`. Maximum buckets returned for each `terms` aggregation. Other aggregation types ignore it.
- `--date-histogram-interval` string. Bucket granularity for each `date_histogram` aggregation. Other aggregation types ignore it.

**Examples**

```sh
# How many monthly downloads does an average iOS app get in Japan?
af explorer aggregate-products 'custom_meta[country=jp].download_estimates_average_30_days/stats' --query='["and",["match","storefronts","apple:ios"],["match","countries","jp"]]'

# What's the rating, category mix, and developer concentration for US iOS apps in the $100k–$10M/mo net-revenue tier?
af explorer aggregate-products all_rating/stats,categories.all/terms,developer_id/cardinality --query='["and",["match","storefronts","apple:ios"],["nested","custom_meta",["and",["match","custom_meta.revenue_estimates_sum_30_days",["number_range",100000,10000000]],["match","custom_meta.country","us"]]]]'

# Are new iOS games still launching at the same rate as two years ago?
af explorer aggregate-products release_date/date_histogram --query='["and",["match","storefronts","apple:ios"],["match","categories.all",6014],["match","release_date",["range","2024-01-01","2025-12-31"]]]'

# What SDKs do apps commonly ship alongside OneSignal?
af explorer aggregate-products 'all_sdks[*].id/terms' --query='["nested","all_sdks",["and",["match","all_sdks.id","onesignal"],["match","all_sdks.active",true]]]'

# How do iOS app ratings distribute?
af explorer aggregate-products all_rating/histogram --query='["match","storefronts","apple:ios"]'

# How many apps are on each storefront?
af explorer aggregate-products storefronts/terms
```

---

<a id="explorer-describe-fields"></a>
<a id="command-explorer-describe-fields"></a>
### af explorer describe-fields

`af explorer describe-fields [flags]`

List the catalog fields and the current user's access level for each. Search by keyword to find fields. Same field set [`explorer list-products`](#command-explorer-list-products) and [`explorer aggregate-products`](#command-explorer-aggregate-products) accept.

**Options**

- `--count` integer, default `50`. Number of results to return.
- `--page` integer, default `1`. Page number.
- `--q` string. Filter by `path`, `title`, `description`, `type`.

**Examples**

```sh
# Search for revenue-related fields.
af explorer describe-fields --q=revenue

# List every catalog field with the current user's access level.
af explorer describe-fields
```

---

<a id="metrics-query"></a>
<a id="command-metrics-query"></a>
### af metrics query

`af metrics query <dataset> [flags]`

Query any numeric dataset for one or more apps. Optionally grouped by up to two dimensions, returned as a nested partition tree, not app records. Independently filterable by country, device type, and date range. `filterAppsBy*` options narrow the app set (by ID, storefront, source, or type); without one, a query covers every app the account tracks.

**Options**

- `<dataset>` required string. Dataset to query (e.g. sales.combined_downloads). See [`metrics describe-datasets`](#command-metrics-describe-datasets) for the full list and which datasets are private data (visible only for apps you own or that were shared).
- `--group-by` string[]. Dimensions to group by. Max 2: the first slot becomes the outer entity type, the second the inner series. Each dimension multiplies the result size.
- `--granularity` string. Time granularity when grouping by date
- `--count` integer. Row cap. With `--group-by`, top N of the outer entity type by value (earliest N when grouping by date). Without `--group-by`, single-page preview.
- `--countries` string[]. Filter to one or more ISO country codes (e.g. US, JP, GB)
- `--device-type` string. Device type
- `--all-time` boolean, default `false`. Opt in to the entire history. Without this flag (and without `start`/`end`), the query defaults to the last 30 days. Mutually exclusive with `start` and `end`.
- `--filter-apps-by-id` (integer or string)[]. Only include data about specific apps, by product ID or unified app ID. Takes precedence over the other `filterAppsBy*` keys when set. Storefront, source, or type filters are better for app sets that can be described by those criteria.
- `--filter-apps-by-storefront` string[]. Narrow the account's tracked apps to those on these storefronts (e.g. apple:ios, google_play).
- `--filter-apps-by-source` string[]. Narrow the account's tracked apps by tracking relationship.
- `--filter-apps-by-type` string[]. Narrow the account's tracked apps to products of these types.
- `--start` string. Start date (YYYY-MM-DD)
- `--end` string. End date (YYYY-MM-DD, defaults to today)

**Examples**

```sh
# Get total downloads across your apps with private-data access.
af metrics query sales.combined_downloads --filter-apps-by-source=own,shared

# Get revenue split by storefront, plus a top-level total.
af metrics query sales.combined_revenue --filter-apps-by-source=own,shared --group-by=storefront

# Rank the top 5 tracked competitors by estimated monthly revenue.
af metrics query estimates.revenue --filter-apps-by-source=manual --group-by=product --count=5

# Track Candy Crush Saga's daily download estimates.
af metrics query estimates.sales --filter-apps-by-id=ua_V1Q1uX --group-by=date --granularity=daily

# Track net monthly recurring revenue per app, month over month.
af metrics query subscriptions.mrr --filter-apps-by-source=own,shared --group-by=product,date --granularity=monthly

# Get Minecraft's new ratings.
af metrics query ratings.new_total --filter-apps-by-id=ua_X7iNgb

# Track daily ad spend across your apps.
af metrics query adspend.cost --filter-apps-by-source=own,shared --group-by=date --granularity=daily

# Compare Candy Crush's December 2025 downloads across the US, Japan, and UK.
af metrics query estimates.sales --filter-apps-by-id=ua_V1Q1uX --countries=US,JP,GB --group-by=country --start=2025-12-01 --end=2025-12-31

# Track all-time monthly revenue across your apps with private-data access.
af metrics query sales.combined_revenue --filter-apps-by-source=own,shared --group-by=date --granularity=monthly --all-time

# Get Minecraft's review volume by country.
af metrics query reviews.total --filter-apps-by-id=ua_X7iNgb --group-by=country
```

---

<a id="metrics-describe-datasets"></a>
<a id="command-metrics-describe-datasets"></a>
### af metrics describe-datasets

`af metrics describe-datasets [flags]`

List every numeric dataset [`metrics query`](#command-metrics-query) accepts, one row per dataset with its value type and whether it's limited to your own apps.

**Options**

- `--count` integer, default `50`. Number of results to return.
- `--page` integer, default `1`. Page number.
- `--q` string. Filter by `dataset`, `value_type`, `label`, `description`.

**Examples**

```sh
# Search by keyword (matches name, label, or description).
af metrics describe-datasets --q='combined downloads'

# Look up one dataset by its exact name.
af metrics describe-datasets --q=sales.combined_downloads

# List every dataset with its value type and whether it's limited to your own apps.
af metrics describe-datasets
```

---

<a id="store-app-ranks"></a>
<a id="command-store-app-ranks"></a>
### af store app-ranks

`af store app-ranks <app-ids> [flags]`

Trace rank history for one or more apps across countries, device types, category subtypes, and categories, as time-series positions with day-over-day deltas.

**Options**

- `<app-ids>` required (integer or string)[]. App identifiers (unified app IDs or product IDs)
- `--countries` string[]. Country codes to query. Defaults to every country with rank coverage.
- `--granularity` string, default `hourly`. Sampling rate. Hourly gives the freshest data; pass `--granularity=daily` for compact multi-day history.
- `--device-types` string[], default `["handheld"]`. Which device types to include; each ranks in its own chart. Add more to widen the response.
- `--subtypes` string[], default `["free"]`. Which category subtypes to include; each ranks in its own chart. Add more to widen the response.
- `--category-ids` integer[]. Filter response rows to specific category IDs; omit for all. Category IDs come from [`store categories`](#command-store-categories).
- `--start` string. Start date (YYYY-MM-DD)
- `--end` string. End date (YYYY-MM-DD, defaults to today)
- `--count` integer, default `10`. Number of results to return.
- `--page` integer, default `1`. Page number.

**Examples**

```sh
# Check ChatGPT's current ranks (unified app).
af store app-ranks ua_miTXv6 --countries=US

# Check ChatGPT's current ranks on one storefront.
af store app-ranks 336744124021 --countries=US

# Compare ChatGPT's ranks across the US, UK, and Japan.
af store app-ranks ua_miTXv6 --countries=US,GB,JP

# Check Procreate's paid iPad chart ranks.
af store app-ranks ua_CxA1MS --subtypes=paid --device-types=tablet --countries=US

# Trace ChatGPT's chart history through December 2025.
af store app-ranks ua_miTXv6 --granularity=daily --start=2025-12-01 --end=2025-12-31 --countries=US

# Check ChatGPT's rank in one category (US iOS Productivity).
af store app-ranks 336744124021 --category-ids=6007 --countries=US
```

---

<a id="store-top-charts"></a>
<a id="command-store-top-charts"></a>
### af store top-charts

`af store top-charts [flags]`

List the top apps in a category chart for a given country and category, with current positions and day-over-day deltas.

**Options**

- `--country` required string. ISO country code (e.g. US, JP, GB)
- `--category-id` required integer. Category IDs come from [`store categories`](#command-store-categories).
- `--subtype` string, default `free`. Category subtype (chart variant within the category).
- `--date` string. Snapshot date (YYYY-MM-DD, defaults to current).
- `--count` integer, default `10`. Number of results to return.
- `--page` integer, default `1`. Page number.

**Examples**

```sh
# Find the Games category, then pull its US chart.
af store categories --q=games
af store top-charts --country=US --category-id=6014

# List top paid apps on the US App Store.
af store top-charts --country=US --category-id=25204 --subtype=paid

# List top free apps on the Japan App Store.
af store top-charts --country=JP --category-id=25204

# List top free apps on Google Play in the US.
af store top-charts --country=US --category-id=100

# List top free apps on the US App Store in December 2025.
af store top-charts --country=US --category-id=25204 --date=2025-12-01
```

---

<a id="store-categories"></a>
<a id="command-store-categories"></a>
### af store categories

`af store categories [flags]`

List every store category with its ID. Numeric category IDs required by [`store app-ranks --category-ids`](#command-store-app-ranks) and [`store top-charts --category-id`](#command-store-top-charts) are available here.

**Options**

- `--count` integer, default `50`. Number of results to return.
- `--page` integer, default `1`. Page number.
- `--q` string. Filter by `name`.
- `--sort` string. Field to sort by. Omit to order by relevance when `q` is set, otherwise list order.
- `--order` string, default `desc`. Sort direction.
- `--category-id` integer[]. Only return these category IDs.
- `--parent-id` integer. Only include subcategories of this parent category (drill-down by id).
- `--storefront` string[]. Only include categories from these storefronts (e.g. `apple:ios`, `google_play`).
- `--device-type` string[]. Only include categories for these device types (e.g. `handheld`, `tablet`).
- `--all` boolean, default `false`. Include non-rank stores (roku, vizio, etc.). These have categories but no rank data.

**Examples**

```sh
# Find the Games category.
af store categories --q=games

# List every Apple iOS category.
af store categories --storefront=apple:ios

# List subcategories of a parent category (here, Apple Games).
af store categories --parent-id=6014

# Include categories from non-rank-supporting stores (e.g. Roku, Vizio) (not common).
af store categories --all
```

---

<a id="store-featured"></a>
<a id="command-store-featured"></a>
### af store featured

`af store featured <app-id> [flags]`

List featured and editorial placements for an app or storefront product. Request 0 rows for summary stats only.

**Options**

- `<app-id>` required integer or string. The app's unified app ID or product ID.
- `--countries` string[]. Countries to include. Omit to query US only, or pass multiple to compare markets. To include every country, set `--all-countries` instead.
- `--all-countries` boolean, default `false`. Include every country. Cannot be combined with `--countries`.
- `--include-rank-trend` boolean, default `false`. Include per-interval rank_trend for each placement.
- `--sort` string, default `relevance`. Sort placements by relevance or date.
- `--order` string, default `desc`. Sort direction.
- `--start` string. Start date (YYYY-MM-DD)
- `--end` string. End date (YYYY-MM-DD, defaults to today). Spans at most 31 days.
- `--count` integer, default `10`. Number of results to return.
- `--page` integer, default `1`. Page number.

**Examples**

```sh
# List Minecraft's recent featured placements (unified app).
af store featured ua_X7iNgb

# List Minecraft's recent featured placements on one storefront.
af store featured 10157213

# Compare Minecraft's placement coverage across the US, UK, and Japan.
af store featured ua_X7iNgb --countries=US,GB,JP

# List Minecraft's placements during a specific month (December 2025).
af store featured ua_X7iNgb --start=2025-12-01 --end=2025-12-31

# List Minecraft's placements with rank history.
af store featured ua_X7iNgb --include-rank-trend

# List Minecraft's placements sorted by end date, newest run first.
af store featured ua_X7iNgb --sort=date --order=desc

# Get Minecraft's placement summary only.
af store featured ua_X7iNgb --count=0
```

---

<a id="store-app-listing"></a>
<a id="command-store-app-listing"></a>
### af store app-listing

`af store app-listing <product-id> [flags]`

Read the full store listing for one storefront: localized text (name, subtitle, description, release notes) plus screenshots, video, categories, monetization, supported devices, country availability, price, file size, and age rating. Takes a numeric product ID (one storefront at a time; a unified app has one product per storefront). One locale per request.

**Options**

- `<product-id>` required integer. Numeric product ID for one storefront. Not a unified app ID. Member product_id values are available from [`apps get '<unified-app-id>'`](#command-apps-get).
- `--language` string. Locale (e.g. en, ja, zh-Hans) for name, subtitle, description, release notes, and screenshots. Defaults to en; falls back to the first available locale when the requested one has no metadata. The response echoes the resolved language.
- `--device-type` string, default `handheld`. Relevant to Apple apps. Pick handheld for iPhone-specific metadata, tablet for iPad, desktop for Mac, etc.

**Examples**

```sh
# Read Minecraft's store listing.
af store app-listing 10157213

# Read Minecraft's Japanese-localized listing.
af store app-listing 10157213 --language=ja

# Read Minecraft's iPad screenshots.
af store app-listing 10157213 --device-type=tablet
```

---

<a id="audience-demographics"></a>
<a id="command-audience-demographics"></a>
### af audience demographics

`af audience demographics <app-id>`

Read an app's audience demographics: the estimated age and gender breakdown.

**Options**

- `<app-id>` required integer or string. The app's unified app ID or product ID.

**Examples**

```sh
# Read Minecraft's audience across all its storefronts (unified app ID).
af audience demographics ua_X7iNgb

# Read Minecraft's audience on Google Play only (product ID).
af audience demographics 6938219
```

---

<a id="audience-cross-usage"></a>
<a id="command-audience-cross-usage"></a>
### af audience cross-usage

`af audience cross-usage <app-id> [flags]`

Find the apps that an app's users also use.

**Options**

- `<app-id>` required integer or string. The app's unified app ID or product ID.
- `--count` integer, default `10`. Number of results to return.
- `--page` integer, default `1`. Page number.

**Examples**

```sh
# Find the apps ChatGPT's users also use (unified app ID).
af audience cross-usage ua_miTXv6

# Find the apps ChatGPT's users also use on the App Store only (product ID).
af audience cross-usage 336744124021
```

---

<a id="reviews-list"></a>
<a id="command-reviews-list"></a>
### af reviews list

`af reviews list [flags]`

Read individual reviews for one or more apps. Returns review text, star rating, country, and app version. Filterable by star rating, date range, country, version, and tracking relationship.

**Options**

- `--stars` number[]. Filter by star rating.
- `--versions` string[]. Filter by app version. Pass multiple to combine.
- `--countries` string[]. Filter to one or more ISO country codes (e.g. US, JP, GB).
- `--q` string. Search review title and body. Pass multiple keywords to match any. Case-insensitive; combines with other filters.
- `--sort` string. Sort by review date or star rating.
- `--order` string, default `desc`. Sort direction.
- `--filter-apps-by-id` (integer or string)[]. Only include data about specific apps, by product ID or unified app ID. Takes precedence over the other `filterAppsBy*` keys when set. Storefront, source, or type filters are better for app sets that can be described by those criteria.
- `--filter-apps-by-storefront` string[]. Narrow the account's tracked apps to those on these storefronts (e.g. apple:ios, google_play).
- `--filter-apps-by-source` string[]. Narrow the account's tracked apps by tracking relationship.
- `--filter-apps-by-type` string[]. Narrow the account's tracked apps to products of these types.
- `--start` string. Start date (YYYY-MM-DD)
- `--end` string. End date (YYYY-MM-DD, defaults to today)
- `--count` integer, default `10`. Number of results to return.
- `--page` integer, default `1`. Page number. 1-500.

**Examples**

```sh
# Read Minecraft's recent reviews.
af reviews list --filter-apps-by-id=ua_X7iNgb

# Read Minecraft's 5-star reviews.
af reviews list --filter-apps-by-id=ua_X7iNgb --stars=5

# Compare Minecraft's reviews across the US, Japan, and the UK.
af reviews list --filter-apps-by-id=ua_X7iNgb --countries=US,JP,GB

# Read Minecraft's December 2025 reviews.
af reviews list --filter-apps-by-id=ua_X7iNgb --start=2025-12-01 --end=2025-12-31

# Page through long results across your own apps.
af reviews list --filter-apps-by-source=own --count=50 --page=2
```

---

<a id="reviews-breakdown"></a>
<a id="command-reviews-breakdown"></a>
### af reviews breakdown

`af reviews breakdown [flags]`

Aggregate review counts for one or more apps, bucketed by dimension. Returns one count per dimension value, plus a global total across the matched set.

**Options**

- `--stars` number[]. Filter by star rating.
- `--versions` string[]. Filter by app version. Pass multiple to combine.
- `--countries` string[]. Filter to one or more ISO country codes (e.g. US, JP, GB).
- `--q` string. Search review title and body. Pass multiple keywords to match any. Case-insensitive; combines with other filters.
- `--filter-apps-by-id` (integer or string)[]. Only include data about specific apps, by product ID or unified app ID. Takes precedence over the other `filterAppsBy*` keys when set. Storefront, source, or type filters are better for app sets that can be described by those criteria.
- `--filter-apps-by-storefront` string[]. Narrow the account's tracked apps to those on these storefronts (e.g. apple:ios, google_play).
- `--filter-apps-by-source` string[]. Narrow the account's tracked apps by tracking relationship.
- `--filter-apps-by-type` string[]. Narrow the account's tracked apps to products of these types.
- `--start` string. Start date (YYYY-MM-DD)
- `--end` string. End date (YYYY-MM-DD, defaults to today)
- `--by` string[]. Limit the response to these dimensions; omit to return all.
- `--top` integer, default `20`. Maximum values returned per dimension; the rest are summed under `__other__`.

**Examples**

```sh
# Break down Minecraft's recent reviews.
af reviews breakdown --filter-apps-by-id=ua_X7iNgb

# Where are Minecraft's biggest fans writing from?
af reviews breakdown --filter-apps-by-id=ua_X7iNgb --stars=5

# Count Minecraft's December 2025 5-star reviews.
af reviews breakdown --filter-apps-by-id=ua_X7iNgb --stars=5 --start=2025-12-01 --end=2025-12-31

# How many Minecraft reviewers raved?
af reviews breakdown --filter-apps-by-id=ua_X7iNgb --q='love amazing fun great best'

# Compare review volume across your own apps.
af reviews breakdown --filter-apps-by-source=own
```

---

<a id="reviews-reply"></a>
<a id="command-reviews-reply"></a>
### af reviews reply

`af reviews reply <review-id> [flags]`

Post or withdraw a developer response on a specific review. Pass `content` to post; pass `delete: true` to withdraw a previously-posted response. Returns the resulting state (`published`/`pending` for a post, `removed`/`removal_pending` for a withdrawal) along with the submitting account.

**Options**

- `<review-id>` required string. Review to act on. Use `review_id` from [`reviews list`](#command-reviews-list).
- `--content` string. Response text the developer wants to publish.
- `--delete` boolean. Withdraw the previously-posted response on this review. Mutually exclusive with `content`.

**Examples**

```sh
# Reply to a low-star review after shipping a fix.
af reviews reply rev123 --content='We just shipped a fix in v2.1. Let us know if you still see this.'

# Withdraw a previously-posted response.
af reviews reply rev123 --delete
```

---

<a id="keywords-organic"></a>
<a id="command-keywords-organic"></a>
### af keywords organic

`af keywords organic [flags]`

Check the organic keywords one or more apps rank for, with position, popularity, and competitiveness.

**Options**

- `--product-ids` integer[]. Product identifiers (numeric, one storefront each).
- `--countries` required string[]. One or more ISO country codes (e.g. US, JP, GB). Pass several to compare markets.
- `--device-type` string. Device type
- `--count` integer, default `10`. Number of results to return (min 10).
- `--page` integer, default `1`. Page number.

**Examples**

```sh
# Check ChatGPT's current US keyword rankings.
af keywords organic --product-ids=336744124021 --countries=US

# Check ChatGPT's iPad-only keyword rankings.
af keywords organic --product-ids=336744124021 --countries=US --device-type=tablet

# Compare ChatGPT and Gemini's US keyword rankings.
af keywords organic --product-ids=336744124021,337217072531 --countries=US
```

---

<a id="keywords-paid"></a>
<a id="command-keywords-paid"></a>
### af keywords paid

`af keywords paid <product-ids> [flags]`

List the paid keywords one or more apps run ads on, with impression share and organic rank.

**Options**

- `<product-ids>` required integer[]. Product identifiers (numeric, one storefront each).
- `--days` integer, default `180`. Lookback period in days. Common values: 7, 14, 30, 90, 180, 365.
- `--countries` required string[]. One or more ISO country codes (e.g. US, JP, GB). Pass several to compare markets.
- `--device-types` string[]. Filter by device type. Defaults to handheld.
- `--count` integer, default `10`. Number of results to return (min 10).
- `--page` integer, default `1`. Page number.

**Examples**

```sh
# Find Headspace's US paid keywords.
af keywords paid 15250929 --countries=US

# Compare Headspace's and Calm's US paid keywords.
af keywords paid 15250929,304554144 --countries=US

# Find Headspace's paid keywords across the US, UK, and Japan.
af keywords paid 15250929 --countries=US,GB,JP

# Find Headspace's US iPad paid keywords.
af keywords paid 15250929 --countries=US --device-types=tablet

# Check Headspace's US paid keywords over the last 30 days.
af keywords paid 15250929 --countries=US --days=30
```

---

<a id="keywords-tracked-ranks"></a>
<a id="command-keywords-tracked-ranks"></a>
### af keywords tracked-ranks

`af keywords tracked-ranks <product-id> [flags]`

View where all your tracked keywords rank for a single app+country combo, with each keyword's current position, movement since it last changed, starting position, popularity, and competitiveness.

**Options**

- `<product-id>` required integer. Numeric product ID for one storefront. Not a unified app ID. Member product_id values are available from [`apps get '<unified-app-id>'`](#command-apps-get).
- `--country` required string. ISO country code (e.g. US, JP, GB)
- `--device-type` string. Device to read ranks for. Omit to use the store default.
- `--count` integer, default `10`. Number of results to return (min 10).
- `--page` integer, default `1`. Page number.
- `--sort` string. Field to order results by.
- `--order` string, default `desc`. Sort direction.
- `--start` string. Start of the window (YYYY-MM-DD). Omit the range for the last 7 days; the rank on the start date is the starting-position baseline.
- `--end` string. End of the window (YYYY-MM-DD, defaults to today). Spans at most 31 days.
- `--keyword-term` string. Only include tracked keywords whose term contains this text.
- `--min-position` integer. Best rank to include (1 = top).
- `--max-position` integer. Worst rank to include.
- `--min-popularity` integer. Lowest popularity to include (0-100).
- `--max-popularity` integer. Highest popularity to include (0-100).
- `--min-competitiveness` integer. Lowest competitiveness to include (0-100).
- `--max-competitiveness` integer. Highest competitiveness to include (0-100).

**Examples**

```sh
# Check how ChatGPT's tracked keywords are ranking.
af keywords tracked-ranks 336744124021 --country=US

# List ChatGPT's best-ranking keywords first.
af keywords tracked-ranks 336744124021 --country=US --sort=position --order=asc

# Show only the keywords ChatGPT ranks in the top 10.
af keywords tracked-ranks 336744124021 --country=US --max-position=10

# Find the most-searched keywords ChatGPT should prioritize.
af keywords tracked-ranks 336744124021 --country=US --sort=popularity --min-popularity=50

# Trace ChatGPT's keyword movement across a custom week.
af keywords tracked-ranks 336744124021 --country=US --start=2026-01-06 --end=2026-01-12

# Page through a long tracked keyword set.
af keywords tracked-ranks 336744124021 --country=US --page=2
```

---

<a id="keywords-tracked-trend"></a>
<a id="command-keywords-tracked-trend"></a>
### af keywords tracked-trend

`af keywords tracked-trend <keyword-id> [flags]`

Trace how one tracked keyword's rank changes over time for a single app+country combo. Each point gives the rank and how many positions it moved since the one before.

**Options**

- `<keyword-id>` required string. The keyword to trace. Must be tracked for this app and country; its opaque id comes from [`keywords tracked-ranks`](#command-keywords-tracked-ranks) or [`keywords tracked`](#command-keywords-tracked).
- `--product-id` required integer. Numeric product ID for one storefront. Not a unified app ID. Member product_id values are available from [`apps get '<unified-app-id>'`](#command-apps-get).
- `--country` required string. ISO country code (e.g. US, JP, GB)
- `--device-type` string. Device to read ranks for. Omit to use the store default.
- `--granularity` string, default `daily`. Sampling rate.
- `--start` string. Start of the window (YYYY-MM-DD). Omit the range for the last 7 days.
- `--end` string. End of the window (YYYY-MM-DD, defaults to today). Spans at most 14 days for hourly granularity, 31 for daily.

**Examples**

```sh
# Find a tracked keyword, then trace its rank day by day.
af keywords tracked-ranks 336744124021 --country=US
af keywords tracked-trend 00f2b1ead3a0990b818517356cb40280 --product-id=336744124021 --country=US

# Trace a keyword's rank for ChatGPT across a specific week.
af keywords tracked-trend 00f2b1ead3a0990b818517356cb40280 --product-id=336744124021 --country=US --start=2026-01-06 --end=2026-01-12

# Trace a keyword hour by hour.
af keywords tracked-trend 00f2b1ead3a0990b818517356cb40280 --product-id=336744124021 --country=US --granularity=hourly
```

---

<a id="keywords-suggestions"></a>
<a id="command-keywords-suggestions"></a>
### af keywords suggestions

`af keywords suggestions <product-id> [flags]`

Discover keyword ideas to consider targeting for a single app+country combo, ranked by relevance to the app and including some drawn from apps you compete with. Each comes with its popularity, competitiveness, and the app's current rank.

**Options**

- `<product-id>` required integer. Numeric product ID for one storefront. Not a unified app ID. Member product_id values are available from [`apps get '<unified-app-id>'`](#command-apps-get).
- `--country` required string. ISO country code (e.g. US, JP, GB)
- `--device-type` string. Device to read ranks for. Omit to use the store default.
- `--count` integer, default `10`. Number of results to return.
- `--page` integer, default `1`. Page number.

**Examples**

```sh
# Discover keywords ChatGPT should consider targeting.
af keywords suggestions 336744124021 --country=US

# Find keyword ideas for ChatGPT in Japan.
af keywords suggestions 336744124021 --country=JP

# Pull a broader set of suggestions.
af keywords suggestions 336744124021 --country=US --count=50
```

---

<a id="keywords-ranking-apps"></a>
<a id="command-keywords-ranking-apps"></a>
### af keywords ranking-apps

`af keywords ranking-apps <keyword-term> [flags]`

List the apps ranking for a specific keyword in organic search, plus the keyword's own popularity and competitiveness scores.

**Options**

- `<keyword-term>` required string. Keyword to look up.
- `--country` required string. ISO country code (e.g. US, JP, GB)
- `--storefront` required string. App store platform (e.g. apple:ios, google_play, amazon_appstore, steam, windows10, apple:mac, apple:tv, apple:imessage, or another supported storefront).
- `--device-type` string. Device type
- `--count` integer, default `10`. Number of results to return.
- `--page` integer, default `1`. Page number.

**Examples**

```sh
# Find US iOS apps ranking for "fitness".
af keywords ranking-apps fitness --country=US --storefront=apple:ios

# Find Google Play apps ranking for "fitness".
af keywords ranking-apps fitness --country=US --storefront=google_play

# Find US iPad apps ranking for "meditation".
af keywords ranking-apps meditation --country=US --storefront=apple:ios --device-type=tablet
```

---

<a id="keywords-advertisers"></a>
<a id="command-keywords-advertisers"></a>
### af keywords advertisers

`af keywords advertisers <keyword-term> [flags]`

List the apps advertising on a specific keyword, with each advertiser's impression share, organic rank, and how long they've been bidding.

**Options**

- `<keyword-term>` required string. Keyword to look up advertisers for
- `--days` integer, default `180`. Lookback period in days. Common values: 7, 14, 30, 90, 180, 365.
- `--country` required string. ISO country code (e.g. US, JP, GB)
- `--device-type` string. Device type
- `--count` integer, default `10`. Number of results to return.
- `--page` integer, default `1`. Page number.

**Examples**

```sh
# Find US apps advertising on the "notion" brand.
af keywords advertisers notion --country=US

# Find US iPad apps advertising on "meditation".
af keywords advertisers meditation --country=US --device-type=tablet

# Find US apps advertising on "fitness" over the last 30 days.
af keywords advertisers fitness --country=US --days=30
```

---

<a id="keywords-related"></a>
<a id="command-keywords-related"></a>
### af keywords related

`af keywords related <keyword-term> [flags]`

Find keywords related to a seed term for ASO research. Useful for finding alternatives with a similar audience that are more popular or less competitive.

**Options**

- `<keyword-term>` required string. Seed keyword to find related terms for.
- `--country` required string. ISO country code (e.g. US, JP, GB)
- `--storefront` required string. App store platform (e.g. apple:ios, google_play, amazon_appstore, steam, windows10, apple:mac, apple:tv, apple:imessage, or another supported storefront).
- `--device-type` string. Device type
- `--count` integer, default `10`. Number of results to return.
- `--page` integer, default `1`. Page number.

**Examples**

```sh
# Find US iOS keywords related to "fitness".
af keywords related fitness --country=US --storefront=apple:ios

# Find US Google Play keywords related to "fitness".
af keywords related fitness --country=US --storefront=google_play

# Find US iPad keywords related to "meditation".
af keywords related meditation --country=US --storefront=apple:ios --device-type=tablet
```

---

<a id="keywords-tracked"></a>
<a id="command-keywords-tracked"></a>
### af keywords tracked

`af keywords tracked [flags]`

List tracked keywords with their opaque IDs.

**Options**

- `--count` integer, default `10`. Number of results to return.
- `--page` integer, default `1`. Page number.
- `--q` string. Filter by `keyword_term`.
- `--sort` string. Field to sort by. Omit to order by relevance when `q` is set, otherwise list order.
- `--order` string, default `desc`. Sort direction.
- `--include-relationships` boolean, default `false`. Include per-(product, country) tracking detail and sync state on each row. Off by default; adds a nested block per tracked (product, country) pair.

**Examples**

```sh
# List every tracked keyword.
af keywords tracked

# Search tracked keywords for "fitness".
af keywords tracked --q=fitness

# List the most-recently-tracked keywords first.
af keywords tracked --sort=added_on

# Show each keyword's tracking and sync detail.
af keywords tracked --include-relationships
```

---

<a id="keywords-track"></a>
<a id="command-keywords-track"></a>
### af keywords track

`af keywords track <keyword-term> [flags]`

Track a keyword to monitor your app's hourly rank for it over time and get automatic alerts when its position moves.

**Options**

- `<keyword-term>` required string. Keyword to start tracking
- `--product-id` required integer. Product ID of the app to track the keyword for
- `--country` required string. ISO country code (e.g. US, JP, GB)

**Examples**

```sh
# Start tracking "meditation" for one of your apps in the US.
af keywords track meditation --product-id=336744124021 --country=US

# Track "workout" in Japan.
af keywords track workout --product-id=336744124021 --country=JP
```

---

<a id="keywords-untrack"></a>
<a id="command-keywords-untrack"></a>
### af keywords untrack

`af keywords untrack <keyword-id>`

Stop tracking a keyword.

**Options**

- `<keyword-id>` required string. Identifier of a tracked keyword row (returned by [`keywords tracked`](#command-keywords-tracked)). Not the keyword text.

**Examples**

```sh
# Stop tracking a keyword.
af keywords untrack a1b2c3d4e5f6a7b8c9d0e1f2a3b4c5d6
```

---

<a id="apple-ads-organizations"></a>
<a id="command-apple-ads-organizations"></a>
### af apple-ads organizations

`af apple-ads organizations [flags]`

List the Apple Ads organizations you manage campaigns in, with each one's currency and timezone.

**Options**

- `--count` integer, default `10`. Number of results to return.
- `--page` integer, default `1`. Page number.

**Examples**

```sh
# List all the Apple Ads organizations you manage.
af apple-ads organizations
```

---

<a id="apple-ads-campaigns"></a>
<a id="command-apple-ads-campaigns"></a>
### af apple-ads campaigns

`af apple-ads campaigns [flags]`

List your Apple Ads campaigns with each one's status, budget, targeted countries, and schedule.

**Options**

- `--display-status` string. Filter to campaigns in one status. Omit to include all statuses.
- `--name` string. Filter to campaigns whose name contains this text.
- `--countries` string[]. Filter to campaigns targeting any of these countries.
- `--count` integer, default `10`. Number of results to return.
- `--page` integer, default `1`. Page number.

**Examples**

```sh
# List all your Apple Ads campaigns.
af apple-ads campaigns

# List only your running campaigns.
af apple-ads campaigns --display-status=running

# Search your campaigns by name.
af apple-ads campaigns --name=Brand

# List campaigns targeting the US or UK.
af apple-ads campaigns --countries=US,GB
```

---

<a id="apple-ads-ad-groups"></a>
<a id="command-apple-ads-ad-groups"></a>
### af apple-ads ad-groups

`af apple-ads ad-groups [campaign-id] [flags]`

List Apple Ads ad groups with each one's default bid, CPA cap, pricing model, and schedule.

**Options**

- `[campaign-id]` string. Scope to ad groups in one campaign.
- `--display-status` string. Filter to ad groups in one status. Omit to include all statuses.
- `--name` string. Filter to ad groups whose name contains this substring.
- `--count` integer, default `10`. Number of results to return.
- `--page` integer, default `1`. Page number.

**Examples**

```sh
# List all your ad groups across every campaign.
af apple-ads ad-groups

# List the ad groups in one campaign.
af apple-ads ad-groups aac_W9YthU
```

---

<a id="apple-ads-keywords"></a>
<a id="command-apple-ads-keywords"></a>
### af apple-ads keywords

`af apple-ads keywords <campaign-id> [flags]`

List a campaign's bid keywords with each keyword's performance (impressions, taps, installs, spend, cost-per-install) over a date range, plus its match type, bid, and whether it's a targeting or negative term.

**Options**

- `<campaign-id>` required string. Campaign whose bid keywords to list.
- `--ad-group-id` string. Filter to keywords in one ad group.
- `--status` string. Filter to keywords in one status. Omit to include all statuses.
- `--match-type` string. Filter to one match type. Omit to include both.
- `--name` string. Filter to keywords whose text contains this substring.
- `--sort` string. Order keywords by a performance metric or the bid. Omit for newest first.
- `--order` string, default `desc`. Sort direction.
- `--start` string. Start date (YYYY-MM-DD)
- `--end` string. End date (YYYY-MM-DD, defaults to today)
- `--count` integer, default `10`. Number of results to return.
- `--page` integer, default `1`. Page number.

**Examples**

```sh
# Find a running campaign, then list its bid keywords.
af apple-ads campaigns --display-status=running
af apple-ads keywords aac_W9YthU

# See how a campaign's keywords performed over July 2026.
af apple-ads keywords aac_W9YthU --start=2026-07-01 --end=2026-07-31

# Find a campaign's highest-spending keywords.
af apple-ads keywords aac_W9YthU --sort=spend
```

---

<a id="apple-ads-search-terms"></a>
<a id="command-apple-ads-search-terms"></a>
### af apple-ads search-terms

`af apple-ads search-terms <campaign-id> [flags]`

List the actual user search terms that triggered a campaign's ads, each with its all-time performance (impressions, taps, installs, spend, cost-per-install). Use these to discover new keywords to bid on or exclude.

**Options**

- `<campaign-id>` required string. Campaign whose search terms to list.
- `--count` integer, default `10`. Number of results to return.
- `--page` integer, default `1`. Page number.

**Examples**

```sh
# Find a campaign, then see the user searches that triggered its ads.
af apple-ads campaigns
af apple-ads search-terms aac_uDGL4v
```

---

<a id="apple-ads-report"></a>
<a id="command-apple-ads-report"></a>
### af apple-ads report

`af apple-ads report [flags]`

Report Apple Ads performance per campaign (impressions, taps, installs, spend, cost-per-install), plus an account-wide total, over a date range.

**Options**

- `--campaign-ids` string[]. Limit the report to specific campaigns.
- `--start` string. Start date (YYYY-MM-DD)
- `--end` string. End date (YYYY-MM-DD, defaults to today)
- `--count` integer, default `10`. Number of results to return.
- `--page` integer, default `1`. Page number.

**Examples**

```sh
# See how much an install costs across all your campaigns.
af apple-ads report

# Report on July 2026 performance.
af apple-ads report --start=2026-07-01 --end=2026-07-31

# List campaigns, then report on specific ones.
af apple-ads campaigns
af apple-ads report --campaign-ids=aac_uDGL4v
```

---

<a id="apple-ads-top-keywords"></a>
<a id="command-apple-ads-top-keywords"></a>
### af apple-ads top-keywords

`af apple-ads top-keywords <campaign-id> [flags]`

Rank a campaign's top-performing keywords by conversion rate, spend, and installs over a date range. Each list holds the top keywords on one metric.

**Options**

- `<campaign-id>` required string. Campaign to rank keywords for.
- `--top` integer, default `5`. How many keywords to return per ranking (max 10).
- `--start` string. Start date (YYYY-MM-DD)
- `--end` string. End date (YYYY-MM-DD, defaults to today)

**Examples**

```sh
# Find a campaign, then see its top-performing keywords.
af apple-ads campaigns
af apple-ads top-keywords aac_uDGL4v

# Rank a campaign's keywords for a specific week.
af apple-ads top-keywords aac_uDGL4v --start=2026-07-25 --end=2026-07-31

# Widen each ranking to the maximum of 10 keywords.
af apple-ads top-keywords aac_uDGL4v --top=10
```

---

<a id="sdks-list"></a>
<a id="command-sdks-list"></a>
### af sdks list

`af sdks list [flags]`

List every known SDK with its id, or search to find a specific one.

**Options**

- `--count` integer, default `50`. Number of results to return.
- `--page` integer, default `1`. Page number.
- `--q` string. Filter by `name`, `description`, `tags`.
- `--sort` string. Field to sort by. Omit to order by relevance when `q` is set, otherwise list order.
- `--order` string, default `desc`. Sort direction.
- `--sdk-id` string[]. Only return these SDK ids.
- `--include-inactive` boolean, default `false`. Include inactive SDKs. Rare; most callers want active only.

**Examples**

```sh
# Find OneSignal's id.
af sdks list --q=OneSignal

# Search for analytics SDKs.
af sdks list --q=analytics

# Look up details for several SDK ids.
af sdks list --sdk-id=firebase,admob,onesignal

# Include inactive SDKs in the listing (not common).
af sdks list --include-inactive
```

---

<a id="docs-get"></a>
<a id="command-docs-get"></a>
### af docs get

`af docs get <slug>`

Return a reference doc or guide by slug.

**Options**

- `<slug>` required string. Which reference to return

**Examples**

```sh
# Read the dataset naming conventions and pitfalls for [`metrics query`](#command-metrics-query); the dataset list itself is in [`metrics describe-datasets`](#command-metrics-describe-datasets).
af docs get numeric_metrics

# Read the catalog query grammar, field-reference syntax, aggregation rules, and worked queries for [`explorer list-products`](#command-explorer-list-products) and [`explorer aggregate-products`](#command-explorer-aggregate-products).
af docs get catalog_playbook

# Read the glossary of domain terms seen in responses.
af docs get glossary
```

---

<a id="command-api"></a>
### af api

`af api <path> [flags]`

Make a raw API request for endpoints without a dedicated command. Endpoints, parameters, and response shapes are documented at https://docs.appfigures.com.

**Options**

- `<path>` required string. API path (e.g. /users, /products)
- `--method` string, default `GET`. HTTP method (one of: GET, POST, PUT, PATCH, DELETE, HEAD, OPTIONS)
- `--body` string. Request body (JSON string)

---

<a id="command-mcp"></a>
### af mcp

`af mcp`

Run an MCP server over stdio for MCP clients like Claude Desktop and Cursor to call Appfigures tools.

---

<a id="auth-login"></a>
<a id="command-auth-login"></a>
### af auth login

`af auth login [flags]`

Sign in to Appfigures

Prints a URL to authorize this CLI. Open it, approve access, then re-run with `--code <code>` using the code shown after approval. Pass `--interactive` to sign in through a guided browser flow instead.

For unattended use (CI, scripts), set APPFIGURES_API_KEY in the environment instead. No browser flow needed.

**Options**

- `--code` string. Authorization code from the OAuth consent page. Second step of `af auth login`: exchanges the code, saves the token, exits.
- `--interactive` boolean. Sign in through a guided browser flow.

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
