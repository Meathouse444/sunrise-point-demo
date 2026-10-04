# Sunrise Point demo kit

Everything Claude Code needs to make a demo video of the website redesign.

## How to run it

1. **Unzip this folder** somewhere on your computer. (1 min)
2. **Open a terminal in the folder and start Claude Code** (`claude`). It reads
   CLAUDE.md automatically. (1 min)
3. **Type:** `Follow PROMPT.md and make the demo video.` (15–25 min, hands-off.
   It may ask to install Playwright and ffmpeg; say yes.)
4. **Watch `out/sunrise-point-demo.mp4`** and ask for changes in plain words, like
   "slow down the rooms filters" or "make the title card longer". (10 min)

To look at the pages yourself first, run `python3 -m http.server` in the
`site/` folder and open http://localhost:8000.

## What's inside

- `site/`: six pages (home, rooms, gallery, offers, story, home-mobile),
  the logo files, and 12 room photos from your Google Drive
- `PROMPT.md`: the full brief for the video
- `CLAUDE.md`: short standing rules Claude Code picks up automatically

## Still placeholders

Photos for Matt & MJ, breakfast, the gardens, the sailing package, and five rooms
(Russo, E.B. White, Longfellow, Wyeth, Thaxter). Rates show as [$___].
Drop new photos into `site/photos/` and ask Claude Code to swap them in.
