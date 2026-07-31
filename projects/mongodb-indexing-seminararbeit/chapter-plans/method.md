# Kapitelplan: method Referenzworkload, Indexdesign und Benchmarkmethode

## Funktion im Gesamtargument

Das Kapitel übersetzt die Forschungsfrage in ein reproduzierbares, vor Ergebniskenntnis festgelegtes Verfahren. Es dokumentiert Referenzdaten, Query Shapes, neun Kandidaten, fünf Messstufen, Metriken und Validierungsregeln, ohne Ergebnisse oder finale Konfiguration zu nennen.

## Teilfrage und erwartetes Ergebnis

- **Teilfrage:** Operationalisiert alle vier Teilfragen.
- **Erwartetes Ergebnis:** Eine nachvollziehbare Versuchsmatrix, aus der Kapitel 4 Kandidatenvergleich, Reduktion und Grenzen ableiten kann.

## Wortbudget

**850 Wörter**

## Voraussetzungen und Übergabe

- **Voraussetzungen:** Kriterienraster aus Kapitel 2; freigegebenes Scoping; vor G4 noch ausstehende technische Implementierung und Smoke-Test.
- **Übergabe:** Fixiert Auswahl- und Berichtsregeln, sodass Kapitel 4 weder Kandidaten noch das Kombiset nachträglich begründet.

## Festes Design, auf das der Plan Bezug nimmt

| Query | Exakte Referenzform | Vorab festgelegte Kandidaten |
| --- | --- | --- |
| Q1 | `find({category: <commonCategory\|rareCategory>, isActive: true}).sort({price: -1}).limit(24)` | I1 `{category: 1}`; I2 `{category: 1, price: -1}`; I3 `{price: -1, category: 1, isActive: 1}`; I4 `{category: 1, isActive: 1, price: -1}`; I5 `{category: 1, price: -1}` mit `partialFilterExpression: {isActive: true}`. |
| Q2 | `find({tags: <commonTag\|rareTag>, isActive: true}).limit(24)` | I6 `{tags: 1}`; I7 `{tags: 1, isActive: 1}`; I8 `{tags: 1}` mit `partialFilterExpression: {isActive: true}`. |
| Q3 | `findOne({productId: <existingProductId>, isActive: true})` | I9 `{productId: 1}`, `unique: true`. |

## Absatzplan

