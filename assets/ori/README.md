# Ori-Bildassets

Dieser Ordner enthält die verbindlichen Bildreferenzen und Sprite-Sheets für Ori. Die Dateien sollen von allen Spielen gemeinsam verwendet werden, damit Aussehen, Proportionen und Details des Charakters konsistent bleiben.

## Aktueller Bestand

```text
assets/ori/
├── README.md
├── reference/
│   └── ori_character_master.png
└── sprite_sheets/
    └── flight/
        ├── ori_flight_front_4x3.png
        ├── ori_flight_left_4x3.png
        ├── ori_flight_right_4x3.png
        └── ori_flight_back_4x3.png
```

| Datei | Inhalt | Vorgesehene Verwendung |
|---|---|---|
| `reference/ori_character_master.png` | Verbindliches Hauptbild von Ori | Charakterreferenz für neue Bilder, Sprites und Animationen |
| `sprite_sheets/flight/ori_flight_front_4x3.png` | Flug- und Schwebevarianten von vorne | Frontale Flugdarstellung, Menüs und ruhige Schwebeanimationen |
| `sprite_sheets/flight/ori_flight_left_4x3.png` | Flug- und Schwebevarianten nach links | Bewegung beziehungsweise Blickrichtung nach links |
| `sprite_sheets/flight/ori_flight_right_4x3.png` | Flug- und Schwebevarianten nach rechts | Bewegung beziehungsweise Blickrichtung nach rechts |
| `sprite_sheets/flight/ori_flight_back_4x3.png` | Flug- und Schwebevarianten von hinten | Wegflug, Drehungen und Szenen mit Blick vom Spieler weg |

## Verbindliche Charakterreferenz

`ori_character_master.png` ist die wichtigste Referenz. Neue Darstellungen müssen sich daran orientieren und dürfen den Charakter nicht neu interpretieren.

Besonders konsistent bleiben müssen:

- Kopfform und Körperproportionen
- Form, Länge, Krümmung und Struktur der Hörner
- Form und Position der Ohren
- Anzahl, Position und Form der Kristallohrringe
- Augenform und Augenfarben
- beschädigte Gesichtshälfte und das veränderte Auge immer auf derselben Seite
- kleiner Kopfkamm
- Hände, Finger, Füsse und Zehen
- dunkelblauer Mantel mit Sternenmuster
- Farbpalette, Materialwirkung und Beleuchtungsstil

Neue Frames dürfen keine wechselnden Hörner, Schmuckstücke, Augen, Fingerzahlen oder Manteldetails enthalten.

## Aufbau der Flug-Sprite-Sheets

Alle aktuellen Flugdateien verwenden ein Raster aus **4 Spalten × 3 Reihen** und enthalten damit **12 Frames**.

Die Frames werden zeilenweise gelesen:

```text
01  02  03  04
05  06  07  08
09  10  11  12
```

Im Code entspricht das normalerweise den Frame-Indizes `0` bis `11`.

```js
const COLUMNS = 4;
const ROWS = 3;
const FRAME_COUNT = COLUMNS * ROWS;

function drawOriFrame(ctx, image, frameIndex, x, y, width, height) {
  const frameWidth = image.naturalWidth / COLUMNS;
  const frameHeight = image.naturalHeight / ROWS;
  const frame = ((frameIndex % FRAME_COUNT) + FRAME_COUNT) % FRAME_COUNT;
  const column = frame % COLUMNS;
  const row = Math.floor(frame / COLUMNS);

  ctx.drawImage(
    image,
    column * frameWidth,
    row * frameHeight,
    frameWidth,
    frameHeight,
    x,
    y,
    width,
    height
  );
}
```

## Animationshinweise

Die zwölf Bilder eines Sheets sind aktuell Variationen einer Flug- beziehungsweise Schwebepose. Sie können als ruhige Animation verwendet werden, sollten aber vor der endgültigen Integration visuell in ihrer Reihenfolge geprüft werden.

Empfohlener Ausgangspunkt:

- ruhiges Schweben: etwa `6–10 FPS`
- sanfter Wechsel zwischen Posen
- keine starke Skalierung oder harte Rotation des gesamten Sprites
- Frame 12 soll möglichst weich zu Frame 1 zurückführen
- Figurengrösse und Mittelpunkt während der Animation stabil halten

Für eine besonders saubere Endlosschleife kann später eine explizite Frame-Reihenfolge in einer JSON-Datei ergänzt werden.

## Technische Anforderungen

Für produktive Spielassets gelten folgende Ziele:

- PNG mit echter RGBA-Transparenz
- kein Hintergrund und kein Bodenschatten
- keine Texte, Zahlen, Rahmen oder Rasterlinien
- keine abgeschnittenen Hörner, Ohren, Hände, Füsse oder Mantelteile
- identische Zellengrösse innerhalb eines Sprite-Sheets
- möglichst einheitlicher Mittelpunkt und Pivot
- ausreichend transparenter Abstand zu allen Zellrändern

Vor dem Einsatz im Spiel sollten Alpha-Kanal, Zellgrenzen und Frame-Ausrichtung automatisiert oder visuell geprüft werden.

## Pfade im Spiel

Beispiel aus einem Spielordner wie `sternenflug_v3/`:

```js
const oriFront = new Image();
oriFront.src = "../assets/ori/sprite_sheets/flight/ori_flight_front_4x3.png";
```

Das Hauptbild kann so geladen werden:

```js
const oriReference = new Image();
oriReference.src = "../assets/ori/reference/ori_character_master.png";
```

Die Pfade sollen relativ bleiben, damit die Spiele lokal, über GitHub Pages und in anderen statischen Hosts funktionieren.

## Benennung zukünftiger Dateien

Bitte nur kleine Buchstaben, englische Bezeichnungen und Unterstriche verwenden.

Schema für Sprite-Sheets:

```text
ori_<aktion>_<ansicht>_<spalten>x<reihen>.png
```

Beispiele:

```text
ori_flight_front_4x3.png
ori_idle_front_4x3.png
ori_boost_right_4x3.png
ori_hit_left_4x2.png
ori_magic_front_4x3.png
```

Schema für einzelne Frames:

```text
ori_<aktion>_<ansicht>_<frame>.png
```

Beispiele:

```text
ori_flight_front_01.png
ori_flight_front_02.png
ori_boost_right_01.png
```

## Sinnvolle zukünftige Erweiterungen

```text
assets/ori/
├── reference/
├── sprite_sheets/
│   ├── flight/
│   ├── idle/
│   ├── boost/
│   ├── hit/
│   ├── magic/
│   └── reactions/
├── frames/
│   ├── flight/
│   └── reactions/
├── atlases/
└── metadata/
```

Als nächste Animationszustände sind besonders sinnvoll:

- ruhiges Schweben
- Steigen
- Sinken
- schneller Boost
- Trefferreaktion
- Nova beziehungsweise Magie
- Einsammeln eines Sterns
- Sieg
- Schlafen oder Game Over

## Grundregel

Das Hauptbild definiert **wer Ori ist**. Die Sprite-Sheets definieren **wie Ori sich bewegt**. Neue Assets müssen beides respektieren: Die Bewegung darf variieren, das Charakterdesign nicht.
