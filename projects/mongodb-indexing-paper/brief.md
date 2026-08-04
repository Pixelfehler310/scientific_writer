# Forschungsauftrag

**Status:** Entwurf zur menschlichen Prüfung für G1  
**Grundlage:** `planning/2026-08-04-neuer-seminarbeitsplan.md` und Abgleich mit dem Benchmark-Code vom 04.08.2026

## Thema

Workloadbasierte Auswahl und empirische Validierung geeigneter MongoDB-Indexsets für einen E-Commerce-Produktkatalog.

## Problemstellung

Für einen MongoDB-Produktkatalog können mehrere Indizes einzelne Abfragen beschleunigen, verursachen zusammen aber zusätzlichen Speicherbedarf und Pflegeaufwand bei Schreiboperationen. Die isolierte Bewertung einzelner Indextypen beantwortet deshalb nicht, welches vollständige Indexset für einen festgelegten Workload einen nachvollziehbaren Kompromiss bildet. Die Arbeit ersetzt den verworfenen Ansatz einer Enumeration von 256 Indexsets mit Proxy-Kostenmodell, Pareto-Screening und nachgelagerter Finalistenauswahl durch einen kontrollierten Direktvergleich weniger vorab begründeter Konfigurationen.

## Forschungsfrage

Welche MongoDB-Indexsets eignen sich für einen definierten E-Commerce-Produktkatalog-Workload unter Berücksichtigung von Query-Latenz, strukturellen Explain-Metriken, Indexspeicher und Write-Aufwand, und welche bedingte Empfehlung lässt sich daraus ableiten?

Die Frage verlangt keine universell optimale Konfiguration. „Geeignet“ bezeichnet ausschließlich einen im dokumentierten Versuchsraum empirisch begründeten Kompromiss.

## Ziel und erwarteter Beitrag

Die Arbeit soll drei bis vier vor der Ergebnisauswertung festgelegte vollständige Indexkonfigurationen fair vergleichen. Ihr Ergebnis ist keine automatische Empfehlung und keine globale Rangliste, sondern eine begrenzte, bedingte Empfehlung für den untersuchten Datenbestand, Workload und die Messumgebung. Erwartet wird außerdem ein reproduzierbares Benchmark-Artefakt, das den Datenzustand, die installierte Konfiguration, Messparameter, Ergebnisvalidierung und Rohartefakte nachvollziehbar festhält.

Erwartungsmuster wie schnellere szenariospezifische Lesezugriffe oder höhere Write-Kosten sind Hypothesen. Sie dürfen nicht als Ergebnisse formuliert werden, bevor der eingefrorene Referenzlauf ausgewertet ist.

Geeignete Indexsets können den Ressourcenaufwand einzelner Queries verringern und damit unter gleicher Infrastruktur indirekt Kapazitätsreserven für parallele Zugriffe schaffen. Die tatsächliche Belastbarkeit bei gleichzeitigem Zugriff oder in einem MongoDB-Cluster wird jedoch nicht gemessen und ist kein Ergebnisanspruch dieser Arbeit.

## Untersuchungsgegenstand

Untersucht wird eine einzelne MongoDB-Collection mit synthetischen Produktdokumenten. Relevante Felder sind mindestens `productId`, `category`, `brand`, `price`, `isActive`, `rating`, `stock`, `tags`, `createdAt` und `updatedAt`. Verteilungen, Kardinalitäten und häufige beziehungsweise seltene Parameterwerte werden vor dem Referenzlauf dokumentiert und eingefroren.

Die Skalierungsanalyse umfasst drei vorab definierte Bestandsgrößen: 1.000, 10.000 und 100.000 Dokumente. Der 1.000er-Bestand dient primär als Smoke- und Plausibilitätsstufe; die 10.000er- und 100.000er-Bestände bilden die regulären Vergleichsstufen. Die Skalierung ist eine Analyseperspektive und keine eigenständige Forschungsfrage.

## Workload

Die IDs werden für Plan, Code und spätere Auswertung wie folgt vereinheitlicht:

