---
name: add-publication
description: Add a study to the SureAdhere Evidence page (publications/index.html) in the established pub-item format, including the required date, number, journal citation, and filter attributes. Use when adding a publication, study, or research citation to the evidence list.
---

# Add a publication to the Evidence page

Studies live in `publications/index.html` inside the `<div class="pub-list">`. Each one is a numbered `pub-item`. Insert new items in the right position and keep the numbering sequential.

## The pub-item template

```html
<div class="pub-item" data-type="TYPE" data-region="REGION">
  <div class="pub-num">NN</div>
  <div class="pub-body">
    <span class="pub-rct-tag">RCT</span>
    <div class="pub-title"><a href="STUDY URL" target="_blank" rel="noopener">FULL STUDY TITLE</a></div>
    <div class="pub-meta">AUTHORS. <em>JOURNAL.</em> YEAR;VOL(ISSUE):PAGES.</div>
    <div class="pub-finding">ONE OR TWO SENTENCES ON THE KEY FINDING.</div>
  </div>
  <div class="pub-year">MON YYYY</div>
  <div class="pub-location">COUNTRY<span class="pub-location-region">REGION LABEL</span></div>
</div>
```

## Field rules

- `data-type`: matches the page's filter controls (e.g. `rct`, `feasibility`). Reuse an existing value; check the filter buttons near the top of the page for the valid set.
- `data-region`: e.g. `usa`, `asia`. Reuse an existing value.
- `pub-num`: two digits (`01`, `02`, ...). Keep the list sequentially numbered; if you insert in the middle, renumber the items after it.
- `pub-rct-tag`: include the `<span class="pub-rct-tag">RCT</span>` line **only** for randomized controlled trials; omit it otherwise.
- `pub-meta`: a standard citation, with the journal name in `<em>...</em>`.
- **`pub-year`: required. Format `MON YYYY`** (e.g. `Jan 2022`). Every study must show its date.
- `pub-location`: country, plus a region label in the inner span.
- Study links open in a new tab: `target="_blank" rel="noopener"`.

## Steps

1. Find the right position in `<div class="pub-list">` (match the existing order, usually grouped or newest first).
2. Insert the new `pub-item` using the template above.
3. Renumber the `pub-num` values if you inserted before existing items.
4. Use a `data-type` and `data-region` that the page's filter UI already supports.

## Verify

- Preview `publications/`: the new item renders, the date shows, and the page's search and filters still include it.
- Confirm the numbering is sequential, with no duplicates or gaps.
