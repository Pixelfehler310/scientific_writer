# Kapitelplan: method — Vorab festgelegter, reproduzierbarer Setvergleich

## Funktion im Gesamtargument

Das Kapitel operationalisiert die Forschungsfrage vollständig, bevor Ergebnisse erscheinen. Es trennt fachliche Setdefinition, technische Umsetzung und Messprotokoll so, dass der Referenzlauf reproduzierbar und gegen nachträgliche Kandidatenanpassung geschützt ist.

## Teilfrage und erwartetes Ergebnis

Wie werden Datenmodell, Workload, B/L/W1/W2 und Messdimensionen kontrolliert verglichen? Ergebnis ist ein prüfbares Versuchsprotokoll mit klaren internen Artefakten.

## Wortbudget

850 Wörter.

## Voraussetzungen und Übergabe

Voraussetzungen sind die in `foundations` erklärten Mechanismen und ein technisch erfolgreicher Pilot. Das Kapitel übergibt an `evaluation` exakt die zulässigen Beobachtungen, Kennzahlen und Aussagegrenzen.

## Absatzplan

### ME-03-P01 — Untersuchungslogik und Vorabfestlegung

- **Funktion:** Designprinzip und Schutz vor Ergebnisanpassung erklären.
- **Kernaussage:** Vier vollständige Sets werden aus denselben Ausgangsdaten gemessen; Kandidaten und Parameter stehen vor dem Referenzlauf fest.
- **Begründung:** Der Direktvergleich ersetzt Enumeration und iterative Ergebnisoptimierung.
- **Evidenzbedarf:** Benchmarkmethodik und freigegebene Projektentscheidung.
- **Evidenztyp:** `external`
- **Evidenzstatus:** `ready`
- **Beziehung davor:** Übernahme der theoretischen Synthese.
- **Beziehung danach:** Konkretisierung durch Datenmodell und Verteilungen.
- **Zielwörter:** 105.
- **Medium:** Ablaufübersicht optional.
- **Offene Recherche:** Benchmarkfallen präzise zuordnen.

<details>
<summary>Evidenz und menschliche Prüfung</summary>

#### E-ME-03-P01-01 — Mehrere Konfigurationen am repräsentativen Workload prüfen

- **Rolle:** `methodisch`
- **Vollbeleg / Artefakt:** MongoDB Inc. (o. J.): *Indexing Strategies*. MongoDB Database Manual 8.0.
- **Link / Projektpfad:** https://www.mongodb.com/docs/v8.0/applications/indexes/
- **Ausgabe / Version:** MongoDB Database Manual 8.0; abgerufen am 05.08.2026.
- **Fundstelle:** Einleitungsabschnitt.
- **Originalauszug / quellennaher Auszug:**

  > Quellennahe Inhaltsnotiz: Geeignete Indizes hängen von erwarteten Queries, Lese-/Schreibverhältnis und verfügbarem Speicher ab. Die Dokumentation empfiehlt, mehrere Indexkonfigurationen mit repräsentativen Daten zu profilieren.

- **Eigene Zusammenfassung:** Ein workloadbezogener Vergleich vollständiger Sets ist sachgerecht; die Kandidaten werden wegen des begrenzten Erkenntnisanspruchs vorab festgelegt.
- **Grenze und Kontext:** Herstellerdokumentation ist keine allgemeine Benchmarktheorie und beweist nicht, dass B/L/W1/W2 den globalen Suchraum abdecken.
- **Reviewentscheidung:** `übernehmen`
- **Menschliche Prüfung:** verified

#### E-ME-03-P01-02 — Kontrollierter und reproduzierbarer Vergleich