| ID | Operation | Verbindliche fachliche Form |
| --- | --- | --- |
| Q1 | Kategorieansicht | `category`, `isActive = true`, Preisbereich; Sortierung nach `price` aufsteigend; getrennte enge und breite Preisbereichsvarianten; erste Seite mit `limit: 24` |
| Q2 | Tag-Suche | `tags`, `isActive = true`; ein häufiger und ein seltener Tagwert als getrennte Varianten; erste Seite mit `limit: 24` |
| Q3 | Marken-/Filteransicht | `brand`, `category`, `isActive = true`; je eine häufige und seltene Markenvariante bei gleicher Kategorie; Sortierung nach `price` aufsteigend; erste Seite mit `limit: 24` |
| Q4 | Produktdetail | eindeutiger Lookup über `productId`; Kontroll- und Korrektheitsszenario, kein Optimierungskern |
| W1 | Produktanlage | Batch-Insert von 1.000 deterministisch erzeugten neuen Produkten aus identischem Ausgangszustand |
| W2a | nicht indexiertes Update | Batch-Update von `stock` für dieselben 1.000 vorab bestimmten Produkte |
| W2b | potenziell indexiertes Update | Batch-Update von `price` für dieselben 1.000 vorab bestimmten Produkte; `price` ist nur in den dafür vorgesehenen Konfigurationen indexiert |

Die Messung betrachtet bei Q1 bis Q3 ausschließlich die erste Ergebnisseite; `skip`-basierte Pagination, Cursor-Fortsetzung und Facettenaggregation sind nicht Teil dieses Workloads. Konkrete Kategorien, Marken, Tags, Preisgrenzen und der Seed werden spätestens mit G2 festgelegt und danach nur über dokumentierte Invalidierung geändert.

## Indexkonfigurationen

Alle Konfigurationen enthalten den unvermeidlichen `_id`-Index und denselben eindeutigen Index auf `productId`. Dieser gemeinsame Anteil wird bei Speicher- und Write-Auswertungen offengelegt. Folgende Designrollen werden vorab festgelegt:

- **B – Basis:** nur die gemeinsamen notwendigen Indizes; Referenzkonfiguration.
- **L – query-lokal:** je ein unmittelbar auf Q1, Q2 und Q3 zugeschnittener Index. L ist eine leseorientierte Vergleichshypothese, keine garantierte Leistungsobergrenze.
- **W1 – workloadorientiert, leseorientiert:** ein kleineres Set, in dem insbesondere der Q1-Index auch Q3 teilweise unterstützt und dadurch einen Spezialindex einspart.
- **W2 – ressourcenbewusst:** ein nochmals kompakteres Set, das einen für `isActive = true` begründeten Partial Index prüft und bewusst schwächere Unterstützung einzelner Szenarien zulässt.

Die vollständigen Schlüsseldefinitionen einschließlich Reihenfolge, Richtung, `unique` und `partialFilterExpression` werden in Scoping und Outline festgelegt und vor jeder Referenzmessung versioniert eingefroren. Falls zwei Konfigurationen nach dieser Definition keinen materiellen Designunterschied besitzen, wird nicht künstlich an beiden festgehalten.

## Methode

Die Untersuchung ist ein kontrolliertes technisch-empirisches Fallbeispiel:

1. Für jede Kombination aus Datenmenge und Indexkonfiguration wird derselbe deterministische Ausgangsbestand hergestellt.
2. Es wird genau eine vollständige Kandidatenkonfiguration installiert und im Manifest dokumentiert.
3. Query-Varianten erhalten drei nicht gewertete Warmups und anschließend 30 Messwiederholungen. Berichtet werden Median und Interquartilsabstand; ein p95 wird bei diesem Stichprobenumfang nicht als belastbare Hauptkennzahl verwendet.
4. Die Reihenfolge der Konfigurationen wird je Messblock deterministisch rotiert, um zeitlichen Drift nicht systematisch einer Konfiguration zuzuordnen.
5. Laufzeitmessungen und `explain("executionStats")` werden getrennt erhoben. Explain-Aufrufe fließen nicht in die Query-Latenz ein.
6. Erfasst werden Query-Latenz, `nReturned`, `totalDocsExamined`, `totalKeysExamined`, Planstufen, sekundärer Indexspeicher sowie Insert- und Update-Batchzeiten.
7. Für Write-Batches werden zehn vollständige Wiederholungen aus jeweils identischem Ausgangszustand ausgeführt und mit Median sowie Spannweite berichtet.
8. Ergebnis-IDs beziehungsweise Ergebnismengen müssen zwischen allen Konfigurationen fachlich gleich sein. Eine Verletzung ist ein Validierungsfehler, kein Performanceergebnis.

