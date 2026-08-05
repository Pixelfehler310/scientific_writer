# Kapitelplan: evaluation — Von Szenarioergebnissen zur bedingten Empfehlung

## Funktion im Gesamtargument

Das Kapitel trennt Beobachtung, technische Erklärung, Kostenabwägung und Reichweitenbegrenzung. Eigene empirische Evidenz trägt die Setbewertung; Literatur dient nur zur Erklärung und Begrenzung.

## Teilfrage und erwartetes Ergebnis

Welche Sets wirken je Query-Variante, welche Speicher-/Write-Kosten entstehen und welche Empfehlung folgt unter Lese- beziehungsweise Ressourcenpriorität? Ergebnis ist eine begrenzte Antwort, keine globale Rangliste.

## Wortbudget

1.250 Wörter.

## Voraussetzungen und Übergabe

Voraussetzungen sind ein eingefrorener erfolgreicher Referenzlauf, übereinstimmende Ergebnissignaturen und vollständige Manifeste. Das Kapitel übergibt an `conclusion` die expliziten Antwortbausteine und Grenzen.

## Absatzplan

### EV-04-P01 — Gültigkeit des Referenzlaufs

- **Funktion:** Bevor Ergebnisse erscheinen, Datengrundlage und Validierungsstatus feststellen.
- **Kernaussage:** Ausgewertet wird genau der manifestierte Lauf mit dokumentierter Version, Konfiguration und fachlich gleichen Ergebnissen.
- **Begründung:** Ohne Gültigkeitsnachweis wären Performanceunterschiede nicht interpretierbar.
- **Evidenzbedarf:** internes Manifest, Logs und Signaturen.
- **Evidenztyp:** `internal`
- **Evidenzstatus:** `ready`
- **Beziehung davor:** Übernahme des Methodenprotokolls.
- **Beziehung danach:** Folge — scenario-lokale Ergebnisse dürfen berichtet werden.
- **Zielwörter:** 110.
- **Medium:** keine.
- **Offene Recherche:** keine; Referenzlauf und Vollständigkeitsprüfung liegen vor.

<details>
<summary>Evidenz und menschliche Prüfung</summary>

#### E-EV-04-P01-01 — Gültigkeit und Vollständigkeit des Referenzlaufs

- **Rolle:** `interne Evidenz`
- **Vollbeleg / Artefakt:** Full-Run `2026-08-05_210546_567224_full_b24e5dfb`, Manifest, Laufprotokoll und vollständige Metrikartefakte.
- **Link / Projektpfad:** D:/projects/uni/mongodb_indexing/artifacts/runs/2026-08-05_210546_567224_full_b24e5dfb/manifest.json; D:/projects/uni/mongodb_indexing/artifacts/runs/2026-08-05_210546_567224_full_b24e5dfb/logs/run.log; D:/projects/uni/mongodb_indexing/artifacts/runs/2026-08-05_210546_567224_full_b24e5dfb/metrics
- **Ausgabe / Version:** MongoDB 8.2.11; Benchmark-Commit `7752eed9083f63833c907cabd4ea33b381ed19bc`; Python 3.14.5; Full-Profil vom 05.08.2026.
- **Fundstelle:** Manifestfelder `status`, `git_commit_if_available`, `git_dirty_if_available`, `environment`, `scales`, `repetitions`, `write_repetitions`, `warnings` und `analysis_warnings`; Zeilenzählung der CSV-Artefakte.
- **Originalauszug / quellennaher Auszug:**

  > Interne Inhaltsnotiz: Der Lauf endete mit `status: success`, sauberem Git-Stand und ohne Lauf- oder Analysewarnung. Vorhanden sind 2.520 Read-Zeilen in 84 vollständigen Zellen mit je 30 Wiederholungen, 360 Write-Zeilen in 36 Zellen mit je zehn Wiederholungen, 84 Explain-Dateien und 33 Indexbeobachtungen. Alle Write-Batches betrafen die vorgesehenen 100 Dokumente; die Ergebnisanzahlen stimmten zwischen den Sets überein.

- **Eigene Zusammenfassung:** Der manifestierte Referenzlauf erfüllt den eingefrorenen Artefaktvertrag vollständig und kann als Datengrundlage der Evaluation verwendet werden.
- **Grenze und Kontext:** Der erfolgreiche interne Konsistenzcheck schließt Implementierungs- oder Messfehler nicht grundsätzlich aus und ersetzt keine externe Replikation.
- **Reviewentscheidung:** `offen`
- **Menschliche Prüfung:** ausstehend

