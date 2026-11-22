# CLAUDE.md

The **RoomIQ Meta Ads landing page**: the page every Meta ad lands on. RoomIQ is room-by-room underwriting for rent-by-the-room houses, sold to small co-living operators and the investors learning from them. This page sells one action per visit: a free forecast on an address, or the Co-Living Deal Readiness Scorecard (a separate tool the page links to).

It is its own Vercel project. Vercel serves the repo root as it is (`vercel.json`: no install, no build; `.vercelignore` keeps `CLAUDE.md` and `CHANGELOG.md` off the site), so the page lives at `/` on its own domain. Edit `index.html` here directly.

## Anti-regression

- Before any edit, pull the newest copy of the default branch (`git pull --ff-only`) and work on it. Never replace `index.html` with a copy from anywhere else (the main landing page, a zip, an earlier commit, a chat).
- Every change is a new version: raise `<meta name="page-version">` in `index.html` by one and add `## vN` to the top of `CHANGELOG.md`, in the same commit.
- Nothing on the page may disappear unasked. Things removed on purpose are listed in `REMOVED` in the GTM repo's `meta-launch/build/check.py`, which fails if any comes back.

## Layout

- `index.html`: the whole page. Its CSS (`<style>`) and script (`<script>`) are inside it. The site facts sit in `SITE` near the top of the script (`siteUrl`, `formEndpoint`, `metaPixelId`, `pricingUrl`, `scorecardUrl`, `vsl`, `coveredMarkets`, `seatsLeft`).
- `?v=` picks a whitelisted headline that matches the ad (`forecast`, `borrowed`, `rooms`, `supply`, `formula`, `str`, `next`, `score`); `?v=score` leads with the scorecard's buttons. The ads' website URLs use these names: never rename one without changing the ads.
- `media/`: the product previews (`forecast`, `market` and `baseline` screens, 320 to 1920 px) and the deal demo's three made-up houses (`media/demo/`), copied from the main landing page.
- **Screens zoom on their detail** (since v4, as on the main landing page). Each screen is an empty `<figure class="cf" data-screen='{...}'>` that the script builds from `SHOTS` (sizes, widths, image versions, placeholders and named boxes); `zoom` names the box to zoom to (`zoomNarrow` under 700 px), `zoomW` how much of the screen's width stays in view, `data-zoom-card` zooms inside a window, `start` begins zoomed, `alt` is the screen's alt text. Once a screen is in view a cursor (a touch mark on phones) clicks the detail and it zooms in; the button over it zooms out and in. The frame and zoom CSS and script, and the `SHOTS` entries, are copied from the main page: if they change there, copy them here.
- `fonts/`: Bricolage Grotesque (display) and Instrument Sans (text).
- `og.png` (the share image, 1200 x 630) and `favicon.svg`.

## Where it comes from

- **The main landing page** is `Saasrelaunchdev/RoomIQproductionlandingpage` (`index.html`). This page's shared CSS (tokens, base, calculator, form, dialogs, screen frames and zoom) and the zoom script are copied from it: change both. The "See Portfolio and Operator" link goes to the main page's pricing section through `SITE.pricingUrl`; it stays hidden until that is set.
- **The Deal Readiness Scorecard** is a separate tool, built on its own (removed from this page in v3, as from the main page in its v5). The four "score" buttons (hero link, the `?v=score` panel, the final section, the phone sticky bar on `?v=score`) link to it once `SITE.scorecardUrl` is set, campaign parameters carried. Until then they are placeholders (`href="#"`) and a click does nothing. The tool, not this page, sends `ScorecardStart` and `ScorecardComplete` to the same Meta dataset.
- **The ads, the VSL script and the plan** are in `Saasrelaunchdev/roomIQGTMBuild`, `meta-launch/` (`PLAN.md`, `README.md`). The VSL chapter times on this page come from `meta-launch/build/vsl_content.py`; if the script changes, update the chapter list here.
- **Check it** from `meta-launch/build` in the GTM repo, with this repo checked out next to it:
  - `python3 check.py ../../../roomiqmetaadspage` checks the copy budget, dashes, banned claims and names, that every figure in the ads and the VSL appears on this page, and that the VSL chapters match the script.
  - Serve this repo at port 8787 (`python3 -m http.server 8787`), then run `node qa/qa_meta.js`, `node qa/qa_track.js` and `node qa/qa_axe.js`.

## Non-negotiables

1. **Claims.** Only what the GTM repo (`GTM-BRIEF.md` Section 5) documents. No customer logos, counts, testimonials, ratings, outcome or saving figures unless measured, sourced and permitted. Unknown facts stay `{TBD · reason}` (shown with `?view=preview`, hidden in production).
2. **Compare structure and evidence, never people.** No competitor names (categories only). No host name, listing title or photo, exact address or named data-source platform. Every house, market and figure in the demos is made up and says so.
3. **Housing ad rules.** The campaign runs under Meta's housing special ad category: nothing on the page targets or segments by age, gender or ZIP.
4. **Tracking.** The pixel loads only after consent and honors Global Privacy Control. No personal data in events or URLs. Each browser event carries an `event_id` so the Conversions API copy is deduplicated.
5. **Screens are honest, without captions.** Since v2 there is no caption or note under any screen, illustration or visual (as on the main landing page since its v4; `check.py` in the GTM repo fails if one comes back). Honesty lives in the footer note (every screen is a product preview with demo data; demos and the video are made up) and in each image's alt text: keep both accurate when you change a screen. Never present a mockup as real.
6. **Copy budget.** Headline up to 8 words, the line under it up to 16, labels up to 5, other sentences up to 20, FAQ answers up to 35. No em dashes anywhere.
7. **Numbers agree.** Everything runs at 4.35 weeks a month, as the product prints. If you change a figure, change the arithmetic it comes from, and the ads and VSL that quote it.
8. **Phone first, WCAG 2.2 AA.** Check at 390 px and 1440 px, light and dark (`?theme=dark`), and with reduced motion (`?tier=c`). Keep keyboard and screen-reader support on every interactive part.
9. **Images are cached for a year.** Image URLs carry `?v=`; if you replace a file in `media/`, change its `?v=` value in `index.html`, or visitors keep the old one.

## Before launch (open)

- Set in `SITE`: `siteUrl` (this project's domain), `formEndpoint` (forecast request, seat application; it also sends the Conversions API events), `metaPixelId`, `pricingUrl`, `scorecardUrl`, `vsl` (once recorded), `coveredMarkets` and `seatsLeft`.
- Make the share image absolute (`og:image`) once the domain is set.
- Keep `noindex`: the page is for paid traffic.
