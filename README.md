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
- **Agent mode by default.** Compact JSON, agent-directed `NOTE:` hints on stderr, no prompts or spinners; pass `--human` for interactive use.
- **Self-describing, offline.** `af docs get catalog_playbook` and `af docs get numeric_metrics` print the full query grammar and dataset catalog without hitting the API — plan queries without round-trips or stale training data.
- **Unattended auth.** Set `APPFIGURES_API_KEY` in the environment; every command runs without a browser or prompt.
- **Raw API escape hatch.** `af api <path>` proxies any endpoint the first-class commands don't cover.

## MCP server

`af mcp` runs a local [Model Context Protocol](https://modelcontextprotocol.io) server over stdio, exposing the CLI's app-intelligence commands as MCP tools. Point any MCP client (Claude Desktop, Cursor, and others) at it to let an agent query app metrics, reviews, and store data directly.

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
| <a href="#command-explorer-describe-fields"><code>af&nbsp;explorer&nbsp;describe&#8209;fields</code></a> | List the catalog fields and the current user's access level for each. Same field set [`explorer list-products`](#command-explorer-list-products) and [`explorer aggregate-products`](#command-explorer-aggregate-products) accept. Pass `--q` to search the catalog by keyword. |

### Metrics

Query numeric datasets across dimensions.

| Command | Description |
| ------- | ----------- |
| <a href="#command-metrics-query"><code>af&nbsp;metrics&nbsp;query</code></a> | Query any numeric dataset for one or more apps. Optionally grouped by up to two dimensions, returned as a nested partition tree, not app records. Independently filterable by country, device type, and date range. `filterAppsBy*` options narrow the app set (by ID, storefront, source, or type). Some datasets require app ownership; others work for any app (with the right plan). |

### Store

App store presence: listing content, category ranks, top charts, and featured placements.

| Command | Description |
| ------- | ----------- |
| <a href="#command-store-app-ranks"><code>af&nbsp;store&nbsp;app&#8209;ranks</code></a> | Trace rank history for one or more apps across countries, device types, category subtypes, and categories, as time-series positions with day-over-day deltas. |
| <a href="#command-store-top-charts"><code>af&nbsp;store&nbsp;top&#8209;charts</code></a> | List the top apps in a category chart for a given country and category, with current positions and day-over-day deltas. |
| <a href="#command-store-categories"><code>af&nbsp;store&nbsp;categories</code></a> | List every store category with its ID. Numeric category IDs required by [`store app-ranks --category-ids`](#command-store-app-ranks) and [`store top-charts --category-id`](#command-store-top-charts) are available here. |
| <a href="#command-store-featured"><code>af&nbsp;store&nbsp;featured</code></a> | List featured and editorial placements for an app or storefront product. Pass `--count=0` for summary stats only. |
| <a href="#command-store-app-listing"><code>af&nbsp;store&nbsp;app&#8209;listing</code></a> | Read the full store listing for one storefront: localized text (name, subtitle, description, release notes) plus screenshots, video, categories, monetization, supported devices, country availability, price, file size, and age rating. Takes a numeric product ID (one storefront at a time; a unified app has one product per storefront). One locale per request. |

### Reviews

Search, summarize, and reply to iOS and Google Play app store reviews.

| Command | Description |
| ------- | ----------- |
| <a href="#command-reviews-list"><code>af&nbsp;reviews&nbsp;list</code></a> | Read individual reviews for one or more apps. Returns review text, star rating, country, and app version. Filterable by star rating, date range, country, version, and tracking relationship. |
| <a href="#command-reviews-breakdown"><code>af&nbsp;reviews&nbsp;breakdown</code></a> | Aggregate review counts for one or more apps, bucketed by dimension. Returns one count per dimension value, plus a global total across the matched set. |
| <a href="#command-reviews-reply"><code>af&nbsp;reviews&nbsp;reply</code></a> | Post or withdraw a developer response on a specific review. Pass `content` to post; pass `delete: true` to withdraw a previously-posted response. Returns the resulting state (`published`/`pending` for a post, `removed`/`removal_pending` for a withdrawal) along with the submitting account. |

### Keywords

Organic keyword visibility and tracking.

| Command | Description |
| ------- | ----------- |
| <a href="#command-keywords-list"><code>af&nbsp;keywords&nbsp;list</code></a> | List tracked keywords with their opaque IDs. |
| <a href="#command-keywords-rankings"><code>af&nbsp;keywords&nbsp;rankings</code></a> | Check the organic keywords one or more apps rank for, with position, popularity, and competitiveness. |

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

`af apps search <query> [flags]`

Find apps by name or publisher. Returns one row per unified app. Default returns Apple and Google listings; pass `--all-stores` to include other storefronts. To filter apps by estimate values (e.g. apps with >100k downloads last month) use [`explorer list-products`](#command-explorer-list-products). For estimates broken down by time, country, or storefront, use [`metrics query`](#command-metrics-query) with datasets estimates.sales or estimates.revenue.

**Options**

- `<query>` required string. Search query (app name or publisher).
- `--all-stores` boolean, default `false`. Include storefronts beyond Apple and Google: Amazon, Windows, Steam, Roku, LG TV, Samsung TV, and others.
- `--count` integer, default `10`. Number of results to return
- `--page` integer, default `1`. Page number.

---

<a id="apps-tracked"></a>
<a id="command-apps-tracked"></a>
### af apps tracked

`af apps tracked [flags]`

List the apps your Appfigures account tracks.

**Options**

- `--count` integer, default `10`. Number of results to return
- `--page` integer, default `1`. Page number.
- `--q` string. App name to filter by.
- `--filter-apps-by-id` (integer or string)[]. Only include data about specific apps, by product ID or unified app ID. Takes precedence over the other `filterAppsBy*` keys when set. Storefront, source, or type filters are better for app sets that can be described by those criteria.
- `--filter-apps-by-storefront` string[]. Narrow the account's tracked apps to those on these storefronts (e.g. apple:ios, google_play).
- `--filter-apps-by-source` string[]. Narrow the account's tracked apps by tracking relationship.
- `--filter-apps-by-type` string[]. Narrow the account's tracked apps to products of these types.

---

<a id="apps-get"></a>
<a id="command-apps-get"></a>
### af apps get

`af apps get <app-id> [flags]`

Get an app's record: basic metadata (name, developer, etc) and, if the user tracks it, what data they can access. Pass a product ID for one storefront; unified app ID for all storefronts together.

**Options**

- `<app-id>` required integer or string. App identifier. Unified app ID or product ID.
- `--all-stores` boolean, default `false`. For a unified app ID: include member products across all storefronts (Amazon, Steam, Windows, Roku, etc.). When false, `member_products` is restricted to storefronts with app-intelligence coverage (iOS + Google Play). Ignored for product IDs.

---

<a id="explorer-list-products"></a>
<a id="command-explorer-list-products"></a>
### af explorer list-products

`af explorer list-products [flags]`

Read catalog fields for one app or many. Fields referenced by `query` or `sort` come back automatically; pass `--extra-fields` for more. Use `["match","product_id",<id>]` for a single app, or combine filters for population queries (e.g. iOS apps using Firebase with $1M+ US revenue). The 120+ fields span ranks, ratings, download and revenue estimates, SDKs, demographics, and more; query grammar and field list in [`docs get catalog_playbook`](#command-docs-get).

**Options**

- `--query` array. Explorer query in JSON array format to select matching catalog Products. Missing values and `[]` match every Product across every storefront. The full field list and query syntax are documented in [`docs get catalog_playbook`](#command-docs-get).
- `--extra-fields` string[]. Additional fields to include. The full field list is documented in [`docs get catalog_playbook`](#command-docs-get).
- `--sort` string. Explorer field name. The full field list is documented in [`docs get catalog_playbook`](#command-docs-get).
- `--order` string, default `desc`. Sort direction.
- `--count` integer, default `10`. Number of results to return
- `--page` integer, default `1`. Page number.
- `--allow-unscoped-nested` boolean, default `false`. Escape hatch for intentionally broad queries. Bypasses the default block on unscoped nested predicates that usually inflate results.

---

<a id="explorer-aggregate-products"></a>
<a id="command-explorer-aggregate-products"></a>
### af explorer aggregate-products

`af explorer aggregate-products <fields> [flags]`

Aggregate across the full catalog of millions of products across Apple, Google Play, Amazon, and other major stores: counts, averages, min/max, and histograms over any set of matching products. Uses the same query grammar as [`explorer list-products`](#command-explorer-list-products); returns aggregates, not product records. For market sizing, benchmarking, and segment analysis.

**Options**

- `--query` array. Explorer query in JSON array format to select matching catalog Products. Missing values and `[]` match every Product across every storefront. The full field list and query syntax are documented in [`docs get catalog_playbook`](#command-docs-get).
- `<fields>` required string[]. Field+aggregation pairs (e.g. `all_rating/stats`, `storefronts/terms`). Aggregations: `stats`, `terms`, `histogram`, `date_histogram`, `cardinality`. The full field list is documented in [`docs get catalog_playbook`](#command-docs-get).
- `--allow-unscoped-nested` boolean, default `false`. Escape hatch for intentionally broad queries. Bypasses the default block on unscoped nested predicates that usually inflate results.
- `--terms-count` integer, default `20`. Maximum buckets returned for each `terms` aggregation. Other aggregation types ignore it.
- `--date-histogram-interval` string. Bucket granularity for each `date_histogram` aggregation. Other aggregation types ignore it.

---

<a id="explorer-describe-fields"></a>
<a id="command-explorer-describe-fields"></a>
### af explorer describe-fields

`af explorer describe-fields [flags]`

List the catalog fields and the current user's access level for each. Same field set [`explorer list-products`](#command-explorer-list-products) and [`explorer aggregate-products`](#command-explorer-aggregate-products) accept. Pass `--q` to search the catalog by keyword.

**Options**

- `--count` integer, default `50`. Number of results to return
- `--page` integer, default `1`. Page number.
- `--q` string. Filter by `path`, `title`, `description`, `type`.

---

<a id="metrics-query"></a>
<a id="command-metrics-query"></a>
### af metrics query

`af metrics query <dataset> [flags]`

Query any numeric dataset for one or more apps. Optionally grouped by up to two dimensions, returned as a nested partition tree, not app records. Independently filterable by country, device type, and date range. `filterAppsBy*` options narrow the app set (by ID, storefront, source, or type). Some datasets require app ownership; others work for any app (with the right plan).

**Options**

- `<dataset>` required string. Dataset to query (e.g. sales.combined_downloads). The full list is documented in [`docs get numeric_metrics`](#command-docs-get).
- `--group-by` string[]. Dimensions to group by. Max 2: the first slot becomes the outer entity type, the second the inner series.
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
- `--device-types` string[], default `["handheld"]`. Filter response rows by device type.
- `--subtypes` string[], default `["free"]`. Filter response rows by category subtype.
- `--category-ids` integer[]. Filter response rows by category ID.
- `--start` string. Start date (YYYY-MM-DD)
- `--end` string. End date (YYYY-MM-DD, defaults to today)
- `--count` integer, default `10`. Number of results to return
- `--page` integer, default `1`. Page number.

---

<a id="store-top-charts"></a>
<a id="command-store-top-charts"></a>
### af store top-charts

`af store top-charts [flags]`

List the top apps in a category chart for a given country and category, with current positions and day-over-day deltas.

**Options**

- `--country` required string. ISO country code (e.g. US, JP, GB)
- `--category-id` required integer. Store category ID
- `--subtype` string, default `free`. Category subtype (chart variant within the category).
- `--date` string. Snapshot date (YYYY-MM-DD, defaults to current).
- `--count` integer, default `10`. Number of results to return
- `--page` integer, default `1`. Page number.

---

<a id="store-categories"></a>
<a id="command-store-categories"></a>
### af store categories

`af store categories [flags]`

List every store category with its ID. Numeric category IDs required by [`store app-ranks --category-ids`](#command-store-app-ranks) and [`store top-charts --category-id`](#command-store-top-charts) are available here.

**Options**

- `--count` integer, default `50`. Number of results to return
- `--page` integer, default `1`. Page number.
- `--q` string. Filter by `name`.
- `--sort-by` string. Sort order. Default: relevance when `q` is set, otherwise list order.
- `--id` integer[]. Only return these category IDs.
- `--parent-id` integer. Only include subcategories of this parent category (drill-down by id).
- `--storefront` string[]. Only include categories from these storefronts (e.g. `apple:ios`, `google_play`).
- `--device-type` string[]. Only include categories for these device types (e.g. `handheld`, `tablet`).
- `--all` boolean, default `false`. Include non-rank stores (roku, vizio, etc.). These have categories but no rank data.

---

<a id="store-featured"></a>
<a id="command-store-featured"></a>
### af store featured

`af store featured <app-id> [flags]`

List featured and editorial placements for an app or storefront product. Pass `--count=0` for summary stats only.

**Options**

- `<app-id>` required integer or string. App identifier. Unified app ID or product ID.
- `--countries` string[]. Countries to include. Pass multiple countries to compare markets; omit when using `--all-countries`.
- `--all-countries` boolean, default `false`. Include every country by omitting the `--countries` filter.
- `--include-rank-trend` boolean, default `false`. Include per-interval rank_trend for each placement.
- `--sort` string, default `relevance`. Sort placements by relevance or date.
- `--order` string, default `desc`. Sort direction.
- `--start` string. Start date (YYYY-MM-DD)
- `--end` string. End date (YYYY-MM-DD, defaults to today)
- `--count` integer, default `10`. Number of results to return
- `--page` integer, default `1`. Page number.

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
- `--count` integer, default `10`. Number of results to return
- `--page` integer, default `1`. Page number. 1-500.

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

---

<a id="keywords-list"></a>
<a id="command-keywords-list"></a>
### af keywords list

`af keywords list [flags]`

List tracked keywords with their opaque IDs.

**Options**

- `--count` integer, default `10`. Number of results to return
- `--page` integer, default `1`. Page number.
- `--q` string. Filter by `keyword_term`.
- `--sort-by` string. Sort order. Default: relevance when `q` is set, otherwise list order.
- `--include-relationships` boolean, default `false`. Include per-(product, country) tracking detail and sync state on each row. Off by default; adds a nested block per tracked (product, country) pair.

---

<a id="keywords-rankings"></a>
<a id="command-keywords-rankings"></a>
### af keywords rankings

`af keywords rankings [flags]`

Check the organic keywords one or more apps rank for, with position, popularity, and competitiveness.

**Options**

- `--product-ids` integer[]. Product identifiers (numeric, one storefront each).
- `--country` required string. ISO country code (e.g. US, JP, GB)
- `--device-type` string. Device type
- `--count` integer, default `10`. Number of results to return (min 10).
- `--page` integer, default `1`. Page number.

---

<a id="sdks-list"></a>
<a id="command-sdks-list"></a>
### af sdks list

`af sdks list [flags]`

List every known SDK with its id, or search to find a specific one.

**Options**

- `--count` integer, default `50`. Number of results to return
- `--page` integer, default `1`. Page number.
- `--q` string. Filter by `name`, `description`, `tags`.
- `--sort-by` string. Sort order. Default: relevance when `q` is set, otherwise list order.
- `--id` string[]. Only return these SDK ids.
- `--include-inactive` boolean, default `false`. Include inactive SDKs. Rare; most callers want active only.

---

<a id="docs-get"></a>
<a id="command-docs-get"></a>
### af docs get

`af docs get <slug>`

Return a reference doc or guide by slug.

**Options**

- `<slug>` required string. Which reference to return

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

Prints a URL to authorize this CLI. Open it, approve access, then re-run with `--code <code>` using the code shown after approval.

For unattended use (CI, scripts), set APPFIGURES_API_KEY in the environment instead. No browser flow needed.

**Options**

- `--code` string. Authorization code from the OAuth consent page. Second step of `af auth login`: exchanges the code, saves the token, exits.

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