</details>

### EV-04-P02 — Q1: Bereichsbreite und Sortierung

- **Funktion:** Q1-eng/breit zunächst beschreibend vergleichen.
- **Kernaussage:** Unterschiede zwischen Sets werden anhand Median/IQR und Scan-/Sortierstruktur für beide Bereichsbreiten getrennt berichtet.
- **Begründung:** Zusammenfassung würde den Selektivitätseffekt verdecken.
- **Evidenzbedarf:** eigene Q1-Metriken und Explain-Artefakte.
- **Evidenztyp:** `internal`
- **Evidenzstatus:** `ready`
- **Beziehung davor:** Folge.
- **Beziehung danach:** Fortführung mit anderer Selektivitätsform in Q2.
- **Zielwörter:** 160.
- **Medium:** Teil von EV-M01.
- **Offene Recherche:** keine; Ergebnisrichtung ist durch den Referenzlauf beobachtet.

<details>
<summary>Evidenz und menschliche Prüfung</summary>

#### E-EV-04-P02-01 — Q1-Metriken und Planstruktur

- **Rolle:** `interne Evidenz`
- **Vollbeleg / Artefakt:** Full-Run `2026-08-05_210546_567224_full_b24e5dfb`, Read-Zusammenfassung und Q1-Explain-Dokumente.
- **Link / Projektpfad:** D:/projects/uni/mongodb_indexing/artifacts/runs/2026-08-05_210546_567224_full_b24e5dfb/metrics/summary_by_scenario.csv; D:/projects/uni/mongodb_indexing/artifacts/runs/2026-08-05_210546_567224_full_b24e5dfb/raw_explain/medium/q1_narrow; D:/projects/uni/mongodb_indexing/artifacts/runs/2026-08-05_210546_567224_full_b24e5dfb/raw_explain/medium/q1_broad
- **Ausgabe / Version:** Full-Profil, 30 Wiederholungen je Zelle, 100.000-Dokument-Stufe.
- **Fundstelle:** Zeilen `medium/q1_narrow/*` und `medium/q1_broad/*` in `summary_by_scenario.csv`.
- **Originalauszug / quellennaher Auszug:**

  > Interne Inhaltsnotiz: B untersuchte bei Q1-eng und Q1-breit jeweils 100.000 Dokumente, verwendete keinen Indexschlüssel und zeigte in allen Explain-Auswertungen eine Sortierstufe; die Medianlatenzen betrugen 18,459 beziehungsweise 19,757 ms. L, W1 und W2 untersuchten jeweils 24 Dokumente und 24 Schlüssel ohne Sortierstufe. Ihre Mediane lagen bei Q1-eng zwischen 0,879 und 1,165 ms und bei Q1-breit zwischen 0,935 und 1,182 ms; W2 hatte in beiden Varianten den niedrigsten beobachteten Median.

- **Eigene Zusammenfassung:** Die drei Q1-geeigneten Sets begrenzen Scan- und Sortierarbeit deutlich gegenüber B; Unterschiede innerhalb dieser drei Sets sind zeitlich klein und müssen zusammen mit IQR und Versuchsgrenzen berichtet werden.
- **Grenze und Kontext:** Die niedrigsten Mediane belegen keine statistisch gesicherte Überlegenheit von W2; gemessen wurden warme Einzelclient-Zugriffe auf die erste Ergebnisseite.
- **Reviewentscheidung:** `offen`
- **Menschliche Prüfung:** ausstehend

</details>

### EV-04-P03 — Q2: häufiges und seltenes Tag

- **Funktion:** Multikey-Wirkung über zwei Taghäufigkeiten vergleichen.
- **Kernaussage:** Q2-häufig/selten zeigen getrennt, wie Tagverteilung und zusätzliche Aktivitätsprüfung Suchaufwand und Latenz beeinflussen.
- **Begründung:** Q2 unterscheidet L/W1 und das kompaktere W2 besonders über Schlüsselumfang und residuale Prüfung.
- **Evidenzbedarf:** eigene Q2-Metriken, Tagzählungen und Explain-Pläne.
- **Evidenztyp:** `internal`
- **Evidenzstatus:** `ready`
- **Beziehung davor:** Fortführung.
- **Beziehung danach:** Kontrast zur Spezialindex-/Wiederverwendungsfrage in Q3.
- **Zielwörter:** 155.
- **Medium:** Teil von EV-M01.
- **Offene Recherche:** keine.

