# RateAlbum

A lightweight, single-page album rating site hosted with GitHub Pages.

- **Live site:** https://youngyoungyoungyoungyoungyoung.github.io/album/
- **Repository:** https://github.com/youngyoungyoungyoungyoungyoung/album
- **Main app:** `index.html`
- **Last handoff:** November 25, 2026

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
- Notes belong to individual tracks, are optional, and are not included in shared links. Notes added to the public homepage feature are visible to visitors.
- Shared links contain the album and rating data needed for a read-only view. A visitor can copy the album into their own list to rate it.

## Design direction

The current direction is an album journal with black-and-green dark colors, a soft green-tinted light theme, and a vivid mint welcome panel. Keep both themes, with no background dots, gradients, glow, or thin divider lines between homepage sections. The first screen contains only horizontal navigation and a split-screen welcome with the exact headline “Albums, rated your way.” Scrolling reveals the owner’s featured album card, all track scores, supplied notes, and album average beside short explanations. The owner pick is labeled “Handpicked” / “Owner’s pick.” Follow with recently rated albums from this browser, excluding a duplicate of the featured pick. Album search and shared-rating name entry live on the Rate page; stats live on My ratings. The header stays at the top of the page with the theme switch and navigation together, then scrolls away with the page.

The featured card uses owner-maintained OWNER_FEATURED_PICK data: album ID or title and artist, optional cover URL, album average, and tracks containing name, score, and optional note. The current pick is “BAD ASS F*CKING KID” by Nettspend. The screenshot supplied the 15 track scores and 7.8 average. The “LAUGHIN” note is “Great beat but the flow was not it.” Update the owner setting and publish to change the public feature; the site is static, so a public on-page editor cannot publish changes for everyone. Notes added to this public featured card are visible to visitors. Sign in is a placeholder for now; real accounts are a later feature.

The homepage no longer falls back to an automatically chosen chart album. The left side of the welcome is a crisp, CSS-built listening journal card using the owner’s current album title, average, track scores, and note.

## How it is built

- One vanilla HTML/CSS/JavaScript file; no build step or framework.
- MusicBrainz is queried with `fetch`. Deezer is accessed through JSONP because of browser cross-origin restrictions.
- Cover Art Archive is a fallback for MusicBrainz artwork.
- Hash routes include album view (`#a=...`), shared view (`#s=...`), album comparison (`#v=...`), and the ratings list (`#ratings`).

## Recent changes

- **Popularity-ranked search:** resolve exact artist matches, collect Deezer artist albums and top tracks, rank releases using track popularity and album rank, and deduplicate results.
- **Album-only search:** filter singles while keeping EPs.
- **Track notes:** add an optional, per-track note stored privately with local ratings.
- **New home design:** replace the sidebar and first-screen chart card with a mint split-screen welcome, then a scroll-revealed owner-picked album card.
- **Journal home flow:** reveal owner track scores, selected notes, cover art, and album average with explanatory cards on scroll; follow with recently rated albums. Keep search on Rate and stats on My ratings.
- **Black-and-green UI:** recolor the dark theme, align the theme toggle, remove the topbar divider, and refine navigation, album detail, and rating dialog styling.
- **Direct score entry:** make the large score at the top of the rating dialog itself editable from 0 to 10 in one-tenth steps, alongside the slider, with no separate score box; keep the optional track note field full-width.
- **Featured notes:** support optional public per-track notes and calculate the album average from the supplied scores when no explicit average is set.
- **Theme and navigation:** move the light/dark switch into the top menu and apply both palettes to the whole page; hide the browser scrollbars while keeping scrolling enabled.
- **Homepage visual:** replace the low-resolution cropped listener photo with a sharp, responsive editorial scorecard built in HTML/CSS from the current owner-picked album and track notes.
- **Shared ratings:** ask for a username when creating a share link if none is saved, and label shared scores as listener ratings rather than official scores.
- **Header and score colors:** keep the full menu and theme switch together at the top of every page, then let them scroll away; distinguish best tracks in lime, worst and very low scores in red, and keep ordinary and unrated tracks neutral. Color the album average by its actual score on a red-to-amber-to-green scale. Align homepage explainer copy and recently rated covers.
- **Album vs album winner:** frame the winning album and place a tilted crown emoji over the cover, without adding a label that shifts the comparison layout.
- **Centered journal intro:** this was the previous layout; the scroll walkthrough replaces it.
- **Page-load repair:** commit [bff70da](https://github.com/youngyoungyoungyoungyoungyoung/album/commit/bff70dae82f238da5b6d081a9101df81dce247e1) removed literal `\\n` text that had accidentally entered a JavaScript statement and stopped the app script from parsing.

Relevant commits:

- [1ab9d1d — rank search by artist and Deezer popularity](https://github.com/youngyoungyoungyoungyoungyoung/album/commit/1ab9d1dbed3d91db143c138bbc9d92bd589faec7)
- [efd4365 — deduplicate popularity-ranked artist results](https://github.com/youngyoungyoungyoungyoungyoung/album/commit/efd436550cd3a44568d9816583d11221f560f394)
- [48d25d8 — filter singles and add private track notes](https://github.com/youngyoungyoungyoungyoungyoung/album/commit/48d25d8c1ce9659094b1f96bb5b575029c3581eb)
- [8404083 — include EPs](https://github.com/youngyoungyoungyoungyoungyoung/album/commit/84040831c2bcea5ccffa041f0df35f780e798b08)
- [bff70da — repair the page-loading script](https://github.com/youngyoungyoungyoungyoungyoung/album/commit/bff70dae82f238da5b6d081a9101df81dce247e1)

## Next session

1. Check the newest **Actions → Pages build and deployment** run on `main` after the latest commit.
2. Review the desktop and mobile split-screen welcome, repeatable scroll reveal, Rate page, My ratings stats, and dark/light themes.
3. Confirm the owner-picked card shows its cover, all 15 track scores, the “LAUGHIN” note, and 7.8 average.
4. Try an artist search such as “Yeat”; confirm popular Yeat albums appear first and singles are absent while EPs remain eligible.
5. Rate a track, add a note, and confirm it appears on recent album cards and remains private in shared links.

Start by reading this README and `index.html`, then continue from the checklist above.

