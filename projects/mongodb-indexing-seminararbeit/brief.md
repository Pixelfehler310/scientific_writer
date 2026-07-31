# Forschungsauftrag

Status: Durch G1 freigegeben.

## Thema

Workload-basierte Entwicklung und empirische Validierung einer gemeinsamen MongoDB-Indexkonfiguration für ausgewählte typische Lesezugriffe auf den Produktkatalog eines E-Commerce-Shops.

## Problemstellung

Ein E-Commerce-Produktkatalog muss unterschiedliche Lesezugriffe unterstützen. Dazu gehören insbesondere gefilterte und sortierte Produktlisten, die Navigation über Tags oder vergleichbare Arraymerkmale sowie der punktuelle Abruf einzelner Produkte. Diese Query Shapes stellen unterschiedliche Anforderungen an Feldwahl, Feldreihenfolge, Selektivität und Gültigkeitsbereich eines Indexes.

MongoDB-Indizes können den strukturellen Abfrageaufwand und die Ausführungszeit deutlich reduzieren. Ein für eine einzelne Query geeigneter Index ist jedoch nicht automatisch Bestandteil einer effizienten Gesamtkonfiguration. Mehrere query-spezifische Indizes können sich über Compound-Präfixe überschneiden, nur unter bestimmten Filterbedingungen nutzbar sein oder zusätzlichen Speicher- und Wartungsaufwand erzeugen. Deshalb genügt es nicht, Indexarten isoliert vorzustellen oder für jede Abfrage unabhängig einen Index zu empfehlen.

Die Arbeit untersucht stattdessen einen begrenzten Referenzworkload auf einer gemeinsamen Produkt-Collection. Für jede Query Shape werden fachlich plausible Indexkandidaten aus ihrer Filter-, Sortier-, Array- oder Lookup-Struktur abgeleitet und empirisch verglichen. Anschließend werden die bestgeeigneten beziehungsweise konditional geeigneten Kandidaten unter Berücksichtigung von Redundanzen zu einer gemeinsamen Ausgangskonfiguration zusammengeführt und erneut als Gesamtheit validiert.

## Forschungsfrage

**Hauptforschungsfrage**

Wie lässt sich für den Produktkatalog eines E-Commerce-Shops aus ausgewählten typischen Query Shapes unter Berücksichtigung von Selektivität und Datenmenge eine geeignete gemeinsame MongoDB-Indexkonfiguration ableiten und empirisch validieren?

**Teilfragen**

1. Welche Query Shapes bilden einen begrenzten und fachlich plausiblen Referenzworkload für typische Lesezugriffe auf einen E-Commerce-Produktkatalog?
2. Welche Indexkandidaten lassen sich aus den Filter-, Sortier-, Array- und Lookup-Anforderungen der einzelnen Query Shapes ableiten?
3. Wie unterscheiden sich die Indexkandidaten bei variierender Datenmenge und – soweit relevant – Selektivität hinsichtlich strukturellem Abfrageaufwand, Ausführungszeit und Indexspeicherbedarf?
4. Welche Kandidaten können unter Berücksichtigung von Compound-Präfixen, Redundanzen und bedingter Nutzbarkeit zu einer gemeinsamen Ausgangskonfiguration kombiniert werden, und wie bewährt sich diese gegenüber einer Baseline hinsichtlich Leseeffizienz, Indexspeicherbedarf und ausgewähltem indexbedingtem Schreibaufwand?

## Ziel und erwarteter Beitrag

Ziel ist die nachvollziehbare Entwicklung einer initialen MongoDB-Indexkonfiguration für die in dieser Arbeit definierte Produktdokumentstruktur und den dazugehörigen Referenzworkload. Die Arbeit soll nicht lediglich demonstrieren, dass verschiedene Indexarten in ihren vorgesehenen Einsatzfällen funktionieren. Sie soll zeigen, wie aus konkreten Query Shapes plausible Kandidaten entstehen, wie diese anhand einheitlicher Kriterien verglichen werden und wie aus den Einzelentscheidungen eine möglichst überschneidungsarme gemeinsame Konfiguration abgeleitet wird.

