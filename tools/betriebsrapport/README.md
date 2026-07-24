# Betriebsrapport

Persönlicher Gesundheits-Tracker: Schlaf, Mahlzeiten, Wasser, Alkohol, Substanzen.
Eine statische Web-App. Kein Server, kein Konto, keine KI, keine Abhängigkeit von irgendeinem Anbieter.

## Dateien

| Datei | Zweck |
|---|---|
| `index.html` | Kleiner Loader für die Pages-App. |
| `part-*.b64` | Die eigentliche App, statisch codiert und vom Loader zusammengesetzt. |
| `manifest.webmanifest` | Macht die App zur PWA (Home-Screen-Icon, Vollbild ohne Browserleiste). |
| `sw.js` | Service Worker für Offline-Betrieb. |
| `icon.svg` | App-Icon für Browser und PWA. |

Die App hat keine externen Abhängigkeiten und lädt keine Schrift, Analyse oder Drittanbieter-Ressource nach.

## Deploy auf GitHub Pages

Die App wird in diesem Repository unter `tools/betriebsrapport/` veröffentlicht. Die GitHub-Pages-Adresse endet entsprechend auf `/tools/betriebsrapport/`. Sie ist bewusst nicht auf der Spiele-Startseite verlinkt und enthält `noindex`; die URL ist trotzdem nicht privat.

Änderungen: Dateien im Repo bearbeiten und committen. Bei Änderungen an der App die Cache-Version in `sw.js` erhöhen, zum Beispiel von `rapport-v2` auf `rapport-v3`. Dadurch wird der alte Offline-Cache ersetzt.

## Aufs Handy holen

**iPhone:** URL in Safari öffnen → Teilen-Symbol → *Zum Home-Bildschirm*.
**Android:** URL in Chrome öffnen → Menü → *App installieren*.

Danach hat die App ein eigenes Icon, startet ohne Browserleiste und funktioniert offline.

## Tägliche Erinnerung

Diese Version plant selbst keine täglichen Benachrichtigungen. Zwei einfache Wege:

- **Erinnerungen-App:** Täglicher Eintrag um 21:30 mit der URL im Notizfeld.
- **Kurzbefehle (iOS), empfohlen:** *Automation → Tageszeit → 21:30 → Aktion «Web-Seite öffnen»*
  mit deiner Pages-URL, «Vor dem Ausführen fragen» deaktivieren. Dann öffnet sich der
  Rapport um 21:30 direkt. Auf Android geht das mit einer entsprechenden Routine.

## Wo die Daten liegen

Im `localStorage` des Browsers, auf dem Gerät, unter dem Schlüssel `betriebsrapport-v1`.
Sie verlassen das Gerät nie. Das heisst auch:

- Handy und Laptop haben **getrennte** Datenbestände.
- Browserdaten löschen, App deinstallieren oder «Website-Daten entfernen» löscht den Rapport.
- Browser oder Betriebssystem können Website-Daten beim Bereinigen oder unter Speicherdruck entfernen.
- Die Daten sind nicht verschlüsselt. Wer Zugriff auf dein entsperrtes Browserprofil hat, kann sie lesen.

**Darum: unter *Ziele → Export* regelmässig die JSON-Datei sichern.** Die Exportdatei enthält sensible Angaben und sollte entsprechend geschützt werden. Sie enthält alles und lässt sich auf jedem Gerät per *Import* wieder einlesen.

## Datenformat

```json
{
  "days": {
    "2026-07-24": {
      "sleep": 6.5,
      "meals": 2,
      "water": 1200,
      "alcohol": 3,
      "drugs": { "count": 1, "note": "..." }
    }
  },
  "targets": { "sleepMin": 7, "sleepMax": 9, "meals": 3, "waterMl": 2000, "alcoholMax": 2 }
}
```

`null` heisst *nicht erfasst* und zählt nirgends mit. `0` heisst *bewusst als Null gemeldet* und zählt als Ziel erreicht.

## Auswertung

- **Pünktlichkeit** — Tagesscore über alle erfassten Metriken.
- **Anzeigetafel** — Durchschnitt über 7, 14 oder 30 Tage gegen den Sollwert.
- **Fahrplan** — 28 Tage × 5 Metriken als Matrix.
- **Wochenmuster** — Durchschnittlicher Alkohol je Wochentag.

Falls das Tracking allein nicht reicht: [safezone.ch](https://www.safezone.ch) berät anonym und kostenlos.
