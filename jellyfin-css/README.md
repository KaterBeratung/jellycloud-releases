# Jellycloud für Jellyfin (CSS)

Die Optik des Jellycloud Players im Web-Client von Jellyfin, samt passender Gestaltung der **Media Bar**:

- Midnight-Schwarz mit den Grautönen der App, die Schrift der App (Segoe UI, sonst die Systemschrift)
- Kopfzeile und Seitenmenü als Milchglas, Tabs (Startseite / Favoriten) als Glas-Kapsel mit heller Auswahl
- Karten mit großer Rundung, Anheben und sanftem Leuchten beim Überfahren, Glas-Abspielknopf, Fortschritt und Haken in der Akzentfarbe
- Hauptknöpfe (Abspielen, Anmelden) als Akzentfläche mit Lichtkante, übrige Knöpfe, Eingabefelder, Dialoge und Menüs als Liquid Glass
- Media Bar: Abdunklung wie im Banner der App, Abspielen als Akzent-Kapsel, Details und Favorit als runde Glas-Knöpfe, Seitenpunkte in einer Glas-Kapsel mit länglichem aktivem Punkt, Glas-Pfeile, Einstellungsfenster als Glas
- Eine Akzentfarbe für alles (drei Variablen), schmale Fenster und „weniger Bewegung“ berücksichtigt

**Vorschau ohne Jellyfin:** `preview.html` im Browser öffnen (Doppelklick). Sie bildet Kopfzeile, Startseite, Karten, Detailknöpfe, Dialog und Media Bar nach und lädt `jellycloud.css`.

## Einbauen

1. Im Jellyfin-Dashboard: **Allgemein** (je nach Version unter *Branding*) → **Benutzerdefinierter CSS-Code**.
2. Diese eine Zeile einfügen und speichern:

   ```css
   @import url("https://cdn.jsdelivr.net/gh/KaterBeratung/jellycloud-releases@main/jellyfin-css/jellycloud.css");
   ```

3. Die Jellyfin-Seite neu laden, am besten mit **Strg + F5**.

Die Datei liegt in einem öffentlichen GitHub-Repo und wird über den CDN-Dienst jsDelivr ausgeliefert (GitHub selbst liefert Stylesheets mit einem Typ, den Browser für `@import` ablehnen). Die Seitenaufrufe deiner Nutzer gehen dabei an `cdn.jsdelivr.net`. Änderungen an der Datei kommen nach spätestens 12 Stunden bei dir an (der CDN hält sie so lange vor); wer es sofort braucht, ruft einmal `https://purge.jsdelivr.net/gh/KaterBeratung/jellycloud-releases@main/jellyfin-css/jellycloud.css` im Browser auf.

Ohne Fremdadresse geht es auch: den gesamten Inhalt von [`jellycloud.css`](jellycloud.css) statt der Zeile einfügen. (Die Datei ist reines ASCII, es gibt keine Zeichensatz-Probleme.) Eigene Änderungen, zum Beispiel die Akzentfarbe, stehen dann in demselben Feld unter der `@import`-Zeile oder in der kopierten Datei.

Das CSS wirkt überall dort, wo Jellyfins **Web-Oberfläche** angezeigt wird: im Browser, im Jellyfin Media Player (Desktop) und in Apps, die die Web-Oberfläche in einer WebView zeigen. Native Apps (zum Beispiel Android TV, Swiftfin) ignorieren eigenes CSS.

## Media Bar

Die Media Bar ist ein Zusatz von Dritten, nicht von Jellyfin selbst. Das CSS stellt sie nur um, es installiert sie nicht:

- **Original** (MakD): <https://github.com/MakD/Jellyfin-Media-Bar>, Einbau über `index.html` des Web-Clients (Skript und Stylesheet per jsDelivr)
- **Plugin-Fassungen**: <https://github.com/IAmParadox27/jellyfin-plugin-media-bar> (braucht das Plugin *File Transformation* und passt zu Jellyfin 10.10), Fassung für 10.11: <https://github.com/CodeDevMLH/jellyfin-plugin-media-bar-enhanced>

