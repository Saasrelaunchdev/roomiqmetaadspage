# Changelog

Every change to the page is a new version: raise `<meta name="page-version">` in `index.html` by one and add an entry here, in the same commit.

## v4

- The screens zoom on their detail, as on the main landing page: once a screen is in view, a cursor (a touch mark on phones) clicks the detail and the screen zooms in on it, ringed, with no click from the visitor. A button over each screen zooms out and in again. With reduced motion (`?tier=c`) the screens start zoomed in.
- Ported main's screen frames and zoom (CSS, script, the zoom icons) and the `SHOTS` entries for the forecast, market and baseline screens. The steps zoom as on main: the market's occupancy legend, the bath mix, the scenarios. The hero zooms to the revenue card.
- The evidence section now shows the whole forecast screen, starting zoomed on its working (the board on wide screens, the revenue card on phones), as main's matching section does. The cropped revenue card it used before, and its images, are gone.
- Added the screen widths the frames load (320 to 1920 px), copied from the main landing page.

## v3

- Removed the in-page scorecard flow, as on the main landing page (its v5): the quiz dialog, its email gate and results, its script, styles, questions and tiers data, and its scoring arithmetic. The page no longer sends `ScorecardStart` or `ScorecardComplete`; the scorecard tool will.
- Kept the four buttons that opened it (hero link, the `?v=score` panel, the final section, the phone sticky bar on `?v=score`) as links. They go to the tool once `SITE.scorecardUrl` is set, campaign parameters carried; until then their href is the placeholder "#" and a click does nothing.
- The `?v=score` panel's classes are now `score-card` and `score-topics`.

## v2

- Removed every caption and note under a screen or visual, as on the main landing page (its v4): the five "Product preview · demo data" captions, the line under the video, the note under the cost list, the demo example line, "No accuracy claim. Working you can check." and the calculator disclaimer under the three steps.
- Honesty now lives in the footer (every screen is a product preview with demo data; houses, markets and figures in the demos and the video are made up) and in each image's alt text.
- `.vercelignore` keeps `CLAUDE.md` and `CHANGELOG.md` off the site.

## v1

- The Meta Ads landing page as its own Vercel project, moved from `meta/index.html` in `Saasrelaunchdev/RoomIQproductionlandingpage`.
