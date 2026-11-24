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

The refreshed direction follows the user's marked-up reference: a clean horizontal menu for Discover, My ratings, Album vs album, and a Sign in placeholder. The home page pairs album search with a featured popular recent album card. Use a flat midnight-blue palette in dark mode and a pale neutral palette in light mode. Keep both themes, with no background dots, gradients, or glow. UI text stays in English.

The featured pick comes from Deezer's current album chart, choosing the newest release among charted albums from roughly the last two years (or the newest chart entry when none meet that window). It shows the user's saved rating when one exists; otherwise it clearly says “Not rated yet” and offers to rate the album. Do not invent a score. Sign in is a placeholder for now; real accounts are a later feature.

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
- **New home design:** replace the left sidebar and record illustration with horizontal navigation, a prominent search field, a chart-picked album card, and flat dark/light themes without decorative glow or gradients.
- **Page-load repair:** commit [bff70da](https://github.com/youngyoungyoungyoungyoungyoung/album/commit/bff70dae82f238da5b6d081a9101df81dce247e1) removed literal `\\n` text that had accidentally entered a JavaScript statement and stopped the app script from parsing.

Relevant commits:

- [1ab9d1d — rank search by artist and Deezer popularity](https://github.com/youngyoungyoungyoungyoungyoung/album/commit/1ab9d1dbed3d91db143c138bbc9d92bd589faec7)
- [efd4365 — deduplicate popularity-ranked artist results](https://github.com/youngyoungyoungyoungyoungyoung/album/commit/efd436550cd3a44568d9816583d11221f560f394)
- [48d25d8 — filter singles and add private track notes](https://github.com/youngyoungyoungyoungyoungyoung/album/commit/48d25d8c1ce9659094b1f96bb5b575029c3581eb)
- [8404083 — include EPs](https://github.com/youngyoungyoungyoungyoungyoung/album/commit/84040831c2bcea5ccffa041f0df35f780e798b08)
- [bff70da — repair the page-loading script](https://github.com/youngyoungyoungyoungyoungyoung/album/commit/bff70dae82f238da5b6d081a9101df81dce247e1)

## Next session

1. Check the newest **Actions → Pages build and deployment** run on `main`, then open the live site and confirm it loads.
2. Review the new desktop and mobile navigation, flat dark/light themes, and Sign in placeholder.
3. Confirm the featured chart card loads a current album and handles a missing chart response with its search fallback.
4. Try an artist search such as “Yeat”; confirm popular Yeat albums appear first and singles are absent while EPs remain eligible.
5. Rate a track, add a note, close/reopen its rating dialog, and confirm the note persists. Check that the featured album shows a score only if it has a saved rating.

Start by reading this README and `index.html`, then continue from the checklist above.


