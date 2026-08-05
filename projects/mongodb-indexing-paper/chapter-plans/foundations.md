# Kapitelplan: foundations — Warum Indexsets unterschiedlich wirken

## Funktion im Gesamtargument

Das Kapitel liefert nur die Mechanismen, die später zur Begründung der Kandidaten und zur Interpretation von Latenz, Scanaufwand, Speicher und Writes benötigt werden. Vorhandene geeignete Theorieprosa wird bevorzugt wortgleich übernommen, sofern Funktion, Query Shapes und MongoDB-Version passen.

## Teilfrage und erwartetes Ergebnis

Welche Eigenschaften von Dokumentmodell, Query Shape, Selektivität, Indexentwurf und Plananalyse erklären die Unterschiede zwischen B, L, W1 und W2? Ergebnis ist ein kompakter Interpretationsrahmen, kein allgemeines MongoDB-Lehrbuchkapitel.

## Wortbudget

Nominal 1.100 Wörter. Wegen der freigegebenen Bestandsübernahme gilt ein Arbeitskorridor von etwa 1.150 bis 1.400 Wörtern. Der rote Faden hat Vorrang; redundante oder nicht mehr passende Altpassagen werden als ganze Absätze ausgelassen.

## Voraussetzungen und Übergabe

Voraussetzungen sind die finalen Query Shapes und Setdefinitionen aus Scoping/Outline. Das Kapitel übergibt an `method` die begründeten Designregeln und die Bedeutung der Messgrößen.

## Absatzplan

> Alle Evidenzblöcke bleiben bis nach G3 leer. Die Bestandsübernahme bezeichnet nur eine redaktionelle Absicht, keinen neuen Quellen- oder Prüfstatus.

### TH-02-P01 — Dokumentmodell als Zugriffsbasis

- **Funktion:** Dokumentmodell und gemeinsame Ablage knapp einführen.
- **Kernaussage:** Produktattribute können als gemeinsam gelesenes Produktdokument mit eingebetteten Feldern und Arrays modelliert werden. Diese gemeinsame Ablage bildet die Datenbasis, legt aber noch keinen effizienten Zugriffspfad fest.
- **Begründung:** Die spätere Indexdiskussion braucht nur den Zusammenhang zwischen Produktdokument, Arrays/eingebetteten Feldern und Zugriffsmustern.
- **Evidenzbedarf:** Dokumentmodell und gemeinsame Ablage zusammengehöriger Daten.
- **Evidenztyp:** `external`
- **Evidenzstatus:** `ready`
- **Beziehung davor:** Übernahme aus der Fallankündigung der Einleitung.
- **Beziehung danach:** Folge — die späteren Zugriffsmuster bestimmen, welche Felder und Kombinationen relevant werden.
- **Zielwörter:** 140.
- **Medium:** keines.
- **Offene Recherche:** genaue Ausgabe-/Seitenangaben vorhandener Fachbücher prüfen.
- **Bestandsübernahme:** bevorzugt; geeignete vorhandene Absatzblöcke wortgleich samt später zuzuordnender Prüfhistorie.

<details>
<summary>Evidenz und menschliche Prüfung</summary>

#### E-TH-02-P01-01 — Zusammengehörige Daten im Dokumentmodell

- **Rolle:** `stützt`
- **Vollbeleg / Artefakt:** MongoDB Inc. (o. J.): *Embedded Data in Your MongoDB Schema*. MongoDB Database Manual 8.0.
- **Link / Projektpfad:** https://www.mongodb.com/docs/v8.0/data-modeling/embedding/
- **Ausgabe / Version:** MongoDB Database Manual 8.0; abgerufen am 05.08.2026.
- **Fundstelle:** Einleitungsabschnitt und Abschnitt „Use Cases“.
- **Originalauszug / quellennaher Auszug:**

  > Quellennahe Inhaltsnotiz: Eingebettete Dokumente halten zusammengehörige Daten in einer Dokumentstruktur; Arrays und Unterdokumente können dadurch gemeinsam in einer Datenbankoperation gelesen werden.

- **Eigene Zusammenfassung:** Die Quelle stützt das Produktdokument als gemeinsamen Datenverbund und erklärt, warum Arrays und eingebettete Attribute Bestandteil desselben Zugriffsmusters sein können.
- **Grenze und Kontext:** Aus der gemeinsamen Speicherung folgt noch nicht, welcher Index für einen konkreten Workload geeignet ist; Einbettung ist zudem nicht für jedes Beziehungsmodell optimal.
- **Reviewentscheidung:** `übernehmen`
- **Menschliche Prüfung:** verified

#### E-TH-02-P01-02 — Aufbau eines MongoDB-Dokuments

