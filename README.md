<div align="center">

<img src="icon.svg" alt="Kitly logo" width="120" height="120">

# Kitly

### The football-kit browser for Wikimedia Commons

**Finds newly uploaded football-kit files on Wikimedia Commons, automatically groups them by team → season → kit set, assembles the individual parts into the Wikipedia kit figure and copies the ready-to-use infobox code. All in a single HTML file – no installation, with an English and German interface plus light/dark mode.**

[![Version](https://img.shields.io/github/v/release/V-Toll/Kitly?label=Version&color=0A734C)](https://github.com/V-Toll/Kitly/releases)
[![License](https://img.shields.io/badge/License-Unlicense-4CAF50)](LICENSE)
[![Single-File](https://img.shields.io/badge/Single--File-HTML-FF8C00)](Kitly.html)
[![Language](https://img.shields.io/badge/Language-EN%20%2F%20DE-2E7D32)](#-language--sprache)
[![for Wikimedia Commons](https://img.shields.io/badge/for-Wikimedia%20Commons-lightgrey?logo=wikimediacommons&logoColor=white)](https://commons.wikimedia.org)

[⬇️ Download the latest version](https://github.com/V-Toll/Kitly/releases/latest) · [📜 Changelog](CHANGELOG.md) · [🐞 Report an issue](https://github.com/V-Toll/Kitly/issues)

</div>

---

## ✨ Why this tool?

- 🗂️ **A single file** – download it, open it in your browser, done. No installation, no trackers.
- 🆕 **Genuine new uploads only** – Kitly determines each file's *actual* upload date via the MediaWiki API and, by default, hides pure metadata edits as well as older files that merely received a new version.
- 🧩 **Automatic grouping** – individual parts (`Kit_body_…`, `Kit_shorts_…`, `Kit_socks_…`, sleeves) are parsed into team, season and kit set and neatly combined.
- 👕 **Composited kit figure** – the parts are assembled into a complete kit, just like in the Wikipedia infobox.
- 📋 **Infobox code at the click of a button** – copies the ready-to-use Wikipedia infobox block per season (home = 1, away = 2, third = 3 …), including automatically detected kit colours.
- ✅ **Check off & remember** – completed teams, kit sets or individual files can be hidden; the state is stored locally, per season and kit set. A ticked kit set reappears when it gains a new part, a ticked file reappears when a new version is uploaded.
- 🌐 **Bilingual** (English / German) and with **light/dark mode** including an automatic option.
- 🔔 **Optional notification** – get a desktop notification when a refresh finishes (with a permission-free tab-title flash as a fallback that works even for local files).
- 🔒 **Privacy-friendly** – runs locally in your browser; data comes exclusively from Wikimedia Commons (the optional PetScan source additionally uses a small CORS proxy).
- 🛰️ **Three data sources** – *Commons search* (live, the default), *PetScan* (category crawl) or *both at once*, merged by filename. Switchable in the filter panel.
- ⏱️ **Since last visit** – one click sets the date back to your last retrieval, so you never have to remember when you last looked.

---

## 🚀 Usage

1. Download [`Kitly.html`](https://github.com/V-Toll/Kitly/releases/latest) and open the file in the browser of your choice.
2. Under **📥 Data retrieval**, pick the **Data source** (Commons search by default) and set the **Date (after)** – Kitly shows kit files uploaded *on or after* that day (default: yesterday).
3. Click **🔄 Refresh**. The new kits appear grouped by team and season. While it runs, a progress bar shows the current step and an estimated time left; **Cancel** stops the retrieval and leaves the current list untouched.
4. Click **📋 Infobox code** for a season, paste the code into the Wikipedia article – and give it a quick check before saving. ✅

> [!TIP]
> Clicking a thumbnail opens the corresponding Commons file page in a new background tab. The **⚽** version badge at the top opens the changelog.

<details>
<summary><strong>🔎 All filters & views at a glance</strong></summary>

<br>

- **Data source** – *Commons search (live)* queries the MediaWiki API directly and always reflects the current state of Commons; *PetScan (categories)* crawls the kit categories and catches files the title search misses, but runs on a database replica that can lag by several days; *Both* runs the two in sequence and merges them by filename.
- **Date (after)** – lower bound for the actual upload date. The **⏱️ Since last visit** button appears underneath once you have run a retrieval before, and jumps the date back to that day.
- **Limit** – maximum number of results (default 1000, up to 10000 for wide time ranges).
- **Season** – restrict to a specific season.
- **Kit type** – home / away / third / goalkeeper …
- **Kit parts only** – hide non-kit files (on by default).
- **Hide file updates** – show genuine new uploads only; files whose first upload predates the chosen date are hidden (on by default, see below).
- **View** – *Teams* (grouped by team/season), *Kit sets* (side by side like the infobox) or *Single*.
- **Thumb width** – size of the thumbnails.
- **Theme** (🌗) – Auto / Light / Dark. **Language** (🌐) – English / German. **Notifications** (🔔) – notify when a refresh completes.

</details>

---

## 🆕 "Hide file updates"

PetScan returns files based on the page timestamp – so old kits show up even when only the description or a new version was edited. Kitly therefore fetches both the **latest *and* the first** upload of each file via the MediaWiki `imageinfo` API (the Commons search source resolves the same data while paging, so the option behaves identically there):

- By default, only files whose **first upload** is on or after the chosen date are shown – i.e. **genuine new additions**.
- Files that merely received a new version within the period (first upload predates it) are hidden.
- The option can be toggled at any time without reloading.

> [!NOTE]
> The date filters by the **upload time on Commons**, not by season. So a 2026/27 kit uploaded back in July only appears once the "Date (after)" is set early enough.

---

## 🌐 Data source & CORS proxy

Kitly offers three sources, switchable under **📥 Data retrieval**:

- **Commons search (live)** – the default. Kitly runs five title searches (`Kit body`, `Kit shorts`, `Kit socks`, `Kit left arm`, `Kit right arm`) against the Commons search API, newest first, and stops a stream once a page contains no creations within the selected period. It then walks the kit categories live via `list=categorymembers` (newest first, bounded by the chosen date) to pick up files whose name breaks the scheme. It reflects the live state of Commons, but finds *new versions* of older files only incidentally.
- **PetScan (categories)** – the original behaviour: a category crawl via [PetScan](https://petscan.wmcloud.org/). It covers the categories completely, including edits to existing files, but runs on a Toolforge database replica that can lag behind Commons by several days. When the newest result is two or more days old, Kitly says so above the results.
- **Both** – runs the Commons source first, then PetScan, and merges the results by filename. Slower, and only as current as PetScan allows for the part that comes from there; if PetScan fails, the Commons result is shown anyway with a note.

Thumbnails, original URLs and upload timestamps always come directly from the Commons MediaWiki API (`origin=*`), in batches of 50 with retry and backoff on rate limiting. Since PetScan sends no CORS headers, that query runs through a small, public CORS proxy on [Wikimedia Toolforge](https://toolforge.org/) (`kitproxy.toolforge.org`). Only public Commons data is retrieved; nothing is sent to third parties.

---

## 🎨 Design, themes & language

Kitly comes in a "pitch" look (grass green) with **light, dark and automatic modes** (the automatic mode follows your operating system). The green identity is kept in both modes. The kit icon to the left of the title as well as the browser-tab favicon are embedded SVG – no external image files.

## 🌍 Language / Sprache

The entire interface is available in **English and German**; on first launch the language is guessed from your browser language (defaulting to English) and can be switched at any time via the **🌐** button. The choice is stored locally.

---

## 📦 Version & changelog

Current version: **v6.15.0**. You'll find the full history in [CHANGELOG.md](CHANGELOG.md) as well as directly in the tool via the **⚽** version badge.

## 📄 License

Released under the [Unlicense](LICENSE) (public domain) – free to use, modify and share.

---

<div align="center">
<sub>Crafted over countless hours – by hand and with the help of <a href="https://www.anthropic.com/claude">Claude</a> (Anthropic). 🤖✍️</sub>
</div>
