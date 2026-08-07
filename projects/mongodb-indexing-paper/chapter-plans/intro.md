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

### IN-01-P01 — Praxisrelevanz digitaler Shop-Anwendungen

- **Funktion:** Die aktuelle Relevanz der Untersuchung eröffnen und zum technischen Problem hinführen.
- **Kernaussage:** Digitalisierung senkt Markteintrittshürden und digitale Angebote sind bei Gründungen verbreitet. Der Onlinehandel erreicht einen großen Teil der Bevölkerung. Daher ist die leistungsfähige Bereitstellung eines Produktkatalogs eine relevante technische Aufgabe.
- **Begründung:** Der E-Commerce-Produktkatalog wird als praxisnaher Untersuchungsfall motiviert, bevor die Indexproblematik eingeführt wird.
- **Evidenzbedarf:** aktuelle Gründungs- und Nutzungsdaten für digitale Angebote und Online-Einkäufe.
- **Evidenztyp:** `external`
- **Evidenzstatus:** `verified`
- **Beziehung davor:** Themenbeginn.
- **Beziehung danach:** Konkretisierung — die technische Wirkung und die Kosten von Indizes werden erläutert.
- **Zielwörter:** 105.
- **Medium:** keines.
- **Offene Recherche:** keine.

<details>
<summary>Evidenz und menschliche Prüfung</summary>

#### E-IN-01-P01-01 — Digitale Angebote bei Existenzgründungen

- **Rolle:** `stützt`
- **Vollbeleg / Artefakt:** Metzger, Georg (2026): *Gründungstätigkeit in Deutschland: Trend zum Nebenerwerb verfestigt sich, junge Erwachsene prägen Gründerlandschaft*. KfW-Gründungsmonitor 2026.
- **Link / Projektpfad:** https://www.kfw.de/PDF/Download-Center/Konzernthemen/Research/PDF-Dokumente-Gr%C3%BCndungsmonitor/KfW-Gr%C3%BCndungsmonitor-2026.pdf
- **Ausgabe / Version:** KfW-Gründungsmonitor 2026, Mai 2026.
- **Fundstelle:** Seite 2, Abschnitt „Bedeutung digitaler Angebote steigt weiter“; Seite 7 zur Verteilung digitaler Gründungen auf Handel und Dienstleistungen.
- **Originalauszug / quellennaher Auszug:**

  > Quellennahe Inhaltsnotiz: 44 % der Existenzgründungen des Jahres 2025 hatten digitale Angebote. Die Digitalisierung senkt Markteintrittshürden oder schafft Geschäftsgelegenheiten. Digitale Gründungen finden sich besonders in Handel und Dienstleistungen.

- **Eigene Zusammenfassung:** Die Quelle begründet die aktuelle Relevanz digitaler Geschäftsmodelle und damit die Wahl eines Online-Shop-Szenarios als Praxisrahmen.
- **Grenze und Kontext:** Der Monitor weist keine gesonderte Zahl für E-Commerce-Gründungen aus und erlaubt keine Aussage über das technische Vorwissen der Gründerinnen und Gründer.
- **Reviewentscheidung:** `freigegeben`
- **Menschliche Prüfung:** Simon Sucker, 06.08.2026: am Original geprüft. Die Formulierung bleibt innerhalb der ausgewiesenen Daten und ihrer Reichweite.

#### E-IN-01-P01-02 — Verbreitung des Online-Einkaufs

- **Rolle:** `stützt`
- **Vollbeleg / Artefakt:** Statistisches Bundesamt (Destatis) (2025): *Personen, die das Internet nutzen und online einkaufen nach Geschlecht und Alter*.
- **Link / Projektpfad:** https://www.destatis.de/DE/Themen/Gesellschaft-Umwelt/Einkommen-Konsum-Lebensbedingungen/IT-Nutzung/Tabellen/nutzung-internet-onlinekaeufe-geschlecht-alter-mz-ikt.html
- **Ausgabe / Version:** IKT-Erhebung private Haushalte, Stand 27.11.2025.
- **Fundstelle:** Tabelle „Internetnutzung und Online-Einkäufe von Personen zu privaten Zwecken 2025 nach Geschlecht und Alter“, Zeile „Insgesamt“, Spalte „in den letzten drei Monaten“.
- **Originalauszug / quellennaher Auszug:**

  > Quellennahe Inhaltsnotiz: 70 % der 16- bis 74-jährigen Personen in Deutschland kauften in den drei Monaten vor der Befragung Waren oder Dienstleistungen online.