<details>
<summary>Evidenz und menschliche Prüfung</summary>

#### E-EV-04-P03-01 — Q2-Metriken und Multikey-Arbeit

- **Rolle:** `interne Evidenz`
- **Vollbeleg / Artefakt:** Full-Run `2026-08-05_210546_567224_full_b24e5dfb`, Q2-Zusammenfassung, Seed-Verteilungen und Explain-Dokumente.
- **Link / Projektpfad:** D:/projects/uni/mongodb_indexing/artifacts/runs/2026-08-05_210546_567224_full_b24e5dfb/metrics/summary_by_scenario.csv; D:/projects/uni/mongodb_indexing/artifacts/runs/2026-08-05_210546_567224_full_b24e5dfb/manifest.json; D:/projects/uni/mongodb_indexing/artifacts/runs/2026-08-05_210546_567224_full_b24e5dfb/raw_explain/medium
- **Ausgabe / Version:** Full-Profil, 30 Wiederholungen je Zelle, 100.000-Dokument-Stufe.
- **Fundstelle:** Zeilen `medium/q2_common/*` und `medium/q2_rare/*`; Manifestabschnitt `seed_summaries.medium.tag_counts`.
- **Originalauszug / quellennaher Auszug:**

  > Interne Inhaltsnotiz: Bei Q2-häufig lagen die Mediane aller Sets zwischen 0,845 und 0,928 ms, bei Q2-selten zwischen 0,795 und 0,824 ms. L und W1 untersuchten jeweils 24 Dokumente und Schlüssel; W2 benötigte 34 bei Q2-häufig und 30 bei Q2-selten. B verwendete wegen des frühen Limits einen Collection Scan über 59 beziehungsweise 145 Dokumente. Keine Variante zeigte eine Sortierstufe.

- **Eigene Zusammenfassung:** Die Multikey-Indizes reduzieren die strukturelle Dokumentarbeit, erzeugen in diesem kleinen, limitierten Ergebnisfenster jedoch keinen entsprechend klaren Latenzvorsprung gegenüber dem früh abbrechenden Collection Scan.
- **Grenze und Kontext:** Das Resultat gilt für die konkreten Tagverteilungen, `isActive`-Bedingung und `limit: 24`; es erlaubt keine allgemeine Aussage gegen Multikey-Indizes.
- **Reviewentscheidung:** `offen`
- **Menschliche Prüfung:** ausstehend

</details>

### EV-04-P04 — Q3 und Q4: Spezialindex, Wiederverwendung und Kontrolle

- **Funktion:** Q3-Designkontrast erklären und Q4 knapp als Kontrolle dokumentieren.
- **Kernaussage:** Q3 prüft, ob der Spezialindex von L gegenüber W1/W2 einen materiellen Vorteil bietet; Q4 bestätigt die gemeinsame Unique-Basis und entscheidet die Empfehlung nicht.
- **Begründung:** Dies ist der Kernvergleich query-lokal versus workloadorientiert.
- **Evidenzbedarf:** eigene Q3-/Q4-Metriken und Pläne.
- **Evidenztyp:** `internal`
- **Evidenzstatus:** `ready`
- **Beziehung davor:** Kontrast.
- **Beziehung danach:** Synthese der Leseergebnisse über Skalen.
- **Zielwörter:** 165.
- **Medium:** Teil von EV-M01.
- **Offene Recherche:** keine.

<details>
<summary>Evidenz und menschliche Prüfung</summary>

#### E-EV-04-P04-01 — Q3-Spezialindex und gemeinsame Q4-Basis

