# Scoping-Recherche

Stand: 31.07.2026. Durch G2 freigegebenes Scoping nach G1-Neuausrichtung. Diese Notizen prüfen Begriffe, Forschungsanschluss und Machbarkeit. Sie ersetzen weder Tiefenrecherche noch Quellensteckbriefe oder die spätere Belegprüfung.

## Ergebnis des Scopings

Die neue Forschungsfrage ist wissenschaftlich und praktisch bearbeitbar, wenn drei Ebenen strikt getrennt werden:

1. **Kandidatenprofilierung:** Baseline und einzelne optionale Indizes liefern ein vereinfachtes Kostenmodell.
2. **Indexset-Suche:** Alle 256 Teilmengen von I1 bis I8 werden rechnerisch bewertet; I9 ist fester Integritätsindex.
3. **Finalvalidierung:** Höchstens drei vorab regelbasiert ausgewählte Sets werden physisch isoliert und ohne `hint()` gemessen.

Die Arbeit entwickelt damit keinen globalen MongoDB Index Advisor. Sie untersucht ein endliches Index Selection Problem und realisiert einen wiederverwendbaren Evaluator für registrierte, ausführbare Query Shapes und vorgegebene Kandidaten.

## Forschungsanschluss

Chaudhuri und Narasayya (VLDB 1997) beschreiben die workload- und kostengetriebene Auswahl eines Indexsets, die Reduktion des Kandidatenraums und eine günstige Bewertung vieler Konfigurationen. CoPhy formalisiert große Kandidatenräume mit harten und weichen Constraints als Optimierungsproblem. SWIRL stellt das Index Selection Problem mit gewichteten Querykosten sowie Speicher- oder Kardinalitätsgrenzen dar; DRLISA überträgt workloadabhängige Auswahl auf NoSQL-Systeme mittels Deep Reinforcement Learning.

Diese Arbeiten begründen Problemformulierung, Gewichte, Constraints und die Trennung von Kostenschätzung und materialisierter Leistung. Ihre relationalen What-if-Mechanismen beziehungsweise lernenden Suchverfahren werden nicht auf MongoDB übertragen. Für acht optionale Kandidaten ist vollständige Enumeration mit 256 Sets nachvollziehbarer als lineare Optimierung, Reinforcement Learning oder genetische Suche.

Der MongoDB Atlas Performance Advisor bildet eine funktionale Nachbarschaft, aber kein identisches Werkzeug: Er gruppiert langsame Operationen nach Query Shape, bewertet Vorschläge anhand eines Impact-Maßes und dedupliziert Präfixüberschneidungen. Aus der öffentlichen Dokumentation folgt keine vollständige mehrdimensionale Indexset-Suche mit materialisierter Finalvalidierung.

Orientierende Kernquellen:

- Chaudhuri/Narasayya, *An Efficient, Cost-Driven Index Selection Tool for Microsoft SQL Server*: https://www.microsoft.com/en-us/research/publication/an-efficient-cost-driven-index-selection-tool-for-microsoft-sql-server/
- CoPhy: https://arxiv.org/abs/1104.3214
- SWIRL: https://openproceedings.org/2022/conf/edbt/paper-37.pdf
- DRLISA: https://arxiv.org/abs/2006.08842
- MongoDB Atlas Performance Advisor, Index Ranking: https://www.mongodb.com/docs/atlas/performance-advisor/index-ranking/

## Referenzworkload

Die Fallstudie nutzt eine Collection `products` und fünf konkrete Queryvarianten aus drei gleich gewichteten Query Shapes:

### Q1 – Aktive Produkte einer Kategorie

```javascript
db.products.find({
  category: <commonCategory|rareCategory>,
  isActive: true
}).sort({ price: -1 }).limit(24)
```

Q1 besitzt eine häufige und eine seltene Parametervariante. Geprüft werden Equality-Felder, Sortierunterstützung, Compound-Reihenfolge, Partial-Eignung und Präfixredundanz.