- **Eigene Zusammenfassung:** Die amtliche Erhebung zeigt die breite Nutzung des Online-Einkaufs und stützt damit die gesellschaftliche Relevanz des gewählten Anwendungsfalls.
- **Grenze und Kontext:** Die Erhebung misst private Online-Einkäufe, nicht die Zahl oder Größe von Online-Shops und auch keine Anforderungen an deren Datenbanken.
- **Reviewentscheidung:** `freigegeben`
- **Menschliche Prüfung:** Simon Sucker, 06.08.2026: am Original geprüft. Die Formulierung übernimmt ausschließlich Anteil, Altersgruppe und Erhebungszeitraum.

</details>

### IN-01-P02 — Nutzen-Kosten-Zielkonflikt vollständiger Indexsets

- **Funktion:** Den technischen Zielkonflikt der Indexauswahl erklären.
- **Kernaussage:** Mehrere für einzelne Queries plausible Indizes ergeben nicht automatisch ein für den gesamten Workload geeignetes Set, weil jeder zusätzliche Index Speicher- und Pflegekosten verursacht.
- **Begründung:** Die Arbeit benötigt einen Mehrzielkonflikt als Ausgangspunkt, nicht nur die Behauptung, Indizes beschleunigten Abfragen.
- **Evidenzbedarf:** allgemeine Indexwirkung und Write-/Speicherkosten.
- **Evidenztyp:** `external`
- **Evidenzstatus:** `verified`
- **Beziehung davor:** Konkretisierung der Praxisrelevanz.
- **Beziehung danach:** Folge — aus dem Zielkonflikt entsteht die konkrete Forschungslücke des Fallbeispiels.
- **Zielwörter:** 105.
- **Medium:** keines.
- **Offene Recherche:** keine.

<details>
<summary>Evidenz und menschliche Prüfung</summary>

#### E-IN-01-P02-01 — Nutzen und Kosten zusätzlicher Indizes

- **Rolle:** `stützt`
- **Vollbeleg / Artefakt:** MongoDB Inc. (o. J.): *Indexes*. MongoDB Database Manual 8.0.
- **Link / Projektpfad:** https://www.mongodb.com/docs/v8.0/indexes/
- **Ausgabe / Version:** MongoDB Database Manual 8.0; abgerufen am 05.08.2026.
- **Fundstelle:** Einleitungsabschnitt „Indexes“, insbesondere Aussagen zu Collection Scan, Scanbegrenzung und Write-Auswirkung; Abschnitt „Details“ zur geordneten Indexstruktur.
- **Originalauszug / quellennaher Auszug:**

  > Quellennahe Inhaltsnotiz: Ohne passenden Index muss MongoDB jedes Dokument prüfen; ein geeigneter Index kann die Zahl geprüfter Dokumente begrenzen. Zusätzliche Indizes belasten Schreiboperationen, weil Inserts auch die Indizes aktualisieren.

- **Eigene Zusammenfassung:** Die Quelle trägt den einleitenden Zielkonflikt: Indizes können Lesezugriffe begrenzen, sind aber keine kostenlose Ergänzung einer Collection.
- **Grenze und Kontext:** Die Dokumentation belegt weder, dass jeder Index jede Query beschleunigt, noch die konkrete Höhe der Kosten im untersuchten Produktkatalog.
- **Reviewentscheidung:** `freigegeben`
- **Menschliche Prüfung:** Simon Sucker, 06.08.2026: am Original geprüft. Aussageumfang, Fundstelle und eigenständige Formulierung bestätigt.

#### E-IN-01-P02-02 — DBMS-übergreifender Nutzen-Kosten-Zielkonflikt

- **Rolle:** `stützt`
- **Vollbeleg / Artefakt:** PostgreSQL Global Development Group (o. J.): *Indexes*. PostgreSQL 18 Documentation, Kapitel 11.
- **Link / Projektpfad:** https://www.postgresql.org/docs/18/indexes.html
- **Ausgabe / Version:** PostgreSQL 18 Documentation; abgerufen am 05.08.2026.
- **Fundstelle:** Einleitung zu Kapitel 11 „Indexes“.
- **Originalauszug / quellennaher Auszug:**

  > Quellennahe Inhaltsnotiz: Indizes können das Auffinden und Abrufen bestimmter Zeilen gegenüber einem Zugriff ohne Index deutlich beschleunigen, verursachen zugleich aber zusätzlichen Aufwand für das Datenbanksystem und sollen daher gezielt eingesetzt werden.

