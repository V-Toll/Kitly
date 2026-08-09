# Changelog

Alle nennenswerten Änderungen an *Kitly* – neueste Version zuerst. Die Einträge sind auch direkt im Tool über das **⚽**-Versionsabzeichen einsehbar.

Benannte Minor-Versionen (mit Codename) sind Funktions-Releases; Patches betreffen Fehlerbehebungen und Feinschliff.


## v6.7.0 — 2026-08-09 — „Neuzugang"

- Neu: **„Datei-Updates ausblenden"** (standardmäßig aktiv). Dateien, die im gewählten Zeitraum nur eine *neue Version* erhielten, deren **Erst-Upload** aber davor liegt, werden ausgeblendet – es bleiben also nur echte Neu-Uploads. Umschaltbar ohne Neuladen.
- Grundlage: Pro Datei wird jetzt zusätzlich der **Erst-Upload** über die MediaWiki-API ermittelt (älteste Dateiversion), nicht nur der jüngste Upload.
- Das **Limit** lässt sich jetzt bis **10000** stellen (vorher 2000; Standard bleibt 1000) – für breite Zeiträume, in denen sonst Treffer abgeschnitten würden.

## v6.6.0 — 2026-08-09 — „Kitly"

- Das Tool heißt jetzt **Kitly** – ein Name, der auch international funktioniert.
- Neu: **Sprachumschaltung DE/EN** (🌐-Button). Die komplette Oberfläche gibt es jetzt auf Deutsch und Englisch; die Sprache wird gespeichert und beim ersten Start aus der Browsersprache erraten.
- Neu: **Hell-/Dunkelmodus mit Automatik** (🌗-Button, Auto / Hell / Dunkel). „Auto" folgt dem Betriebssystem; die grüne Pitch-Identität bleibt in beiden Modi erhalten. Auswahl wird gespeichert.
- Neu: Klick auf ein **Vorschaubild** öffnet die Commons-Dateiseite in einem neuen Hintergrund-Tab.
- Saison-Spannen über mehrere Jahre werden jetzt als Bereich angezeigt: `8792` → „Saison 1987–1992" (statt „Saison 8792"), während aufeinanderfolgende Saisons weiter als `1987/88` erscheinen.
- Zähler stehen jetzt im korrekten Singular/Plural („1 Team gefunden", „1 Teil" bzw. „1 team found", „1 part").

## v6.5.1 — 2026-08-09 — „Feinschliff"