- **Rolle:** `interne Evidenz`
- **Vollbeleg / Artefakt:** Full-Run `2026-08-05_210546_567224_full_b24e5dfb`, Q3-/Q4-Zusammenfassung und Explain-Dokumente.
- **Link / Projektpfad:** D:/projects/uni/mongodb_indexing/artifacts/runs/2026-08-05_210546_567224_full_b24e5dfb/metrics/summary_by_scenario.csv; D:/projects/uni/mongodb_indexing/artifacts/runs/2026-08-05_210546_567224_full_b24e5dfb/raw_explain/medium/q3_common; D:/projects/uni/mongodb_indexing/artifacts/runs/2026-08-05_210546_567224_full_b24e5dfb/raw_explain/medium/q3_rare; D:/projects/uni/mongodb_indexing/artifacts/runs/2026-08-05_210546_567224_full_b24e5dfb/raw_explain/medium/q4_product_lookup
- **Ausgabe / Version:** Full-Profil, 30 Wiederholungen je Zelle, 100.000-Dokument-Stufe.
- **Fundstelle:** Zeilen `medium/q3_common/*`, `medium/q3_rare/*` und `medium/q4_product_lookup/*`.
- **Originalauszug / quellennaher Auszug:**

  > Interne Inhaltsnotiz: Bei Q3-häufig untersuchte L 24 Dokumente/Schlüssel gegenüber 90 bei W1/W2; die Mediane lagen dennoch bei 1,192 ms für L, 1,077 ms für W1 und 1,017 ms für W2. Bei Q3-selten untersuchte L 24 statt 397 Dokumente/Schlüssel und erreichte 1,120 ms gegenüber 1,680 ms für W1 und 1,587 ms für W2. B scannte in beiden Varianten 100.000 Dokumente mit Sortierung und benötigte 18,202 beziehungsweise 17,134 ms. Q4 untersuchte bei allen Sets genau ein Dokument und einen Schlüssel; die Mediane lagen eng zwischen 0,708 und 0,742 ms.

- **Eigene Zusammenfassung:** Der Q3-Spezialindex von L zeigt den klarsten strukturellen und zeitlichen Vorteil bei der seltenen Marke; bei der häufigen Marke ist seine geringere Scanarbeit nicht mit dem niedrigsten Median verbunden. Q4 bestätigt erwartungsgemäß die gemeinsame Unique-Basis.
- **Grenze und Kontext:** Kleine Zeitunterschiede bei Q3-häufig und Q4 dürfen nicht als robuste Rangfolge interpretiert werden; die strukturellen Unterschiede sind davon getrennt zu berichten.
- **Reviewentscheidung:** `offen`
- **Menschliche Prüfung:** ausstehend

</details>

### EV-04-P05 — Skalierung und strukturelle Erklärung

- **Funktion:** Ergebnisse über 1.000/10.000/100.000 Dokumente zusammenführen, ohne die Smoke-Stufe gleichzugewichten.
- **Kernaussage:** Die regulären Skalen zeigen, ob absolute Sucharbeit und Latenzunterschiede mit der Datenmenge plausibel wachsen; Planstruktur erklärt, aber ersetzt die Latenz nicht.
- **Begründung:** Einzelwerte einer Größe reichen für die begrenzte Skalierungsaussage nicht.
- **Evidenzbedarf:** eigene skalierte Metriken; Grundlagen zur Selektivität und Explain-Grenze.
- **Evidenztyp:** `internal`
- **Evidenzstatus:** `ready`
- **Beziehung davor:** Synthese.
- **Beziehung danach:** Kontrast — Lesevorteile werden den Kosten gegenübergestellt.
- **Zielwörter:** 150.
- **Medium:** EV-M02.
- **Offene Recherche:** keine.

<details>
<summary>Evidenz und menschliche Prüfung</summary>

#### E-EV-04-P05-01 — Skalierung über drei Bestandsgrößen

- **Rolle:** `interne Evidenz`
- **Vollbeleg / Artefakt:** Full-Run `2026-08-05_210546_567224_full_b24e5dfb`, skalierte Read-Zusammenfassung und Übersichtsgrafik.
- **Link / Projektpfad:** D:/projects/uni/mongodb_indexing/artifacts/runs/2026-08-05_210546_567224_full_b24e5dfb/metrics/summary_by_scenario.csv; D:/projects/uni/mongodb_indexing/artifacts/runs/2026-08-05_210546_567224_full_b24e5dfb/charts/laufzeit_uebersicht.png
- **Ausgabe / Version:** Full-Profil mit 1.000, 10.000 und 100.000 Dokumenten sowie je 30 Read-Wiederholungen.
- **Fundstelle:** Sämtliche Skalenzeilen für Q1–Q4 in `summary_by_scenario.csv`.
- **Originalauszug / quellennaher Auszug:**

  > Interne Inhaltsnotiz: Bei B wächst die untersuchte Dokumentzahl für Q1 und Q3 von 1.000 über 10.000 auf 100.000; die zugehörigen Mediane steigen von etwa 0,9–1,0 ms über 2,3–2,6 ms auf 17,1–19,8 ms. Die indexgestützten Q1-Varianten bleiben bei höchstens 24 untersuchten Dokumenten/Schlüsseln und unter 1,2 ms Median; Q3-selten erreicht bei W1/W2 auf der größten Stufe 397 untersuchte Schlüssel und 1,59–1,68 ms. Q2 und Q4 bleiben über die drei Stufen zeitlich eng beieinander.

