# Kapitelplan: foundations – Workloadbasierte Auswahl von MongoDB-Indexsets

## Funktion im Gesamtargument

Das Kapitel entwickelt genau die Begriffe und technischen Regeln, aus denen Kandidatenraum, Kostenmodell, Pareto-Auswertung und Grenzen des Screenings folgen. Allgemeine MongoDB-Einführungen und nicht verwendete Indexarten bleiben ausgeschlossen.

## Teilfrage und erwartetes Ergebnis

- **Teilfragen:** Begründet Teilfrage 1 und 2 konzeptionell; liefert Interpretationsbegriffe für Teilfrage 3 und 4.
- **Erwartetes Ergebnis:** Ein konsistentes Modell aus Workload, Queryvarianten und Gewichten, Pflicht- und optionalen Indizes, zulässigen Sets, Zielgrößen, Constraints und Pareto-Dominanz.

## Wortbudget

**1.300 Wörter**

## Voraussetzungen und Übergabe

- **Voraussetzungen:** G2-Scope, Referenzworkload und vorab festgelegter Kandidatenraum.
- **Übergabe:** Kapitel 3 instanziiert das Modell als Evaluator und reproduzierbares Experiment.

## Geplante Unterstruktur

| Abschnitt | Inhalt | Absatz-IDs |
| --- | --- | --- |
| 2.1 | Query Workload, Query Shape und Gewichte | FO-02-P01 |
| 2.2 | MongoDB-Indexzugriff, Planner und Explain | FO-02-P02 |
| 2.3 | Compound-Präfixe, ESR, Partial-, Multikey- und Unique-Eigenschaften | FO-02-P03 bis FO-02-P04 |
| 2.4 | Index Selection Problem, Constraints und Pareto-Dominanz | FO-02-P05 bis FO-02-P06 |
| 2.5 | Grenzen von Kostenschätzung und MongoDB-Screeningmechanismen | FO-02-P07 bis FO-02-P08 |

## Absatzplan

