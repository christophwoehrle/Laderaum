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
| Länge | 13,50 m |
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

- Alle Maßangaben in Millimeter (Länge × Breite × Höhe). In der Pakettabelle
  wird i. d. R. nur die **Länge** eingegeben; **Breite (Vorgabe 1100 mm)**
  und **Höhe (Vorgabe 700 mm)** sind voreingestellt und per Klick änderbar.
- Pakete werden **nicht gedreht** – die Länge bleibt immer parallel zur
  Fahrtrichtung; verschoben wird nur längs und quer.
- **10 mm Abstand** in alle Richtungen zwischen den Paketen.
- **Unterlagshölzer** (Höhe 72 mm, 2 Stück) unter jedem Paket – auch beim
  Stapeln liegen sie jeweils unter dem aktuellen Paket.
- Von unten nach oben wird der Innenraum maximal ausgenutzt.
- Obere Lagen stehen nur auf **vollständig tragender** unterer Lage,
  **kein Überstand** in Längsrichtung, **kein Schweben**. Ein kürzeres
  (oder gleich langes) Paket **gleicher Breite** darf auf einem längeren
  stehen – dadurch wird der Laderaum maximal genutzt.
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
- **Auftrag speichern / laden**: Eingaben werden automatisch im Browser
  gespeichert (überstehen einen Reload) und lassen sich als JSON-Datei
  exportieren bzw. wieder importieren (Archiv/Weitergabe).
- Optionales **m³-netto-Feld** je Paket (Netto-Holzvolumen): ist es gesetzt,
  bestimmt es das Gewicht (Gewicht = m³ netto × Faktor); der Raumbedarf/die
  90-m³-Grenze bleibt über das Umriss-Volumen L×B×H. Leer = Volumen aus L×B×H.
- Kennzahlen: geladene Pakete, Gewicht, Volumen (Raum) inkl. Netto-Holzvolumen,
  Lademeter, Lagen.
- **Beladeplan drucken / als PDF speichern**: druckoptimierte Ausgabe mit
  Auftrag/Datum, Kennzahlen, Lastverteilung, Seiten- und Draufsicht sowie
  der Liste nicht geladener Pakete (Eingabefelder, 3D und Bedienelemente
  werden im Druck ausgeblendet; die Ansichten drucken vektorscharf).
- **Aufstellung nicht geladener Pakete** mit Grund je Paketnummer:
  zu groß für den Laderaum (inkl. überschrittenem Maß), kein Platz auf der
  Ladefläche, Maximalgewicht (24 t) oder Maximalvolumen (90 m³) erreicht.
- Responsive **Seitenansicht** (mit Rädern) und **Draufsicht** (ohne Räder).
- **Manuelle Anordnung** (Drag & Drop, Draufsicht): Pakete lassen sich frei
  greifen und verschieben, in die **Ablage „neben dem LKW"** ziehen (entfernen)
  und von dort wieder platzieren; ein Paket auf ein anderes ziehen stapelt es.
  Die Länge bleibt immer fixiert (kein Drehen). Live-Anzeige von geladenen
  Paketen, Gewicht, Sattel-/Achslast und Schwerpunkt sowie eine mitlaufende
  Seitenansicht; „↺ Auto-Anordnung" stellt die automatische Beladung wieder her.
- **Frei drehbare 3D-Simulation** der Beladung (eigenständiger Canvas-
  Renderer ohne Fremdbibliothek): Drehen per Maus/Touch, Zoomen per
  Scrollrad, Auto-Rotation und „Ansicht zurücksetzen".
- **Lastverteilung / Achslasten** (Lastverteilungsplan): Nutzlast wird nach
  dem Hebelgesetz auf Königszapfen (Sattellast) und Achsaggregat (Achslast)
  verteilt; Lastschwerpunkt, zulässiger Schwerpunktbereich und Ampelstatus
  werden angezeigt. Die Reihen werden längs automatisch so verschoben, dass
  der Schwerpunkt möglichst im zulässigen Fenster liegt. Fahrzeugdaten
  (Königszapfen-/Achsposition, max. Sattel-/Achslast) sind einstellbar.
  Optional kann das **Eigengewicht (Tara)** als Grundlast auf Königszapfen
  und Achsaggregat angegeben werden – es wird zur Nutzlast addiert, sodass
  die Grenzwerte gegen die Gesamtlast geprüft werden (0 = nur Nutzlast).
  Zusätzlich kann eine **Mindest-Sattellast** (Untergrenze Königszapfen)
  vorgegeben werden – sie sichert Traktion/Lenkstabilität, begrenzt den
  Schwerpunkt nach hinten und wird geprüft (Warnung bei Unterschreitung).

## Berechnungslogik (Kurz)

1. Pro Paket wird die maximale Lagenzahl bestimmt
   (Höhenbegrenzung bzw. max. 3 bei Höhe ≤ 780 mm).
2. Säulen werden gebildet – entweder aus gleicher Paketnummer/Maßen oder
   **gemischt** (kürzeres/gleich langes Paket auf längerem, gleiche Breite,
   volle Auflage). Beide Varianten gehen ins Ensemble ein; es gewinnt die
   mit den meisten geladenen Paketen.
3. Grundflächen-Packung: es werden mehrere Misch-Strategien gerechnet und
   die Variante mit den **meisten geladenen Paketen** übernommen –
   (a) gemischte Reihen (breitester Stapel gibt die Reihenbreite vor, Rest
   kürzeste-zuerst), (b) nach Breite gruppierte Reihen mit Knapsack-Auswahl
   über die Ladebreite (kein Breitenverlust) und (c) ausgewogene Reihen
   (LPT: längster Stapel zuerst in die leerste Reihe – entspricht dem realen
   Beladen „lange Pakete hinter die Kabine, kurze dahinter" und füllt Reihen
   gleichmäßig dicht) und (d) ein **lagenweiser Packer**: Basislage füllen,
   obere Lagen auf die tragende Länge darunter – ein Paket darf über
   mehreren unteren liegen („Brücke"), verboten ist nur ein Überstand in
   Längsrichtung. So wird die Anordnung je nach Maßen automatisch für
   maximale Beladung gewählt.
4. Emission Lage für Lage von unten nach oben unter Beachtung von
   Gewicht (24 t) und Volumen (90 m³).
5. Längs-Optimierung der Lastverteilung: Die Reihen werden innerhalb ihres
   freien Spielraums verschoben, bis der Lastschwerpunkt im zulässigen
   Fenster liegt (Achslasten nach Hebelgesetz). Reicht das nicht aus, wird
   gewarnt (Ladung reduzieren bzw. anders verteilen).
