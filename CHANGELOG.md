# Changelog

All notable changes to *Kitly* – newest version first. These entries are also available directly in the tool via the **⚽** version badge.

Named minor versions (with a codename) are feature releases; patches cover bug fixes and polish.


## v6.15.0 — 2026-11-16 — "Anzeigetafel"

- **A progress bar replaces the plain "Loading data" text.** Below the bouncing ball the overlay now shows the current step ("Step 2 of 3: Categories"), a bar, a detail line (which title search or category, how many files so far, "n of total" while upload dates are checked) and the time elapsed.
- **Estimated time left.** Calculated from elapsed time and progress, shown from about 10 % onwards and rounded to 5 seconds; before that the overlay says it is still estimating. It is an estimate, not a measurement: the five title searches have no fixed number of pages (each stops once a page holds nothing new), so the figure can jump early on. The bar is a weighted share across the steps of the chosen source (Commons search, PetScan, or both).
- **Cancel button.** Stops the retrieval at once, including a request still in flight or waiting after a rate limit, and leaves the list from the previous retrieval untouched: no error message, no notification, and the "since last visit" day is not updated. Background work such as the usage scan is not affected.
- Keyboard: focus moves to the Cancel button while the overlay is open and returns to where it was afterwards. Screen readers announce the step line.
- Internal: the shared API helper accepts an optional abort signal, and the error handlers that swallow a failure to keep a partial result now let a cancellation through.

## v6.14.0 — 2026-09-14 — "Nachspielzeit"

- **Colours are cached per file version, not per filename.** Replacing a file on Commons with a corrected version left the old colour in place until local storage was cleared. The key carries the upload timestamp now. The previous colour cache (`kitColorCache_v1`) is removed on the first start of this version: it is a pure derived cache, holds nothing you entered, and is rebuilt within seconds. Nothing else in local storage is touched, and no completion data is affected.
- **The usage scan reads its own cache.** It has written it since v6.13.0 but never read it, so every reload fetched everything again although the data is valid for 24 hours. A second run over the same files costs no requests at all now.
- **The first-upload date of files with more than 50 versions was wrong.** A batch query returns at most 50 versions per file and offers no continuation marker, so the oldest version returned was taken for the original upload. Affected files are fetched individually now. This matters for the "hide file updates" filter, which classified those files as new uploads.
- **The category walk stops at the limit.** Each of the eight kit categories used to page through up to 10,000 entries even with the limit set to 100. It stops once the limit is reached; a category is ordered by the time entries were filed, newest first, so what drops out was filed after the limit's worth of other entries in the same category.
- **A batch whose usage list ran into the API budget is split completely.** Until now only files that came back empty were re-fetched. The file at which the budget runs out gets a truncated list rather than an empty one and was waved through as complete. The split also goes deep enough to reach individual files, so their result is cached instead of being fetched anew every time.
- Fix: long goalkeeper socks (`gkhl`, `gkal`, `gktl`) were not recognised as long, so they appeared next to the short ones instead of replacing them.
- Fix: `.tif`, `.tiff` and `.webp` were missing from the filename analysis and from the pattern token of the infobox code, so the extension stayed in the name and broke kit-type and season detection for those files.
- Fix: clicking "Fetch data" twice no longer starts a second retrieval alongside the first, where whichever finished last won regardless of the filters it had started with.
- Fix: the PetScan query passed the namespace as `ns=1`, which is not a valid parameter and was ignored. It is `ns[6]=1` now. Checked live: the result is identical either way, because the kit categories contain practically nothing but files.
- The statistics line says "completed in total" for the tick counts; they cover everything ever ticked, not only the current view.

## v6.13.2 — 2026-09-14 — Bug fix

- Fix: **the error message from a failed retrieval was overwritten immediately.** Since v6.13.1 the redraw ran after the message had been set, so what stayed on screen was "No teams found" rather than the reason. Introduced in v6.13.1, fixed here.
- Fix: **the post-render timer existed twice.** The wrapper from v5.2 survived alongside the shared timer introduced in v6.13.1, so every redraw scheduled two passes. The wrapper is gone; the five call sites schedule the pass themselves. Introduced in v6.13.1, fixed here.
- Fix: the **season filter** no longer narrows its own list of options. Picking a season used to drop every other season from the dropdown, so getting back meant resetting the filter.
- Fix: **numbering in the generated infobox code.** Kit parts with a fixed number could collide with automatically numbered ones, which produced two kits sharing a number.
- Fix: a failed colour analysis is no longer cached. Until now one failed image load stored an empty colour for that file for the rest of the session.
- Fix: the kit-set heading no longer runs underneath the tick button, and in light mode links in the card header keep a readable hover colour.
- Accessibility: the tick buttons carry a label and the checkbox role in every view, the three dialogs announce themselves as dialogs, keep the focus inside while open and hand it back on close, and the version badge is a real button reachable by keyboard.
- Internal: retries and error handling for all API calls sit in one place now; dead code and 15 unused translation keys removed; several stale comments corrected.

