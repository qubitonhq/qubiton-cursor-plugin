# QubitOn MCP

**Connect your AI to global vendor and supplier data validation with the Model Context Protocol**

Turn your AI assistant into a compliance and data-quality tool. [QubitOn MCP](https://www.qubiton.com) gives your AI direct access to address validation, tax ID validation, bank account validation, sanctions and PEP screening, and firmographic enrichment across 200+ countries — no custom integrations required.

---

## What is QubitOn MCP?

[QubitOn](https://www.qubiton.com) is a standardized way to connect AI assistants to global data-quality and compliance data. It enables your AI to:

- 🏢 Validate addresses across 200+ countries (USPS, Royal Mail, BAN, and country-specific postal authorities)
- 🧾 Validate tax IDs across 193 countries and 242 tax types — with live authority checks (EU VAT / VIES, UK VAT, India GSTIN, US EIN, ABN, …)
- 🏦 Validate bank accounts (IBAN, SWIFT, ACH, and country-specific registries)
- 📞 Validate phone numbers and email addresses
- 🔎 Enrich company records with firmographic, registration, and officer data
- 🚨 Screen entities and persons against OFAC, EU, UN, UK HMT sanctions
- 🏛️ Flag Politically Exposed Persons (PEP), relatives, and close associates
- 🌍 Cover 200+ countries through a single MCP endpoint

All through natural language — just describe what you need validated.

---

## Key Features

- **200+ Countries** — Global coverage for address, tax, bank, sanctions
- **Live Authority Validation** — Not just format checks; real calls to VIES, HMRC, GSTIN, USPS CASS, and more
- **Sanctions & PEP** — OFAC, EU, UN, UK HMT, and Dow Jones-powered PEP screening
- **Natural Language** — No manual API wiring or field mapping
- **Secure by Default** — API-key auth, per-plan quotas, full audit trail
- **Multiple Client Support** — Works with Cursor, Claude Desktop, Windsurf, and any MCP-compatible client

---

## Getting Started

**1. Get your API key**

Sign up at [www.qubiton.com](https://www.qubiton.com) and copy your API key from **Dashboard → API Keys**. A free tier with limited quota is available.

**2. Install this plugin**

In Cursor (Settings → Cursor Settings → Plugins): search for **QubitOn** in the marketplace and install.

**3. Connect the MCP server**

After installing, in **Settings → Cursor Settings → Tools & MCP**, find the `qubiton` server and set the `apikey` header to your key (or set `QUBITON_API_KEY` in your environment).

**4. Try it**

Open a chat and ask:

- *"Validate the address 1600 Amphitheatre Parkway, Mountain View, CA 94043"*
- *"Check if VAT number DE136695976 is valid"*
- *"Screen ACME Corp against OFAC sanctions"*
- *"Enrich Google LLC with firmographic data"*

---

## What's in this repository

This is a multi-plugin Cursor marketplace repository. Currently it ships one plugin:

| Plugin | Path | Description |
|--------|------|-------------|
| **qubiton** | [`plugins/qubiton/`](./plugins/qubiton) | Validation, enrichment, and screening via QubitOn MCP |

See [`plugins/qubiton/README.md`](./plugins/qubiton/README.md) for plugin-specific details and the included rules/skills.

---

## Configuration

The plugin connects to the QubitOn MCP server at `https://mcp.qubiton.com/mcp` (streamable HTTP) using the `apikey` header for authentication. The MCP config (`mcp.json`) references a `QUBITON_API_KEY` environment variable so you don't have to paste the key into config files:

```json
{
  "mcpServers": {
    "qubiton": {
      "url": "https://mcp.qubiton.com/mcp",
      "headers": {
        "apikey": "${QUBITON_API_KEY}"
      }
    }
  }
}
```

---

## License

MIT — see [LICENSE](./LICENSE).

## Links

- Website: [www.qubiton.com](https://www.qubiton.com)
- Docs: [www.qubiton.com/docs](https://www.qubiton.com/docs)
- Status: [status.qubiton.com](https://status.qubiton.com)
- Support: support@qubiton.com