- **Rolle:** `stützt`
- **Vollbeleg / Artefakt:** MongoDB Inc. (o. J.): *Documents*. MongoDB Database Manual 8.0.
- **Link / Projektpfad:** https://www.mongodb.com/docs/v8.0/core/document/
- **Ausgabe / Version:** MongoDB Database Manual 8.0; abgerufen am 05.08.2026.
- **Fundstelle:** Einleitungsabschnitt und Abschnitt „Document Structure“.
- **Originalauszug / quellennaher Auszug:**

  > Quellennahe Inhaltsnotiz: MongoDB speichert Datensätze als BSON-Dokumente aus Feld-Wert-Paaren. Feldwerte können unter anderem weitere Dokumente, Arrays und Arrays von Dokumenten sein.

- **Eigene Zusammenfassung:** Die Dokumentstruktur erlaubt, die im Produktkatalog gemeinsam benötigten skalaren Attribute, eingebetteten Felder und Arrays in einem Produktdokument abzubilden.
- **Grenze und Kontext:** Die mögliche Dokumentstruktur begründet weder eine bestimmte Modellierungsentscheidung noch die Eignung eines konkreten Indexes.
- **Reviewentscheidung:** `übernehmen`
- **Menschliche Prüfung:** verified

</details>

### TH-02-P02 — Query Shapes und Selektivität

- **Funktion:** Query Shape als Ausgangspunkt des Indexentwurfs und Selektivität als Interpretationsgröße einführen.
- **Kernaussage:** Filter, Sortierung und Projektion beschreiben, welche Daten eine Query benötigt; die Selektivität ihrer Bedingungen bestimmt, wie stark sie den Produktbestand eingrenzen.
- **Begründung:** Bevor ein Zugriffspfad oder eine Feldreihenfolge erklärt wird, muss klar sein, was eine Query fachlich anfordert und warum Bedingungen unterschiedlicher Selektivität nicht gleich wirken.
- **Evidenzbedarf:** MongoDB-Dokumentation zu Query Shapes und Selektivität; begriffliche Einordnung.
- **Evidenztyp:** `external`
- **Evidenzstatus:** `ready`
- **Beziehung davor:** Folge.
- **Beziehung danach:** Ursache — geordnete Indizes können nur für einen durch den Shape und seine Grenzen bestimmten Schlüsselbereich arbeiten.
- **Zielwörter:** 150.
- **Medium:** optionaler Verweis auf die kompakte Query-Shape-zu-Indexset-Tabelle in Kapitel 3, keine Doppelung.
- **Offene Recherche:** Abgrenzung von Query Shape und zusätzlich festgelegten Workloadparametern wie Limit präzise formulieren.
- **Bestandsübernahme:** teilweise; vorhandene produktkatalogspezifische Absätze nur bei Übereinstimmung mit Q1–Q4 übernehmen, Selektivitätsbezug ergänzen und erneut prüfen.

<details>
<summary>Evidenz und menschliche Prüfung</summary>

#### E-TH-02-P02-01 — Bestandteile eines Query Shape

- **Rolle:** `definiert`
- **Vollbeleg / Artefakt:** MongoDB Inc. (o. J.): *Query Shapes*. MongoDB Database Manual 8.0.
- **Link / Projektpfad:** https://www.mongodb.com/docs/v8.0/core/query-shapes/
- **Ausgabe / Version:** MongoDB Database Manual 8.0; abgerufen am 05.08.2026.
- **Fundstelle:** Einleitung sowie Abschnitte „Matching Query Shapes“ und „Different Query Shape“.
- **Originalauszug / quellennaher Auszug:**

  > Quellennahe Inhaltsnotiz: Ein Query Shape gruppiert Operationen anhand struktureller Spezifikationen; dazu können unter anderem Filter, Sortierung, Projektion, Namespace und Pipeline-Stufen gehören.

- **Eigene Zusammenfassung:** Filter und Sortierung der Q1–Q3-Varianten sind Teil ihrer strukturellen Form; konkrete Werte werden zusätzlich als Workloadparameter festgelegt.
- **Grenze und Kontext:** Die Seite definiert in MongoDB 8.0 neben dem bisherigen Plan-Cache-Shape einen neuen Query-Shape-Begriff. Der Absatz darf beide nicht unpräzise gleichsetzen; `limit` wird nicht als alleinige Kerndefinition behauptet.
- **Reviewentscheidung:** `übernehmen`
- **Menschliche Prüfung:** verified

#### E-TH-02-P02-02 — Selektivität