- **Rolle:** `methodisch`
- **Vollbeleg / Artefakt:** Mark Raasveldt, Pedro Holanda, Tim Gubner und Hannes Mühleisen (2018): „Fair Benchmarking Considered Difficult: Common Pitfalls In Database Performance Testing“. In: *DBTest’18*, S. 1–6. https://doi.org/10.1145/3209950.3209955.
- **Link / Projektpfad:** https://hannes.muehleisen.org/publications/DBTEST2018-performance-testing.pdf
- **Ausgabe / Version:** veröffentlichte ACM-Workshopfassung, 2018.
- **Fundstelle:** S. 2–3, Abschnitte 2 und 3.1; Anhang A, S. 6.
- **Originalauszug / quellennaher Auszug:**

  > Quellennahe Inhaltsnotiz: Faire Performancevergleiche sollen einzelne Variablen in kontrollierter Umgebung untersuchen. Reproduzierbarkeit verlangt unter anderem Hardware-/Softwarekonfiguration, DBMS-Version und -Parameter, Code, Daten, Schema und Queries.

  > Echte Quelle: How To Avoid. In order to allow for reproducible experiments,it is important that all configuration parameters are known so theexperiment can be exactly reproduced. This includes seeminglyminor factors such as the operating system the machine is runningon, how the server is installed, the server version, how the serveris setup and the server configuration flags. In addition, when com-paring against the authors’ own algorithm or implementation, thesource code should always be available to allow people to reproducethe experiments

  > Echte Quelle 2:  In many papers the code used by the authors to run the benchmark is keptas closed-source or “on request” from a non-responsive e-mailaddress. In other cases, the data or queries used are not disclosed,or proprietary hardware or back-end systems are used to run theexperiments. Often, “intellectual property” and related reasons arecited as reasons for the inability to allow reproduction.

- **Freigabe langer Auszug:** Simon Sucker; als vom Autor ergänzte Prüfbasis im verifizierten Evidenzblock beibehalten.
- **Eigene Zusammenfassung:** Identische Ausgangsdaten, offengelegte Parameter und vorab unveränderte Kandidaten schützen den Vergleich vor versteckten Konfigurationsunterschieden und nachträglicher Ergebnisanpassung.
- **Grenze und Kontext:** Der Beitrag vergleicht typische DBMS-Benchmarkfallen und schreibt keine konkrete B/L/W1/W2-Struktur vor.
- **Reviewentscheidung:** `unterstützend, nicht perfekt`
- **Menschliche Prüfung:** verified

</details>

### ME-03-P02 — Produktmodell und Verteilungen

- **Funktion:** Datengrundlage reproduzierbar definieren.
- **Kernaussage:** Deterministischer Seed erzeugt 1.000/10.000/100.000 Dokumente mit dokumentierten Kategorie-, Marken-, Tag-, Aktivitäts- und Preisverteilungen sowie benötigten Write-Feldern.
- **Begründung:** Selektivitätsvarianten und Skalen benötigen nachvollziehbare Verteilungen.
- **Evidenzbedarf:** interne Generator- und Seed-Artefakte.
- **Evidenztyp:** `internal`
- **Evidenzstatus:** `ready`
- **Beziehung davor:** Konkretisierung.
- **Beziehung danach:** Folge — Workloadparameter greifen genau auf diese Verteilungen zu.
- **Zielwörter:** 115.
- **Medium:** kompakte Verteilungstabelle.
- **Offene Recherche:** keine; Referenzmessung bleibt eine nachgelagerte empirische Phase.

<details>
<summary>Evidenz und menschliche Prüfung</summary>

#### E-ME-03-P02-01 — Finaler Generatorstand und Pilotverteilungen

