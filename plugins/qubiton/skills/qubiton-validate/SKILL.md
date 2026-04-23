---
name: qubiton-validate
description: Validate individual records or batch files against QubitOn — addresses, tax IDs, bank accounts, phone numbers, emails — and interpret the results correctly. Use when the user asks to "validate X", "check this VAT / GSTIN / EIN / IBAN", "verify this address / bank account / phone", or hands over a spreadsheet/CSV of vendor records to clean.
---

# QubitOn validate

Run validations against QubitOn MCP and surface the results in a form the
user can act on.

## Step 1: Classify the input and pick the right tool

QubitOn MCP exposes 43 tools. Match the user's input to the tool below
by task. Tool names are case-sensitive.

### Address, phone, email

| Task | Tool |
|------|------|
| Validate & standardize a postal address | `ValidateAddress` |
| Validate a phone number | `ValidatePhone` |
| Validate an email address | `ValidateEmail` |

### Tax identifiers

| Task | Tool |
|------|------|
| Fast offline format + checksum check | `ValidateTaxFormat` |
| Full validation against the tax authority (VIES, HMRC, GSTIN, …) | `ValidateTax` |
| Detect which tax type a number is (ambiguous country/type) | `DetectTaxFormat` |
| List the supported tax types per country | `GetSupportedTaxFormats` |

### Bank accounts

| Task | Tool |
|------|------|
| Validate a bank account (IBAN or country-specific routing + account) | `ValidateBankAccount` |
| Validate a SWIFT / BIC identifier | `ValidateBankIdentifier` |

### Business identity & registration

| Task | Tool |
|------|------|
| Validate a business registration with the country's registrar | `ValidateBusinessRegistration` |
| Look up company hierarchy (parent / subsidiaries) | `LookupHierarchy` |
| Look up full corporate hierarchy tree | `LookupCorporateHierarchy` |
| Look up beneficial ownership (UBOs) | `LookupBeneficialOwnership` |
| DUNS number lookup | `LookupDunsNumber` |
| Business classification (NAICS / SIC / industry codes) | `LookupBusinessClassification` |
| Supplier profile validation | `ValidateSupplierProfile` |
| SAP Ariba supplier profile lookup | `LookupSupplierAribaProfile` |

### Sanctions, PEP, and risk screening

| Task | Tool |
|------|------|
| Screen entity or person against sanctions (OFAC / EU / UN / UK HMT / …) | `ProhibitedLookup` |
| PEP / RCA / Special Interest Person lookup | `LookupPep` |
| Aggregate entity-level risk scoring | `AssessEntityRisk` |
| ESG score | `ValidateEsgScore` |
| Disqualified director (UK Companies House, etc.) | `ValidateDisqualifiedDirector` |
| EPA prosecution history — lookup | `LookupEpaProsecution` |
| EPA prosecution history — validate match | `ValidateEpaProsecution` |

### Credit, bankruptcy, failure risk

| Task | Tool |
|------|------|
| Credit score | `ValidateCreditScoreLookup` |
| Full credit analysis lookup | `LookupCreditAnalysis` |
| Bankruptcy check | `ValidateBankruptcyLookup` |
| Fail rate / business failure prediction | `ValidateFailRateLookup` |

### Industry-specific

| Task | Tool |
|------|------|
| US healthcare provider NPI | `ValidateNpi` |
| Medpass provider registry | `ValidateMedpass` |
| India identity (Aadhaar / PAN / GSTIN / etc.) | `ValidateInIdentity` |
| US DOT motor carrier | `LookupDotMotorCarrier` |
| Healthcare provider exclusion (OIG / SAM / state lists) — lookup | `LookupProviderExclusion` |
| Healthcare provider exclusion — validate | `ValidateProviderExclusion` |
| Validate a professional certification | `ValidateCertification` |
| Look up professional certifications | `LookupCertification` |

### Finance & commerce

| Task | Tool |
|------|------|
| Analyze payment terms (Net 30 / 2/10 Net 30 / …) | `AnalyzePaymentTerms` |
| Current exchange rates | `LookupExchangeRates` |
| Peppol network participant ID | `ValidatePeppolId` |
| Peppol supported schemes list | `GetPeppolSchemes` |

