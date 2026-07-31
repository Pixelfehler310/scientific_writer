# Forschungsauftrag

## Thema

Workload-basierte Auswahl geeigneter MongoDB-Indizes für typische Lesezugriffe auf den Produktkatalog eines E-Commerce-Shops.

## Problemstellung

MongoDB stellt mehrere Indexarten und Indexoptionen für unterschiedliche Daten- und Abfrageformen bereit. Ob ein Index einen Lesezugriff effizient unterstützt, hängt jedoch nicht allein von seiner Existenz ab, sondern insbesondere von der Abfrageform, der Reihenfolge indexierter Felder, der Selektivität und der Datenmenge. Ungeeignete Indizes können zusätzlichen Speicher belegen, ohne die Abfrageausführung zu verbessern.

Gleichzeitig erleichtern KI-gestützte Entwicklungswerkzeuge die Erstellung neuer E-Commerce-Anwendungen. Daraus entsteht ein praktischer Bedarf an nachvollziehbaren Ausgangsregeln für die Indexauswahl. Eine pauschale, für alle Shops gültige Indexstruktur lässt sich daraus nicht ableiten; benötigt wird stattdessen eine empirisch begründete Zuordnung typischer Abfrageanforderungen zu geeigneten Indexstrategien. Die Aktualität und Reichweite der KI-bezogenen Motivation ist in der Scoping-Recherche mit belastbaren Quellen zu prüfen.

## Forschungsfrage

**Hauptforschungsfrage**

Wie lassen sich anhand von Query Shape, Selektivität und Datenmenge geeignete MongoDB-Indizes für ausgewählte typische Lesezugriffe auf den Produktkatalog eines E-Commerce-Shops bestimmen?

**Teilfragen**

1. Welche Anforderungen an die Indexgestaltung ergeben sich aus den betrachteten Abfrageformen eines E-Commerce-Produktkatalogs?
2. Wie verändert sich der strukturelle Abfrageaufwand gegenüber einem Collection Scan, wenn bei wachsender Datenmenge ein auf die jeweilige Abfrage abgestimmter Index eingesetzt wird?
3. Unter welchen Bedingungen führt ein vorhandener und von MongoDB verwendeter Index dennoch nicht zu einer geringeren Laufzeit?
4. Wie beeinflusst die Beschränkung eines Indexes auf eine relevante Dokumentteilmenge das Verhältnis zwischen Zugriffseffizienz und Speicherbedarf?

## Ziel und erwarteter Beitrag

Ziel ist ein empirisch begründeter Entscheidungsleitfaden für die initiale Indexauswahl bei MongoDB-basierten E-Commerce-Produktkatalogen. Er soll ausgewählte typische Abfrageformen geeigneten Indexstrategien zuordnen, deren Nutzen anhand einheitlicher Effizienzkriterien bewerten und Grenzen der Übertragbarkeit sichtbar machen. Der Beitrag richtet sich besonders an Entwicklerinnen und Entwickler, die Shop-Anwendungen mit KI-Unterstützung erstellen, ersetzt jedoch keine Messung am tatsächlichen Produktionsworkload.

Als Ergebnisform werden szenariobezogene Messvergleiche, Diagramme und daraus abgeleitete Auswahlregeln erwartet. Dieser Forschungsauftrag legt keine konkreten Messergebnisse oder eine universell beste Indexstrategie vorab fest. Bereits vorhandene Benchmarkresultate werden in späteren Phasen als empirische Evidenz und nicht als Erwartung behandelt.

## Untersuchungsgegenstand

Untersucht wird die Collection `products` eines deterministisch erzeugten, synthetischen E-Commerce-Produktkatalogs in MongoDB Community 8.2.11. Die Datenmengen umfassen 10.000, 100.000 und 500.000 Dokumente. Als typische Lesezugriffe werden für diese Arbeit operationalisiert:

- Filterung nach einer Produktkategorie;
- kombinierte Filterung nach Kategorie und Aktivstatus mit Sortierung nach Preis;
- Suche nach häufigen und selektiveren Tags in einem Arrayfeld;
- punktueller Abruf eines aktiven Produkts über seine Produkt-ID.

Die zu vergleichenden Lösungsansätze werden aus den Abfrageanforderungen abgeleitet. Der Versuchsaufbau enthält eine Baseline ohne benutzerdefinierten Index, einen Single-Field-Index, unterschiedlich angeordnete Compound-Indizes, einen durch ein Arrayfeld entstehenden Multikey-Index sowie vollständige und mit `partialFilterExpression` eingeschränkte Produkt-ID-Indizes. Der Partial Index wird als Indexoption und nicht als eigenständige MongoDB-Grundindexart behandelt.

## Methode

