# Claim-Evidence-Ledger

Dieses Dokument ist die aktive Arbeitsgrundlage für Review und Schreiben. Eine
Belegakte behandelt genau eine geplante, belegpflichtige Aussage. Ihre ID stimmt
mit `evidence_id` in `evidence-matrix.csv` überein. Wörtliche Auszüge und
quellennahe Inhaltsnotizen stehen ausschließlich hier, nicht in den
Quellensteckbriefen. Alle Reviewentscheidungen bleiben bis zum menschlichen
Originalabgleich offen; `ready` bedeutet keine menschliche Verifikation.

## EXT-001 – Workloadbezogene Indexkonfiguration

- **Absatz-ID und Funktion:** `IN-01-P01`; eröffnet das Problem als Setentscheidung für einen Workload.
- **Belegpflichtige Aussage:** Workloadbezogene Indexauswahl betrifft eine Indexkonfiguration und nicht nur isolierte Einzelindizes.
- **Beabsichtigte eigene Formulierung:** Für einen Workload ist nicht der jeweils lokal günstigste Einzelindex maßgeblich, sondern eine gemeinsam zu bewertende Indexmenge.
- **Evidenztyp:** `external`
- **Status:** `ready`

### SCI-001 – stützende Evidenz

- **Rolle:** stützend
- **Vollbeleg:** Chaudhuri, Surajit; Narasayya, Vivek R. (1997): An Efficient, Cost-Driven Index Selection Tool for Microsoft SQL Server. In: VLDB 1997, S. 146–155.
- **Originalquelle:** [SCI-001](https://www.vldb.org/conf/1997/P146.PDF)
- **Ausgabe / Version:** VLDB 1997
- **Fundstelle:** S. 147, Abschnitt 2.1 `Problem Statement`
- **Originalaussage / quellennaher Auszug:**

  > Our goal is to pick a set of indexes that is suitable for a given database and workload.

- **Eigene Interpretation:** Die Stelle formuliert Indexauswahl ausdrücklich als Auswahl einer Indexmenge für einen gegebenen Workload.
- **Grenze und Kontext:** SQL Server und optimizerbasierte Kosten; keine MongoDB-spezifische Auswahlmethode.
- **Reviewentscheidung:** offen

### SCI-003 – unabhängige stützende Evidenz

- **Rolle:** stützend
- **Vollbeleg:** Kossmann, Jan; Kastius, Alexander; Schlosser, Rainer (2022): SWIRL: Selection of Workload-aware Indexes using Reinforcement Learning. EDBT 2022, S. 155–168.
- **Originalquelle:** [SCI-003](https://doi.org/10.48786/EDBT.2022.06)
- **Ausgabe / Version:** EDBT 2022
- **Fundstelle:** S. 156, Abschnitt 2.1 `The Index Selection Problem`
- **Originalaussage / quellennaher Auszug:**

  > [Quellennahe Inhaltsnotiz] SWIRL fasst Indexauswahl als Bestimmung einer Teilmenge aus einer Kandidatenmenge für einen Workload auf.

- **Eigene Interpretation:** Die neuere Arbeit bestätigt die Set- und Workloadperspektive unabhängig von SCI-001.
- **Grenze und Kontext:** PostgreSQL und Reinforcement Learning; keine Übertragung des Suchverfahrens auf MongoDB.
- **Reviewentscheidung:** offen

## EXT-002 – Gemeinsame Kosten- und Constraintperspektive

- **Absatz-ID und Funktion:** `IN-01-P02`; präzisiert den Entscheidungskonflikt.
- **Belegpflichtige Aussage:** Indexauswahl kann Workloadkosten, Speicher und Write-Aufwand als Ziele oder Constraints gemeinsam berücksichtigen.
- **Beabsichtigte eigene Formulierung:** Ein geeignetes Set muss den Leseaufwand gegen Speicherbedarf und zusätzlichen Schreibaufwand abwägen; Grenzen können den zulässigen Raum einschränken.
- **Evidenztyp:** `external`
- **Status:** `ready`

### SCI-002 – stützende Evidenz

- **Rolle:** stützend
- **Vollbeleg:** Dash, Debabrata; Polyzotis, Neoklis; Ailamaki, Anastasia (2011): CoPhy: A Scalable, Portable, and Interactive Index Advisor for Large Workloads. PVLDB 4(6).
- **Originalquelle:** [SCI-002](https://www.cs.cmu.edu/~ddash/cophy.pdf)
- **Ausgabe / Version:** PVLDB 2011; offene Autorenfassung
- **Fundstelle:** Dokumentseite 8, Abschnitt 4 `Adding Constraints`; Dokumentseite 19, Abschnitt A.4 `Soft Constraints`
- **Originalaussage / quellennaher Auszug:**

  > [Quellennahe Inhaltsnotiz] CoPhy berücksichtigt Speicher- und Updategrenzen und beschreibt mehrere weiche Ziele über nicht dominierte Lösungen.

- **Eigene Interpretation:** Die Quelle belegt, dass Workloadkosten nicht zwingend die einzige Bewertungsdimension der Indexauswahl sind.
- **Grenze und Kontext:** CoPhy verwendet relationale Optimizerkosten und eine andere Optimierungsformulierung; die drei Ziele der Fallstudie sind eine eigene Setzung.
- **Reviewentscheidung:** offen

### SCI-003 – ergänzende Evidenz

- **Rolle:** stützend
- **Vollbeleg:** Kossmann, Jan; Kastius, Alexander; Schlosser, Rainer (2022): SWIRL: Selection of Workload-aware Indexes using Reinforcement Learning. EDBT 2022, S. 155–168.
- **Originalquelle:** [SCI-003](https://doi.org/10.48786/EDBT.2022.06)
- **Ausgabe / Version:** EDBT 2022
- **Fundstelle:** S. 156, Abschnitt 2.2 `Problem Formalization`
- **Originalaussage / quellennaher Auszug:**

  > [Quellennahe Inhaltsnotiz] Die Problemformalisierung verwendet Workloadfrequenzen sowie Speicher- oder Indexanzahlgrenzen.

- **Eigene Interpretation:** Die Stelle stützt Gewichte und Constraints als Bestandteile einer workloadbezogenen Auswahl.
- **Grenze und Kontext:** Sie begründet weder die konkreten Gleichgewichte noch die Proxyformeln der Fallstudie.
- **Reviewentscheidung:** offen

### MDB-011 – begrenzende Produktevidenz

- **Rolle:** stützend und begrenzend
- **Vollbeleg:** MongoDB, Inc. (2026): Write Operation Performance. MongoDB Database Manual.
- **Originalquelle:** [MDB-011](https://www.mongodb.com/docs/manual/core/write-performance/)
- **Ausgabe / Version:** aktuelle Manual-Seite; abgerufen 2026-08-01
- **Fundstelle:** Abschnitt `Indexes`
- **Originalaussage / quellennaher Auszug:**

  > Each index on a collection adds some amount of overhead to the performance of write operations.

- **Eigene Interpretation:** Zusätzliche MongoDB-Indizes begründen eine eigene Write-Kostendimension.
- **Grenze und Kontext:** Die Dokumentation quantifiziert den Overhead nicht und belegt keine additive Proxyformel.
- **Reviewentscheidung:** offen

## EXT-003 – Query Shape und experimentelle Workloadschicht

- **Absatz-ID und Funktion:** `FO-02-P01`; definiert die Einheit der späteren Kostenbewertung.
- **Belegpflichtige Aussage:** MongoDB gruppiert strukturell ähnliche Queries als Query Shapes; konkrete Parameterfälle und Gewichte bilden eine zusätzliche experimentelle Workloadschicht.
- **Beabsichtigte eigene Formulierung:** Die Fallstudie trennt die strukturelle Query Shape von ihren konkreten Parameterfällen und den dafür gesetzten Gewichten.
- **Evidenztyp:** `external`
- **Status:** `ready`

### MDB-001 – stützende Evidenz

- **Rolle:** stützend
- **Vollbeleg:** MongoDB, Inc. (2026): Query Shapes. MongoDB Database Manual v8.0.
- **Originalquelle:** [MDB-001](https://www.mongodb.com/docs/v8.0/core/query-shapes/)
- **Ausgabe / Version:** MongoDB 8.0; abgerufen 2026-08-01
- **Fundstelle:** Abschnitt `Query Shapes`, Definition und `Behavior`
- **Originalaussage / quellennaher Auszug:**

  > A query shape is a set of specifications that group similar queries together.

- **Eigene Interpretation:** Die Produktdokumentation liefert den strukturellen Shape-Begriff, von dem die experimentellen Parameterfälle getrennt werden.
- **Grenze und Kontext:** Gewichte und Fallstudienparameter sind keine Eigenschaften der MongoDB-Definition.
- **Reviewentscheidung:** offen

### SCI-003 – ergänzende Evidenz

- **Rolle:** stützend
- **Vollbeleg:** Kossmann, Jan; Kastius, Alexander; Schlosser, Rainer (2022): SWIRL: Selection of Workload-aware Indexes using Reinforcement Learning. EDBT 2022, S. 155–168.
- **Originalquelle:** [SCI-003](https://doi.org/10.48786/EDBT.2022.06)
- **Ausgabe / Version:** EDBT 2022
- **Fundstelle:** S. 156, Abschnitt 2.2 `Problem Formalization`
- **Originalaussage / quellennaher Auszug:**

  > [Quellennahe Inhaltsnotiz] SWIRL ordnet den Queries eines Workloads Auftretenshäufigkeiten für die Kostenaggregation zu.

- **Eigene Interpretation:** Die Stelle begründet Gewichte als zusätzliche Workloadinformation.
- **Grenze und Kontext:** Die Gleichgewichtung der Fallstudie ist eine eigene Referenzannahme und keine empirische Häufigkeitsaussage.
- **Reviewentscheidung:** offen

## EXT-004 – Explain-Struktur und Laufzeit

- **Absatz-ID und Funktion:** `FO-02-P02`; trennt Planbeobachtung und Zeitmessung.
- **Belegpflichtige Aussage:** Explain-Strukturmetriken und separat gemessene Laufzeiten besitzen unterschiedliche Aussagekraft.
- **Beabsichtigte eigene Formulierung:** Explain zeigt Planstufen und strukturelle Ausführungsmetriken; die reale Queryzeit wird davon getrennt gemessen und interpretiert.
- **Evidenztyp:** `external`
- **Status:** `ready`

### MDB-004 – stützende und begrenzende Evidenz

- **Rolle:** stützend und begrenzend
- **Vollbeleg:** MongoDB, Inc. (2026): Explain Results. MongoDB Database Manual v8.0.
- **Originalquelle:** [MDB-004](https://www.mongodb.com/docs/v8.0/reference/explain-results/)
- **Ausgabe / Version:** MongoDB 8.0; abgerufen 2026-08-01
- **Fundstelle:** Abschnitt `executionStats`; Felder `nReturned`, `executionTimeMillis`, `totalKeysExamined` und `totalDocsExamined`
- **Originalaussage / quellennaher Auszug:**

  > The time reported by explain.executionStats.executionTimeMillis is not necessarily representative of actual query time.

- **Eigene Interpretation:** Die MongoDB-Dokumentation selbst begrenzt die Aussagekraft der Explain-Zeit; Strukturmetriken und separat gemessene Laufzeit dürfen nicht gleichgesetzt werden.
- **Grenze und Kontext:** Auch separat gemessene Zeit wird dadurch nicht automatisch reproduzierbar; dafür ist das Versuchsdesign maßgeblich.
- **Reviewentscheidung:** offen

## EXT-005 – Compound-Reihenfolge, Präfixe und ESR

- **Absatz-ID und Funktion:** `FO-02-P03`; leitet die Q1-Kandidaten fachlich her.
- **Belegpflichtige Aussage:** Feldreihenfolge und Anfangspräfixe bestimmen die Nutzbarkeit von Compound-Indizes für Equality und Sortierung.
- **Beabsichtigte eigene Formulierung:** Bei Compound-Indizes beeinflussen Anfangsfelder und ESR-Reihenfolge, welche Filter- und Sortieranteile einer Query unterstützt werden.
- **Evidenztyp:** `external`
- **Status:** `ready`

### MDB-002 – stützende Evidenz

- **Rolle:** stützend
- **Vollbeleg:** MongoDB, Inc. (2026): The ESR (Equality, Sort, Range) Guideline. MongoDB Database Manual v8.0.
- **Originalquelle:** [MDB-002](https://www.mongodb.com/docs/v8.0/tutorial/equality-sort-range-guideline/)
- **Ausgabe / Version:** MongoDB 8.0; abgerufen 2026-08-01
- **Fundstelle:** Einleitung sowie Abschnitte `Equality`, `Sort` und `Range`
- **Originalaussage / quellennaher Auszug:**

  > Ensure that equality fields always come first.

- **Eigene Interpretation:** Die ESR-Leitlinie begründet Equality-Felder am Anfang der für Q1 entworfenen Kandidaten.
- **Grenze und Kontext:** ESR ist eine Leitlinie und kein Nachweis einer allgemein schnellsten Feldreihenfolge; ERS kann je nach Range-Selektivität sinnvoll sein.
- **Reviewentscheidung:** offen

### MDB-003 – ergänzende Evidenz

- **Rolle:** stützend
- **Vollbeleg:** MongoDB, Inc. (2026): Compound Indexes / Create a Compound Index. MongoDB Database Manual v8.0.
- **Originalquelle:** [MDB-003](https://www.mongodb.com/docs/v8.0/core/indexes/index-types/index-compound/create-compound-index/)
- **Ausgabe / Version:** MongoDB 8.0; abgerufen 2026-08-01
- **Fundstelle:** Abschnitte `Results` und `Index Prefix`
- **Originalaussage / quellennaher Auszug:**

  > Compound indexes improve performance for queries on exactly the fields in the index or fields in the index prefix.

- **Eigene Interpretation:** Die Präfixregel begründet, warum sich Kandidaten funktional überschneiden können.
- **Grenze und Kontext:** Präfixabdeckung beweist weder identische Kosten noch die Redundanz eines kleineren Indexes im untersuchten Workload.
- **Reviewentscheidung:** offen

## EXT-006 – Partial, Multikey und Unique

- **Absatz-ID und Funktion:** `FO-02-P04`; begründet Eligibility und Pflichtindex.
- **Belegpflichtige Aussage:** Partial-Indizes erfordern passende Querybedingungen, Arrayindizes werden multikey und Unique-Indizes können fachliche Eindeutigkeit erzwingen.
- **Beabsichtigte eigene Formulierung:** Die Kandidaten sind nur zulässig, wenn Partial-Filter logisch erfüllt und Multikey-Grenzen eingehalten werden; I9 bleibt wegen seiner Unique-Integritätsfunktion verpflichtend.
- **Evidenztyp:** `external`
- **Status:** `ready`

### MDB-005 – stützende Evidenz

- **Rolle:** stützend
- **Vollbeleg:** MongoDB, Inc. (2026): Partial Indexes. MongoDB Database Manual v8.2.
- **Originalquelle:** [MDB-005](https://www.mongodb.com/docs/v8.2/core/index-partial/)
- **Ausgabe / Version:** MongoDB 8.2; abgerufen 2026-08-01
- **Fundstelle:** Abschnitt `Behavior – Query Coverage`; Abschnitt `Restrictions`
- **Originalaussage / quellennaher Auszug:**

  > To use the partial index, a query must contain the filter expression.

- **Eigene Interpretation:** Die Stelle stützt die Eligibility von I5 und I8 nur für Queryvarianten mit der passenden Aktivbedingung.
- **Grenze und Kontext:** Sie belegt keine konkrete Speicher- oder Laufzeitersparnis.
- **Reviewentscheidung:** offen

### MDB-006 – stützende Evidenz

- **Rolle:** stützend
- **Vollbeleg:** MongoDB, Inc. (2026): Multikey Indexes. MongoDB Database Manual.
- **Originalquelle:** [MDB-006](https://www.mongodb.com/docs/manual/core/indexes/index-types/index-multikey/)
- **Ausgabe / Version:** aktuelle Manual-Seite; abgerufen 2026-08-01
- **Fundstelle:** Einleitung und Abschnitt `Compound Multikey Indexes`
- **Originalaussage / quellennaher Auszug:**

  > MongoDB automatically sets that index to be a multikey index.

- **Eigene Interpretation:** Ein Index auf `tags` erhält Multikey-Semantik; die Compound-Grenzen bestimmen die fachliche Zulässigkeit von I7.
- **Grenze und Kontext:** Die konkrete Registry und Koexistenz aller Kandidaten müssen durch Smoke-Tests belegt werden.
- **Reviewentscheidung:** offen

### MDB-007 – stützende Evidenz

- **Rolle:** stützend
- **Vollbeleg:** MongoDB, Inc. (2026): Unique Indexes. MongoDB Database Manual.
- **Originalquelle:** [MDB-007](https://www.mongodb.com/docs/manual/core/index-unique/)
- **Ausgabe / Version:** aktuelle Manual-Seite; abgerufen 2026-08-01
- **Fundstelle:** Einleitung und Abschnitt `Unique Constraint Across Separate Documents`
- **Originalaussage / quellennaher Auszug:**

  > The unique index prevents different documents in the collection from having the same value for the indexed key.

- **Eigene Interpretation:** I9 kann die Eindeutigkeit der fachlichen `productId` absichern und wird deshalb getrennt von optionalen Optimierungsindizes modelliert.
- **Grenze und Kontext:** Die separate `productId` statt ihrer Nutzung als `_id` ist eine eigene Schemaentscheidung.
- **Reviewentscheidung:** offen

## EXT-007 – Endliches Index Selection Problem

- **Absatz-ID und Funktion:** `FO-02-P05`; formalisiert Baseline, Kandidaten und zulässige Sets.
- **Belegpflichtige Aussage:** Ein Index Selection Problem wählt eine Kandidatenteilmenge für einen gewichteten Workload unter Grenzen.
- **Beabsichtigte eigene Formulierung:** Aus Basiskonfiguration und optionalen Kandidaten entsteht ein endlicher Konfigurationsraum, dessen Sets workloadbezogen und unter optionalen Grenzen bewertet werden.
- **Evidenztyp:** `external`
- **Status:** `ready`

### SCI-001 – stützende Evidenz

- **Rolle:** stützend
- **Vollbeleg:** Chaudhuri, Surajit; Narasayya, Vivek R. (1997): An Efficient, Cost-Driven Index Selection Tool for Microsoft SQL Server. In: VLDB 1997, S. 146–155.
- **Originalquelle:** [SCI-001](https://www.vldb.org/conf/1997/P146.PDF)
- **Ausgabe / Version:** VLDB 1997
- **Fundstelle:** S. 147–148, Abschnitt 2.2 `Architecture of the Index Selection Tool`
- **Originalaussage / quellennaher Auszug:**

  > [Quellennahe Inhaltsnotiz] Die Architektur trennt Kandidatenauswahl, Suche im Konfigurationsraum und Kostenbewertung für den Workload.

- **Eigene Interpretation:** Die Quelle stützt die Zerlegung des Auswahlproblems in Kandidaten, Konfigurationen und Bewertung.
- **Grenze und Kontext:** Die Fallstudie nutzt physische Profile statt hypothetischer SQL-Server-Indizes.
- **Reviewentscheidung:** offen

### SCI-003 – ergänzende Evidenz

- **Rolle:** stützend
- **Vollbeleg:** Kossmann, Jan; Kastius, Alexander; Schlosser, Rainer (2022): SWIRL: Selection of Workload-aware Indexes using Reinforcement Learning. EDBT 2022, S. 155–168.
- **Originalquelle:** [SCI-003](https://doi.org/10.48786/EDBT.2022.06)
- **Ausgabe / Version:** EDBT 2022
- **Fundstelle:** S. 156, Abschnitt 2.2 `Problem Formalization`
- **Originalaussage / quellennaher Auszug:**

  > [Quellennahe Inhaltsnotiz] Kandidatenmenge, Workloadfrequenzen und Speicher- beziehungsweise Kardinalitätsgrenzen bilden gemeinsam das Auswahlproblem.

- **Eigene Interpretation:** Die Stelle unterstützt die formale Mengen- und Constraintperspektive.
- **Grenze und Kontext:** Acht Kandidaten und vollständige Enumeration sind projektspezifisch.
- **Reviewentscheidung:** offen

## EXT-008 – Pareto-Dominanz bei mehreren Zielen

- **Absatz-ID und Funktion:** `FO-02-P06`; erklärt die Mehrzielauswertung.
- **Belegpflichtige Aussage:** Bei mehreren Zielen besteht die Lösung aus nicht dominierten beziehungsweise Pareto-optimalen Punkten statt zwingend aus einem eindeutigen Optimum.
- **Beabsichtigte eigene Formulierung:** Ein Set bleibt auf der Pareto-Front, wenn kein anderes Set in allen betrachteten Zielen mindestens gleich gut und in einem Ziel besser ist.
- **Evidenztyp:** `external`
- **Status:** `ready`

### SCI-002 – stützende Evidenz

- **Rolle:** stützend
- **Vollbeleg:** Dash, Debabrata; Polyzotis, Neoklis; Ailamaki, Anastasia (2011): CoPhy: A Scalable, Portable, and Interactive Index Advisor for Large Workloads. PVLDB 4(6).
- **Originalquelle:** [SCI-002](https://www.cs.cmu.edu/~ddash/cophy.pdf)
- **Ausgabe / Version:** PVLDB 2011; offene Autorenfassung
- **Fundstelle:** Dokumentseite 19, Abschnitt A.4 `Soft Constraints`
- **Originalaussage / quellennaher Auszug:**

  > The set of such points is called “pareto-optimal” or “sky-line” points.

- **Eigene Interpretation:** CoPhy verwendet nicht dominierte Punkte ausdrücklich für mehrere Ziele der Indexauswahl.
- **Grenze und Kontext:** CoPhy scalarisiert Teilprobleme; die Fallstudie prüft Dominanz direkt über vollständig enumerierte Sets.
- **Reviewentscheidung:** offen

## EXT-009 – Grenzen isolierter und geschätzter Indexkosten

- **Absatz-ID und Funktion:** `FO-02-P07`; begrenzt das Screening-Modell und technische Simulationsmechanismen.
- **Belegpflichtige Aussage:** Einzel- oder geschätzte Indexkosten können Interaktionen und reale Setleistung verfehlen; Hint, Query Settings und Hidden Indexes simulieren keine vollständige physische Konfiguration.
- **Beabsichtigte eigene Formulierung:** Einzelprofile reduzieren den Suchraum, bilden aber weder Indexinteraktionen noch alle Kosten einer real materialisierten Konfiguration ab.
- **Evidenztyp:** `external`
- **Status:** `ready`

### SCI-003 – stützende Evidenz

- **Rolle:** stützend
- **Vollbeleg:** Kossmann, Jan; Kastius, Alexander; Schlosser, Rainer (2022): SWIRL: Selection of Workload-aware Indexes using Reinforcement Learning. EDBT 2022, S. 155–168.
- **Originalquelle:** [SCI-003](https://doi.org/10.48786/EDBT.2022.06)
- **Ausgabe / Version:** EDBT 2022
- **Fundstelle:** S. 156, Abschnitt 2.1 `The Index Selection Problem`
- **Originalaussage / quellennaher Auszug:**

  > During index selection, the candidates cannot be considered independent because indexes interact.

- **Eigene Interpretation:** Die Stelle widerspricht einer uneingeschränkten Addition unabhängig gemessener Kandidatenwirkungen.
- **Grenze und Kontext:** Art und Größe der Interaktionen im MongoDB-Referenzworkload bleiben empirisch offen.
- **Reviewentscheidung:** offen

### MDB-008 – begrenzende Evidenz

- **Rolle:** begrenzend
- **Vollbeleg:** MongoDB, Inc. (2026): cursor.hint() (mongosh method). MongoDB Database Manual v8.0.
- **Originalquelle:** [MDB-008](https://www.mongodb.com/docs/v8.0/reference/method/cursor.hint/)
- **Ausgabe / Version:** MongoDB 8.0; abgerufen 2026-08-01
- **Fundstelle:** Abschnitte `Definition` und `Behavior`
- **Originalaussage / quellennaher Auszug:**

  > Call this method on a query to override MongoDB's default index selection and query optimization process.

- **Eigene Interpretation:** `hint()` eignet sich zur Isolation eines Zugriffspfads, zeigt aber gerade nicht die natürliche Plannerwahl.
- **Grenze und Kontext:** Ein gehinteter Einzelpfad ist kein Nachweis der Leistung eines Sets ohne Hint.
- **Reviewentscheidung:** offen

### MDB-009 – begrenzende Evidenz

- **Rolle:** begrenzend
- **Vollbeleg:** MongoDB, Inc. (2026): setQuerySettings (database command). MongoDB Database Manual v8.0.
- **Originalquelle:** [MDB-009](https://www.mongodb.com/docs/v8.0/reference/command/setquerysettings/)
- **Ausgabe / Version:** MongoDB 8.0; abgerufen 2026-08-01
- **Fundstelle:** Abschnitt `Definition`; Hinweis zu `indexHints.allowedIndexes`
- **Originalaussage / quellennaher Auszug:**

  > Index hints in query settings restrict the set of indexes available to the planner, but don't guarantee use.

- **Eigene Interpretation:** Query Settings begrenzen Kandidaten, materialisieren aber keine reale kleinere Indexkonfiguration.
- **Grenze und Kontext:** Diese Funktion ist versions- und clusterabhängig und garantiert keinen Indexzugriff.
- **Reviewentscheidung:** offen

### MDB-010 – begrenzende Evidenz

- **Rolle:** begrenzend
- **Vollbeleg:** MongoDB, Inc. (2026): Hidden Indexes. MongoDB Database Manual.
- **Originalquelle:** [MDB-010](https://www.mongodb.com/docs/manual/core/index-hidden/)
- **Ausgabe / Version:** aktuelle Manual-Seite; abgerufen 2026-08-01
- **Fundstelle:** Einleitung und Abschnitt `Behavior`
- **Originalaussage / quellennaher Auszug:**

  > Hidden indexes are updated upon write operations and continue to consume disk space and memory.

- **Eigene Interpretation:** Hidden Indexes simulieren weder die Speicher- noch die Write-Kosten eines physisch entfernten Indexes.
- **Grenze und Kontext:** Die Stelle quantifiziert die verbleibenden Kosten nicht.
- **Reviewentscheidung:** offen

### SCI-005 – MongoDB-spezifische ergänzende Evidenz aus Altbestand

- **Rolle:** stützend und begrenzend
- **Vollbeleg:** Tao, Dawei; Liu, Enqi; Randeni Kadupitige, Sidath; Cahill, Michael; Fekete, Alan; Röhm, Uwe (2025): First Past the Post: Evaluating Query Optimization in MongoDB. In: Databases Theory and Applications, LNCS 15449, S. 99–113.
- **Originalquelle:** [SCI-005](https://doi.org/10.1007/978-981-96-1242-0_8)
- **Ausgabe / Version:** ADC 2024 Proceedings, veröffentlicht 2025; DOI-Fassung
- **Fundstelle:** S. 99–103 und 106–111
- **Originalaussage / quellennaher Auszug:**

  > [Quellennahe Inhaltsnotiz aus dem geprüften Altbestand] Die Studie beschreibt eine kurze trial-basierte Planwahl und beobachtet Fälle, in denen ein Index Scan gegenüber einem Collection Scan nachteilig ist.

- **Eigene Interpretation:** Die Quelle liefert MongoDB-spezifische Evidenz dafür, dass Indexverfügbarkeit oder Indexnutzung keine automatische Laufzeitüberlegenheit bedeutet.
- **Grenze und Kontext:** MongoDB 7.0.1 und einfache Queries mit zwei Range-Prädikaten; keine quantitative Übertragung auf die Fallstudie oder MongoDB 8.2.11.
- **Reviewentscheidung:** offen

## EXT-010 – Notwendigkeit der physischen Finalvalidierung

- **Absatz-ID und Funktion:** `FO-02-P08`; synthetisiert die Konsequenz aus den Screening-Grenzen.
- **Belegpflichtige Aussage:** Wegen der Grenzen der Kostenschätzung müssen ausgewählte Sets materialisiert und mit natürlicher Plannerwahl überprüft werden.
- **Beabsichtigte eigene Formulierung:** Das Screening erzeugt Kandidaten für eine Empfehlung; belastbare Aussagen zur Setleistung folgen erst aus physisch materialisierten, ungehinteten Finalistenläufen.
- **Evidenztyp:** `external`
- **Status:** `ready`

### SCI-003 – stützende Evidenz

- **Rolle:** stützend
- **Vollbeleg:** Kossmann, Jan; Kastius, Alexander; Schlosser, Rainer (2022): SWIRL: Selection of Workload-aware Indexes using Reinforcement Learning. EDBT 2022, S. 155–168.
- **Originalquelle:** [SCI-003](https://doi.org/10.48786/EDBT.2022.06)
- **Ausgabe / Version:** EDBT 2022
- **Fundstelle:** S. 156, Abschnitte 2.1–2.2
- **Originalaussage / quellennaher Auszug:**

  > [Quellennahe Inhaltsnotiz] Die Publikation weist auf Indexinteraktionen und Abweichungen zwischen geschätzten und realen Ausführungskosten hin.

- **Eigene Interpretation:** Wenn Schätzung reale Ausführung verfehlen kann, ist eine getrennte physische Prüfung der ausgewählten Sets sachlich begründet.
- **Grenze und Kontext:** Die konkrete Finalistenregel ist eine eigene methodische Synthese.
- **Reviewentscheidung:** offen

### SCI-005 – MongoDB-spezifische ergänzende Evidenz

- **Rolle:** stützend
- **Vollbeleg:** Tao, Dawei; Liu, Enqi; Randeni Kadupitige, Sidath; Cahill, Michael; Fekete, Alan; Röhm, Uwe (2025): First Past the Post: Evaluating Query Optimization in MongoDB. In: Databases Theory and Applications, LNCS 15449, S. 99–113.
- **Originalquelle:** [SCI-005](https://doi.org/10.1007/978-981-96-1242-0_8)
- **Ausgabe / Version:** ADC 2024 Proceedings, veröffentlicht 2025; DOI-Fassung
- **Fundstelle:** S. 99–103
- **Originalaussage / quellennaher Auszug:**

  > [Quellennahe Inhaltsnotiz aus dem geprüften Altbestand] Kandidatenpläne werden in einer begrenzten Testphase bewertet; der Gewinner kann anschließend für dieselbe Query Shape im Plan Cache wiederverwendet werden.

- **Eigene Interpretation:** Die natürliche MongoDB-Planwahl ist ein eigenständiger Teil der realen Setausführung und wird durch isolierte Einzelprofile nicht vollständig abgebildet.
- **Grenze und Kontext:** Der beschriebene Mechanismus darf ohne zusätzlichen Versionsbeleg nicht als unverändert für MongoDB 8.2.11 ausgegeben werden.
- **Reviewentscheidung:** offen

### MDB-008, MDB-009 und MDB-010 – technische Abgrenzung

- **Rolle:** begrenzend
- **Vollbeleg:**
  - MongoDB, Inc. (2026): cursor.hint() (mongosh method). MongoDB Database Manual v8.0.
  - MongoDB, Inc. (2026): setQuerySettings (database command). MongoDB Database Manual v8.0.
  - MongoDB, Inc. (2026): Hidden Indexes. MongoDB Database Manual.
- **Originalquelle:**
  - [MDB-008](https://www.mongodb.com/docs/v8.0/reference/method/cursor.hint/)
  - [MDB-009](https://www.mongodb.com/docs/v8.0/reference/command/setquerysettings/)
  - [MDB-010](https://www.mongodb.com/docs/manual/core/index-hidden/)
- **Ausgabe / Version:**
  - MongoDB 8.0; abgerufen 2026-08-01
  - MongoDB 8.0; abgerufen 2026-08-01
  - aktuelle Manual-Seite; abgerufen 2026-08-01
- **Fundstelle:** MDB-008 `Definition/Behavior`; MDB-009 `indexHints.allowedIndexes`; MDB-010 `Behavior`
- **Originalaussage / quellennaher Auszug:**

  > [Quellennahe Inhaltsnotiz] Hint erzwingt einen Einzelpfad, Query Settings begrenzen Plannerkandidaten ohne Nutzungszusage, Hidden Indexes bleiben physisch gepflegt.

- **Eigene Interpretation:** Keiner der drei Mechanismen bildet allein eine physisch isolierte Setkonfiguration mit natürlicher Plannerwahl und realen Ressourcenwirkungen ab.
- **Grenze und Kontext:** Die physische Finalvalidierung ist eine begründete Projektentscheidung und keine wörtliche Verfahrensvorgabe der Dokumentation.
- **Reviewentscheidung:** offen

## EXT-011 – Reproduzierbare Benchmarkbeschreibung

- **Absatz-ID und Funktion:** `ME-03-P01`; ordnet die Phasen des Evaluators methodisch ein.
- **Belegpflichtige Aussage:** Reproduzierbare DBMS-Benchmarks müssen Ablauf, Umgebung, Daten, Queries und Konfiguration offenlegen.
- **Beabsichtigte eigene Formulierung:** Der Evaluator dokumentiert neben Messwerten auch Daten, Queries, Systemumgebung, Konfiguration und Ablauf des Runs.
- **Evidenztyp:** `external`
- **Status:** `ready`

### SCI-004 – stützende Evidenz

- **Rolle:** stützend
- **Vollbeleg:** Raasveldt, Mark; Holanda, Pedro; Gubner, Tim; Mühleisen, Hannes (2018): Fair Benchmarking Considered Difficult: Common Pitfalls in Database Performance Testing. DBTest 2018, S. 1–6.
- **Originalquelle:** [SCI-004](https://doi.org/10.1145/3209950.3209955)
- **Ausgabe / Version:** DBTest 2018
- **Fundstelle:** S. 2–3, Abschnitt 3.1 `Non-Reproducibility`; S. 6, Appendix A
- **Originalaussage / quellennaher Auszug:**

  > [Quellennahe Inhaltsnotiz] Die Checkliste verlangt nachvollziehbare Angaben zu System, Software, Daten, Queries, Konfiguration und Versuchsausführung.

- **Eigene Interpretation:** Die Stelle stützt Run-Manifeste und die Offenlegung aller experimentell relevanten Zustände.
- **Grenze und Kontext:** Die Quelle vergleicht primär DBMS; die Übertragung auf Konfigurationen desselben Systems ist methodisch, nicht wörtlich.
- **Reviewentscheidung:** offen

## EXT-012 – Zustände, Wiederholungen und Ergebnisprüfung

- **Absatz-ID und Funktion:** `ME-03-P05`; begründet Kontrollen der Kandidatenprofilierung.
- **Belegpflichtige Aussage:** Cold-/Hot-Zustände, mehrere Wiederholungen, robuste Kennzahlen und Ergebnisprüfung müssen explizit kontrolliert werden.
- **Beabsichtigte eigene Formulierung:** Die Profilierung hält Zustände und Reihenfolge kontrolliert, wiederholt Messungen, berichtet Medianwerte und prüft die Ergebnisrichtigkeit.
- **Evidenztyp:** `external`
- **Status:** `ready`

### SCI-004 – stützende Evidenz

- **Rolle:** stützend
- **Vollbeleg:** Raasveldt, Mark; Holanda, Pedro; Gubner, Tim; Mühleisen, Hannes (2018): Fair Benchmarking Considered Difficult: Common Pitfalls in Database Performance Testing. DBTest 2018, S. 1–6.
- **Originalquelle:** [SCI-004](https://doi.org/10.1145/3209950.3209955)
- **Ausgabe / Version:** DBTest 2018
- **Fundstelle:** S. 5, Abschnitte 3.5 `Cold vs Hot Runs` und 3.6 `Cold vs Warm Runs`; S. 6, Appendix A
- **Originalaussage / quellennaher Auszug:**

  > We report the median value together with non-parametric, quantile-based 95% confidence intervals.

- **Eigene Interpretation:** Die Quelle stützt getrennte Zustände, wiederholte Läufe, robuste Zusammenfassung und Korrektheitsprüfung.
- **Grenze und Kontext:** Sie rechtfertigt nicht automatisch exakt drei Warmups oder zehn Wiederholungen und verpflichtet die Fallstudie nicht zu demselben Konfidenzverfahren.
- **Reviewentscheidung:** offen

### MDB-004 – begrenzende Evidenz

- **Rolle:** begrenzend
- **Vollbeleg:** MongoDB, Inc. (2026): Explain Results. MongoDB Database Manual v8.0.
- **Originalquelle:** [MDB-004](https://www.mongodb.com/docs/v8.0/reference/explain-results/)
- **Ausgabe / Version:** MongoDB 8.0; abgerufen 2026-08-01
- **Fundstelle:** Feldbeschreibung `executionTimeMillis`
- **Originalaussage / quellennaher Auszug:**

  > [Quellennahe Inhaltsnotiz] Die Explain-Laufzeit ist nicht zwingend repräsentativ für die normale Queryzeit.

- **Eigene Interpretation:** Dies begründet die getrennte Explain-Erhebung statt ihrer Nutzung als alleinige Zeitmessung.
- **Grenze und Kontext:** Es legt keine konkrete Wiederholungszahl fest.
- **Reviewentscheidung:** offen

## EXT-013 – Äquivalente Zustände der Finalistenläufe

- **Absatz-ID und Funktion:** `ME-03-P09`; begründet die Vergleichbarkeit der physischen Finalvalidierung.
- **Belegpflichtige Aussage:** Finalistenläufe benötigen äquivalente Zustände, dokumentierte Konfigurationen und geprüfte Ergebnisse.
- **Beabsichtigte eigene Formulierung:** Baseline und Finalisten werden aus äquivalenten Datenzuständen mit vollständig dokumentierten Indexkonfigurationen und geprüften Ergebnissen ausgeführt.
- **Evidenztyp:** `external`
- **Status:** `ready`

### SCI-004 – stützende Evidenz

- **Rolle:** stützend
- **Vollbeleg:** Raasveldt, Mark; Holanda, Pedro; Gubner, Tim; Mühleisen, Hannes (2018): Fair Benchmarking Considered Difficult: Common Pitfalls in Database Performance Testing. DBTest 2018, S. 1–6.
- **Originalquelle:** [SCI-004](https://doi.org/10.1145/3209950.3209955)
- **Ausgabe / Version:** DBTest 2018
- **Fundstelle:** S. 2–3, Abschnitt 3.1; S. 6, Appendix A
- **Originalaussage / quellennaher Auszug:**

  > [Quellennahe Inhaltsnotiz] Reproduzierbare Vergleiche setzen dokumentierte System- und Versuchsparameter sowie korrekte Ergebnisprüfung voraus.

- **Eigene Interpretation:** Äquivalente Zustände und Manifeste sind notwendige Vergleichsvoraussetzungen.
- **Grenze und Kontext:** Batchgrößen, Resetstrategie und Wiederholungszahlen bleiben projektspezifisch.
- **Reviewentscheidung:** offen

### MDB-010 und MDB-011 – begrenzende Produktevidenz

- **Rolle:** begrenzend
- **Vollbeleg:**
  - MongoDB, Inc. (2026): Hidden Indexes. MongoDB Database Manual.
  - MongoDB, Inc. (2026): Write Operation Performance. MongoDB Database Manual.
- **Originalquelle:**
  - [MDB-010](https://www.mongodb.com/docs/manual/core/index-hidden/)
  - [MDB-011](https://www.mongodb.com/docs/manual/core/write-performance/)
- **Ausgabe / Version:**
  - aktuelle Manual-Seite; abgerufen 2026-08-01
  - aktuelle Manual-Seite; abgerufen 2026-08-01
- **Fundstelle:** MDB-010 Abschnitt `Behavior`; MDB-011 Abschnitt `Indexes`
- **Originalaussage / quellennaher Auszug:**

  > [Quellennahe Inhaltsnotiz] Versteckte Indizes bleiben bei Writes und Ressourcenverbrauch aktiv; zusätzliche Indizes erhöhen den Write-Aufwand.

- **Eigene Interpretation:** Finalisten müssen physisch dieselben tatsächlich vorhandenen Indizes besitzen, wenn Speicher- und Write-Kosten verglichen werden.
- **Grenze und Kontext:** Die konkrete Größe des Effekts muss im Projekt gemessen werden.
- **Reviewentscheidung:** offen

## EXT-014 – Technische Plausibilität der Storefront-Queries

- **Absatz-ID und Funktion:** `IN-01-P01`; plausibilisiert die Fallstudienzugriffe ohne Repräsentativitätsanspruch.
- **Belegpflichtige Aussage:** Produktlisten mit Verfügbarkeits-, Kategorie-, Preis- und Tagfiltern sowie Preissortierung sind als Storefront-Zugriffe technisch plausibel.
- **Beabsichtigte eigene Formulierung:** Die Fallstudienqueries bilden technisch mögliche Produktlisten- und Filterzugriffe ab; ihre Häufigkeiten werden nicht als reale Shopverteilung ausgegeben.
- **Evidenztyp:** `external`
- **Status:** `ready`

### WEB-001 – stützende und begrenzende Evidenz

- **Rolle:** stützend und begrenzend
- **Vollbeleg:** Shopify (2026): products query; ProductFilter; ProductSortKeys. Storefront API 2026-07.
- **Originalquelle:** [WEB-001](https://shopify.dev/docs/api/storefront/latest/queries/products)
- **Ausgabe / Version:** Storefront API 2026-07; abgerufen 2026-08-01
- **Fundstelle:** `products` – ProductConnection-Argumente; `ProductFilter` – Fields; `ProductSortKeys` – `PRICE`
- **Originalaussage / quellennaher Auszug:**

  > Returns a paginated list of the shop's products.

- **Eigene Interpretation:** Die API dokumentiert Produktlisten, einschlägige Filter und Preissortierung als reale Storefront-Funktionen.
- **Grenze und Kontext:** Keine Aussage über Häufigkeit, Repräsentativität oder die interne Datenbankimplementierung von Shopify.
- **Reviewentscheidung:** offen

## EXT-015 – Funktionale Abgrenzung zum Atlas Performance Advisor

- **Absatz-ID und Funktion:** `IN-01-P04`; grenzt den eigenen Evaluator eng von einem benachbarten Werkzeug ab.
- **Belegpflichtige Aussage:** Der Atlas Performance Advisor gruppiert langsame Queries nach Shape, rankt Indexvorschläge nach Impact und dedupliziert Präfixüberlappungen.
- **Beabsichtigte eigene Formulierung:** Der Atlas Performance Advisor ist funktional benachbart; öffentlich dokumentiert sind Shape-Gruppierung, Impact-Ranking und Präfix-Deduplizierung, nicht das hier untersuchte vollständige Mehrziel-Screening mit physischer Finalvalidierung.
- **Evidenztyp:** `external`
- **Status:** `ready`

### MDB-012 – stützende und begrenzende Evidenz

- **Rolle:** stützend und begrenzend
- **Vollbeleg:** MongoDB, Inc. (2026): Review Index Ranking. Atlas Performance Advisor Documentation.
- **Originalquelle:** [MDB-012](https://www.mongodb.com/docs/atlas/performance-advisor/index-ranking/)
- **Ausgabe / Version:** Atlas-Dokumentation; abgerufen 2026-08-01
- **Fundstelle:** `How Performance Advisor Suggests and Ranks Indexes`; `Index De-Duplication`
- **Originalaussage / quellennaher Auszug:**

  > The Performance Advisor orders the indexes that it suggests by their respective Impact.

- **Eigene Interpretation:** Die Dokumentation erlaubt einen positiven Funktionsvergleich der öffentlich beschriebenen Advisor-Leistungen.
- **Grenze und Kontext:** Aus fehlender öffentlicher Dokumentation folgt keine Negativbehauptung über interne oder zukünftige Atlas-Funktionen.
- **Reviewentscheidung:** offen

## INT-001 – Eingabemodell des Evaluators

- **Absatz-ID und Funktion:** `ME-03-P02`; spezifiziert die Reichweite des Softwareartefakts.
- **Belegpflichtige Aussage:** Der Evaluator verarbeitet registrierte ausführbare Query Shapes und validierte Konfigurationen, nicht beliebige Querytexte.
- **Beabsichtigte eigene Formulierung:** Der Prototyp führt registrierte Queryfunktionen mit validierten Parametern aus und ist kein Parser für beliebige MongoDB-Abfragen.
- **Evidenztyp:** `internal`
- **Status:** `planned`
- **Geplante interne Evidenz:** Konfigurationsschema, Query-Registry, Validierungsregeln und Tests nach der G4-Freigabe.
- **Grenze und Kontext:** Ohne Implementierungsabgleich keine Behauptung über unterstützte Felder oder Fehlerfälle.

## INT-002 – Versionierter Referenzworkload

- **Absatz-ID und Funktion:** `ME-03-P03`; belegt Workload, Gewichte und Datenzustände.
- **Belegpflichtige Aussage:** Referenzworkload, Gewichte, Seeds, Skalen und beobachtete Verteilungen sind versioniert und reproduzierbar.
- **Beabsichtigte eigene Formulierung:** Konfiguration und Run-Manifeste halten Queryvarianten, Gewichte, Seeds, Skalierungen und erreichte Verteilungen fest.
- **Evidenztyp:** `internal`
- **Status:** `planned`
- **Geplante interne Evidenz:** Workloadkonfiguration, Generator, Manifest und Verteilungsstatistik.
- **Grenze und Kontext:** Geplante Zielverteilungen nicht mit tatsächlich erreichten Werten verwechseln.

## INT-003 – Baseline und Kandidatenraum

- **Absatz-ID und Funktion:** `ME-03-P04`; fixiert Pflicht- und optionale Indizes.
- **Belegpflichtige Aussage:** `B` enthält `_id` und I9; I1 bis I8 sind optional und ergeben 256 Sets.
- **Beabsichtigte eigene Formulierung:** Mit acht optionalen Kandidaten und der festen Basiskonfiguration entstehen exakt 256 zulässige optionale Kombinationen.
- **Evidenztyp:** `internal`
- **Status:** `planned`
- **Geplante interne Evidenz:** Indexregistry, Eligibility-Matrix, Enumerationstest und Smoke-Tests.
- **Grenze und Kontext:** Die mathematische Anzahl genügt nicht als Nachweis der technischen Koexistenz aller Kandidaten.

## INT-004 – Implementiertes Screening-Kostenmodell

- **Absatz-ID und Funktion:** `ME-03-P06`; operationalisiert Read-, Speicher- und Write-Proxys.
- **Belegpflichtige Aussage:** Normalisierte Read-Kosten sowie additive Speicher- und Write-Proxys werden gemäß G2 berechnet.
- **Beabsichtigte eigene Formulierung:** Die Implementierung normalisiert Queryzeiten gegen `B` und berechnet die ausdrücklich als Näherung gekennzeichneten Set-Proxys deterministisch.
- **Evidenztyp:** `internal`
- **Status:** `planned`
- **Geplante interne Evidenz:** Kostenmodellcode, Formelspezifikation, Unit-Tests und Profilinputs.
- **Grenze und Kontext:** Proxys nicht als tatsächlich gemessene Setkosten ausgeben.

## INT-005 – Vollständige Enumeration und Pareto-Front

- **Absatz-ID und Funktion:** `ME-03-P07`; belegt Suchraum und Dominanzberechnung.
- **Belegpflichtige Aussage:** Der Enumerator erzeugt alle 256 Sets und berechnet je Skalierung eine korrekte Pareto-Front.
- **Beabsichtigte eigene Formulierung:** Alle optionalen Teilmengen werden vollständig erzeugt und mit deterministisch getesteter Dominanzlogik je Skalierung ausgewertet.
- **Evidenztyp:** `internal`
- **Status:** `planned`
- **Geplante interne Evidenz:** Enumerator, Anzahlstest, Dominanz-/Constraint-Tests und Ergebnisartefakte.
- **Grenze und Kontext:** Keine Frontmitglieder vor validierter Ausführung nennen.

## INT-006 – Deterministische Finalistenregel

- **Absatz-ID und Funktion:** `ME-03-P08`; präregistriert Auswahl und Sensitivität.
- **Belegpflichtige Aussage:** Read-Anker und 5-/10-/20-%-Kompromisse folgen deterministischen Regeln und Tie-Breakern.
- **Beabsichtigte eigene Formulierung:** Die Auswahl verwendet das freigegebene 10-%-Band; 5 % und 20 % dienen ausschließlich der rechnerischen Sensitivitätsanalyse.
- **Evidenztyp:** `internal`
- **Status:** `planned`
- **Geplante interne Evidenz:** Auswahlimplementierung, Tie-Breaker-Tests und Finalistenmanifest.
- **Grenze und Kontext:** Sensitivitätssets werden nicht nachträglich als zusätzliche physische Finalisten aufgenommen.

## INT-007 – Physische Finalvalidierung

- **Absatz-ID und Funktion:** `ME-03-P09`; belegt die reale Setmessung.
- **Belegpflichtige Aussage:** Baseline und primäre Finalisten werden physisch isoliert ohne Hint und unter äquivalenten Zuständen gemessen.
- **Beabsichtigte eigene Formulierung:** Jeder Finalist wird mit ausschließlich seinen Indizes, ohne `hint()` und aus einem äquivalenten Ausgangszustand ausgeführt.
- **Evidenztyp:** `internal`
- **Status:** `planned`
- **Geplante interne Evidenz:** Runner, Resetlogik, Manifeste, Umgebungsdaten und Rohmessungen.
- **Grenze und Kontext:** Praktische Ergebnisse erst nach G4 erzeugen und verwenden.

## INT-008 – Eingangskontrolle der Profile

- **Absatz-ID und Funktion:** `EV-04-P01`; begrenzt die Auswertung auf valide Inputs.
- **Belegpflichtige Aussage:** Nur vollständige und valide Profile gehen in die Auswertung ein.
- **Beabsichtigte eigene Formulierung:** Unvollständige, inkonsistente oder fachlich falsche Profile werden vor der Kostenmatrix ausgeschlossen und dokumentiert.
- **Evidenztyp:** `internal`
- **Status:** `planned`
- **Geplante interne Evidenz:** Validierungsberichte und Ausschlussprotokoll.
- **Grenze und Kontext:** Kriterien und Ausschlusszahlen erst aus finalen Artefakten einsetzen.

## INT-009 – Kostenmatrix aus Einzelprofilen

- **Absatz-ID und Funktion:** `EV-04-P02`; zeigt die Screening-Grundlage.
- **Belegpflichtige Aussage:** Einzelprofile liefern die normalisierte Kostenmatrix und Strukturkontrollen.
- **Beabsichtigte eigene Formulierung:** Zeitmediane bilden die Read-Kosten; Explain-Metriken dienen der getrennten Plausibilitätskontrolle.
- **Evidenztyp:** `internal`
- **Status:** `planned`
- **Geplante interne Evidenz:** Profil-CSV, Explain-Rohdaten und Kostenmatrix.
- **Grenze und Kontext:** Einzelkandidat nicht als Setempfehlung ausgeben.

## INT-010 – Skalenspezifische Pareto-Fronten

- **Absatz-ID und Funktion:** `EV-04-P03`; berichtet die nicht dominierten Sets.
- **Belegpflichtige Aussage:** Pareto-Fronten werden für 10k, 100k und 500k getrennt berichtet.
- **Beabsichtigte eigene Formulierung:** Jede Datenskalierung besitzt eine eigene Front; die Skalen werden nicht in einer Zielfunktion vermischt.
- **Evidenztyp:** `internal`
- **Status:** `planned`
- **Geplante interne Evidenz:** Pareto-Ausgaben und Dominanzprüfungen je Skalierung.
- **Grenze und Kontext:** Ergebnisse und Frontgröße nicht vorwegnehmen.

## INT-011 – Stabilität über Skalierungen

- **Absatz-ID und Funktion:** `EV-04-P04`; vergleicht Pareto-Mitgliedschaften.
- **Belegpflichtige Aussage:** Stabile und wechselnde Pareto-Mitgliedschaften werden nur anhand beobachteter Daten erklärt.
- **Beabsichtigte eigene Formulierung:** Wechsel werden auf gemessene Zielwerte und beobachtete Planstrukturen bezogen; unbeobachtete Ursachen bleiben Hypothesen.
- **Evidenztyp:** `internal`
- **Status:** `planned`
- **Geplante interne Evidenz:** Skalenvergleich, Profilwerte und Explain-Strukturen.
- **Grenze und Kontext:** Keine kausale Cache- oder Plannererklärung ohne passende Daten.

## INT-012 – Primäre Finalisten aus dem 10-%-Band

- **Absatz-ID und Funktion:** `EV-04-P05`; weist die vorab festgelegte Auswahl nach.
- **Belegpflichtige Aussage:** Primäre Finalisten entstehen ausschließlich aus der freigegebenen 10-%-Regel.
- **Beabsichtigte eigene Formulierung:** Read-Anker, Speicher- und Write-Kompromiss werden reproduzierbar aus dem primären Band bestimmt; Duplikate führen zu weniger Sets.
- **Evidenztyp:** `internal`
- **Status:** `planned`
- **Geplante interne Evidenz:** Finalistenmanifest und Auswahltests.
- **Grenze und Kontext:** Keine Nachnominierung anhand späterer Messwerte.

## INT-013 – Sensitivitätsbänder

- **Absatz-ID und Funktion:** `EV-04-P06`; prüft die Abhängigkeit von der gesetzten Read-Toleranz.
- **Belegpflichtige Aussage:** 5-%- und 20-%-Bänder dienen ausschließlich der rechnerischen Sensitivitätsanalyse.
- **Beabsichtigte eigene Formulierung:** Alternative Bänder zeigen, ob sich die Screening-Auswahl bei strengerer oder weiterer Read-Toleranz ändert.
- **Evidenztyp:** `internal`
- **Status:** `planned`
- **Geplante interne Evidenz:** Sensitivitätsausgabe und Vergleichstabelle.
- **Grenze und Kontext:** Für nur dort auftretende Sets besteht keine zusätzliche physische Evidenz.

## INT-014 – Tatsächliche Setkosten

- **Absatz-ID und Funktion:** `EV-04-P07`; berichtet reale Read-, Speicher- und Write-Trade-offs.
- **Belegpflichtige Aussage:** Baseline und Finalisten werden anhand tatsächlicher Read-, Speicher- und Write-Messungen verglichen.
- **Beabsichtigte eigene Formulierung:** Für Baseline und primäre Finalisten werden unhinted Laufzeiten, Plannerwahl, gesamte Indexgröße sowie Insert- und Updatekosten gegenübergestellt.
- **Evidenztyp:** `internal`
- **Status:** `planned`
- **Geplante interne Evidenz:** Finalistenläufe, Explain-Daten, Größen- und Write-Manifeste.
- **Grenze und Kontext:** Nur validierte Wiederholungen aus äquivalenten Zuständen verwenden.

## INT-015 – Schätzung gegen Messung

- **Absatz-ID und Funktion:** `EV-04-P08`; quantifiziert die Modellabweichung.
- **Belegpflichtige Aussage:** Die Abweichung zwischen Screening-Schätzung und materialisierter Setleistung wird quantifiziert.
- **Beabsichtigte eigene Formulierung:** Ein vorab definiertes Fehlermaß vergleicht geschätzte und tatsächlich gemessene Zielwerte je Finalist.
- **Evidenztyp:** `internal`
- **Status:** `planned`
- **Geplante interne Evidenz:** Schätz-/Messvergleich, Fehlerdefinition und Plannerbeobachtungen.
- **Grenze und Kontext:** Vorzeichenkonvention und Fehlermaß vor Auswertung fixieren; Ursachen nur bei Beobachtung benennen.

## INT-016 – Bedingte Empfehlung

- **Absatz-ID und Funktion:** `EV-04-P09`; synthetisiert Ergebnis und Geltungsgrenze.
- **Belegpflichtige Aussage:** Die Empfehlung wird auf Workload, Kandidatenraum, Daten, Umgebung und Kostenmodell begrenzt.
- **Beabsichtigte eigene Formulierung:** Empfohlen wird nur ein unter den gesetzten Präferenzen geeignetes Set für die konkrete Fallstudie; andere Kontexte verlangen neue Profile und Validierung.
- **Evidenztyp:** `internal`
- **Status:** `planned`
- **Geplante interne Evidenz:** Synthese aus validierten Fronten, Finalistenmessungen und dokumentierten Limitationen.
- **Grenze und Kontext:** Kein Anspruch auf globale Optimalität oder universelle MongoDB-Empfehlung.
