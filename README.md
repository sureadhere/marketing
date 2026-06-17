# SureAdhere pre-login website

The public marketing site for **SureAdhere by Dimagi**. It is a static HTML site: no framework, no build step, no database. You edit HTML and CSS files directly, or have Claude Code do it using the skills below.

This README is the orientation guide. The day-to-day editing rules live in `CLAUDE.md`, which Claude Code loads automatically.

## Status: staging, awaiting handover

This repo is the staging copy. The site is moving to **`sureadhere.dimagi.com`** (mirroring `commcare.dimagi.com` and `connect.dimagi.com`). The HTML metadata, `sitemap.xml`, and `robots.txt` already point at the new host. The SureAdhere team will move this out of the current repo and wire DNS + deploy. Open items before launch:

1. **Repo home.** This copy lives at `github.com/dimagi-internal/sureadhere-prelogin`. The team may move it into a SureAdhere-owned org or fold it into an existing main repo.
2. **Legacy `sureadhere.com`.** Already owned; today it 301-redirects to `dimagi.com/sureadhere/`. At cutover, repoint that redirect to `https://sureadhere.dimagi.com/`.
3. **DNS + deploy.** Point `sureadhere.dimagi.com` at the chosen host (Cloudflare Workers static assets, mirroring CommCare, is the working assumption). No CI workflow exists in this repo yet; the team wires push-to-main auto-deploy.
4. **WordPress 301s.** Add redirects from `dimagi.com/sureadhere/*` to `sureadhere.dimagi.com/*` at the same time DNS flips.
5. **App store fields.** Update the App Store and Play Store privacy-policy URL fields to `https://sureadhere.dimagi.com/privacy-policy/`.
6. **HubSpot contact form.** Embed is wired (portalId `503070`, formId `e20b08b5-de23-42b1-9040-b37acff28623`). The team should confirm submissions land in a watched inbox.
7. **HubSpot cookie banner.** The HubSpot tracking loader is wired site-wide and renders the cookie banner from the HubSpot dashboard (Settings, Privacy and Consent, Cookies), the same setup CommCare and dimagi.com use. Confirm categories (necessary, analytics, advertising) are enabled before launch.
8. **Legal sign-off.** The two privacy policies (English and Mandarin) carry SureAdhere-specific language. Get Dimagi legal's written sign-off before launch.

## Pages and where they live

Live URLs are relative to `https://sureadhere.dimagi.com/`.

| File | Live URL | Notes |
|---|---|---|
| `index.html` | `/` | Homepage |
| `clinical-trials/index.html` | `/clinical-trials/` | Solution page |
| `public-health/index.html` | `/public-health/` | Solution page |
| `behavioral-health/index.html` | `/behavioral-health/` | Solution page |
| `resources/index.html` | `/resources/` | FAQ, timeline, release notes |
| `publications/index.html` | `/publications/` | Shown as "Evidence" in the nav: list of studies |
| `contact/index.html` | `/contact/` | Demo request and contact |
| `privacy-policy/index.html` | `/privacy-policy/` | Legal (sync with Dimagi legal) |
| `privacy-policy-chinese/index.html` | `/privacy-policy-chinese/` | Legal, Chinese |
| `login/index.html` | `/login/` | Bare redirect to the secure app |
| `404.html` | served on not-found | Has nav and footer |

Shared assets: `assets/styles.css` (all shared CSS), `assets/images/` (all images), `assets/favicon.png`.
SEO files: `sitemap.xml`, `robots.txt`.

## Run and preview locally

The site is plain files, so any static server works. The repo is already configured two ways:

- **Claude Code preview** (recommended when working in Claude Code): ask Claude to preview; the config in `.claude/launch.json` serves it on port 3030.
- **Manually:** from this folder run `npx serve -p 3030 .` and open `http://localhost:3030`.

Always view the page you changed plus one other page, to catch header/footer drift.

## Making common changes

| You want to... | Do this |
|---|---|
| Change a nav link, the logo, or anything in the footer | Run the `update-header-footer` skill (ask Claude: "update the header/footer"). It edits every page consistently. |
| Add a whole new page | Run the `add-page` skill. It scaffolds the page and registers it in the sitemap and footer. |
| Add a study to the Evidence page | Run the `add-publication` skill. |
| Edit copy or images on one existing page | Edit that page's HTML directly, following the brand and content rules in `CLAUDE.md`. |
| Change colors, spacing, or shared components | Edit `assets/styles.css`, using the existing CSS variables. |

## Skills (in `.claude/skills/`)
- **update-header-footer**: propagate a nav/footer change to all 10 pages, handling the root-vs-subpage path difference and the active-nav state.
- **add-page**: create a new page from the canonical template and register it everywhere it needs to appear.
- **add-publication**: add a correctly formatted study to the Evidence page.

## SEO files to keep current
- `sitemap.xml` lists every public URL on `sureadhere.dimagi.com`. Update it when you add, remove, or rename a page.
- `robots.txt` points the sitemap at `https://sureadhere.dimagi.com/sitemap.xml`.
- Each page has its own `<title>`, `<meta name="description">`, canonical URL, and Open Graph (`og:`) tags in its `<head>`. When you clone a page, change all of these; do not leave the source page's metadata in place.

## Brand and assets
- Teal `#0DA89D` primary; Work Sans and JetBrains Mono fonts.
- Logo: `assets/images/sureadhere-logo-full-color.png` (rendered white via a CSS filter in the nav and footer).
- App store badges: `assets/images/badge-app-store.svg`, `assets/images/badge-google-play.png`.

## Handle with care
- **Legal pages.** The two privacy policies are legal text. Coordinate edits with Dimagi legal.
- **External links.** The "Sign In" button, the login redirect, the help site, and the app store badges point to live systems. Do not change these URLs without confirming the new target.
- **Version control.** This folder is a git repository with its origin at `github.com/dimagi-internal/sureadhere-prelogin` (the staging copy). Commit every change there so the team has history and a safe undo, and run `git push` after committing. Confirm the repo's visibility (public vs private) in its GitHub settings before sharing it widely. The SureAdhere team may relocate this repo to a SureAdhere-owned org at handover.

## Deploy

This repo is staging only. See the "Status" section at the top for the open items (repo home, DNS, host, redirects) the SureAdhere team needs to resolve. Replace this section with the confirmed steps and the list of who can publish once the deploy pipeline is wired.

All canonical URLs, `og:url`, JSON-LD `@id`/`url`, `sitemap.xml`, and `robots.txt` already point to `https://sureadhere.dimagi.com/`. They are correct for the new host and do not need to be flipped at cutover.
