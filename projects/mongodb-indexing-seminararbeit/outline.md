# Argumentationslinie und Gliederung

Status: Durch G2 freigegeben.

## Forschungsfrage

**Hauptforschungsfrage**

Wie lässt sich für den Produktkatalog eines E-Commerce-Shops aus ausgewählten typischen Query Shapes unter Berücksichtigung von Selektivität und Datenmenge eine geeignete gemeinsame MongoDB-Indexkonfiguration ableiten und empirisch validieren?

**Teilfragen**

1. Welche Query Shapes bilden einen begrenzten und fachlich plausiblen Referenzworkload für typische Lesezugriffe auf einen E-Commerce-Produktkatalog?
2. Welche Indexkandidaten lassen sich aus den Filter-, Sortier-, Array- und Lookup-Anforderungen der einzelnen Query Shapes ableiten?
3. Wie unterscheiden sich die Indexkandidaten bei variierender Datenmenge und – soweit relevant – Selektivität hinsichtlich strukturellem Abfrageaufwand, Ausführungszeit und Indexspeicherbedarf?
4. Welche Kandidaten können unter Berücksichtigung von Compound-Präfixen, Redundanzen und bedingter Nutzbarkeit zu einer gemeinsamen Ausgangskonfiguration kombiniert werden, und wie bewährt sich diese gegenüber einer Baseline hinsichtlich Leseeffizienz, Indexspeicherbedarf und ausgewähltem indexbedingtem Schreibaufwand?

## Argumentationslinie

1. Ein Produktkatalog unterstützt mehrere wiederkehrende Lesezugriffe. Shopify-Storefront-Funktionen und ein historischer E-Commerce-Referenzworkload plausibilisieren gefilterte und sortierte Produktlisten, Tagfilter und Produktdetailabrufe, ohne Vollständigkeit oder repräsentative Häufigkeiten zu behaupten.
2. MongoDBs Dokumentmodell bildet heterogene Produktattribute und Arrays gemeinsam ab. Die Eignung eines Indexes folgt jedoch nicht aus seinem Typ allein, sondern aus Query Shape, Feldreihenfolge, Selektivität, Datenmenge und gegebenenfalls einer Partial-Bedingung.
3. Aus den drei festgelegten Query Shapes werden vor der Messung neun fachlich begründete Kandidaten abgeleitet. Die Theorie liefert damit Entwurfsregeln; sie nimmt keine Gewinner vorweg.
4. Der Benchmark trennt fünf Funktionen: indexlose Baseline, wiederholte Hint-Vergleiche im gemeinsamen Kandidatenpool, natürliche Planner-Auswahl, Reduktion redundanter oder unterlegener Kandidaten und Abschlussvalidierung des finalen Kombisets.
5. Explain-Metriken beschreiben den strukturellen Aufwand; separat gemessene Queryzeiten ergänzen ihn. Planner-Auswahl, tatsächliche Laufzeit und Indexeignung werden nicht gleichgesetzt.
6. Die Reduktion berücksichtigt nicht nur query-spezifische Messwerte, sondern auch Compound-Präfixe, Indexgrößen und bedingte Nutzbarkeit. Dadurch entsteht eine gemeinsame Konfiguration statt einer ungeprüften Sammlung einzelner Empfehlungen.
7. Eine begrenzte Insert-/Aktivstatus-Gegenprüfung quantifiziert den Schreibpreis des finalen Sets. Sie erweitert die Bewertung, ohne einen repräsentativen Schreibworkload zu behaupten.
8. Die Antwort besteht aus einer empirisch validierten Ausgangskonfiguration für die Referenzstruktur und einem Entscheidungsleitfaden. Beide gelten nur für vergleichbare Daten- und Zugriffsmuster und müssen an einem realen Workload erneut geprüft werden.

## Wortbudget

