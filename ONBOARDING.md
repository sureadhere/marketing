# Managing the SureAdhere Pre-Login Website

Advancements in AI now make it possible to build and maintain marketing websites significantly faster than before, without needing a dedicated web developer. With that shift, Dimagi is moving away from a world where the marketing team manages every product website in WordPress and toward one where each product division takes full ownership of its own pre-login site.

> **Important: Full ownership transfer.** By accepting this website, the SureAdhere team takes on complete ownership, including all future content updates, design changes, development work, and hosting. This site is being moved out of Dimagi.com's infrastructure and will no longer be supported by the marketing team. After handoff, the marketing team will not be available for ongoing edits, technical support, or hosting. Plan accordingly when scoping team resources.

This guide is split into two parts:

- **Transitioning Over:** the one-time setup to take ownership of your website.
- **Maintaining the Site:** your ongoing workflow for making edits.

No coding experience is required for basic edits, especially if you use Claude.

> **Good news: this is the simplest of the pre-login sites.** It's plain static HTML, with no build step and no framework. You preview it by opening a file in your browser, no servers or installs required to look at it. The trade-off is two manual rules you have to respect (header/footer consistency and link paths); both are covered below, and the built-in skills handle them for you.

---

# Part 1: Transitioning Over

One-time setup to take ownership of the SureAdhere pre-login website.

## Create a GitHub Account

GitHub is where the website files are stored. Think of it like a shared folder in the cloud where every change is tracked.

