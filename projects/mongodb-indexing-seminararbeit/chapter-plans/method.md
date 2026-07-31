# Kapitelplan: method Referenzworkload, Indexdesign und Benchmarkmethode

## Funktion im Gesamtargument

Das Kapitel übersetzt die Forschungsfrage in ein reproduzierbares, vor Ergebniskenntnis festgelegtes Verfahren. Es dokumentiert Referenzdaten, Query Shapes, elf Kandidaten, fünf Messstufen, Metriken und Validierungsregeln, ohne Ergebnisse oder finale Konfiguration zu nennen.

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
| Q3 | `find({category: <commonCategory\|rareCategory>, isActive: true, price: {$gte: <minPrice>, $lte: <maxPrice>}}).sort({createdAt: -1}).limit(24)` | I9 `{category: 1, isActive: 1, createdAt: -1, price: 1}`; I10 `{category: 1, isActive: 1, price: 1, createdAt: -1}`; I11 `{category: 1, createdAt: -1, price: 1}` mit `partialFilterExpression: {isActive: true}`. |

## Query-übergreifende Kandidatenprüfung

Die Zuordnung eines Kandidaten zu Q1, Q2 oder Q3 begründet seine Herleitung, beschränkt aber nicht seinen späteren Vergleich. Alle technisch und semantisch kompatiblen Kandidaten werden per Hint gegen den jeweiligen Query Shape geprüft. Dadurch kann beispielsweise ein zunächst für Q3 entwickelter Compound-Index auch für Q1 als Ersatz oder Ergänzung relevant werden.

| Query | Kontrolliert verglichene Kandidaten | Ausschlussgrund anderer Kandidaten |
| --- | --- | --- |
| Q1 | I1–I5 sowie I9–I11 | I6–I8 enthalten nur `tags` und bilden keinen fachlich sinnvollen Zugriffspfad für die Kategorie-/Preisabfrage. |
| Q2 | I6–I8 | I1–I5 und I9–I11 enthalten kein `tags`-Feld und würden nur künstliche Scan-Vergleiche erzeugen. |
| Q3 | I1–I5 sowie I9–I11 | I6–I8 enthalten nur `tags` und bilden keinen fachlich sinnvollen Zugriffspfad für Filter, Preisbereich und Neuheitssortierung. |

## Absatzplan