Gestaltet werden die Klassen aus `slideshowpure.css` (Version 6): `#slides-container`, `.slide`, `.backdrop`, `.gradient-overlay`, `.logo`, `.plot`, `.play-button`, `.detail-button`, `.favorite-button`, `.dots-container`, `.dot`, `.arrow`, `.ss-settings` und die Zeilen der Layouts *classic*, *plate* und *marquee*. Das Banner behält die Größe, die das Plugin vorgibt. Wer es wie in der App haben will (88 % der Fensterhöhe, höchstens 860 px), schaltet in Abschnitt 9 die zwei auskommentierten Zeilen `--slideshow-height` und `--slideshow-content-top` ein.

## Akzentfarbe ändern

Standard ist das Silberweiß der App. Für eine andere Farbe diesen Block **unter die `@import`-Zeile** im Jellyfin-Feld setzen (er überschreibt die Vorgaben; bei einer kopierten Datei die drei Variablen im ersten Block ersetzen):

```css
:root {
  --jc-accent: #3b6bff;
  --jc-accent-rgb: 59 107 255;
  --jc-accent-ink: #ffffff;
}
```

`--jc-accent` ist die Farbe für Flächen, `--jc-accent-rgb` die für durchscheinende Tönungen (bei hellen Farben dunkler) und `--jc-accent-ink` die Schrift auf Akzentflächen. Die Werte der App-Vorgaben:

| Farbe | `--jc-accent` | `--jc-accent-rgb` | `--jc-accent-ink` |
| --- | --- | --- | --- |
| Silberweiß (Standard) | `#f5f5f7` | `147 147 148` | `#12131a` |
| Dunkelblau | `#0b2dbf` | `11 45 191` | `#ffffff` |
| Blau | `#3b6bff` | `59 107 255` | `#ffffff` |
| Violett | `#7a5cff` | `122 92 255` | `#ffffff` |
| Türkis | `#19c3c3` | `25 195 195` | `#12131a` |
| Grün | `#35d07f` | `53 208 127` | `#12131a` |
| Gelb | `#f5c518` | `147 118 14` | `#12131a` |
| Orange | `#ff8a1f` | `255 138 31` | `#ffffff` |
| Rosa | `#ff4d6d` | `255 77 109` | `#ffffff` |

Rundung der Ecken: `--jc-radius` (Standard `18px`). Weitere Grautöne und Glasflächen stehen im selben Block.

## Was anders ist als in der App

Es ist ein Anstrich, kein Nachbau: Die Startanimation, der Hintergrundton aus dem Artwork, der Ken-Burns-Zoom im Banner, Trailer im Banner und das Mitwandern der Kopfzeile beim Scrollen gibt es im Web-Client nicht (oder sie liefert die Media Bar selbst). Die Kopfzeile ist durchgehend Glas. Die Daylight-Variante der App gibt es hier nicht, das CSS ist dunkel. Auf Karten steht bewusst kein Weichzeichner (`backdrop-filter`): bei Hunderten Karten kostet er viel Arbeitsspeicher und Grafikleistung.

## Geprüft und nicht geprüft

Geprüft ist das CSS an der nachgebauten Vorschauseite (`tools/shots/jellyfin-css.js`, 27 Prüfungen in Electron/Chromium: Regeln greifen, Akzentfarbe, schmales Fenster, keine Fremdadressen, kein Weichzeichner auf Karten). **Nicht** geprüft ist es gegen einen laufenden Jellyfin-Server und eine installierte Media Bar: Klassennamen und Aufbau stammen aus der Kenntnis des Web-Clients (10.9 bis 10.11, nicht gegen eine laufende Installation abgeglichen) und aus `slideshowpure.css` (v6.1.4). Zeigt etwas in deiner Version anders, im Browser **F12** drücken, das Element anklicken und den Klassennamen ansehen: Abweichungen lassen sich meist mit einer Zeile in Abschnitt 5 bis 9 beheben. `!important` steht dort, wo Jellyfin eigene Farben erzwingt.