- **Rolle:** `interne Evidenz`
- **Vollbeleg / Artefakt:** Benchmark-Repository, Commit `7752eed`: Produktgenerator, Verteilungen, Seed-Zusammenfassung und Full-Run `2026-08-05_210546_567224_full_b24e5dfb`.
- **Link / Projektpfad:** D:/projects/uni/mongodb_indexing/src/mongodb_indexing_lab/data/factory.py; D:/projects/uni/mongodb_indexing/src/mongodb_indexing_lab/data/distributions.py; D:/projects/uni/mongodb_indexing/src/mongodb_indexing_lab/data/seed.py
- **Ausgabe / Version:** Git-Stand `7752eed` vom 05.08.2026; Full-Manifest mit `git_dirty_if_available: false`.
- **Fundstelle:** `ProductFactory.build_product`, `BRAND_WEIGHTS`, `RunProfile.full`, `seed_collection`, `SeedResult` sowie `artifacts/runs/2026-08-05_210546_567224_full_b24e5dfb/manifest.json`.
- **Originalauszug / quellennaher Auszug:**

  > Interne Inhaltsnotiz: Der Generator verwendet einen lokalen `random.Random(seed)`, materialisiert `brand` auf oberster Ebene, gewichtet Northstar mit 0,28 und Summit mit 0,05 und erzeugt `stock`. Das Referenzprofil nutzt 1.000/10.000/100.000 Dokumente. Der 1.000er-Pilot zählte global 293 Northstar- und 61 Summit-Produkte; die vollständigen Q3-Prädikate trafen 41 beziehungsweise 6 Dokumente, Q1-eng 3 und Q1-breit 104.

- **Eigene Zusammenfassung:** Datenmodell, gewichtete Verteilungen, Write-Feld und alle drei Referenzskalen sind implementiert, ausgeführt und im erfolgreichen Full-Manifest dokumentiert.
- **Grenze und Kontext:** Die synthetischen Verteilungen bilden einen kontrollierten Versuchsraum und keine empirisch erhobene Produktionsverteilung.
- **Reviewentscheidung:** `übernehmen`
- **Menschliche Prüfung:** ausstehend

</details>

### ME-03-P03 — Q1–Q4 und Write-Batches

- **Funktion:** Operationen und Varianten vollständig festlegen.
- **Kernaussage:** Sechs Lesevarianten plus Q4 sowie W1/W2a/W2b verwenden feste Filter, Sortierung, `limit: 24`, Produktmengen und gleiche Ziel-IDs. Enge und breite Preisbereiche sowie häufige und seltene Tags beziehungsweise Marken werden als getrennte Varianten festgelegt.
- **Begründung:** Ohne feste Parameter wären Setvergleiche nachträglich steuerbar.
- **Evidenzbedarf:** interne Scenario-Registry und Manifest.
- **Evidenztyp:** `internal`
- **Evidenzstatus:** `ready`
- **Beziehung davor:** Folge.
- **Beziehung danach:** Fortführung — Sets bilden die unabhängige Variable.
- **Zielwörter:** 125.
- **Medium:** Query-Varianten-Tabelle.
- **Offene Recherche:** keine.

<details>
<summary>Evidenz und menschliche Prüfung</summary>

#### E-ME-03-P03-01 — Eingefrorene Read- und Write-Operationen

- **Rolle:** `interne Evidenz`
- **Vollbeleg / Artefakt:** Benchmark-Repository, Commit `7752eed`: Query-Definitionen, Scenario-Registry, Messmodul und Profile.
- **Link / Projektpfad:** D:/projects/uni/mongodb_indexing/src/mongodb_indexing_lab/scenarios/queries.py; D:/projects/uni/mongodb_indexing/src/mongodb_indexing_lab/scenarios/registry.py; D:/projects/uni/mongodb_indexing/src/mongodb_indexing_lab/benchmark/measurement.py; D:/projects/uni/mongodb_indexing/src/mongodb_indexing_lab/benchmark/profiles.py
- **Ausgabe / Version:** Git-Stand `7752eed` vom 05.08.2026.
- **Fundstelle:** `q1_price_range`, `q2_tag`, `q3_brand`, `q4_product_lookup`, `READ_SCENARIO_NAMES`, `WRITE_OPERATIONS` und `measure_write_operation`.
- **Originalauszug / quellennaher Auszug:**

  > Interne Inhaltsnotiz: Die Registry enthält Q1-eng/Q1-breit, Q2-häufig/Q2-selten, Q3-häufig/Q3-selten und Q4 mit den eingefrorenen Filtern, Sortierungen und Limits. Das Messmodul führt W1-Insert, W2a-Stock-Update und W2b-Price-Update auf festen 100er-Batches aus; der Runner erfasst im Vollprofil zehn Wiederholungen.

