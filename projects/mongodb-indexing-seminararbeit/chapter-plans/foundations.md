# Kapitelplan: foundations Von Query Shapes zur Indexkonfiguration

## Funktion im Gesamtargument

Das Kapitel entwickelt ausschließlich die Begriffe und Entwurfsregeln, aus denen Kandidatenpool, Vergleichskriterien und spätere Reduktionsentscheidung folgen. Es ersetzt einen Katalog von Indexarten durch ein für Q1–Q3 verwendbares Kriterienraster.

## Teilfrage und erwartetes Ergebnis

- **Teilfrage:** Begründet Teilfrage 1, beantwortet Teilfrage 2 konzeptionell und schafft Kriterien für Teilfragen 3 und 4.
- **Erwartetes Ergebnis:** Ein Kriterienraster „Queryanforderung – Indexmerkmal – erwartete Planwirkung – Kosten/Risiko“, das in Kapitel 3 auf die neun Kandidaten angewandt wird.

## Wortbudget

**1.550 Wörter**

## Voraussetzungen und Übergabe

- **Voraussetzungen:** Festgelegte Query Shapes aus Scoping; verifizierte MongoDB- und Grundlagenquellen folgen erst nach G3.
- **Übergabe:** Liefert Regeln zu Equality/Sortierung, Präfixen, Multikey, Partial, Unique, Explain und Kosten für die Kandidatenmatrix und Messauswertung in Kapitel 3.

## Absatzplan

