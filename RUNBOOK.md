# How to edit the SureAdhere website

**The one thing to know:** to change anything on the SureAdhere website, open
Claude, point it at this project, and tell it what you want in plain English.
Claude does everything else and shows you a preview and a publish button. You do
**not** need to write code, use the command line, or open GitHub.

### The whole process in 60 seconds

1. Open **Claude** on the `sureadhere/marketing` project.
2. Type what you want changed, for example:
   *"On the homepage, change the headline to 'adherence matters'."*
3. Claude replies with a **preview link** — click it to check your change.
4. Claude shows a **Merge now** button — click it to publish.
5. Wait about a minute, then refresh the live site. Done.

That is the entire job. Everything below just explains each step, how to get set
up the first time, and what to do if something doesn't work.

> **New here? Read the next section first.** It's a one-time setup checklist.

---

## Before your first edit: get set up (one time)

You need three things. Ask your engineering contact (or whoever invited you) to
confirm each one, then tick them off:

- [ ] **A Claude session pointed at the `sureadhere/marketing` project.** This is
      where you type your requests, and it's the main thing you need.
- [ ] **GitHub access to the repo.** Claude uses this *on your behalf* to save
      and publish changes. You normally never open GitHub yourself.
- [ ] **A Cloudflare dashboard login** (optional). Only needed if you ever want
      to find a preview link yourself instead of letting Claude hand it to you.

**Quick test that you're ready:** open your Claude session and ask
*"What pages does this website have?"* If it answers with the real pages
(Homepage, Clinical Trials, Behavioral Health, and so on), you're set. If it
can't, see **Troubleshooting: is Claude set up correctly?** near the bottom.

---

## Two ways to edit

- **The easy way (recommended): just ask Claude.** You describe the change in
  plain language; Claude does all the GitHub work and hands you a preview link
  and a merge button. You never open github.com. **Start here.**
- **The manual way (fallback):** do the steps yourself on the GitHub website.
  Use this only if Claude isn't available. It's further down under
  "Manual fallback."

---

## The easy way: ask Claude (recommended)

This is the standard way to edit the site. Open a Claude session on the
`sureadhere/marketing` repo and just talk to it.

1. **Describe your change** in plain language, for example:
   > "On the homepage, change the headline to 'adherence matters' and remove
   > the word 'most'."
2. **Claude makes the change and prepares it.** It edits the file, saves it on
   a safe branch, and opens a proposed change (a "pull request") for you. You
   don't touch GitHub.
3. **Claude sends you a preview link.** Within a minute or so, Claude posts a
   private preview link (it ends in `.workers.dev`). Click it and confirm the
   change looks right.
4. **Claude shows you a publish button.** You'll see a choice like
   **"Publish this change?"** with **Merge now** / **Not yet**. Click
   **Merge now** when you're happy.
5. **It goes live.** Claude publishes the change; the live site updates within a
   minute or two. If you still see the old version, do a hard refresh
   (**Cmd+Shift+R** on Mac, **Ctrl+Shift+R** on Windows).

That's it. If the preview doesn't look right, just tell Claude what to fix and
it will update the same change and send a new preview.

> **What you need:** access to a Claude session pointed at the
> `sureadhere/marketing` repo. Ask your engineering contact to set this up if
> you don't have it.

---

## Manual fallback: do it yourself on GitHub

Use this only if you can't use Claude. It does the same three things by hand:
**edit → preview → publish**, entirely in your web browser.

> Throughout this section, *"the repo"* means the project's files on GitHub at
> **https://github.com/sureadhere/marketing**. A *"PR" (pull request)* is just a
> proposed set of changes that you review before they go live.

**What you need:** a **GitHub account** with access to the repo, and (optional
but recommended) a **Cloudflare dashboard** login to see the preview. Ask your
engineering contact for both.

### Step 1 — Edit a page

1. Go to **https://github.com/sureadhere/marketing**.
2. Open the file you want to change. The site's pages live in folders, for
   example:
   - Homepage: `index.html`
   - Clinical Trials page: `clinical-trials/index.html`
   - Behavioral Health page: `behavioral-health/index.html`
   - (Each section has its own folder with an `index.html` inside.)
3. Click the **pencil icon** (✏️ "Edit this file") in the top-right of the file
   view.

   > 📸 *Screenshot: the pencil/Edit icon on a file page.*

4. Make your text change directly in the editor. For simple wording changes,
   find the text and type over it. (For anything involving layout, images, or
   new pages, ask your engineering contact or start an AI-assisted session
   instead of editing by hand.)
5. When done, scroll down to the **"Commit changes"** box (or click the green
   **Commit changes...** button, top-right).