## v6.13.1 — 2026-09-14 — Bug fix

- Fix: **usage figures were wrong and stayed wrong.** The usage query asks for up to 500 entries, but that budget applies *per request*, not per file. One heavily used file (`File:Kit body.svg` has tens of thousands of usages) consumed the entire budget, so every file behind it in the same batch came back empty – and Kitly cached that emptiness for 24 hours. It now follows the API's continuation, splits a batch that hits the budget, and caches a file only once its list is provably complete.
- The usage cache key moves to `usage_v3_`; the previous entries are dropped rather than reused, since they may contain exactly the incomplete data described above. Nothing else in local storage is touched and no completion data is affected.
- Fix: **a failing part of the retrieval no longer discards the rest.** If one of the five title searches, the category walk or one of the two sources fails, Kitly keeps what the others delivered and reports how much is missing. API errors that Commons returns with HTTP 200 and an error body are recognised as errors now, and only genuinely retryable failures (429, 5xx, network) are retried, respecting `Retry-After`.
- Fix: **ticks no longer get lost through the spelling of a filename.** Commons treats `Kit body x.png` and `Kit_body_x.png` as the same file; Kitly stored them under two keys, so a tick set in one view could be invisible in another. Both spellings are read now, and unticking clears every variant. Stored data is left untouched.
- The **"Depth"** field is gone from the interface. The PetScan category crawl now always runs at maximum depth. The setting had no useful case: the kit-template category tree was already covered in full, so any lower value could only lose hits. The functionality itself is unchanged.
- Fix: an unparseable date no longer sinks a kit set to the bottom of the list. Invalid dates are ignored both for the "newest" date and for sorting.
- Fix: a season written two ways in filenames (`16` and `2016`) produced two groups under one and the same heading. Both now land in the same season group; genuine spans such as `1617` stay separate.
- Fix: loosening a filter after a usage scan re-scans the files that were not covered before, instead of showing them with an empty usage list.
- Fix: a refresh started while a usage scan is still running no longer lets the old scan write its results into the new list.
- Fix: if a retrieval fails, the list from the previous retrieval is cleared instead of remaining on screen underneath the error message.
- Fix: if the browser refuses the automatic copy of the infobox code (Safari, after the colour analysis), the code now appears in a dialog ready to be copied by hand, instead of only in the console.
- Fix: in the retrieval panel, the data source and the limit sat lower than the date field, because the whole row was aligned to the bottom edge of the "Since last visit" button. The three fields line up again now, with the button below the date. That button also had its dark-mode surface in light mode, where it showed up as a grey block with white text.
- Fix: the hover colour of the view toggle no longer turns the label white on a light background; the tooltips of two filter labels are translated as well.
- Internal: one shared post-render timer instead of one per render, and filenames containing quotation marks are escaped consistently in the generated markup.

## v6.13.0 — 2026-09-14 — "Flügelspiel"

- New: a third data source, **Both (Commons + PetScan)**. It runs the Commons search first, then adds whatever PetScan knows on top, merging the two by filename so nothing appears twice. If PetScan fails or is unreachable, the Commons result is still shown together with a note, instead of the whole retrieval failing.
- New: the Commons source no longer depends on the filename scheme alone. On top of the five title searches it now walks the kit categories **live** via the Commons API (`list=categorymembers`, newest first, bounded by the chosen date), which brings PetScan's coverage without the Toolforge replica. Worth knowing: freshly uploaded kit files are frequently not categorised at all yet, so the title search still carries most of the load and the category walk is a complement, not a replacement.
- New: a **"Since last visit"** button below the date field. Kitly remembers the day of the last successful retrieval and sets the date to it on one click, then refreshes. The button only shows up when there is something to jump back to.
- The **usage cache** is bounded now: one key per file instead of up to three spellings of the same name, at most 400 entries, oldest pruned first. If local storage runs out regardless, Kitly shows a visible warning rather than swallowing the quota error.
- Fix: in the **Kit sets** view a kit set holding both short and long socks now shows the long ones only, matching the behaviour the Teams view already had.
- Fix: switching the language now relabels the **Season** and **Kit type** dropdowns as well; they used to keep the previous language until the next refresh.
- Fix: the changelog and usage dialogs referenced two CSS variables that were never defined, which could produce white text on a white background in light mode.
- Fix: a ticked team button rendered **two check marks**, and it followed the operating-system colour scheme directly instead of the theme setting, so it looked wrong when the theme was overridden.
- Fix: the filter panel could cut off its content on short windows; it scrolls now.
- Internal: filename analysis is cached per retrieval instead of being recomputed for every file on every render, and `readControls()` no longer writes to the date field as a side effect of being read.