- **Eigene Zusammenfassung:** Eine von MongoDB unabhängige DBMS-Dokumentation bestätigt den allgemeinen Zielkonflikt zwischen beschleunigten Lesezugriffen und zusätzlichem Systemaufwand durch Indizes.
- **Grenze und Kontext:** Die Quelle beschreibt PostgreSQL und belegt weder MongoDB-spezifische Pflegevorgänge noch die quantitative Höhe des Aufwands im Benchmark.
- **Reviewentscheidung:** `freigegeben`
- **Menschliche Prüfung:** Simon Sucker, 06.08.2026: am Original geprüft. Aussageumfang, Fundstelle und eigenständige Formulierung bestätigt.

#### E-IN-01-P02-03 — Indexabhängiger Pflegeaufwand

- **Rolle:** `stützt`
- **Vollbeleg / Artefakt:** MongoDB Inc. (o. J.): *Write Operation Performance*. MongoDB Database Manual 8.0.
- **Link / Projektpfad:** https://www.mongodb.com/docs/v8.0/core/write-performance/
- **Ausgabe / Version:** MongoDB Database Manual 8.0; abgerufen am 05.08.2026.
- **Fundstelle:** Abschnitt „Indexes“.
- **Originalauszug / quellennaher Auszug:**

  > Quellennahe Inhaltsnotiz: Inserts und Deletes ändern die zugehörigen Schlüssel in jedem Index; Updates betreffen abhängig von den geänderten Schlüsseln nur eine Teilmenge der Indizes.

- **Eigene Zusammenfassung:** Die Pflegekosten entstehen auf Ebene des gesamten Indexsets und hängen bei Updates davon ab, welche indexierten Felder betroffen sind.
- **Grenze und Kontext:** Die Quelle quantifiziert den Aufwand nicht und erlaubt keine Vorhersage der Messergebnisse von W1/W2.
- **Reviewentscheidung:** `freigegeben`
- **Menschliche Prüfung:** Simon Sucker, 06.08.2026: am Original geprüft. Aussageumfang, Fundstelle und eigenständige Formulierung bestätigt.

</details>

### IN-01-P03 — MongoDB-Fall und Erkenntnisinteresse

- **Funktion:** Untersuchungsgegenstand, Datenbanksystem und kontrollierte Fallgrenze konkretisieren.
- **Kernaussage:** Die Arbeit untersucht einen kontrollierten Online-Produktkatalog in MongoDB. Der Versuchsaufbau begrenzt die Produktdaten auf einen gemeinsamen Kern, um den Einfluss vollständiger Indexsets auf unterschiedliche Lese- und Schreibzugriffe isoliert zu betrachten.
- **Begründung:** Die Festlegung macht den Untersuchungsrahmen technisch konkret und empirisch beantwortbar, ohne die Wahl verschiedener Datenbanksysteme oder heterogener Datenmodelle zu bewerten.
- **Evidenzbedarf:** Projektentscheidung und Beschreibung des eigenen Artefakts.
- **Evidenztyp:** `own_reasoning`
- **Evidenzstatus:** `not_required`
- **Beziehung davor:** Konkretisierung.
- **Beziehung danach:** Folge — Forschungsfrage und Ziel werden formulierbar.
- **Zielwörter:** 95.
- **Medium:** keines.
- **Offene Recherche:** keine.

### IN-01-P04 — Forschungsfrage und Beitrag

- **Funktion:** Forschungsfrage, Bewertungsdimensionen und erwartete Ergebnisform nennen.
- **Kernaussage:** Die Arbeit vergleicht B, L, W1 und W2 anhand von Query-Antwortzeiten, Indexspeicher und Write-Aufwand. Explain-Daten ergänzen die Messung durch Angaben zu untersuchten Schlüsseln und Dokumenten sowie zum gewählten Zugriffspfad. Aus den Trade-offs wird eine Empfehlung für die Indexauswahl abgeleitet.
- **Begründung:** Leserinnen und Leser müssen vor der Methodik verstehen, welche Dimensionen die Antwort tragen und welche Rolle Explain-Daten dabei spielen.
- **Evidenzbedarf:** Projektentscheidung und Beschreibung des eigenen Messprotokolls.
- **Evidenztyp:** `not_required`
- **Evidenzstatus:** `not_required`
- **Beziehung davor:** Synthese aus Problem und Fall.
- **Beziehung danach:** Einschränkung — Reichweite und Nichtziele folgen.
- **Zielwörter:** 105.
- **Medium:** keines.
- **Offene Recherche:** keine.

### IN-01-P05 — Scope und Kapitelweg

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
