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
- **Evidenzstatus:** `ready`
- **Beziehung davor:** Themenbeginn.
- **Beziehung danach:** Folge — aus dem Zielkonflikt entsteht die konkrete Forschungslücke des Fallbeispiels.
- **Zielwörter:** 100.
- **Medium:** keines.
- **Offene Recherche:** geeigneten knappen Primärbeleg bestimmen.

<details>
<summary>Evidenz und menschliche Prüfung</summary>

#### E-IN-01-P01-01 — Nutzen und Kosten zusätzlicher Indizes

- **Rolle:** `stützt`
- **Vollbeleg / Artefakt:** MongoDB Inc. (o. J.): *Indexes*. MongoDB Database Manual 8.0.
- **Link / Projektpfad:** https://www.mongodb.com/docs/v8.0/indexes/
- **Ausgabe / Version:** MongoDB Database Manual 8.0; abgerufen am 05.08.2026.
- **Fundstelle:** Einleitungsabschnitt „Indexes“, insbesondere Aussagen zu Collection Scan, Scanbegrenzung und Write-Auswirkung; Abschnitt „Details“ zur geordneten Indexstruktur.
- **Originalauszug / quellennaher Auszug:**

  > Quellennahe Inhaltsnotiz: Ohne passenden Index muss MongoDB jedes Dokument prüfen; ein geeigneter Index kann die Zahl geprüfter Dokumente begrenzen. Zusätzliche Indizes belasten Schreiboperationen, weil Inserts auch die Indizes aktualisieren.

- **Eigene Zusammenfassung:** Die Quelle trägt den einleitenden Zielkonflikt: Indizes können Lesezugriffe begrenzen, sind aber keine kostenlose Ergänzung einer Collection.
- **Grenze und Kontext:** Die Dokumentation belegt weder, dass jeder Index jede Query beschleunigt, noch die konkrete Höhe der Kosten im untersuchten Produktkatalog.
- **Reviewentscheidung:** `offen`
- **Menschliche Prüfung:** ausstehend

#### E-IN-01-P01-02 — DBMS-übergreifender Nutzen-Kosten-Zielkonflikt

- **Rolle:** `stützt`
- **Vollbeleg / Artefakt:** PostgreSQL Global Development Group (o. J.): *Indexes*. PostgreSQL 18 Documentation, Kapitel 11.
- **Link / Projektpfad:** https://www.postgresql.org/docs/18/indexes.html
- **Ausgabe / Version:** PostgreSQL 18 Documentation; abgerufen am 05.08.2026.
- **Fundstelle:** Einleitung zu Kapitel 11 „Indexes“.
- **Originalauszug / quellennaher Auszug:**

  > Quellennahe Inhaltsnotiz: Indizes können das Auffinden und Abrufen bestimmter Zeilen gegenüber einem Zugriff ohne Index deutlich beschleunigen, verursachen zugleich aber zusätzlichen Aufwand für das Datenbanksystem und sollen daher gezielt eingesetzt werden.

- **Eigene Zusammenfassung:** Eine von MongoDB unabhängige DBMS-Dokumentation bestätigt den allgemeinen Zielkonflikt zwischen beschleunigten Lesezugriffen und zusätzlichem Systemaufwand durch Indizes.
- **Grenze und Kontext:** Die Quelle beschreibt PostgreSQL und belegt weder MongoDB-spezifische Pflegevorgänge noch die quantitative Höhe des Aufwands im Benchmark.
- **Reviewentscheidung:** `offen`
- **Menschliche Prüfung:** ausstehend

#### E-IN-01-P01-03 — Indexabhängiger Pflegeaufwand

- **Rolle:** `stützt`
- **Vollbeleg / Artefakt:** MongoDB Inc. (o. J.): *Write Operation Performance*. MongoDB Database Manual 8.0.
- **Link / Projektpfad:** https://www.mongodb.com/docs/v8.0/core/write-performance/
- **Ausgabe / Version:** MongoDB Database Manual 8.0; abgerufen am 05.08.2026.
- **Fundstelle:** Abschnitt „Indexes“.
- **Originalauszug / quellennaher Auszug:**

  > Quellennahe Inhaltsnotiz: Inserts und Deletes ändern die zugehörigen Schlüssel in jedem Index; Updates betreffen abhängig von den geänderten Schlüsseln nur eine Teilmenge der Indizes.

- **Eigene Zusammenfassung:** Die Pflegekosten entstehen auf Ebene des gesamten Indexsets und hängen bei Updates davon ab, welche indexierten Felder betroffen sind.
- **Grenze und Kontext:** Die Quelle quantifiziert den Aufwand nicht und erlaubt keine Vorhersage der Messergebnisse von W1/W2.
- **Reviewentscheidung:** `offen`
- **Menschliche Prüfung:** ausstehend

</details>

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
