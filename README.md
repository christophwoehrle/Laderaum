# Laderaumoptimierung Sattelzug (Tautliner)

HTML-Anwendung für die Logistikabteilung zur Beladungsoptimierung eines
Sattelzugs (Tautliner) mit Holzpaketen (Bretter, Latten, Balken auf
Kanthölzern). Ziel ist das Verladen der maximalen Paketanzahl unter
Einhaltung aller Rahmenbedingungen – mit Seitenansicht und Draufsicht.

## Nutzung

`index.html` im Browser öffnen – keine Installation nötig.
Für den PDF-Import wird eine Internetverbindung benötigt
(pdf.js für Text-PDFs, Tesseract.js für die OCR gescannter PDFs – beide über CDN).

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

- **Mehrere LKW automatisch**: Reicht ein Sattelzug nicht aus, wird die
  Ladung automatisch auf einen 2., 3., … x. LKW verteilt (jeder LKW wird
  für sich optimal gepackt; Gewicht 24 t, Volumen 90 m³ und Achslasten
  gelten je Fahrzeug). Alle LKW werden **nebeneinander** angezeigt
  (quer scrollbar) und **durchnummeriert** („LKW 1 / N"). Seiten-, Drauf-
  und 3D-Ansicht sowie die **frei verschiebbare** Anordnung gibt es je LKW
  nebeneinander; eine Kopfzeile nennt die Gesamtzahl der benötigten LKW.
- Auftragsnummer und Datum (Kalender, ab tagesaktuellem Datum).
- Pakettabelle: Paketnr., L × B × H (mm), Anzahl, Kategorie – jede
  Paketnummer erhält eine durchgehend eindeutige Farbe.
- **Import aus PDF** (einheitliche Logik – jedes PDF wird gleich behandelt):
  erkennt das Sägewerks-/AV-Listen-Format und wendet für jede eingelesene
  Datei dieselbe Parselogik an. Je Längen-Zeile werden erkannt: Brett-
  Querschnitt **B×H**, **Länge** (m mit Komma), **Gesamt-Stückzahl**,
  Bretter **breit×hoch** je Paket und **Zwischenlatte S<nn>** (Höhe in mm,
  z. B. S18→18 mm, S12→12 mm). Daraus werden erzeugt:
  - **Anzahl Pakete** = Stück ÷ (breit × hoch) – 0-Stück-Zeilen entfallen,
  - **Länge** = L, **Breite** = breit × Brett-H,
  - **Höhe** = hoch × Brett-B + (hoch-1) × Latte,
  - **m³ netto** = (Stück ÷ Anzahl) × Brett-B × Brett-H × L (je Paket) –
    bei vollen Bündeln entspricht das breit × hoch, bei Teilbündeln (z. B.
    Handelsware **HA** mit 5 Brettern in einem 3×2-Raster) entsprechend
    weniger. Die Warenart (HW Hobelware, SW Sägeware, HA Handelsware) wird
    beim Packen gleich behandelt; die Kategorie/Gewichtsfaktor bitte prüfen.
    **Handelsware (HA)** wird als solche markiert: die Paketnummer bekommt
    das Präfix „HA·" und in der Legende erscheint ein Badge „Handelsware".

  Die AV-Listen-Nr. wird als Auftragsnummer übernommen. Ist ein PDF ein
  **Scan ohne Textebene**, wird automatisch **OCR (Tesseract.js)** angewandt
  und der erkannte Text durch dieselbe Parselogik geführt. Wird kein
  Sägewerks-Format erkannt, greift ein generischer L×B×H-Fallback.
  (Nur in der Standalone-`index.html`/auf der gehosteten Seite – im Artifact
  sind pdf.js/OCR aus Sicherheitsgründen deaktiviert.)
- **Alle löschen**: leert die Pakettabelle mit einem Klick (nach Rückfrage) –
  alle Zeilen werden entfernt und eine leere Zeile bleibt zum Erfassen eines
  neuen Auftrags.
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
- **Bretter- und Latten-Darstellung**: Stammt ein Paket aus dem PDF-Import
  (oder ist der Aufbau hinterlegt), werden die einzelnen Bretter und die
  Zwischenlatten gezeichnet – in der Seitenansicht die Brettlagen mit den
  Latten dazwischen, in der Draufsicht die nebeneinander liegenden Bretter
  (breit × hoch, Brett-Querschnitt und Lattenhöhe aus dem Auftrag).
- **Manuelle Anordnung** (Drag & Drop, Draufsicht): Pakete lassen sich frei
  greifen und verschieben, in die **Ablage „neben dem LKW"** ziehen (entfernen)
  und von dort wieder platzieren; ein Paket auf ein anderes ziehen stapelt es.
  Die Länge bleibt immer fixiert (kein Drehen). Live-Anzeige von geladenen
  Paketen, Gewicht, Sattel-/Achslast und Schwerpunkt sowie eine mitlaufende
  Seitenansicht; „↺ Auto-Anordnung" stellt die automatische Beladung wieder
  her, „🗑 Alle entfernen" räumt den LKW leer (alle Pakete wandern in die
  Ablage), um von Hand neu zu beladen.
- **Frei drehbare 3D-Simulation** der Beladung (eigenständiger Canvas-
  Renderer ohne Fremdbibliothek): Drehen per Maus/Touch, Zoomen per
  Scrollrad, Auto-Rotation und „Ansicht zurücksetzen".
- **Lastverteilung / Achslasten** (Lastverteilungsplan): Nutzlast wird nach
  dem Hebelgesetz auf Königszapfen (Sattellast) und Achsaggregat (Achslast)
  verteilt; Lastschwerpunkt, zulässiger Schwerpunktbereich und Ampelstatus
  werden angezeigt. Die Beladung beginnt **immer bündig an der vorderen
  Ladewand** (Stirnwand) – so entsteht vorne kein Leerraum; nur wenn dadurch
  die zulässige Sattellast überschritten würde, werden die Reihen so wenig
  wie nötig nach hinten verschoben. Fahrzeugdaten
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
   Längsrichtung. Und (e) ein **Boden-voll-Packer**: die unterste Lage wird
   je Reihe möglichst über die volle Ladelänge (13,50 m) gefüllt, indem
   mehrere Pakete hintereinander kombiniert werden (Best-Fit-Decreasing in
   Reihen à 13,50 m); die längsten Reihen kommen nach unten, kürzere darüber
   (volle Auflage). So wird der LKW von unten nach oben maximal beladen.
   Bei Gleichstand der Paketanzahl gewinnt die Variante mit der **am besten
   (länger) gefüllten Bodenlage**.
4. Emission Lage für Lage von unten nach oben unter Beachtung von
   Gewicht (24 t) und Volumen (90 m³).
5. Beladung immer bündig an der vorderen Ladewand (Stirnwand) beginnen –
   jede Reihe startet vorne, es entsteht vorne kein Leerraum. Der
   Lastschwerpunkt bleibt damit so weit vorne wie möglich; nur wenn die
   zulässige Sattellast (Königszapfen) dadurch überschritten würde, werden
   die Reihen innerhalb ihres freien (hinteren) Spielraums so wenig wie
   nötig nach hinten verschoben. Reicht auch das nicht aus, wird gewarnt.
6. Reicht ein LKW nicht aus, wird automatisch ein weiterer befüllt (siehe
   „Mehrere LKW automatisch").
