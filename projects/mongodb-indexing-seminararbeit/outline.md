# Argumentationslinie und Gliederung

Status: Durch G2 freigegeben.

## Forschungsfrage

**Hauptforschungsfrage**

Wie kann aus einem festgelegten MongoDB-Query-Workload und einer begrenzten Menge möglicher Indizes ein passendes Indexset ausgewählt und durch Messungen überprüft werden, wenn Leseleistung, Speicherbedarf und Schreibaufwand gemeinsam berücksichtigt werden?

**Teilfragen**

1. Wie lassen sich Workload, Indexkandidaten und Messgrößen so festlegen, dass verschiedene Indexsets nachvollziehbar und reproduzierbar verglichen werden können?
2. Wie können alle möglichen Indexsets unter Berücksichtigung von Querygewichten und festgelegten Grenzen systematisch bewertet und nicht dominierte Lösungen ermittelt werden?
3. Welche Indexsets sind im E-Commerce-Referenzworkload bei den untersuchten Datenmengen und Selektivitäten nicht dominiert, und wie stabil ist diese Auswahl?
4. Wie gut sagen Baseline- und Einzelindexmessungen die tatsächliche Leistung ausgewählter Indexsets voraus, und welche Empfehlung lässt sich daraus für den Referenzworkload ableiten?

## Argumentationslinie

1. MongoDB-Indizes verbessern einzelne Lesezugriffe, verursachen aber Speicher- und Write-Kosten. Mehrere lokal günstige Einzelindizes bilden deshalb nicht automatisch ein geeignetes Workload-Set.
2. Workloadbasierte Indexauswahl lässt sich als endliches Auswahlproblem formulieren: Querygewichte und geschätzte Kosten bestimmen den Read-Nutzen; Speicher, Write-Aufwand und Indexanzahl wirken als weitere Ziele oder Constraints.
3. MongoDB-spezifische Eigenschaften wie Compound-Reihenfolge, Präfixe, Partial- und Multikey-Bedingungen begrenzen, welche Kandidaten für eine Query fachlich zulässig sind. Der Kandidatenpool muss vor der Ergebnisbetrachtung feststehen.
4. Die Fallstudie operationalisiert diese Begriffe mit drei Query Shapes, fünf Parameterfällen, acht optionalen Kandidaten und I9 als verpflichtendem Unique-Index. Die Baseline besteht folglich aus `_id` und I9.
5. Isolierte Baseline- und Einzelindexmessungen liefern je Skalierung ein normalisiertes Read-Kostenmodell sowie additive Speicher- und Write-Proxys. Die 256 optionalen Sets können vollständig enumeriert und ohne Heuristik als Pareto-Front ausgewertet werden.
6. Das Screening ist bewusst unvollständig: Minimum-Einzelkosten bilden Indexinteraktionen, Cacheeffekte und natürliche Plannerentscheidungen nicht ab. Query Settings oder Hidden Indexes ersetzen keine physische Setmessung.
7. Eine vorab festgelegte Regel wählt mit einem primären 10-%-Read-Band höchstens drei Pareto-Finalisten; zusätzliche 5-%- und 20-%-Bänder prüfen die Sensitivität dieser Wahl. Nur die primären Sets werden mit ausschließlich ihren Indizes, ohne `hint()` und aus äquivalenten Ausgangszuständen gemessen.
8. Der Vergleich zwischen geschätzten und tatsächlichen Setkosten zeigt, wie tragfähig das vereinfachte Modell ist. Daraus folgen eine bedingte Empfehlung für die Fallstudie und Grenzen des wiederverwendbaren Evaluators.

## Wortbudget

| Block | Kapitel | Zielwörter |
| --- | --- | ---: |
| Einleitung | 1 – Problem, Forschungsfrage und Beitrag | 350 |
| Theorie/Grundlagen | 2 – Workloadbasierte Auswahl von MongoDB-Indexsets | 1.300 |
| Hauptteil | 3 – Evaluator und empirisches Untersuchungsdesign | 1.200 |
| Hauptteil | 4 – Pareto-Ergebnisse, Finalvalidierung und Diskussion | 1.300 |
| Fazit | 5 – Antwort und Reichweite | 350 |
| **Gesamt** |  | **4.500** |

