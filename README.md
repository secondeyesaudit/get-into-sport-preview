# Get Into Sport — production build

This folder is the real, deployable version of the Get Into Sport website. It's a
plain static site (no server, no database, no build step) — upload the contents
of this folder to any static host (Netlify, Vercel, Cloudflare Pages, GitHub
Pages, or ordinary FTP hosting) and it will work as-is.

## Files

- `index.html` — the whole site (Home, Contact, Camps, Lettings — one file,
  switched by JavaScript, see "Known architecture limitation" below).
- `privacy-policy.html` — a **draft placeholder** privacy policy, added
  2026-08-02 so the footer's Privacy Policy link resolves to something. It is
  clearly marked as a draft on the page itself and is not a real, legally
  sufficient policy — see "What still doesn't work" below.
- `assets/img/` — 20 real image files, extracted and deduplicated from the
  original demo build (see "Where this came from").
- `assets/gis-hero-video.mp4` — the hero background video.
- `favicon/` — full favicon set (ICO, PNG sizes, Apple touch icon), generated
  from the real official logo.
- `robots.txt`, `sitemap.xml` — SEO basics, already in place.
- `404.html` — a branded not-found page. Check your host's docs for how it
  wants this named/placed (some want `404.html` at the root exactly as here;
  others want it configured in a dashboard).

## Where this came from

There is a second, separate copy of this site at
`Clients/Get Into Sport/build/get-into-sport-cinematic.html` — that one is a
**demo-only** version where every image is embedded directly in the HTML as
base64 data. It exists purely because the chat preview tool used to build this
site cannot load local or relative image files, only fully self-contained
ones. **Do not deploy that file** — it's ~3.9MB and doesn't cache images
separately. This `build-production/` folder is the one to actually launch.

**If you need to make a text/copy/CSS change:** edit the demo file first (it's
easier to work with as one file), then regenerate this production folder by
re-running the extraction script that pulls the images back out. That script
lives in the session history this project was built in — ask whoever's
maintaining the site for it, or rebuild the folder by hand if needed (find
every `data:image/...;base64,...` string in the demo file, decode each unique
one to a file, and replace the data URI with a relative path).

## Known architecture limitation

This is a single HTML page with four sections shown/hidden by JavaScript —
there are no real separate URLs for `/camps`, `/lettings` or `/contact`. This was flagged
as a launch blocker in the original QA review: search engines can only index
one page, and the whole page's content ships on every load. It works, and it
looks right, but it isn't how a site this size should be built long-term. If
the business grows (more camps, more facilities, needs its own booking
dashboard), budget for a proper rebuild — a small framework with real pages,
or a CMS-backed site — rather than continuing to hand-edit this file.

## Placeholders still waiting on real values

Search `index.html` for these exact strings before launch:

- `G-XXXXXXXXXX` — Google Analytics 4 Measurement ID (2 places)
- `CLARITY_PROJECT_ID` — Microsoft Clarity project ID (1 place)
- `PLACEHOLDER_VERIFY_CODE` — Google Search Console verification code (1 place)
- `getintosportltd.com` — confirm this is the final domain everywhere it
  appears (canonical tag, Open Graph, Twitter Card, JSON-LD, sitemap.xml,
  robots.txt)

None of GA4, Clarity, or Search Console will do anything useful until their
real IDs replace the placeholders above. The cookie consent banner is already
wired to gate GA4/Clarity behind visitor consent, so nothing needs to change
there — just the IDs.

## What still doesn't work (do not launch without fixing these)

1. **Both enquiry forms don't send data anywhere.** They show a success
   message but the data goes nowhere. Needs a real destination (email, CRM,
   or a form-handling service) before launch.
2. **Booking is still not wired to a live provider.** The Camps page now has
   a branded "Book Now on ClassForKids" button and the Lettings page has a
   "Book Now" CTA leading to a Bookteq widget slot, but both point at
   placeholders (`href="#"` on Camps, an empty `.embed-slot` on Lettings) —
   see the `PLACEHOLDER:` comments next to each in `index.html`. Needs the
   real ClassForKids URL and the real Bookteq widget embed code once those
   accounts are confirmed live.
3. **The privacy policy is a draft placeholder, not a real one.** See
   `privacy-policy.html` — it's clearly marked as a draft on the page itself.
   Required by UK GDPR the moment any form collects personal data, and before
   GA4/Clarity go live for real.

Full detail on all of the above, plus everything else reviewed this session
(SEO, conversion, accessibility, and a full pre-launch checklist), is in the
`QA/` folder one level up from `build/` and `build-production/`.

## 2026-08-02 — client demo feedback build

Targeted build against confirmed feedback from a client demo walkthrough (see
`08_Meetings/2026-08-02 Client Demo Feedback.md` in the Second Eyes OS client
folder for the full brief). Summary: homepage reduced to hero + 3 CTAs only;
a new Contact page was split out to hold the enquiry form that used to live
on the homepage; the Camps page was restructured to About Us → Coaches →
Reviews → Booking; the Lettings page gained a top-of-page Book Now CTA and a
Bookteq widget placeholder plus an About Us / what-to-expect section; the
accent colour (`--rust`/`--rust-hi`) was re-hued from orange to a blue-green
placeholder pending real brand hex values; the footer gained a Legal column
with a Privacy Policy link and registered office details. Full change list in
the Second Eyes OS client folder's `00_Dashboard/CHANGELOG.md`.