- **Eigene Zusammenfassung:** Alle sieben Read-Varianten und drei Write-Arten sind als ausführbare, getestete Versuchsobjekte implementiert.
- **Grenze und Kontext:** Der Full-Run setzt die vorab festgelegten 30 Read-Wiederholungen, zehn Write-Wiederholungen und Batchgröße 100 um; andere Parameterkombinationen wurden nicht untersucht.
- **Reviewentscheidung:** `übernehmen`
- **Menschliche Prüfung:** ausstehend

</details>

### ME-03-P04 — Vollständige B/L/W1/W2-Sets

- **Funktion:** Kandidaten und Designkontrast dokumentieren.
- **Kernaussage:** Gemeinsame `_id`-/Unique-Basis und zusätzliche Indexdefinitionen unterscheiden Spezialunterstützung, Wiederverwendung und Partial-/Schlüsselumfang.
- **Begründung:** Nur vollständige, verifizierte Zustände beantworten die Forschungsfrage.
- **Evidenzbedarf:** interne Konfigurationsregistry; theoretische Begründung aus Kapitel 2.
- **Evidenztyp:** `internal`
- **Evidenzstatus:** `ready`
- **Beziehung davor:** Fortführung.
- **Beziehung danach:** Folge — Messablauf muss jeden Zustand fair herstellen.
- **Zielwörter:** 125.
- **Medium:** B/L/W1/W2-Tabelle.
- **Offene Recherche:** keine, sofern G2-Definition unverändert bleibt.

<details>
<summary>Evidenz und menschliche Prüfung</summary>

#### E-ME-03-P04-01 — Vollständige und verifizierte B/L/W1/W2-Sets

- **Rolle:** `interne Evidenz`
- **Vollbeleg / Artefakt:** Benchmark-Repository, Commit `7752eed`: Indexdefinitionen, Manager und MongoDB-Integrationstests.
- **Link / Projektpfad:** D:/projects/uni/mongodb_indexing/src/mongodb_indexing_lab/indexes/registry.py; D:/projects/uni/mongodb_indexing/src/mongodb_indexing_lab/indexes/manager.py
- **Ausgabe / Version:** Git-Stand `7752eed` vom 05.08.2026; 51 Tests einschließlich MongoDB-Integration und End-to-End-Smoke erfolgreich.
- **Fundstelle:** `IndexDefinition`, `_build_registry`, `_apply_strategy` und `_verify_indexes`.
- **Originalauszug / quellennaher Auszug:**

  > Interne Inhaltsnotiz: Die Registry definiert exakt B, L, W1 und W2 einschließlich gemeinsamer Unique-productId-Basis. Der Manager erstellt mehrere Indizes, misst den Gesamtindexspeicher und verifiziert `_id_`, Namen, Schlüsselreihenfolge, Partial-Filter und Unique-Eigenschaft gegen den vollständigen erwarteten Zustand.

- **Eigene Zusammenfassung:** Die vier freigegebenen Setzustände sind vollständig implementiert, in MongoDB geprüft und als Gesamtheit vermessbar.
- **Grenze und Kontext:** Der Pilot bestätigt Installation und unterscheidbare Pläne, nicht die spätere Leistungsrangfolge der Referenzskalen.
- **Reviewentscheidung:** `übernehmen`
- **Menschliche Prüfung:** ausstehend

</details>

### ME-03-P05 — Reset, Rotation und Versionskontrolle