Die Forschungsfrage wird durch ein kontrolliertes, reproduzierbares Benchmarkexperiment beantwortet. Der synthetische Datensatz wird für jede Skalierungsstufe mit dem festen Zufallsseed 42 erzeugt. Für jedes relevante Paar aus Abfrageszenario und Indexstrategie werden benutzerdefinierte Indizes kontrolliert entfernt und neu angelegt. Zwischen Strategien wird der Plan Cache geleert. Nach zwei Warmup-Ausführungen folgen fünf gemessene Wiederholungen mit `explain("executionStats")`; die Rohdaten und die Versuchskonfiguration werden unverändert gespeichert.

Die Effizienz wird primär anhand von `totalDocsExamined`, `totalKeysExamined`, `nReturned`, dem Ausführungsplan und erforderlichen Sortierstufen bewertet. Ergänzend werden die mediane Query-Ausführungszeit sowie, soweit für den Vergleich relevant, der zusätzliche Indexspeicherbedarf berücksichtigt. Laufzeiten werden wegen ihrer Abhängigkeit von Hostlast, Caching und Docker-Scheduling nur gemeinsam mit den strukturellen Metriken interpretiert. Die Auswertung ist deskriptiv und erhebt keinen Anspruch auf inferenzstatistische Verallgemeinerung.

## Scope

- MongoDB-basierte Produktkataloge mit einem den untersuchten Abfrageformen vergleichbaren Lesezugriff;
- MongoDB Community 8.2.11 als einzelne lokale Docker-Instanz mit deklarierten Limits von 2 CPUs und 2 GB RAM;
- deterministische synthetische Daten mit 10.000 bis 500.000 Produktdokumenten;
- Untersuchung von Query Shape, Feldreihenfolge, Selektivität, Datenmenge und partiellem Indexumfang;
- Bewertung der Leseeffizienz anhand struktureller Explain-Metriken, Laufzeitmedianen und relevanter Indexgrößen;
- Ableitung kontextgebundener Auswahlregeln statt einer universellen oder direkt übernehmbaren Gesamtindexstruktur.

## Explizite Ausschlüsse

- Schreib-, Aktualisierungs- und Löschworkloads sowie der Wartungsaufwand von Indizes;
- parallele Zugriffe, gemischte Produktionsworkloads und Netzwerklatenzen;
- Replikation, Sharding und Mehrknotencluster;
- Vergleiche mit relationalen oder anderen Datenbanksystemen;
- Geospatial- und Hashed-Indizes, da weder Geodatenabfragen noch Sharding untersucht werden;
- Text-, MongoDB-Search- und Vector-Search-Indizes, da Volltextsuche und Relevanzranking außerhalb des Untersuchungsgegenstands liegen;
- Wildcard-Indizes, da das untersuchte Schema und die abgefragten Felder vorab bekannt sind;
- Clustered Indexes, da keine alternative physische Organisation der Collection untersucht wird;
- eine vollständige E-Commerce-Anwendung oder eine allgemeingültige Indexkonfiguration für beliebige Shops.

## Zielgruppe

Die Arbeit richtet sich an Studierende sowie Entwicklerinnen und Entwickler mit grundlegenden MongoDB-Kenntnissen, insbesondere an Personen, die einen MongoDB-basierten E-Commerce-Produktkatalog mit KI-Unterstützung entwerfen oder überprüfen.

## Praktische Artefakte

- containerisierte MongoDB-Testumgebung;
- deterministischer Generator für synthetische Produktdaten;
- Python-CLI zur Verwaltung von Daten, Indizes und Benchmarkläufen;
- versionierte Abfrageszenarien und Indexdefinitionen;
- Manifeste, rohe Explain-Ausgaben und normalisierte Messdaten;
- statische Diagramme, interaktives Dashboard und Ergebniszusammenfassung;
- automatisierte Unit-, Integrations- und End-to-End-Tests.

## Bekannte Risiken und offene Entscheidungen

- Die Repräsentativität der vier ausgewählten Lesezugriffe als „typisch“ muss in der Scoping-Recherche belegt und begrenzt werden.
- Die KI-bezogene Motivation benötigt belastbare Quellen; die bloße Verfügbarkeit von KI-Shop-Buildern belegt noch keine konkrete Anzahl neu entstehender Shops.
- Der synthetische Datensatz bildet keine reale Produkt-, Zugriffs- oder Änderungsverteilung vollständig ab.
- Fünf Messwiederholungen und die Millisekundenauflösung von `executionTimeMillis` erlauben nur eine deskriptive Laufzeitauswertung.
- Der vorhandene Seminar-Benchmark wurde laut Manifest in einem nicht sauberen Git-Arbeitsbaum ausgeführt. Diese Einschränkung der exakten Code-Provenienz wurde am 31.07.2026 menschlich akzeptiert und wird bei der späteren Evidenzbewertung transparent dokumentiert; ein erneuter Lauf ist dafür nicht verpflichtend.
- Da Indizes szenarioweise und nicht gemeinsam in einem gemischten Shop-Workload getestet werden, kann die Arbeit Auswahlregeln, aber keine vollständig validierte Gesamtindexstruktur liefern.
