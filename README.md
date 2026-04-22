# @appfigures/cli

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

For CI or any non-interactive context, `af auth login` will not run — set `APPFIGURES_API_KEY` in the environment instead.

## Support

- Issues: [github.com/appfigures/cli/issues](https://github.com/appfigures/cli/issues)
- API docs: [docs.appfigures.com](https://docs.appfigures.com)

## Contributing

This package is developed in a private monorepo and published here as a mirror. To report bugs or request features, [open an issue](https://github.com/appfigures/cli/issues).

## License

Apache 2.0