- **Rolle:** `definiert`
- **Vollbeleg / Artefakt:** MongoDB Inc. (o. J.): *Create Selective Indexes to Answer Queries Efficiently*. MongoDB Database Manual 8.0.
- **Link / Projektpfad:** https://www.mongodb.com/docs/v8.0/tutorial/create-queries-that-ensure-selectivity/
- **Ausgabe / Version:** MongoDB Database Manual 8.0; abgerufen am 05.08.2026.
- **Fundstelle:** Einleitungsabschnitt und Beispiel „Selectivity with Many Common Values“.
- **Originalauszug / quellennaher Auszug:**

  > Quellennahe Inhaltsnotiz: Query-Selektivität beschreibt das Verhältnis passender Dokumente zur Gesamtzahl; hohe Selektivität liegt vor, wenn nur ein kleiner Anteil passt.

- **Eigene Zusammenfassung:** Parameterwerte unterschiedlicher Häufigkeit sowie Bereiche unterschiedlicher Breite können unterschiedlich große Kandidatenmengen erzeugen.
- **Grenze und Kontext:** Selektivität allein bestimmt weder den gewählten Plan noch die gemessene End-to-End-Latenz.
- **Reviewentscheidung:** `übernehmen`
- **Menschliche Prüfung:** geprüft

</details>

### TH-02-P03 — Geordnete Indexzugriffe und ihr Pflegeaufwand

- **Funktion:** Kurz erklären, wie geordnete Baumstrukturen Suchräume begrenzen können, und den Zielkonflikt zu Speicher und Writes begründen.
- **Kernaussage:** MongoDB-Indizes organisieren Schlüssel geordnet in baumartigen Zugriffspfaden. Bei Equality- oder Range-Bedingungen kann die Suche an einem passenden Schlüsselbereich beginnen und Teilbereiche außerhalb der Grenzen auslassen; bei breiten oder wenig selektiven Bedingungen bleibt der relevante Bereich dennoch groß. Zusätzliche Indizes benötigen Speicher und ihre Einträge müssen bei Inserts, Deletes sowie Änderungen indexierter Werte gepflegt werden.
- **Begründung:** Dieser Absatz erklärt zugleich, warum Indexe Q1-eng, Q1-breit und die Häufigkeitsvarianten unterschiedlich unterstützen können und warum zusätzliche Indizes Write-Aufwand verursachen. Er behauptet keine nicht überprüfte MongoDB-interne Rebalancing-Logik.
- **Evidenzbedarf:** B-Baum-Grundlage, MongoDB-Indexbeschreibung und Write-Performance-Dokumentation.
- **Evidenztyp:** `external`
- **Evidenzstatus:** `ready`
- **Beziehung davor:** Ursache.
- **Beziehung danach:** Folge — Indexfelder und -eigenschaften müssen passend zu den Shapes gewählt werden.
- **Zielwörter:** 180.
- **Medium:** keines.
- **Offene Recherche:** MongoDB-spezifische Terminologie am Original verifizieren; Pflegekosten präzise von allgemeinem Baum-Rebalancing trennen.
- **Bestandsübernahme:** bevorzugt aus den vorhandenen Absätzen zu Indexzugriff, Balance/Bereich und Write-Kosten, aber als zusammenhängender neuer Absatz erst nach Evidenzprüfung.

<details>
<summary>Evidenz und menschliche Prüfung</summary>

#### E-TH-02-P03-01 — Geordnete MongoDB-Indizes

- **Rolle:** `stützt`
- **Vollbeleg / Artefakt:** MongoDB Inc. (o. J.): *Indexes*. MongoDB Database Manual 8.0.
- **Link / Projektpfad:** https://www.mongodb.com/docs/v8.0/indexes/
- **Ausgabe / Version:** MongoDB Database Manual 8.0; abgerufen am 05.08.2026.
- **Fundstelle:** Abschnitt „Details“ sowie Einleitungsabschnitt.
- **Originalauszug / quellennaher Auszug:**

  > Quellennahe Inhaltsnotiz: MongoDB-Indizes verwenden eine B-Baum-Struktur und speichern Feldwerte geordnet; die Ordnung unterstützt Gleichheits-, Bereichs- und passende Sortieroperationen und kann die Zahl geprüfter Dokumente begrenzen.

- **Eigene Zusammenfassung:** Die geordnete Schlüsselstruktur erklärt, warum ein passender Index einen begrenzten Schlüsselbereich statt der vollständigen Collection bearbeiten kann.
- **Grenze und Kontext:** Die Dokumentation garantiert nicht, dass der Planner den vorhandenen Index wählt oder dass ein breiter Bereich schnell ist.
- **Reviewentscheidung:** `offen`
- **Menschliche Prüfung:** ausstehend

#### E-TH-02-P03-02 — B-Baum-Suche und Balance

