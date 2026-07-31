# Kapitelplan: method – Evaluator und empirisches Untersuchungsdesign

## Funktion im Gesamtargument

Das Kapitel operationalisiert die Forschungsfragen vollständig vor Kenntnis der neuen Messergebnisse. Es dokumentiert Eingaben, Fallstudie, Kandidatenprofilierung, Kostenmodell, Enumeration, Finalistenregel und physische Validierung so, dass Kapitel 4 die Regeln nicht nachträglich verändern kann.

## Teilfrage und erwartetes Ergebnis

- **Teilfragen:** Beantwortet Teilfrage 1 und 2 methodisch; schafft die Messbasis für Teilfrage 3 und 4.
- **Erwartetes Ergebnis:** Ein reproduzierbares Verfahren mit `B = {_id, I9}`, acht optionalen Kandidaten, 256 vollständig enumerierten Sets, getrennten Pareto-Fronten je Skalierung und höchstens drei durch das primäre 10-%-Band bestimmten Finalisten.

## Wortbudget

**1.200 Wörter**

## Voraussetzungen und Übergabe

- **Voraussetzungen:** Begriffsmodell aus Kapitel 2; versionierte Implementierung und erfolgreiche Smoke-Tests vor der späteren G4-Freigabe.
- **Übergabe:** Kapitel 4 kann Screening, Finalistenauswahl und tatsächliche Setleistung berichten, ohne Kandidaten oder Regeln ergebnisabhängig anzupassen.

## Geplante Unterstruktur

| Abschnitt | Inhalt | Absatz-IDs |
| --- | --- | --- |
| 3.1 | Eingabemodell des Evaluators | ME-03-P01 bis ME-03-P02 |
| 3.2 | Referenzworkload, Daten und Kandidaten | ME-03-P03 bis ME-03-P04 |
| 3.3 | Isolierte Profilierung und Messkontrollen | ME-03-P05 |
| 3.4 | Normalisiertes Read-Modell, Speicher-/Write-Proxys und Enumeration | ME-03-P06 bis ME-03-P07 |
| 3.5 | Pareto- und Finalistenregel | ME-03-P08 |
| 3.6 | Physische Finalvalidierung und Reproduzierbarkeit | ME-03-P09 |

## Absatzplan

