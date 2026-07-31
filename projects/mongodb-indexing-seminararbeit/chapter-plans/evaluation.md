# Kapitelplan: evaluation Kandidatenvergleich, Kombiset und Diskussion

## Funktion im Gesamtargument

Das Kapitel beantwortet die empirischen Teilfragen: Es berichtet zunächst kontrollierte Einzelbefunde, trennt diese von der natürlichen Plannerwahl, reduziert dann transparent zum Kombiset und bewertet dieses workloadweit einschließlich begrenzter Schreibkosten und Grenzen.

## Teilfrage und erwartetes Ergebnis

- **Teilfrage:** Beantwortet Teilfrage 3 empirisch und Teilfrage 4 durch Reduktion und Schlussvalidierung; synthetisiert Teilfragen 1 und 2.
- **Erwartetes Ergebnis:** Eine evidenzbasierte, bedingte Ausgangskonfiguration mit nachvollziehbaren verworfenen/bedingt geeigneten Kandidaten. Konkrete Ergebnisse bleiben bis zum neuen Benchmark offen.

## Wortbudget

**1.400 Wörter**

## Voraussetzungen und Übergabe

- **Voraussetzungen:** Vollständiger neuer Benchmarklauf, validierte Ergebnisse, rohe Explain-Ausgaben, Laufzeit- und Speicherwerte, Write-Manifeste; der alte Lauf bleibt Pilot.
- **Übergabe:** Liefert die verdichtete Konfiguration, Entscheidungsregeln und Übertragungsgrenzen für Kapitel 5.

## Absatzplan

| Absatz-ID | Funktion | Kernaussage | Begründung | Evidenzbedarf | Beziehung davor | Beziehung danach | Zielwörter | Medium | Offene Recherche |
| --- | --- | --- | --- | --- | --- | --- | ---: | --- | --- |
| EV-04-P01 | Ergebnislesart fixieren | Die Auswertung folgt Kapitel 3 und trennt strukturelle Explain-Metriken, reale Laufzeit, Plannerwahl und Konfigurationsentscheidung. | Verhindert selektive Siegerlogik und bereitet die Lesart der Medien vor. | Eigene Methode und Ergebnisdaten. | Übergabe aus Methode. | Voraussetzung → EV-04-P02. | 80 | keines | Nach dem Lauf Vollständigkeit aller geplanten Messartefakte prüfen. |
| EV-04-P02 | Q1-Kandidatenbefund | Baseline und I1–I5 werden für beide Kategorieselektivitäten und drei Skalen anhand von Struktur, Laufzeit und Speicher verglichen, ohne aus einem Einzelwert zu entscheiden. | Q1 prüft Equality-/Sortierreihenfolge, Fetch-Aufwand, Partial-Eignung und Präfixredundanz. | Eigene validierte Read-, Explain- und Indexgrößendaten; Kapitel-2-Kriterien. | Anwendung. | Vergleich → EV-04-P03. | 220 | M-EV-01 | Gewinner, Größenordnungen und Plannerentscheidungen bleiben bis zum Lauf offen. |
| EV-04-P03 | Q2-Kandidatenbefund | I6–I8 werden unter beiden Tagselektivitäten und drei Skalen bewertet; nur zuvor validierte Ergebnisse mit korrekter Kardinalität und Prädikaterfüllung gehen ein. | Q2 prüft Multikey- und Partial-Nutzbarkeit unter einer anderen Query Shape. | Eigene validierte Read-, Explain- und Indexgrößendaten; Multikey-/Partial-Theorie. | Parallele Anwendung. | Übergabe → EV-04-P04. | 180 | M-EV-02 | Automatisierte Q2-Ergebnisvalidierung im Run-Manifest bestätigen. |
| EV-04-P04 | Q3-Kandidatenbefund | Der `productId`-Unique-Index wird gegen die Baseline für den Detailabruf eingeordnet; die konzeptionelle `_id`-Alternative bleibt von der Messung getrennt. | Vermeidet eine überzogene Generalisierung aus einem Punktlookup. | Eigene Daten; Referenzschema; dokumentierte Unique-Regeln. | Themenwechsel. | Synthese → EV-04-P05. | 100 | M-EV-03 | Aktive/inaktive Produktsemantik und Existenz des Zielprodukts im Manifest prüfen. |
| EV-04-P05 | Plannerwahl einordnen | Der Lauf ohne Hint zeigt je Query den gewählten Pfad; Übereinstimmung oder Abweichung zu den kontrollierten Messungen wird als eigene Beobachtung berichtet. | Plannerwahl und gemessene Kandidatenleistung sind verschiedene Evidenzen. | Eigene Planner- und Hint-Daten; Explain-Dokumentation. | Synthese. | Folge → EV-04-P06. | 130 | M-EV-03 | Plan-Cache- und Cachezustand aus Manifest kontrollieren. |
| EV-04-P06 | Kombiset ableiten | Die Reduktion dokumentiert pro Kandidat Beibehalten, Entfernen oder bedingte Eignung anhand von Abdeckung, Redundanz, Struktur, Laufzeit, Speicher und Eligibility; Nichtauswahl durch den Planner ist allein kein Ausschlussgrund. | Erst diese mehrdimensionale Abwägung beantwortet die Frage nach einer gemeinsamen Konfiguration. | Eigene vollständige Kandidatenmatrix; Kriterien aus Kapitel 2. | Ursache. | Folge → EV-04-P07. | 220 | M-EV-04 | Endgültige Mitglieder erst nach Messung eintragen; keine vorab festgelegte Finalmenge. |
| EV-04-P07 | Finalvalidierung berichten | Das reduzierte Set wird ohne Hint auf allen drei Queries gegen die `_id`-Baseline validiert; verwendete Pfade und gesamte Indexgröße werden ausgewiesen. | Prüft die reale gemeinsame Konfiguration statt nur Einzelkandidaten im Pool. | Eigene finale Read-, Explain- und Speicherartefakte. | Prüfung der Folge. | Einschränkung → EV-04-P08. | 140 | M-EV-04 | Sauber getrennte Run-IDs für Pool- und Finalzustand prüfen. |
| EV-04-P08 | Schreibtrade-off berichten | Der begrenzte Vergleich von 1.000 Inserts und 1.000 Aktivstatus-Updates bei 500k stellt Baseline und finales Set mit fünf Wiederholungen gegenüber. | Macht Indexwartung sichtbar, ohne Schreiblast umfassend zu modellieren. | Eigene Write-Manifeste; MongoDB-Quelle zur Einordnung. | Einschränkung. | Synthese → EV-04-P09. | 100 | M-EV-04 | Identische Ausgangszustände, Write Concern, Bulk-Optionen und Fehlerfreiheit verifizieren. |
| EV-04-P09 | Befunde synthetisieren | Quer über Q1–Q3 wird herausgearbeitet, wie Query Shape, Selektivität und Datenmenge die gemessenen Unterschiede und die bedingte Indexwahl erklären. | Verbindet Einzelbefunde mit Teilfrage 3 und bereitet die direkte Forschungsantwort vor. | Eigene validierte Befunde; Erklärungsbegriffe aus Kapitel 2. | Synthese der Einzelergebnisse. | Einschränkung → EV-04-P10. | 110 | keines | Erst nach dem Lauf festlegen, welche Abhängigkeiten tatsächlich durch Daten gestützt sind. |
| EV-04-P10 | Reichweite begrenzen | Synthetische Verteilungen, drei ausgewählte Queries, einzelner Host/Cachezustand, fehlende Konkurrenzlast, maximale Skala und begrenzter Write-Test bestimmen interne und externe Übertragungsgrenzen. | Die resultierende Konfiguration ist eine validierte Ausgangskonfiguration, kein universelles Produktionsschema. | Eigene Methoden- und Ergebnisgrenzen; ggf. Benchmarkliteratur. | Einschränkung. | Übergabe → Kapitel 5. | 120 | keines | Nach Ergebnissen prüfen, welche Grenzen die Schlussfolgerung konkret abschwächen. |