- **Eigene Zusammenfassung:** Die Skalen stützen eine plausible Verbindung zwischen wachsender Scanarbeit und Latenz bei den vollständigen Scans; begrenzte Indexarbeit hält die beobachtete Latenz im untersuchten Bereich deutlich stabiler.
- **Grenze und Kontext:** Drei lokale Skalen belegen kein asymptotisches Gesetz. Cachezustand, erste Ergebnisseite und feste Verteilungen begrenzen die Skalierungsaussage.
- **Reviewentscheidung:** `offen`
- **Menschliche Prüfung:** ausstehend

</details>

### EV-04-P06 — Indexspeicher und Write-Batches

- **Funktion:** Gegenkosten der Leseunterstützung darstellen.
- **Kernaussage:** Setgröße, W1-Insert, W2a-Stock und W2b-Preis werden getrennt verglichen, weil nicht indexierte und indexierte Updates unterschiedliche Pflegearbeit auslösen können.
- **Begründung:** Ohne diese Dimensionen wäre „geeignet“ auf Lesen verkürzt.
- **Evidenzbedarf:** eigene Indexgrößen- und Write-Artefakte; Literatur nur zur Erklärung.
- **Evidenztyp:** `internal`
- **Evidenzstatus:** `ready`
- **Beziehung davor:** Kontrast.
- **Beziehung danach:** Synthese zur bedingten Auswahl.
- **Zielwörter:** 170.
- **Medium:** EV-M03.
- **Offene Recherche:** keine.

<details>
<summary>Evidenz und menschliche Prüfung</summary>

#### E-EV-04-P06-01 — Speicher- und Write-Kosten der vollständigen Sets

- **Rolle:** `interne Evidenz`
- **Vollbeleg / Artefakt:** Full-Run `2026-08-05_210546_567224_full_b24e5dfb`, Indexbeobachtungen und Write-Zusammenfassung.
- **Link / Projektpfad:** D:/projects/uni/mongodb_indexing/artifacts/runs/2026-08-05_210546_567224_full_b24e5dfb/metrics/index_observations.csv; D:/projects/uni/mongodb_indexing/artifacts/runs/2026-08-05_210546_567224_full_b24e5dfb/metrics/write_summary.csv; D:/projects/uni/mongodb_indexing/artifacts/runs/2026-08-05_210546_567224_full_b24e5dfb/metrics/write_results.csv
- **Ausgabe / Version:** Full-Profil, 100.000-Dokument-Stufe, zehn Wiederholungen je Write-Zelle und Batches zu je 100 Dokumenten.
- **Fundstelle:** Set-Gesamtgrößen für `medium`; Zeilen `medium/*/*` in `write_summary.csv`.
- **Originalauszug / quellennaher Auszug:**

  > Interne Inhaltsnotiz: Der Gesamtindexspeicher beträgt bei 100.000 Dokumenten 5.036 KiB für B, 10.240 KiB für L, 8.396 KiB für W1 und 8.072 KiB für W2. Beim Insert-Batch liegen die Mediane bei 3,406 ms (B), 10,466 ms (L), 7,367 ms (W1) und 6,436 ms (W2). Beim Stock-Update ist W1 mit 3,108 ms der niedrigste beobachtete Median, während die vier Werte zwischen 3,108 und 3,443 ms liegen. Beim Preis-Update erreicht B 3,049 ms gegenüber 9,559 ms (L), 5,890 ms (W1) und 5,967 ms (W2).

- **Eigene Zusammenfassung:** B ist bei Speicher, Inserts und Änderungen des indexierten Preisfelds am günstigsten; W2 reduziert Speicher und Insert-Aufwand gegenüber L/W1, während das nicht indexierte Stock-Update keine stabile Rangfolge aus den kleinen Medianunterschieden rechtfertigt.
- **Grenze und Kontext:** Gemessen wurden lokale Batches ohne konkurrierende Clients; Reset, Seed und Indexaufbau liegen außerhalb des Write-Zeitfensters.
- **Reviewentscheidung:** `offen`
- **Menschliche Prüfung:** ausstehend