- **Funktion:** Vergleichbarkeit und Umgebungsprotokoll erklären.
- **Kernaussage:** Jeder Block nutzt identischen Datenzustand, deterministische Rotation und dokumentierte MongoDB-/Image-/Git-/Serverparameter.
- **Begründung:** Reihenfolge, Drift und mutable Softwarestände dürfen keine Strategie systematisch bevorzugen.
- **Evidenzbedarf:** Benchmarkmethodik und interne Manifeste.
- **Evidenztyp:** `external`
- **Evidenzstatus:** `ready`
- **Beziehung davor:** Folge.
- **Beziehung danach:** Fortführung — konkrete Messpfade werden getrennt.
- **Zielwörter:** 115.
- **Medium:** kein zusätzliches Medium, falls Ablaufübersicht ME-M01 genutzt wird.
- **Offene Recherche:** keine; Pilotversion und Runtime-Digest sind erfasst.

<details>
<summary>Evidenz und menschliche Prüfung</summary>

#### E-ME-03-P05-01 — Versions- und Umgebungsprotokoll

- **Rolle:** `methodisch`
- **Vollbeleg / Artefakt:** Mark Raasveldt, Pedro Holanda, Tim Gubner und Hannes Mühleisen (2018): „Fair Benchmarking Considered Difficult: Common Pitfalls In Database Performance Testing“. In: *DBTest’18*, S. 1–6. https://doi.org/10.1145/3209950.3209955.
- **Link / Projektpfad:** https://hannes.muehleisen.org/publications/DBTEST2018-performance-testing.pdf
- **Ausgabe / Version:** veröffentlichte ACM-Workshopfassung, 2018.
- **Fundstelle:** Abschnitt 3.1 „Non-Reproducibility“, S. 3; Checkliste in Anhang A, S. 6.
- **Originalauszug / quellennaher Auszug:**

  > Quellennahe Inhaltsnotiz: Für reproduzierbare Experimente sollen auch scheinbar kleine Faktoren wie Betriebssystem, Installationsart, Serverversion, Setup und Konfigurationsparameter offengelegt werden; die Checkliste ergänzt Hardware, Daten, Schema und Queries.

- **Eigene Zusammenfassung:** MongoDB-Patchversion, Image-Digest, Git-Stand und Serverparameter gehören in das Manifest; identische Zustände und deterministische Reihenfolge machen Abweichungen nachvollziehbar.
- **Grenze und Kontext:** Die Quelle fordert kontrollierte Vergleichbarkeit, belegt aber nicht speziell die gewählte Rotationsfunktion; deren Determinismus ist eine eigene Designentscheidung.
- **Reviewentscheidung:** `offen`
- **Menschliche Prüfung:** ausstehend

#### E-ME-03-P05-02 — Setrotation, Zustandskontrolle und vollständige Versionsbindung

- **Rolle:** `interne Evidenz`
- **Vollbeleg / Artefakt:** Benchmark-Repository, Commit `7752eed`: Runner, Profile, Docker Compose und Full-Manifest.
- **Link / Projektpfad:** D:/projects/uni/mongodb_indexing/src/mongodb_indexing_lab/benchmark/runner.py; D:/projects/uni/mongodb_indexing/src/mongodb_indexing_lab/benchmark/profiles.py; D:/projects/uni/mongodb_indexing/docker-compose.yml
- **Ausgabe / Version:** Git-Stand `7752eed` und Full-Run `2026-08-05_210546_567224_full_b24e5dfb`; MongoDB 8.2.11.
- **Fundstelle:** `run_benchmark`, `deterministic_rotation`, `_runtime_image_identity`, `_validate_runtime_image`, `RunProfile.full`, Service `mongo` und `artifacts/runs/2026-08-05_210546_567224_full_b24e5dfb/manifest.json`.
- **Originalauszug / quellennaher Auszug:**

  > Interne Inhaltsnotiz: Für Reads erhält jedes Set eine separat und deterministisch erzeugte Collection; pro Szenario und Wiederholung rotiert die Reihenfolge zyklisch. Vor jeder Write-Wiederholung wird die Write-Collection gelöscht, neu erzeugt und neu indexiert. Compose und Referenzlauf verwenden den Digest `49f1…1250c`; das Manifest erfasst beobachtete Patchversion, Container-Image-ID, Repo-Digest, Serverstartparameter, Git-Stand und Rotationsplan. `full` bricht bei nicht verifizierbarem Digest ab.

