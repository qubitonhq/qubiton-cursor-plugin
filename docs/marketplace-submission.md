# Cursor marketplace submission

**Submission channel:** email `kniparko@anysphere.com` (or the Cursor Slack
community) with the repo link, per
[cursor/plugin-template](https://github.com/cursor/plugin-template#submission).

---

## Email draft

**To:** `kniparko@anysphere.com`
**Subject:** Plugin submission — QubitOn (MCP validation & enrichment, 43 tools)

Hi,

Submitting the official **QubitOn** plugin for the Cursor marketplace.

- **Repo:** https://github.com/qubitonhq/qubiton-cursor-plugin
- **Plugin manifest:** [`plugins/qubiton/.cursor-plugin/plugin.json`](https://github.com/qubitonhq/qubiton-cursor-plugin/blob/main/plugins/qubiton/.cursor-plugin/plugin.json)
- **MCP server:** `https://mcp.qubiton.com/mcp` (streamable HTTP, `apikey` header auth)
- **License:** MIT
- **Validator:** passes `cursor/plugin-template` `validate-template.mjs`
  (CI on [every push/PR](https://github.com/qubitonhq/qubiton-cursor-plugin/actions/workflows/validate.yml))

### What it does

QubitOn is a remote MCP server that gives AI assistants access to global
business-data validation and enrichment: addresses across 200+ countries,
tax IDs across 193 countries, bank accounts (IBAN + country-specific),
phone, email, firmographic enrichment, and sanctions/PEP screening
against OFAC, EU, UN, and UK HMT. 43 tools exposed via MCP — all named
verbatim in the `qubiton-validate` skill.

### What ships in the plugin

- `qubiton-lifecycle` — always-active rule for auth, quota, error handling,
  and result interpretation.
- `qubiton-setup` — onboarding / API-key configuration (covers both the
  `${QUBITON_API_KEY}` env-var path and the direct-header fallback in
  Cursor's MCP settings UI).
- `qubiton-validate` — tool-selection lookup covering all 43 live MCP
  tools grouped by task (address/phone/email, tax, bank, business
  identity, sanctions+PEP+risk, credit+failure, industry-specific,
  finance+commerce, fraud+data-quality).
- `qubiton-status` — health check + coverage diagnosis (plan / quota
  explicitly deferred to the QubitOn Dashboard, since there's no MCP
  tool for it).

### User setup

1. Sign up at [www.qubiton.com](https://www.qubiton.com) (free tier available).
2. Copy the API key from Dashboard → API Keys.
3. Install the plugin in Cursor.
4. Set `QUBITON_API_KEY` (env var) or paste the key directly into the
   `apikey` header in Cursor's MCP settings.
5. Ask in chat: "setup qubiton" — the `qubiton-setup` skill runs a
   verification call and suggests first validations.

Happy to answer questions or adjust anything for marketplace fit.

Thanks,
Akhilesh Agarwal
QubitOn Team — aagarwal@apexanalytix.com

---

## Marketplace listing text (short form, ~200 chars)

> Validate addresses, tax IDs, bank accounts, phones, and emails across
> 200+ countries. Screen entities against OFAC, EU, UN, UK HMT sanctions
> and PEP lists. 43 tools via MCP.

## Marketplace listing text (long form, ~1000 chars)

> **QubitOn** turns your AI assistant into a global data-quality and
> compliance tool. 43 MCP tools let you validate and enrich business
> data through natural language.
>
> **What you can do:**
>
> - Validate addresses against USPS, Royal Mail, BAN, and 200+ country
>   postal authorities — with standardized output and change codes.
> - Validate tax IDs (EU VAT / VIES, UK HMRC, India GSTIN, US EIN,
>   Australia ABN, and many more) with live authority checks —
>   distinguish format-valid from authority-confirmed.
> - Validate bank accounts (IBAN, SWIFT/BIC, ACH, country-specific
>   routing + account) and surface canonical bank name, branch, and IBAN
>   spacing.
> - Screen entities and people against OFAC, EU, UN, UK HMT sanctions
>   lists and global PEP / RCA / Special Interest registries — with
>   confidence scoring.
> - Enrich companies with firmographic data (DUNS, NAICS, hierarchy,
>   beneficial ownership, status, officers).
> - Industry-specific validation: healthcare NPI, India identity, motor
>   carrier, ESG, credit, bankruptcy, fail-rate, Peppol, payment terms.
>
> Free tier available. Paid plans add higher quota and gated
> capabilities.

## Suggested screenshots / demo

Cursor marketplace listings usually want 2–4 screenshots or a short
demo clip. Recommended captures:

1. **Setup:** fresh chat, user types "setup qubiton", the plugin walks
   through API key config and runs a first validation (e.g.
   `ValidateTaxFormat DE136695976`). Shows the skill in action.
2. **Multi-tool validation:** user pastes a vendor record (name +
   address + VAT + IBAN) and asks "validate this vendor". The chat
   shows parallel tool calls, then a summary table with standardized
   addresses, VAT authority confirmation, IBAN canonical form, and
   sanctions clear.
3. **Sanctions screening:** user types "screen ACME Corp against
   OFAC", gets back match candidates with confidence scores and
   matched-list attribution (OFAC / EU / UN / UK HMT).
4. **Batch validation:** user drops a CSV of suppliers, chat confirms
   "~50 API calls, proceed?", shows progress, returns a results table.

A 30–60 second screen-recording of scenario #2 or #4 makes the best
marketplace hero media.

## Topics / tags (already set on the repo)

`cursor`, `cursor-plugin`, `mcp`, `model-context-protocol`, `qubiton`,
`data-validation`, `address-validation`, `tax-validation`,
`bank-validation`, `sanctions-screening`, `pep-screening`, `kyb`, `kyc`,
`supplier-data`, `ai-tools`

## Contact

- Repo issues: https://github.com/qubitonhq/qubiton-cursor-plugin/issues
- QubitOn support: support@qubiton.com
- Plugin maintainer: Akhilesh Agarwal — aagarwal@apexanalytix.com