| Absatz-ID | Funktion | Kernaussage | Begründung | Evidenzbedarf | Beziehung davor | Beziehung danach | Zielwörter | Medium | Offene Recherche |
| --- | --- | --- | --- | --- | --- | --- | ---: | --- | --- |
| ME-03-P01 | Untersuchungsablauf festlegen | Der Evaluator trennt Konfiguration, Baseline-/Einzelprofilierung, Set-Screening, regelbasierte Finalistenauswahl und physische Validierung. | Gibt eine reproduzierbare Reihenfolge vor und verhindert die Vermischung von Schätzung und Messung. | Eigene Methodenentscheidung und versionierte Artefakte; Benchmarkliteratur zur Einordnung. | Übergabe aus Kapitel 2. | Zerlegung → ME-03-P02. | 100 | M-ME-01 | Geeignete methodische Primärquelle für Datenbank-Mikrobenchmarks bestimmen. |
| ME-03-P02 | Eingabemodell des Evaluators spezifizieren | Eingaben sind registrierte ausführbare Query Shapes mit Varianten und Gewichten, Pflicht- und optionale Indizes, Skalen, Messparameter sowie optionale Constraints; beliebige MongoDB-Abfragen werden nicht geparst. | Definiert die Wiederverwendbarkeit des Softwareartefakts präzise. | Eigene Konfigurationsschemata, Validierungsregeln und Tests. | Konkretisierung. | Instanziierung → ME-03-P03. | 130 | M-ME-01 | Konfigurationsfelder und Fehlermeldungen vor G4 mit Implementierung abgleichen. |
| ME-03-P03 | Referenzworkload und Daten festlegen | Die Fallstudie nutzt Q1 Kategorie/Preis, Q2 Tags und Q3 `productId` mit fünf Varianten, gleichen Shape-Gewichten sowie 10k, 100k und 500k deterministisch erzeugten Dokumenten. | Macht Selektivität, Skalierung und Gewichtung kontrollierbar, ohne Repräsentativität zu behaupten. | Eigene Workload- und Seed-Konfigurationen; Manifeste mit tatsächlich erreichten Verteilungen. | Anwendung. | Voraussetzung → ME-03-P04. | 140 | M-ME-02 | Exakte Werte/Seeds und tatsächlich erreichte Selektivitäten vor dem finalen Lauf fixieren beziehungsweise protokollieren. |
| ME-03-P04 | Kandidatenraum und Baseline fixieren | `B = {_id, I9}` ist in jeder Konfiguration vorhanden; I1 bis I8 sind optional und werden nur für logisch zulässige Queryvarianten profiliert. | Trennt Integrität von Optimierung und begründet exakt 256 optionale Sets. | Eigene Kandidatenregistry; MongoDB-Regeln zu Partial-, Multikey-, Compound- und Unique-Indizes. | Fortführung. | Folge → ME-03-P05. | 150 | M-ME-02 | Indexnamen, Key Patterns, Optionen und Eligibility-Matrix durch Smoke-Tests verifizieren. |
| ME-03-P05 | Kandidatenprofilierung kontrollieren | Je Skalierung werden `B` und `B ∪ {i}` aus äquivalentem Zustand gemessen; drei Warmups, zehn Reads, deterministische Rotation, vollständiger Cursorverbrauch und getrennte Explain-Erhebung gelten einheitlich. | Liefert vergleichbare Einzelkosten und Strukturkontrollen für das Screening. | Eigene Runner-Manifeste und Rohdaten; MongoDB-Dokumentation zu `hint()` und `explain`; Benchmarkliteratur. | Operationalisierung. | Folge → ME-03-P06. | 150 | M-ME-01 | Cacheprotokoll, Zeitmessgrenzen, Rotationsschema und Ergebnisäquivalenz technisch absichern. |
| ME-03-P06 | Kostenmodell und Proxys definieren | Medianzeiten werden je Query gegen `B` normalisiert und gewichtet; Set-Readkosten verwenden das Minimum verfügbarer Einzelkosten, optionaler Speicher und nichtnegative Insert-/Update-Overheads werden additiv geschätzt. | Ermöglicht transparente Bewertung aller Sets und kennzeichnet die Näherungen ausdrücklich. | Eigene Formeln und Messdefinitionen; Literatur zur Kostenmodellierung; eigene Einzelprofile. | Folge. | Berechnung → ME-03-P07. | 140 | keines | Einheit, Rundung, fehlende/ungültige Profile und Null-/Rauschbehandlung in Implementierung und Bericht festlegen. |
| ME-03-P07 | Enumeration und Pareto-Regel festlegen | Alle 256 Sets werden je Skalierung separat als Vektor aus Read-Kosten, Write-Proxy und Speicher bewertet; Dominanz und optionale Constraints folgen deterministischen Regeln. | Vermeidet Heuristiken und eine willkürliche skalare Gesamtstrafe. | Eigener Enumerator, Dominanz-/Constraint-Tests und Ergebnisartefakte; methodische Literatur. | Folge. | Auswahl → ME-03-P08. | 140 | M-ME-01 | Tie-Behandlung, numerische Toleranzen und lexikografische Reihenfolge in Tests fixieren. |
| ME-03-P08 | Finalistenregel präregistrieren | Aus der Vereinigung der Skalenfronten werden Read-Anker sowie Speicher- und Write-Kompromiss im primären 10-%-Read-Band gewählt; 5 % und 20 % dienen nur der Sensitivitätsanalyse, Duplikate werden nicht ersetzt. | Begrenzt physische Läufe und verhindert opportunistische Nachnominierung. | Eigene freigegebene G2-Regel, Auswahltests und Finalistenmanifest. | Einschränkung und Auswahl. | Prüfung → ME-03-P09. | 130 | M-ME-01 | Exakte Auslegung des Read-Bands bei skalenspezifischen Kosten und numerischen Gleichständen in Code/Tests bestätigen. |
| ME-03-P09 | Finalvalidierung und Reproduzierbarkeit definieren | `B` und primäre Finalisten werden mit ausschließlich ihren Indizes, ohne `hint()`, über alle Varianten und Skalen gemessen; bei 500k kommen fünf identische Insert-/Update-Batches sowie der Vergleich von Schätzung und Messung hinzu. | Prüft Plannerwahl, Interaktionen, reale Gesamtgröße und Write-Kosten unter äquivalenten Zuständen. | Eigene Run-Manifeste, Explain-/Zeit-/Größen-/Write-Rohdaten und Umgebungsmetadaten. | Prüfung und Synthese. | Übergabe → Kapitel 4. | 120 | M-ME-01 | Resetstrategie, Write Concern, Batchoptionen, Fehlerregeln und Umgebungsprovenienz vor Durchführung festschreiben. |

**Summe Zielwörter: 100 + 130 + 140 + 150 + 150 + 140 + 140 + 130 + 120 = 1.200.**

## Medienplan

| Medium-ID | Typ | Aussagefunktion | Herkunft/Erzeugung | Beschriftung | Einführung und Interpretation |
| --- | --- | --- | --- | --- | --- |
| M-ME-01 | Ablauf-/Versuchsmatrix | Verdichtet Phasen, Zustände, Messungen, Wiederholungen, Ausgaben und Auswahlregeln des Evaluators. | Eigene Methodenspezifikation und versionierte Konfiguration. | „Ablauf von der Kandidatenprofilierung zur Finalvalidierung“ | ME-03-P01 führt die Stufen ein; ME-03-P05 bis P09 erläutern Mess- und Auswahlzeilen. |
| M-ME-02 | Tabelle | Ordnet Q1 bis Q3, fünf Varianten, Gewichte sowie I1 bis I9 mit Rolle, Definition und Eligibility zu. | Eigene Workload- und Indexregistry. | „Referenzworkload und vorab festgelegter Kandidatenraum“ | ME-03-P03 führt Queries und Gewichte ein; ME-03-P04 interpretiert Baseline, optionale Kandidaten und Suchraum. |

## Offene Entscheidungen

- Alle offenen Implementierungsdetails sind vor Messbeginn zu schließen und anschließend als versionierte Konfiguration oder Manifest zu belegen.
- Der alte Lauf vom 24.07.2026 bleibt Pilot und wird weder in Kostenmatrix noch Ergebnisdarstellung übernommen.
- Das Kapitel enthält keine Pareto-Mitglieder, Finalisten, Messwerte oder Empfehlung.