Als Ergebnis werden erwartet:

- ein begründeter Referenzworkload aus wenigen ausgewählten Katalogabfragen;
- eine dokumentierte Index-Design-Phase mit query-spezifischen Kandidaten;
- reproduzierbare Kandidatenvergleiche über mehrere Datenmengen und relevante Selektivitäten;
- eine konkrete gemeinsame Indexaufstellung einschließlich möglicher Präfixabdeckung, bedingter Kandidaten und verworfener Redundanzen;
- eine abschließende Validierung aller Query Shapes bei gleichzeitig installierter Ausgangskonfiguration;
- eine begrenzte quantitative Gegenprüfung des Schreibaufwands der finalen Konfiguration gegenüber der Baseline;
- ein Entscheidungsleitfaden, mit dem Leserinnen und Leser die Empfehlung auf einen ähnlichen, aber nicht identischen Workload übertragen und dort erneut prüfen können.

Die Indexaufstellung ist keine universelle Standardkonfiguration für beliebige Shops. Sie ist eine empirisch validierte Ausgangskonfiguration für die festgelegte Referenzdokumentstruktur und vergleichbare Lesezugriffe.

## Untersuchungsgegenstand

Untersucht wird die Collection `products` eines deterministisch erzeugten, synthetischen E-Commerce-Produktkatalogs in MongoDB Community 8.2.x. Die bestehende Dokumentstruktur mit unter anderem `productId`, `category`, `price`, `tags` und `isActive` bleibt grundsätzlich erhalten. Die Skalierungsstufen von 10.000, 100.000 und 500.000 Dokumenten dienen der kontrollierten Untersuchung wachsenden Datenvolumens und werden nicht als typische Größen jedes E-Commerce-Shops ausgegeben.

Der vorläufige Referenzworkload wird auf höchstens drei Query Shapes begrenzt:

1. **Produktlistenabfrage:** Produkte einer Kategorie und mit aktivem Status filtern, nach Preis sortieren und als begrenzte beziehungsweise paginierte Ergebnisliste abrufen. Sie bildet den Schwerpunkt der Kandidatenvergleiche und untersucht insbesondere Filterabdeckung, Sortierunterstützung, Feldreihenfolge und partielle Begrenzung.
2. **Tag-basierte Filterung:** Produkte anhand eines Arraymerkmals abrufen. Eine häufige und eine selektivere Tag-Ausprägung dienen dazu, die Nutzbarkeit eines Multikey-Index unter verschiedenen Trefferanteilen zu beurteilen.
3. **Produktdetailabruf:** Ein einzelnes Produkt über eine fachliche Produkt-ID abrufen und gegebenenfalls seinen Aktivstatus berücksichtigen. Dabei wird auch geprüft, ob ein separater Index erforderlich ist, als Unique Index ausgeführt werden sollte oder die fachliche ID sinnvoll durch das vorhandene `_id`-Konzept abgedeckt werden kann.

Die exakten Querydefinitionen, Limits, Sortierungen und Kandidaten werden in G2 nach Abgleich mit dokumentierten Storefront-Zugriffen und dem Referenzschema festgelegt. Es werden je Query nur fachlich plausible Kandidaten verglichen. Die Arbeit erzwingt keine identische Kandidatenzahl und keine künstliche Beteiligung aller MongoDB-Indexarten.

## Methode

Die Untersuchung folgt einem mehrstufigen kontrollierten Benchmarkdesign.

**Stufe 1 – Baseline:** Auf der Produkt-Collection ist nur der standardmäßige `_id`-Index vorhanden. Alle Query Shapes werden mit identischen Parametern ausgeführt. Die Baseline liefert den Vergleichswert für strukturellen Aufwand und Laufzeit; ein Collection Scan kann für Kontrollmessungen zusätzlich explizit über `$natural` erzwungen werden.

