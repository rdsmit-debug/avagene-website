# Track C Deployment Checklist — Cloudflare + GitHub Pages

End-to-end steps to get `https://www.avagene.org/fcoi.html` live and Track-C compliant.
Estimated time: **45 minutes of active work**, plus 30–60 minutes of waiting for DNS/SSL.

---

## Prerequisites

- [x] `avagene.org` registered at Cloudflare
- [ ] A GitHub account (free tier is fine)
- [ ] Files from `avagene-site.zip` downloaded and unzipped locally

---

## Step 1 — Push the site to GitHub  *(~10 min)*

1. Go to **github.com** → click **+** (top right) → **New repository**
2. Repository name: `avagene-site` (or whatever you like — name doesn't matter)
3. **Public** repo
4. Do NOT initialize with a README (you have one already)
5. Click **Create repository**
6. On the next screen, click **uploading an existing file**
7. Drag every file from the unzipped `avagene-site` folder into the upload area
   - Make sure `CNAME`, `README.md`, all `.html` files, and `styles.css` all upload
   - The `CNAME` file is what tells GitHub to serve the custom domain — easy to miss
8. Commit message: "Initial site"
9. Click **Commit changes**

---

## Step 2 — Enable GitHub Pages  *(~2 min)*

1. In your repo, click **Settings** (top right of the repo page)
2. Left sidebar → **Pages**
3. **Source**: Deploy from a branch
4. **Branch**: `main` · **Folder**: `/ (root)` · **Save**
5. Wait ~30 seconds, then refresh. You'll see a green box: "Your site is live at `https://<your-username>.github.io/avagene-site/`"
6. **Click that link to verify the site loads.** Make sure the FCOI page, home page, and styling all look right. If it works at this URL, the rest is just DNS plumbing.

---

## Step 3 — Set the custom domain in GitHub  *(~1 min)*

Still in **Settings → Pages**:

1. Under **Custom domain**, type: `www.avagene.org` → **Save**
2. GitHub will show "DNS check in progress" — that's expected. It'll fail until you do Step 4. Move on.

---

## Step 4 — Configure DNS in Cloudflare  *(~5 min)*

1. Go to **dash.cloudflare.com** → click on the `avagene.org` domain
2. Left sidebar → **DNS** → **Records**
3. Delete any existing records that Cloudflare auto-created (often one A record pointing somewhere generic, and a CNAME for www)
4. Click **+ Add record** five times to add these:

| Type   | Name    | IPv4 / Target                | Proxy status      |
| ------ | ------- | ---------------------------- | ----------------- |
| `A`    | `@`     | `185.199.108.153`            | **DNS only** (gray cloud) |
| `A`    | `@`     | `185.199.109.153`            | **DNS only** (gray cloud) |
| `A`    | `@`     | `185.199.110.153`            | **DNS only** (gray cloud) |
| `A`    | `@`     | `185.199.111.153`            | **DNS only** (gray cloud) |
| `CNAME`| `www`   | `<your-username>.github.io`  | **DNS only** (gray cloud) |

> **Important:** for each record, click the orange cloud icon to turn it **gray** ("DNS only"). If you leave Cloudflare's proxy on (orange cloud), GitHub Pages' SSL certificate provisioning will fail. You can turn the proxy back on later once everything works, but start with it off.

> Replace `<your-username>` with your actual GitHub username (lowercase, no spaces).

5. Save each record.

---

## Step 5 — Wait for DNS, then enable HTTPS in GitHub  *(~10–60 min waiting)*

1. Wait ~10 minutes for DNS to propagate
2. Go back to **GitHub → Settings → Pages**
3. The "DNS check" box should now show a green check
4. Tick **Enforce HTTPS** (the checkbox below the custom domain field)
5. The TLS certificate provisions automatically — usually 10 minutes, sometimes up to an hour. The page will say "TLS certificate is being provisioned" then "Your site is live at https://www.avagene.org" when done.

---

## Step 6 — Set up `fcoi@avagene.org` email forwarding  *(~5 min)*

This satisfies the policy's commitment to respond to 50.605(a)(5) information requests.

1. In Cloudflare dashboard → `avagene.org` → left sidebar → **Email** → **Email Routing**
2. Click **Get started** / **Enable Email Routing**
3. Cloudflare will offer to add the necessary MX and TXT records automatically — click **Add records and enable**
4. Once enabled, go to the **Routing rules** tab
5. Click **Create address**
6. Custom address: `fcoi` → Destination: `<your-real-email-address>` → **Save**
7. Verify your destination email (Cloudflare sends a confirmation link)
8. (Optional but recommended) Repeat for `hello@avagene.org` and `rupert@avagene.org`

Send a test email to `fcoi@avagene.org` from your phone. It should land in your real inbox within a minute.

---

## Step 7 — Finalize the FCOI policy page  *(~3 min)*

Open `fcoi.html` (in GitHub, click the file → pencil icon to edit):

1. **Delete the orange draft banner** at the top — the entire `<div class="draft-banner">…</div>` block (around line 36–38)
2. Find `[INSERT EFFECTIVE DATE]` (around line 47) → replace with today's date (e.g., `April 17, 2026`)
3. Scroll to the bottom signature block, find `[INSERT DATE]` → replace with the same date
4. Commit changes — message: "Adopt FCOI Policy v1.0"

GitHub Pages auto-redeploys within a minute.

---

## Step 8 — Sign the Word version for your records  *(~2 min)*

The web version is the public-facing one. You also need a signed paper/PDF copy in your records:

1. Open `Avagene_FCOI_Policy_TEMPLATE.docx` (the one I generated for the JIT package)
2. Fill in the same effective date
3. Print, sign, scan
4. Save as `Avagene_FCOI_Policy_v1.0_signed.pdf` in your company records

---

## Step 9 — Verify everything is live  *(~2 min)*

Visit each in a fresh browser tab:

- [ ] `https://www.avagene.org` — home page loads, no certificate warnings
- [ ] `https://avagene.org` — redirects to `https://www.avagene.org`
- [ ] `https://www.avagene.org/fcoi.html` — FCOI policy loads, **no draft banner**, dates filled in
- [ ] Email test: send from your phone to `fcoi@avagene.org`, confirm it arrives

---

## Step 10 — Submit to NIH  *(~10 min)*

This is the JIT-relevant step.

1. **eRA Commons IPF Module** (NOT the JIT screen — different module)
   - Log in → Institution Profile → FCOI Policy
   - Paste: `https://www.avagene.org/fcoi.html`
   - Submit
2. **Complete NIH FCOI training** for Rupert: <https://grants.nih.gov/grants/policy/coi/tutorial2018/fcoi.htm> (~45 min, free, get the certificate at the end and save it to company records)
3. If you have any other senior/key personnel on the SBIR (you don't currently — you're solo at Avagene), they need to complete the same training before any PHS expenditures
4. Sign and date the FCOI Policy Word document for company records

---

## When something doesn't work

- **GitHub Pages shows 404 at the github.io URL after Step 2** → wait 2 more minutes, then hard-refresh. Sometimes the first deploy takes a beat.
- **DNS check in GitHub keeps failing after 30 minutes** → check that Cloudflare proxy is OFF (gray cloud, not orange) on all five records. This is the #1 cause.
- **HTTPS won't enable after 1 hour** → toggle Enforce HTTPS off, wait 10 min, toggle it back on. Forces re-provisioning.
- **Email forwarding test never arrives** → check your destination email's spam folder; Cloudflare sometimes lands in there on first send.
