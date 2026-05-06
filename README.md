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

## Built for agents

Designed to be driven by LLM agents as much as by humans.

- **JSON on stdout, everything else on stderr.** Every command emits a single JSON value on stdout — `jq`-ready, no interleaved logs, hints, or update notices.
- **Auto-detected agent mode.** When stdout isn't a TTY (piped, redirected, or captured by an agent harness) the CLI switches to non-interactive: no prompts, no spinners, and errors include an agent-directed `NOTE:` hint on stderr to steer the next action.
- **Self-describing, offline.** `af catalog query-docs` and `af metrics dataset-docs` print the full query grammar and dataset catalog without hitting the API — plan queries without round-trips or stale training data.
- **Unattended auth.** Set `APPFIGURES_API_KEY` in the environment; every command runs without a browser or prompt.
- **Raw API escape hatch.** `af api <path>` proxies any endpoint the first-class commands don't cover.

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

## Environment

| Variable             | Purpose                                     |
| -------------------- | ------------------------------------------- |
| `APPFIGURES_API_KEY` | API key; skips interactive auth             |
| `AF_VERBOSE`         | Log HTTP to stderr (same as `-v`)           |
| `NO_COLOR`           | Disable ANSI color                          |
| `NO_UPDATE_NOTIFIER` | Skip the npm-registry update check          |
| `CI`                 | Also skips the update check (any CI system) |

<!-- prettier-ignore-start -->
<!-- BEGIN auto-generated COMMANDS — do not edit; run `pnpm generate-docs` to update -->
## Commands

**`af apps`** — Find and identify apps.