| Block | Kapitel | Zielwörter |
| --- | --- | ---: |
| Einleitung | 1 – Problemstellung und Untersuchungsziel | 350 |
| Theorie/Grundlagen | 2 – Von Query Shapes zur Indexkonfiguration | 1.550 |
| Hauptteil | 3 – Referenzworkload, Indexdesign und Benchmarkmethode | 850 |
| Hauptteil | 4 – Kandidatenvergleich, Kombiset und Diskussion | 1.400 |
| Fazit | 5 – Beantwortung der Forschungsfrage | 350 |
| **Gesamt** |  | **4.500** |

Die Zielwörter betreffen den Fließtext der fünf vorhandenen Manuskriptkapitel. Literaturverzeichnis, Verzeichnisse und Anhänge werden mangels abweichender Hochschulvorgabe nicht in das Planbudget eingerechnet. Der mündlich genannte Zielumfang von 4.000 Wörtern und die nicht dokumentierte formale Toleranz bleiben als Randbedingung bestehen.

Der bestehende Grundlagenentwurf umfasst näherungsweise 1.613 Wörter. Das neue Ziel von 1.550 Wörtern verlangt nur eine moderate Straffung. Frei werdender Raum und der erhöhte Hauptteilumfang werden für Kandidatendesign, Kombisetvalidierung und Schreibkostenprüfung verwendet. Der vorhandene Methodik- und Ergebnistext wird wegen des neuen Experiments nicht als belastbarer Zielwortstand behandelt.

## Kapitel

### intro – Problemstellung und Untersuchungsziel

- **Vorgesehene Unterstruktur:** typische Katalogzugriffe und praktische Relevanz; MongoDB als Dokumentdatenbank für die Referenzstruktur; Problem workload-bezogener Indexauswahl; Forschungsfragen, Beitrag und Aufbau.
- **Funktion:** Führt direkt von plausiblen E-Commerce-Zugriffen zur Notwendigkeit einer gemeinsamen, empirisch validierten Indexkonfiguration.
- **Teilfragen:** Rahmt alle Teilfragen, beantwortet sie aber noch nicht.
- **Erwartetes Ergebnis:** Klar begrenztes Untersuchungsversprechen: drei Query Shapes, query-spezifische Kandidaten, Reduktion zum Kombiset sowie Lese-, Speicher- und begrenzte Schreibkostenbewertung.
- **Voraussetzungen:** Bestätigter G1-Brief; Scoping-Belege zur Plausibilität der Query Shapes; kurze MongoDB-Begründung. KI bleibt optionaler Nebenmotivator.
- **Übergabe an Folgekapitel:** Benennt Query Shape, Selektivität, Datenmenge, Feldreihenfolge und Indexkosten als theoretisch zu klärende Kriterien.
- **Zielwörter:** 350.
- **Evidenzbedarf:** Shopify-Storefront-Dokumentation; MongoDB-Datenmodell und Indexing Strategies; workload-basierte Indexauswahl.
- **Praktische Artefakte:** Keine.
- **Medien:** Keine.

### foundations – Von Query Shapes zur Indexkonfiguration

- **Vorgesehene Unterstruktur:** 2.1 Dokumentmodell und Zugriffsmuster; 2.2 geordneter Indexzugriff, Query Planner, Hint und Explain; 2.3 Feldreihenfolge, Compound-Präfixe, Selektivität, Multikey, Partial und Unique; 2.4 Lese-/Schreib-/Speicher-Trade-off.
- **Funktion:** Entwickelt die Kriterien, aus denen der Kandidatenpool und seine Bewertungslogik nachvollziehbar folgen.
- **Teilfragen:** Begründet Teilfrage 1 und beantwortet Teilfrage 2 konzeptionell; schafft die Metrikgrundlage für Teilfragen 3 und 4.
- **Erwartetes Ergebnis:** Ein kompaktes Kriterienraster „Queryanforderung – Kandidatenmerkmal – erwartete Planwirkung – mögliche Kosten“.
- **Voraussetzungen:** Festgelegte Query Shapes; MongoDB-8.2-Dokumentation; B-Tree- und workload-basierte Grundlagen.
- **Übergabe an Folgekapitel:** Die Kriterien werden in Kapitel 3 auf Q1 bis Q3 angewandt und als konkrete Kandidatenmatrix operationalisiert.
- **Zielwörter:** 1.550. Der bestehende Entwurf wird fachlich korrigiert und um ungefähr 60 Wörter gestrafft; ein katalogartiger Überblick nicht verwendeter Indexarten entfällt.
- **Evidenzbedarf:** B-Tree-Grundlage; Query Shapes; Compound/ESR/Präfixe; Multikey; Partial; Unique; Hint, Explain, Plan Cache; Write Operation Performance; mehrdimensionale Indexauswahl.
- **Praktische Artefakte:** Query- und Indexdefinitionen zur Konsistenzprüfung, noch keine neuen Ergebnisse.
- **Medien:** Höchstens eine kompakte Kriterienmatrix, falls sie Prosa ersetzt.