**Stufe 2 – Kandidatenpool und kontrollierter Vergleich:** Für jede Query Shape werden vor Kenntnis der neuen Messergebnisse fachlich plausible Kandidaten aus dokumentierten MongoDB-Entwurfsregeln und den konkreten Filter-, Sortier- und Datenanforderungen hergeleitet. Alle Kandidaten werden gemeinsam installiert. Jeder für eine Query logisch geeignete Kandidat wird anschließend über `hint()` erzwungen und nach einheitlichen Warmups mehrfach gemessen. Partial Indizes werden nur für Query Shapes erzwungen, deren Prädikat die partielle Filterbedingung hinreichend einschließt. Ergebnisanzahl beziehungsweise Ergebnisidentität werden zwischen den Zugriffswegen validiert. Eine deterministisch rotierte Ausführungsreihenfolge begrenzt systematische Cache- und Reihenfolgeeffekte.

**Stufe 3 – Natürliche Planner-Auswahl:** Dieselben Query Shapes werden bei vollständig installiertem Kandidatenpool ohne `hint()` ausgeführt. `explain("executionStats")` dokumentiert, welchen Zugriffsplan MongoDB unter den verfügbaren Kandidaten auswählt. Der kontrollierte Hint-Vergleich und die natürliche Planner-Auswahl werden getrennt berichtet: Die Auswahl durch den Planner beweist nicht automatisch, dass der gewählte Index in wiederholten Laufzeitmessungen jeden anderen Kandidaten übertrifft.

**Stufe 4 – Reduktion und gemeinsame Indexkonfiguration:** Strukturell unterlegene, ungenutzte, durch Compound-Präfixe hinreichend abgedeckte oder gemessen nicht gerechtfertigte Kandidaten werden entfernt. Falls kein Kandidat alle Kriterien dominiert, wird die Auswahl als workloadabhängige Abwägung dokumentiert. Nur die empfohlene gemeinsame Konfiguration bleibt installiert.

**Stufe 5 – Abschlussvalidierung und Schreibkostenprüfung:** Alle drei Query Shapes werden mit der reduzierten Konfiguration ohne `hint()` erneut gegen die Baseline validiert. Zusätzlich wird bei einer vorab festgelegten repräsentativen Skalierungsstufe der Aufwand ausgewählter Schreiboperationen zwischen Baseline und finaler Konfiguration verglichen. Vorgesehen sind das Einfügen eines standardisierten Produktbatches und eine getrennt ausgeführte Aktualisierung eines indexrelevanten Feldes. Preisänderung und Wechsel des Aktivstatus werden nicht in derselben Messoperation vermischt; die endgültige Update-Variante wird in G2 festgelegt.

Der synthetische Datensatz wird je Skalierungsstufe mit festem Zufallsseed erzeugt. Kandidatenpool, Indexzustände, Queryparameter, Plan Cache, Warmups, Wiederholungen und Ausführungsreihenfolge werden kontrolliert; Rohdaten und Manifeste werden archiviert. Die konkrete Zahl der Warmups und Wiederholungen wird vor dem finalen Lauf festgeschrieben und für alle Kandidaten gleich angewendet. Für Schreibmessungen werden identische Batchgrößen, Ausgangszustände und Write-Concern-Einstellungen verwendet.

Die Bewertung erfolgt mehrdimensional:

- primär über `totalDocsExamined`, `totalKeysExamined`, `nReturned`, Planstufen und erforderliche Sortierstufen;
- ergänzend über mediane Laufzeiten wiederholter, vollständig ausgeführter Queries; `executionTimeMillis` aus `explain()` wird nur gemeinsam mit diesen und den Strukturmetriken interpretiert;
- für einzelne und kombinierte Konfigurationen über Indexspeicherbedarf;
- für die finale Konfiguration ergänzend über die mediane Dauer beziehungsweise den Durchsatz der ausgewählten Schreiboperationen gegenüber der Baseline;
- qualitativ über Indexnutzbarkeit, Präfixabdeckung und Redundanz.

