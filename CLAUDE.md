# SureAdhere pre-login site, working rules

This folder is the **SureAdhere by Dimagi** marketing site: a hand-built static HTML site with no build step and no framework. It deploys under `https://dimagi.com/sureadhere/`. The rules below apply to every change. Read them before editing.

## How the site is built
- 10 pages, each a complete standalone HTML file. There is **no templating**: the `<head>`, the top nav, and the footer are copied inline into every page.
- One shared stylesheet: `assets/styles.css`. Page-specific CSS lives in a `<style>` block in that page's `<head>`.
- Images live in `assets/images/`.
- `login/index.html` is a bare redirect to the secure app. It has no nav or footer; leave it alone unless the login URL changes.

## The two rules that break the site most often

**1. The header and footer must stay identical on every page.**
If you change the nav or the footer, apply the same change to all 10 pages. Do not edit one page's footer and leave the others behind. Use the `update-header-footer` skill, which does this for you.

**2. Root pages and subpages use different link paths.**
- Root-level pages (`index.html`, `404.html`) link like `clinical-trials/index.html` and `assets/styles.css`.
- Subpages (everything in a subfolder) link like `../clinical-trials/index.html` and `../assets/styles.css`.

Copy-pasting a block from the homepage into a subpage without adding `../` breaks every link. The skills handle this; if you edit by hand, mind the `../`. Subpages also mark their own nav item with `class="nav-active" aria-current="page"`.

## Brand and style
- Primary color: teal `#0DA89D` (defined as CSS variables in `assets/styles.css`; use the variables, not raw hex, in new CSS).
- Fonts: Work Sans (body) and JetBrains Mono (mono and eyebrows), loaded from Google Fonts in each `<head>`.
- Buttons: `btn btn-primary` (filled) and `btn btn-ghost` (outline). Reuse the existing classes; do not invent new button styles.

## Content rules
- **Never use em dashes.** Use commas, colons, parentheses, or "to" for ranges.
- **Real images only.** Use images already in `assets/images/` or the real asset from the source page. Never use placeholder images or guessed external URLs.
- **Every card or list item that references a study, article, or release shows its date** (month and year).
- Keep alt text on every image, and keep the `aria-label`s that are already there.

## Before you publish
- Preview locally and look at the page you changed **and** at least one other page, to catch header/footer drift.
- If you added, removed, or renamed a page, update `sitemap.xml` and the footer "Product" list. The `add-page` skill does this.
- The two privacy policies (English and Chinese) are legal documents: sync wording with Dimagi legal, do not freely rewrite them.

## Skills available in this folder
- `update-header-footer`: change the nav or footer once, propagate to all pages.
- `add-page`: scaffold a new page with the correct head, nav, footer, and paths, and register it in the sitemap and footer.
- `add-publication`: add a study to the Evidence (publications) page in the correct format.