### method – Referenzworkload, Indexdesign und Benchmarkmethode

- **Vorgesehene Unterstruktur:** 3.1 Referenzdokument, Datenverteilungen und drei Query Shapes; 3.2 Herleitung und Installation des Kandidatenpools; 3.3 Baseline-, Hint-, Planner-, Reduktions- und Finallauf; 3.4 Metriken, tatsächliche Queryzeit und Schreibkostenprüfung; 3.5 Umgebung und Reproduzierbarkeit.
- **Funktion:** Übersetzt die Forschungsfragen in ein kontrolliertes Verfahren und dokumentiert die Indexentscheidung vor Kenntnis der neuen Ergebnisse.
- **Teilfragen:** Operationalisiert alle vier Teilfragen.
- **Erwartetes Ergebnis:** Reproduzierbare Versuchsmatrix mit fünf Messstufen, neun Kandidaten, drei Skalen, relevanten Selektivitätsvarianten und einer begrenzten Schreibgegenprüfung.
- **Voraussetzungen:** Kriterienraster aus Kapitel 2; überarbeiteter Benchmarkcode; erfolgreicher Smoke-Test für Kandidatenpool und Hints.
- **Übergabe an Folgekapitel:** Legt Berichtsreihenfolge und Auswahlkriterien fest, sodass Kapitel 4 keine nachträglich erfundene Siegerlogik verwendet.
- **Zielwörter:** 850.
- **Evidenzbedarf:** Methodische Benchmarkquelle; MongoDB-Dokumentation zu Hint, Explain, Plan Cache, Partial-Eignung und Write Performance; eigene technische Artefakte.
- **Praktische Artefakte:** Generator, Query- und Indexregistries, Kandidatenmatrix, Benchmarkrunner, Tests, Manifest, Rohdaten und Write-Messartefakte.
- **Medien:** Eine Versuchsmatrix und optional ein kurzer Ablauf als nummerierte Folge; keine längeren Codeblöcke.

### evaluation – Kandidatenvergleich, Kombiset und Diskussion

- **Vorgesehene Unterstruktur:** 4.1 Baseline und Kandidatenvergleich für Q1; 4.2 Q2 und Q3; 4.3 natürliche Planner-Auswahl; 4.4 Reduktion und finale gemeinsame Konfiguration; 4.5 workloadweite Lesevalidierung und Schreibkostenprüfung; 4.6 Einordnung, Übertragbarkeit und Limitationen.
- **Funktion:** Berichtet zunächst die query-spezifischen Befunde und führt sie anschließend zu einer gemeinsamen, tatsächlich getesteten Indexaufstellung zusammen.
- **Teilfragen:** Beantwortet Teilfrage 3 empirisch und Teilfrage 4 durch Reduktion, Finallauf und Schreibgegenprüfung; synthetisiert die konzeptionellen Antworten aus Teilfragen 1 und 2.
- **Erwartetes Ergebnis:** Pro Query nachvollziehbare Kandidatenbefunde; Vergleich zwischen kontrolliert bestem Zugriffspfad und natürlicher Planner-Auswahl; exakte finale `createIndex`-Definitionen; Gesamtindexgröße; Baselinevergleich aller Queries; quantifizierter Insert-/Update-Mehraufwand; bedingte Handlungsempfehlungen.
- **Voraussetzungen:** Neuer erfolgreicher Benchmarklauf mit sauberer Artefaktzuordnung und vollständiger Ergebnisvalidierung.
- **Übergabe an Folgekapitel:** Liefert die verdichtete Konfiguration, Auswahlregeln und Grenzen für das Fazit.
- **Zielwörter:** 1.400.
- **Evidenzbedarf:** Eigene Mess- und Explain-Daten; MongoDB-Dokumentation für Planinterpretation und Kosten; Literatur zur workload-basierten Auswahl und Benchmarkvalidität.
- **Praktische Artefakte:** Baseline-, Hint-, Planner- und Finallaufdaten, Indexgrößen, Write-Messungen, Manifest und Registry.
- **Medien:** Je eine verdichtete Kandidatengrafik für Q1 und Q2, eine kompakte Tabelle für Q3/Planner-Auswahl sowie eine zentrale Tabelle der finalen Konfiguration mit Lese-, Speicher- und Schreibtrade-offs. Nur Medien mit eigener Aussagefunktion werden übernommen.