- **Rolle:** `stützt`
- **Vollbeleg / Artefakt:** Douglas Comer (1979): „The Ubiquitous B-Tree“. *ACM Computing Surveys*, 11(2), S. 121–137. https://doi.org/10.1145/356770.356776.
- **Link / Projektpfad:** D:/projects/uni/mongodb_indexing_paper/research/library/d-comer_the_ubiqutous_B-Tree_1979.pdf
- **Ausgabe / Version:** veröffentlichte Zeitschriftenfassung, Juni 1979.
- **Fundstelle:** S. 123–125 (Suchpfade, Balance) und S. 127 (logarithmischer Suchaufwand).
- **Originalauszug / quellennaher Auszug:**

  > Quellennahe Inhaltsnotiz: Die Suche folgt an jedem Knoten dem durch Schlüsselvergleiche bestimmten Teilpfad. Ein balancierter B-Baum hält alle Blätter auf gleicher Tiefe; die Zahl besuchter Knoten wächst logarithmisch mit der Dateigröße.

- **Eigene Zusammenfassung:** Der allgemeine B-Baum-Mechanismus liefert die theoretische Erklärung für begrenzte Suchpfade, ohne MongoDB-spezifische interne Details vorwegzunehmen.
- **Grenze und Kontext:** Der historische Artikel modelliert Sekundärspeicherzugriffe und keine aktuelle MongoDB-Engine; aus der asymptotischen Eigenschaft folgt keine konkrete Latenz für den Benchmark.
- **Reviewentscheidung:** `offen`
- **Menschliche Prüfung:** ausstehend

#### E-TH-02-P03-03 — Pflegeaufwand bei Writes

- **Rolle:** `stützt`
- **Vollbeleg / Artefakt:** MongoDB Inc. (o. J.): *Write Operation Performance*. MongoDB Database Manual 8.0.
- **Link / Projektpfad:** https://www.mongodb.com/docs/v8.0/core/write-performance/
- **Ausgabe / Version:** MongoDB Database Manual 8.0; abgerufen am 05.08.2026.
- **Fundstelle:** Abschnitt „Indexes“.
- **Originalauszug / quellennaher Auszug:**

  > Quellennahe Inhaltsnotiz: Inserts und Deletes ändern Schlüssel in jedem Collection-Index; Updates ändern abhängig von den betroffenen Schlüsseln eine Teilmenge der Indizes. Partial-Indizes werden nur gepflegt, wenn die betroffenen Dokumente im Index enthalten sind.

- **Eigene Zusammenfassung:** Mehr und breitere Indizes können zusätzliche Pflegearbeit verursachen; indexierte und nicht indexierte Updates sind deshalb getrennt zu messen.
- **Grenze und Kontext:** Die Dokumentation nennt keinen festen Kostenfaktor und stützt keine quantitative Vorhersage.
- **Reviewentscheidung:** `offen`
- **Menschliche Prüfung:** ausstehend

</details>

### TH-02-P04 — Indexentwurf: Compound-Indizes, Präfixe und ESR

- **Funktion:** Die allgemeine Entwurfslogik für die Q1-/Q3-Unterstützung und die Unterschiede zwischen L und W1 erklären.
- **Kernaussage:** Compound-Indizes ordnen mehrere Felder in einer festen Reihenfolge; geeignete Präfixe können weitere Zugriffsmuster unterstützen. Die ESR-Regel ordnet Equality-, Sort- und Range-Anteile als Entwurfsleitlinie, ersetzt aber keine Prüfung gegen den konkreten Shape, seine Selektivität und die Datenmenge.
- **Begründung:** Der Spezialindex von L und die bewusste Wiederverwendung eines Q1-orientierten Indexes durch W1 lassen sich nur über Feldreihenfolge, Präfixe und die Grenze von ESR verständlich vergleichen.
- **Evidenzbedarf:** MongoDB-Primärdokumentation zu Compound-Präfixen, Sortierung und ESR.
- **Evidenztyp:** `external`
- **Evidenzstatus:** `ready`
- **Beziehung davor:** Folge.
- **Beziehung danach:** Fortführung — Multikey-, Partial- und Unique-Eigenschaften ergänzen diese Entwurfsentscheidung für Q2, W2 und die gemeinsame Basis.
- **Zielwörter:** 210.
- **Medium:** TH-M01.
- **Offene Recherche:** Range-und-Sort-Grenze für die konkrete Q1-Feldreihenfolge präzise prüfen.
- **Bestandsübernahme:** bevorzugt; Sortierrichtung und projektspezifische Beispiele fachlich angleichen und danach neu prüfen.

<details>
<summary>Evidenz und menschliche Prüfung</summary>

#### E-TH-02-P04-01 — Feldreihenfolge und Präfixe