Das Budget umfasst den Fließtext der fünf Manuskriptkapitel. Literaturverzeichnis, Verzeichnisse und Anhänge werden mangels abweichender Hochschulvorgabe nicht eingerechnet. Gegenüber der alten Planung wandern 250 Wörter aus dem Grundlagenblock in die Methodik: Die neue Arbeit benötigt weniger katalogartige Indexerklärung, aber deutlich mehr Raum für Kostenmodell, Enumeration, Pareto-Regel und Finalvalidierung.

## Kapitel

### intro – Problem, Forschungsfrage und Beitrag

- **Unterstruktur:** praktische Indexset-Entscheidung; Forschungslücke im begrenzten MongoDB-Kontext; Forschungsfrage und Teilfragen; zwei Arbeitsergebnisse; Aufbau.
- **Funktion:** Motiviert die Auswahl eines Sets statt den isolierten Vergleich einzelner Indizes.
- **Teilfragen:** Rahmt alle Teilfragen.
- **Erwartetes Ergebnis:** Präzises Untersuchungsversprechen mit klarer Begrenzung auf Workload, Kandidatenraum, Daten, Messumgebung und Kostenmodell.
- **Voraussetzungen:** freigegebener G1-Brief; Scoping zur workloadbasierten Auswahl und funktionalen Abgrenzung zum Atlas Performance Advisor.
- **Übergabe:** Benennt die Begriffe und Zielgrößen, die Kapitel 2 formal und MongoDB-spezifisch klärt.
- **Zielwörter:** 350.
- **Evidenzbedarf:** klassische Index-Advisor-Forschung; MongoDB Write-/Storage-Trade-off; begrenzte E-Commerce-Plausibilisierung.
- **Praktische Artefakte:** keine.
- **Medien:** keine.

### foundations – Workloadbasierte Auswahl von MongoDB-Indexsets

- **Unterstruktur:** 2.1 Query Workload, Query Shape und Gewichte; 2.2 MongoDB-Indexzugriff, Planner und Explain; 2.3 Compound-Präfixe, ESR, Partial-, Multikey- und Unique-Eigenschaften; 2.4 Index Selection Problem, Constraints und Pareto-Dominanz; 2.5 Grenzen von Kostenschätzung und MongoDB-Screeningmechanismen.
- **Funktion:** Liefert genau die Begriffe und Regeln, aus denen Kandidatenraum, Kostenmodell und Auswertungslogik folgen.
- **Teilfragen:** begründet Teilfragen 1 und 2 konzeptionell.
- **Erwartetes Ergebnis:** Ein Modell, das Workload, Kandidatenmenge, Pflichtindizes, Set, gewichtete Read-Kosten, Speicher, Write-Aufwand, Constraints und Dominanz sauber unterscheidet.
- **Voraussetzungen:** G1-Scope und verifizierte wissenschaftliche sowie technische Primärquellen.
- **Übergabe:** Kapitel 3 instanziiert das Modell als Evaluator und E-Commerce-Experiment.
- **Zielwörter:** 1.300. Der vorhandene Grundlagenentwurf wird deutlich fokussiert; nicht verwendete Indexarten und allgemeine MongoDB-Einführungen entfallen.
- **Evidenzbedarf:** Chaudhuri/Narasayya; CoPhy oder SWIRL; MongoDB-Dokumentation zu Query Shapes, Compound/ESR, Partial, Multikey, Unique, Explain, Query Settings, Hidden Indexes und Write Performance.
- **Praktische Artefakte:** Begriffs- und Formelkonsistenz mit der späteren Workloadkonfiguration.
- **Medien:** eine kleine Modellgrafik oder Tabelle nur dann, wenn sie die Beziehungen zwischen Workload, Kandidatenprofilen, Enumerator, Pareto-Front und Finalvalidierung kompakter als Prosa zeigt.