6. In the dialog:
   - Write a short description of what you changed (e.g. "Update homepage
     stat to 99%").
   - Choose **"Create a new branch for this commit and start a pull request."**
     Give the branch a simple name (e.g. `update-homepage-stat`).
   - Click **Propose changes**, then **Create pull request** on the next
     screen.

   > 📸 *Screenshot: the "Commit changes" dialog with "Create a new branch"
   > selected.*

You've now created a **pull request (PR)** — a proposed change that is *not yet
live*. Next you'll preview it.

---

### Step 2 — Preview your change

You have two ways to preview, depending on what's set up. Use whichever is
available to you.

#### Option A (recommended): Cloudflare preview

When the repo is connected to **Cloudflare Workers Builds**, every pull request
automatically gets its own private preview link.

1. Log in at **https://dash.cloudflare.com**.
2. Go to **Workers & Pages**, then open the **`marketing`** project.
3. Click the **Builds** (or **Deployments**) tab.
4. Find the build that matches your pull request. Click it.
5. Open the **preview URL** it shows (it ends in `.workers.dev`).

   > 📸 *Screenshot: the Builds tab with a preview build and its preview URL.*

This preview shows your change exactly as it will look live, without affecting
the real site.

#### Option B: ask for a preview link in the PR

If you don't have Cloudflare access, post a comment on your pull request asking
your engineering contact (or the AI assistant watching the PR) to share the
preview link. The same Cloudflare preview can be opened by anyone with access.

> **Tip:** If a preview looks out of date, refresh the page. If it still looks
> wrong, do a "hard refresh": **Ctrl+Shift+R** (Windows) or **Cmd+Shift+R**
> (Mac). Browsers cache styling, so this forces the newest version to load.

---

### Step 3 — Publish (make it live)

When the preview looks right:

1. Go back to your pull request on GitHub.
2. Make sure any automatic checks at the bottom show green checkmarks. If
   something is red, ask your engineering contact before continuing.
3. Click the green **Merge pull request** button, then **Confirm merge**.

   > 📸 *Screenshot: the green "Merge pull request" button.*

4. (Optional) Click **Delete branch** afterward to keep things tidy. This does
   not affect the live site.

Merging publishes your change. The live site updates automatically within a
couple of minutes.

---

### After publishing: confirm it's live

1. Wait 1–2 minutes for the site to rebuild.
2. Open the live site and check your change.
3. If you still see the old version, do a **hard refresh**
   (**Ctrl+Shift+R** / **Cmd+Shift+R**) or open the page in a private/incognito
   window. Styling and images are cached by your browser and by the network, so
   a fresh load is sometimes needed.

---

## Troubleshooting: is Claude set up correctly?

Most problems are about Claude's **access**. Here's how to check and fix each
one. You can paste the suggested questions straight into your Claude session.

### How to confirm Claude is connected to the right project

Ask Claude:

> "Can you confirm you have access to the `sureadhere/marketing` repository and
> that you can open and merge pull requests?"

A healthy answer names the repo and says it can open and merge PRs. If it says
it cannot, use the fixes below.

### "Claude can't find the project or the files"

- **What it looks like:** Claude says it doesn't know what site you mean, or
  can't list the pages.
- **Fix:** Your session probably isn't pointed at the right project. Start a new
  Claude session from the `sureadhere/marketing` project, or ask your
  engineering contact to add the repo to your session.

### "Claude made the change but can't save or publish it"

- **What it looks like:** Claude edits text but then says it can't push, can't
  open a pull request, or doesn't have permission.
- **Fix:** Claude's GitHub access for this repo is missing. Ask your engineering
  contact to grant the Claude integration (and your account) **write access** to
  `sureadhere/marketing`. Then start a fresh session and try again.

### "No preview link ever appears"

- **First, wait a minute.** The preview is built after Claude saves the change
  and usually takes about a minute.
- **Then ask Claude:** *"Has the Cloudflare preview finished? Can you share the
  preview link?"* Claude can check the change for you and pass along the link.
- **If there's still no preview:** the Cloudflare auto-preview may not be
  connected to the repo. Tell Claude *"There's no preview link, please check
  why"*, and if it can't resolve it, ask your engineering contact to confirm
  **Cloudflare Workers Builds** is connected to `sureadhere/marketing`.

### "Claude opened the change but didn't publish it"

- That's expected. Claude waits for **you** to approve. Click the **Merge now**
  button (or tell Claude "publish it"). If Claude says it can't merge, that's a
  permissions issue, see "can't save or publish it" above.

### "I published, but the live site still looks old"

- Give it 1–2 minutes, then do a **hard refresh**: **Cmd+Shift+R** (Mac) or
  **Ctrl+Shift+R** (Windows). Browsers and the network cache the site, so a
  normal refresh sometimes shows the old version. An incognito/private window
  also works.

### Still stuck?

Tell Claude exactly what you see (copy any error message). If it can't fix it,
contact your engineering contact and include: what you asked for, what Claude
replied, and any link or error it gave you.

---

## Quick glossary

| Term | Plain meaning |
| --- | --- |
| Repo / repository | The folder of all the site's files on GitHub. |
| Branch | A safe copy of the files where your change lives until it's published. |
| Commit | A saved change with a short description. |
| Pull request (PR) | A proposed change you review before it goes live. |
| Merge | The button that publishes a pull request to the live site. |
| Preview / build | A private copy of the site showing your change before it's live. |
| Hard refresh | Reloading a page while ignoring the cached (old) version. |

---

## When to ask for help instead of editing yourself

Do these with your engineering contact or an AI-assisted session, not by hand:

- Adding, removing, or renaming a whole page.
- Changing the top navigation menu or the footer (these must stay identical on
  every page).
- Adding or swapping images.
- Editing either privacy policy (English or Chinese) — these are legal
  documents and must be cleared with Dimagi legal first.

For everything else, the three steps above are all you need.
