# Kapitelplan: intro — Einleitung

## Funktion im Gesamtargument

Die Einleitung führt vom praktischen Problem mehrerer möglicher MongoDB-Indizes zur begrenzten Bewertungsfrage nach vollständigen Indexsets. Sie verspricht genau den Erkenntnisumfang, den Methodik und Evaluation später tragen können.

## Teilfrage und erwartetes Ergebnis

Warum ist ein kontrollierter Vergleich vollständiger Indexsets für einen definierten Produktkatalog-Workload nötig, und welche Art von Antwort liefert die Arbeit? Ergebnis ist eine präzise Forschungsfrage mit Beitrag, Scope und Kapitelweg.

## Wortbudget

Nominal 400 Wörter.

## Voraussetzungen und Übergabe

Voraussetzungen sind der freigegebene Brief und die G2-Gliederung. Das Kapitel übergibt an `foundations` die Frage, welche technischen Mechanismen Lesevorteile und Gegenkosten erklären.

## Absatzplan

> Alle Evidenzblöcke bleiben bis nach G3 leer. Noch keine Quellen, Links oder Auszüge ergänzen.

### IN-01-P01 — Praxisproblem vollständiger Indexsets

- **Funktion:** Problem und Relevanz eröffnen.
- **Kernaussage:** Mehrere für einzelne Queries plausible Indizes ergeben nicht automatisch ein für den gesamten Workload geeignetes Set, weil jeder zusätzliche Index Speicher- und Pflegekosten verursacht.
- **Begründung:** Die Arbeit benötigt einen Mehrzielkonflikt als Ausgangspunkt, nicht nur die Behauptung, Indizes beschleunigten Abfragen.
- **Evidenzbedarf:** allgemeine Indexwirkung und Write-/Speicherkosten.
- **Evidenztyp:** `external`
- **Evidenzstatus:** `planned`
- **Beziehung davor:** Themenbeginn.
- **Beziehung danach:** Folge — aus dem Zielkonflikt entsteht die konkrete Forschungslücke des Fallbeispiels.
- **Zielwörter:** 100.
- **Medium:** keines.
- **Offene Recherche:** geeigneten knappen Primärbeleg bestimmen.

### IN-01-P02 — Fall und Erkenntnislücke

- **Funktion:** Untersuchungsgegenstand und Erkenntnislücke konkretisieren.
- **Kernaussage:** Der E-Commerce-Produktkatalog dient als kontrollierter Fall, in dem wenige vorab definierte vollständige Sets statt isolierter Indextypen oder eines automatischen Advisors verglichen werden.
- **Begründung:** Die Fallgrenze macht die Untersuchung empirisch beantwortbar und grenzt den verworfenen Enumerationsansatz ab.
- **Evidenzbedarf:** Projektentscheidung und Beschreibung des eigenen Artefakts.
- **Evidenztyp:** `own_reasoning`
- **Evidenzstatus:** `not_required`
- **Beziehung davor:** Konkretisierung.
- **Beziehung danach:** Folge — Forschungsfrage und Ziel werden formulierbar.
- **Zielwörter:** 95.
- **Medium:** keines.
- **Offene Recherche:** keine.

### IN-01-P03 — Forschungsfrage und Beitrag

- **Funktion:** Forschungsfrage, Bewertungsdimensionen und erwartete Ergebnisform nennen.
- **Kernaussage:** Die Arbeit bewertet B/L/W1/W2 anhand von Query-Latenz, Explain-Struktur, Indexspeicher und Write-Aufwand und leitet daraus eine bedingte Empfehlung ab.
- **Begründung:** Leserinnen und Leser müssen vor der Methodik wissen, welche Beobachtungen die Antwort tragen.
- **Evidenzbedarf:** keine externe Evidenz; wortgleiche Forschungsfrage aus freigegebenem Brief.
- **Evidenztyp:** `not_required`
- **Evidenzstatus:** `not_required`
- **Beziehung davor:** Synthese aus Problem und Fall.
- **Beziehung danach:** Einschränkung — Reichweite und Nichtziele folgen.
- **Zielwörter:** 105.
- **Medium:** keines.
- **Offene Recherche:** keine.

### IN-01-P04 — Scope und Kapitelweg

- **Funktion:** Geltungsbereich begrenzen und Argumentationsweg ankündigen.
- **Kernaussage:** Die Empfehlung gilt nur für Datenmodell, Verteilungen, Query-Varianten, MongoDB-Version und Einzelclient-Umgebung; Grundlagen, Methode, Evaluation und Fazit bilden den Antwortweg.
- **Begründung:** Die Einleitung darf keine Universalität oder Parallel-Lastmessung versprechen.
- **Evidenzbedarf:** Projektentscheidungen.
- **Evidenztyp:** `not_required`
- **Evidenzstatus:** `not_required`
- **Beziehung davor:** Einschränkung.
- **Beziehung danach:** Themenwechsel zu den erklärenden Grundlagen.
- **Zielwörter:** 100.
- **Medium:** keines.
- **Offene Recherche:** keine.

## Medienplan

| Medium-ID | Typ | Aussagefunktion | Herkunft/Erzeugung | Beschriftung | Einführung und Interpretation |
| --- | --- | --- | --- | --- | --- |
| — | — | Kein Medium erforderlich | — | — | — |

## Offene Entscheidungen

Keine.