- Go to [github.com](https://github.com) and click **Sign up**.
- Enter your Dimagi email address (`@dimagi.com`), choose a username and password, and complete the verification steps.
- Check your inbox for a confirmation email and verify your account.

## Request Access to the Dimagi-Internal Organization

Once your account is created, you need to be added to the private organization where the websites live.

- Send your **GitHub username** to the person who manages the websites (or post it in the relevant Slack channel).
- You will receive an email invitation from GitHub titled something like *"You've been invited to join dimagi-internal."*
- Click **Accept invitation** in that email.

You now have access to the website repositories.

## Your Website's Repository

| Item | Value |
|------|-------|
| GitHub repository | [dimagi-internal/sureadhere-prelogin](https://github.com/dimagi-internal/sureadhere-prelogin) |
| Type | Static HTML (no build step, no framework) |
| Hosting | To be wired by your team (Cloudflare Workers static assets is the working assumption) |
| Public URL | `sureadhere.dimagi.com` *(migrating from `dimagi.com/sureadhere/`; DNS + deploy pending, see README)* |

- Go to [github.com/dimagi-internal/sureadhere-prelogin](https://github.com/dimagi-internal/sureadhere-prelogin) while signed in.
- Click the repository name to open it. You'll see all the files that make up the website.

> The repo's **`README.md`** has a checklist of launch items still open (DNS, deploy pipeline, legacy `sureadhere.com` redirect, app-store privacy URLs). Review it with your engineering team before go-live.

## Install Your Editing Tools

You'll use Claude Code (an AI assistant) to make edits without writing code. You only need to install these once.

- **Install Git:** Go to [git-scm.com/downloads](https://git-scm.com/downloads), download the installer for your operating system, and run it with the default settings.
- **Install Claude Code:** Go to [claude.ai/download](https://claude.ai/download) and download the Claude desktop app, or install the Claude Code CLI by running this in a Terminal:
  ```
  npm install -g @anthropic-ai/claude-code
  ```
- **Clone the repository** (download the files to your computer). Open a Terminal and run:
  ```
  git clone https://github.com/dimagi-internal/sureadhere-prelogin.git
  cd sureadhere-prelogin
  ```
- **Sign in to GitHub from the terminal** (recommended) so Claude can push changes on your behalf:
  ```
  gh auth login
  ```
  Follow the prompts to authenticate. Install the GitHub CLI first if needed: [cli.github.com](https://cli.github.com).

> No Node.js or build tools are required. This site is just HTML and CSS files.

## Connect to Your Main Website

The site is moving to its permanent home at `sureadhere.dimagi.com`. Wiring DNS and the publish pipeline is a one-time setup you do with your engineering team. The repo metadata, `sitemap.xml`, and `robots.txt` already point at the new host.

**What to ask engineering to set up:**

- Point `sureadhere.dimagi.com` at the chosen host and set up push-to-main auto-deploy (no CI workflow exists in the repo yet).
- Repoint the legacy `sureadhere.com` 301 redirect to `https://sureadhere.dimagi.com/`.
- Add WordPress 301 redirects from `dimagi.com/sureadhere/*` to the new URLs at cutover.
- Update the App Store and Play Store privacy-policy URL fields to `https://sureadhere.dimagi.com/privacy-policy/`.

**What you will learn from them:**

- How to test changes before they go public.
- Whether you publish directly or through a review step.
- Who else needs to approve changes before they go live.

---

# Part 2: Maintaining the Site

Your ongoing workflow for making edits to the website.

## The Two Rules That Break This Site Most Often

This site has no templating, so a few things are manual. Claude and the built-in skills handle them, but know they exist:

1. **The header and footer must be identical on every page.** There are 10 pages, and each has its own copy of the nav and footer. If you change one, you must change all of them. Always ask Claude to use the **`update-header-footer`** skill so it propagates everywhere.
2. **Root pages and subpages use different link paths.** Subpages link with `../` in front (e.g. `../assets/styles.css`); the homepage does not. Copy-pasting a block between them without fixing the `../` breaks the links. The **`add-page`** skill handles this when you create a page.

## Built-In Skills

This repo includes helper skills you can ask Claude to use by name:

- **`update-header-footer`:** change the nav or footer once and apply it to all pages.
- **`add-page`:** scaffold a new page with the correct head, nav, footer, and link paths, and register it in the sitemap and footer.
- **`add-publication`:** add a study to the Evidence (publications) page in the right format.

Just describe what you want; Claude will reach for the right one.

## Open Claude in Your Repository

Claude works best when opened directly inside your repository folder. This gives it full context of your website's files.

- Open a Terminal and navigate into your repository folder:
  ```
  cd path/to/sureadhere-prelogin
  ```
- Launch Claude Code from that folder:
  ```
  claude
  ```
  Claude will automatically load the context of your website.

> **Tip:** You can keep Claude running across multiple edits in one session. No need to restart between changes.

## Preview the Site Locally

No server needed. To check a change before publishing, open the repo folder in [Visual Studio Code](https://code.visualstudio.com) (or any file browser), find the `.html` file you changed, and drag it into a browser window. You'll see a live local preview without touching GitHub.

> Always look at the page you changed **and** at least one other page, to catch any header/footer drift.

## Make Changes with Claude

- Describe the change you want in plain English. Examples:
  - *"Update the hero headline on the homepage"*
  - *"Add a new study to the Evidence page"* (Claude will use the `add-publication` skill)
  - *"Change the demo-request email in the footer"* (Claude will propagate it to all pages)
  - *"Replace the hero image with the file called new-hero.png"*
- Claude will find the right files, make the edits, and show you what changed before writing anything.
- Review the changes. If something looks wrong, tell Claude: *"Actually, change that back."*
- When you're happy, publish. The simplest way is to ask Claude:
  - *"Go ahead and commit and push these changes."*

  Or do it yourself:
  ```
  git add .
  git commit -m "Brief description of what you changed"
  git push
  ```

> **Brand and content reminders:** primary color is teal `#0DA89D` (use the CSS variable, not raw hex). Never use em dashes. Use real images only (from `assets/images/`), never placeholders. Every study/release card shows its month and year. The English and Chinese privacy policies are legal documents, sync wording with Dimagi legal rather than rewriting freely.

## Verify Your Changes Are Live

- Once your deploy pipeline is wired (see Part 1), open the live URL.
- Do a hard refresh (**Cmd+Shift+R** on Mac, **Ctrl+Shift+R** on Windows) to clear the cache.
- Confirm your changes appear as expected, and glance at a second page to confirm the header/footer still match.

If you made a mistake, don't panic. Every change is saved and reversible. Ask Claude to *"undo my last change"* or, in GitHub, open the file, click **History**, and restore the version before your change.

## External Dependencies

Some parts of the website are pulled in from third-party services rather than living in your GitHub repo. These cannot be changed by editing the website files; you have to go to the service that hosts them.

### HubSpot Embed Forms

The contact and "Schedule a demo" forms are HubSpot forms, embedded via a HubSpot script (portal `503070`, the same account used by CommCare and Dimagi.com). The form fields, validation, where submissions are routed, and any follow-up automations are all configured inside HubSpot, not in the website's HTML.

- **Who manages it:** The marketing team, who owns the HubSpot account.
- **What lives in your repo:** Only the embed snippet and the CSS that styles how the form looks. You can adjust the visual styling, but you cannot change the form's fields or submission behavior from GitHub.
- **To change a form** (add or remove fields, change required fields, update where submissions go, edit follow-up emails): contact the marketing team and request the change in HubSpot.
- **If a form stops appearing or submissions stop coming through:** check with the marketing team first. The form may have been edited, archived, or unpublished in HubSpot.

### Cookie Banner

The cookie consent banner is rendered by HubSpot from the dashboard (Settings → Privacy and Consent → Cookies), not from code in this repo. Since the same HubSpot portal already serves CommCare and Dimagi.com, the categories should already be configured. It needs only a quick sanity check before cutover, no code change.
