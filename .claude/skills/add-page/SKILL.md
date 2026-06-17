---
name: add-page
description: Create a new page for the SureAdhere site from the canonical template, with correct head metadata, the shared nav and footer, root-vs-subpage link paths, and register it in sitemap.xml and the footer. Use when adding a brand-new page to the site.
---

# Add a new page

New SureAdhere pages live in their own folder as `index.html`, so the URL is clean (e.g. `new-thing/`, not `new-thing.html`). A new page touches up to four places: the new file, `sitemap.xml`, the footer on every page, and the nav (if it should appear there).

## 1. Create the folder and file

Create `<slug>/index.html` with a lowercase, hyphenated slug. Because it sits in a subfolder, all internal links and asset paths use `../`.

## 2. Use this head template

Fill in every value in CAPS. Keep the fonts and stylesheet links exactly as shown.

```html
<!doctype html>
<html lang="en">
<head>
<meta charset="utf-8" />
<link rel="icon" type="image/png" href="../assets/favicon.png">
<meta name="viewport" content="width=device-width, initial-scale=1" />
<title>PAGE TITLE | SureAdhere</title>
<meta name="description" content="ONE SENTENCE DESCRIPTION OF THIS PAGE.">

<meta property="og:type" content="website">
<meta property="og:url" content="https://sureadhere.dimagi.com/SLUG/">
<link rel="canonical" href="https://sureadhere.dimagi.com/SLUG/">
<meta property="og:title" content="PAGE TITLE">
<meta property="og:description" content="ONE SENTENCE DESCRIPTION.">
<meta property="og:image" content="https://sureadhere.dimagi.com/assets/images/SOME-REAL-IMAGE.png">
<meta name="twitter:card" content="summary_large_image">
<link rel="preconnect" href="https://fonts.googleapis.com">
<link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
<link href="https://fonts.googleapis.com/css2?family=Work+Sans:wght@200;300;400;500;600;700&family=JetBrains+Mono:wght@400;500;600&display=swap" rel="stylesheet">
<link rel="stylesheet" href="../assets/styles.css">
<style>
  /* page-specific CSS only; shared styles live in assets/styles.css */
</style>
</head>
<body>
```

Every value (title, description, canonical, og:url, og:image) must be unique to this page. `og:image` must be a real file in `assets/images/`.

## 3. Add the shared nav and footer

Copy the **subpage** version of the nav (`<div class="nav-wrap">...</div>`) and footer (`<footer>...</footer>`) from an existing subpage such as `clinical-trials/index.html`; those already use `../` paths. Then copy the hamburger `<script>` from the bottom of that page, placed just before `</body>`.

If this new page should appear in the top nav, add it to the `nav-links` and give it `class="nav-active" aria-current="page"` on this page only. Adding it to the nav means it must be added to the nav on every other page too: use the `update-header-footer` skill.

## 4. Register the page

- **`sitemap.xml`:** add a `<url>` block with `<loc>https://sureadhere.dimagi.com/SLUG/</loc>` and a sensible `changefreq` and `priority` (match the existing entries).
- **Footer "Product" list:** if the page belongs there, add a `<li><a href="...">` entry, then propagate the footer to all pages with the `update-header-footer` skill.

## 5. Verify

- Preview the new page: the nav and footer render, the stylesheet loads (no unstyled flash), and all links resolve (the `../` paths work).
- Confirm the page metadata is unique (title, description, canonical), not left over from the source page.
- Confirm the new URL is present in `sitemap.xml`.
