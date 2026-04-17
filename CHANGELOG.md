# Changelog

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
  After:  `af catalog find --sort all_rating --order desc`

  The old convention hit a citty/util.parseArgs bug: at intermediate subcommand levels, values starting with `-` that contain `_` were split into single-char boolean flags (including `-_`), which set `values._ = true` and destroyed the positionals array — throwing "boolean true is not iterable".

## [1.0.0-alpha.0] — 2026-04-12

Initial alpha release.
