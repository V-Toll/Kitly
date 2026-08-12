# Changelog

All notable changes to *Kitly* – newest version first. These entries are also available directly in the tool via the **⚽** version badge.

Named minor versions (with a codename) are feature releases; patches cover bug fixes and polish.


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
