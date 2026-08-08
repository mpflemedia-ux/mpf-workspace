# Maybank Parser Fix (Δ RM2,573.10)

## Root cause (from REV-DEBUG logs)
- pdf.js extracts the same dated amount lines multiple times (same running balance e.g. `18,692.62`)
- Parser treated each repeat as a new transaction
- 5× RM324.85 pairs + 1× RM948.85 pair = exactly RM2,573.10 on both IN and OUT

## Fix applied in local build
1. Fingerprint de-dupe: `date|type|amount|runningBalance` before push
2. Skip pure `REV 2025xxx` continuation lines early
3. Stronger footerContentRe to stop boilerplate leaking into desc
4. Removed TEMP-DEBUG console spam

## Deploy status
Full `index.html` is 1.05MB — tool channel cannot reliably push that size.

**To apply:** Replace the entire `function _brParseMaybank(lines) { ... }` in index.html with the version in the agent artifacts (`brParseMaybank_FIXED.js`).

Or ask the agent to provide the function text for paste into GitHub editor.