## v6.12.1 — 2026-09-14 — Bug fix

- Fix: the file meta line showed `undefined Bytes` for the Commons source, which, unlike PetScan, returns no file size with the result rows. The size is now taken from the `imageinfo` query and omitted entirely when unknown.
- Fix: the limit warning fired almost every time with the Commons source. It compared the number of scanned search hits against the limit rather than the number of files actually kept.

## v6.12.0 — 2026-09-14 — "Doppelpass"

- New: **selectable data source** under "📥 Data retrieval". **Commons search (live)** is the new default and queries the Commons API directly, so it always sees today's uploads. **PetScan (categories)** remains available and still finds kit parts whose filename breaks the usual scheme, but it reads a Toolforge database replica: while that replica lags behind, PetScan returns nothing for the most recent days. That is what made Kitly appear to stop finding new kits. The Commons source matches on the title (`Kit body` / `Kit shorts` / `Kit socks` / `Kit left arm` / `Kit right arm`) and therefore reports new files reliably, new versions of older files only incidentally. The "Depth" field applies to PetScan only and is hidden for the Commons source.
- New: a **warning when the source looks stale**. If the newest hit is more than a day behind the current date, Kitly says so instead of just showing an empty list.
- Fix: **ticking a team no longer hides its future kits.** The team tick was stored under the club name alone, without a season or a timestamp, so once "Chelsea" had been ticked every Chelsea kit of every later season stayed hidden, including files that did not exist when the tick was set. A team's state is now derived from its kit sets and is no longer stored separately.
- Fix: **a tick in the Teams view and a tick in the Kit sets view are now the same tick.** The two views built different key formats for the same kit set (`team-season-type` versus `team_season_type`), so a kit set ticked in one view still appeared unticked in the other.
- New: a ticked kit set **reopens when a new part shows up**, and a ticked file reopens when a new version is uploaded. Ticks record what they covered instead of hiding whatever later matches the key.
- Data migration: completion data moves from `kitStorage_v5` to `kitStorage_v6` on first start. Kit-set and file ticks are carried over, team ticks are dissolved into the files they covered. Nothing is deleted: the previous state is kept under `kitStorage_v5_backup`, and the result of the migration is reported once in the header.
- Robustness: Commons API calls retry with escalating backoff on HTTP 429 and 5xx (honouring `Retry-After`) instead of silently dropping part of the result.
- Changed: the "kit sets completed" figure replaces "teams completed" in the statistics line, matching the new storage model.

## v6.11.1 — 2026-09-01 — Bug fix

- Fix: the second Copa Libertadores spelling with a `_wc_la` suffix, e.g. `Kit_body_corinthians13H_wc_la.png`, is now parsed correctly – team "Corinthians", season "2013", type "Copa Libertadores home" – instead of team "Corinthians13H_wc" with no season. Role `H`/`A`/`T` optional (defaults to home).
- New: a **warning when the result limit is reached**. If PetScan returns exactly as many hits as the limit allows, further files in the period were silently cut off – Kitly now says so and suggests raising the limit or narrowing the date range.

## v6.11.0 — 2026-08-31 — Search notifications

- New: optional **notification when a refresh finishes**. Enable it with the 🔔 button in the header. Where the browser allows it you get a real desktop notification; as a permission-free fallback that works everywhere (including local `file://` and other tabs) the **browser-tab title flashes** "✅ Kitly" until you return. Reports how many new kit files were found.
- Polish: the "Loading data…" overlay now has a small animation – a bouncing, rolling football with animated dots.

