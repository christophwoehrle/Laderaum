# Laderaumoptimierung Sattelzug (Tautliner)

HTML-Anwendung für die Logistikabteilung zur Beladungsoptimierung eines
Sattelzugs (Tautliner) mit Holzpaketen (Bretter, Latten, Balken auf
Kanthölzern). Ziel ist das Verladen der maximalen Paketanzahl unter
Einhaltung aller Rahmenbedingungen – mit Seitenansicht und Draufsicht.

## Nutzung

`index.html` im Browser öffnen – keine Installation nötig.
Für den PDF-Import und die Visualisierungsschrift wird eine
Internetverbindung benötigt (pdf.js über CDN).

## Rahmendaten des LKW

| Größe | Wert |
|-------|------|
| Länge | 13,60 m |
| Breite | 2,48 m |
| Höhe | 2,80 m |
| Max. Volumen | 90 m³ |
| Max. Gewicht | 24 t |

## Kategorien & Gewichtsfaktoren

Gewicht = Volumen × Faktor (kg/m³)

| Kategorie | Faktor | Referenzvolumen |
|-----------|--------|-----------------|
| Seitenware frisch | 750 | 32 m³ ≙ 24 t |
| Seitenware getrocknet | 460 | 52 m³ ≙ 24 t |
| Schnittholz frisch | 570 | 42 m³ ≙ 24 t |
| Schnittholz getrocknet | 460 | 52 m³ ≙ 24 t |

## Beladungsregeln

- Alle Maßangaben in Millimeter (Länge × Breite × Höhe).
- Pakete werden **nicht gedreht** – die Länge bleibt immer parallel zur
  Fahrtrichtung; verschoben wird nur längs und quer.
- **10 mm Abstand** in alle Richtungen zwischen den Paketen.
- **Unterlagshölzer** (Höhe 72 mm, 2 Stück) unter jedem Paket – auch beim
  Stapeln liegen sie jeweils unter dem aktuellen Paket.
- Von unten nach oben wird der Innenraum maximal ausgenutzt.
- Obere Lagen stehen nur auf **vollständig tragender** unterer Lage,
  **kein Überstand** in Längsrichtung, **kein Schweben**.
- Paket­höhe ≤ 780 mm → **maximal 3 Lagen** (darüber höhenbegrenzt).
- Gewicht (24 t) und Volumen (90 m³) werden lagenweise berücksichtigt.
- Das **Maximalgewicht (24 t) lässt sich per Schalter deaktivieren**: das
  Gewicht wird dann weiterhin berechnet und angezeigt, aber nicht mehr als
  Beladegrenze angewendet (Warnhinweis bei Überschreitung).

## Funktionen

- Auftragsnummer und Datum (Kalender, ab tagesaktuellem Datum).
- Pakettabelle: Paketnr., L × B × H (mm), Anzahl, Kategorie – jede
  Paketnummer erhält eine durchgehend eindeutige Farbe.
- **Import aus PDF** (best effort): erkennt Zeilen mit Maßangaben
  (L × B × H) und optionaler Stückzahl sowie Auftragsnummer/Datum.
- Kennzahlen: geladene Pakete, Gewicht, Volumen, Lademeter, Lagen.
- Responsive **Seitenansicht** (mit Rädern) und **Draufsicht** (ohne Räder).

## Berechnungslogik (Kurz)

1. Pro Paket wird die maximale Lagenzahl bestimmt
   (Höhenbegrenzung bzw. max. 3 bei Höhe ≤ 780 mm).
2. Stapel gleicher Paketnummer garantieren volle Auflage ohne Überstand.
3. Grundflächen-Packung reihenweise über die Breite: je Reihe wird der
   breiteste Stapel gesetzt und der Rest kürzeste-zuerst aufgefüllt
   (maximale Stapelanzahl je Reihe).
4. Emission Lage für Lage von unten nach oben unter Beachtung von
   Gewicht (24 t) und Volumen (90 m³).