</details>

### EV-04-P07 — Dominanz und bedingte Empfehlung

- **Funktion:** Forschungsfrage anhand der vorab festgelegten Entscheidungslogik beantworten.
- **Kernaussage:** Nicht dominierte Sets werden ohne Gesamtscore für Lese- und Ressourcenpriorität bewertet; bei echtem Zielkonflikt bleiben zwei bedingte Empfehlungen zulässig.
- **Begründung:** Ein erzwungener Sieger würde unbegründete Gewichte verstecken.
- **Evidenzbedarf:** eigene zusammengeführte Messwerte und freigegebene Entscheidungsregel.
- **Evidenztyp:** `internal`
- **Evidenzstatus:** `ready`
- **Beziehung davor:** Synthese.
- **Beziehung danach:** Einschränkung durch Validitätsgrenzen.
- **Zielwörter:** 175.
- **Medium:** keine zusätzliche Ranglistengrafik; vorhandene Medien referenzieren.
- **Offene Recherche:** keine; Formulierung bleibt bis zur menschlichen Ergebnisprüfung vorläufig.

<details>
<summary>Evidenz und menschliche Prüfung</summary>

#### E-EV-04-P07-01 — Mehrdimensionale Bewertung ohne Gesamtscore

- **Rolle:** `interne Evidenz und begrenzte Ableitung`
- **Vollbeleg / Artefakt:** Full-Run `2026-08-05_210546_567224_full_b24e5dfb`, Vergleichsbewertung sowie Read-, Write- und Speicherzusammenfassungen.
- **Link / Projektpfad:** D:/projects/uni/mongodb_indexing/artifacts/runs/2026-08-05_210546_567224_full_b24e5dfb/analysis/comparison_assessment.json; D:/projects/uni/mongodb_indexing/artifacts/runs/2026-08-05_210546_567224_full_b24e5dfb/metrics/summary_by_scenario.csv; D:/projects/uni/mongodb_indexing/artifacts/runs/2026-08-05_210546_567224_full_b24e5dfb/metrics/write_summary.csv
- **Ausgabe / Version:** Auswertung des Full-Profils vom 05.08.2026 nach der vorab implementierten Entscheidungslogik in Commit `7752eed`.
- **Fundstelle:** Vergleichseinträge `READ-*`, `STORAGE`, `WRITE-*`, `DOMINANCE`, `RECOMMEND-READ` und `RECOMMEND-RESOURCE`.
- **Originalauszug / quellennaher Auszug:**

  > Interne Inhaltsnotiz: Kein Set ist über alle erhobenen Dimensionen Pareto-dominant. Die niedrigsten Mediane auf der größten Stufe verteilen sich bei Reads auf W2 (Q1-eng, Q1-breit, Q3-häufig), B (beide Q2-Varianten) und L (Q3-selten, Q4). Bei Ressourcen und Writes führt B bei Speicher, Insert und Preis-Update; W1 hat beim Stock-Update den niedrigsten beobachteten Median.

- **Eigene Zusammenfassung:** Es gibt keinen empirisch begründbaren Gesamtsieger. Eine Empfehlung muss benennen, welche Query-Szenarien beziehungsweise Speicher- und Write-Dimensionen im konkreten Workload priorisiert werden.
- **Grenze und Kontext:** Die automatische Bewertung nennt lokale Medianführer, aber keine Signifikanz und keine Workloadgewichte; sehr kleine Differenzen dürfen nicht zu einer scheinbar eindeutigen Setrangfolge verdichtet werden.
- **Reviewentscheidung:** `offen`
- **Menschliche Prüfung:** ausstehend

</details>

### EV-04-P08 — Limitationen und Übertragbarkeit