### method – Evaluator und empirisches Untersuchungsdesign

- **Unterstruktur:** 3.1 Eingabemodell des Evaluators; 3.2 Referenzworkload, Daten und Kandidaten; 3.3 isolierte Profilierung und Messkontrollen; 3.4 normalisiertes Read-Modell, Speicher-/Write-Proxys und Enumeration; 3.5 Pareto- und Finalistenregel; 3.6 physische Finalvalidierung und Reproduzierbarkeit.
- **Funktion:** Operationalisiert alle Forschungsfragen vor Kenntnis der neuen Ergebnisse.
- **Teilfragen:** beantwortet Teilfragen 1 und 2 methodisch und schafft die Messbasis für Teilfragen 3 und 4.
- **Erwartetes Ergebnis:** Vollständig spezifiziertes Verfahren mit `B = {_id, I9}`, 256 optionalen Sets, getrennten Skalenfronten, deterministischen Tie-Breakern und höchstens drei Finalisten.
- **Voraussetzungen:** Kapitel-2-Modell; implementierter und getesteter Evaluator; erfolgreiche Smoke-Tests für Workloadvarianten, Partial-Hints, Indexzustände und Ergebnisvalidierung.
- **Übergabe:** Kapitel 4 kann Screening- und Finalergebnisse berichten, ohne Auswahlregeln nachträglich zu verändern.
- **Zielwörter:** 1.200.
- **Evidenzbedarf:** Benchmarkmethodik; MongoDB Explain-/Hint-/Write-Dokumentation; eigene versionierte Konfigurationen und Artefakte.
- **Praktische Artefakte:** Workload- und Indexdefinitionen, Profiler, Kostenmatrix, Enumerator, Pareto-/Constraint-Auswertung, Finalistenauswahl, Runner, Tests und Manifeste.
- **Medien:** eine Versuchsmatrix und ein kompakter Ablauf von Profilierung über Enumeration zur Finalvalidierung.

### evaluation – Pareto-Ergebnisse, Finalvalidierung und Diskussion

- **Unterstruktur:** 4.1 Qualität und Plausibilität der Kandidatenprofile; 4.2 Pareto-Fronten und Skalenwechsel; 4.3 regelbasierte Finalistenauswahl und Sensitivität der 5-/10-/20-%-Bänder; 4.4 tatsächliche Read-, Speicher- und Write-Ergebnisse; 4.5 Schätzung gegen Messung; 4.6 Empfehlung, Übertragbarkeit und Limitationen.
- **Funktion:** Trennt Screening-Ergebnisse von belastbarer Setmessung und führt beide zur Antwort zusammen.
- **Teilfragen:** beantwortet Teilfrage 3 durch die Fronten und Teilfrage 4 durch Finalvalidierung und Modellabweichung.
- **Erwartetes Ergebnis:** Nicht dominierte Sets je Skalierung; Stabilität oder Wechsel der Kompromisssets über 5 %, 10 % und 20 % Read-Toleranz; bis zu drei nach dem primären 10-%-Band ausgewählte Finalisten; gemessene Plannerwahl und Kosten; Abweichung des Screening-Modells; bedingte Empfehlung für den Referenzworkload.
- **Voraussetzungen:** vollständige Rohdaten, erfolgreiche Ergebnisvalidierung, dokumentierte Messumgebung und unveränderte Auswahlregeln.
- **Übergabe:** Verdichtet Methode, Setempfehlung und Geltungsgrenzen für das Fazit.
- **Zielwörter:** 1.300.
- **Evidenzbedarf:** primär eigene Messdaten; technische Dokumentation zur Planinterpretation; wissenschaftliche Literatur zur Begrenztheit kostenbasierter Empfehlungen.
- **Praktische Artefakte:** Kostenmatrix, Pareto-Tabellen, Finalistenmanifeste, Explain-Daten, reale Laufzeiten, Indexgrößen, Insert-/Update-Messungen und Schätzungsfehler.
- **Medien:** eine Pareto-Darstellung pro sinnvoll zusammenfassbarer Skalengruppe, eine Finalistentabelle und eine Schätzung-vs.-Messung-Grafik. Keine separaten Diagramme ohne eigenständige Aussage.