| Absatz-ID | Funktion | Kernaussage | Begründung | Evidenzbedarf | Beziehung davor | Beziehung danach | Zielwörter | Medium | Offene Recherche |
| --- | --- | --- | --- | --- | --- | --- | ---: | --- | --- |
| ME-03-P01 | Designüberblick | Das Benchmarkdesign trennt Baseline, kontrollierten Kandidatenvergleich, Plannerbeobachtung, Reduktion und Abschlussvalidierung. | Die fünf Stufen verhindern, dass automatische Planwahl oder Einzellaufzeit als alleiniger Nachweis dient. | Eigene Methodenentscheidung; methodische Benchmarkquelle und MongoDB-Dokumentation. | Übergabe aus Theorie. | Zerlegung → ME-03-P02. | 70 | Ablaufmatrix M-ME-01 | Methodische Quelle für Datenbank-Mikrobenchmarks prüfen. |
| ME-03-P02 | Daten- und Workloaddefinition | Die gemeinsame `products`-Collection wird deterministisch für 10k, 100k und 500k Dokumente erzeugt und enthält zusätzlich `createdAt`; Q1, Q2 und Q3 werden mit festgelegten Filtern, Sortierung, Limits und Parametern definiert. | Wiederholbarkeit und Queryäquivalenz verlangen feste Daten- und Querydefinitionen. | Eigene Artefakte; Scoping-Plausibilisierung. | Konkretisierung. | Voraussetzung → ME-03-P03. | 120 | M-ME-01 | Tatsächlich erreichte Verteilungen später im Manifest dokumentieren; keine Werte vorwegnehmen. |
| ME-03-P03 | Selektivitätsvariation | Q1 und Q3 nutzen common/rare Kategorien mit Zielanteilen 30–40 % bzw. 3–5 %, Q2 common/rare Tags mit 40–60 % bzw. 5–10 % und `isActive` etwa 80 %. Q3 verwendet zusätzlich einen vor Messbeginn festgelegten Preisbereich, dessen tatsächliche Trefferanteile im Manifest stehen. | Die Variation prüft kontrollierte Kontraste, ohne reale Shopverteilungen zu behaupten. | Eigene experimentelle Vorgabe; tatsächliche Anteile aus Manifest. | Fortführung. | Ursache → ME-03-P04. | 90 | M-ME-01 | Preisgrenzen, Generatorverteilung und Manifestfelder vor dem Lauf technisch fixieren. |
| ME-03-P04 | Kandidatenpool und Kompatibilitätsmatrix festlegen | Die elf Kandidaten I1–I11 werden gemeinsam installiert. Ihre ursprüngliche Herleitung bleibt query-spezifisch, der Hint-Vergleich folgt jedoch einer vorab festgelegten Kompatibilitätsmatrix: Q1 und Q3 vergleichen jeweils I1–I5 sowie I9–I11, Q2 I6–I8. | Nur der query-übergreifende Vergleich kann zeigen, ob ein Compound-Index mehrere Query Shapes abdeckt und einen anderen Index überflüssig macht. | Eigene Kandidaten- und Kompatibilitätsmatrix; Regeln aus Kapitel 2. | Anwendung. | Folge → ME-03-P05. | 120 | Tabelle M-ME-02 | Smoke-Test: gleichzeitige Full-/Partial-Kandidaten mit gleichem Key Pattern in MongoDB 8.2.11, Hint über Namen. |
| ME-03-P05 | Kontrollierte Read-Messung | Die Baseline enthält nur `_id`; alle in der Kompatibilitätsmatrix vorgesehenen Kandidaten werden je Query im Pool per Indexname gehintet, nach drei Warmups zehnmal real ausgeführt und bei vollständig konsumiertem Ergebnis in deterministisch rotierter Reihenfolge gemessen. | Erfasst Kandidatenwirkung unter gleicher Wiederholungsregel und begrenzt Reihenfolgeeffekte, ohne Kandidaten auf ihre ursprüngliche Query-Zuordnung zu beschränken. | MongoDB Hint; eigene Runner-Konfiguration. | Fortführung. | Kontrast → ME-03-P06. | 110 | M-ME-01 | Zeitmessgrenzen und vollständigen Cursor-Konsum technisch verifizieren. |
| ME-03-P06 | Planner- und Ergebnisprüfung definieren | Zusätzlich wird ohne Hint die Plannerwahl erhoben; Explain-Struktur und reale Queryzeit bleiben getrennte Evidenzen. Q1 und Q3 prüfen Filter, Kardinalität und verlangte Sortierreihenfolge; Q2 prüft wegen des unsortierten Limits Kardinalität und Prädikaterfüllung statt identischer Dokument-IDs. | Verhindert die Gleichsetzung von Plannerwahl, Explain-Zeit und Ergebnisäquivalenz. | MongoDB Explain/Query Plans; eigene Validierungsartefakte. | Kontrast. | Folge → ME-03-P07. | 90 | keines | Umsetzung der Ergebnisprüfung für Q1–Q3 automatisiert absichern; Gleichstände der Sortierwerte berücksichtigen. |
| ME-03-P07 | Kombisets ableiten und validieren | Aus der Kompatibilitätsmatrix werden dominierte oder redundant abgedeckte Kandidaten entfernt. Aus den verbleibenden, nicht dominierten Kandidaten entstehen höchstens drei plausible Kombisets; jedes wird ohne Hint workloadweit gegen die Baseline validiert. Das kleinste Set mit hinreichend belegter Abdeckung bildet die finale Empfehlung. | Die finale Empfehlung entsteht aus einer transparenten Set-Abwägung und nicht aus einer ungeprüften Sammlung von Einzelsiegern. | Eigene Entscheidungsregel; Kapitel-2-Kriterien. | Synthese. | Einschränkung → ME-03-P08. | 80 | M-ME-01 | „Hinreichende Abdeckung“ und Behandlung nicht dominierter Kandidaten vor dem Lauf regelbasiert präzisieren. |
| ME-03-P08 | Schreibtest begrenzen | Bei 500k vergleicht der Test nur Baseline und finales Set: 1.000 deterministische Inserts und 1.000 über `_id` adressierte Aktivstatus-Updates, jeweils fünfmal bei identischen Ausgangszuständen und Optionen. | Quantifiziert Indexwartung, ohne einen vollständigen Schreibworkload zu beanspruchen oder Lookup-Vorteile einzumischen. | MongoDB Write Performance; eigene Write-Artefakte. | Einschränkung. | Voraussetzung → ME-03-P09. | 80 | keines | Reset-Strategie, Write Concern und Bulk-Optionen vor Durchführung fixieren. |
| ME-03-P09 | Reproduzierbarkeit dokumentieren | MongoDB-Version, Container-/Hostumgebung, Seeds, Indexzustände, Queryparameter, Kompatibilitäts- und Kombisetdefinitionen, Cachebehandlung, Run-IDs, Manifeste, Rohdaten und Fehlerprüfungen werden versioniert festgehalten. | Erst die dokumentierte Umgebung und Artefaktkette macht den Lauf nachvollziehbar und trennt Pool-, Baseline- und Kombisetvalidierungen. | Eigene Konfigurationen und Artefakte; methodische Benchmarkquelle. | Synthese. | Übergabe → Kapitel 4. | 90 | M-ME-01 | Exakte Indexgrößenmessung, Cacheprotokoll und Ausschluss-/Fehlerregeln vor dem finalen Lauf festschreiben. |

**Summe Zielwörter: 70 + 120 + 90 + 120 + 110 + 90 + 80 + 80 + 90 = 850.**

## Medienplan

| Medium-ID | Typ | Aussagefunktion | Herkunft/Erzeugung | Beschriftung | Einführung und Interpretation |
| --- | --- | --- | --- | --- | --- |
| M-ME-01 | Tabelle | Verdichtet Skalen, Selektivitätsvarianten, Messstufen, Wiederholungen, Kompatibilitätsmatrix und Kombisetvalidierung in eine reproduzierbare Versuchsmatrix. | Eigene Methodenbeschreibung. | „Versuchsmatrix des Referenzworkloads“ | In ME-03-P01 einführen; ME-03-P02 bis P09 erläutern die Zeilen der Matrix. |
| M-ME-02 | Tabelle | Ordnet I1–I11 mit Key Pattern, Partial-Option, ursprünglicher Herleitung und query-übergreifender Kompatibilität zu, ohne ihre spätere Eignung zu bewerten. | Eigene Kandidatenregistry. | „Vorab festgelegter Indexkandidatenpool und Kompatibilitätsmatrix“ | In ME-03-P04 einführen und in Kapitel 4 als Kennzeichnung wiederverwenden. |

## Offene Entscheidungen

- Der Preisbereich für Q3, seine manifeste Trefferquote sowie die Behandlung gleicher `createdAt`-Werte müssen vor dem Lauf festgelegt werden.
- Full-/Partial-Smoketest, automatisierte Ergebnisprüfung für Q1–Q3, Cursor-Konsum und gleiche Schreib-Ausgangszustände müssen vor G4 technisch verifiziert werden.
- Das Kapitel darf keine finale Indexkombination oder Messwerte enthalten.
