Make a demo video of a website redesign for The Inn at Sunrise Point, a 12-room
oceanfront inn in Lincolnville, Maine. The video is for the owners: they should
see the new site the way a guest would, with a smooth scroll through each page
and a few clicks on the interactive parts.

## Inputs (all in ./site/, ready to use)
- home.html, rooms.html, gallery.html, offers.html, story.html: desktop pages,
  designed for a 1440 px wide window
- home-mobile.html: the homepage at phone size, exactly 390×844, one screen
- logo.png (gold sun over "THE INN / AT SUNRISE POINT", dark text) and
  logo-light.png (same logo with cream text, for dark backgrounds)
- photos/: 12 real room photos, already placed in the pages
- index.html: a simple list of the pages, for browsing

The pages load two Google Fonts, Italiana (headlines) and Jost (text), so the
machine needs internet while rendering. Some spots are still labelled grey
placeholder boxes ("Photo coming soon", "Photo: Matt & MJ…"), and rates show as
[$___]. Leave all of those exactly as they are. Don't invent photos, prices or
copy, and don't edit the pages except to fix a rendering bug (say what you changed).

## Output (put everything in ./out/)
- sunrise-point-demo.mp4: 1920×1080, 30 fps, H.264 (yuv420p, +faststart), about
  90–120 seconds, no audio
- sunrise-point-demo-square.mp4: 1080×1080 cut of the same video, for texting
  and Instagram
- storyboard.png: one contact sheet with a frame from each section

## Method
- Serve ./site with a local static server (for example `npx http-server site -p 8080`
  or `python3 -m http.server`), never file://.
- Use Playwright with Chromium. Render desktop pages at a 1920×1080 viewport
  with deviceScaleFactor 1. The pages are fluid with a 1280 px content column,
  so they fill 1920 cleanly.
- Capture frames yourself with page.screenshot on a fixed 1/30 s time step, and
  drive scrolling with window.scrollTo inside that loop. Don't use recordVideo:
  frame-by-frame capture gives smooth, repeatable scrolls.
- Before capturing each page, wait for network idle, then
  `await page.evaluate(() => document.fonts.ready)`, then confirm
  `document.fonts.check('40px Italiana')` is true. If it's false, stop and tell
  me the fonts didn't load. Don't record with fallback fonts.
- Scrolls: ease-in-out cubic, no faster than about 900 px per second, a 1.2 s
  hold at the top of each page and a 0.8 s hold on each key section.
- Assemble with ffmpeg. Use 0.5 s crossfades between sections (xfade).

## If this is a Claude Code cloud session
- Playwright's own browser download may be blocked by the network allowlist. If
  `npx playwright install chromium` fails, install Chrome for Testing instead
  with `npx @puppeteer/browsers install chrome-headless-shell@stable` (it
  downloads from storage.googleapis.com) and pass its path to
  `chromium.launch({ executablePath })`.
- Get ffmpeg with `apt-get install -y ffmpeg`. If that fails, use the
  `ffmpeg-static` npm package.
- When the videos pass the checks, commit `out/sunrise-point-demo.mp4`,
  `out/sunrise-point-demo-square.mp4` and `out/storyboard.png` (not out/tmp) and
  push the branch, so I can download them from GitHub on my phone. Reply with a
  direct link to each file on GitHub.

## Shot list
1. Title card, 3 s: logo-light.png centred on #1B2A33 at about 420 px wide,
   then the line "A new website for The Inn at Sunrise Point" below it in
   Italiana, 56 px, color #EEF1EC. Fade in and out. Build the cards as small
   HTML pages and capture them the same way, so the fonts match.
2. Homepage (home.html), about 25 s: hold on the hero (the sunrise and the
   date picker), then scroll through: welcome from the hosts, "Cottages at the
   water's edge", "Every stay includes", the reviews, offers, "The Midcoast by
   season", the FAQ, and the footer.
3. Rooms & Cottages (rooms.html), about 20 s: hold on the header. Scroll until
   the filter buttons and first row of cards are in view, then click in order,
   holding 1.5 s after each: the "Cottages" pill
   (`[data-group="cottage"]`), then "Direct ocean view" (`[data-feat="direct"]`),
   then "All 12" (`[data-group="all"]`) and clear the ocean-view filter by
   clicking it again. Then scroll to the "Compare every room" table.
   Before each click, show a soft cursor dot (a 22 px #D49A2E circle at 70%
   opacity) gliding to the button over 0.6 s, injected as a fixed-position div.
4. Gallery (gallery.html), about 12 s: the same cursor treatment, clicking the
   tabs "Cottages" (`[data-tab="cottages"]`), "Main House" (`[data-tab="main"]`),
   "Baths" (`[data-tab="baths"]`), then "All" (`[data-tab="all"]`), holding
   about 1.5 s on each.
5. Offers (offers.html), about 15 s: scroll through the three packages, the
   Garden Spa price table, the extras, the elopement band and the gift
   certificate section.
6. Our Story (story.html), about 15 s: scroll through the hosts' intro,
   "A day at Sunrise Point", breakfast, the gardens and the press section.
7. On a phone (home-mobile.html), about 8 s: render at 390×844 with
   deviceScaleFactor 2. Place the screenshot inside a simple phone outline
   (rounded rectangle, 48 px corner radius, 14 px #1B2A33 bezel, soft shadow)
   centred on a #F2F3EF background at about 900 px tall. This page is a single
   screen, so don't scroll it: do a slow 3% zoom-in, then pulse the gold
   "Check dates & rates" button once with the cursor dot.
8. End card, 4 s: logo.png on #F2F3EF, with "Launching spring 2027" below it
   in Jost, 34 px, letter-spacing 0.2em, uppercase, color #1B2A33.

Before each page section (2 through 7), show a lower-third label for the first
2 s: "01 · Homepage", "02 · Rooms & Cottages", "03 · Gallery", "04 · Offers",
"05 · Our Story", "06 · On a phone". Style: Jost 28 px, uppercase,
letter-spacing 0.2em, color #EEF1EC, on a #1B2A33 band at 85% opacity,
bottom-left, 48 px from the edges. Add it as an overlay in the captured page,
not burned in afterwards, so it matches the fonts.

## Square version
For the desktop sections, crop the centre 1080×1080 of each 1920×1080 frame.
The content column is 1280 px wide, so the crop trims the edges of the nav and
of wide rows: that's fine, as long as no lower-third label is cut. Move the
labels inside the crop for this version if needed. For the
phone section and the title and end cards, re-render them at 1080×1080 instead
of cropping.

## Checks before you finish
- Run ffprobe on both videos and report duration, resolution and fps.
- Extract 10 evenly spaced frames from the main video and look at them. Fix and
  re-render if you see: a blank or half-loaded frame, a fallback serif instead
  of Italiana, a cut-off label, a filter or tab click that didn't change the
  grid, or the cursor dot left on screen between sections.
- Build storyboard.png from one frame per section, labelled.
- Tell me anything you couldn't do and why.
