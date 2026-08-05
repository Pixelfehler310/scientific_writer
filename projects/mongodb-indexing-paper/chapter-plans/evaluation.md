# Kapitelplan: evaluation — Von Szenarioergebnissen zur bedingten Empfehlung

## Funktion im Gesamtargument

Das Kapitel trennt Beobachtung, technische Erklärung, Kostenabwägung und Reichweitenbegrenzung. Eigene empirische Evidenz trägt die Setbewertung; Literatur dient nur zur Erklärung und Begrenzung.

## Teilfrage und erwartetes Ergebnis

Welche Sets wirken je Query-Variante, welche Speicher-/Write-Kosten entstehen und welche Empfehlung folgt unter Lese- beziehungsweise Ressourcenpriorität? Ergebnis ist eine begrenzte Antwort, keine globale Rangliste.

## Wortbudget

1.250 Wörter.

## Voraussetzungen und Übergabe

Voraussetzungen sind ein eingefrorener erfolgreicher Referenzlauf, übereinstimmende Ergebnissignaturen und vollständige Manifeste. Das Kapitel übergibt an `conclusion` die expliziten Antwortbausteine und Grenzen.

## Absatzplan

### EV-04-P01 — Gültigkeit des Referenzlaufs

- **Funktion:** Bevor Ergebnisse erscheinen, Datengrundlage und Validierungsstatus feststellen.
- **Kernaussage:** Ausgewertet wird genau der manifestierte Lauf mit dokumentierter Version, Konfiguration und fachlich gleichen Ergebnissen.
- **Begründung:** Ohne Gültigkeitsnachweis wären Performanceunterschiede nicht interpretierbar.
- **Evidenzbedarf:** internes Manifest, Logs und Signaturen.
- **Evidenztyp:** `internal`
- **Evidenzstatus:** `planned`
- **Beziehung davor:** Übernahme des Methodenprotokolls.
- **Beziehung danach:** Folge — scenario-lokale Ergebnisse dürfen berichtet werden.
- **Zielwörter:** 110.
- **Medium:** keine.
- **Offene Recherche:** keine; praktische Evidenz fehlt bis Referenzlauf.

### EV-04-P02 — Q1: Bereichsbreite und Sortierung

- **Funktion:** Q1-eng/breit zunächst beschreibend vergleichen.
- **Kernaussage:** Unterschiede zwischen Sets werden anhand Median/IQR und Scan-/Sortierstruktur für beide Bereichsbreiten getrennt berichtet.
- **Begründung:** Zusammenfassung würde den Selektivitätseffekt verdecken.
- **Evidenzbedarf:** eigene Q1-Metriken und Explain-Artefakte.
- **Evidenztyp:** `internal`
- **Evidenzstatus:** `planned`
- **Beziehung davor:** Folge.
- **Beziehung danach:** Fortführung mit anderer Selektivitätsform in Q2.
- **Zielwörter:** 160.
- **Medium:** Teil von EV-M01.
- **Offene Recherche:** keine Ergebnisrichtung vorwegnehmen.

### EV-04-P03 — Q2: häufiges und seltenes Tag

- **Funktion:** Multikey-Wirkung über zwei Taghäufigkeiten vergleichen.
- **Kernaussage:** Q2-häufig/selten zeigen getrennt, wie Tagverteilung und zusätzliche Aktivitätsprüfung Suchaufwand und Latenz beeinflussen.
- **Begründung:** Q2 unterscheidet L/W1 und das kompaktere W2 besonders über Schlüsselumfang und residuale Prüfung.
- **Evidenzbedarf:** eigene Q2-Metriken, Tagzählungen und Explain-Pläne.
- **Evidenztyp:** `internal`
- **Evidenzstatus:** `planned`
- **Beziehung davor:** Fortführung.
- **Beziehung danach:** Kontrast zur Spezialindex-/Wiederverwendungsfrage in Q3.
- **Zielwörter:** 155.
- **Medium:** Teil von EV-M01.
- **Offene Recherche:** keine.

### EV-04-P04 — Q3 und Q4: Spezialindex, Wiederverwendung und Kontrolle

- **Funktion:** Q3-Designkontrast erklären und Q4 knapp als Kontrolle dokumentieren.
- **Kernaussage:** Q3 prüft, ob der Spezialindex von L gegenüber W1/W2 einen materiellen Vorteil bietet; Q4 bestätigt die gemeinsame Unique-Basis und entscheidet die Empfehlung nicht.
- **Begründung:** Dies ist der Kernvergleich query-lokal versus workloadorientiert.
- **Evidenzbedarf:** eigene Q3-/Q4-Metriken und Pläne.
- **Evidenztyp:** `internal`
- **Evidenzstatus:** `planned`
- **Beziehung davor:** Kontrast.
- **Beziehung danach:** Synthese der Leseergebnisse über Skalen.
- **Zielwörter:** 165.
- **Medium:** Teil von EV-M01.
- **Offene Recherche:** keine.

### EV-04-P05 — Skalierung und strukturelle Erklärung