- Saison-Spannen korrekt erkannt: `1819`/`1920`/`2021` werden jetzt als „2018/19"/„2019/20"/„2020/21" angezeigt und sortiert – echte Einzeljahre (1923, 1975, 2026) bleiben unverändert.
- Verbose-Dateinamen: alleinstehendes Jahr (z. B. `FC_Tokyo_2026_HOME`) wird nicht mehr in den Teamnamen übernommen.
- Trikotset-Ansicht zeigt die Saison im Klartext (`2627` → „2026/27").
- Trikottyp-Filter greift auch für Torhüter-Varianten (`gkd`/`g`) zuverlässig.
- Statistik zählt Dateien in ausgeblendeten Teams nicht mehr als sichtbar; Abhak-Buttons werden entprellt nachgezogen (weniger DOM-Last bei großen Listen).

## v6.5.0 — 2026-08-09 — „Aufgeräumte Kabine"

- Neu: **Trikottyp-Filter** (Dropdown Heim/Auswärts/Ausweich/Torhüter) in der Filter-Gruppe.
- Die beiden parallelen Abhak-Systeme wurden zu **einem** zusammengeführt (ein einziger Speicher). Team-, Kitset- und Datei-Abhaken laufen jetzt über dieselbe Logik.
- 2-stellige Saison wird korrekt angezeigt: `02` → „Saison 2002".
- Aufgeräumt: toter Auto-Refresh-Code, verwaiste Hilfsfunktionen, ein ungenutzter Zugriff und alle Debug-Ausgaben (console.log) entfernt.

## v6.4.0 — 2026-08-08 — „Chronik"

- Alle Saisons eines Teams stehen jetzt unter **einer** Team-Karte, chronologisch sortiert (neueste Saison oben). Beispiel: „Gelp" mit 1996/98 über 1993/95.
- Wird ein Team als erledigt abgehakt, verschwinden **alle** Saisons dieses Teams mit.
- Der „Infobox-Code"-Button sitzt jetzt **pro Saison** (statt pro Team), damit die Kit-Nummern nicht über Saisons hinweg kollidieren.

## v6.3.11 — 2026-08-08 — Saison-Filter angeglichen

- Der Saison-Filter zeigt jetzt exakt dieselbe Saison wie die Team-Anzeige (gemeinsame Logik). Ungewöhnliche Spannen wie „1993/95" oder „1998/2001" erscheinen im Dropdown genauso wie an den Teams.

## v6.3.10 — 2026-08-08 — Saison-Spannen

- Nicht fortlaufende Saison-Spannen werden jetzt korrekt angezeigt: `9395` → „1993/95". Bei Jahrhundertwechsel wird das Endjahr 4-stellig gezeigt: `9801` → „1998/2001", `9900` → „1999/2000". Echte Einzeljahre (1923, 1975) bleiben unverändert.

## v6.3.9 — 2026-08-08 — Bindestrich-Saisons

- Dateinamen mit Trennzeichen-Saison wie `Kit_body_spartak-vn-26-27-a.png` werden jetzt korrekt gelesen: Team („spartak-vn"), Saison („2026/27") und Typ (Auswärts). Heim und Auswärts landen im selben Team. Unterstützt „YY-YY", „YYYY-YY" und einzelne 4-stellige Jahresblöcke.

## v6.3.8 — 2026-08-08 — Infobox-Nummerierung

- Der „Infobox-Code" nummeriert die Kits jetzt nach Typ statt nach Reihenfolge: Heim = 1, Auswärts = 2, Ausweich = 3, Viertes = 4. Ein reines Auswärtsset liefert also korrekt `…2` (statt fälschlich `…1`).

## v6.3.7 — 2026-08-08 — Feinschliff

- In der Team-Zeile sind die Trikottypen jetzt einheitlich sortiert: Heim, Auswärts, Ausweich, dann Torwart – passend zur Reihenfolge der Kitset-Karten.

## v6.3.6 — 2026-08-08 — Ausführliche Dateinamen

- Neues Namensschema erkannt: `Kit_body_FC_Tokyo_2026_-_27_AWAY_FP.png` & Co. Team („FC Tokyo"), Saison („2026/27") und Typ werden jetzt korrekt gelesen; Heim und Auswärts landen im selben Team.
- `FP` = Feldspieler (→ Heim/Auswärts/Ausweich), `GK` = Torwart (→ Torhüter Heim/Auswärts). Bisher wurde stattdessen „fp" als Typ und der ganze Name als Team angezeigt.

## v6.3.5 — 2026-08-03 — Torhüter Heim/Auswärts

- Torwart-Trikots werden jetzt nach Heim/Auswärts unterschieden: `gkh` → „Torhüter Heim", `gka` → „Torhüter Auswärts" (auch `gkt` → „Torhüter Ausweich"). Gilt für Anzeige, Kitset-Titel und Trikottyp-Filter.

## v6.3.4 — 2026-08-03 — Fehlerbehebung

- Dateien mit 4-stelliger Jahreszahl ohne Typ-Kürzel (z. B. `Kit_body_River_1975.png`) werden jetzt korrekt erkannt: Saison = 1975, Typ = Heim (kein „h"/„a" am Ende bedeutet Heimtrikot). Bisher landeten sie unter „Keine Saison • unknown".

## v6.3.3 — 2026-08-03 — Feinschliff

- Die Eingabefelder „Limit" (max. 2000) und „Tiefe" (max. 100) haben jetzt Obergrenzen, passend zu den Limits des kitproxy-Servers – so ist die Grenze direkt im Feld sichtbar.

## v6.3.2 — 2026-08-03 — Aufräumen

- Kategorie-Abfrage verschlankt: „Association football kit templates" entfernt – sie ist eine direkte Unterkategorie beider Ober-Kategorien und bei Tiefe 50 ohnehin enthalten (empirisch geprüft, Ergebnismenge unverändert).

## v6.3.1 — 2026-08-03 — „Ordnung"

- Erledigte Teams werden jetzt nur noch über **einen** Weg ein-/ausgeblendet: den „Erledigte anzeigen"-Button. Die doppelte Checkbox in der Filter-Gruppe wurde entfernt.

## v6.3 — 2026-08-03 — „Ordnung"

- Die Einstellungen sind jetzt thematisch gruppiert: **Datenabruf** (Datum, Limit, Tiefe), **Filter** (Saison, Nur Trikot-Teile, Abgeschlossene Teams) und **Darstellung** (Ansicht, Thumb-Breite).
- Die Checkbox „Abgeschlossene Teams" ist jetzt ein kompaktes Inline-Element (kein großer Kasten mehr) und passt zu „Nur Trikot-Teile".

## v6.2.5 — 2026-08-03 — Kopfbereich

- Einheitliches Erscheinungsbild: die globale Eingabe-Basis (früher kräftiger 2px-Rahmen) ist jetzt dezent wie die Filterfelder.
- Die Checkboxen „Nur Trikot-Teile" und „Abgeschlossene Teams anzeigen" passen jetzt farblich und in der Größe zum neuen Filter-Look.

## v6.2.4 — 2026-08-03 — Kopfbereich

- Der obere Bereich wirkt aufgeräumter: schlankere, dezentere Filterfelder (feiner Rahmen statt kräftigem Weiß, mit sanftem Fokus-Rahmen) und kompaktere Buttons.
- Die „Erledigte anzeigen"-Leiste ist jetzt schmaler und zurückhaltender.
- Aufräumen: ungenutztes CSS (4 tote `.controls`-Blöcke, verwaiste Media-Queries) und eine unsichtbare, nicht mehr genutzte Statistik-Leiste entfernt.

## v6.2.3 — 2026-08-03 — Aufräumen

- Fehlerbehebung: Ein alter, überflüssiger Code-Block warf bei jedem Laden einen Konsolenfehler (`completedTeams is not defined`). Der tote Alt-Code wurde vollständig entfernt.
- Aufräumen: Dreifach vorhandener Thumbnail-Lade-Code auf eine Fassung reduziert; toter Code und ein ungenutztes Toast-Element entfernt (~650 Zeilen weniger).
- Performance: Erkannte Trikot-Farben werden jetzt dauerhaft gespeichert (schneller nach Neuladen) und für sichtbare Figuren gebündelt statt einzeln abgefragt.
- Barrierefreiheit: Die Kit-Figur hat jetzt ein sprechendes Label für Screenreader.
- Kleinigkeit: Dateinamen mit „/" werden nicht mehr fälschlich ausgeschlossen (Kategorien werden ohnehin über die Datei-Endung erkannt).

## v6.2.2 — 2026-08-02 — Feinschliff

- Die Abhak-Buttons (Team & Kitset) fügen sich jetzt ins Design ein: dunkler, transparenter Hintergrund mit hellem Rahmen; abgehakt = grün mit weißem Haken. Das grelle Weiß und der graue Rahmen sind weg.

## v6.2.1 — 2026-08-02 — „Reihe"

- Die Kitsets werden jetzt auch in der **Trikotsets**-Ansicht nebeneinander dargestellt (wie in der Teams-Ansicht).

## v6.2 — 2026-08-02 — „Reihe"

- Die Kits eines Teams (Heim, Auswärts, Ausweich) werden jetzt **nebeneinander** dargestellt – wie in der Wikipedia-Infobox. Auf schmalen Bildschirmen brechen sie automatisch um.
- Die Kit-Figur ist jetzt **auf die tatsächlichen Teile zugeschnitten** – kein großer Leerraum mehr um kleine Kits.
- Das **weiße Rechteck** unter dem Trikot (Stutzen-Platzhalter) wurde entfernt: In der Figur werden nur noch vorhandene Teile gezeigt. (Der Infobox-Code füllt die Stutzenfarbe weiterhin mit der Trikot-Farbe.)

## v6.1 — 2026-08-02 — „Aufstellung"

- Neu: In der **Teams**- und **Trikotsets**-Ansicht wird jedes Kitset jetzt als zusammengesetzte **Trikot-Figur** dargestellt – genau wie die Wikipedia-Vorlage `{{Football kit}}`: Ärmel + Trikot bilden das Shirt, darunter die Hose, darunter die Stutzen.
- Die Farbflächen hinter den Mustern werden je Teil automatisch erkannt (gleiche Engine wie der Infobox-Kopierer). Fehlt ein Stutzen-Bild, erhält die Stutzen-Fläche die Trikot-Farbe.
- Die Einzelteile bleiben darunter sichtbar (mit 📊-Verwendung und Abhaken). Die Figur-Größe folgt der **Thumb-Breite**.
- Performance: Farben werden nur für sichtbare Figuren berechnet (Lazy-Loading) und zwischengespeichert.

## v6.0.3 — 2026-08-02 — Fehlerbehebung

- Fehlerbehebung: **Thumb-Breite** ändert jetzt sichtbar die Vorschaugröße. Bisher wurde nur die geladene Bildauflösung angepasst, die Anzeige blieb wegen fester CSS-Maße gleich groß. Jetzt steuern Spaltenbreite und Vorschau-Höhe direkt der eingestellte Wert.

## v6.0.2 — 2026-08-02 — Fehlerbehebung

- Fehlerbehebung: Filter wirken wieder sofort. Ändern von **Saison**, **Thumb-Breite**, **Ansicht** oder **„Nur Trikot-Teile"** aktualisiert die Liste jetzt direkt (bisher wurde der Wert nur gespeichert). **Datum**, **Limit** und **Tiefe** laden die Daten neu, da sie die Abfrage selbst betreffen.

## v6.0.1 — 2026-08-02 — Fehlerbehebung

- Fehlerbehebung: Kategorien (z. B. `Football_kit_body/VARAMAN_specific_patterns`) tauchten fälschlich als „Datei" in den Ergebnissen auf. Es werden jetzt nur noch echte Mediendateien (mit Bild-Endung) angezeigt.

## v6.0 — 2026-07-23 — „Der Schneider"

- Neu: Jede Team-Kachel hat jetzt den Button **„📋 Infobox-Code"**. Er kopiert den kompletten Wikipedia-Infobox-Block für *alle* Kits des Teams (Heim = 1, Auswärts = 2, …) direkt in die Zwischenablage.
- Die `pattern_*`-Werte werden aus den Dateinamen abgeleitet (z. B. `Kit_body_kaizerchiefs2627h.png` → `_kaizerchiefs2627h`).
- Die Farbwerte (`leftarm`, `body`, `rightarm`, `shorts`, `socks`) werden je Teil aus dem Originalbild erkannt – mit derselben Silhouetten-Farberkennung wie das Userscript „Wikipedia Football Kit Code Copier".
- Fehlt ein Stutzen-Bild, bleibt `pattern_so` leer und die Stutzenfarbe übernimmt die Haupt-Farbe des Trikots (`body`).

## v5.4.2 — 2026-07-02 — Fehlerbehebung

- Fehlerbehebung: „Verwendung anzeigen" zeigt jetzt auch die **lokale Commons-Verwendung** (z. B. Galerieseiten wie „2026–27 Swiss Super League kits"). Bisher wurde nur die Verwendung auf anderen Wikis (`globalusage`) geladen; die lokale Nutzung wird nun über `fileusage` ergänzt.

## v5.4.1 — 2026-07-02 — Fehlerbehebung

- Fehlerbehebung: Fehlerhaft escapte Backticks in `showStatistics()` machten den ersten Skript-Block ungültig. Nach der Korrektur läuft dieser wieder vollständig.

## v5.4.0 — 2026-07-02 — Echte Neuzugänge

- Neu: Es werden jetzt nur noch Trikot-Teile mit einem echten **neuen Bild-Upload** ab dem gewählten Datum angezeigt. Reine Text-/Metadaten-Änderungen an alten Dateien tauchen nicht mehr auf.
- Grundlage: das tatsächliche Upload-Datum wird über die MediaWiki-API (`imageinfo`) ermittelt – statt des Seiten-Zeitstempels `touched`, der bei jeder Bearbeitung wechselt.
- Die Datum-Anzeige auf den Karten zeigt nun das echte Upload-Datum; sortiert wird ebenfalls danach.
- Fehlerbehebung: Das automatische Speichern der Filter (Datum, Limit & Co.) funktioniert wieder – ein fehlendes Bedienelement hatte zuvor das Skript abgebrochen.

## v5.3.2 — 2026-06-10 — Lesbare Zeitstempel

- Zeitstempel in der Datei-Metazeile (`file-meta`) werden jetzt im lesbaren Format angezeigt: `09.06.2026, 22:58` statt `20260609205821`
- Konvertierung: MediaWiki-UTC-Timestamp → lokale Browserzeit (z. B. CEST) im deutschen Format
- Robustes Parsing mit Fallback auf Originalwert bei ungültigem Timestamp

## v5.3.1 — 2026-04-27 — Historische Trikots

- Bugfix: Historische vierstellige Jahreszahlen korrekt (`tur1923h` → „Saison 1923“ statt „2019/2023“)
- Bugfix: Nummernvarianten `h1`, `h2`, `h2l` als Heim erkannt (`crc90h1`, `jacuipense26h2l`, `marciliodias26h2l`)
- Bugfix: Saison bei nummerierten Kit-Suffixen korrekt geparst (nicht mehr „Keine Saison“)
- Bugfix: Lange Stutzen mit Nummernvariant (`h2l`) korrekt als Heim + Langstutzen
- Dropdown zeigt historische Einzeljahre (z. B. „1923“) korrekt an
- Alle nummerierten Varianten normalisiert: `h2→h`, `a2→a`, `t2→t`, `gk2→gk`
- Changelog-Modal hinzugefügt (Klick auf Versionsnummer)

## v5.3.0 — Lazy Kit Loader

- Thumbnails per IntersectionObserver on-demand via MediaWiki-API (kein ORB-Blocking)
- Batch-Loading bis zu 50 Dateien pro API-Aufruf
- Rate-Limit-Schutz: 120 ms Pause zwischen Batches

## v5.2.0 — Unified Storage

- Einheitliches Speichersystem für Filter und Einstellungen
- Kompakte Filter-Steuerleiste
