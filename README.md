# Verbandskasten-Kontrolle

Web-App zum Prüfen von Verbandskästen: Inhalt zählen, Haltbarkeitsdaten erfassen, Fehlmengen automatisch als Bestell-E-Mail erzeugen. Läuft als installierbare PWA vom Home-Bildschirm, offline, ohne Server und ohne Build-Schritt.


## Auf GitHub Pages veröffentlichen

1. Neues Repository anlegen, z. B. `verbandskasten`. Sichtbarkeit **Public** (bei privaten Repos braucht Pages einen kostenpflichtigen Plan).
2. Alle Dateien dieses Ordners ins Repo hochladen — Struktur beibehalten:
   ```
   index.html
   manifest.webmanifest
   sw.js
   icons/
   ```
   Über die Weboberfläche: **Add file → Upload files**, den Ordner `icons` per Drag-and-drop mitziehen.
3. **Settings → Pages → Source: „Deploy from a branch"**, Branch `main`, Ordner `/ (root)`, **Save**.
4. Nach ein bis zwei Minuten ist die App erreichbar unter:
   `https://DEIN-BENUTZERNAME.github.io/verbandskasten/`

HTTPS ist Pflicht für Service Worker und Installation — GitHub Pages liefert das automatisch mit.

## Auf dem iPhone/iPad als App einrichten

1. Die Adresse **in Safari** öffnen (nicht Chrome — nur Safari darf auf dem Home-Bildschirm installieren).
2. Auf **Teilen** tippen → **Zum Home-Bildschirm**.
3. Die App startet danach im Vollbild ohne Safari-Leiste, mit eigenem Icon.

Unter Android: Chrome → Menü → **App installieren**.

## Was die App macht

- **Kästen verwalten** — beliebig viele, je mit Standort, Norm-Vorlage und Prüfintervall.
- **Vorlagen** — DIN 13164 (Kfz), DIN 13157 (Betrieb klein), DIN 13169 (Betrieb groß) oder leer.
- **Prüfen** — pro Artikel Ist-Menge über Plus/Minus und Haltbarkeitsdatum monatsgenau. Der farbige Balken links zeigt sofort: vollständig, läuft ab, Fehlmenge, abgelaufen.
- **Bestellen** — alle Fehlmengen und abgelaufenen Positionen werden gesammelt. Abgelaufene Artikel werden komplett zum Austausch vorgeschlagen, Fehlmengen nur in der Differenz (3 von 4 vorhanden → 1 Stück). „E-Mail schreiben" öffnet den fertigen Entwurf mit Bezeichnung, Artikelnummer, Menge und Grund. Gesendet wird erst per Tipp in der Mail-App.
- **Protokoll** — jede abgeschlossene Prüfung wird mit Datum, Prüfer und Ergebnis dokumentiert.
- **Sammelbestellung** — Positionen über alle Kästen hinweg zusammengefasst.

## Einstellungen

Zahnrad oben rechts: Empfängeradresse für Bestellungen (voreingestellt `verbandstest@gmail.com`), Name des Prüfers, Vorwarnzeit für ablaufende Haltbarkeit (Standard 3 Monate).

## Daten

Alles liegt ausschließlich lokal im Browser des Geräts (`localStorage`) — nichts wird übertragen. Daraus folgt:

- Jedes Gerät hat seinen eigenen Datenbestand.
- Websitedaten löschen oder „Website-Daten entfernen" in den iOS-Einstellungen löscht auch die Prüfdaten.
- Vor Gerätewechsel: **Einstellungen → Exportieren**, die JSON-Datei sichern, auf dem neuen Gerät importieren.

Für mehrere Prüfer mit gemeinsamem Datenbestand wäre ein Backend nötig (z. B. Supabase oder Firebase) — die Datenstruktur in `index.html` (`state.boxes`) ist darauf vorbereitet.

## Anpassen

- **Artikelnummern**: Die Vorlagen enthalten Platzhalter (`VK-13164-07`). In der Prüfansicht bei jedem Artikel auf „bearbeiten" tippen und durch die echte Nummer des Lieferanten ersetzen — genau dieser Wert landet in der Bestellung.
- **Inhaltslisten**: Die Konstanten `T13164` und `T13157` in `index.html` bearbeiten. Format: `["Art.-Nr.", "Bezeichnung", Sollmenge, 1]` — die optionale `1` am Ende markiert Artikel ohne Haltbarkeitsdatum (z. B. Schere).
- **Farben**: Alle Werte stehen als CSS-Variablen im `:root`-Block ganz oben.

Nach jeder Änderung an `index.html` in `sw.js` die Zeile `const CACHE = "vk-v1"` hochzählen (`vk-v2`, …), sonst laden bereits installierte Geräte weiter die alte Version aus dem Cache.

## Hinweis

Die mitgelieferten Inhaltslisten sind Vorlagen zur Arbeitserleichterung und ersetzen keine Rechtsberatung. Maßgeblich sind die jeweils gültige Fassung der DIN-Norm, die Angaben des Herstellers und die betrieblichen Vorgaben. Sollmengen und Positionen bitte vor dem ersten Einsatz gegen die aktuelle Norm abgleichen.
