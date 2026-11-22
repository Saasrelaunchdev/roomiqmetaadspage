# CLAUDE.md

The **RoomIQ Meta Ads landing page**: the page every Meta ad lands on. RoomIQ is room-by-room underwriting for rent-by-the-room houses, sold to small co-living operators and the investors learning from them. This page sells one action per visit: a free forecast on an address, or the Co-Living Deal Readiness Scorecard.

It is its own Vercel project. Vercel serves the repo root as it is (`vercel.json`: no install, no build), so the page lives at `/` on its own domain. Edit `index.html` here directly.

## Layout

- `index.html`: the whole page. Its CSS (`<style>`) and script (`<script>`) are inside it. The site facts sit in `SITE` near the top of the script (`siteUrl`, `formEndpoint`, `metaPixelId`, `pricingUrl`, `vsl`, `coveredMarkets`, `seatsLeft`); the scorecard sits in `SCORE`.
- `?v=` picks a whitelisted headline that matches the ad (`forecast`, `borrowed`, `rooms`, `supply`, `formula`, `str`, `next`, `score`); `?v=score` leads with the scorecard. The ads' website URLs use these names: never rename one without changing the ads.
- `media/`: the product previews and the deal demo's three made-up houses (`media/demo/`), copied from the main landing page.
- `fonts/`: Bricolage Grotesque (display) and Instrument Sans (text).
- `og.png` (the share image, 1200 x 630) and `favicon.svg`.

## Where it comes from

- **The main landing page** is `Saasrelaunchdev/RoomIQproductionlandingpage` (`index.html`). This page's shared CSS (tokens, base, calculator, form, dialogs, scorecard) and the scorecard data are copied from it: change both. The "See Portfolio and Operator" link goes to the main page's pricing section through `SITE.pricingUrl`; it stays hidden until that is set.
- **The ads, the VSL script and the plan** are in `Saasrelaunchdev/roomIQGTMBuild`, `meta-launch/` (`PLAN.md`, `README.md`). The VSL chapter times on this page come from `meta-launch/build/vsl_content.py`; if the script changes, update the chapter list here.
- **Check it** from `meta-launch/build` in the GTM repo, with this repo checked out next to it:
  - `python3 check.py ../../../roomiqmetaadspage` checks the copy budget, dashes, banned claims and names, that every figure in the ads and the VSL appears on this page, and that the VSL chapters match the script.
  - Serve this repo at port 8787 (`python3 -m http.server 8787`), then run `node qa/qa_meta.js`, `node qa/qa_track.js` and `node qa/qa_axe.js`.

## Non-negotiables

1. **Claims.** Only what the GTM repo (`GTM-BRIEF.md` Section 5) documents. No customer logos, counts, testimonials, ratings, outcome or saving figures unless measured, sourced and permitted. Unknown facts stay `{TBD · reason}` (shown with `?view=preview`, hidden in production).
2. **Compare structure and evidence, never people.** No competitor names (categories only). No host name, listing title or photo, exact address or named data-source platform. Every house, market and figure in the demos is made up and says so.
3. **Housing ad rules.** The campaign runs under Meta's housing special ad category: nothing on the page targets or segments by age, gender or ZIP.
4. **Tracking.** The pixel loads only after consent and honors Global Privacy Control. No personal data in events or URLs. Each browser event carries an `event_id` so the Conversions API copy is deduplicated.
5. **Screens are honest.** Product previews are mockups of features RoomIQ really has, captioned as previews; never call one a real screen.
6. **Copy budget.** Headline up to 8 words, the line under it up to 16, labels up to 5, other sentences up to 20, FAQ answers up to 35. No em dashes anywhere.
7. **Numbers agree.** Everything runs at 4.35 weeks a month, as the product prints. If you change a figure, change the arithmetic it comes from, and the ads and VSL that quote it.
8. **Phone first, WCAG 2.2 AA.** Check at 390 px and 1440 px, light and dark (`?theme=dark`), and with reduced motion (`?tier=c`). Keep keyboard and screen-reader support on every interactive part.
9. **Images are cached for a year.** Image URLs carry `?v=`; if you replace a file in `media/`, change its `?v=` value in `index.html`, or visitors keep the old one.

## Before launch (open)

- Set in `SITE`: `siteUrl` (this project's domain), `formEndpoint` (forecast request, scorecard email, seat application; it also sends the Conversions API events), `metaPixelId`, `pricingUrl`, `vsl` (once recorded), `coveredMarkets` and `seatsLeft`.
- Make the share image absolute (`og:image`) once the domain is set.
- Keep `noindex`: the page is for paid traffic.