**Summe Zielwörter: 80 + 220 + 180 + 100 + 130 + 220 + 140 + 100 + 110 + 120 = 1.400.**

## Medienplan

| Medium-ID | Typ | Aussagefunktion | Herkunft/Erzeugung | Beschriftung | Einführung und Interpretation |
| --- | --- | --- | --- | --- | --- |
| M-EV-01 | Grafik oder Tabelle | Zeigt Q1-Kandidaten über Skalen und Selektivitäten mit Strukturmetriken und medianer Laufzeit; nur eine Darstellungsform nach Datenprüfung wählen. | Eigene Benchmarkdaten. | „Q1: Kandidatenvergleich nach Datenmenge und Selektivität“ | EV-04-P02 führt die Kodierung ein und interpretiert Unterschiede nicht isoliert. |
| M-EV-02 | Grafik oder Tabelle | Zeigt Q2-Multikey-/Partial-Kandidaten über common/rare Tags und Skalen. | Eigene Benchmarkdaten. | „Q2: Kandidatenvergleich für tagbasierte Filterung“ | EV-04-P03 führt ein und interpretiert Eligibility getrennt von Leistung. |
| M-EV-03 | Tabelle | Verdichtet Q3 sowie Plannerwahl je Query; verhindert übergroße Visualisierung für Punktlookup. | Eigene Benchmark- und Explain-Daten. | „Produktdetailabruf und natürliche Planner-Auswahl“ | EV-04-P04 stellt Q3 dar; EV-04-P05 ergänzt die Planner-Spalten. |
| M-EV-04 | Tabelle | Zeigt finale Konfiguration, begründete Kandidatenentscheidung, Gesamtindexgröße, Read-Validierung und begrenzte Write-Trade-offs zusammen. | Eigene Reduktions- und Benchmarkdaten. | „Finales Kombiset und Validierung gegenüber der Baseline“ | EV-04-P06 führt die Auswahl ein; EV-04-P07/P08 interpretieren die Validierungs- und Kostenfelder. |

## Offene Entscheidungen

- Keine Messwerte, Gewinner, finale `createIndex`-Definition oder Reduktionsentscheidung vor dem neuen Lauf.
- Medienform von M-EV-01/M-EV-02 erst nach Datenform und Lesbarkeit entscheiden; pro Aussage nur die kompaktere Form verwenden.
- Ausreißer-, Fehler- und Vollständigkeitsbehandlung der Messdaten muss vor Auswertung dokumentiert werden.
