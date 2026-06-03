# SureAdhere pre-login website

The public marketing site for **SureAdhere by Dimagi**, served at `https://dimagi.com/sureadhere/`. It is a static HTML site: no framework, no build step, no database. You edit HTML and CSS files directly, or have Claude Code do it using the skills below.

This README is the orientation guide. The day-to-day editing rules live in `CLAUDE.md`, which Claude Code loads automatically.

## Pages and where they live

| File | Live URL | Notes |
|---|---|---|
| `index.html` | `/sureadhere/` | Homepage |
| `clinical-trials/index.html` | `/sureadhere/clinical-trials/` | Solution page |
| `public-health/index.html` | `/sureadhere/public-health/` | Solution page |
| `behavioral-health/index.html` | `/sureadhere/behavioral-health/` | Solution page |
| `resources/index.html` | `/sureadhere/resources/` | FAQ, timeline, release notes |
| `publications/index.html` | `/sureadhere/publications/` | Shown as "Evidence" in the nav: list of studies |
| `contact/index.html` | `/sureadhere/contact/` | Demo request and contact |
| `privacy-policy/index.html` | `/sureadhere/privacy-policy/` | Legal (sync with Dimagi legal) |
| `privacy-policy-chinese/index.html` | `/sureadhere/privacy-policy-chinese/` | Legal, Chinese |
| `login/index.html` | `/sureadhere/login/` | Bare redirect to the secure app |
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
- `sitemap.xml` lists every public URL. Update it when you add, remove, or rename a page.
- `robots.txt` currently points the sitemap at `/sitemap.xml`. Update it to the absolute URL if SureAdhere moves to its own domain.
- Each page has its own `<title>`, `<meta name="description">`, canonical URL, and Open Graph (`og:`) tags in its `<head>`. When you clone a page, change all of these; do not leave the source page's metadata in place.

## Brand and assets
- Teal `#0DA89D` primary; Work Sans and JetBrains Mono fonts.
- Logo: `assets/images/sureadhere-logo-full-color.png` (rendered white via a CSS filter in the nav and footer).
- App store badges: `assets/images/badge-app-store.svg`, `assets/images/badge-google-play.png`.

## Handle with care
- **Legal pages.** The two privacy policies are legal text. Coordinate edits with Dimagi legal.
- **External links.** The "Sign In" button, the login redirect, the help site, and the app store badges point to live systems. Do not change these URLs without confirming the new target.
- **Version control.** This folder is a git repository with its origin at `github.com/dimagi-internal/sureadhere-prelogin`. Commit every change there so the team has history and a safe undo, and run `git push` after committing. Confirm the repo's visibility (public vs private) in its GitHub settings before sharing it widely.

## Deploy

> **[TO BE CONFIRMED, Gillian to fill in.]**
> How the files get from this folder to `https://dimagi.com/sureadhere/` (part of the dimagi.com static migration? a push to a branch? a manual upload to Kinsta?), and who has access to do it. This is the most important section for the SureAdhere team; complete it before handover.
