---
name: qubiton-setup
description: Set up QubitOn MCP and start using it. Introduces what QubitOn can validate and enrich, walks through API key setup and authentication, verifies the connection, and suggests a first validation to run. Use when getting started, troubleshooting connection issues, or when the user asks "what can I do with QubitOn", "how do I set up QubitOn", "show me how the QubitOn plugin works", or "what is QubitOn MCP".
---

# QubitOn setup

Introduce QubitOn MCP, get the user authenticated, verify the connection, and
suggest a first validation to run.

## Step 1: Introduction

Start with a short pitch so the user understands what QubitOn unlocks:

> "QubitOn MCP connects your AI to global vendor and supplier data validation —
> addresses (USPS, Royal Mail, BAN, and 200+ country authorities), tax IDs
> (VAT, GSTIN, EIN, and many more), bank accounts (IBAN, SWIFT, ACH, and
> country-specific routing), phone and email validation, firmographic
> enrichment, and sanctions/PEP screening against OFAC, EU, UN, and UK HMT
> lists. Ask in plain language — 'validate this address', 'check this VAT
> number', 'screen this company against sanctions' — and I'll run it."

Then check connection.

### Check connection

Attempt to list available QubitOn tools. Based on what you find:

- **Tools are available**: The user is already connected. Give a shorter
  intro — "You've got QubitOn MCP connected. Let me verify your setup and
  suggest a first test." — then skip to Step 3.
- **No QubitOn tools available**: The server is installed but needs
  authentication. Proceed to Step 2.

## Step 2: Authentication

QubitOn MCP uses an API key sent in the `apikey` header.

1. **Get an API key.** Direct the user to
   [www.qubiton.com](https://www.qubiton.com):
   - If they don't have an account: **Get Started** → sign up → verify email.
     A free plan with limited quota is available.
   - Once signed in: **Dashboard → API Keys** → copy the key
     (prefix `svm_...`).

2. **Configure the MCP client.** The plugin's `mcp.json` references a
   `QUBITON_API_KEY` environment variable. Two paths work:

   **Path A — environment variable (preferred):**
   - Set `QUBITON_API_KEY=<key>` in the shell where you launch Cursor
     (e.g., your shell profile, `.zshenv` / `.bashrc`). Relaunch Cursor.
   - The plugin's `mcp.json` header substitutes `${QUBITON_API_KEY}` at
     load time.

   **Path B — direct header (fallback, always works):**
   - Open **Settings → Cursor Settings → Tools & MCP** → find the
     **qubiton** server.
   - If the `apikey` header shows the literal string `${QUBITON_API_KEY}`
     instead of being expanded (you'll see a 401 on the first call),
     paste the key value directly into the `apikey` header field in
     Cursor's UI.

   In **Claude Desktop** and other MCP clients: find the QubitOn MCP server
   entry and set the `apikey` header similarly — same fallback applies if
   env substitution isn't supported by that client.

3. **Reconnect.** After setting the key, click **Connect** / reconnect the
   server so the new credentials take effect. A successful first tool call
   (like `ValidateTaxFormat`) confirms the key is being sent correctly.

Never ask the user to paste their API key into chat.

## Step 3: Verify & suggest a first run

Once tools are available, confirm the setup works end-to-end by running a
quick non-sensitive validation. Good first calls:

- `ValidateTaxFormat` with a well-formed tax ID (fast; offline format
  check, doesn't hit a live authority)
- `ValidateAddress` with a well-known public address (e.g., "1600
  Amphitheatre Parkway, Mountain View, CA 94043")
- `ProhibitedLookup` on a generic clean name (e.g., the user's own
  company) to demonstrate sanctions screening with a negative result

Report the result with both the status and the canonical/standardized form
if one was returned.

Then hand off with a short list of what else QubitOn can do:

> "You're connected. Some things you can ask me to do:
>
> - Validate an address, tax ID, bank account, phone, or email
> - Enrich a company record with firmographic data
> - Screen an entity or person against OFAC, EU, UN, UK HMT sanctions
> - Check PEP (politically exposed person) status
> - Look up business registration details by country
> - Validate a batch of vendors from a spreadsheet or CSV"

## Troubleshooting

- **401 Unauthorized**: API key missing or invalid. Re-check the key and
  that it's set in the right place.
- **403 Forbidden**: The user's plan doesn't include this capability. Point
  to [qubiton.com/pricing](https://www.qubiton.com/pricing) for plan tiers.
- **429 Too Many Requests**: Rate limit or quota exceeded. The
  `Retry-After` header tells them how long to wait.
- **Server not showing up in client**: The user may need to restart the MCP
  client after adding the plugin.