### Q2 – Aktive Produkte mit Tag

```javascript
db.products.find({
  tags: <commonTag|rareTag>,
  isActive: true
}).limit(24)
```

Q2 besitzt ebenfalls eine häufige und eine seltene Parametervariante. Sie untersucht Multikey-Zugriff, das zusätzliche Equality-Feld und Partial-Indizes. Weil keine Sortierung festgelegt ist, wird nicht die Identität derselben 24 Dokumente verlangt; geprüft werden Ergebnisumfang, Prädikaterfüllung und vollständiger Cursorverbrauch bis zum Limit.

### Q3 – Produktdetail über fachliche ID

```javascript
db.products.findOne({
  productId: <existingProductId>,
  isActive: true
})
```

I9 garantiert die Eindeutigkeit der fachlichen `productId` und ist Teil jeder zulässigen Konfiguration; `isActive` bleibt ein nachgelagertes Prädikat. Q3 prüft damit die feste Basiskonfiguration, entscheidet aber nicht zwischen den optionalen Sets. Die Alternative, `productId` direkt als `_id` zu modellieren, wird nur als Schemaentscheidung diskutiert.

Die drei Skalierungen bleiben 10.000, 100.000 und 500.000 Dokumente. Häufige und seltene Werte sind kontrollierte Versuchsfaktoren; beobachtete Trefferanteile werden je Lauf protokolliert und nicht als reale Shopverteilung ausgegeben.

## Kandidatenraum

| ID | Rolle | Indexdefinition |
| --- | --- | --- |
| I1 | optional, Q1 | `{category: 1}` |
| I2 | optional, Q1 | `{category: 1, price: -1}` |
| I3 | optional, Q1 | `{price: -1, category: 1, isActive: 1}` |
| I4 | optional, Q1 | `{category: 1, isActive: 1, price: -1}` |
| I5 | optional, Q1 | `{category: 1, price: -1}`, partial `{isActive: true}` |
| I6 | optional, Q2 | `{tags: 1}` |
| I7 | optional, Q2 | `{tags: 1, isActive: 1}` |
| I8 | optional, Q2 | `{tags: 1}`, partial `{isActive: true}` |
| I9 | verpflichtend, Q3 | `{productId: 1}`, `unique: true` |

Der optionale Suchraum ist `I_optional = {I1, ..., I8}`. Jedes untersuchte Set hat die Form `B ∪ S` mit Basiskonfiguration `B = {_id, I9}` und `S ⊆ I_optional`. Einschließlich des leeren optionalen Sets entstehen `2^8 = 256` zulässige Konfigurationen.

I3 bleibt als bewusst schwächer erwartete Sort-first-Alternative erhalten. Die Kandidaten sind vorab begründete Untersuchungshypothesen, keine Empfehlungen. I1 kann trotz Präfixabdeckung kleiner sein; I5 und I8 können Speicher und Wartung reduzieren; I7 ist zulässig, weil nur `tags` ein Arrayfeld ist. Partial-Kandidaten dürfen nur für Queries mit `isActive: true` profiliert werden.

## Technische Screening-Grenzen

MongoDB 8.0 führte Query Settings ein. `allowedIndexes` begrenzt die vom Planner betrachteten Indizes, garantiert aber keinen bestimmten Index und lässt einen Collection Scan weiterhin zu. Hidden Indexes sind plannerunsichtbar, werden jedoch bei Writes weiter gepflegt und verbrauchen Speicher und Arbeitsspeicher. Beide Mechanismen können Diagnose oder Read-Screening unterstützen, simulieren aber keine physisch kleinere Konfiguration.

Deshalb wird die Kandidatenprofilierung mit `B` plus genau einem optionalen Kandidaten durchgeführt. `hint()` isoliert nur den logisch zulässigen Zugriffspfad. Finalisten werden dagegen mit ausschließlich ihren tatsächlichen Indizes materialisiert und ohne `hint()` ausgeführt.

