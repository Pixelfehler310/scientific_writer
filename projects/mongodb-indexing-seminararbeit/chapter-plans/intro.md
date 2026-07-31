# Kapitelplan: intro – Problem, Forschungsfrage und Beitrag

## Funktion im Gesamtargument

Das Kapitel motiviert die Auswahl eines gemeinsamen Indexsets statt der isolierten Optimierung einzelner Abfragen. Es grenzt die Untersuchung auf einen festgelegten MongoDB-Workload, einen endlichen Kandidatenraum und drei Kostenperspektiven ein, ohne Ergebnisse oder geeignete Sets vorwegzunehmen.

## Teilfrage und erwartetes Ergebnis

- **Teilfrage:** Rahmt alle vier Teilfragen, beantwortet sie aber noch nicht.
- **Erwartetes Ergebnis:** Ein präzises Untersuchungsversprechen: Ein wiederverwendbarer Evaluator bewertet alle zulässigen Sets eines vorgegebenen Kandidatenraums; eine E-Commerce-Fallstudie prüft anschließend ausgewählte Sets physisch.

## Wortbudget

**350 Wörter**

## Voraussetzungen und Übergabe

- **Voraussetzungen:** freigegebene Forschungsfrage, Scope, Argumentationslinie und Wortbudget aus G2.
- **Übergabe:** Kapitel 2 klärt die Begriffe und Regeln, die zur Formulierung des Auswahlproblems und zur späteren Bewertung benötigt werden.

## Absatzplan

| Absatz-ID | Funktion | Kernaussage | Begründung | Evidenzbedarf | Beziehung davor | Beziehung danach | Zielwörter | Medium | Offene Recherche |
| --- | --- | --- | --- | --- | --- | --- | ---: | --- | --- |
| IN-01-P01 | Praxisproblem eröffnen | Unterschiedliche Lesezugriffe eines Produktkatalogs können verschiedene Indizes begünstigen; die Summe lokal guter Einzelentscheidungen ist deshalb noch keine begründete Workload-Konfiguration. | Führt vom konkreten Anwendungsproblem zum Indexset statt zu einem Katalog von Indexarten. | Literatur: workloadbasierte Indexauswahl; MongoDB-Dokumentation zum Read-/Write-Trade-off. Keine Aussage über reale Shop-Häufigkeiten. | Einstieg. | Ursache → IN-01-P02. | 80 | keines | Belastbare Fundstelle für den allgemeinen Workloadbezug und die Kosten zusätzlicher Indizes bestimmen. |
| IN-01-P02 | Entscheidungskonflikt präzisieren | Ein geeignetes Set muss Leseleistung, Speicherbedarf und Schreibaufwand gemeinsam berücksichtigen und gilt nur für den betrachteten Workload, Kandidatenraum und Messkontext. | Begründet Mehrzielbetrachtung und Aussagegrenzen, bevor die Forschungsfrage gestellt wird. | Literatur: Index Selection Problem und Constraints; technische Primärquelle zu Speicher-/Write-Kosten. | Fortführung. | Folge → IN-01-P03. | 90 | keines | Exakte wissenschaftliche Fundstellen werden in der Tiefenrecherche verifiziert. |
| IN-01-P03 | Forschungsfrage und Vorgehen formulieren | Die Arbeit fragt nach einer nachvollziehbaren Auswahl und empirischen Überprüfung von Indexsets; sie trennt Einzelprofilierung und vollständige Enumeration von der physischen Finalvalidierung ohne `hint()`. | Macht Methode und Prüfidee verständlich, ohne Resultate vorwegzunehmen. | Eigene freigegebene Forschungsfrage und Methodenentscheidung; keine externe Evidenz für Designentscheidungen erforderlich. | Konkretisierung. | Folge → IN-01-P04. | 100 | keines | Keine. |
| IN-01-P04 | Beitrag, Scope und Aufbau ankündigen | Ergebnis sind ein prototypischer Evaluator und eine bedingte Fallstudienempfehlung, nicht ein globaler MongoDB Index Advisor oder ein universell bestes Set. | Schärft Beitrag und Reichweite und führt durch Theorie, Methode, Evaluation und Fazit. | Eigene Scopeentscheidung; ggf. Atlas Performance Advisor nur zur funktionalen Abgrenzung. | Einschränkung und Synthese. | Themenwechsel → Kapitel 2. | 80 | keines | Prüfen, ob die Advisor-Abgrenzung in der Einleitung nötig ist oder ausschließlich in Kapitel 2 bleibt. |

**Summe Zielwörter: 80 + 90 + 100 + 80 = 350.**

## Medienplan

Keine Medien. Die Modellbeziehungen werden in Kapitel 2 und der konkrete Ablauf in Kapitel 3 jeweils einmal dargestellt.

## Offene Entscheidungen

- Die Einleitung nennt keine finalen Indexsets, erwarteten Leistungsgewinne oder vorläufigen Pilotbefunde.
- Der Produktkatalog dient als kontrollierte Fallstudie; seine Größen und Querygewichte werden nicht als repräsentativ für reale Shops bezeichnet.