- **Rolle:** `stützt`
- **Vollbeleg / Artefakt:** MongoDB Inc. (o. J.): *Compound Indexes*. MongoDB Database Manual 8.0.
- **Link / Projektpfad:** https://www.mongodb.com/docs/v8.0/core/indexes/index-types/index-compound/
- **Ausgabe / Version:** MongoDB Database Manual 8.0; abgerufen am 05.08.2026.
- **Fundstelle:** Einleitungsabschnitt und Abschnitt „Index Prefixes“.
- **Originalauszug / quellennaher Auszug:**

  > Quellennahe Inhaltsnotiz: Compound-Indizes ordnen Werte mehrerer Felder in der angegebenen Feldreihenfolge. Sie können Abfragen auf dem ersten Feld und auf beginnenden Präfixen der Indexdefinition unterstützen.

- **Eigene Zusammenfassung:** Die Feldreihenfolge ist Teil des Indexentwurfs; W1 kann den Q1-Index nur in dem Umfang für Q3 wiederverwenden, den sein führendes Präfix ermöglicht.
- **Grenze und Kontext:** Präfixnutzbarkeit bedeutet nicht, dass sämtliche nachgelagerten Filter oder Sortierungen vollständig im Index erledigt werden.
- **Reviewentscheidung:** `offen`
- **Menschliche Prüfung:** ausstehend

#### E-TH-02-P04-02 — ESR als Leitlinie mit ERS-Ausnahme

- **Rolle:** `begrenzt`
- **Vollbeleg / Artefakt:** MongoDB Inc. (o. J.): *The ESR (Equality, Sort, Range) Guideline*. MongoDB Database Manual 8.0.
- **Link / Projektpfad:** https://www.mongodb.com/docs/v8.0/tutorial/equality-sort-range-guideline/
- **Ausgabe / Version:** MongoDB Database Manual 8.0; abgerufen am 05.08.2026.
- **Fundstelle:** Einleitungsabschnitt sowie Abschnitte „Equality“, „Sort“ und „Range“.
- **Originalauszug / quellennaher Auszug:**

  > Quellennahe Inhaltsnotiz: Equality-Felder sollen zuerst stehen. Ob danach Sort oder Range folgt, hängt vom Ziel ab: ESR priorisiert das Vermeiden einer In-Memory-Sortierung, während ein sehr selektiver Range-Filter ERS sinnvoll machen kann.

- **Eigene Zusammenfassung:** ESR begründet die Kandidatenreihenfolge als Entwurfsheuristik, macht aber die konkrete Messung von Q1-eng und Q1-breit nicht überflüssig.
- **Grenze und Kontext:** Die Richtlinie ist keine Garantie für den schnellsten Plan und deckt die vollständige Wirkung von Datenverteilung, Cache und Planner nicht ab.
- **Reviewentscheidung:** `offen`
- **Menschliche Prüfung:** ausstehend

</details>

### TH-02-P05 — Indexentwurf: Multikey, Partial und Unique

- **Funktion:** Die besonderen Indexeigenschaften in denselben Entwurfsblock einordnen.
- **Kernaussage:** Das Arrayfeld `tags` erzeugt Multikey-Verhalten; ein Partial Index ist nur für Queries mit passender Filterbedingung vollständig nutzbar; der gemeinsame Unique-Index auf `productId` verbindet Detailzugriff und Integritätsregel. Diese Eigenschaften unterscheiden insbesondere Q2, W2 und die gemeinsame Basis aller Sets.
- **Begründung:** Multikey, Partial und Unique sind keine isolierten Indextypen im Kapitel, sondern konkrete Gestaltungsoptionen desselben vollständigen Indexsets.
- **Evidenzbedarf:** MongoDB-Primärdokumentation zu Multikey-, Partial- und Unique-Regeln sowie Write-Pflege bei Partial-Indizes.
- **Evidenztyp:** `external`
- **Evidenzstatus:** `ready`
- **Beziehung davor:** Fortführung.
- **Beziehung danach:** Folge — erst der Planner macht sichtbar, welcher dieser Zugriffspfade für eine konkrete Query gewählt wird.
- **Zielwörter:** 210.
- **Medium:** TH-M01.
- **Offene Recherche:** bisher ungeprüften Unique- und Partial-Write-Teil erstmals am Original prüfen.
- **Bestandsübernahme:** Multikey und Partial bevorzugt; Unique- und Partial-Write-Aussagen nur nach neuer Prüfung.

<details>
<summary>Evidenz und menschliche Prüfung</summary>

#### E-TH-02-P05-01 — Multikey-Verhalten des Tag-Felds

- **Rolle:** `stützt`
- **Vollbeleg / Artefakt:** MongoDB Inc. (o. J.): *Multikey Indexes*. MongoDB Database Manual 8.0.
- **Link / Projektpfad:** https://www.mongodb.com/docs/v8.0/core/indexes/index-types/index-multikey/
- **Ausgabe / Version:** MongoDB Database Manual 8.0; abgerufen am 05.08.2026.
- **Fundstelle:** Einleitungsabschnitt, „Use Cases“ und „Compound Multikey Indexes“.
- **Originalauszug / quellennaher Auszug:**

  > Quellennahe Inhaltsnotiz: Ein Index auf einem Arrayfeld wird automatisch als Multikey-Index geführt. Bei Compound-Multikey-Indizes darf pro indexiertem Dokument höchstens eines der Indexfelder ein Array enthalten.