Relevante MongoDB-Primärdokumentation:

- Query Settings und `allowedIndexes`: https://www.mongodb.com/docs/v8.0/reference/command/setquerysettings/
- Hidden Indexes: https://www.mongodb.com/docs/manual/core/index-hidden/
- Explain Results: https://www.mongodb.com/docs/manual/reference/explain-results/
- Partial Indexes: https://www.mongodb.com/docs/v8.0/core/index-partial/
- Multikey Indexes: https://www.mongodb.com/docs/manual/core/indexes/index-types/index-multikey/
- Write Operation Performance: https://www.mongodb.com/docs/manual/core/write-performance/

## Messplan der Kandidatenprofilierung

Für jede Skalierung wird zuerst `B` gemessen. Anschließend wird pro optionalem Kandidaten ein äquivalenter Datenzustand mit `B ∪ {i}` hergestellt.

- drei Warmups und zehn Messwiederholungen je zulässiger Query-Kandidaten-Kombination;
- deterministisch rotierte Reihenfolge;
- reale Wall-Clock-Zeit einer vollständig bis `limit(24)` konsumierten Query als primäre Zeitmessung;
- `explain("executionStats")` getrennt davon für `totalDocsExamined`, `totalKeysExamined`, `nReturned`, Indexname sowie Fetch-/Sort-Stufen;
- Ergebnisvalidierung und Laufmanifest pro Messung;
- Indexgröße des optionalen Kandidaten als inkrementeller Speicherbedarf;
- bei 500.000 Ausgangsdokumenten zusätzlich fünf Wiederholungen von 1.000 Inserts und 1.000 deterministisch ausgewählten `isActive`-Änderungen für `B` und `B ∪ {i}`.

MongoDB weist ausdrücklich darauf hin, dass `explain.executionStats.executionTimeMillis` nicht zwingend die tatsächliche eingeschwungene Queryzeit repräsentiert. Explain-Struktur und separat gemessene Laufzeit bleiben deshalb unterschiedliche Messgrößen.

## Read-Kostenmodell

Die drei Query Shapes werden je Skalierung gleich gewichtet. Innerhalb von Q1 und Q2 werden häufige und seltene Variante gleich geteilt:

| Queryvariante | Gewicht |
| --- | ---: |
| Q1 häufig | 1/6 |
| Q1 selten | 1/6 |
| Q2 häufig | 1/6 |
| Q2 selten | 1/6 |
| Q3 | 1/3 |

Sei `t_d(q, B)` die mediane reale Queryzeit der Basiskonfiguration bei Skalierung `d` und `t_d(q, i)` die isolierte Messung mit Kandidat `i`. Die normalisierten Kandidatenkosten lauten:

`r_d(q, i) = t_d(q, i) / t_d(q, B)`

Für ein optionales Set `S` gilt als Screening-Schätzung:

`ĉ_d(q, S) = min(1, min_{i ∈ S und zulässig für q} r_d(q, i))`

`Ĉ_read,d(S) = Σ_q w_q · ĉ_d(q, S)`

Damit besitzt das leere optionale Set pro Skalierung Read-Kosten von 1. Die Normalisierung verhindert, dass langsame Querytypen oder die größte Skalierung allein wegen ihrer absoluten Dauer die Auswahl dominieren. Sie ist eine experimentelle Bewertungsentscheidung; absolute Medianzeiten werden zusätzlich berichtet.

Die Minimum-Schätzung nimmt jeweils den besten einzeln gemessenen Zugriff an. Sie bildet weder Index Intersection noch Cachekonkurrenz, gemeinsame Plannerentscheidungen oder sonstige Indexinteraktionen ab. Genau diese Abweichung wird durch die Finalvalidierung untersucht.

## Speicher- und Write-Proxys

Der Screening-Speicherbedarf ist die Summe der einzeln gemessenen optionalen Indexgrößen:

`M̂_d(S) = Σ_{i ∈ S} size_d(i)`

