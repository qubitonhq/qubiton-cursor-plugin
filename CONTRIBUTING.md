# Contributing

Thanks for your interest in the QubitOn Cursor plugin.

## Development

### Repo layout

This is a **multi-plugin Cursor marketplace repo** following the
[cursor/plugin-template](https://github.com/cursor/plugin-template) layout:

```text
.cursor-plugin/marketplace.json     — org + plugin registry
plugins/<plugin-name>/
  .cursor-plugin/plugin.json        — plugin manifest
  mcp.json                          — MCP server config
  rules/*.mdc                       — always-active or conditional rules
  skills/<name>/SKILL.md            — task-specific skills
  assets/logo.svg                   — marketplace logo
  README.md                         — user-facing docs
```

Each skill, rule, agent, and command **must** have YAML frontmatter
(`name`, `description` at minimum). The Cursor template validator enforces
this — see **Validate** below.

### Validate locally

```bash
curl -sL -o /tmp/validate-template.mjs \
  https://raw.githubusercontent.com/cursor/plugin-template/main/scripts/validate-template.mjs
node /tmp/validate-template.mjs
```

Expected: `Validation passed.` Warnings about missing `hooks/hooks.json` are
fine (we don't use hooks).

CI runs the same validator plus a JSON lint pass on every push and PR —
see [`.github/workflows/validate.yml`](./.github/workflows/validate.yml).

### Test with Cursor locally

1. Install from a local directory: in Cursor, open the command palette,
   run **"Plugins: Install from folder"**, and point at the plugin
   directory (e.g. `plugins/qubiton/`).
2. Set `QUBITON_API_KEY` in the shell that launches Cursor, then relaunch.
3. Open **Settings → Cursor Settings → Tools & MCP** to verify the
   `qubiton` server connects. A successful call to `ValidateTaxFormat`
   on a well-formed tax ID confirms end-to-end.

## Adding a new plugin

1. Create `plugins/<new-plugin>/.cursor-plugin/plugin.json` with the
   required fields (`name` in lowercase kebab-case, `displayName`,
   `version`, `description`, `author`).
2. Add `mcp.json`, `rules/`, `skills/`, and `assets/logo.svg` as needed.
3. Register the plugin in `.cursor-plugin/marketplace.json`.
4. Run the validator; fix any errors.
5. Open a PR — CI will re-run the validator.

See also: [cursor/plugin-template `docs/add-a-plugin.md`](https://github.com/cursor/plugin-template/blob/main/docs/add-a-plugin.md).

## Reporting issues

- Plugin bugs, wrong tool descriptions, missing tool coverage:
  [open an issue](https://github.com/qubitonhq/qubiton-cursor-plugin/issues).
- QubitOn API service issues or questions about account/quota:
  use support channels at [www.qubiton.com](https://www.qubiton.com)
  or email support@qubiton.com.

## Releasing

This repo uses [semver](https://semver.org/). When changing
`plugins/qubiton/.cursor-plugin/plugin.json`:

- **Patch** (`1.0.x`) — doc tweaks, error message improvements,
  non-behavioral fixes.
- **Minor** (`1.x.0`) — new skill, new rule, new MCP tool coverage,
  backwards-compatible behavioral changes.
- **Major** (`x.0.0`) — breaking changes to the MCP server URL, required
  client configuration, or skill contracts.

Update [`CHANGELOG.md`](./CHANGELOG.md) in the same PR.

## License

By contributing you agree your contributions are licensed under the
repo's [MIT license](./LICENSE).
