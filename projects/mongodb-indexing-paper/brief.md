# Forschungsauftrag

## Thema

Workloadbasierte Auswahl und empirische Validierung geeigneter MongoDB-Indexsets für einen E-Commerce-Produktkatalog.

## Problemstellung

Der Neustart ersetzt den verworfenen Ansatz einer vollständigen Enumeration von 256 Indexsets mit Proxy-Kostenmodell, Pareto-Screening und Finalistenauswahl. Untersucht werden stattdessen drei bis vier vorab festgelegte vollständige Indexkonfigurationen in einem kontrollierten Fallbeispiel.

## Forschungsfrage

Welche MongoDB-Indexsets eignen sich für einen definierten E-Commerce-Produktkatalog-Workload unter Berücksichtigung von Query-Latenz, strukturellen Explain-Metriken, Indexspeicher und Write-Aufwand, und welche bedingte Empfehlung lässt sich daraus ableiten?

## Ziel und erwarteter Beitrag

Ziel ist eine nachvollziehbare, auf den dokumentierten Versuchsraum begrenzte Empfehlung. Das praktische Artefakt ist eine reproduzierbare, konfigurationsbasierte MongoDB-Benchmark-Suite.

## Untersuchungsgegenstand

Ein synthetischer E-Commerce-Produktkatalog, ein vorab definierter Lese- und Schreibworkload sowie vollständige Indexkonfigurationen B, L, W1 und gegebenenfalls W2.

## Methode

Kontrollierter technisch-empirischer Vergleich mit identischen Ausgangsdaten, eingefrorenen Indexdefinitionen, wiederholten Laufzeitmessungen, getrennten Explain-Aufrufen, Speicher- und Write-Messungen sowie Ergebnisvalidierung.

## Scope

MongoDB, ein lokaler Produktkatalog-Fall, registrierte Query- und Schreibszenarien, dokumentierte Datenverteilungen und eine konkrete Messumgebung.

## Explizite Ausschlüsse

Keine automatische Indexempfehlung, keine vollständige Enumeration, kein Datenbanksystemvergleich, kein Sharding, keine verteilten Cluster, keine umfassenden Parallel-Lasttests und keine universelle Optimalaussage.

## Zielgruppe

Technisch versierte Leserinnen und Leser mit Grundlagenwissen zu Datenbanken und MongoDB.

## Praktische Artefakte

- bestehendes Benchmark-Code-Repository `D:\projects\uni\mongodb_indexing` als technische Ausgangsbasis;
- neue vollständige Indexkonfigurationen und Write-Batches;
- reproduzierbare Rohdaten-, Explain-, Manifest-, Tabellen- und Grafikartefakte;
- ein neuer, vorab eingefrorener Referenzlauf.

## Bekannte Risiken und offene Entscheidungen

Dieser Brief ist ein Initialisierungsentwurf und noch nicht G1-reif. Vor G1/G2 sind mindestens zu klären:

1. Query-Set und IDs vereinheitlichen; die neue Markenquery Q3 fehlt im Code.
2. Entscheidungslogik für „workloadbasiert“ vorab festlegen, ohne nachträglichen Gesamtscore.
3. B, L, W1 und W2 vollständig inklusive Feldreihenfolge, Richtung, Unique- und Partial-Eigenschaften einfrieren.
4. Wiederholungszahl und robuste Kennzahlen konsistent festlegen; bei wenigen Wiederholungen kein belastbares p95 behaupten.
5. Insert- und Update-Batches exakt definieren und Updates indexierter/nicht indexierter Felder trennen.
6. Zielumfang verbindlich auf 4.000 Wörter festlegen und Kapitelbudgets abstimmen.
7. L nur als leseorientierte Vergleichskonfiguration beziehungsweise Hypothese bezeichnen, nicht als garantierte Leistungsobergrenze.