Die konstante Größe von `_id` und I9 wird separat als Basis ausgewiesen und beeinflusst die Dominanz optionaler Sets nicht.

Für Insert und Aktivstatus-Update wird bei 500.000 Dokumenten je Kandidat der nichtnegative relative Mehraufwand gegenüber `B` bestimmt. Negative Einzelabweichungen werden als Messrauschen auf null begrenzt. Der Write-Screening-Proxy ist die Summe der pro Kandidat gleich gewichteten Insert- und Update-Overheads:

`ŵ(i) = 0,5 · max(0, T_insert(B∪{i}) / T_insert(B) - 1) + 0,5 · max(0, T_update(B∪{i}) / T_update(B) - 1)`

`Ŵ(S) = Σ_{i ∈ S} ŵ(i)`

Diese Additivität ist ausdrücklich nur ein Ranking-Proxy. Belastbare Write-Aussagen stammen ausschließlich aus den materialisierten Finalisten.

## Pareto-Auswertung und Skalen

Für jede Skalierung wird ein eigener Zielvektor ausgewertet:

`F_d(S) = (Ĉ_read,d(S), Ŵ(S), M̂_d(S))`

Ein Set wird entfernt, wenn ein anderes Set in allen drei Dimensionen mindestens gleich gut und in mindestens einer Dimension besser ist. Es gibt keine frei erfundene skalare Gesamtstrafe. Das Werkzeug unterstützt zusätzlich optionale Grenzen für Speicher, Indexanzahl und Write-Proxy.

Die Skalen werden nicht zu einer einzigen Kostenfunktion vermischt. Berichtet werden Pareto-Mitgliedschaft und Rangwechsel je Skalierung. Dadurch bleibt sichtbar, ob ein Set nur bei kleinen Datenmengen oder robust über mehrere Skalierungen attraktiv ist.

## Deterministische Finalistenauswahl

Die Basiskonfiguration `B` wird immer separat validiert und zählt nicht als Finalist. Für Speicher- und Write-Kompromiss werden zunächst drei Read-Toleranzbänder von 5 %, 10 % und 20 % rechnerisch ausgewertet. Das 10-%-Band ist die vorab festgelegte primäre Auswahlregel; 5 % und 20 % bilden eine Sensitivitätsanalyse. Aus der Vereinigung der drei Pareto-Fronten werden anhand des primären Bandes höchstens drei unterschiedliche Sets gewählt:

1. **Read-Anker:** niedrigstes `Ĉ_read` bei 500.000 Dokumenten; Tie-Breaker: weniger optionale Indizes, weniger Speicher, niedrigerer Write-Proxy, anschließend lexikografische Index-IDs.
2. **Speicherkompromiss:** unter den Sets mit höchstens 10 % höheren Read-Kosten als der Read-Anker bei 500.000 Dokumenten das Set mit dem geringsten optionalen Speicher; dieselben Tie-Breaker.
3. **Write-Kompromiss:** innerhalb desselben primären 10-%-Read-Bandes das Set mit dem niedrigsten Write-Proxy; dieselben Tie-Breaker.

Für die Sensitivitätsanalyse werden die beiden Kompromissfunktionen zusätzlich mit 5 % und 20 % ausgeführt. Es wird ausgewiesen, ob dieselben Sets gewählt werden, welche Kandidaten wechseln und wie groß die Änderungen bei Indexanzahl, Speicher- und Write-Proxy sind. Diese Alternativen werden nicht allein wegen eines günstigeren späteren Ergebnisses als Finalisten nachnominiert.

Duplikate im primären 10-%-Band werden nicht ersetzt; dadurch können weniger als drei Finalisten entstehen. Das 10-%-Band ist eine explizite Fallstudienpräferenz und keine natürliche MongoDB-Grenze. Die flankierenden Bänder prüfen, wie stark die Auswahl von dieser Präferenz abhängt. Alle Regeln werden vor Kenntnis der Finalmessungen festgeschrieben.