| Absatz-ID | Funktion | Kernaussage | Begründung | Evidenzbedarf | Beziehung davor | Beziehung danach | Zielwörter | Medium | Offene Recherche |
| --- | --- | --- | --- | --- | --- | --- | ---: | --- | --- |
| FO-02-P01 | Workloadbegriff definieren | Ein Workload besteht aus registrierten Query Shapes, konkreten Parametervarianten und Gewichten; Shape und experimenteller Parameterfall werden getrennt behandelt. | Schafft die Einheit, auf die Nutzen und Kosten später bezogen werden. | Literatur: workloadbasierte Indexauswahl; MongoDB-Primärdokumentation zum Query-Shape-Begriff. | Anschluss an Einleitung. | Konkretisierung → FO-02-P02. | 150 | M-FO-01 optional | Versionsgenaue Bedeutung von Query Shape und Abgrenzung zu Parametern verifizieren. |
| FO-02-P02 | MongoDB-Zugriff und Messbeobachtung erklären | Indizes eröffnen dem Planner alternative Zugriffspfade; Explain-Strukturmetriken und separat gemessene Laufzeiten beleuchten unterschiedliche Aspekte eines Plans. | Begründet, warum Laufzeit, untersuchte Schlüssel/Dokumente, Fetch-/Sort-Stufen und Plannerwahl nicht gleichgesetzt werden dürfen. | MongoDB-Primärdokumentation zu Query Planner, `explain`, Planstufen und Messgrenzen. | Konkretisierung des Kostenbegriffs. | Anwendung → FO-02-P03. | 175 | keines | Exakte Felder und versionsabhängige Planstufen gegen die eingesetzte MongoDB-Version prüfen. |
| FO-02-P03 | Compound-Regeln ableiten | Feldreihenfolge, ESR-Regel und nutzbare Compound-Präfixe bestimmen, welche Q1-Kandidaten Filter und Sortierung unterstützen und wo potenzielle Redundanz entsteht. | Leitet I1 bis I5 fachlich her, ohne einen Sieger zu behaupten. | MongoDB-Primärdokumentation zu Compound Indexes, Präfixen und ESR. | Anwendung. | Kontrast → FO-02-P04. | 165 | keines | ESR-Reichweite bei Equality plus absteigender Sortierung präzise und ohne Überverallgemeinerung belegen. |
| FO-02-P04 | Partial-, Multikey- und Unique-Bedingungen erklären | Partial-Indizes sind nur bei logisch implizierter Filterbedingung zulässig, `tags` erzeugt Multikey-Semantik, und I9 erfüllt als Unique-Index eine Integritäts- statt Wahlfunktion. | Begründet Eligibility für I5/I8, die Kandidaten I6 bis I8 sowie die feste Basiskonfiguration mit I9. | MongoDB-Primärdokumentation zu Partial, Multikey und Unique Indexes; eigene Schemaentscheidung zu `productId`. | Kontrast und Erweiterung. | Synthese → FO-02-P05. | 170 | keines | Koexistenz der vorgesehenen Full-/Partial-Key-Patterns und relevante Multikey-Grenzen technisch verifizieren. |
| FO-02-P05 | Auswahlproblem formalisieren | Aus einer festen Basiskonfiguration und optionalen Kandidaten entstehen zulässige Indexsets; Gewichte, Kosten und optionale Grenzen machen daraus ein endliches Index Selection Problem. | Verbindet den Workload mit der Mengenentscheidung und trennt Integritätsindizes von Optimierungsoptionen. | Wissenschaftliche Primärquellen: Chaudhuri/Narasayya, CoPhy und SWIRL; eigene formale Notation für `B` und `S`. | Abstraktion. | Erweiterung → FO-02-P06. | 170 | M-FO-01 optional | Exakte Fundstellen zu Workloadgewichten, Constraints und Konfigurationskosten erfassen. |
| FO-02-P06 | Mehrzielbewertung erklären | Ein Set wird durch Read-Kosten, optionalen Speicher und Write-Aufwand beschrieben; Pareto-Dominanz entfernt nur Sets, die in keiner Dimension besser sind. | Vermeidet eine unbegründete Gesamtnote und bereitet getrennte Fronten je Skalierung vor. | Wissenschaftliche Literatur zu mehrzieliger Indexauswahl und Pareto-Begriff; eigene Auswahl der drei Zielgrößen. | Synthese. | Folge → FO-02-P07. | 160 | M-FO-01 optional | Prüfen, welche Quelle die Pareto-Anwendung im Indexdesign am direktesten stützt. |
| FO-02-P07 | Schätzmodell begrenzen | Baseline-normalisierte Minimum-Einzelkosten sowie additive Speicher- und Write-Proxys ermöglichen vollständiges Screening, modellieren aber keine Indexinteraktionen oder natürliche Plannerentscheidungen. | Erklärt Nutzen und bewusste Unvollständigkeit des späteren Kostenmodells. | Index-Advisor-Literatur zur Kostenschätzung/What-if-Trennung; MongoDB-Dokumentation zu `hint()`, Query Settings und Hidden Indexes. | Einschränkung. | Folge → FO-02-P08. | 160 | keines | Fundstellen zur Reichweite von `allowedIndexes`, Hidden Indexes und `hint()` versionsgenau prüfen. |
| FO-02-P08 | Konsequenz für Validierung synthetisieren | Weil Screening nur schätzt, müssen wenige regelbasiert gewählte Sets physisch materialisiert und ohne erzwungene Indexwahl gemessen werden; Empfehlungen bleiben kontextgebunden. | Schließt die Theorie mit der methodischen Notwendigkeit der Finalvalidierung. | Wissenschaftliche Literatur zur Trennung von Kostenschätzung und realer Ausführung; technische Dokumentation zur Plannerwahl. | Folge und Synthese. | Übergabe → Kapitel 3. | 150 | keines | Reichweite der herangezogenen relationalen Literatur klar von MongoDB-spezifischen Schlussfolgerungen trennen. |

**Summe Zielwörter: 150 + 175 + 165 + 170 + 170 + 160 + 160 + 150 = 1.300.**

## Medienplan

| Medium-ID | Typ | Aussagefunktion | Herkunft/Erzeugung | Beschriftung | Einführung und Interpretation |
| --- | --- | --- | --- | --- | --- |
| M-FO-01 | Modellgrafik oder kompakte Tabelle, optional | Zeigt die Beziehungen zwischen Workload, Basiskonfiguration, optionalen Kandidaten, Set-Enumerator, Zielvektor, Pareto-Front und Finalvalidierung. | Eigene Synthese auf Basis der definierten Begriffe. | „Vom Query-Workload zum validierten Indexset“ | FO-02-P01 führt die Eingaben ein; FO-02-P06 bis P08 interpretieren Auswertung und Validierungsgrenze. Nur aufnehmen, wenn die Darstellung kompakter als Prosa ist. |

## Offene Entscheidungen

- Bei der Tiefenrecherche ist je fachlicher Aussage eine überprüfbare Fundstelle zu erfassen; Scoping-Links allein genügen nicht.
- Relationale Index-Advisor-Verfahren werden als Problem- und Methodikanschluss genutzt, nicht als Beleg für MongoDB-interne What-if-Fähigkeiten.
- Nicht verwendete Indexarten, allgemeine NoSQL-Geschichte und ausführliche B-Tree-Details bleiben außerhalb des Kapitels.
