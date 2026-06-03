---
name: update-header-footer
description: Propagate a change to the site-wide top navigation or footer across every page of the SureAdhere site, keeping them identical and fixing the root-vs-subpage link paths. Use whenever editing the nav, logo, nav links, nav CTAs, or anything in the footer.
---

# Update the header or footer across all pages

The SureAdhere site has no templating, so the nav and footer are copied into every page. This skill makes a header/footer change once and applies it consistently everywhere.

## Pages that contain the shared nav and footer (10 total)

**Root-level** (links use no prefix, e.g. `clinical-trials/index.html`, `assets/styles.css`):
- `index.html`
- `404.html`

**Subpages** (links use `../`, e.g. `../clinical-trials/index.html`, `../assets/styles.css`):
- `clinical-trials/index.html`
- `public-health/index.html`
- `behavioral-health/index.html`
- `resources/index.html`
- `publications/index.html`
- `contact/index.html`
- `privacy-policy/index.html`
- `privacy-policy-chinese/index.html`

**Do NOT touch:** `login/index.html` (a bare redirect with no nav or footer).

## How link paths differ between root and subpages

When you copy a nav/footer block, internal links must match the page's depth:

| Link target | Root pages (`index.html`, `404.html`) | Subpages |
|---|---|---|
| Home | `index.html` | `../index.html` |
| A solution page | `clinical-trials/index.html` | `../clinical-trials/index.html` |
| Contact | `contact/` | `../contact/` |
| Stylesheet / images | `assets/...` | `../assets/...` |
| External (`https://`, `mailto:`) | unchanged | unchanged |

On a subpage, the nav link to **that same page** also carries `class="nav-active" aria-current="page"`. Only the four nav items have an active state: Clinical Trials, Public Health, Behavioral Health, Resources. The other subpages have no active nav item.

## Steps

1. **Make the change on `index.html` first.** Locate the block:
   - Nav: the `<div class="nav-wrap">` ... `</div>` block, just after `<body>`.
   - Footer: the `<footer>` ... `</footer>` block near the end.

   Edit it there to the desired new state, using root-style paths.

2. **Apply the same change to `404.html`** (also root-style paths, identical to `index.html`).

3. **Apply the same change to each of the 8 subpages.** Take the new block and:
   - Add `../` to every internal link and asset path.
   - On the subpage's own nav item, keep or add `class="nav-active" aria-current="page"`.

4. **Verify.** After editing:
   - Confirm the nav link set and footer link set are the same on every page; only the `../` prefix and the active item should differ.
   - Preview `index.html` and at least one subpage; check the nav and footer render and that links resolve.

## Verification commands

Run from the site root to spot drift:

```bash
# Count footers per page (every listed page should show exactly 1)
for f in index.html 404.html clinical-trials/index.html public-health/index.html behavioral-health/index.html resources/index.html publications/index.html contact/index.html privacy-policy/index.html privacy-policy-chinese/index.html; do printf "%-36s footer:%s nav:%s\n" "$f" "$(grep -c '<footer' "$f")" "$(grep -c 'nav-wrap' "$f")"; done

# Compare the nav links block between the homepage and a subpage
grep -A6 'nav-links' index.html
grep -A6 'nav-links' clinical-trials/index.html
```

If you changed a nav label or a footer link, also check the footer "Product" list (it mirrors the nav) and any in-page links pointing to that destination.