- **Eigene Zusammenfassung:** Datenzustände, Reihenfolge und ausführbare Softwareidentität sind im Runner kontrolliert und im Manifest auditierbar.
- **Grenze und Kontext:** Die Runtime-Erfassung setzt beim lokalen Referenzlauf den dokumentierten Compose-Container voraus; für andere MongoDB-Endpunkte wird keine Docker-Identität behauptet.
- **Reviewentscheidung:** `übernehmen`
- **Menschliche Prüfung:** ausstehend

</details>

### ME-03-P06 — Getrennte Latenz-, Explain- und Write-Messung

- **Funktion:** Messprotokoll und Kennzahlen festlegen.
- **Kernaussage:** Normale Queries erhalten drei Warmups und 30 Wiederholungen; Explain wird separat erhoben; Writes werden zehnmal aus identischen Zuständen gemessen.
- **Begründung:** Explain-Zeit darf normale Latenz nicht ersetzen; Write-Kosten brauchen eigene Resetlogik.
- **Evidenzbedarf:** MongoDB-Plan-Cache-/Explain-Dokumentation und interne Runner-Artefakte.
- **Evidenztyp:** `external`
- **Evidenzstatus:** `ready`
- **Beziehung davor:** Fortführung.
- **Beziehung danach:** Folge — Validierung und Auswertung bestimmen zulässige Ergebnisse.
- **Zielwörter:** 145.
- **Medium:** Ablaufübersicht.
- **Offene Recherche:** genaue Client-seitige Zeitgrenze im Artefakt dokumentieren.

<details>
<summary>Evidenz und menschliche Prüfung</summary>

#### E-ME-03-P06-01 — Explain getrennt von normaler Latenz

- **Rolle:** `methodisch`
- **Vollbeleg / Artefakt:** MongoDB Inc. (o. J.): *Explain Results*. MongoDB Database Manual 8.0.
- **Link / Projektpfad:** https://www.mongodb.com/docs/v8.0/reference/explain-results/
- **Ausgabe / Version:** MongoDB Database Manual 8.0; abgerufen am 05.08.2026.
- **Fundstelle:** Einleitender Plan-Cache-Hinweis und Feldbeschreibung `explain.executionStats.executionTimeMillis`.
- **Originalauszug / quellennaher Auszug:**

  > Quellennahe Inhaltsnotiz: `explain` ignoriert den Plan-Cache und verhindert einen neuen Cache-Eintrag. Seine gemeldete Ausführungszeit enthält Planwahl und Serverausführung, nicht jedoch Netzwerkzeit, und ist nicht zwingend repräsentativ für normale Query-Laufzeit.

- **Eigene Zusammenfassung:** Normale Client-Queries liefern die Latenzstichprobe; ein separater Explain-Aufruf liefert Plan und Scanmetriken, ohne beide Messpfade zu vermischen.
- **Grenze und Kontext:** Die Quelle bestimmt weder drei Warmups noch 30 Wiederholungen; diese Zahlen sind vorab festgelegte Projektparameter.
- **Reviewentscheidung:** `offen`
- **Menschliche Prüfung:** ausstehend

#### E-ME-03-P06-02 — Warm-/Hot-Zustand und Wiederholungen