- **Funktion:** Ergebnisse über 1.000/10.000/100.000 Dokumente zusammenführen, ohne die Smoke-Stufe gleichzugewichten.
- **Kernaussage:** Die regulären Skalen zeigen, ob absolute Sucharbeit und Latenzunterschiede mit der Datenmenge plausibel wachsen; Planstruktur erklärt, aber ersetzt die Latenz nicht.
- **Begründung:** Einzelwerte einer Größe reichen für die begrenzte Skalierungsaussage nicht.
- **Evidenzbedarf:** eigene skalierte Metriken; Grundlagen zur Selektivität und Explain-Grenze.
- **Evidenztyp:** `internal`
- **Evidenzstatus:** `planned`
- **Beziehung davor:** Synthese.
- **Beziehung danach:** Kontrast — Lesevorteile werden den Kosten gegenübergestellt.
- **Zielwörter:** 150.
- **Medium:** EV-M02.
- **Offene Recherche:** keine.

### EV-04-P06 — Indexspeicher und Write-Batches

- **Funktion:** Gegenkosten der Leseunterstützung darstellen.
- **Kernaussage:** Setgröße, W1-Insert, W2a-Stock und W2b-Preis werden getrennt verglichen, weil nicht indexierte und indexierte Updates unterschiedliche Pflegearbeit auslösen können.
- **Begründung:** Ohne diese Dimensionen wäre „geeignet“ auf Lesen verkürzt.
- **Evidenzbedarf:** eigene Indexgrößen- und Write-Artefakte; Literatur nur zur Erklärung.
- **Evidenztyp:** `internal`
- **Evidenzstatus:** `planned`
- **Beziehung davor:** Kontrast.
- **Beziehung danach:** Synthese zur bedingten Auswahl.
- **Zielwörter:** 170.
- **Medium:** EV-M03.
- **Offene Recherche:** keine.

### EV-04-P07 — Dominanz und bedingte Empfehlung

- **Funktion:** Forschungsfrage anhand der vorab festgelegten Entscheidungslogik beantworten.
- **Kernaussage:** Nicht dominierte Sets werden ohne Gesamtscore für Lese- und Ressourcenpriorität bewertet; bei echtem Zielkonflikt bleiben zwei bedingte Empfehlungen zulässig.
- **Begründung:** Ein erzwungener Sieger würde unbegründete Gewichte verstecken.
- **Evidenzbedarf:** eigene zusammengeführte Messwerte und freigegebene Entscheidungsregel.
- **Evidenztyp:** `own_reasoning`
- **Evidenzstatus:** `planned`
- **Beziehung davor:** Synthese.
- **Beziehung danach:** Einschränkung durch Validitätsgrenzen.
- **Zielwörter:** 175.
- **Medium:** keine zusätzliche Ranglistengrafik; vorhandene Medien referenzieren.
- **Offene Recherche:** Formulierung erst nach vollständiger Ergebnisprüfung.

### EV-04-P08 — Limitationen und Übertragbarkeit

- **Funktion:** Reichweite der Empfehlung begrenzen und alternative Erklärungen sichtbar machen.
- **Kernaussage:** Synthetische Verteilungen, erste Ergebnisseite, Einzelclient, begrenzte Sets, lokale Umgebung und konkrete MongoDB-Version verhindern eine Übertragung der konkreten Rangfolge auf andere Systeme.
- **Begründung:** Die Methode ist übertragbarer als das Messergebnis.
- **Evidenzbedarf:** eigene Designgrenzen; gegebenenfalls Benchmarkmethodik.
- **Evidenztyp:** `own_reasoning`
- **Evidenzstatus:** `planned`
- **Beziehung davor:** Einschränkung.
- **Beziehung danach:** Übergabe an die knappe Schlussantwort.
- **Zielwörter:** 165.
- **Medium:** keines.
- **Offene Recherche:** beobachtete Laufartefakt-Warnungen ergänzen, ohne neue Ergebnisse einzuführen.

## Medienplan

| Medium-ID | Typ | Aussagefunktion | Herkunft/Erzeugung | Beschriftung | Einführung und Interpretation |
| --- | --- | --- | --- | --- | --- |
| EV-M01 | gruppierte Tabelle oder Small Multiples | Median/IQR und Planindikatoren je Q1–Q3-Variante und Set zeigen | Referenzlauf | „Leseergebnisse nach Query-Variante“ | In EV-04-P02 einführen, in P03/P04 abschnittsweise interpretieren |
| EV-M02 | Skalierungsgrafik | reguläre Skalen für ausgewählte strukturelle und Latenzkennzahlen vergleichen | Referenzlauf | „Suchaufwand und Latenz über Bestandsgrößen“ | In EV-04-P05 einführen und Grenzen der 1.000er-Stufe nennen |
| EV-M03 | kompakte Trade-off-Tabelle | Indexspeicher sowie W1/W2a/W2b ohne Gesamtscore gegenüberstellen | Referenzlauf | „Speicher- und Write-Kosten der vollständigen Sets“ | In EV-04-P06 einführen; P07 verwendet sie für die bedingte Auswahl |

## Offene Entscheidungen

- Konkrete Medienform erst nach Sichtung der Daten wählen; maximal drei dichte Medien.
- Fehlende oder ungültige Messzellen dürfen nicht imputiert oder still ausgeschlossen werden.
