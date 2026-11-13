# Trending Now page: design spec

A new screen for the Glow TV custom launcher (Onn 4K / Google TV).
Open `mockup.html` in a browser to see it working (arrow keys work like the remote).
This file describes the final design. Where the mockup and this file disagree, follow this file.

Everything in the mockup is placeholder data: titles, poster art, scores and cast.

## 1. Home screen stays as it is

The existing home screen (wallpaper, Home / All apps pills, weather + clock, "Your apps" row, per-app focus glow) is unchanged.
Nothing about trending appears on the home screen.

## 2. Opening and closing

- **Open:** pressing **D-pad down** while any app tile in "Your apps" is focused opens the Trending Now page.
  - It opens on the **Shows** tab, focused on the first trending title from the app that was focused (e.g. down on Disney+ starts on a Disney+ show). If that app has no titles (e.g. YouTube), start at the first item.
- **Close:** **Back** closes the page and returns focus to the same app tile.
- **Auto-close:** after **30 seconds** with no button press, the page closes by itself.

## 3. Layout

- The wallpaper stays visible behind the page but heavily **frosted** (blurred, slightly darkened, slightly more saturated).
- The page is one large **frosted glass panel** covering most of the screen (about 94% width, 90% height, centered), with rounded corners, a light 1px-ish white rim, an inner top highlight and a **soft outer glow in the current streamer's color** (see section 5).
- The home screen's pills and app row fade out while the page is open.

Inside the panel:

**Header row**
- Left: round app icon for the focused title's streamer, then the title **"Trending now"** with a small caps subtitle **"Across all your apps"**.
- Over the carousel: two glass pill tabs, **"▲ Shows"** and **"Movies ▼"**. The active tab is bright white with dark text and glows in the streamer color; the inactive tab is dimmed.
- Right: the current time.

**Left column (about one third of the width): info for the focused title**
In this order:
1. Streamer badge + "ON NETFLIX" (or Disney+, Prime Video, Apple TV, Plex)
2. Title (large display serif)
3. Facts row: content rating in an outlined box (TV-14, PG-13…), year, number of seasons (shows) or runtime (movies), TMDB rating
4. Two score chips: **Rotten Tomatoes %** and **Audience score %**
5. Genre tags as small pills tinted with the streamer color
6. Description (up to 4 lines, then truncate)
7. "Starring" + up to 3 cast names

A thin vertical divider separates this column from the carousel.
The info column cross-fades when the focused title changes.

**Right side: poster carousel (cover-flow style)**
- **Tall portrait posters** (about 2:3), like streaming app posters.
- The focused poster is in the center, large, facing forward, with a white rim and a **big soft glow in the streamer color** plus a drop shadow.
- Neighbors on each side are smaller (about 70%), angled away in 3D (about 55°), and dimmed. About 2 visible per side; further ones fade out.
- Side posters show artwork only (no text).
- The focused poster shows, over the bottom of the artwork: the **title** and one full-width **"▶ Watch now"** button.
  - The button is white with dark text and **glows in the same streamer color as the card** (colored rim + soft outer glow).
  - There is **no** "My List" button.
- Each poster has a small streamer badge in the top-right corner (N, D+, prime, tv, plex).
- Movement between posters animates smoothly (about 0.45s ease-out).

## 4. Controls (remote)

| Button | Action |
|---|---|
| ◀ / ▶ | Previous / next poster |
| ▲ | Switch to **Shows** tab |
| ▼ | Switch to **Movies** tab |
| OK | Open the focused title in its streaming app |
| Back | Close the page |

- Switching tabs slides the carousel in (down = from below, up = from above).
- Each tab **remembers its own position**: flipping to Movies and back returns to the same show.

## 5. Glow color follows the streamer

The accent color comes from the streaming app of the **focused title** and updates as you scroll.
It is used for: the panel's outer glow, the focused poster's glow, the Watch now button glow, the active tab glow, the genre tag tint and the header app icon.

| Streamer | Glow color |
|---|---|
| Netflix | `#FF1F2D` |
| Disney+ | `#3F8BFF` |
| Prime Video | `#22B5F0` |
| Apple TV | `#C9D3E6` |
| Plex | `#E5A00D` |

Color changes should transition smoothly (about 0.6s).

## 6. Data

- **Trending lists:** TMDB API, `trending/tv/week` for Shows and `trending/movie/week` for Movies. Free API key from themoviedb.org. Attribution to TMDB is required by their terms (e.g. a small logo or credit in Settings).
- **Where it streams:** TMDB "watch providers" for each title, for the user's country (US). Only keep titles available on apps installed on the TV (Netflix, Disney+, Prime Video, Apple TV, Plex). Mix them so the list alternates between streamers.
- **Details:** TMDB title details: content rating, runtime / number of seasons, genres, overview, top cast, poster image, TMDB rating.
- **Rotten Tomatoes and audience scores:** not in TMDB. Use OMDb (omdbapi.com, needs its own free key), looked up by the title's IMDb ID (TMDB `external_ids`). If no score is available, hide that chip instead of showing a blank.
- About 15 titles per tab is enough.
- Cache results (e.g. refresh a few times a day) so the page opens instantly.
- API keys must not be hard-coded in a public repo.

## 7. Opening a title (OK / Watch now)

Launch the title's streaming app. Deep-link to the exact title where the app supports it; otherwise open the app's search for the title, or just open the app.

## 8. Out of scope for this page

Clock style on the home screen is a separate decision (the mockup has a picker for it; it does not affect this page).