## v6.10.3 — 2026-08-31 — Transparent kit type

- New: the `tr` kit-type suffix, e.g. `Kit_body_kashima26tr.png`, is now labelled "Transparent" (team "Kashima", season "2026") instead of the raw code "tr".

## v6.10.2 — 2026-08-30 — Copa Libertadores kits

- New: recognizes the Copa Libertadores naming scheme `<team>L<role><yy>`, e.g. `Kit_body_gremioLT17.png` → team "Gremio", season "2017", type "Copa Libertadores third". Role `H`/`A`/`T` = home / away / third; a trailing `_2` version counter is ignored.

## v6.10.1 — 2026-08-20 — Bug fix

- Fix: file names with an ordinal kit number after the season, e.g. `Kit_body_bohemians2026-3rd.png`, are now parsed correctly – team "Bohemians", season "2026", type "Third" – instead of team "Bohemians2026-3" with no season. Recognizes 1st/2nd/3rd/4th (home / away / third / fourth).

## v6.10.0 — 2026-08-18 — Categories

- New: below the "Used in" line, each kit set now also lists the **Commons categories** of its files (deduplicated, named once even when shared by several parts), with links to the category pages. Only visible content categories are shown; hidden license/maintenance categories are omitted.
- Fix: file names with a trailing tag after a compact season, e.g. `Kit_body_cisf16h_recopa.png`, are now parsed correctly – team "Cisf", season "2016", tag "recopa" – instead of team "Cisf16h" with no season.

## v6.9.1 — 2026-08-11 — Bug fix

- Fix: the "All done" hint counted teams together with their nested kit sets (e.g. "16" for 7 teams). It now counts only the top-level entries and names them per view: "All N teams / kit sets / kit parts completed".

## v6.9.0 — 2026-08-11 — Code review fixes

- New: the file meta line now also shows the **author** (uploader of the oldest version), e.g. „274 Bytes · 11.08.2026 · JonasBR“.
- New: **„Reset filters“** button (below the filter groups) restores the filter/display options to their defaults.
- Fix: the **kit type dropdown** no longer loses its other options after a selection.
- Fix: the country chip **counts** in „Hide if used in“ are no longer inflated.
- Fix: the **season filter** is now restored after a reload (like the kit type filter).
- Fix: the „All done“ hint now also appears in the Teams view.
- Hardening: file names are no longer written into inline event handlers (delegated click via `data-file`), and page titles/URLs in the usage modal are HTML-escaped – closing a narrow injection surface from external file names. A failed cache write no longer turns a successful fetch into an error.
- Housekeeping: the usage cache is pruned (entries older than 24 h) and the color cache is capped, so `localStorage` no longer grows without bound. Removed a batch of dead CSS and stale code.

## v6.8.3 — 2026-08-11 — Anniversary editions

- Anniversary/jubilee kits with a `<years>y` suffix (e.g. `Kit_body_djurgardens135y.png`) are now recognized: team = „Djurgardens“, type = „135-year edition“, no season – instead of team „Djurgardens1“, „Season 1935“ and type „Y“. Also works for 50y, 100y, 150y …

## v6.8.2 — 2026-08-11 — English by default

- **English is now the project's main language.** The interface still auto-detects your browser language and switches to German for German browsers, but the default and fallback is now always English (previously German).
- The GitHub docs (README and this changelog) are now in English.

## v6.8.1 — 2026-08-10 — Hyphenated season on the team

- Filenames with a hyphenated season attached directly to the team (e.g. `Kit_body_cskasofia26-27a.png`) are now recognized correctly: team = „Cskasofia“, season = „2026/27“ (instead of team „Cskasofia26-“ and „Season 2027“).

## v6.8.0 — 2026-08-09 — „Einsatz"

- New: **Usage shown directly in the kit set**. Below each kit set there is now „Used in: …“ with links to the pages where the parts are already embedded – without clicking 📊. The details of all parts are combined and deduplicated.
- In front of each page is the **country flag** of the wiki language variant (for Commons the **Commons logo**; without a matching flag the language code). Order: **de → en → pt → Commons → rest**.
- Loaded **as you scroll** (only visible kit sets are queried) and cached for 24 h – so a later click on 📊 is instant.
- New toggle **„Show usage“** (on by default) to switch it off/on.
- New filter **„Hide if used in …“**: activate one or more wikis via flag chips; every file already embedded there is hidden. This shows at a glance which kit parts are still missing in, for example, the German Wikipedia. Sets/teams with no remaining parts disappear automatically. (For this it checks the usage of all matches once, with a progress indicator and 24-h cache.)
- Fast & gentle: The usage scan runs **in batches** (up to 50 files per request instead of one per file) in the background – the interface appears immediately. In addition, **the thumbnail loader retries failed requests**, so previews are preserved instead of falling back to „No preview“.

