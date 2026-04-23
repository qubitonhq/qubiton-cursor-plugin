# QubitOn Plugin

Validate addresses, tax IDs, bank accounts, phone numbers, and screen entities
against global sanctions and PEP lists across 200+ countries — directly from
your AI assistant.

## Quick Start

After installing this plugin:

1. Grab your API key from [www.qubiton.com](https://www.qubiton.com) →
   **Dashboard → API Keys** (free plan available).
2. Configure the `qubiton` MCP server in your client:
   - **Cursor:** Settings → Cursor Settings → Tools & MCP → find **qubiton**
     → set the `apikey` header (or `QUBITON_API_KEY` env var) → **Connect**.
   - **Claude Desktop:** Customize → Connectors → find **QubitOn** →
     configure the `apikey` header → **Connect**.
3. Open a chat and say **"setup qubiton"** to verify and run a first test.

## What's Included

| Component | Description |
|-----------|-------------|
| **qubiton-lifecycle** rule | Always-active safety and error-handling rules for QubitOn MCP. Governs authentication, quota awareness, result interpretation, and privacy. |
| **qubiton-setup** skill | Onboarding and connection management. Walks through API key setup, verifies the connection, and suggests a first validation to run. |
| **qubiton-validate** skill | Run validations against any QubitOn capability — address, tax, bank, phone, email, company, sanctions, PEP — and interpret results correctly. Handles single records and batch files. |
| **qubiton-status** skill | Three modes: health check (is the server connected?), quota (plan + usage), coverage (is country X / data type Y supported?). |

## Capabilities

QubitOn MCP provides tools across these data-quality categories:

- **Address validation** — USPS, Royal Mail, BAN, and 200+ country authorities.
- **Tax ID validation** — EU VAT (VIES), UK VAT, India GSTIN, US EIN, Australia
  ABN, and many more. Format + live authority where available.
- **Bank account validation** — IBAN, SWIFT/BIC, US ACH routing, and
  country-specific bank registries (e.g., Brazil BCB, Argentina BCRA).
- **Phone & email validation** — format + deliverability checks.
- **Firmographic enrichment** — company lookups with status, officers,
  registration details, industry codes.
- **Sanctions screening** — OFAC, EU, UN, UK HMT consolidated lists.
- **PEP screening** — Politically Exposed Persons, RCA, and Special Interest.

Exact tool names and signatures are discovered by your MCP client when
connected. Run `qubiton-status` in coverage mode for the live matrix.

## Safety Model

All QubitOn MCP tools are **read-only** — they validate and screen data,
they do not mutate anything in your systems. It is always safe to run them.

Every call counts against your plan quota, so the plugin will estimate and
confirm before running large batches (>10 calls).

## Requirements

- A QubitOn account — free tier available at
  [www.qubiton.com](https://www.qubiton.com).
- An MCP-compatible AI client — Cursor, Claude Desktop, Windsurf, or any
  client supporting the Model Context Protocol.

## Support

- Docs: [www.qubiton.com/docs](https://www.qubiton.com/docs)
- Issues: [github.com/qubitonhq/qubiton-cursor-plugin/issues](https://github.com/qubitonhq/qubiton-cursor-plugin/issues)
- Email: support@qubiton.com