| Absatz-ID | Funktion | Kernaussage | Begründung | Evidenzbedarf | Beziehung davor | Beziehung danach | Zielwörter | Medium | Offene Recherche |
| --- | --- | --- | --- | --- | --- | --- | ---: | --- | --- |
| FO-02-P01 | Begriffsrahmen | Ein Query Shape umfasst Filter, Sortierung, Limit und erwartete Ergebnismenge; die gleiche Collection kann deshalb mehrere Indexanforderungen erzeugen. | Macht die workload-basierte statt indexartenbasierte Betrachtung terminologisch präzise. | MongoDB-Query-/Indexdokumentation; workload-basierte Indexauswahl. | Anschluss an Einleitung. | Voraussetzung → FO-02-P02. | 150 | keines | Terminologische Quelle für Query Shape/Workload auswählen. |
| FO-02-P02 | Modellbezug | Das Produktdokument verbindet Kategorie, Preis, Tags, Aktivstatus und fachliche ID; relevante Felder werden nur insoweit erklärt, wie Q1–Q3 sie nutzen. | Verhindert eine allgemeine Einführung in MongoDB-Datenmodellierung. | MongoDB-Dokumentmodell; eigene Referenzstruktur. | Konkretisierung. | Grundlage → FO-02-P03. | 150 | keines | Prüfen, welche vorhandenen Grundlagenpassagen wiederverwendbar und zu kürzen sind. |
| FO-02-P03 | Mechanismus erklären | B-Tree-Indizes ermöglichen geordneten Schlüsselzugriff; Explain unterscheidet u. a. Index-/Collection-Scan, Fetch und Sortierstufe. | Strukturelle Metriken können nur mit dieser Planlogik sinnvoll interpretiert werden. | MongoDB Explain und Indexgrundlagen. | Fortführung. | Voraussetzung → FO-02-P04. | 220 | keines | Planstufen und Kennzahlen exakt gegen MongoDB-Version dokumentieren. |
| FO-02-P04 | Feldreihenfolge begründen | Bei Q1 beeinflussen Equality-Felder und Sortierung die Eignung eines Compound-Index; unterschiedliche Reihenfolgen sind daher Kandidaten, keine austauschbaren Varianten. | Leitet I1–I5 ohne Siegerannahme her. | MongoDB ESR-/Compound-Index-Dokumentation. | Ursache. | Kontrast → FO-02-P05. | 220 | Kriterienmatrix M-FO-01 | Prüfen, wie ESR ohne Überverallgemeinerung für diese konkrete Gleichheits-/Sortierquery erläutert wird. |
| FO-02-P05 | Präfix und Redundanz einordnen | Ein Compound-Index kann bestimmte Präfixzugriffe unterstützen, ersetzt aber nicht automatisch jeden Einzelindex; Redundanz ist empirisch und workloadbezogen zu beurteilen. | Begründet spätere Reduktionsstufe statt automatischer Löschung. | MongoDB Compound-Index-Präfixe; ggf. workload-design-Literatur. | Einschränkung. | Themenwechsel → FO-02-P06. | 180 | M-FO-01 | Verifizierbare Quelle zur Präfixnutzung und Grenzen auswählen. |
| FO-02-P06 | Arrayzugriff erklären | Ein Index auf `tags` wird multikey; seine Nutzbarkeit für Q2 muss mit der Arraysemantik und den konkreten Filtern beurteilt werden. | Verbindet Q2 mit I6–I8, ohne andere Indexarten auszuführen. | MongoDB Multikey-Dokumentation. | Anwendung eines Sonderfalls. | Folge → FO-02-P07. | 170 | keines | Grenzen von Multikey-Compound-Indizes nur soweit Q2 relevant prüfen. |
| FO-02-P07 | Bedingte Indexierung erklären | Partial Indizes sparen nur unter passender Filterbedingung Schlüssel und Wartung; sie dürfen nur für logisch berechtigte Queries verglichen werden. | Begründet die Eligibility-Regel für I5 und I8. | MongoDB Partial-Index-Dokumentation. | Kontrast. | Themenwechsel → FO-02-P08. | 180 | M-FO-01 | Im Smoke-Test später technische Zulässigkeit gleichartiger Full-/Partial-Key-Patterns bestätigen; hier noch offen markieren. |
| FO-02-P08 | Eindeutigkeit und Lookup einordnen | Für den Detailabruf ist ein eindeutiger fachlicher Schlüssel eine Modellierungs- und Zugriffentscheidung; der Unique-Index wird nicht mit einer allgemeinen `_id`-Empfehlung gleichgesetzt. | Begründet I9 und die konzeptionelle, nicht gemessene `_id`-Alternative. | MongoDB Unique-Index-Dokumentation; Referenzschema. | Fortführung. | Voraussetzung → FO-02-P09. | 150 | keines | Quellen- und Modellbezug für `productId`-Eindeutigkeit präzisieren. |
| FO-02-P09 | Bewertungslogik begrenzen | `totalDocsExamined`, `totalKeysExamined`, `nReturned` und Planstufen beschreiben Struktur; reale wiederholte Queryzeiten ergänzen sie, während Explain-Zeit und Plannerwahl keine direkte Siegerbehauptung erlauben. | Sichert die getrennte Bewertung in Kapitel 3/4 methodisch ab. | MongoDB Explain, Query Plans, Hint/Plan Cache. | Synthese. | Folge → FO-02-P10. | 190 | keines | Exakte Dokumentationsstellen zu Explain und Plan Cache erfassen. |
| FO-02-P10 | Kosten und Kriterien synthetisieren | Indexauswahl ist eine Abwägung aus Leseaufwand, Speicher, bedingter Nutzbarkeit und Schreibwartung; daraus folgt das Kriterienraster für die Methode. | Verhindert die Reduktion auf eine einzelne Laufzeit. | MongoDB Write Performance/Indexing Strategies; ggf. workload-Auswahlliteratur. | Synthese. | Übergabe → Kapitel 3. | 140 | M-FO-01 | Methodische Referenz für mehrdimensionale Bewertung auswählen. |

**Summe Zielwörter: 150 + 150 + 220 + 220 + 180 + 170 + 180 + 150 + 190 + 140 = 1.550.**

## Medienplan

| Medium-ID | Typ | Aussagefunktion | Herkunft/Erzeugung | Beschriftung | Einführung und Interpretation |
| --- | --- | --- | --- | --- | --- |
| M-FO-01 | Tabelle | Verknüpft Q1–Q3-Anforderungen mit relevanten Indexmerkmalen, erwarteter Planwirkung und Kosten; ersetzt Wiederholungsprosa. | Eigene Synthese aus später verifizierter Literatur und freigegebenem Design. | „Kriterien zur Ableitung und Bewertung der Indexkandidaten“ | Nach FO-02-P04 einführen; in FO-02-P05 bis P10 nur gezielt referenzieren und in Kapitel 3 operationalisieren. |

## Offene Entscheidungen

- Keine Darstellung ausgeschlossener Indexarten außer einer knappen Scope-Erinnerung, falls nötig.
- Der bestehende Grundlagenentwurf wird nur übernommen, wenn er den neuen Absatzfunktionen entspricht; Zielumfang erfordert etwa 60 Wörter Straffung.
- Smoke-Test-Ergebnis und konkrete Kandidatenleistungen gehören nicht in dieses Kapitel.
