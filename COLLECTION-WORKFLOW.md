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