- **Rolle:** `methodisch`
- **Vollbeleg / Artefakt:** Mark Raasveldt, Pedro Holanda, Tim Gubner und Hannes Mühleisen (2018): „Fair Benchmarking Considered Difficult: Common Pitfalls In Database Performance Testing“. In: *DBTest’18*, S. 1–6. https://doi.org/10.1145/3209950.3209955.
- **Link / Projektpfad:** https://hannes.muehleisen.org/publications/DBTEST2018-performance-testing.pdf
- **Ausgabe / Version:** veröffentlichte ACM-Workshopfassung, 2018.
- **Fundstelle:** Abschnitt 3.6 „Cold vs Warm Runs“, S. 5; Checkliste Anhang A, S. 6.
- **Originalauszug / quellennaher Auszug:**

  > Quellennahe Inhaltsnotiz: Cold- und Hot-Runs können deutlich verschiedene Zeiten liefern und sollen getrennt behandelt werden. Für Hot-Runs empfiehlt die Checkliste, Initialläufe auszuschließen; mehrere Läufe und robuste Kennzahlen reduzieren die Wirkung von Störungen.

- **Eigene Zusammenfassung:** Deklarierte Warmups und wiederholte Messungen definieren bewusst einen warmen Vergleichszustand; Median und Streuung sind geeigneter als ein einzelner Lauf.
- **Grenze und Kontext:** Der Beitrag liefert keine universelle optimale Wiederholungszahl und verlangt für andere Fragestellungen gegebenenfalls getrennte Cold-Run-Messungen.
- **Reviewentscheidung:** `offen`
- **Menschliche Prüfung:** ausstehend

#### E-ME-03-P06-03 — Getrennte Read-, Explain- und Write-Messpfade

- **Rolle:** `interne Evidenz`
- **Vollbeleg / Artefakt:** Benchmark-Repository, Commit `7752eed`: Benchmark-Runner, Messmodul, Profile und Metrikartefakte.
- **Link / Projektpfad:** D:/projects/uni/mongodb_indexing/src/mongodb_indexing_lab/benchmark/runner.py; D:/projects/uni/mongodb_indexing/src/mongodb_indexing_lab/benchmark/measurement.py; D:/projects/uni/mongodb_indexing/src/mongodb_indexing_lab/benchmark/profiles.py; D:/projects/uni/mongodb_indexing/src/mongodb_indexing_lab/benchmark/metrics.py
- **Ausgabe / Version:** Git-Stand `7752eed` vom 05.08.2026; Full-Run `2026-08-05_210546_567224_full_b24e5dfb` erfolgreich.
- **Fundstelle:** `measure_query_latency`, `measure_write_operation`, Warmup-/Explain-/Repetition-Schleifen in `run_benchmark`, `RunProfile.full`, `results.csv` und `write_results.csv`.
- **Originalauszug / quellennaher Auszug:**

  > Interne Inhaltsnotiz: `measure_query_latency` umfasst mit `perf_counter` den normalen `find`-Aufruf einschließlich Cursorverbrauch. Nach drei Warmups erzeugt der Runner 30 solche Wiederholungen; pro Szenario/Set wird Explain separat einmal erhoben. W1/W2a/W2b werden im Vollprofil je zehnmal aus frisch gesäten und indexierten Ausgangszuständen gemessen; Reset und Indexaufbau liegen außerhalb des Write-Zeitfensters.

- **Eigene Zusammenfassung:** Clientlatenz, Explain-Struktur und Write-Aufwand sind technisch getrennte Messpfade mit den vorab festgelegten Wiederholungszahlen.
- **Grenze und Kontext:** Der Full-Run erzeugt die vorgesehene Stichprobe für genau die drei festgelegten Skalen und den warmen Einzelclient-Zustand.
- **Reviewentscheidung:** `übernehmen`
- **Menschliche Prüfung:** ausstehend

</details>

### ME-03-P07 — Korrektheit und Auswertungsregel