- **Eigene Zusammenfassung:** Der Index auf `tags` besitzt wegen des Arrayfelds Multikey-Semantik; die kombinierte Definition mit dem skalaren Feld `isActive` verletzt die dokumentierte Ein-Array-Grenze nicht.
- **Grenze und Kontext:** Die Quelle sagt nicht voraus, wie stark Q2-häufig oder Q2-selten beschleunigt wird.
- **Reviewentscheidung:** `offen`
- **Menschliche Prüfung:** ausstehend

#### E-TH-02-P05-02 — Abdeckung und Kosten eines Partial Index

- **Rolle:** `stützt`
- **Vollbeleg / Artefakt:** MongoDB Inc. (o. J.): *Partial Indexes*. MongoDB Database Manual 8.0.
- **Link / Projektpfad:** https://www.mongodb.com/docs/v8.0/core/index-partial/
- **Ausgabe / Version:** MongoDB Database Manual 8.0; abgerufen am 05.08.2026.
- **Fundstelle:** Einleitungsabschnitt und Abschnitt „Query Coverage“.
- **Originalauszug / quellennaher Auszug:**

  > Quellennahe Inhaltsnotiz: Partial-Indizes enthalten nur Dokumente, die eine Filterexpression erfüllen. MongoDB nutzt sie nicht, wenn dadurch eine unvollständige Ergebnismenge entstünde; die Query muss die Filterbedingung oder eine engere Bedingung enthalten.

- **Eigene Zusammenfassung:** Der W2-Index für aktive Produkte kann Q1/Q3 nur deshalb vollständig unterstützen, weil diese Queries `isActive = true` enthalten; das kleinere Indexset ist bewusst an diese Bedingung gebunden.
- **Grenze und Kontext:** Niedrigerer Speicher- und Pflegebedarf ist eine dokumentierte Möglichkeit, aber die konkrete Einsparung muss gemessen werden.
- **Reviewentscheidung:** `offen`
- **Menschliche Prüfung:** ausstehend

#### E-TH-02-P05-03 — Unique-Index als Integritätsregel

- **Rolle:** `definiert`
- **Vollbeleg / Artefakt:** MongoDB Inc. (o. J.): *Unique Indexes*. MongoDB Database Manual 8.0.
- **Link / Projektpfad:** https://www.mongodb.com/docs/v8.0/core/index-unique/
- **Ausgabe / Version:** MongoDB Database Manual 8.0; abgerufen am 05.08.2026.
- **Fundstelle:** Einleitung und Abschnitt „Create a Unique Index“.
- **Originalauszug / quellennaher Auszug:**

  > Quellennahe Inhaltsnotiz: Ein Unique-Index verhindert doppelte Werte in den indexierten Feldern; MongoDB legt für Collections standardmäßig einen eindeutigen `_id`-Index an.

- **Eigene Zusammenfassung:** Der gemeinsame eindeutige `productId`-Index dient sowohl dem Q4-Zugriff als auch der Integritätsregel und gehört deshalb in jede Konfiguration.
- **Grenze und Kontext:** Die Quelle begründet nicht, dass ein zusätzlicher `productId`-Index notwendig wäre, wenn `_id` fachlich dieselbe Rolle übernehmen würde; dies bleibt eine Modellentscheidung des Projekts.
- **Reviewentscheidung:** `offen`
- **Menschliche Prüfung:** ausstehend

#### E-TH-02-P05-04 — Sonderfall: Write-Pflege bei Partial-Indizes

- **Rolle:** `stützt`
- **Vollbeleg / Artefakt:** MongoDB Inc. (o. J.): *Write Operation Performance*. MongoDB Database Manual 8.0.
- **Link / Projektpfad:** https://www.mongodb.com/docs/v8.0/core/write-performance/
- **Ausgabe / Version:** MongoDB Database Manual 8.0; abgerufen am 05.08.2026.
- **Fundstelle:** Abschnitt „Indexes“, insbesondere Hinweis zu Partial- und Sparse-Indizes.
- **Originalauszug / quellennaher Auszug:**

  > Quellennahe Inhaltsnotiz: MongoDB aktualisiert einen Partial-Index nur dann, wenn die an einer Schreiboperation beteiligten Dokumente im Index enthalten sind.

