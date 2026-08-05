# Bestandsprüfung und Wiederverwendung

Stand: 02.08.2026. Geprüft wurden die bisherigen Theorieentwürfe unter `manuscript/02_Theoretischer_Hintergrund`, die alte Belegmatrix und das alte Quellenregister unter `manuscript/08_Quellen_und_Notizen/Recherche` sowie der Pilot-Snapshot vom 24.07.2026.

Maßgeblich sind die durch G2 und G3 freigegebene neue Forschungsfrage, Gliederung und Absatzplanung. Wiederverwendung bedeutet deshalb die Übernahme geprüfter Sachkerne und Fundstellen, nicht das unveränderte Kopieren alter Prosa.

## Inhaltliche Entscheidung

| Altbestand | Neue Zielstelle | Entscheidung | Begründung und Grenze |
| --- | --- | --- | --- |
| Dokumentdatenbanken und Aggregatmodell aus altem Abschnitt 2.1 | keine eigene Zielstelle | nicht aktiv übernehmen | Die neue Grundlagenstruktur setzt beim Workload- und Indexsetproblem an; eine allgemeine NoSQL-Einführung verbraucht Budget ohne direkte Argumentfunktion. |
| B-Tree-Aufbau und Balance aus altem Abschnitt 2.2 | keine eigene Zielstelle | nicht aktiv übernehmen | Ausführliche B-Tree-Theorie ist in `foundations.md` ausdrücklich ausgeschlossen. Nur der allgemeine Begriff eines Indexzugriffspfads bleibt als Hintergrundwissen. |
| Query Shape, Planner und Explain-Metriken aus altem Abschnitt 2.2 | `FO-02-P01`, `FO-02-P02` | Sachkern wiederverwenden, Prosa neu formulieren | Fundstellen zu Query Shapes und Explain sind brauchbar. Die alte unbeschränkte Übertragung der FPTP-Details auf MongoDB 8.2.11 wird nicht übernommen. |
| `hint()` als Isolationsinstrument | `FO-02-P07`, `ME-03-P05` | wiederverwenden | Die kontrollierte Einzelprofilierung nutzt Hint; natürliche Plannerwahl und Setleistung werden daraus nicht abgeleitet. |
| Compound-Präfixe und ESR aus altem Abschnitt 2.3 | `FO-02-P03` | Sachkern und Fundstellen wiederverwenden | Feldreihenfolge und Präfixnutzung begründen Kandidaten, beweisen aber weder Sieger noch Redundanz. |
| Partial-, Multikey- und Unique-Eigenschaften | `FO-02-P04`, `ME-03-P04` | Sachkern und Fundstellen wiederverwenden | Verwendbar für Eligibility und die Pflichtrolle von I9; technische Koexistenz bleibt durch Smoke-Tests zu belegen. |
| Selektivitätspassage | `ME-03-P03`, später `EV-04-P02` | nur als begrenzte Interpretationsnotiz | Häufige und seltene Werte sind kontrollierte Versuchsfaktoren; die alte allgemeine Theoriepassage wird nicht als eigener Grundlagenabsatz übernommen. |
| Alte Schlussfolgerung „geeignete gemeinsame Konfiguration“ | `FO-02-P05` bis `FO-02-P08` | vollständig neu aufbauen | Das neue Argument benötigt Index Selection Problem, Constraints, Pareto-Dominanz, Screening-Grenzen und physische Finalvalidierung. Diese Struktur fehlt im Alttext. |
| Szenarien A bis D und Diagramme des Runs vom 24.07.2026 | keine aktive Evidenz | nur Pilot/Entwicklungswissen | Der Lauf untersucht isolierte Strategien und beantwortet die neue Indexset-Forschungsfrage nicht. Zahlen, Ergebnisinterpretation und Grafiken werden nicht in Kapitel 4 übernommen. |
| Benchmark-Pipeline, deterministische Datenerzeugung und Explain-Parsing | `ME-03-P01` bis `ME-03-P09` | technische Basis wiederverwenden, nach G4 neu belegen | Architekturideen und Code können erweitert werden; Aussagen müssen anschließend an die neue Registry, Manifeste, Tests und Runs gebunden werden. |

## Quellenentscheidung

| Alte ID | Neue ID oder Rolle | Entscheidung |
| --- | --- | --- |
| S23 – ESR Guideline | MDB-002 | direkt weitergeführt; Fundstellen in EXT-005 |
| S24 – Multikey Indexes | MDB-006 | direkt weitergeführt; Fundstellen in EXT-006 |
| S25 – Partial Indexes | MDB-005 | direkt weitergeführt; Fundstellen in EXT-006 |
| S26 – Explain Results | MDB-004 | direkt weitergeführt; Fundstellen in EXT-004 und EXT-012 |
| S28 – Tao et al. | SCI-005 | neu in den Skillzustand übernommen; ergänzt EXT-009 und EXT-010 mit MongoDB-spezifischer Evidenz |
| S22 – MongoDB Indexes | MDB-011 nur für Write-Trade-off; ansonsten Reserve | Allgemeine B-Tree-Aussage entfällt; Write-Kosten werden mit der direkteren Write-Performance-Seite belegt. |
| S27 – PyMongo `Database.command` | Reserve | Ein konkreter API-Aufruf kann später am Repository belegt werden; für die neue Argumentationslinie ist kein eigener externer Claim nötig. |
| S01–S05 – B-Tree/Operatorgrundlagen | Reserve | Fachlich brauchbar, aber im freigegebenen Grundlagenkapitel nicht mehr benötigt. |
| S06–S09 und S20 – allgemeine Optimizer-/Tuningliteratur | durch SCI-001–SCI-003 und SCI-005 ersetzt | Die neuen Quellen passen direkter zum Index Selection Problem, zu Mehrzielauswahl, Interaktionen und MongoDB-Planwahl. |
| S10–S12 – NoSQL/Dokumentdatenbanken | Reserve | Allgemeine Dokumentdatenbank-Einführung entfällt aus dem neuen Argument. |
| S13–S18 und S21 – allgemeine Benchmarkmethodik | Reserve hinter SCI-004 | SCI-004 ist datenbankspezifisch und deckt die aktuell geplanten Methodenclaims direkter ab; exakt drei Warmups und zehn beziehungsweise fünf Wiederholungen bleiben projektspezifisch. |
| S19 – Softwarearchitektur | Reserve | Projektstruktur wird primär durch Code, Konfigurationen, Tests und Manifeste belegt. |

## Konsequenz für das Schreiben

- Finaler Text entsteht ausschließlich aus den freigegebenen Absatzplänen, diesem Claim-Evidence-Ledger und späterer interner Evidenz.
- Alte Fließtextformulierungen werden nicht als Entwurfsgrundlage kopiert.
- Die Quellensteckbriefe enthalten keine Originalauszüge und keine claimbezogene Interpretation mehr.
- Kein externer Eintrag gilt als `verified`, bevor ein Mensch Claim, Originalfundstelle, Kontext, Paraphrase und Attribution in `source-audit.csv` geprüft hat.