- **Funktion:** Reichweite der Empfehlung begrenzen und alternative Erklärungen sichtbar machen.
- **Kernaussage:** Synthetische Verteilungen, erste Ergebnisseite, Einzelclient, begrenzte Sets, lokale Umgebung und konkrete MongoDB-Version verhindern eine Übertragung der konkreten Rangfolge auf andere Systeme.
- **Begründung:** Die Methode ist übertragbarer als das Messergebnis.
- **Evidenzbedarf:** eigene Designgrenzen; gegebenenfalls Benchmarkmethodik.
- **Evidenztyp:** `internal`
- **Evidenzstatus:** `ready`
- **Beziehung davor:** Einschränkung.
- **Beziehung danach:** Übergabe an die knappe Schlussantwort.
- **Zielwörter:** 165.
- **Medium:** keines.
- **Offene Recherche:** keine Laufartefakt-Warnungen; bekannte Designgrenzen vollständig aufführen.

<details>
<summary>Evidenz und menschliche Prüfung</summary>

#### E-EV-04-P08-01 — Reichweite des Referenzlaufs

- **Rolle:** `begrenzt`
- **Vollbeleg / Artefakt:** Manifest und eingefrorene Profil-/Scenario-Registry des Full-Runs `2026-08-05_210546_567224_full_b24e5dfb`.
- **Link / Projektpfad:** D:/projects/uni/mongodb_indexing/artifacts/runs/2026-08-05_210546_567224_full_b24e5dfb/manifest.json; D:/projects/uni/mongodb_indexing/src/mongodb_indexing_lab/benchmark/profiles.py; D:/projects/uni/mongodb_indexing/src/mongodb_indexing_lab/scenarios/queries.py
- **Ausgabe / Version:** MongoDB 8.2.11; Benchmark-Commit `7752eed`; lokale Windows-/Docker-Umgebung mit 2 CPUs und 2.048 MB deklariertem Limit.
- **Fundstelle:** Manifestfelder `environment`, `config`, `scales`, `scenarios`, `strategies`, `warmups` und `repetitions`; Registrydefinitionen des Full-Profils.
- **Originalauszug / quellennaher Auszug:**

  > Interne Inhaltsnotiz: Der Lauf verwendet synthetisch und deterministisch erzeugte Daten, drei Größen bis 100.000 Dokumente, sieben feste Read-Szenarien mit erster Ergebnisseite, drei Write-Batches, vier vorab ausgewählte Sets, einen lokalen Client und eine konkrete MongoDB-Patchversion. Es wurden keine Lauf- oder Analysewarnungen protokolliert.

- **Eigene Zusammenfassung:** Die konkrete Rangfolge ist auf diesen Versuchsraum begrenzt; übertragbar ist primär das kontrollierte Vergleichsverfahren mit getrennten Messdimensionen.
- **Grenze und Kontext:** Nicht untersucht sind reale Produktionsverteilungen, tiefe Pagination, parallele Last, andere Hardware-/Cachezustände, weitere Indexsets und andere MongoDB-Versionen.
- **Reviewentscheidung:** `offen`
- **Menschliche Prüfung:** ausstehend

</details>

## Medienplan

| Medium-ID | Typ | Aussagefunktion | Herkunft/Erzeugung | Beschriftung | Einführung und Interpretation |
| --- | --- | --- | --- | --- | --- |
| EV-M01 | kompakte Ergebnistabelle | Median/IQR und Planindikatoren je Q1–Q3-Variante und Set zeigen | `metrics/summary_by_scenario.csv` des Referenzlaufs | „Leseergebnisse nach Query-Variante“ | In EV-04-P02 einführen, in P03/P04 abschnittsweise interpretieren |
| EV-M02 | Small-Multiples-Skalierungsgrafik | Mediane über die drei Bestandsgrößen vergleichen | `charts/laufzeit_uebersicht.png` des Referenzlaufs | „Ausführungszeit nach Datenmenge und Indexset“ | In EV-04-P05 einführen; wegen unterschiedlicher Achsenskalierung und nur drei Stützstellen begrenzen |
| EV-M03 | kompakte Trade-off-Tabelle | Indexspeicher sowie W1/W2a/W2b ohne Gesamtscore gegenüberstellen | `metrics/index_observations.csv` und `metrics/write_summary.csv` des Referenzlaufs | „Speicher- und Write-Kosten der vollständigen Sets“ | In EV-04-P06 einführen; P07 verwendet sie für die bedingte Auswahl |

## Offene Entscheidungen

- Die drei Medienformen sind nach Sichtung des Referenzlaufs festgelegt; beim Schreiben nur tatsächlich argumentativ benötigte Spalten übernehmen.
- Fehlende oder ungültige Messzellen dürfen nicht imputiert oder still ausgeschlossen werden.
