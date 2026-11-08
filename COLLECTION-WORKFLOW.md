# Pokémon Binder — repeatable collection intake, valuation, and reconciliation SOP

**Established:** 2026-11-07. **Status:** active. Use these rules across future ChatGPT threads in the same Pokémon Card Collection project.

## Canonical resources

| Purpose | Location |
| --- | --- |
| Master (private, editable) | [Pokémon Collection — Live Master](https://docs.google.com/spreadsheets/d/1yXNt8EkZxKbHeRDEAqz9ze3eezCcZyt3367h2WUmHws/edit) |
| National Pokédex binder (published) | https://hfrog713.github.io/pokemon-binder/ |
| Published set checklists | https://hfrog713.github.io/pokemon-binder/sets.html |
| Published value history | https://hfrog713.github.io/pokemon-binder/values.html |
| Published Google Sheets mirror | [Read-only mirror](https://docs.google.com/spreadsheets/d/1Jrw80VgoSuXHK0RVL1nvubvQzTWDPGrHR_KkBsdX26o/edit) |
| Form slot source | `pocket-map.json` and live site's `index.html` |
| Set catalog source | `set-catalog.json` |

Use the **same** master and repository from all chats. Do not start a second inventory or invent a separate website.

## Source of truth by tab

- **All Cards**: Canonical **identity, physical ownership, quantity, provenance, placement, set and print variant**. Includes historical photo-review/pending rows, some of which refer to the same physical cards; these are **not automatically countable**. Owner Inventory is a historical source-list, **not** the active valuation.
- **Collection Holdings**: One row per countable, independently owned physical card or documented identical-card lot; `Include in value? = Yes`. Holds quantity, condition, latest unit price, price source/date, owner costs if known, formula-based extended value. This is the sole working basis for collection dollar value.
- **Pull Inbox**: Intake and review status for every submitted photo/batch. Keep the batch and photo provenance even after the card is filed.
- **Card Price History**: **Append-only** dated per-card price and quantity events (first price, price refresh, corrections). Never overwrite previous events.
- **Collection Value History**: **Append-only fixed-number** snapshots of all included holdings, including number of IDs and copies and missing-price coverage. Never link historical dollar values to a live formula. Right-hand N:O summary displays current working totals and is *not* the historical record.
- **Set Card Checklist** and **Set Completion**: Set-specific owned and missing collector numbers, including secrets and known variants; these are **currently static snapshots** and need explicit updating after each pull. **Recorded Set Variants** tracks finishes actually reported.
- **Species Owned**, **Pocket Map**, **Virtual Binder**, **Visible Cards**, **Other Visible Cards**: Species, physical form pockets, front cards, and site-facing current data.
- **Collection Workflow**: This SOP's short action checklist inside the master workbook.

## A. New-pulls thread — perform on each newly uploaded batch

1. **Batch and provenance.** Assign `PULL-YYYYMMDD`, with `-02`, `-03` if another batch occurs the same date. Record image references, date, and the user's pack/store/source if given; never infer the cost from retail prices.
2. **Identify before valuing.** For every distinct physical card or confirmed identical-card lot: printed name, National Dex number (if applicable), set name/code, collector number/denominator, rarity, language, exact finish (normal/holo/reverse holo/full art/etc.), edition/promo stamp, copy count, and target binder location. Preserve ambiguous details as unverified and ask for close-up only when necessary.
3. **Deduplicate.** Compare physical card, exact set/collector/finish, source photos, existing `All Cards` IDs, `Collection Holdings` and `Pull Inbox`. The same card photographed again, moved between pockets, or later seen behind a front card is **not a new physical copy**. Distinct identical copies need explicit owner confirmation or separately visible physical evidence. Keep legacy `N-...` photo IDs as reference aliases when they describe an already counted `P-...` card.
4. **Record owned cards once.** Assign a stable unique new ID (suggest `C-YYYYMMDD-001`, never reuse); add the physical card to `All Cards` with status, quantity and source. If it is independently owned, add *one* countable `Collection Holdings` row (or adjust confirmed lot quantity). The extended value formula in each new holdings row is `=IF(Pn="Yes",IF(ISNUMBER(In),ROUND(Fn*In,2),0),0)`, where `n` is its row.
5. **Binder and forms.** Update National Dex species count only if truly new species. A Pokémon's regional/alternate form may have a distinct custom `.1`, `.2` pocket but **does not** increment unique species count. Preserve the physical pocket numbering and confirmed form descriptions. File location must be owner-confirmed; mark unfiled otherwise.
6. **Set checklist.** If expansion is new, create the *full* known numbered + secret checklist, special promos and documented finishes, fill ownership from inventory, add it to `Set Completion`, public mirror and `set-catalog.json` so `sets.html` shows it. If expansion already exists, explicitly update the correct static checklist row's Owned/Qty/Finish/Source IDs and refresh set summary. **Updating All Cards alone will not update existing static checklists.**
7. **Condition and authenticity.** The owner's blanket description is **Mint (owner-reported, ungraded)** and their claim of first printing does *not* imply First Edition, a stamp or a professional grade. Record photos of backs/corners/surface if later provided. Mark genuine/edition uncertainty separately. Dark Charmeleon Team Rocket #32/82 is owner-confirmed **Unlimited**, not 1st Edition.
8. **Unit valuation.** Prefer exact TCGplayer market for the matching printing and ungraded near-mint condition; alternatively use recent *sold* eBay comparables. Record date and source URL. A price supplied by the owner must explicitly say **Owner-provided** and not masquerade as independently verified. For missing ordinary Energy prices, owner's assumption is **$0.01 per copy**, not a sourced market quote. Do not infer actual sale proceeds or grading premium.
9. **Audit history.** For every first price or changed unit value/quantity, append a row to `Card Price History` (price as-of, evidence and event ID). **Then** update current unit value in Holdings and matching `All Cards` / `Visible Cards` source. Do not backdate or rewrite previous event rows.
10. **Snapshot total.** Calculate from `Collection Holdings` rows with Include=Yes: number of unique IDs, sum quantities, quantities without prices, and sum of formula `Extended value $`. Append a **static** row to `Collection Value History`, even if the batch adds a $0 card or only corrections, with event ID, date and coverage notes. Record optional acquisition cost where actually known; do **not** report profit as a precise figure if costs are incomplete.
11. **Publish and verify.** Update current front-card feed, extras feed, site if needed, and the public-read-only mirror. Confirm Sheets readback and site/Pages deployment if files changed. Report what was added, existing IDs matched, new species/forms, each set completion delta, total value/valuation delta, and outstanding ambiguities. No asynchronous or background updates occur between submissions.

## B. Hidden-behind-front-card thread

Use event IDs `HIDDEN-YYYYMMDD`. Photograph individual fronts of cards uncovered behind a displayed Pokémon and specify the **original binder page, row, column and stack order**. Preserve the existing front card and physical placeholder; a hidden card gets its own inventory identity/quantity only when independently owned. Match historical pull/photo records first, then reconcile All Cards and Collection Holdings, finish/checklist variants, price changes and a new value snapshot. Do not assume every hidden card is a new species or a new collection item.

## C. Later valuation review without new pulls

A repricing session is an event (e.g. `PRICE-YYYYMMDD`): match every checked card's printing and finish, record dated sources or user values, **append** the event to Card Price History, update current Holdings and any matching site-facing per-card values, and append one fixed Collection Value History snapshot. The latest number is a *recorded estimate* based on most recent verified/owner-provided checks. It is **not** a continuous price ticker; updates require an explicit chat request or separately configured automation.

## Baseline and exclusions

Opening snapshot **2026-11-07**: **$132.90** estimated ungraded value across **220 countable inventory IDs and 222 reported copies**. Includes two reported copies of **Zinnia's Trust** and two of **Grisham** (provisional counts pending hidden-card/duplicate verification). This corrects the previously quoted **$132.68**, which had erroneously included unowned **Quaxly $0.29** while omitting an additional **$0.51** for the reported second copies. User-reported Mint is **not** a professional grade. Unidentified or overlapping photo IDs (including unverified Energy) and hidden cards not yet inspected are excluded. The historical snapshot will **never be silently rewritten** after new facts; log a dated correction snapshot instead.

**Important:** Although `All Cards` now includes the 42 filed P-0150 through P-0191 rows that were missing, the legacy N-photo entries may still overlap the same physical cards. Do not sum `All Cards!U` to value the collection. The designated valuation truth is `Collection Holdings` with inclusion flags.

## Suggested messages to start future threads

> **New Pulls:** Use the Pokémon collection workflow in the existing master Google Sheet and GitHub repository. Start a new pull batch. Identify/deduplicate every physical card, update All Cards and Collection Holdings, create any new set checklists, log every price and collection-value snapshot, and verify the live site. Here are the new pulls: [photos]

> **Hidden Cards:** Use the same Pokémon collection workflow. These cards came from behind existing front cards. Reconcile each physical card against existing inventory and old photo IDs first, preserve the binder front placement, record independently owned extras, update values/set checklists, and take a dated value snapshot. Here are the pages, pockets, and stack photos: [photos]

## End-of-batch acceptance checklist

- [ ] No new inventory IDs for duplicate photographs or the same previously filed physical card.
- [ ] Every recorded card has source photo, set, collector number, finish, quantity and condition/status or explicit uncertainty.
- [ ] New form-pocket ownership and every affected set's static checklist reconciled.
- [ ] Existing or new valuation row counted once, with a dated source/basis.
- [ ] Price history appended for changes; no previous event overwritten.
- [ ] Static collection-value snapshot appended and verified against active holdings.
- [ ] Website / public mirror verified, and deployment checked if GitHub files changed.
- [ ] User given a compact batch receipt including changes, value total, known limitations, and unresolved requests.


## D. Virtual binder display modes (2026-11-08)

- **Most Valuable (default):** The site reads the public **Display Candidates** feed, which mirrors a formula-derived **Public Display Candidates** tab in the existing master. Only inventory IDs included in **Collection Holdings** qualify, with the recorded ungraded *unit* price. The highest-valued card for each mapped species/form pocket is shown even when physically stored behind another card. Missing prices cannot be asserted as cheaper; ties prefer the existing physical front.
- **Physical Binder:** Always displays the actual front card recorded in **Visible Cards**; no card is physically moved or replaced by switching the view.
- **Forms:** A dedicated custom form pocket (e.g., Mega Rayquaza #384.1) stays separate from the base species pocket. A view toggle never counts additional National Dex species or duplicates.
- **On new pulls or repricing:** Update **All Cards** identity/location and **Collection Holdings** unit price/status as usual. The formula and public mirror refresh candidate values without creating another spreadsheet. Update **Visible Cards** only when the physical front card actually changes.
- Check the live site mode selector and compare a species with two card prices (Eevee is an example) after changes. If the live price feed is unavailable, do not claim a verified most-valuable ranking; the site falls back to recorded front cards.


## E. Top 10 raw and hypothetical PSA 10 website rankings (2026-11-08)

- **Web view:** `values.html#top-cards`; linked from the binder home page. Both rankings sort **one price per distinct physical-card inventory ID**, not copies multiplied by quantity. A duplicate is recorded in the holdings quantity, not automatically a second ranked printing.
- **Raw/Mint-NM source:** the existing master `Collection Holdings` is authoritative. The existing public-read-only spreadsheet now contains the `Top Card Values` tab, one feed row for **each independently countable holding** with `Include in value? = Yes`. Current unit value, ID, exact printing and finish, qty, price date, owner/review basis, image and physical status are mirrored there. The website sorts its raw price column G descending and displays the first ten; it does **not** calculate value from `All Cards`, historical snapshots, or displayed fronts alone.
- **PSA 10 source:** the **same master** now includes `PSA 10 Estimates` (ID, exact printing, variant, hypothetical PSA 10 *unit* estimate, estimate date, source URL and evidence notes). The public `Top Card Values` feed mirrors these values by ID into columns I–L. The website sorts positive I values descending and displays up to ten. All cards are still **owner-reported Mint / ungraded**, with no PSA grade, sale guarantee, or automatic grading premium assigned.
- **Initial 2026-11-08 coverage:** ten matching printings have researched hypothetical PSA 10 values; this is a **top ten among researched estimates**, not a complete census of graded comparisons for all holdings. Sparse sales, modeled values, older sales and variant ambiguities must be labeled. Blank PSA values mean **unresearched/unavailable**, not $0 or less valuable than the ranked cards. Avoid assigning PSA 10 values to ambiguous printing/foil versions.
- **MANDATORY on EVERY new pull, hidden-card verification, or repricing batch:**
  1. Complete the normal identity, deduplication, valuation, history and set-checklist steps.
  2. Sync the entire current set of `Include in value? = Yes` holdings (currently 231) to the existing public `Top Card Values` tab, retaining unique inventory IDs. Update unit raw prices, quantities, printings and dates; delete stale noncountable IDs only from the mirror, never historical holdings/event rows. No new spreadsheet is needed.
  3. For every newly acquired or newly repriced card that could enter either Top 10, look up **exact-printing PSA 10** sold comps or a clearly labeled third-party estimate. Append/update its dated record in the master `PSA 10 Estimates` and mirror it to the matching public row. Do not fabricate a PSA premium if reliable comps are absent; leave blank and report that the graded leaderboard has incomplete coverage.
  4. Verify the public `Top Card Values` count matches countable master holdings, cross-check current Top 10 raw and researched PSA 10 values, and open/refresh `values.html#top-cards`. Changes to public cells automatically rerank on page load/Refresh rankings; no GitHub commit is needed per pack unless page logic or styling changes.
  5. Quote dated raw and hypothetical PSA 10 values separately in pull receipts. Do **not** add ungraded cards' hypothetical PSA 10 valuations to the actual collection-value total. The website is a live reader of the **published public mirror**, not a continuously repricing market engine.

- Grading decisions must consider centering, corners, surface, print lines, PSA population and fees; a card described as Mint by the owner is not necessarily PSA 10.



## F. Exact-printing image completeness — mandatory with every pull (2026-11-08)

**Do not allow newly recorded owned cards to be image-less on the live virtual binder.** For each new pull, hidden-card identification, or existing collection-image correction:

1. Identify the **exact English set, collector number and printed card** first. Source a working HTTPS image for *that exact print*, using the official Pokémon card database, indexed card catalog or verified English scan. For Delta Reign (set code `me06`), use the exact card-number English scan available through Bills Archive only after confirming that it depicts the matching name and version; Mega Rayquaza ex *Double Rare* is **084/103**, not the 138/103 SIR or 141/103 gold version. For MEE Basic Energy 001–008, distinguish the Mega Evolution layout from older Scarlet & Violet energy cards and from stamped/alternate finishes.
2. Write the image URL into **All Cards!X** for each countable, independently owned inventory ID; propagate it to **Visible Cards!U** for actual physical front cards. All Cards should have **zero included Collection Holdings IDs with an empty image URL**. Keep uncertain physical IDs separate and labeled; an illustrative image for an unverified printing must say it is a reference, not the actual card.
3. For all extras (Trainers, Supporters, Items, Stadiums, Energy, Fossils), populate **All Cards!X**, which flows through **Public Extras Data** into the existing public **Extras** read feed. Do not append new values inside formula spill output ranges in either `Public Extras Data` (QUERY) or public `Extras` (IMPORTRANGE), since that produces `#REF!` and hides historical extras. Add new extras only to **All Cards**, then verify the public sheet includes all rows with image URLs.
4. Keep the existing public **Top Card Values!M** image mirror current for **all countable holdings**, including cards physically behind fronts, so the website's Most Valuable display has exact images. Verify **Card Values!D** also shows the images for front cards.
5. The website `index.html` chooses images by stable inventory ID: explicit current image, public Top Card Values image manifest, Card Values feed, and verified static fallbacks; separate physical and most-valuable views must show the same exact printing. The extras section reads the same public feed and has offline fallback for the most recent confirmed extras. Card image updates should **not** change card IDs, copy counts, pocket placements, market values or the history logs.
6. **QA gate before confirming any pack:** verify no countable `All Cards` row or `Top Card Values` row lacks an image, inspect the **Mega Rayquaza 084/103** custom form pocket when affected, check images in both Physical Binder and Most Valuable modes, and check the extras at the bottom of the page for complete populated images. If the external image host breaks, replace it with a confirmed working exact-print URL instead of silently displaying a blank box.
7. **One legacy uncertainty:** `N-20261106-069` is a Lightning Energy photo-alias with an unverified printing and **not a separate included holding**; the extras view displays explicitly labeled representative Lightning Energy artwork until the owner verifies the identity. Never automatically assign the MEE 004 printing to that ID or count it as a duplicate copy.