- **Eigene Zusammenfassung:** Die Pflegekosten eines Partial-Index hängen zusätzlich von seiner Mitgliedschaftsbedingung ab und dürfen nicht pauschal mit denen eines Vollindex gleichgesetzt werden.
- **Grenze und Kontext:** Auch diese Aussage quantifiziert keinen Laufzeitvorteil für W2.
- **Reviewentscheidung:** `offen`
- **Menschliche Prüfung:** ausstehend

</details>

### TH-02-P06 — Planwahl, Planstufen und Explain-Analyse

- **Funktion:** Planner, sichtbare Planstufen und Explain-Metriken als zusammenhängende Beobachtungsebene erklären.
- **Kernaussage:** Auf Basis von Query Shape und verfügbaren Indizes kann MongoDB unterschiedliche Pläne wählen. `COLLSCAN`, `IXSCAN`, `FETCH` und `SORT` machen Collection-, Index-, Dokument- und Sortierarbeit sichtbar; `nReturned`, `totalDocsExamined` und `totalKeysExamined` quantifizieren den ausgeführten Plan. Ein `IXSCAN` allein beweist jedoch keine Eignung, und Explain-Zeit ist wegen abweichenden Plan-Cache-Verhaltens nicht mit wiederholter End-to-End-Latenz gleichzusetzen.
- **Begründung:** Die Evaluation benötigt eine gemeinsame Sprache, um die Setwirkung technisch zu erklären, ohne Planstruktur oder Explain-Zeit als Ersatz für die Latenzmessung zu behandeln.
- **Evidenzbedarf:** versionspassende MongoDB-Dokumentation zu Planwahl, Explain-Ausgabe und Kennzahlen; Forschung zur Begrenzung der Plannerinterpretation.
- **Evidenztyp:** `external`
- **Evidenzstatus:** `ready`
- **Beziehung davor:** Folge.
- **Beziehung danach:** Übergabe an `method` — der kontrollierte Setvergleich misst deshalb normale Latenz und Explain getrennt.
- **Zielwörter:** 270.
- **Medium:** optionaler schematischer Planbaum nur, falls er die gemeinsame Lesart der Stufen besser als Prosa trägt.
- **Offene Recherche:** konkrete MongoDB-Version und Planrepräsentation in Kapitel 3 dokumentieren; keine Versionsgrenze als eigenen Grundlagenabsatz schreiben.
- **Bestandsübernahme:** bevorzugt für Planstufen und Metriken; Plannerbeschreibung nur nach Versionsabgleich; ungeprüften Hint-/Rejected-Plans-Exkurs weglassen.

<details>
<summary>Evidenz und menschliche Prüfung</summary>

#### E-TH-02-P06-01 — Planwahl und Plan-Cache in MongoDB 8.0

- **Rolle:** `stützt`
- **Vollbeleg / Artefakt:** MongoDB Inc. (o. J.): *Query Plans*. MongoDB Database Manual 8.0.
- **Link / Projektpfad:** https://www.mongodb.com/docs/v8.0/core/query-plans/
- **Ausgabe / Version:** MongoDB Database Manual 8.0; abgerufen am 05.08.2026.
- **Fundstelle:** Einleitungsabschnitt und Hinweis zum Verhalten von `explain`.
- **Originalauszug / quellennaher Auszug:**

  > Quellennahe Inhaltsnotiz: Für eine Query bewertet der Planner verfügbare Kandidaten während einer Testphase und speichert den gewählten Plan für spätere Queries desselben Plan-Cache-Shapes. `explain` ignoriert vorhandene Cache-Einträge und erzeugt keinen neuen.

- **Eigene Zusammenfassung:** Verfügbare Indexsets beeinflussen den Kandidatenraum; normale Wiederholungsläufe und Explain-Aufrufe besitzen wegen des Cache-Verhaltens unterschiedliche Messbedingungen.
- **Grenze und Kontext:** Diese versionsgebundene 8.0-Seite darf nicht mit der ab MongoDB 8.3 dokumentierten CBR-Erweiterung vermischt werden.
- **Reviewentscheidung:** `offen`
- **Menschliche Prüfung:** ausstehend

#### E-TH-02-P06-02 — Explain-Baum, Stufen und Kennzahlen

- **Rolle:** `definiert`
- **Vollbeleg / Artefakt:** MongoDB Inc. (o. J.): *Explain Results*. MongoDB Database Manual 8.0.
- **Link / Projektpfad:** https://www.mongodb.com/docs/v8.0/reference/explain-results/
- **Ausgabe / Version:** MongoDB Database Manual 8.0; abgerufen am 05.08.2026.
- **Fundstelle:** Abschnitte „Explain Output Structure“, „queryPlanner“ und „executionStats“.
- **Originalauszug / quellennaher Auszug:**

  > Quellennahe Inhaltsnotiz: Explain stellt Pläne als Baum von Stufen dar. `COLLSCAN`, `IXSCAN` und `FETCH` kennzeichnen Collection-, Index- und Dokumentzugriff; `nReturned`, `totalKeysExamined` und `totalDocsExamined` beschreiben die ausgeführte Gewinnerplanung.