Ein Index wird nicht allein aufgrund des niedrigsten einzelnen Laufzeitwertes ausgewählt. Struktureller Aufwand, Stabilität über Skalierungsstufen, Speicherbedarf und Gültigkeit für den Referenzworkload werden gemeinsam interpretiert. Falls kein Kandidat in allen Kriterien dominiert, wird eine bedingte Empfehlung formuliert.

## Scope

- eine gemeinsame MongoDB-Collection mit einer festgelegten Referenzstruktur für Produktdokumente;
- höchstens drei ausgewählte und extern plausibilisierte Query Shapes eines Produktkatalogs;
- query-spezifische Herleitung und empirischer Vergleich plausibler Indexkandidaten;
- gemeinsamer Kandidatenpool mit wiederholten, per `hint()` kontrollierten Vergleichen und zusätzlicher Beobachtung der natürlichen Planner-Auswahl;
- 10.000, 100.000 und 500.000 synthetische Produktdokumente als experimentelle Skalierungsstufen;
- Variation der Selektivität nur dort, wo sie die jeweilige Query- und Indexwirkung fachlich beeinflusst;
- Bewertung der Leseeffizienz anhand von Explain-Metriken und Laufzeitmedianen;
- Bewertung des Speicherbedarfs einzelner Kandidaten und der gemeinsamen Konfiguration;
- abschließende Validierung des gesamten Referenzworkloads bei gleichzeitig installierten empfohlenen Indizes;
- begrenzter Vergleich ausgewählter indexrelevanter Schreiboperationen zwischen Baseline und finaler Konfiguration an einer repräsentativen Skalierungsstufe;
- Ableitung einer konkreten Ausgangskonfiguration und eines übertragbaren Entscheidungsprozesses für ähnliche Produktkataloge.

## Explizite Ausschlüsse

- Anspruch auf Vollständigkeit oder Repräsentativität für alle E-Commerce-Shops;
- vollständige E-Commerce-Anwendungen einschließlich Bestellungen, Kundenkonten, Warenkörben und Zahlungsprozessen;
- vollständige oder repräsentative Schreibworkloads; abgesehen von der begrenzten Insert- und Update-Gegenprüfung werden Schreib-, Lösch- und Indexwartungskosten nicht quantitativ untersucht;
- parallele Zugriffe, Lasttests mit konkurrierenden Clients und Netzwerklatenzen;
- Replikation, Sharding und Mehrknotencluster;
- Vergleiche mit relationalen oder anderen Datenbanksystemen;
- Volltextsuche, Relevanzranking, MongoDB Search und Vector Search;
- Geospatial-, Hashed-, Wildcard- und Clustered-Index-Szenarien;
- eine universelle oder ohne erneute Messung direkt übernehmbare Produktionskonfiguration.

## Zielgruppe

Die Arbeit richtet sich an Studierende sowie Entwicklerinnen und Entwickler mit grundlegenden MongoDB-Kenntnissen, die für einen Produktkatalog mit vergleichbarer Dokumentstruktur und vergleichbaren Lesezugriffen eine erste Indexkonfiguration entwerfen oder überprüfen möchten. KI-gestützte Anwendungsentwicklung bleibt lediglich eine kurze aktuelle Zusatzmotivation und begründet weder die Query-Auswahl noch eine behauptete Datenbankqualität.

## Praktische Artefakte