## Finalvalidierung

Für `B` und jeden Finalisten wird ein sauberer äquivalenter Ausgangszustand hergestellt. Installiert sind nur `_id`, I9 und die optionalen Indizes des jeweiligen Sets.

- alle Queryvarianten ohne `hint()`;
- drei Warmups und zehn reale Zeitmessungen je Variante und Skalierung;
- Explain-Struktur getrennt von realer Laufzeit;
- tatsächliche gesamte und inkrementelle Indexgröße;
- fünf Wiederholungen identischer Batches mit 1.000 Inserts und 1.000 `isActive`-Updates bei 500.000 Ausgangsdokumenten;
- Vergleich von `Ĉ_read,d(S)` mit den aus den tatsächlichen Setläufen berechneten Read-Kosten;
- Abweichung, Plannerwahl und mögliche Indexinteraktionen als eigenes empirisches Ergebnis.

Die physische Validierung bleibt auf den Read-Anker und die höchstens zwei unterschiedlichen Kandidaten des primären 10-%-Bandes begrenzt. Kandidaten, die ausschließlich in der 5-%- oder 20-%-Sensitivitätsanalyse auftreten, werden als bedingte Screening-Ergebnisse berichtet, aber nicht zusätzlich materialisiert.

## Praktische Machbarkeit

Der vorhandene Benchmark besitzt deterministische Datengenerierung, Query- und Indexregistries, Explain-Parsing, Profile, Ergebnisvalidierung und Artefaktarchive. Der aktuelle Runner bildet jedoch eine Scenario-Strategy-Matrix ab und materialisiert einzelne Strategien; Workloadgewichte, Pflichtindizes, Enumeration, Pareto-Analyse, Constraints und Finalistenläufe fehlen noch.

Das Softwareartefakt ist daher eine substanzielle, aber begrenzte Erweiterung. Bei acht optionalen Kandidaten ist die Enumeration rechnerisch trivial. Der Hauptaufwand liegt in reproduzierbarer Profilierung, Zustandsisolation und korrekter Trennung von Screening und Finalmessung.

`$queryStats` kann in einer späteren Anwendung Query Shapes und beobachtete Häufigkeiten liefern, ist für die Fallstudie aber ungeeignet: Die Funktion ist laut MongoDB nicht stabil garantiert und an bestimmte Atlas-Voraussetzungen gebunden. Die Fallstudie verwendet daher versionierte Workloadkonfigurationen.

## Konsequenzen für die Arbeit

- Theorie erläutert nur MongoDB-Indexmerkmale, die im Kandidatenraum vorkommen, und führt anschließend das workloadbasierte Index Selection Problem, Constraints und Pareto-Dominanz ein.
- Methodik trennt Kandidatenprofilierung, Kostenmodell, vollständige Enumeration, Finalistenauswahl und physische Validierung.
- Ergebnisse berichten zuerst Screening und Pareto-Fronten, danach Schätzungsabweichung und reale Trade-offs der Finalisten.
- Die Empfehlung ist bedingt; „global optimal“ und „universell bestes Set“ bleiben ausgeschlossen.
- Der Lauf vom 24.07.2026 ist nur Pilot und keine Evidenz für die neue Forschungsfrage.

## Evidenzlücken für G4

- exakte Fundstellen in den wissenschaftlichen Index-Advisor-Publikationen;
- belastbare Benchmarkliteratur zu Laufzeitmessung, Warmups und Systemeffekten;
- präzise MongoDB-8.x-Fundstellen für Compound-Präfixe, ESR, Partial- und Multikey-Bedingungen;
- eigene Smoke-Tests zur Koexistenz gleichartiger Full-/Partial-Key-Patterns und zur vollständigen Ergebnisvalidierung;
- empirische Prüfung, ob zehn Read- und fünf Write-Wiederholungen unter der konkreten Umgebung stabile Medianwerte liefern.
