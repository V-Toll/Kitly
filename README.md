<div align="center">

<img src="icon.svg" alt="Kitly Logo" width="120" height="120">

# Kitly

### der Trikot-Browser für Wikimedia Commons

**Findet neu hochgeladene Fußball-Trikot-Dateien auf Wikimedia Commons, gruppiert sie automatisch nach Team → Saison → Trikotset, setzt die Einzelteile zur Wikipedia-Kit-Figur zusammen und kopiert den fertigen Infobox-Code. Alles in einer einzigen HTML-Datei – ohne Installation, mit deutscher und englischer Oberfläche sowie Hell-/Dunkelmodus.**

[![Version](https://img.shields.io/github/v/release/V-Toll/Kitly?label=Version&color=0A734C)](https://github.com/V-Toll/Kitly/releases)
[![Lizenz](https://img.shields.io/badge/Lizenz-Unlicense-4CAF50)](LICENSE)
[![Single-File](https://img.shields.io/badge/Single--File-HTML-FF8C00)](Kitly.html)
[![Sprache](https://img.shields.io/badge/Sprache-DE%20%2F%20EN-2E7D32)](#-sprache--language)
[![Für Wikimedia Commons](https://img.shields.io/badge/f%C3%BCr-Wikimedia%20Commons-lightgrey?logo=wikimediacommons&logoColor=white)](https://commons.wikimedia.org)

[⬇️ Neueste Version herunterladen](https://github.com/V-Toll/Kitly/releases/latest) · [📜 Changelog](CHANGELOG.md) · [🐞 Problem melden](https://github.com/V-Toll/Kitly/issues)

</div>

---

## ✨ Warum dieses Tool?

- 🗂️ **Eine einzige Datei** – herunterladen, im Browser öffnen, fertig. Keine Installation, keine Tracker.
- 🆕 **Nur echte Neu-Uploads** – Kitly ermittelt über die MediaWiki-API das *tatsächliche* Upload-Datum jeder Datei und blendet reine Metadaten-Änderungen sowie ältere Dateien, die nur ein Update bekamen, standardmäßig aus.
- 🧩 **Automatische Gruppierung** – aus einzelnen Teilen (`Kit_body_…`, `Kit_shorts_…`, `Kit_socks_…`, Ärmel) werden Team, Saison und Trikotset erkannt und übersichtlich zusammengefasst.
- 👕 **Zusammengesetzte Kit-Figur** – die Teile werden wie in der Wikipedia-Infobox zu einem kompletten Trikot montiert.
- 📋 **Infobox-Code auf Knopfdruck** – kopiert pro Saison den fertigen Wikipedia-Infobox-Block (Heim = 1, Auswärts = 2, Ausweich = 3 …), inklusive automatisch erkannter Trikotfarben.
- ✅ **Abhaken & merken** – erledigte Teams, Trikotsets oder einzelne Dateien lassen sich ausblenden; der Stand bleibt lokal gespeichert.
- 🌐 **Zweisprachig** (Deutsch / Englisch) und mit **Hell-/Dunkelmodus** samt Automatik.
- 🔒 **Datensparsam** – läuft lokal im Browser; Daten kommen ausschließlich von Wikimedia Commons (über einen schlanken CORS-Proxy für PetScan).

---

## 🚀 Nutzung

1. Lade [`Kitly.html`](https://github.com/V-Toll/Kitly/releases/latest) herunter und öffne die Datei im Browser deiner Wahl.
2. Stelle unter **📥 Datenabruf** das **Datum (nach)** ein – Kitly zeigt Trikot-Dateien, die *ab* diesem Tag hochgeladen wurden (Standard: gestern).
3. Klicke auf **🔄 Aktualisieren**. Die neuen Trikots erscheinen nach Team und Saison gruppiert.
4. Pro Saison auf **📋 Infobox-Code** klicken, den Code in den Wikipedia-Artikel einfügen – und vor dem Speichern kurz prüfen. ✅

> [!TIP]
> Ein Klick auf ein Vorschaubild öffnet die zugehörige Commons-Dateiseite in einem neuen Hintergrund-Tab. Über das **⚽**-Versionsabzeichen oben öffnet sich der Changelog.

<details>
<summary><strong>🔎 Alle Filter & Ansichten im Überblick</strong></summary>

<br>

- **Datum (nach)** – Untergrenze für das tatsächliche Upload-Datum.
- **Limit** – maximale Trefferzahl (Standard 1000, bis 10000 für breite Zeiträume).
- **Tiefe** – Tiefe des PetScan-Kategorie-Crawls.
- **Saison** – auf eine bestimmte Saison einschränken.
- **Trikottyp** – Heim / Auswärts / Ausweich / Torhüter …
- **Nur Trikot-Teile** – Nicht-Trikot-Dateien ausblenden (standardmäßig aktiv).
- **Datei-Updates ausblenden** – nur echte Neu-Uploads zeigen; Dateien, deren Erst-Upload vor dem gewählten Datum liegt, werden ausgeblendet (standardmäßig aktiv, siehe unten).
- **Ansicht** – *Teams* (nach Team/Saison gruppiert), *Trikotsets* (nebeneinander wie in der Infobox) oder *Einzeln*.
- **Thumb-Breite** – Größe der Vorschaubilder.
- **Design** (🌗) – Auto / Hell / Dunkel. **Sprache** (🌐) – Deutsch / Englisch.

</details>

---

## 🆕 „Datei-Updates ausblenden"

PetScan liefert Dateien anhand des Seiten-Zeitstempels – dadurch tauchen auch alte Trikots auf, an denen nur die Beschreibung oder eine neue Version bearbeitet wurde. Kitly holt daher über die MediaWiki-`imageinfo`-API pro Datei den **jüngsten *und* den ersten** Upload:

- Standardmäßig werden nur Dateien gezeigt, deren **Erst-Upload** am/nach dem gewählten Datum liegt – also **echte Neuzugänge**.
- Dateien, die im Zeitraum nur eine neue Version erhielten (Erst-Upload liegt davor), werden ausgeblendet.
- Die Option lässt sich jederzeit ohne Neuladen umschalten.

> [!NOTE]
> Das Datum filtert nach dem **Upload-Zeitpunkt auf Commons**, nicht nach der Saison. Ein 2026/27-Trikot, das schon im Juli hochgeladen wurde, erscheint also erst, wenn das „Datum (nach)" entsprechend früh gesetzt wird.

---

## 🌐 Datenquelle & CORS-Proxy

Kitly fragt [PetScan](https://petscan.wmcloud.org/) nach den Trikot-Kategorien ab und lädt Vorschaubilder, Original-URLs und Upload-Zeitstempel direkt über die MediaWiki-API von Commons (`origin=*`). Da PetScan keine CORS-Header setzt, läuft dessen Abfrage über einen schlanken, öffentlichen CORS-Proxy auf [Wikimedia Toolforge](https://toolforge.org/) (`kitproxy.toolforge.org`). Es werden ausschließlich öffentliche Commons-Daten abgerufen; nichts wird an Dritte gesendet.

---

## 🎨 Design, Themes & Sprache

Kitly kommt im „Pitch"-Look (Rasen-Grün) mit **Hell-, Dunkel- und Automatik-Modus** (folgt dem Betriebssystem). Die grüne Identität bleibt in beiden Modi erhalten. Das Trikot-Icon links vom Titel wie auch das Browser-Tab-Favicon sind eingebettetes SVG – ohne externe Bilddateien.

## 🌍 Sprache / Language

Die komplette Oberfläche gibt es auf **Deutsch und Englisch**; beim ersten Start wird die Sprache aus der Browsersprache erraten und lässt sich jederzeit über den **🌐**-Button umschalten. Die Auswahl wird lokal gespeichert.

---

## 📦 Version & Changelog

Aktuelle Version: **v6.7.0 „Neuzugang"**. Den vollständigen Verlauf findest du in der [CHANGELOG.md](CHANGELOG.md) sowie direkt im Tool über das **⚽**-Versionsabzeichen.

## 📄 Lizenz

Veröffentlicht unter der [Unlicense](LICENSE) (Public Domain) – frei nutzbar, veränderbar und weitergebbar.

---

<div align="center">
<sub>In unzähligen Stunden entstanden – von Hand und mit Unterstützung von <a href="https://www.anthropic.com/claude">Claude</a> (Anthropic). 🤖✍️</sub>
</div>