## v6.7.0 — 2026-08-09 — „Neuzugang"

- New: **„Hide file updates“** (on by default). Files that only received a *new version* within the selected period, but whose **first upload** predates it, are hidden – so only genuine new uploads remain. Toggleable without reloading.
- Foundation: For each file, the **first upload** is now additionally determined via the MediaWiki API (oldest file version), not just the most recent upload.
- The **limit** can now be set up to **10000** (previously 2000; default stays 1000) – for wide time spans where matches would otherwise be cut off.

## v6.6.0 — 2026-08-09 — „Kitly"

- The tool is now called **Kitly** – a name that also works internationally.
- New: **DE/EN language switch** (🌐 button). The entire interface is now available in German and English; the language is saved and, on first launch, guessed from the browser language.
- New: **Light/dark mode with automatic option** (🌗 button, Auto / Light / Dark). „Auto“ follows the operating system; the green pitch identity is preserved in both modes. The choice is saved.
- New: Clicking a **thumbnail** opens the Commons file page in a new background tab.
- Season spans across multiple years are now shown as a range: `8792` → „Season 1987–1992“ (instead of „Season 8792“), while consecutive seasons still appear as `1987/88`.
- Counters now use the correct singular/plural („1 team found“, „1 part“ or „1 team found“, „1 part“).

## v6.5.1 — 2026-08-09 — „Feinschliff"

- Season spans recognized correctly: `1819`/`1920`/`2021` are now shown and sorted as „2018/19“/„2019/20“/„2020/21“ – genuine single years (1923, 1975, 2026) stay unchanged.
- Verbose filenames: a standalone year (e.g. `FC_Tokyo_2026_HOME`) is no longer taken into the team name.
- The kit set view shows the season in plain text (`2627` → „2026/27“).
- The kit type filter also works reliably for goalkeeper variants (`gkd`/`g`).
- Statistics no longer count files in hidden teams as visible; check-off buttons are updated with debouncing (less DOM load on large lists).

## v6.5.0 — 2026-08-09 — „Aufgeräumte Kabine"

- New: **Kit type filter** (dropdown home/away/third/goalkeeper) in the filter group.
- The two parallel check-off systems were merged into **one** (a single store). Checking off teams, kit sets, and files now runs through the same logic.
- 2-digit seasons are shown correctly: `02` → „Season 2002“.
- Cleaned up: dead auto-refresh code, orphaned helper functions, one unused access, and all debug output (console.log) removed.

## v6.4.0 — 2026-08-08 — „Chronik"

- All seasons of a team are now shown under **one** team card, sorted chronologically (newest season on top). Example: „Gelp“ with 1996/98 above 1993/95.
- When a team is checked off as completed, **all** seasons of that team disappear with it.
- The „Infobox code“ button now sits **per season** (instead of per team), so the kit numbers don't collide across seasons.

## v6.3.11 — 2026-08-08 — Season filter aligned

- The season filter now shows exactly the same season as the team display (shared logic). Unusual spans like „1993/95“ or „1998/2001“ appear in the dropdown just as they do on the teams.

## v6.3.10 — 2026-08-08 — Season spans

- Non-consecutive season spans are now shown correctly: `9395` → „1993/95“. At a century boundary the end year is shown with 4 digits: `9801` → „1998/2001“, `9900` → „1999/2000“. Genuine single years (1923, 1975) stay unchanged.

## v6.3.9 — 2026-08-08 — Hyphenated seasons

- Filenames with a separator season like `Kit_body_spartak-vn-26-27-a.png` are now read correctly: team („spartak-vn“), season („2026/27“) and type (away). Home and away land in the same team. Supports „YY-YY“, „YYYY-YY“ and single 4-digit year blocks.

## v6.3.8 — 2026-08-08 — Infobox numbering

- The „Infobox code“ now numbers the kits by type instead of by order: home = 1, away = 2, third = 3, fourth = 4. So an away-only set correctly yields `…2` (instead of incorrectly `…1`).

