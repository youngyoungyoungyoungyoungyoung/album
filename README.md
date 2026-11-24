# RateAlbum

A lightweight, single-page album rating site hosted with GitHub Pages.

- **Live site:** https://youngyoungyoungyoungyoungyoung.github.io/album/
- **Repository:** https://github.com/youngyoungyoungyoungyoungyoung/album
- **Main app:** `index.html`
- **Last handoff:** November 24, 2026

## What the app does

- Search for real albums and EPs, then rate tracks from 0 to 10.
- Calculate an album score as the average of tracks that have ratings.
- Add an optional private note to each track (up to 300 characters).
- Compare two albums, mark the best and lowest rated tracks, and share a read-only rating link.
- Use the site in English, with dark and light themes.

## Search behavior

- Search prefers an exact artist match and uses Deezer popularity signals to put that artist's releases near the top. For example, searching for “Yeat” should prioritize Yeat's releases over unrelated title matches.
- Search results are deduplicated by artist and album title.
- Singles are filtered out. Deezer results allow albums, EPs, compilations, and live albums; MusicBrainz fallback results allow albums and EPs.
- Deezer album rank and artist fan counts are shown when available. MusicBrainz provides fallback search results and tracklists.

## Ratings and notes

- Ratings and notes are saved in this browser's local storage under `rate2`; there is no account or server database.
- Notes belong to individual tracks, are optional, and are not included in shared links.
- Shared links contain the album and rating data needed for a read-only view. A visitor can copy the album into their own list to rate it.

## Design direction

The current direction is a clean album journal with a black-and-green dark theme and a soft green-tinted light theme. Keep both themes, no background dots, gradients, or glow. The home page opens on the headline and featured album, then reveals the journal introduction and recently rated records while scrolling. Each recent album can show private track-note excerpts. Album search and the shared-rating name field live on the Rate page; album, track, and average stats live on My ratings. The header uses a compact pill-style navigation and a single aligned theme icon.

The featured card supports an owner-maintained OWNER_FEATURED_PICK setting with the album, owner score, and per-track scores. It displays the pick as “Curator’s pick.” Update this setting and publish to change the public feature; the site is static, so a public on-page editor would not safely publish updates for everyone. Until an owner pick is configured, the featured card uses the existing chart selection. Never invent album or track scores. Sign in is a placeholder for now; real accounts are a later feature.

The featured-card parser accepts the chart response as either a direct `data` list or the wrapped `albums.data` form. If no valid chart entries are returned, it falls back to a search prompt; do not invent a score or hard-code a stale album.

## How it is built

- One vanilla HTML/CSS/JavaScript file; no build step or framework.
- MusicBrainz is queried with `fetch`. Deezer is accessed through JSONP because of browser cross-origin restrictions.
- Cover Art Archive is a fallback for MusicBrainz artwork.
- Hash routes include album view (`#a=...`), shared view (`#s=...`), album comparison (`#v=...`), and the ratings list (`#ratings`).

## Recent changes

- **Popularity-ranked search:** resolve exact artist matches, collect Deezer artist albums and top tracks, rank releases using track popularity and album rank, and deduplicate results.
- **Album-only search:** filter singles while keeping EPs.
- **Track notes:** add an optional, per-track note stored privately with local ratings.
- **New home design:** replace the left sidebar and record illustration with horizontal navigation, a chart-picked album card, and flat dark/light themes without decorative glow or gradients.
- **Journal home flow:** keep the opening view focused on the hero and featured album, reveal the journal intro and recent ratings as the visitor scrolls, show track note previews, move album search and shared-rating name entry to Rate, and move rating stats to My ratings.
- **Black-and-green UI:** recolor the dark theme, align the theme toggle, remove the topbar divider, and refine navigation, album detail, and rating dialog styling.
- **Direct score entry:** allow decimal ratings from 0 to 10 in one-tenth steps alongside the slider, and make the optional track note field fill the rating dialog.
- **Page-load repair:** commit [bff70da](https://github.com/youngyoungyoungyoungyoungyoung/album/commit/bff70dae82f238da5b6d081a9101df81dce247e1) removed literal `\\n` text that had accidentally entered a JavaScript statement and stopped the app script from parsing.

Relevant commits:

- [1ab9d1d — rank search by artist and Deezer popularity](https://github.com/youngyoungyoungyoungyoungyoung/album/commit/1ab9d1dbed3d91db143c138bbc9d92bd589faec7)
- [efd4365 — deduplicate popularity-ranked artist results](https://github.com/youngyoungyoungyoungyoungyoung/album/commit/efd436550cd3a44568d9816583d11221f560f394)
- [48d25d8 — filter singles and add private track notes](https://github.com/youngyoungyoungyoungyoungyoung/album/commit/48d25d8c1ce9659094b1f96bb5b575029c3581eb)
- [8404083 — include EPs](https://github.com/youngyoungyoungyoungyoungyoung/album/commit/84040831c2bcea5ccffa041f0df35f780e798b08)
- [bff70da — repair the page-loading script](https://github.com/youngyoungyoungyoungyoungyoung/album/commit/bff70dae82f238da5b6d081a9101df81dce247e1)

## Next session

1. Check the newest **Actions → Pages build and deployment** run on `main`, then open the live site and confirm it loads.
2. Review the desktop and mobile home hero, scroll reveal, Rate page, My ratings stats, and flat dark/light themes.
3. Confirm the featured chart card loads a current album and handles a missing chart response with its Rate page fallback.
4. Try an artist search such as “Yeat”; confirm popular Yeat albums appear first and singles are absent while EPs remain eligible.
5. Rate a track, add a note, and confirm the note appears on recent album cards and remains private in shared links.

Start by reading this README and `index.html`, then continue from the checklist above.