Der vorhandene Runner misst derzeit einzelne Strategie-Szenario-Kombinationen. Vollständige Konfigurationen, Q3 in der hier festgelegten Form sowie W1/W2a/W2b sind noch zu implementierende praktische Arbeit.

## Entscheidungslogik

Es gibt keine nachträglich konstruierte Gesamtpunktzahl und keine verdeckten Query-Gewichte. Die Auswertung erfolgt in drei Stufen:

1. **Wirksamkeit je Kernquery:** Vergleich von Latenz und strukturellem Suchaufwand für Q1 bis Q3 gegenüber B.
2. **Kosten und Dominanz:** Vergleich von Indexspeicher und Write-Aufwand; eine Konfiguration, die in den relevanten Dimensionen nicht besser und mindestens einmal schlechter ist, kann als dominiert eingeordnet werden.
3. **Bedingte Empfehlung:** getrennte Empfehlung für eine Priorität auf Leseleistung und für eine Priorität auf Ressourcen-/Write-Kosten, falls kein Kandidat beide Ziele überzeugend verbindet.

Q4 sichert die gemeinsame Basis ab und erhält kein Gewicht bei der Auswahl. Die engen/breiten sowie häufigen/seltenen Varianten von Q1 bis Q3 werden jeweils getrennt dargestellt und nicht zu einem Durchschnittswert vermischt.

## Scope

Die Aussage gilt ausschließlich für das festgelegte Produktmodell, die dokumentierten Datenverteilungen und Bestandsgrößen von 1.000, 10.000 und 100.000 Dokumenten, die registrierten Query- und Schreibvarianten, die eingesetzte MongoDB-Version und Serverkonfiguration sowie die konkrete lokale Messumgebung. Übertragbar ist nur die kontrollierte Vergleichsmethode, nicht die konkrete Indexempfehlung.

## Explizite Ausschlüsse

- keine automatische Indexsuche oder Indexberatung;
- keine vollständige Enumeration möglicher Indexsets;
- kein Vergleich mit PostgreSQL oder anderen Datenbanksystemen;
- kein Sharding, kein verteilter Cluster und kein Anwendungscache;
- keine Parallel- oder Mehrbenutzer-Lasttests;
- keine kausale Verallgemeinerung auf produktive E-Commerce-Systeme;
- kein Beweis, dass außerhalb der getesteten Kandidaten kein besseres Indexset existiert.

## Zielgruppe

Entwicklerinnen und Entwickler sowie technisch interessierte Leserinnen und Leser mit grundlegenden Kenntnissen zu Datenbanken und MongoDB, die für einen konkreten Anwendungsfall eine passende Indexstrategie auswählen und deren Zielkonflikte nachvollziehen möchten.

## Praktische Artefakte

- Benchmark-Code im eigenständigen Repository `D:\projects\uni\mongodb_indexing`;
- deklarative vollständige Indexkonfigurationen und vereinheitlichte Szenario-IDs;
- getestete Reset-, Rotations-, Ergebnisvalidierungs-, Speicher- und Write-Messlogik;
- eingefrorenes Konfigurations- und Umgebungsmanifest;
- Rohdaten, getrennte Explain-Artefakte und reproduzierbare Auswertungstabellen beziehungsweise Grafiken;
- technischer Pilot und davon klar getrennter finaler Referenzlauf.

## Bekannte Risiken und Prüfpunkte vor G1

- Die gewählten 30 Query-Wiederholungen und zehn Write-Batches müssen hinsichtlich Gesamtlaufzeit praktisch machbar sein; eine Reduktion würde die Kennzahlenregel erneut zur Entscheidung stellen.
- Q1 bis Q3 benötigen noch exakt eingefrorene Parameterwerte und Limits; dies ist als nachgelagerte Operationalisierung bis G2 vorgesehen.
- Die konkreten L/W1/W2-Schlüssel müssen nach Prüfung der finalen Query Shapes festgelegt werden, ohne Ergebnisse des neuen Referenzlaufs zu verwenden.
- MongoDB-Version, Cache-/Plan-Cache-Behandlung und Maschinenkonfiguration müssen im Scoping reproduzierbar dokumentierbar sein.
- Der Alt-Pilot vom 24.07.2026 darf nur Pipeline- und Parameterschätzung unterstützen und wird nicht als Evidenz für die neue Forschungsfrage behandelt.
