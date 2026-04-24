# Changelog

## [1.0.0-beta.1] — 2026-04-23

### Fixed

- **`auth logout`** no longer hangs if the server revoke is slow.

### Docs

- README: npm badge and “Built for agents” section.

## [1.0.0-beta.0] — 2026-04-23

### Added

- **Pagination** — `--count` / `--page` on `apps mine`, `store featured`, `store app-ranks`, `keywords results`.
- **`metrics query --all-time`**.

### Changed

- **Reference commands renamed** under `catalog` and `metrics`.
- **List responses now include pagination info** on `apps mine`, `store featured`, `store app-ranks`, `keywords results`.
- **`metrics query`** output is tidier — top results first, incomplete recent dates are called out, and dates no longer shift with your timezone.
- **`catalog find`** returns more relevant results when you query `custom_meta.*` fields.

### Fixed

- **`metrics query`** with only `--end` no longer inverts the date range.

## [1.0.0-alpha.5] — 2026-04-22

### Added

- **OAuth 2 browser login** — `af auth login` opens your browser; PAT still supported.
- **`store` group** — `store app-ranks`, `store top-charts`, `store categories`, `store featured`, and `store app-listing`.
- **`apps get`** — fetch full details for any app, with `--all-stores` to span every store it appears on.
- **Env var overrides** — `APPFIGURES_API_KEY` (skip the keychain), `APPFIGURES_API_BASE_URL`, `APPFIGURES_OAUTH_CLIENT_ID`, `APPFIGURES_OAUTH_CLIENT_SECRET`, and `AF_VERBOSE=1`.
- **`auth login --code`** — pass the OAuth code directly instead of pasting it into the prompt.
- **`auth status`** shows where your active credential came from and warns when env and keychain conflict.
- **Examples on `af --help`** — common starting points right on the root help screen.

### Changed

- **Renamed `apps info` → `apps get`.**
- **Renamed `catalog rollup` → `catalog aggregate`.**
- **`--device` is now `--device-type`** across `metrics`, `keywords`, `apple-ads`, and `store`.
- **`metrics query`**: dataset is positional, defaults to all your apps over the last 30 days, and accepts `--app-storefronts` to scope by store.
- **`apple-ads keywords`, `keywords rankings`, `reviews list`, `reviews breakdown`** all default to every tracked app when `appIds` is omitted.
- **`auth logout`** revokes the token server-side and confirms whose account it's clearing.
- **`auth login`** refuses to run when you're already authenticated, including via `APPFIGURES_API_KEY`.
- **`-v` / `--verbose`** logs each request as it fires and redacts the `Authorization` header.
- **Unknown flags now error** instead of silently being ignored.
- **JSON output** is compact in non-TTY mode and pretty-printed in your terminal.

### Fixed

- **`metrics schema`** no longer suggests contradictory granularity options.
- **API errors** with non-standard response bodies now forward the server's message instead of swallowing it.
- **401 / invalid-token errors** automatically suggest `af auth login`.
- **Ctrl-C** exits cleanly (POSIX 130); **SIGTERM** exits 143.
- **`APPFIGURES_API_KEY`** is referred to as an environment variable consistently across help and errors.

## [1.0.0-alpha.4] — 2026-04-17

### Changed

- **`apps search`** now uses the unified-apps endpoint and emits pagination hints when more results exist.
- **Polished help output** — mutating commands hidden by default, exit codes tightened.

## [1.0.0-alpha.3] — 2026-04-15

### Changed

- **Clearer auth errors** — HTTP errors now show the server's response body (not just status), and `auth login --help` explains how to get a token.

## [1.0.0-alpha.2] — 2026-04-13

### Fixed

- **Update notifications now fire reliably** — no longer lost to the process-exit race, and show under `--help`/`--version` too.

## [1.0.0-alpha.1] — 2026-04-12

### Breaking

- **`catalog find --sort`**: The leading `-` prefix for descending order is gone. Use `--order desc` instead.

  Before: `af catalog find --sort -all_rating`
  After: `af catalog find --sort all_rating --order desc`

  The old convention hit a citty/util.parseArgs bug: at intermediate subcommand levels, values starting with `-` that contain `_` were split into single-char boolean flags (including `-_`), which set `values._ = true` and destroyed the positionals array — throwing "boolean true is not iterable".

## [1.0.0-alpha.0] — 2026-04-12

Initial alpha release.
