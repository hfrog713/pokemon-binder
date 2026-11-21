# Pokémon Binder — Website maintenance runbook
Updated: 2026-11-21. Repository: https://github.com/hfrog713/pokemon-binder

## Purpose and live pages
- Binder: https://hfrog713.github.io/pokemon-binder/ (index.html)
- Set and generation completion: https://hfrog713.github.io/pokemon-binder/sets.html
- Collection value and Top 10: https://hfrog713.github.io/pokemon-binder/values.html
- Authoritative master spreadsheet: https://docs.google.com/spreadsheets/d/1yXNt8EkZxKbHeRDEAqz9ze3eezCcZyt3367h2WUmHws/edit
- Public spreadsheet mirror ID: 1Jrw80VgoSuXHK0RVL1nvubvQzTWDPGrHR_KkBsdX26o

## Critical working image-loading implementation — DO NOT REGRESS
The Top 10 displayed broken-image question marks for seven cards until commit 7389e658e3157dd3477aa19b23a9f32340b020b2. The user confirmed that fix worked on their phone.

1. `values.html` loads `./card-artwork.json` using `loadBinderCardImages()`, mapping inventory IDs to verified artwork URLs. This static JSON is built from the master spreadsheet's **Public Card Data**, column P (**Verified card image URL**), keyed by column A (**Card ID**). It had 215 image records when published. Do not change this to a private master-sheet browser request: that caused failed lookups.
2. `cardImageCandidates(c)` prioritizes `CARD_IMAGE_OVERRIDES.get(c.id)` (from manifest), then known exact card image URLs, then other candidate URLs for the same printing. Maintain the exact set/number/variant; never silently substitute artwork for another printing.
3. **Critical fix:** `drawReconciled()` creates images with `onerror="applyImageFallback(this)"` directly in the `<img>` HTML, *before* browser image loading, and `loading="eager"`. Do not replace this with only `querySelectorAll(...).addEventListener("error", ...)` after `innerHTML` insertion: fast/cached failures can happen before the handler is attached. `applyImageFallback(img)` advances through `data-sources` and `data-at`, finally using a placeholder if all candidates fail.
4. When adding/updating card images, update the master inventory's verified image URL **and** regenerate/update `card-artwork.json` for the corresponding inventory ID. Existing manifest IDs may include `P-`, `U-`, and future records. Confirm the URL depicts the exact printing; a syntactically valid URL does not prove the image is accessible or correct.
5. The binder's `index.html` also uses `CARD_ART_BY_ID`, `VERIFIED_ARTWORK_FALLBACKS`, `applyVerifiedArtwork`, `refreshValuations()` and `hydrateCatalogImages()`. The binder may use the public mirror's `Card Values` feed; do not assume the private master spreadsheet has a tab of the same name.
6. The seven user-confirmed problem cards were Pikachu ex (P-0013), Gloom (P-0022), Abra (P-0032), Venusaur (P-0002), Dark Charmeleon (P-0149), Dragonair (P-0068), and Lapras (P-0062). All displayed after the inline-handler fix.

## Website data responsibilities
- `rankings.json`: raw and PSA 10 Top 10 lists and coverage. Prices are estimates with source/date, not guaranteed sale prices. Rankings may include different cards for raw vs PSA.
- `species-completion.json`: owned species, National Pokédex denominator, generation completion. Physical duplicates and alternative printings **must not** inflate distinct species.
- `set-ownership.json`: expansion checklist and quantities; count unique owned collector numbers separately from physical copies.
- `set-catalog.json`: set catalog used by sets page.
- `pulls-2026-11-21.json` and related logs: physical card provenance.
- `card-artwork.json`: published verified image URL manifest.
- `index.html`, `sets.html`, `values.html`: public display pages. Check all three when changing data sources or shared UI.

## Repeatable update process
1. Read current master spreadsheet, relevant GitHub files and latest owner confirmations. Do not rely on earlier chat summary when source is available.
2. Reconcile by **physical inventory ID**, printed name, set, collector number, finish/variant, condition, quantity, and location. Do not silently invent unverified details. Keep the owner's correction authoritative where explicitly confirmed.
3. Update spreadsheet holdings and checklists. Recalculate unique species, set numbers/percentages, physical copies, and valuation independently; avoid double counting.
4. Update relevant GitHub JSON and HTML only after inspecting current file contents and SHA. Use atomic updates with correct current SHA; do not overwrite unrelated changes.
5. If artwork changed, regenerate `card-artwork.json` from verified source records and retain the **inline `onerror` image fallback** in Top 10.
6. Check valid HTML structure (no code after `</html>`, balanced script tags), valid JSON, matching inventory IDs, complete rankings, exact card images, and synchronized totals. Do not say the live site is tested unless browser rendering was actually checked.
7. Commit and report changed paths, commit IDs, what was and was not verified. Share a cache-busting URL (e.g. `values.html?artfix=VERSION#top-cards`) for mobile testing after GitHub Pages publishes.

## Collection handling and current physical storage
The owner confirmed **there are no longer any cards behind other binder cards**. All previously hidden cards have been removed into a **shoe box**, and the owner says they have **already all been confirmed**. Do not keep requesting hidden-card verification. Keep physical storage/location accurate without assuming the shoe-box cards are in binder pockets.

## Guardrails
- Dark Charmeleon is **Unlimited, not 1st Edition**.
- Do not alter National Pokédex species count for a second printing of a species.
- Distinguish market value of an ungraded card from hypothetical PSA 10 value.
- Do not assume all cards are mint, or that a catalog image verifies a physical card's finish.
- Use a consistent public-image strategy across mobile and desktop.