- [`search`](#apps-search) — Find apps by name or publisher. Returns one row per unified app. Default returns Apple and Google listings — pass `--all-stores` to include other storefronts. To filter apps by estimate values (e.g. apps with >100k downloads last month) use [`catalog find`](#catalog-find). For estimates broken down by time, country, or storefront, use [`metrics query`](#metrics-query) with datasets estimates.sales or estimates.revenue.
- [`tracked`](#apps-tracked) — List the apps your Appfigures account tracks.
- [`get`](#apps-get) — Get an app's record — basic metadata (name, developer, etc) and, if the user tracks it, what data they can access. Pass a product ID for one storefront; unified app ID for all storefronts together.

**`af catalog`** — Search and analyze the app catalog — 3M+ apps, 120+ queryable fields.

- [`find`](#catalog-find) — Advanced search across the 3M-app catalog — filter and sort by any of 120+ fields (download/revenue estimates, SDK presence, demographics, ratings, ranks, pricing, release dates, ad activity). Uses a simple array query grammar. Run [`docs get catalog_playbook`](#docs-get) for the full field list or query syntax.
- [`aggregate`](#catalog-aggregate) — Advanced aggregation across the 3M-app catalog — counts, averages, min/max, and histograms over any set of matching apps. Uses the same bespoke JSON query grammar as [`catalog find`](#catalog-find); returns aggregates, not app records. For market sizing, benchmarking, and segment analysis.
- [`fields`](#catalog-fields) — List the catalog fields and the current user's access level for each. Same field set [`catalog find`](#catalog-find) and [`catalog aggregate`](#catalog-aggregate) accept.

**`af metrics`** — Query numeric datasets across dimensions.

- [`query`](#metrics-query) — Query any numeric dataset for one or more apps. Optionally grouped by up to two dimensions, returned as a nested partition tree, not app records. Independently filterable by country, device type, and date range. Use `filterAppsBy*` options to narrow the app set (by ID, storefront, source, or type). Some datasets require app ownership, some work for any app.

**`af store`** — App store presence — listing content, category ranks, top charts, and featured placements.

- [`app-ranks`](#store-app-ranks) — Rank history for one or more apps across countries, device types, subtypes, and categories. Returns time-series positions and day-over-day deltas.
- [`top-charts`](#store-top-charts) — Top apps in a category chart — for a given country and category. Returns ranked entries with current positions and day-over-day deltas.
- [`categories`](#store-categories) — List every store category with its ID. Use to find the numeric category IDs that [`store app-ranks --category-ids`](#store-app-ranks) and [`store top-charts --category-id`](#store-top-charts) require.
- [`featured`](#store-featured) — Featured and editorial placement history for an app.
- [`app-listing`](#store-app-listing) — Full store listing for one storefront — localized text (name, subtitle, description, release notes) plus screenshots, video, categories, monetization, supported devices, country availability, price, file size, and age rating. Takes a numeric product ID (one storefront at a time; a unified app has one product per storefront). One locale per request.

**`af reviews`** — App reviews — read and analyze.

- [`list`](#reviews-list) — Search and filter reviews for one or more apps by star rating, date range, country, language, and app version.
- [`breakdown`](#reviews-breakdown) — Review volume breakdown for one or more apps, by star rating, date, or country.
- [`reply`](#reviews-reply) — Reply to a specific review.

**`af keywords`** — Organic keyword visibility and tracking.

- [`list`](#keywords-list) — List tracked keywords with their opaque IDs.
- [`rankings`](#keywords-rankings) — Current organic keyword rankings for one or more apps. Returns which keywords each app ranks for, with position, popularity, and competitiveness. Works for any app.
- [`results`](#keywords-results) — Apps ranking for a specific keyword in organic search results. Returns the ranked list of apps appearing for that keyword, with keyword popularity and competitiveness scores.
- [`related`](#keywords-related) — Find related and suggested keywords for ASO research.

**`af apple-ads`** — Apple Search Ads intelligence.

- [`keywords`](#apple-ads-keywords) — Paid keyword data for one or more apps — which keywords each app has ad impressions for, with impression share and organic rank.
- [`advertisers`](#apple-ads-advertisers) — Apps advertising on a specific keyword, with impression share, organic rank, first/last seen dates, and lifetime data.

**`af docs`** — Reference docs for composing requests and interpreting responses.

- [`get`](#docs-get) — Returns a reference doc by slug.

**`af api`**

- [`api`](#api) — Make a raw API request. See the API reference at https://docs.appfigures.com.

**`af auth`**

- [`login`](#auth-login) — Sign in to Appfigures
- [`logout`](#auth-logout) — Remove stored credentials
- [`status`](#auth-status) — Show authentication and account status

Run `af <command> --help` for arguments, flags, and examples.
<!-- END auto-generated COMMANDS -->
<!-- prettier-ignore-end -->

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
### `af apps search`

```
af apps search <query> [flags]
```

Find apps by name or publisher. Returns one row per unified app. Default returns Apple and Google listings — pass `--all-stores` to include other storefronts. To filter apps by estimate values (e.g. apps with >100k downloads last month) use [`catalog find`](#catalog-find). For estimates broken down by time, country, or storefront, use [`metrics query`](#metrics-query) with datasets estimates.sales or estimates.revenue.

**Arguments**

- `<query>` — string, **required**. Search query — app name or publisher

**Flags**

- `--all-stores` — boolean, default `false`. Include storefronts beyond Apple and Google — Amazon, Windows, Steam, Roku, LG TV, Samsung TV, and others.
- `--count` — integer, default `10`. Number of results to return
- `--page` — integer, default `1`. Page number (1-indexed)

<a id="apps-tracked"></a>
### `af apps tracked`

```
af apps tracked [flags]
```

List the apps your Appfigures account tracks.

**Flags**

- `--count` — integer, default `10`. Number of results to return
- `--page` — integer, default `1`. Page number (1-indexed)
- `--q` — string. Substring match on app name.
- `--filter-apps-by-id` — (integer or string)[]. Only include data about specific apps, by product ID or unified app ID. Takes precedence over the other `filterAppsBy*` keys when set. If you can describe the apps you want with the other `filterAppsBy*` keys (storefront, source, type), use those instead.
- `--filter-apps-by-storefront` — string[]. Narrow the account's tracked apps to those on these storefronts (e.g. apple:ios, google_play).
- `--filter-apps-by-source` — string[]. Narrow the account's tracked apps by tracking relationship.
- `--filter-apps-by-type` — string[]. Narrow the account's tracked apps to products of these types.

<a id="apps-get"></a>
### `af apps get`

```
af apps get <app-id> [flags]
```

Get an app's record — basic metadata (name, developer, etc) and, if the user tracks it, what data they can access. Pass a product ID for one storefront; unified app ID for all storefronts together.

**Arguments**

- `<app-id>` — integer or string, **required**. App identifier — unified app ID or product ID

**Flags**

- `--all-stores` — boolean, default `false`. For a unified app ID: include member products across all storefronts (Amazon, Steam, Windows, Roku, etc.). When false, member_products is restricted to app-intelligence storefronts (iOS + Google Play). Ignored for product IDs.

<a id="catalog-find"></a>
### `af catalog find`

```
af catalog find [query] [flags]
```

Advanced search across the 3M-app catalog — filter and sort by any of 120+ fields (download/revenue estimates, SDK presence, demographics, ratings, ranks, pricing, release dates, ad activity). Uses a simple array query grammar. Run [`docs get catalog_playbook`](#docs-get) for the full field list or query syntax.

**Arguments**

- `[query]` — array. Explorer query in JSON array format to select matching catalog Products. Omit or pass `[]` to match every Product across every storefront. Use [`docs get catalog_playbook`](#docs-get) for the full field list or query syntax.

**Flags**

- `--fields` — string[]. Explorer field names. Use [`docs get catalog_playbook`](#docs-get) for the full field list.
- `--sort` — string. Explorer field name. Use [`docs get catalog_playbook`](#docs-get) for the full field list.
- `--order` — string. Sort direction
- `--count` — integer, default `10`. Number of results to return
- `--page` — integer, default `1`. Page number (1-indexed)
- `--allow-unscoped-nested` — boolean, default `false`. Escape hatch: only pass true after a query error says the unscoped nested semantics are intentional.

<a id="catalog-aggregate"></a>
### `af catalog aggregate`

```
af catalog aggregate <fields> [flags]
```

Advanced aggregation across the 3M-app catalog — counts, averages, min/max, and histograms over any set of matching apps. Uses the same bespoke JSON query grammar as [`catalog find`](#catalog-find); returns aggregates, not app records. For market sizing, benchmarking, and segment analysis.

**Arguments**

- `<fields>` — string[], **required**. Field+aggregation pairs (e.g. `all_rating/stats`, `storefronts/terms`). Aggregations: `stats`, `terms`, `histogram`, `date_histogram`, `cardinality`. Use [`docs get catalog_playbook`](#docs-get) for the full field list.

**Flags**

- `--query` — array. Explorer query in JSON array format to select matching catalog Products. Omit or pass `[]` to match every Product across every storefront. Use [`docs get catalog_playbook`](#docs-get) for the full field list or query syntax.
- `--allow-unscoped-nested` — boolean, default `false`. Escape hatch: only pass true after a query error says the unscoped nested semantics are intentional.

<a id="catalog-fields"></a>
### `af catalog fields`

```
af catalog fields
```

List the catalog fields and the current user's access level for each. Same field set [`catalog find`](#catalog-find) and [`catalog aggregate`](#catalog-aggregate) accept.

<a id="metrics-query"></a>
### `af metrics query`

```
af metrics query <dataset> [flags]
```

Query any numeric dataset for one or more apps. Optionally grouped by up to two dimensions, returned as a nested partition tree, not app records. Independently filterable by country, device type, and date range. Use `filterAppsBy*` options to narrow the app set (by ID, storefront, source, or type). Some datasets require app ownership, some work for any app.

**Arguments**

- `<dataset>` — string, **required**. Dataset to query (e.g. sales.combined_downloads). Run [`docs get numeric_metrics`](#docs-get) for the full list.

**Flags**

- `--group-by` — string[]. Dimensions to group by. Max 2: the first slot becomes the outer entity type, the second the inner series.
- `--granularity` — string. Time granularity when grouping by date
- `--include-total` — boolean, default `false`. Return a single total alongside the breakdowns. Skips client-side summation for dashboard-style asks.
- `--count` — integer. Cap the number of rows returned. Omit to auto-paginate every page of paginated datasets — set this when you only need a preview and want to avoid pulling the full series.
- `--countries` — string[]. Filter to one or more ISO country codes (e.g. US, JP, GB)
- `--device-type` — string. Device type
- `--all-time` — boolean, default `false`. Opt in to the entire history. Without this flag (and without `start`/`end`), the query defaults to the last 30 days. Mutually exclusive with `start` and `end`.
- `--filter-apps-by-id` — (integer or string)[]. Only include data about specific apps, by product ID or unified app ID. Takes precedence over the other `filterAppsBy*` keys when set. If you can describe the apps you want with the other `filterAppsBy*` keys (storefront, source, type), use those instead.
- `--filter-apps-by-storefront` — string[]. Narrow the account's tracked apps to those on these storefronts (e.g. apple:ios, google_play).
- `--filter-apps-by-source` — string[]. Narrow the account's tracked apps by tracking relationship.
- `--filter-apps-by-type` — string[]. Narrow the account's tracked apps to products of these types.
- `--start` — string. Start date (YYYY-MM-DD)
- `--end` — string. End date (YYYY-MM-DD, defaults to today)

<a id="store-app-ranks"></a>
### `af store app-ranks`

```
af store app-ranks <app-ids> [flags]
```

Rank history for one or more apps across countries, device types, subtypes, and categories. Returns time-series positions and day-over-day deltas.

**Arguments**

- `<app-ids>` — (integer or string)[], **required**. App identifiers (unified app IDs or product IDs)

**Flags**

- `--countries` — string[]. Country codes to query. Defaults to every country with rank coverage.
- `--granularity` — string, default `hourly`. Sampling rate. Hourly gives the freshest data; pass `--granularity=daily` for compact multi-day history.
- `--device-types` — string[], default `["handheld"]`. Filter response rows by device type.
- `--subtypes` — string[], default `["free"]`. Filter response rows by subtype.
- `--category-ids` — integer[]. Filter response rows by category ID.
- `--start` — string. Start date (YYYY-MM-DD)
- `--end` — string. End date (YYYY-MM-DD, defaults to today)
- `--count` — integer, default `10`. Number of results to return
- `--page` — integer, default `1`. Page number (1-indexed)

<a id="store-top-charts"></a>
### `af store top-charts`

```
af store top-charts [flags]
```

Top apps in a category chart — for a given country and category. Returns ranked entries with current positions and day-over-day deltas.

**Flags**

- `--country` — string, **required**. ISO country code (e.g. US, JP, GB)
- `--category-id` — integer, **required**. Store category ID
- `--subtype` — string, default `free`. Which list within the category
- `--date` — string. Snapshot date (YYYY-MM-DD, defaults to current).
- `--count` — integer, default `10`. Number of results to return
- `--page` — integer, default `1`. Page number (1-indexed)

<a id="store-categories"></a>
### `af store categories`

```
af store categories [flags]
```

List every store category with its ID. Use to find the numeric category IDs that [`store app-ranks --category-ids`](#store-app-ranks) and [`store top-charts --category-id`](#store-top-charts) require.

**Flags**

- `--all` — boolean, default `false`. Include non-rank stores (roku, vizio, etc.) — they have categories but no rank data
- `--ids` — integer[]. Only return these category IDs.
- `--q` — string. Substring match on category name.

<a id="store-featured"></a>
### `af store featured`

```
af store featured <app-id> [flags]
```

Featured and editorial placement history for an app.

**Arguments**

- `<app-id>` — integer or string, **required**. App identifier — unified app ID or product ID

**Flags**

- `--start` — string. Start date (YYYY-MM-DD)
- `--end` — string. End date (YYYY-MM-DD, defaults to today)
- `--count` — integer, default `10`. Number of results to return
- `--page` — integer, default `1`. Page number (1-indexed)

<a id="store-app-listing"></a>
### `af store app-listing`

```
af store app-listing <product-id> [flags]
```

Full store listing for one storefront — localized text (name, subtitle, description, release notes) plus screenshots, video, categories, monetization, supported devices, country availability, price, file size, and age rating. Takes a numeric product ID (one storefront at a time; a unified app has one product per storefront). One locale per request.

**Arguments**

- `<product-id>` — integer, **required**. A number representing a product on a single store

**Flags**

- `--language` — string. Locale (e.g. en, ja, zh-Hans) for name, subtitle, description, release notes, and screenshots. Defaults to en; falls back to the first available locale when the requested one has no metadata. The response echoes the resolved language.
- `--device-type` — string, default `handheld`. Relevant to Apple apps. Pick handheld for iPhone-specific metadata, tablet for iPad, desktop for Mac, etc.

<a id="reviews-list"></a>
### `af reviews list`

```
af reviews list [flags]
```

Search and filter reviews for one or more apps by star rating, date range, country, language, and app version.

**Flags**

- `--stars` — number[]. Filter by star rating
- `--language` — string. Filter by language code (e.g. en, ja)
- `--version` — string. Filter by app version
- `--country` — string. ISO country code (e.g. US, JP, GB)
- `--filter-apps-by-id` — (integer or string)[]. Only include data about specific apps, by product ID or unified app ID. Takes precedence over the other `filterAppsBy*` keys when set. If you can describe the apps you want with the other `filterAppsBy*` keys (storefront, source, type), use those instead.
- `--filter-apps-by-storefront` — string[]. Narrow the account's tracked apps to those on these storefronts (e.g. apple:ios, google_play).
- `--filter-apps-by-source` — string[]. Narrow the account's tracked apps by tracking relationship.
- `--filter-apps-by-type` — string[]. Narrow the account's tracked apps to products of these types.
- `--count` — integer, default `10`. Number of results to return
- `--page` — integer, default `1`. Page number (1-indexed)

<a id="reviews-breakdown"></a>
### `af reviews breakdown`

```
af reviews breakdown [flags]
```

Review volume breakdown for one or more apps, by star rating, date, or country.

**Flags**

- `--country` — string. ISO country code (e.g. US, JP, GB)
- `--filter-apps-by-id` — (integer or string)[]. Only include data about specific apps, by product ID or unified app ID. Takes precedence over the other `filterAppsBy*` keys when set. If you can describe the apps you want with the other `filterAppsBy*` keys (storefront, source, type), use those instead.
- `--filter-apps-by-storefront` — string[]. Narrow the account's tracked apps to those on these storefronts (e.g. apple:ios, google_play).
- `--filter-apps-by-source` — string[]. Narrow the account's tracked apps by tracking relationship.
- `--filter-apps-by-type` — string[]. Narrow the account's tracked apps to products of these types.
- `--start` — string. Start date (YYYY-MM-DD)
- `--end` — string. End date (YYYY-MM-DD, defaults to today)

<a id="reviews-reply"></a>
### `af reviews reply`

```
af reviews reply <review-id> [flags]
```

Reply to a specific review.

**Arguments**

- `<review-id>` — string, **required**. Review ID to reply to

**Flags**

- `--body` — string, **required**. Reply text

<a id="keywords-list"></a>
### `af keywords list`

```
af keywords list
```

List tracked keywords with their opaque IDs.

<a id="keywords-rankings"></a>
### `af keywords rankings`

```
af keywords rankings [flags]
```

Current organic keyword rankings for one or more apps. Returns which keywords each app ranks for, with position, popularity, and competitiveness. Works for any app.

**Flags**

- `--product-ids` — integer[]. Product identifiers (numeric, single-store products).
- `--country` — string. ISO country code (e.g. US, JP, GB)
- `--device-type` — string. Device type
- `--count` — integer, default `10`. Number of results to return
- `--page` — integer, default `1`. Page number (1-indexed)

<a id="keywords-results"></a>
### `af keywords results`

```
af keywords results <keyword-name> [flags]
```

Apps ranking for a specific keyword in organic search results. Returns the ranked list of apps appearing for that keyword, with keyword popularity and competitiveness scores.

**Arguments**

- `<keyword-name>` — string, **required**. Keyword to look up

**Flags**

- `--country` — string. ISO country code (e.g. US, JP, GB)
- `--storefront` — string. App store platform — e.g. apple:ios, google_play, amazon_appstore, steam, windows10, apple:mac, apple:tv, apple:imessage (and others the API may add)
- `--device-type` — string. Device type
- `--count` — integer, default `10`. Number of results to return
- `--page` — integer, default `1`. Page number (1-indexed)

<a id="keywords-related"></a>
### `af keywords related`

```
af keywords related <keyword-name> [flags]
```

Find related and suggested keywords for ASO research.

**Arguments**

- `<keyword-name>` — string, **required**. Seed keyword to find related terms for

**Flags**

- `--country` — string. ISO country code (e.g. US, JP, GB)
- `--storefront` — string. App store platform — e.g. apple:ios, google_play, amazon_appstore, steam, windows10, apple:mac, apple:tv, apple:imessage (and others the API may add)
- `--device-type` — string. Device type

<a id="apple-ads-keywords"></a>
### `af apple-ads keywords`

```
af apple-ads keywords <product-ids> [flags]
```

Paid keyword data for one or more apps — which keywords each app has ad impressions for, with impression share and organic rank.

**Arguments**

- `<product-ids>` — integer[], **required**. Product identifiers (numeric, single-store products).

**Flags**

- `--days` — integer, default `180`. Lookback period in days. Common values: 7, 14, 30, 90, 180, 365.
- `--country` — string. ISO country code (e.g. US, JP, GB)
- `--device-type` — string. Device type
- `--count` — integer, default `10`. Number of results to return
- `--page` — integer, default `1`. Page number (1-indexed)

<a id="apple-ads-advertisers"></a>
### `af apple-ads advertisers`

```
af apple-ads advertisers <keyword-name> [flags]
```

Apps advertising on a specific keyword, with impression share, organic rank, first/last seen dates, and lifetime data.

**Arguments**

- `<keyword-name>` — string, **required**. Keyword to look up advertisers for

**Flags**

- `--days` — integer, default `180`. Lookback period in days. Common values: 7, 14, 30, 90, 180, 365.
- `--country` — string. ISO country code (e.g. US, JP, GB)
- `--device-type` — string. Device type
- `--count` — integer, default `10`. Number of results to return
- `--page` — integer, default `1`. Page number (1-indexed)

<a id="docs-get"></a>
### `af docs get`

```
af docs get <slug>
```

Returns a reference doc by slug.

**Arguments**

- `<slug>` — string, **required**. Which reference to return

<a id="api"></a>
### `af api`

```
af api <path> [flags]
```

Make a raw API request. See the API reference at https://docs.appfigures.com.

**Arguments**

- `<path>` — string, **required**. API path (e.g. /users, /products)

**Flags**

- `--method` — string, default `GET`. HTTP method (one of: GET, POST, PUT, PATCH, DELETE, HEAD, OPTIONS)
- `--body` — string. Request body (JSON string)

<a id="auth-login"></a>
### `af auth login`

```
af auth login
```

Sign in to Appfigures

Opens your browser to sign in, then prompts for the code Appfigures
shows after approval. Requires an interactive terminal.

For CI / agents without a user in the loop, set APPFIGURES_API_KEY
in the environment instead — no browser flow needed.

<a id="auth-logout"></a>
### `af auth logout`

```
af auth logout
```

Remove stored credentials

<a id="auth-status"></a>
### `af auth status`

```
af auth status
```

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
