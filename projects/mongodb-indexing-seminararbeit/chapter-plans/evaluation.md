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
| EV-04-P01 | Ergebnislesart fixieren | Die Auswertung folgt der in Kapitel 3 festgelegten Reihenfolge und trennt strukturelle Explain-Metriken, reale Laufzeit, Plannerwahl und Konfigurationsentscheidung. | Verhindert selektive Siegerlogik und bereitet die Lesart der Tabellen vor. | Eigene Methode und Ergebnisdaten. | Übergabe aus Methode. | Voraussetzung → EV-04-P02. | 100 | keines | Nach dem Lauf prüfen, ob alle geplanten Messartefakte vollständig vorliegen. |
| EV-04-P02 | Q1-Befunde berichten | Für Kategorie, Aktivstatus und Preisreihenfolge werden Baseline und I1–I5 über Skalen sowie common/rare Varianten verglichen; Struktur, Laufzeit und Speicher werden gemeinsam interpretiert. | Q1 ist der komplexeste zentrale Listenfall und testet Reihenfolge sowie Partial-Eignung. | Eigene validierte Read-/Explain-/Indexgrößendaten. | Anwendung. | Vergleich → EV-04-P03. | 250 | Grafik M-EV-01 | Ergebnisse offen: keine Gewinner, Größenordnung oder Plannerentscheidung vorwegnehmen. |
| EV-04-P03 | Q2-Befunde berichten | Für tagbasierten Arrayzugriff werden I6–I8 unter common/rare Tags bewertet; Ergebnisprüfung berücksichtigt die fehlende explizite Sortierung. | Q2 prüft Multikey- und Partial-Nutzbarkeit unter anderer Shape-Struktur. | Eigene validierte Daten; Multikey/Partial-Theorie zur Interpretation. | Parallele Anwendung. | Übergabe → EV-04-P04. | 210 | Grafik M-EV-02 | Nach dem Lauf prüfen, ob unsortierte Limit-Ergebnisse korrekt über Prädikat und Kardinalität abgesichert wurden. |
| EV-04-P04 | Q3-Befunde berichten | Der `productId`-Unique-Index wird gegen die Baseline für den Detailabruf eingeordnet; die konzeptionelle `_id`-Alternative bleibt klar von der gemessenen Konfiguration getrennt. | Vermeidet eine überzogene Generalisierung aus einem Punktlookup. | Eigene Daten; Referenzschema; dokumentierte Unique-Regeln. | Themenwechsel. | Synthese → EV-04-P05. | 110 | Tabelle M-EV-03 | Prüfen, ob aktive/inaktive Produktsemantik vollständig abgedeckt ist. |
| EV-04-P05 | Plannerwahl einordnen | Der Lauf ohne Hint zeigt je Query, welchen verfügbaren Pfad MongoDB wählt; Übereinstimmung oder Abweichung zu kontrollierten Messungen wird berichtet, nicht als Widerspruch vereinfacht. | Plannerwahl und gemessene Kandidatenleistung sind verschiedene Evidenzen. | Eigene Planner- und Hint-Daten; Explain-Dokumentation. | Synthese. | Folge → EV-04-P06. | 150 | M-EV-03 | Plan-Cache- und Cachezustand aus Manifest kontrollieren. |
| EV-04-P06 | Kombiset ableiten | Die Reduktion dokumentiert pro Kandidat Beibehalten, Entfernen oder bedingte Eignung mit Bezug auf Abdeckung, Redundanz, Struktur, Laufzeit, Speicher und Eligibility. | Erst diese Abwägung beantwortet die Frage nach einer gemeinsamen Konfiguration. | Eigene vollständige Kandidatenmatrix; Kriterien aus Kapitel 2. | Ursache. | Folge → EV-04-P07. | 240 | Tabelle M-EV-04 | Endgültige Mitglieder des Kombisets erst nach Messung eintragen; keine vorab festgelegte Finalmenge. |
| EV-04-P07 | Finalvalidierung berichten | Das reduzierte Set wird ohne Hint auf allen drei Queries gegen die `_id`-Baseline validiert; dabei wird seine gesamte Indexgröße ausgewiesen. | Prüft die reale gemeinsame Konfiguration statt nur Einzelkandidaten im Pool. | Eigene finale Read-, Explain- und Speicherartefakte. | Prüfung der Folge. | Einschränkung → EV-04-P08. | 150 | M-EV-04 | Prüfen, dass Pool- und Finalzustand sauber getrennte Run-IDs besitzen. |
| EV-04-P08 | Schreibtrade-off berichten | Der begrenzte Vergleich von 1.000 Inserts und 1.000 Aktivstatus-Updates bei 500k stellt Baseline und finales Set mit fünf Wiederholungen gegenüber. | Macht Indexwartung sichtbar, ohne Schreiblast umfassend zu modellieren. | Eigene Write-Manifeste; MongoDB-Quelle zur Einordnung. | Einschränkung. | Synthese → EV-04-P09. | 100 | M-EV-04 | Identische Ausgangszustände, Write Concern und Fehlerfreiheit verifizieren. |
| EV-04-P09 | Diskussion und Grenzen | Die Empfehlung gilt für die dokumentierte Struktur und den Referenzworkload; Selektivität, Cache/Host, fehlende Konkurrenzlast und begrenzter Write-Test definieren ihre Übertragungsgrenzen. | Die Arbeit beantwortet die Forschungsfrage als Entscheidungsleitfaden, nicht als universelles Schema. | Eigene Methoden-/Ergebnisgrenzen; ggf. Benchmarkliteratur. | Synthese. | Übergabe → Kapitel 5. | 90 | keines | Nach Ergebnissen prüfen, welche Grenzen tatsächlich ergebnisrelevant sind. |

**Summe Zielwörter: 100 + 250 + 210 + 110 + 150 + 240 + 150 + 100 + 90 = 1.400.**

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