### conclusion – Antwort und Reichweite

- **Unterstruktur:** direkte Antwort auf die Forschungsfrage; methodischer Beitrag des Evaluators; bedingte Fallstudienempfehlung; Grenzen und kurzer Ausblick.
- **Funktion:** Beantwortet die Forschungsfrage ohne neue Evidenz.
- **Teilfragen:** synthetisiert alle vier Teilantworten.
- **Erwartetes Ergebnis:** Aussage, wann das Verfahren ein geeignetes Set identifiziert, wie zuverlässig das Screening war und welche erneute Profilierung bei anderen Workloads erforderlich bleibt.
- **Voraussetzungen:** freigegebene Interpretation aus Kapitel 4.
- **Übergabe:** keine.
- **Zielwörter:** 350.
- **Evidenzbedarf:** ausschließlich bereits geprüfte Evidenz und eigene Ergebnisse.
- **Praktische Artefakte:** keine neuen.
- **Medien:** keine.

## Festgelegte G2-Methodenentscheidungen

- Hauptgegenstand ist die allgemeine Methode; der Produktkatalog bleibt Fallstudie.
- I1 bis I8 sind optional, I9 und `_id` bilden die feste Basiskonfiguration.
- Q1 und Q2 besitzen je zwei gleich gewichtete Parameterfälle; Q3 erhält ein Drittel des Shape-Gewichts.
- Read-Kosten werden je Skalierung anhand baseline-normalisierter Medianzeiten berechnet; absolute Zeiten und Explain-Metriken bleiben sichtbar.
- Kandidaten werden physisch als `B ∪ {i}` profiliert; `hint()` isoliert nur zulässige Einzelpfade.
- Speicher und Write-Aufwand werden im Screening additiv geschätzt und ausdrücklich als Proxys bezeichnet.
- Alle 256 optionalen Sets werden vollständig enumeriert.
- Pareto-Fronten werden getrennt je Skalierung gebildet; Skalen werden nicht zu einer einzigen Zielfunktion vermischt.
- Die Basiskonfiguration wird separat validiert. Read-Anker, Speicher- und Write-Kompromiss werden mit einer vorab festgelegten primären 10-%-Read-Toleranz und deterministischen Tie-Breakern gewählt; Duplikate führen zu weniger als drei Finalisten.
- Eine rechnerische Sensitivitätsanalyse wiederholt die beiden Kompromissauswahlen mit 5 % und 20 %. Abweichende Sets werden berichtet, aber weder opportunistisch nachnominiert noch zusätzlich physisch validiert.
- Finalisten werden ausschließlich physisch materialisiert und ohne `hint()` gemessen.
- Vorgesehen sind drei Warmups und zehn Read-Wiederholungen sowie fünf Wiederholungen der 1.000er Insert- und Aktivstatus-Update-Batches bei 500.000 Dokumenten.
- Der alte Lauf vom 24.07.2026 bleibt Pilot und wird nicht als Evidenz für die neue Forschungsfrage verwendet.

## Nicht vorweggenommene Ergebnisse

- welche Sets auf den drei Pareto-Fronten liegen;
- ob sich die Front mit Skalierung oder Selektivität ändert;
- welche Kandidaten MongoDB innerhalb materialisierter Sets tatsächlich verwendet;
- ob die drei Auswahlrollen unterschiedliche Sets ergeben;
- wie stark Einzelindexschätzung und reale Setleistung voneinander abweichen;
- welche bedingte Empfehlung schließlich gerechtfertigt ist.