- **Funktion:** Ergebnisvalidierung und entscheidungslogische Grenze definieren.
- **Kernaussage:** Ergebnissignaturen müssen übereinstimmen; ausgewertet werden Median/IQR, Strukturmetriken, Speicher und Write-Kosten ohne Gesamtscore, mit bedingten Prioritätsempfehlungen.
- **Begründung:** Eine semantisch abweichende Query darf nicht als schnellere Strategie gelten.
- **Evidenzbedarf:** interne Validierungs-/Analyseartefakte; eigene vorab freigegebene Entscheidungslogik.
- **Evidenztyp:** `internal`
- **Evidenzstatus:** `ready`
- **Beziehung davor:** Folge.
- **Beziehung danach:** Übergabe an Ergebnisdarstellung.
- **Zielwörter:** 120.
- **Medium:** keines.
- **Offene Recherche:** Dominanzbegriff konsistent und ohne Pareto-Screening-Altlogik verwenden.

<details>
<summary>Evidenz und menschliche Prüfung</summary>

#### E-ME-03-P07-01 — Ergebnissignatur und setbezogene Auswertung

- **Rolle:** `interne Evidenz`
- **Vollbeleg / Artefakt:** Benchmark-Repository, Commit `7752eed`: Ergebnisvalidierung, Runner, Metriken und Analyse.
- **Link / Projektpfad:** D:/projects/uni/mongodb_indexing/src/mongodb_indexing_lab/benchmark/validation.py; D:/projects/uni/mongodb_indexing/src/mongodb_indexing_lab/benchmark/runner.py; D:/projects/uni/mongodb_indexing/src/mongodb_indexing_lab/benchmark/analysis.py
- **Ausgabe / Version:** Git-Stand `7752eed` vom 05.08.2026; Full-Artefakte mit 84/84 Read-Zellen und 36/36 Write-Zellen, jeweils vollständig wiederholt.
- **Fundstelle:** `calculate_result_set_signature`, Signaturvergleich in `run_benchmark`, `build_summary_rows`, `build_write_summary_rows`, `assess_index_sets` und `_dominance_pairs`.
- **Originalauszug / quellennaher Auszug:**

  > Interne Inhaltsnotiz: Der Runner vergleicht pro Skala und Szenario Anzahl und SHA-256-Signatur stabiler `productId`-Mengen über alle vier separat gesäten Set-Collections. Die Analyse bildet Median und IQR pro Read- und Write-Gruppe, nennt Struktur- und Speicherführer, prüft mehrdimensionale Pareto-Dominanz und erzeugt getrennte Empfehlungen für Lese- sowie Ressourcen-/Write-Priorität ohne Gesamtscore.

- **Eigene Zusammenfassung:** Semantische Vergleichbarkeit und die vorab festgelegte mehrdimensionale Entscheidungslogik sind implementiert und als CSV/JSON reproduzierbar.
- **Grenze und Kontext:** Eine übereinstimmende Produkt-ID-Menge belegt fachliche Vergleichbarkeit, aber nicht externe Validität oder globale Optimalität außerhalb der vier Kandidaten.
- **Reviewentscheidung:** `übernehmen`
- **Menschliche Prüfung:** ausstehend

</details>

## Medienplan

| Medium-ID | Typ | Aussagefunktion | Herkunft/Erzeugung | Beschriftung | Einführung und Interpretation |
| --- | --- | --- | --- | --- | --- |
| ME-M01 | Ablaufdiagramm | Reset, Setinstallation, Query-Warmup/Latenz, separates Explain und Writes in korrekter Reihenfolge zeigen | eigene Darstellung aus finalem Runner | „Kontrollierter Ablauf eines Messblocks“ | In ME-03-P05 einführen und in ME-03-P06 interpretieren |
| ME-M02 | Tabelle | Query-Varianten und Sets platzsparend vollständig festhalten | Registry/Manifest | „Vorab festgelegter Workload und Indexsets“ | In ME-03-P03 und ME-03-P04 zweigeteilt erläutern |

## Offene Entscheidungen

- Medien ME-M01/ME-M02 nur verwenden, wenn sie zusammen weniger Raum als gleichwertige Prosa benötigen.
