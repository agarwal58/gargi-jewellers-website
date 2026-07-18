# Gargi Jewellers — Website

**Live site:** https://gargijewellers.in — "Maa Kali Kripa"
Sarafa Bazar, Lashkar, Gwalior 474001, Madhya Pradesh
Hours: 11:00 AM – 7:00 PM, closed Tuesday
Bhanu Pratap Agarwal +91 94253 39098 · Ravi Pratap Agarwal +91 94251 22341
Frames sub-brand: **Shivi Sales** (gold/silver god frames)

---

## How it is hosted

| Thing | Where | Notes |
|---|---|---|
| Hosting | **GitHub Pages** (this repo, `main` branch) | Settings → Pages → custom domain `gargijewellers.in`, Enforce HTTPS ON |
| Domain | Hostinger | DNS: A records for `@` → 185.199.108.153 / .109.153 / .110.153 / .111.153, CNAME `www` → `agarwal58.github.io` |
| Admin login | Decap CMS via `api.decapcms.org` OAuth | Owner logs in with GitHub at **gargijewellers.in/admin**. Netlify is NOT used anymore. |
| Contact form | **Web3Forms** | Enquiries arrive by email. The access key sits in `index.html` (safe by design — it can only send to the registered email). |

Every change (a commit, or an admin-panel edit) triggers a GitHub Pages rebuild — the live site updates in **1–2 minutes**, not instantly.

## Files in this repo

```
index.html              — the entire website (HTML + CSS + JS in one file)
CNAME                   — custom domain (NEVER delete; deleting unlinks gargijewellers.in)
admin/index.html        — loads the Decap CMS admin panel
admin/config.yml        — defines what the admin panel can edit
data/announcement.json  — top-of-site announcement banner (edit via admin)
data/gallery/*.json     — one file per gallery photo (created by admin panel)
data/team/*.json        — one file per person in the "Meet Us" section
images/gallery/         — gallery photos uploaded through the admin panel
images/team/            — "Meet Us" people photos uploaded through the admin panel
```

## What the owner can do at /admin

1. **Gallery Photos** — add a photo, give it a title, pick category `frames` or
   `jewellery`, upload the image (JPG/PNG preferred; HEIC also works — the site
   converts it in the browser). It appears in the matching Gallery album after
   the rebuild. Deleting the entry removes it from the site.
2. **Meet Us (People)** — add/edit the people shown in the "Meet Us" section:
   name, role, 10-digit mobile number, and an optional photo. Call and
   WhatsApp buttons are generated automatically from the number. Without a
   photo, a gold circle with the person's initial is shown.
3. **Site Settings → Announcement / Offer Banner** — toggle the maroon/gold
   banner at the very top of the site on or off, set its text, and optionally
   a link (e.g. `https://wa.me/919425339098`). Great for Diwali offers,
   new-collection announcements, holiday closures.

## How the moving parts work (for future developers)

- The gallery script in `index.html` lists `data/gallery/` through the GitHub
  API and appends one tile per entry to the `#frames-album` or
  `#jewellery-album` grid. Tiles are created dynamically — there is no slot
  limit. Bare filenames get prefixed with `/images/gallery/`, `.md` legacy
  entries with YAML front matter are still understood, and `.heic` images are
  converted to JPEG client-side with heic2any.
- The announcement banner script fetches `data/announcement.json` (cache-busted)
  and shows the bar only when `"enabled": true`.
- The Meet Us script works the same way with `data/team/` and appends one
  card per person; phone numbers are cleaned to digits and turned into
  `tel:` and `wa.me` links.
- The enquiry form POSTs to `https://api.web3forms.com/submit` with the access
  key, shows success only when Web3Forms confirms, and has a hidden honeypot
  field for spam.

## Things to NEVER do

- **Never delete the `CNAME` file** — the domain disconnects from the site.
- **Never re-enable Netlify Identity / Git Gateway** — deprecated, broken.
  Admin login works through GitHub + `api.decapcms.org`; no Netlify needed.
- Don't delete files from `images/gallery/` by hand — remove the photo from
  the admin panel instead, so the entry and image stay in sync.
- Don't rename this repo or the `main` branch without also updating
  `admin/config.yml` (`repo:` and `branch:`).

## Historical notes

- Originally hosted on Netlify with Netlify Forms + Netlify Identity;
  migrated fully to GitHub Pages in July 2026.
- A "Today's Rates" section existed once and was removed on the owner's
  request — do not re-add unless asked.
- BIS Hallmark registration is confirmed; the logo in the Why Us section
  is intentional.
- The Gallery has no hard-coded photos: every photo comes from the admin
  panel. Until photos are added, each album shows a "new photos are being
  added" note that disappears automatically once the first photo loads.
- Avoid stock/watermarked deity images (a "Visual Creation" watermarked
  Jagannath image was rejected earlier over copyright) — always use the
  shop's own photos of its physical frames.