- **Eigene Zusammenfassung:** Diese Felder bilden die strukturelle Beobachtungsebene der Evaluation und erklären, welche Arbeit hinter einer gemessenen Latenz liegt.
- **Grenze und Kontext:** Explain-Felder können sich zwischen Versionen und Ausführungsengines unterscheiden; eine einzelne Stufenbezeichnung ist noch kein Qualitätsurteil.
- **Reviewentscheidung:** `offen`
- **Menschliche Prüfung:** ausstehend

#### E-TH-02-P06-03 — Explain-Zeit ist keine normale End-to-End-Latenz

- **Rolle:** `begrenzt`
- **Vollbeleg / Artefakt:** MongoDB Inc. (o. J.): *Explain Results*. MongoDB Database Manual 8.0.
- **Link / Projektpfad:** https://www.mongodb.com/docs/v8.0/reference/explain-results/
- **Ausgabe / Version:** MongoDB Database Manual 8.0; abgerufen am 05.08.2026.
- **Fundstelle:** `explain.executionStats.executionTimeMillis` sowie einleitender Hinweis zum Plan-Cache.
- **Originalauszug / quellennaher Auszug:**

  > Quellennahe Inhaltsnotiz: `executionTimeMillis` umfasst Planwahl und Serverausführung, aber nicht die Netzwerkübertragung; wegen der abweichenden Plan-Cache-Situation ist der Wert nicht zwingend repräsentativ für normale Query-Laufzeit.

- **Eigene Zusammenfassung:** Die Arbeit muss clientseitige normale Query-Latenz und strukturelle Explain-Erhebung getrennt durchführen und auswerten.
- **Grenze und Kontext:** Auch eine clientseitige Messung bleibt von Umgebung, Treiber und Cachezustand abhängig; sie ist deshalb nur im kontrollierten Versuchsraum vergleichbar.
- **Reviewentscheidung:** `offen`
- **Menschliche Prüfung:** ausstehend

#### E-TH-02-P06-04 — Indexplan kann langsamer als Collection Scan sein

- **Rolle:** `widerspricht`
- **Vollbeleg / Artefakt:** Dawei Tao, Enqi Liu, Sidath Randeni Kadupitige, Michael Cahill, Alan Fekete und Uwe Röhm (2024): *First Past the Post: Evaluating Query Optimization in MongoDB*. arXiv:2409.16544.
- **Link / Projektpfad:** https://arxiv.org/abs/2409.16544
- **Ausgabe / Version:** arXiv-Version vom 25.09.2024; Untersuchung mit MongoDB 7.0.1.
- **Fundstelle:** Abschnitte 4.1 und 4.2, insbesondere Ergebnisse zu Fällen, in denen `COLLSCAN` schneller als der gewählte `IXSCAN` war.
- **Originalauszug / quellennaher Auszug:**

  > Quellennahe Inhaltsnotiz: In den untersuchten MongoDB-7.0.1-Szenarien wählte der Planner teilweise Indexscans, obwohl ein Collection Scan die geringere Laufzeit hatte; der Effekt trat besonders bei wenig selektiven Bedingungen auf.

- **Eigene Zusammenfassung:** Ein sichtbarer `IXSCAN` oder geringer Schlüsselzugriff darf nicht allein als Beweis für das beste Set dienen; Latenz muss unabhängig gemessen werden.
- **Grenze und Kontext:** Das Paper untersucht synthetische, eng definierte Shapes auf MongoDB 7.0.1 und ist nicht unmittelbar auf MongoDB 8.0 oder die Projektqueries übertragbar.
- **Reviewentscheidung:** `offen`
- **Menschliche Prüfung:** ausstehend

</details>

## Medienplan

| Medium-ID | Typ | Aussagefunktion | Herkunft/Erzeugung | Beschriftung | Einführung und Interpretation |
| --- | --- | --- | --- | --- | --- |
| TH-M01 | Tabelle | Zeigt für Q1–Q3, welche Indexeigenschaften und Designrollen jeweils relevant sind | eigene Synthese nach geprüften Grundlagen | „Query Shapes, Indexentwurf und beobachtbare Größen“ | Nach TH-02-P05 einführen; TH-02-P06 erklärt, wie die Wirkung dieser Entwürfe beobachtet wird |

## Offene Entscheidungen

- Nach G3 für jeden Bestandsabsatz entscheiden: `exact reuse`, `editorial`, `structural` oder `substantive`; im Zweifel `substantive`.
- Zielumfang nach evidenzbasierter Auswahl ganzer Absätze erneut schätzen; keine Sätze nur zur Budgeterfüllung zerschneiden.
