# Avagene Therapeutics — Website

Static site for **www.avagene.com**, built to be hosted on GitHub Pages.

The single most important page is `/fcoi.html` — the publicly accessible Financial Conflict of Interest policy required by **42 CFR 50.604(a)** before any PHS expenditure on the SBIR award.

---

## File structure

```
avagene-site/
├── index.html       Home page
├── platform.html    Bodhi platform technical detail
├── about.html       Company, founder, performance sites, IP
├── fcoi.html        FCOI Policy (compliance page — KEY DELIVERABLE)
├── contact.html     Contact info + organizational identifiers
├── 404.html         Custom 404 page
├── styles.css       Shared stylesheet
├── CNAME            Custom domain config for GitHub Pages
└── README.md        This file
```

No build step. No JavaScript framework. No dependencies. Pure HTML + CSS that loads two Google Fonts (Fraunces + IBM Plex Sans/Mono).

---

## Before you publish — three things to do in `fcoi.html`

The FCOI page ships in a "draft" state. Before you make it live:

1. **Remove the orange implementation banner at the top** — the `<div class="draft-banner">` block. It's labeled "Implementation note for Avagene staff" and is meant to be deleted before publication.
2. **Fill in `[INSERT EFFECTIVE DATE]`** in the metadata grid near the top — use the date the policy is signed.
3. **Fill in `[INSERT DATE]` in the signature block** at the bottom — same date, and have Rupert sign the corresponding Word/PDF version of the policy for your records.

Optional but recommended: set up `fcoi@avagene.com` as a real inbox (or alias to Rupert's email). The policy commits Avagene to a 5-business-day response on 50.605(a)(5) information requests.

---

## Deploying to GitHub Pages

### 1. Create the repo

Create a new public repo on GitHub. Name it whatever you like — `avagene-site`, `avagene.com`, `website` all work. (It does **not** need to be named `<username>.github.io` for a custom-domain setup.)

### 2. Upload the files

Either:

**(a) Drag-and-drop in the browser:** open the empty repo → "uploading an existing file" → drag every file in this directory into the upload area → commit.

**(b) From the command line:**

```bash
cd /path/to/avagene-site
git init
git add .
git commit -m "Initial Avagene website"
git branch -M main
git remote add origin git@github.com:<your-username>/<repo-name>.git
git push -u origin main
```

### 3. Enable Pages

In the repo on GitHub:

- **Settings → Pages**
- **Source:** Deploy from a branch
- **Branch:** `main` · **Folder:** `/ (root)` · Save

GitHub will serve the site at `https://<your-username>.github.io/<repo-name>/` within ~1 minute. Open it to confirm the styling looks right.

### 4. Point `avagene.com` at GitHub

In your domain registrar's DNS panel for `avagene.com`, add **five records**:

| Type   | Name / Host | Value                                |
| ------ | ----------- | ------------------------------------ |
| `A`    | `@`         | `185.199.108.153`                    |
| `A`    | `@`         | `185.199.109.153`                    |
| `A`    | `@`         | `185.199.110.153`                    |
| `A`    | `@`         | `185.199.111.153`                    |
| `CNAME`| `www`       | `<your-username>.github.io`          |

The four `A` records are GitHub Pages' apex servers (current as of writing — verify at <https://docs.github.com/en/pages/configuring-a-custom-domain-for-your-github-pages-site/managing-a-custom-domain-for-your-github-pages-site> if needed). The `CNAME` for `www` points to your GitHub user namespace (not the repo URL).

The `CNAME` file already in this repo tells GitHub to expect the domain `www.avagene.com`. Don't delete it.

### 5. Set the custom domain in GitHub

Back in **Settings → Pages**:

- **Custom domain:** `www.avagene.com` → Save
- Wait for the green check ("DNS check successful"). This usually takes 5–30 minutes.
- Once checked, tick **Enforce HTTPS**. The TLS certificate provisions automatically (10 minutes to an hour).

### 6. Verify

Visit:

- `https://www.avagene.com` → home page loads
- `https://avagene.com` → redirects to `www.avagene.com`
- `https://www.avagene.com/fcoi.html` → FCOI policy loads, no draft banner, dates filled in

That URL — `https://www.avagene.com/fcoi.html` — is what you submit via the eRA Commons IPF Module.

---

## Editing the site later

All content is plain HTML — open any file in any text editor, change copy, save, push to GitHub. Pages redeploys automatically within a minute or two.

To change the look: everything is in `styles.css` under CSS variables at the top (`--ink`, `--accent`, `--canvas`, etc.). Change those tokens and the whole site updates.

---

## Notes

- **No analytics, no cookies, no third-party trackers** are loaded. The only external request is to Google Fonts. If you want to remove that too, download the Fraunces and IBM Plex font files and self-host them.
- **Accessibility:** semantic HTML, sufficient color contrast on the primary navy/ivory pairing, focus-visible defaults from the browser.
- **Mobile:** all pages are responsive — tested at the 720px and 900px breakpoints.