## v6.3.7 — 2026-08-08 — Feinschliff

- In the team row the kit types are now sorted consistently: home, away, third, then goalkeeper – matching the order of the kit set cards.

## v6.3.6 — 2026-08-08 — Verbose filenames

- New naming scheme recognized: `Kit_body_FC_Tokyo_2026_-_27_AWAY_FP.png` & co. Team („FC Tokyo“), season („2026/27“) and type are now read correctly; home and away land in the same team.
- `FP` = field player (→ home/away/third), `GK` = goalkeeper (→ goalkeeper home/away). Previously „fp“ was shown as the type and the whole name as the team.

## v6.3.5 — 2026-08-03 — Goalkeeper home/away

- Goalkeeper kits are now distinguished by home/away: `gkh` → „Goalkeeper home“, `gka` → „Goalkeeper away“ (also `gkt` → „Goalkeeper third“). Applies to the display, kit set titles and the kit type filter.

## v6.3.4 — 2026-08-03 — Bug fix

- Files with a 4-digit year and no type code (e.g. `Kit_body_River_1975.png`) are now recognized correctly: season = 1975, type = home (no „h“/„a“ at the end means home kit). Previously they ended up under „No season • unknown“.

## v6.3.3 — 2026-08-03 — Feinschliff

- The „Limit“ (max. 2000) and „Depth“ (max. 100) input fields now have upper bounds, matching the limits of the kitproxy server – so the limit is visible directly in the field.

## v6.3.2 — 2026-08-03 — Cleanup

- Category query streamlined: „Association football kit templates“ removed – it is a direct subcategory of both parent categories and is included anyway at depth 50 (verified empirically, result set unchanged).

## v6.3.1 — 2026-08-03 — „Ordnung"

- Completed teams are now shown/hidden via just **one** route: the „Show completed“ button. The duplicate checkbox in the filter group has been removed.

## v6.3 — 2026-08-03 — „Ordnung"

- The settings are now grouped by topic: **Data retrieval** (date, limit, depth), **Filter** (season, kit parts only, completed teams) and **Display** (view, thumb width).
- The „Completed teams“ checkbox is now a compact inline element (no longer a big box) and matches „Kit parts only“.

## v6.2.5 — 2026-08-03 — Header area

- Consistent appearance: the global input base (previously a bold 2px border) is now subtle like the filter fields.
- The „Kit parts only“ and „Show completed teams“ checkboxes now match the new filter look in color and size.

## v6.2.4 — 2026-08-03 — Header area

- The top area looks tidier: slimmer, more subtle filter fields (fine border instead of bold white, with a soft focus ring) and more compact buttons.
- The „Show completed“ bar is now narrower and more restrained.
- Cleanup: unused CSS (4 dead `.controls` blocks, orphaned media queries) and an invisible, no-longer-used statistics bar removed.

## v6.2.3 — 2026-08-03 — Cleanup

- Bug fix: An old, superfluous code block threw a console error on every load (`completedTeams is not defined`). The dead legacy code was removed entirely.
- Cleanup: Thumbnail loading code that existed three times was reduced to one version; dead code and an unused toast element removed (~650 fewer lines).
- Performance: Detected kit colors are now stored permanently (faster after a reload) and queried in batches for visible figures instead of individually.
- Accessibility: The kit figure now has a descriptive label for screen readers.
- Minor: Filenames with „/“ are no longer incorrectly excluded (categories are recognized via the file extension anyway).

## v6.2.2 — 2026-08-02 — Feinschliff

- The check-off buttons (team & kit set) now blend into the design: dark, transparent background with a light border; checked = green with a white check mark. The glaring white and the gray border are gone.

## v6.2.1 — 2026-08-02 — „Reihe"

- The kit sets are now displayed side by side in the **Kit sets** view too (as in the Teams view).

## v6.2 — 2026-08-02 — „Reihe"

- A team's kits (home, away, third) are now displayed **side by side** – like in the Wikipedia infobox. On narrow screens they wrap automatically.
- The kit figure is now **cropped to the actual parts** – no more large empty space around small kits.
- The **white rectangle** below the shirt (socks placeholder) was removed: the figure now shows only the parts that exist. (The infobox code still fills the socks color with the shirt color.)

## v6.1 — 2026-08-02 — „Aufstellung"

