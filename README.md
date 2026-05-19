# harmon-customs-data

Open-source customs reference data, exported daily from the Harmon
platform's production seed files. Distributed under the MIT License.

## What's in here

| File | What | Why it's distinctive |
|------|------|----------------------|
| `de-minimis.json` | Per-country de-minimis duty/VAT thresholds | Every row carries `sourceUrl` + `lastVerifiedAt` |
| `vat-rates.json` | Standard, reduced, zero VAT rates per country (with HS-prefix overrides) | Sources cited per row |
| `fta-agreements.json` | Free-trade agreement registry | Effective dates + signatories |
| `fta-preferential-rates.json` | Per-FTA preferential duty rates by HS prefix | Rules-of-origin annotations |
| `additional-duties.json` | Section 301, Section 232, IEEPA, etc. | Source tag + effective-from |
| `add-cvd.json` | Anti-dumping + countervailing duty rows | Per-source provenance |
| `restricted-goods.json` | CITES, sanctions, prohibited items | Authority + rule citation |
| `country-sanctions.json` | Country-level sanctions (OFAC, EU, UK, etc.) | Authority + last-verified |
| `excise-duties.json` | Per-country excise duties (alcohol/tobacco/fuel) | National revenue authority sources |
| `eccn.json` | US Export Control Classification Numbers | BIS reference |
| `hazmat.json` | UN dangerous-goods identifiers | UN ECE source |
| `preference-programs.json` | GSP, AGOA, EBA, etc. | Programme authority |
| `tariff-rate-quotas.json` | TRQ in-quota / out-of-quota tables | Source per quota |
| `india-gst-states.json` | India GST rates per state (CGST + SGST + IGST + UTGST) | CBIC + state revenue authorities |
| `us-sales-tax.json` | US state + metro sales-tax rates | State revenue authority sources |
| `canary-baseline.json` | Landed-cost smoke-test scenarios (drift detection) | Frozen expected outputs |

## What makes this different from Zonos / Avalara feeds

Every row has three fields these closed feeds don't expose:

- `sourceUrl` — non-null pointer to the government authority that
  publishes the rule (CBP, HMRC, EU TARIC, gov.au, etc.).
- `lastVerifiedAt` — UTC timestamp of the most recent human or canary
  confirmation against the `sourceUrl`.
- `effectiveFrom` (where applicable) — when the rate began applying.

Drift-detection telemetry, governance procedures, and the live
classifier remain Harmon-proprietary. This repo is the **reference
data**, not the engine.

## Provenance contract

Every row in every file is enforced (in the producing repo's CI) to
carry the three provenance fields. See
[`docs/customs-engineering-principles.md`](https://github.com/with-harmon/harmon-platform/blob/main/docs/customs-engineering-principles.md)
invariant I-1 in the Harmon platform repository.

## Updating cadence

Built daily via GitHub Actions in the producing repo
(`with-harmon/harmon-platform`). Each push to `main` regenerates
`index.json` with row counts + SHA-256 checksums per file.

## License

MIT. Use it. Improve it. Open an issue if you spot a stale rate.

## Reporting issues

Open issues against `with-harmon/harmon-platform` (the producing repo).
This repo is built output and PRs should target the source.