- containerisierte MongoDB-Testumgebung;
- deterministischer Generator für synthetische Produktdaten und kontrollierte Selektivitäten;
- versionierte Definition des Referenzworkloads;
- Registry fachlich begründeter Indexkandidaten;
- reproduzierbare Baseline-, Hint-, Planner- und Gesamtkonfigurationsläufe;
- begrenzter Insert-/Update-Vergleich für Baseline und finale Konfiguration;
- Manifeste, rohe Explain-Ausgaben, normalisierte Messdaten und Indexgrößenbeobachtungen;
- szenariobezogene Vergleichsgrafiken und eine zusammenfassende Konfigurationsmatrix;
- konkrete MongoDB-Indexdefinitionen für die empfohlene Ausgangskonfiguration;
- automatisierte Tests für Query Shapes, Indexdefinitionen, Ergebnisäquivalenz und Auswertung.

## Bekannte Risiken und offene Entscheidungen

- „Typische“ Query Shapes müssen durch aktuelle Storefront-Dokumentation oder geeignete E-Commerce-Referenzworkloads plausibilisiert und ausdrücklich als Auswahl statt als vollständiger Produktionsworkload bezeichnet werden.
- Die exakten Query-Limits und Sortierungen müssen so festgelegt werden, dass alle Kandidaten logisch äquivalente Ergebnisse liefern und reproduzierbar verglichen werden können.
- Die Selektivitätsverteilungen müssen einen fachlich plausiblen und ausreichend deutlichen Kontrast erzeugen; die tatsächlich beobachteten Trefferanteile werden im Laufmanifest dokumentiert.
- Die Produkt-ID-Modellierung ist vor dem finalen Kandidatendesign zu prüfen. Ein Partial Index auf `productId` ist nicht automatisch eine sinnvolle allgemeine Empfehlung, wenn Eindeutigkeit oder Abrufe inaktiver Produkte erforderlich sind.
- Die reduzierte gemeinsame Indexkonfiguration kann gegenüber den im vollständigen Kandidatenpool erzwungenen Einzelpfaden andere Planner-Entscheidungen und Cacheeigenschaften hervorrufen; genau dies ist Gegenstand der Abschlussvalidierung.
- Der gemeinsame Kandidatenpool erzeugt zusätzlichen Speicher- und Cache-Druck. Da alle erzwungenen Kandidatenvergleiche innerhalb desselben Pools stattfinden und die reduzierte Konfiguration separat validiert wird, bleibt dies kontrollierbar; Reihenfolge- und Cacheeffekte müssen dennoch dokumentiert werden.
- Ein mit `hint()` erzwungener Zugriff ist nur dann auswertbar, wenn der Kandidat die Query vollständig und korrekt unterstützt. Insbesondere Partial Indizes erfordern eine passende Filterbedingung.
- `explain()` ignoriert vorhandene Plan-Cache-Einträge, und seine `executionTimeMillis` ist nicht zwingend repräsentativ für den eingeschwungenen Anwendungsbetrieb. Explain-Struktur, wiederholte tatsächliche Query-Laufzeiten und natürliche Planner-Auswahl werden deshalb getrennt erhoben.
- Die kleine Schreibkostenprüfung quantifiziert nur ausgewählte Insert- und Update-Vorgänge. Sie macht die Empfehlung fairer, ersetzt aber keinen repräsentativen Lese-/Schreib-Produktionsworkload.
- Laufzeitmessungen bleiben von Caching, Hostlast, Messauflösung und Docker-Scheduling abhängig und werden nicht isoliert interpretiert.
- „Effizienz“ bezeichnet in den Schlussfolgerungen primär die Leseeffizienz und den Indexspeicherbedarf des Referenzworkloads, ergänzt um den gemessenen Aufwand der ausgewählten Schreiboperationen. Weiterer Schreib- und Wartungsaufwand bleibt eine Übertragungsgrenze.
- Der bisherige Seminar-Lauf bleibt als Pilot- und Vergleichsartefakt erhalten, liefert aber nicht die finale Evidenz für das überarbeitete Design. Für die Schlussfolgerungen ist ein neuer, sauber dokumentierter Benchmarklauf erforderlich.