| Absatz-ID | Funktion | Kernaussage | Begründung | Evidenzbedarf | Beziehung davor | Beziehung danach | Zielwörter | Medium | Offene Recherche |
| --- | --- | --- | --- | --- | --- | --- | ---: | --- | --- |
| ME-03-P01 | Designüberblick | Das Benchmarkdesign trennt Baseline, kontrollierten Kandidatenvergleich, Plannerbeobachtung, Reduktion und Abschlussvalidierung. | Die fünf Stufen verhindern, dass automatische Planwahl oder Einzellaufzeit als alleiniger Nachweis dient. | Eigene Methodenentscheidung; MongoDB-Dokumentation als Begründung. | Übergabe aus Theorie. | Zerlegung → ME-03-P02. | 90 | Ablaufmatrix M-ME-01 | Methodische Quelle für Datenbank-Mikrobenchmark prüfen. |
| ME-03-P02 | Daten- und Workloaddefinition | Die gemeinsame `products`-Collection wird deterministisch für 10k, 100k und 500k Dokumente erzeugt; Q1, Q2 und Q3 werden mit den festgelegten Filtern, Sortierung/Limits und Parametern definiert. | Wiederholbarkeit und Queryäquivalenz verlangen feste Daten- und Querydefinitionen. | Eigene Artefakte; Scoping-Plausibilisierung. | Konkretisierung. | Voraussetzung → ME-03-P03. | 150 | M-ME-01 | Tatsächlich erreichte Verteilungen später im Manifest dokumentieren; keine Werte vorwegnehmen. |
| ME-03-P03 | Selektivitätsvariation | Q1 nutzt common/rare Kategorien mit Zielanteilen 30–40 % bzw. 3–5 %, Q2 common/rare Tags mit 40–60 % bzw. 5–10 %, `isActive` etwa 80 %; `productId` bleibt global eindeutig. | Variation prüft relevante Bedingungen statt allgemeiner „typischer“ Verteilungen zu behaupten. | Eigene experimentelle Vorgabe; tatsächliche Anteile aus Manifest. | Fortführung. | Ursache → ME-03-P04. | 105 | M-ME-01 | Technische Prüfung der Generatorverteilung und Manifestfelder erforderlich. |
| ME-03-P04 | Kandidatenpool festlegen | Die im festen Design aufgeführten neun Kandidaten I1–I9 werden gemeinsam installiert: fünf für Q1, drei für Q2 und der eindeutige `productId`-Index für Q3. | Gemeinsamer Pool ermöglicht sowohl kontrollierte Vergleiche als auch reale Plannerkonkurrenz. | Eigene Kandidatenmatrix; Regeln aus Kapitel 2. | Anwendung. | Folge → ME-03-P05. | 145 | Tabelle M-ME-02 | Smoke-Test: gleichzeitige Full-/Partial-Kandidaten mit gleichem Key Pattern in MongoDB 8.2.11, Hint über Namen. |
| ME-03-P05 | Kontrollierte Read-Messung | Baseline enthält nur `_id`; geeignete Kandidaten werden im Pool per Indexname gehintet, pro Konstellation nach drei Warmups zehnmal gemessen und in deterministisch rotierter Reihenfolge ausgeführt. | Erfasst Kandidatenwirkung unter gleicher Wiederholungsregel und begrenzt Reihenfolgeeffekte. | MongoDB Hint; eigene Runner-Konfiguration. | Fortführung. | Kontrast → ME-03-P06. | 120 | M-ME-01 | Vollständigen Cursor-Konsum und genaue Zeitmessung technisch prüfen. |
| ME-03-P06 | Planner und Messkriterien trennen | Zusätzlich wird ohne Hint die Plannerwahl erhoben; Explain-Struktur und separat gemessene reale Queryzeit werden getrennt ausgewertet, einschließlich Ergebnisvalidierung. | Explain ignoriert Plan Cache und seine Zeit ist nicht identisch mit Anwendungszeit. | MongoDB Explain/Query Plans; eigene Validierungsartefakte. | Kontrast. | Folge → ME-03-P07. | 100 | keines | Q2-Validierung für unsortierte, limitierte Ergebnisse spezifizieren. |
| ME-03-P07 | Reduktion und Finallauf definieren | Kandidaten werden anhand von Korrektheit, Strukturmetriken, Laufzeit, Speicher, Präfixabdeckung und bedingter Nutzbarkeit reduziert; nur das resultierende Set wird ohne Hint workloadweit gegen die Baseline validiert. | Die finale Empfehlung entsteht aus Abwägung, nicht aus einer vorab behaupteten Rangfolge. | Eigene Entscheidungsregel; Kapitel-2-Kriterien. | Synthese. | Einschränkung → ME-03-P08. | 80 | M-ME-01 | Regel für nicht dominierte Kandidaten als bedingte Empfehlung formal festhalten. |
| ME-03-P08 | Schreibtest und Reproduzierbarkeit begrenzen | Bei 500k vergleicht der Test nur Baseline und finales Set: 1.000 Inserts sowie Aktivstatus-Updates an 1.000 via `_id` adressierten Produkten, jeweils fünfmal bei gleichen Ausgangszuständen; Manifeste und Rohdaten sichern Nachvollziehbarkeit. | Quantifiziert Wartungsaufwand, ohne einen vollständigen Schreibworkload zu beanspruchen. | MongoDB Write Performance; eigene Write-Artefakte. | Einschränkung. | Übergabe → Kapitel 4. | 60 | keines | Reset-Strategie, Write Concern und Bulk-Optionen vor Durchführung fixieren. |

**Summe Zielwörter: 90 + 150 + 105 + 145 + 120 + 100 + 80 + 60 = 850.**

## Medienplan

| Medium-ID | Typ | Aussagefunktion | Herkunft/Erzeugung | Beschriftung | Einführung und Interpretation |
| --- | --- | --- | --- | --- | --- |
| M-ME-01 | Tabelle | Verdichtet Skalen, Selektivitätsvarianten, Messstufen, Wiederholungen und Validierung in eine reproduzierbare Versuchsmatrix. | Eigene Methodenbeschreibung. | „Versuchsmatrix des Referenzworkloads“ | In ME-03-P01 einführen; ME-03-P02 bis P08 erläutern die Zeilen der Matrix. |
| M-ME-02 | Tabelle | Ordnet I1–I9 mit Key Pattern, Unique-/Partial-Option und Query-Eignung zu, ohne ihre spätere Eignung zu bewerten. | Eigene Kandidatenregistry. | „Vorab festgelegter Indexkandidatenpool“ | In ME-03-P04 einführen und in Kapitel 4 als Kennzeichnung wiederverwenden. |

## Offene Entscheidungen

- Full-/Partial-Smoketest, Q2-Äquivalenzprüfung, Cursor-Konsum und gleiche Schreib-Ausgangszustände müssen vor G4 technisch verifiziert werden.
- Das Kapitel darf keine finale Indexkombination oder Messwerte enthalten.
