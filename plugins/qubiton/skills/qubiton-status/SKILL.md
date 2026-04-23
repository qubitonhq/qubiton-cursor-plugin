---
name: qubiton-status
description: Check QubitOn MCP connection health, diagnose tool failures, and find out which validation tools and countries are supported. Use when the user asks "is QubitOn connected", "what QubitOn tools are available", "does QubitOn support country X", "what tax IDs can I validate", or when a tool call fails and the user wants to know why.
---

# QubitOn status

Two modes: **health check** and **coverage**. Plan and quota info are managed
in the QubitOn dashboard, not in the MCP server — redirect the user there
when they ask about usage or billing.

## Mode 1: Health check

User asked whether QubitOn is working, or reported that a tool call failed.

1. List the QubitOn MCP tools available in the current session.
2. Interpret:
   - **No QubitOn tools at all** → "QubitOn MCP isn't connected. Let's run
     the `qubiton-setup` skill." Stop.
   - **Tools available** → run a cheap validation against a known-good input
     to confirm the API key is accepted. Good choices:
     - `ValidateTaxFormat` on a well-formed number (doesn't consume quota
       in the same way as live authority validation)
     - `GetSupportedTaxFormats` (metadata-only)
3. Report:
   - ✅ Connected / ⚠️ Connected but key rejected (401) / ❌ Not connected
   - Number of QubitOn tools available in the session
   - Endpoint: `https://mcp.qubiton.com/mcp`

### Error code cheat sheet

When diagnosing a failed call, match the error status:

| Status | Meaning | What to do |
|--------|---------|-----------|
| `401` | API key missing or invalid | Re-run `qubiton-setup` Step 2 |
| `403` | Tool not included in the user's plan | Direct them to [qubiton.com/pricing](https://www.qubiton.com/pricing) |
| `404` | Country or variant not supported for that tool | Run Mode 2 (coverage) below |
| `429` | Rate limit or quota exhausted | Check the `Retry-After` header; for quota, direct them to the [Dashboard](https://www.qubiton.com) → Usage |
| `5xx` | Transient QubitOn service error | Retry once with backoff; if persistent, link [status.qubiton.com](https://status.qubiton.com) |

## Mode 2: Coverage

User asked whether a specific country, data type, or tax variant is supported.

### Tax coverage — use the MCP server

QubitOn exposes a dedicated tool for tax coverage: **`GetSupportedTaxFormats`**.
Call it and filter the response:

- By country (ISO2) — show all tax types that country supports.
- By tax type — show all countries that have that tax type.
- Distinguish **format-only** vs **live authority-backed** validation in the
  output; only some countries have live authority connections (e.g., EU VIES,
  UK HMRC, India GSTIN).

### Address / bank / sanctions / PEP / other — use the portal

There is no introspection tool for non-tax coverage. Instead:

- Address validation: "Address validation is available for 200+ countries
  via country-specific authorities (USPS, Royal Mail, BAN, and more). If a
  specific country returns `404`, it's likely coverage-gapped — check
  [qubiton.com/coverage](https://www.qubiton.com/coverage) for the current
  matrix."
- Bank account: "IBAN countries are universally supported. Non-IBAN
  countries vary — the US (ACH routing), UK (sort code + account), and
  many APAC / LATAM countries have live validation."
- Sanctions screening: "Screens against OFAC, EU, UN, UK HMT, and other
  official lists via `ProhibitedLookup`."
- PEP screening: "Global PEP coverage via `LookupPep`, including relatives
  and close associates."

Don't invent coverage claims — when you don't know, tell the user to check
the dashboard or try the call and interpret the response.

## Plan / quota / billing

There is **no MCP tool** for plan or quota info. When the user asks:

- "What's my QubitOn plan?"
- "How many calls have I used?"
- "When does my quota reset?"

Direct them to the **QubitOn Dashboard** at
[www.qubiton.com](https://www.qubiton.com) → **Billing** / **Usage**. The
dashboard is the source of truth for billing state; surfacing it via MCP
would leak plan data into the agent context unnecessarily.
