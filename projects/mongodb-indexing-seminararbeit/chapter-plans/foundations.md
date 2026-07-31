# Kapitelplan: foundations Von Query Shapes zur Indexkonfiguration

## Funktion im Gesamtargument

Das Kapitel entwickelt ausschließlich die Begriffe und Entwurfsregeln, aus denen Kandidatenpool, Vergleichskriterien und spätere Reduktionsentscheidung folgen. Die vorhandene Gliederung in 2.1 Dokumentmodell und Indexierung, 2.2 Abfrageverarbeitung und `explain` sowie 2.3 relevante Indexstrategien bleibt bestehen. Die Absatzfunktionen dienen der gezielten Überarbeitung des vorhandenen Textes und verlangen keine neue Unterkapitelstruktur.

## Teilfrage und erwartetes Ergebnis

- **Teilfrage:** Begründet Teilfrage 1, beantwortet Teilfrage 2 konzeptionell und schafft Kriterien für Teilfragen 3 und 4.
- **Erwartetes Ergebnis:** Ein in Prosa entwickeltes Kriterienraster „Queryanforderung – Indexmerkmal – erwartete Planwirkung – Kosten/Risiko“, das in Kapitel 3 in der konkreten Kandidaten- und Versuchsmatrix operationalisiert wird.

## Wortbudget

**1.550 Wörter**

## Voraussetzungen und Übergabe

- **Voraussetzungen:** Festgelegte Query Shapes aus Scoping; verifizierte MongoDB- und Grundlagenquellen folgen erst nach G3.
- **Übergabe:** Liefert Regeln zu Equality/Sortierung, Präfixen, Multikey, Partial, Unique, Explain und Kosten für die Kandidatenmatrix und Messauswertung in Kapitel 3.

## Umsetzung im vorhandenen Manuskript

| Bestehender Abschnitt | Zugeordnete Absatzfunktionen | Geplanter Eingriff |
| --- | --- | --- |
| 2.1 Dokumentenorientierte Datenbanken und Indexierung | FO-02-P01, FO-02-P02, FO-02-P11 | Bestehenden Text sprachlich straffen; Produktdokument, Workloadbezug sowie Speicher-/Schreibkosten beibehalten und präzisieren. |
| 2.2 Indexbasierte Abfrageverarbeitung und `explain` | FO-02-P03, FO-02-P10 | Vorhandene Plan- und Explain-Erklärung kürzen; Versionsbezug korrigieren sowie `hint()`, Plan-Cache-Grenze und getrennte reale Queryzeit ergänzen. |
| 2.3 Indexarten und Entwurfsstrategien | FO-02-P04 bis FO-02-P09, FO-02-P11 | Vorhandene Abschnitte zu Selektivität, Single Field, Compound/ESR, Multikey und Partial weiterverwenden; Datenmengeneffekt und Unique-`productId` ergänzen. |

Die Zuordnung ist funktional: Die stabilen Absatzfunktionen werden innerhalb der drei vorhandenen Unterkapitel umgesetzt, ohne daraus elf neue Unterkapitel zu bilden. Jeder Manuskriptabsatz behält dabei eine eindeutige Hauptfunktion. Ziel ist eine punktuelle Überarbeitung statt einer Neufassung.

## Absatzplan

