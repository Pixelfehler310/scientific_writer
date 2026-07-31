# Kapitelplan: intro Problemstellung und Untersuchungsziel

## Funktion im Gesamtargument

Das Kapitel begrenzt das Praxisproblem auf drei ausgewählte Katalog-Lesezugriffe und begründet, warum eine gemeinsame, workload-basierte Indexkonfiguration untersucht wird. Es formuliert Forschungsfrage, Beitrag und Reichweite, ohne eine Indexentscheidung oder Messbefund vorwegzunehmen.

## Teilfrage und erwartetes Ergebnis

- **Teilfrage:** Rahmt alle vier Teilfragen; beantwortet noch keine davon.
- **Erwartetes Ergebnis:** Ein klarer Untersuchungsauftrag: Kandidaten aus drei Query Shapes ableiten, im gemeinsamen Pool kontrolliert vergleichen, zu einem Kombiset reduzieren und dieses einschließlich begrenzter Speicher- und Schreibkosten gegen die Baseline prüfen.

## Wortbudget

**350 Wörter**

## Voraussetzungen und Übergabe

- **Voraussetzungen:** G1-Brief, G2-Scoping und G2-Gliederung; spätere Quellenprüfung der Storefront-Plausibilität und MongoDB-Grundsätze.
- **Übergabe:** Benennt Query Shape, Feldreihenfolge, Selektivität, Datenmenge sowie Speicher- und Schreibkosten als die in Kapitel 2 zu begründenden Kriterien.

## Absatzplan

| Absatz-ID | Funktion | Kernaussage | Begründung | Evidenzbedarf | Beziehung davor | Beziehung danach | Zielwörter | Medium | Offene Recherche |
| --- | --- | --- | --- | --- | --- | --- | ---: | --- | --- |
| IN-01-P01 | Problemrahmung | Ein Produktkatalog muss mehrere unterschiedliche Lesezugriffe zugleich unterstützen; einzelne, isoliert gute Indizes ergeben daher noch keine begründete Gesamtkonfiguration. | Produktlisten, Tagfilter und Detailabrufe stellen verschiedene Zugriffsanforderungen. | Externe Plausibilisierung ausgewählter Storefront-Zugriffe; keine Häufigkeitsbehauptung. | Einstieg. | Ursache → IN-01-P02. | 85 | keines | Aktuelle, zitierfähige Storefront-Referenz für Filter, Sortierung und Produktzugriff prüfen. |
| IN-01-P02 | Problempräzisierung | Indexeignung hängt von Query Shape, Selektivität, Datenmenge und Indexkosten ab; deshalb wird nicht nach einer abstrakt besten Indexart gesucht. | Filter, Sortierung, Arrayzugriff und fachliche ID stellen unterschiedliche Anforderungen; zusätzliche Indizes kosten Speicher und Schreibarbeit. | MongoDB-Dokumentation zu Indexstrategie und Schreibkosten; theoretische Grundlage. | Fortführung. | Folge → IN-01-P03. | 80 | keines | Belegstellen für Kosten- und Workloadbezug auswählen. |
| IN-01-P03 | Untersuchungsansatz abgrenzen | Die Arbeit untersucht eine feste Produkt-Collection mit drei ausgewählten Query Shapes und leitet daraus Kandidaten sowie eine gemeinsame Ausgangskonfiguration ab. | Begrenzung schützt vor dem Anspruch eines repräsentativen Gesamtshops und macht Messung kontrollierbar. | Eigene Designentscheidung, durch Scoping plausibilisiert. | Konkretisierung. | Synthese → IN-01-P04. | 75 | keines | Keine; exakte Querydefinitionen erst in Kapitel 3 referenzieren. |
| IN-01-P04 | Forschungsfrage und Leseanleitung | Die Haupt- und Teilfragen strukturieren die Ableitung, den Vergleich und die Validierung; Ergebnisreichweite und Kapitelabfolge werden knapp angekündigt. | Lesende benötigen ein prüfbares Versprechen und die Trennung von Theorie, Methode, Befunden und Schlussfolgerung. | Freigegebene Forschungsfrage und Gliederung; keine externe Evidenz. | Synthese. | Themenwechsel → Kapitel 2. | 110 | keines | Keine. |

**Summe Zielwörter: 85 + 80 + 75 + 110 = 350.**

## Medienplan

Keine Medien. Bei 350 Wörtern würde ein Schema keine Information ergänzen; die Query Shapes werden in Kapitel 3 präzise tabellarisch dargestellt.

## Offene Entscheidungen

- KI-gestützte Anwendungsentwicklung bleibt höchstens eine knappe Zusatzmotivation und erhält keinen eigenen Argumentationsabsatz.
- Keine Aussage darüber, wie verbreitet Shops mit bestimmten Kataloggrößen sind.
- Die Einleitung darf weder finale Indizes noch erwartete Leistungsgewinne nennen.