- New: In the **Teams** and **Kit sets** views each kit set is now displayed as a composed **kit figure** – exactly like the Wikipedia template `{{Football kit}}`: sleeves + shirt form the top, below it the shorts, below that the socks.
- The color areas behind the patterns are detected automatically per part (same engine as the infobox copier). If a socks image is missing, the socks area takes the shirt color.
- The individual parts remain visible below (with 📊 usage and check-off). The figure size follows the **thumb width**.
- Performance: Colors are computed only for visible figures (lazy loading) and cached.

## v6.0.3 — 2026-08-02 — Bug fix

- Bug fix: **Thumb width** now visibly changes the preview size. Previously only the loaded image resolution was adjusted, while the display stayed the same size due to fixed CSS dimensions. Now the column width and preview height are driven directly by the configured value.

## v6.0.2 — 2026-08-02 — Bug fix

- Bug fix: Filters take effect immediately again. Changing **Season**, **Thumb width**, **View** or **„Kit parts only“** now updates the list directly (previously the value was only saved). **Date**, **Limit** and **Depth** reload the data, since they affect the query itself.

## v6.0.1 — 2026-08-02 — Bug fix

- Bug fix: Categories (e.g. `Football_kit_body/VARAMAN_specific_patterns`) incorrectly showed up as a „file“ in the results. Now only real media files (with an image extension) are shown.

## v6.0 — 2026-07-23 — „Der Schneider"

- New: Each team tile now has the **„📋 Infobox code“** button. It copies the complete Wikipedia infobox block for *all* of the team's kits (home = 1, away = 2, …) directly to the clipboard.
- The `pattern_*` values are derived from the filenames (e.g. `Kit_body_kaizerchiefs2627h.png` → `_kaizerchiefs2627h`).
- The color values (`leftarm`, `body`, `rightarm`, `shorts`, `socks`) are detected per part from the original image – using the same silhouette color detection as the userscript „Wikipedia Football Kit Code Copier“.
- If a socks image is missing, `pattern_so` stays empty and the socks color takes the main color of the shirt (`body`).

## v5.4.2 — 2026-07-02 — Bug fix

- Bug fix: „Show usage“ now also shows the **local Commons usage** (e.g. gallery pages like „2026–27 Swiss Super League kits“). Previously only usage on other wikis (`globalusage`) was loaded; local usage is now added via `fileusage`.

## v5.4.1 — 2026-07-02 — Bug fix

- Bug fix: Incorrectly escaped backticks in `showStatistics()` made the first script block invalid. After the fix it runs fully again.

## v5.4.0 — 2026-07-02 — Genuine new arrivals

- New: Only kit parts with a genuine **new image upload** from the selected date onward are now shown. Pure text/metadata changes to old files no longer appear.
- Foundation: the actual upload date is determined via the MediaWiki API (`imageinfo`) – instead of the page timestamp `touched`, which changes with every edit.
- The date shown on the cards now displays the real upload date; sorting is by that too.
- Bug fix: Automatic saving of the filters (date, limit & co.) works again – a missing control had previously aborted the script.

## v5.3.2 — 2026-06-10 — Readable timestamps

- Timestamps in the file meta line (`file-meta`) are now shown in a readable format: `09.06.2026, 22:58` instead of `20260609205821`
- Conversion: MediaWiki UTC timestamp → local browser time (e.g. CEST) in German format
- Robust parsing with a fallback to the original value on an invalid timestamp

## v5.3.1 — 2026-04-27 — Historical kits

- Bugfix: Historical four-digit years correct (`tur1923h` → „Season 1923“ instead of „2019/2023“)
- Bugfix: Number variants `h1`, `h2`, `h2l` recognized as home (`crc90h1`, `jacuipense26h2l`, `marciliodias26h2l`)
- Bugfix: Season parsed correctly for numbered kit suffixes (no longer „No season“)
- Bugfix: Long socks with a number variant (`h2l`) correctly recognized as home + long socks
- Dropdown shows historical single years (e.g. „1923“) correctly
- All numbered variants normalized: `h2→h`, `a2→a`, `t2→t`, `gk2→gk`
- Changelog modal added (click the version number)

## v5.3.0 — Lazy Kit Loader

- Thumbnails loaded on demand via IntersectionObserver through the MediaWiki API (no ORB blocking)
- Batch loading up to 50 files per API call
- Rate-limit protection: 120 ms pause between batches

## v5.2.0 — Unified Storage

- Unified storage system for filters and settings
- Compact filter control bar