| Absatz-ID | Funktion | Kernaussage | Begründung | Evidenzbedarf | Beziehung davor | Beziehung danach | Zielwörter | Medium | Offene Recherche |
| --- | --- | --- | --- | --- | --- | --- | ---: | --- | --- |
| FO-02-P01 | Begriffsrahmen | Der dokumentierte MongoDB-Begriff Query Shape wird von den zusätzlichen experimentellen Parametern wie Limit und konkretem Filterwert unterschieden; gemeinsam bilden sie den festgelegten Referenzworkload. | Verhindert eine unscharfe Definition und macht die workload-basierte Betrachtung prüfbar. | MongoDB-Dokumentation zu Query Shapes; Literatur zur workload-basierten Indexauswahl. | Anschluss an Einleitung. | Voraussetzung → FO-02-P02. | 120 | keines | Terminologie und Versionsbezug für Query Shape exakt verifizieren. |
| FO-02-P02 | Modellbezug | Das Produktdokument verbindet Kategorie, Preis, Tags, Aktivstatus und fachliche ID; erläutert werden nur die für Q1–Q3 relevanten Strukturmerkmale. | Verhindert eine allgemeine Einführung in MongoDB-Datenmodellierung. | MongoDB-Dokumentmodell; eigene Referenzstruktur. | Konkretisierung. | Grundlage → FO-02-P03. | 100 | keines | Vorhandene Grundlagenpassagen auf Wiederverwendung und Straffung prüfen. |
| FO-02-P03 | Zugriffsmechanismus erklären | Geordnete Indexstrukturen eröffnen alternative Zugriffspfade; Explain macht insbesondere Collection-/Index-Scan, Fetch und Sortierstufen sowie deren Aufwand sichtbar. | Strukturmetriken lassen sich nur vor diesem Planmodell interpretieren. | B-Tree-Grundlage; MongoDB-Index- und Explain-Dokumentation. | Fortführung. | Voraussetzung → FO-02-P04. | 170 | keines | Planstufen, Implementierungsbehauptungen und Kennzahlen gegen MongoDB 8.2 verifizieren. |
| FO-02-P04 | Selektivität und Datenmenge erklären | Der mögliche Indexnutzen hängt davon ab, welchen Anteil des Bestands ein Prädikat trifft und wie sich die absolute Prüfmenge mit wachsendem Datenbestand verändert; Selektivität und Datenmenge sind deshalb getrennt zu beobachten. | Verankert beide Variablen der Forschungsfrage vor ihrer Operationalisierung. | Datenbankgrundlage zu Selektivität; MongoDB-Dokumentation zu selektiven Queries. | Folge aus Zugriffsmechanismus. | Ursache → FO-02-P05. | 150 | keines | Belastbare Definition und Grenzen der Selektivitätsinterpretation auswählen. |
| FO-02-P05 | Feldreihenfolge begründen | Bei Q1 beeinflussen Equality-Felder und Sortierung die Eignung eines Compound-Index; unterschiedliche Reihenfolgen sind daher Kandidaten, keine austauschbaren Varianten. | Leitet I1–I5 ohne Siegerannahme her. | MongoDB ESR-/Compound-Index-Dokumentation. | Anwendung. | Einschränkung → FO-02-P06. | 180 | keines | ESR ohne Überverallgemeinerung für die konkrete Equality-/Sortierquery erläutern. |
| FO-02-P06 | Präfix und Redundanz einordnen | Ein Compound-Index kann bestimmte Präfixzugriffe unterstützen, ersetzt aber nicht automatisch jeden Einzelindex; Redundanz ist empirisch und workloadbezogen zu beurteilen. | Begründet die spätere Reduktionsstufe statt einer automatischen Löschung. | MongoDB Compound-Index-Präfixe; ggf. Literatur zu workload-basiertem Design. | Einschränkung. | Themenwechsel → FO-02-P07. | 140 | keines | Verifizierbare Quelle zur Präfixnutzung und ihren Grenzen auswählen. |
| FO-02-P07 | Arrayzugriff erklären | Ein Index auf `tags` wird multikey; seine Nutzbarkeit für Q2 ist anhand von Arraysemantik und konkreten Filtern zu beurteilen. | Verbindet Q2 mit I6–I8, ohne ausgeschlossene Indexarten auszuführen. | MongoDB Multikey-Dokumentation. | Anwendung eines Sonderfalls. | Folge → FO-02-P08. | 140 | keines | Grenzen von Compound-Multikey-Indizes nur soweit für Q2 relevant prüfen. |
| FO-02-P08 | Bedingte Indexierung erklären | Partial Indizes begrenzen Schlüsselmenge und Wartung nur unter passender Filterbedingung; sie dürfen nur für logisch berechtigte Queries verglichen werden. | Begründet die Eligibility-Regel für I5 und I8. | MongoDB Partial-Index-Dokumentation. | Kontrast. | Themenwechsel → FO-02-P09. | 150 | keines | Technische Zulässigkeit gleichartiger Full-/Partial-Key-Patterns bleibt bis zum Smoke-Test offen. |
| FO-02-P09 | Eindeutigkeit und Lookup einordnen | Für den Detailabruf ist ein eindeutiger fachlicher Schlüssel zugleich Modellierungs- und Zugriffsentscheidung; der Unique-Index ist nicht mit einer allgemeinen `_id`-Empfehlung gleichzusetzen. | Begründet I9 und die nur konzeptionell behandelte `_id`-Alternative. | MongoDB Unique-Index-Dokumentation; Referenzschema. | Fortführung. | Voraussetzung → FO-02-P10. | 110 | keines | Quellen- und Modellbezug für `productId`-Eindeutigkeit präzisieren. |
| FO-02-P10 | Messgrößen abgrenzen | `totalDocsExamined`, `totalKeysExamined`, `nReturned` und Planstufen beschreiben strukturellen Aufwand; wiederholte reale Queryzeiten ergänzen ihn, während Explain-Zeit und Plannerwahl keine direkte Siegerbehauptung erlauben. | Sichert die getrennte Bewertung in Kapitel 3 und 4 methodisch ab. | MongoDB Explain, Query Plans, Hint und Plan Cache. | Synthese. | Folge → FO-02-P11. | 160 | keines | Exakte Dokumentationsstellen zu Explain und Plan Cache erfassen. |
| FO-02-P11 | Kosten und Kriterien synthetisieren | Indexauswahl ist eine Abwägung aus Leseaufwand, Speicher, bedingter Nutzbarkeit und Schreibwartung; daraus folgt das Kriterienraster für die Methode. | Verhindert die Reduktion auf eine einzelne Laufzeit. | MongoDB Write Performance/Indexing Strategies; ggf. workload-Auswahlliteratur. | Synthese. | Übergabe → Kapitel 3. | 130 | keines | Methodische Referenz für mehrdimensionale Bewertung auswählen. |

**Summe Zielwörter: 120 + 100 + 170 + 150 + 180 + 140 + 140 + 150 + 110 + 160 + 130 = 1.550.**

## Medienplan

Keine neue Tabelle im Grundlagenkapitel. Die konkrete Zuordnung von Query Shapes, Kandidaten und Messkriterien wird einmalig in Kapitel 3 dargestellt. Eine vorhandene B-Tree-Skizze bleibt nur optional, falls sie den Zugriffsmechanismus knapper als zusätzliche Prosa erklärt.

## Offene Entscheidungen

- Keine Darstellung ausgeschlossener Indexarten außer einer knappen Scope-Erinnerung, falls nötig.
- Der bestehende Grundlagenentwurf bleibt die Textbasis. Geplant sind nur die in der Manuskriptzuordnung genannten Korrekturen, Ergänzungen und Straffungen.
- Smoke-Test-Ergebnis und konkrete Kandidatenleistungen gehören nicht in dieses Kapitel.