### conclusion – Beantwortung der Forschungsfrage

- **Vorgesehene Unterstruktur:** direkte Antwort; konkrete Ausgangskonfiguration und Entscheidungsleitfaden; Reichweite und kurzer Ausblick.
- **Funktion:** Beantwortet die Hauptforschungsfrage ohne neue Quellen oder Messergebnisse.
- **Teilfragen:** Verdichtet alle vier Teilantworten.
- **Erwartetes Ergebnis:** Aussage, wie aus Query Shapes Kandidaten entstehen, wie Messung und Reduktion zum Kombiset führen und unter welchen Bedingungen dieses auf ähnliche Kataloge übertragbar ist. Der gemessene Schreibpreis bleibt sichtbar.
- **Voraussetzungen:** Freigegebene Interpretation und finale Konfigurationsmatrix.
- **Übergabe an Folgekapitel:** Keine.
- **Zielwörter:** 350.
- **Evidenzbedarf:** Ausschließlich bereits geprüfte Evidenz aus Kapitel 4.
- **Praktische Artefakte:** Keine neuen.
- **Medien:** Keine.

## Festgelegte Strukturentscheidungen

- Die vorhandene Fünf-Kapitel-Struktur bleibt bestehen.
- Drei Query Shapes bilden den maximalen Referenzworkload; Q1 ist der Schwerpunkt.
- Indexarten werden nur erklärt, soweit sie konkrete Kandidaten begründen.
- Alle Kandidaten werden gemeinsam installiert, aber query-spezifisch wiederholt per Hint kontrolliert.
- Die natürliche Planner-Auswahl und der Hint-Vergleich sind getrennte Ergebnisse.
- Erst die reduzierte Konfiguration wird ohne Hint als Kombiset validiert.
- Die Schreibgegenprüfung umfasst nur standardisierten Batch-Insert und Aktivstatus-Update bei einer repräsentativen Skalierung.
- „Anwendungseffizienz“ wird nicht pauschal behauptet; die Arbeit bewertet Leseeffizienz, Indexspeicher und ausgewählten Schreibaufwand des Referenzworkloads.
- Der alte Seminar-Lauf bleibt Pilotartefakt; die finale Argumentation verwendet einen neuen Lauf.

## Noch nicht vorweggenommene Ergebnisentscheidungen

- Welcher Q1-Kandidat die beste Abwägung bildet.
- Ob der einzelne Kategorieindex trotz Compound-Präfix einen messbar gerechtfertigten Zusatznutzen besitzt.
- Ob Q2 einen vollständigen, Compound- oder Partial-Multikey-Index rechtfertigt.
- Ob MongoDB im vollständigen Pool denselben Kandidaten auswählt, der im Hint-Vergleich dominiert.
- Welche genaue gemeinsame Indexkonfiguration nach Reduktion bestehen bleibt.
- Wie groß der gemessene Lesegewinn und der Insert-/Update-Mehraufwand tatsächlich sind.
