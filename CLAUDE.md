# SureAdhere pre-login site, working rules

This folder is the **SureAdhere by Dimagi** marketing site: a hand-built static HTML site with no build step and no framework. It deploys under `https://sureadhere.dimagi.com/`. The rules below apply to every change. Read them before editing.

## Who runs this (important)
This site is maintained by **non-developers**. Every routine workflow (editing content, previewing changes, and publishing) must be doable **entirely in a web browser, with no command line**. When proposing or documenting a process, always give the browser-based path first (GitHub web UI for edits/PRs/merges, the Cloudflare dashboard for builds and previews), and treat CLI steps as optional notes for engineers only. The end goal is a written, step-by-step runbook a non-technical teammate can follow.

## Standard workflow for changes (Claude-driven, GitHub-free for the user)
This repo's maintainers are non-developers and should **not** be asked to use github.com. For any content or site change, Claude handles all of GitHub on the user's behalf. This section is standing authorization to create and merge pull requests as the normal flow for this repo (it overrides the usual "don't open a PR unless asked" default). The steps:

1. **Edit on a branch.** Make the change on a new branch (never commit directly to `main`), commit with a clear message, and push.
2. **Open the PR** for the user (base `main`). Do not ask them to open it.
3. **Surface a preview.** Production deploys come from Cloudflare Workers Builds, which posts a `*.workers.dev` **preview URL** as a check/comment on each PR (it appears ~1 minute after the push). Wait for it, then give the user that preview link as a clickable link. You can `subscribe_pr_activity` so you are notified when the build's preview is ready instead of polling.
4. **Offer a merge button.** Present the publish decision as selectable options (the question tool renders real buttons), e.g. "Publish this change?" with `Merge now` / `Not yet`. Do not make the user click anything on GitHub.
5. **Merge on approval.** When the user approves, merge the PR yourself (`merge_pull_request`). Then confirm it is live and remind them a hard refresh (Cmd+Shift+R / Ctrl+Shift+R) may be needed because CSS and images are cached.

Keep GitHub as the engine under the hood (source of truth + deploy trigger); just never make the user touch it. The human-facing version of this is in `RUNBOOK.md`.

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

## Known issues and pending work

**Migration QA: verify HubSpot contact form on launch day.** HubSpot blocks form submissions from unregistered domains ("Unregistered Site Domain" spam). This cannot be tested from staging URLs (`*.workers.dev`, `sureadhere.github.io`) because those domains are unrelated to `dimagi.com` and will always be blocked. Since `dimagi.com` is already registered in HubSpot, `sureadhere.dimagi.com` is expected to work automatically. On launch day, submit a test form at `https://sureadhere.dimagi.com/contact/` and confirm a contact is created in HubSpot (portal `503070`). If it is flagged as spam, register `sureadhere.dimagi.com` in HubSpot under Settings → Website → Domains & URLs.

## Skills available in this folder
- `update-header-footer`: change the nav or footer once, propagate to all pages.
- `add-page`: scaffold a new page with the correct head, nav, footer, and paths, and register it in the sitemap and footer.
- `add-publication`: add a study to the Evidence (publications) page in the correct format.