### Fraud & data quality

| Task | Tool |
|------|------|
| IP address quality / fraud scoring | `CheckIpQuality` |
| Domain reputation / deliverability report | `DomainReport` |
| Gender inference from a first name | `IdentifyGender` |

If you're unsure which tool to call, list the tools in the session and
pick by name — don't guess at a tool name that doesn't exist. This list
may drift as QubitOn adds tools; the live list in your session is
authoritative.

## Step 2: Gather the required fields

Different validations need different fields. If the user gave you bare
data, ask one short clarifying question before calling — don't burn quota
on a guess.

- **`ValidateAddress`**: at minimum address line + country. Postal code
  strongly improves match rate.
- **`ValidateTax`**: tax ID **and** country ISO2. Same digits mean
  different things in different countries. Use `DetectTaxFormat` first if
  the user didn't give you a country.
- **`ValidateBankAccount`**: for IBAN countries, the IBAN alone is
  sufficient. Otherwise: country + routing/sort/BSB/IFSC + account number
  (and sometimes bank name).
- **`ProhibitedLookup` / `LookupPep`**: name + ideally country,
  registration number (for companies), or date of birth (for individuals).
  More fields = higher match confidence.

## Step 3: Run the validation

For single records, call the tool directly and report.

For batches:

1. Estimate the call count and tell the user (each QubitOn call consumes
   plan quota).
2. Ask for confirmation if it's >10 calls: "This will use ~N QubitOn API
   calls from your quota. Proceed?"
3. Run sequentially, not in parallel — some QubitOn capabilities are
   rate-limited per-second.
4. For anything over 10 records, stream results back as you go so the user
   sees progress.

## Step 4: Interpret the result

Always surface both the **status** and the **canonical/standardized form**
when one is returned.

### `ValidateAddress`

- Report the **match level** (premise, street, locality, admin area,
  country) — not just valid/invalid.
- Show the **standardized components** (cleaned casing, corrected ZIP+4,
  etc.).
- Flag any **change codes** (the input was corrected to match a valid
  record — e.g., ZIP corrected, state abbreviation added).

### `ValidateTaxFormat` / `ValidateTax`

- `ValidateTaxFormat` is a fast regex + checksum check — returns
  "format-valid" or not.
- `ValidateTax` additionally calls the tax authority where supported and
  returns whether the number is **live/active** along with the registered
  company name. Distinguish "format valid" from "authority confirmed".
- Where the authority returns a company name (EU VIES, UK HMRC, India
  GSTIN), show it — it's the most actionable output.

### `ValidateBankAccount` / `ValidateBankIdentifier`

- Surface the **bank name** and **branch** if returned.
- For IBAN: show the canonical IBAN with standard spacing
  (`DE89 3704 0044 0532 0130 00`).
- For SWIFT/BIC: show the country/city/branch breakdown.

### `ProhibitedLookup` (sanctions)

- **Never** declare a match on a fuzzy score alone.
- Present top candidates with confidence score, matched list (OFAC, EU,
  UN, UK HMT, …), and match reason (name, alias, address, identifier).
- Recommend compliance review for any candidate >80% confidence.
- For clean results, say explicitly: "No matches on OFAC, EU, UN, or UK
  HMT sanctions as of <date>."

### `LookupPep` (politically exposed persons)

- Return matches with match type: PEP, RCA (relatives and close
  associates), or Special Interest Person.
- Surface the political position / jurisdiction / date range.
- Same "don't declare a match on fuzzy score alone" rule applies.

### `LookupHierarchy` / `LookupDunsNumber` (enrichment)

- Return the fields the user actually asked for. Don't dump the whole
  record unless they asked for "everything".
- Common fields: legal name, registered address, status (active /
  dissolved), registration number, incorporation date, officers, industry
  codes, parent / ultimate parent.

## Step 5: Report clearly

For single validations: a short structured summary.

For batches: a table with input → status → canonical form → notes, followed
by counts of pass / partial / fail / error.

Always call out records that failed for **reasons other than invalid data**
— quota exhausted, country not supported, service unavailable — so the
user can retry those separately.
