# Changelog

All notable changes to the QubitOn Cursor plugin are tracked here. Format
follows [Keep a Changelog](https://keepachangelog.com/en/1.1.0/); versions
follow [semver](https://semver.org/).

## [1.0.0] — 2026-04-23

### Added

- Initial `qubiton` plugin for the Cursor marketplace.
- `qubiton-lifecycle.mdc` — always-active safety rule covering
  authentication, quota awareness, read-only safety model, error code
  handling, and result interpretation for each QubitOn data type.
- `qubiton-setup` skill — onboarding flow: pitch, API key setup
  (environment-variable and direct-header paths), connection
  verification with real tool calls.
- `qubiton-validate` skill — tool-selection lookup for all 43 live MCP
  tools by input type (address, tax, bank, phone, email, business
  identity, sanctions, PEP, risk, credit, industry-specific, finance,
  fraud), required fields per tool, batch flow with quota guarding,
  result interpretation per data type.
- `qubiton-status` skill — health check, coverage diagnosis via
  `GetSupportedTaxFormats`, HTTP error code cheat sheet; plan / quota
  questions redirected to the QubitOn Dashboard (no MCP tool for that).
- `mcp.json` — points at `https://mcp.qubiton.com/mcp` (streamable
  HTTP) with `apikey` header auth sourced from `${QUBITON_API_KEY}`.
- CI: `.github/workflows/validate.yml` runs the official
  `cursor/plugin-template` validator plus a JSON lint on every push and
  PR.
- MIT license, contributor guide, README.

[1.0.0]: https://github.com/qubitonhq/qubiton-cursor-plugin/releases/tag/v1.0.0
