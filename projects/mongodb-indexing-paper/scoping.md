# Scoping-Recherche

**Status:** Orientierungsrecherche für G2  
**Stand:** 04.08.2026  
**Grenze:** Die folgenden Quellen und Codebefunde dienen der Machbarkeits- und Gliederungsentscheidung. Sie besitzen noch keinen Evidenzstatus für spätere Kapitelabsätze.

## Kernbegriffe und Synonyme

- **Indexset / Indexkonfiguration:** vollständige Menge sekundärer Indizes einer Collection; abzugrenzen von einem einzelnen Index beziehungsweise einer isolierten Indexstrategie.
- **Query Shape / Plan-Cache-Query-Shape:** Struktur aus Filter, Sortierung und Projektion, für die MongoDB Kandidatenpläne bewertet und gegebenenfalls einen Plan zwischenspeichert.
- **Compound Index / Indexpräfix:** mehrfeldriger Index; nutzbare Präfixe beginnen beim ersten Indexfeld. Feldreihenfolge und Sortierrichtung sind deshalb Teil der Konfiguration.
- **B-Baum:** balancierte, geordnete Indexstruktur. Sie führt eine Suche über Schlüsselbereiche schrittweise zu passenden Blättern, statt jedes Dokument der Collection prüfen zu müssen. Diese Ordnung unterstützt auch Bereichsbedingungen und – bei passender Indexreihenfolge – Sortierungen.
- **ESR:** Equality–Sort–Range als Entwurfsrichtlinie für Compound Indexes, nicht als Garantie für den schnellsten Plan.
- **Multikey Index:** Index auf einem Array-Feld; bei einem Compound Multikey Index darf pro Dokument höchstens ein indexiertes Feld ein Array sein.
- **Partial Index:** Index nur für Dokumente, die eine `partialFilterExpression` erfüllen; die Query muss diese Bedingung oder eine engere Bedingung enthalten, damit der Index ohne unvollständiges Ergebnis nutzbar ist.
- **Selektivität:** Anteil beziehungsweise Trennschärfe eines Prädikats; hier über getrennte häufige und seltene Parameterwerte operationalisiert.
- **struktureller Suchaufwand:** insbesondere `totalDocsExamined`, `totalKeysExamined`, `nReturned` und Planstufen; von End-to-End-Latenz zu trennen.
- **Write-Aufwand:** Pflegekosten der betroffenen Indexeinträge bei Insert und Update; bei Updates hängt die betroffene Indexmenge von den geänderten Feldern ab.
- **bedingte Empfehlung:** Auswahl innerhalb des vorab begrenzten Kandidatenraums und unter einem benannten Prioritätsprofil; keine universelle Optimalaussage.

## Standardwerke und Forschungsstränge

1. **MongoDB-Primärdokumentation:** Compound-Präfixe, ESR, Multikey-, Partial- und Unique-Eigenschaften sowie Explain- und Plan-Cache-Semantik. Diese Dokumentation ist für produktspezifische Aussagen vorrangig, muss aber auf die tatsächlich eingesetzte Serverversion bezogen werden.
2. **B-Baum- und Indexgrundlagen:** Comer (1979) sowie Bayer/McCreight als theoretische Basis für geordnete Indexstrukturen. Für die kurze Arbeit reicht eine knappe Funktionsgrundlage; interne MongoDB-Implementierungsdetails werden nicht behauptet, wenn die Primärdokumentation sie nicht trägt.
3. **Query-Verarbeitung:** Graefe (1993) als allgemeiner Forschungsstrang zu Zugriffspfaden und Operatoren. Allgemeine DBMS-Theorie darf nicht unbesehen als MongoDB-spezifische Plansemantik formuliert werden.
4. **Datenbank-Tuning und physisches Design:** Chaudhuri/Narasayya und Tuning-Literatur erklären workloadbezogene Indexwahl, dienen hier aber hauptsächlich der Abgrenzung vom automatischen Advisor und vom kombinatorischen Suchproblem.
5. **Benchmarkmethodik:** Raasveldt et al. (2018) behandeln typische Verzerrungen in DBMS-Performancevergleichen und stützen die Notwendigkeit identischer Ausgangszustände, transparenter Konfigurationen und reproduzierbarer Abläufe.

## Aktuelle und unmittelbar relevante Forschung

- Tao et al., *First Past the Post: Evaluating Query Optimization in MongoDB* (Preprint 2024), untersuchen MongoDB 7.0.1 und zeigen, dass ein gewählter Indexplan nicht automatisch der laufzeitschnellste Plan sein muss. Für diese Arbeit ist das eine wichtige Begrenzung: `IXSCAN` und ein niedriger Scanaufwand sind erklärende Strukturmerkmale, aber kein Ersatz für getrennte Latenzmessung. Die Übertragbarkeit auf die einzufrierende MongoDB-8-Version muss begrenzt werden.
- Die aktuelle MongoDB-Dokumentation beschreibt für geeignete Queries ab MongoDB 8.3 zusätzlich einen Cost-Based Ranker als Rückfall- beziehungsweise Auswahlmechanismus. Deshalb darf die Arbeit MongoDBs Planwahl nicht pauschal und versionsunabhängig ausschließlich als FPTP charakterisieren.
- Die Primärdokumentation empfiehlt, Indexkonfigurationen gegen repräsentative Daten und tatsächliche Query-Muster zu profilieren und dabei Lese-/Schreibverhältnis sowie verfügbaren Speicher zu berücksichtigen. Das passt direkt zum kontrollierten Fallvergleich, begründet aber keine Allgemeingültigkeit der Kandidaten.

Orientierungsquellen:

- MongoDB, Query Plans: https://www.mongodb.com/docs/manual/core/query-plans/
- MongoDB, Explain Results: https://www.mongodb.com/docs/v8.0/reference/explain-results/
- MongoDB, Compound Indexes: https://www.mongodb.com/docs/manual/core/indexes/index-types/index-compound/
- MongoDB, ESR Guideline: https://www.mongodb.com/docs/v8.0/tutorial/equality-sort-range-guideline/
- MongoDB, Multikey Indexes: https://www.mongodb.com/docs/manual/core/indexes/index-types/index-multikey/
- MongoDB, Partial Indexes: https://www.mongodb.com/docs/v8.0/core/index-partial/
- MongoDB, Write Operation Performance: https://www.mongodb.com/docs/manual/core/write-performance/
- Tao et al.: https://arxiv.org/abs/2409.16544
- Raasveldt et al.: https://doi.org/10.1145/3209950.3209955

## Gegenpositionen und offene Fragen

- **Indexnutzung ist kein Erfolgskriterium für sich:** Ein `IXSCAN` kann wegen geringer Selektivität oder zusätzlicher Fetch-Arbeit schlechter als ein Collection Scan sein. Latenz und Strukturmetriken müssen gemeinsam interpretiert werden.
- **B-Baum-Grundlage erklärt keinen festen MongoDB-Plan:** Die allgemeine Struktur begründet, warum geordnete Zugriffswege Bereichssuche und Sortierung unterstützen können. Ob MongoDB diesen Weg wählt und wie groß der Vorteil ist, hängt dennoch von Query Shape, Verteilung, Selektivität und Serverversion ab.
- **`explain` ist keine normale Latenzmessung:** `explain` ignoriert bestehende Plan-Cache-Einträge, legt keinen neuen Cache-Eintrag an und seine `executionTimeMillis` ist laut Dokumentation nicht notwendig repräsentativ für normale Query-Laufzeit.
- **ESR ist eine Richtlinie:** Die geeignete Feldreihenfolge hängt von Query Shape, Bereichsbreite und Sortieranforderung ab. Die Kandidaten müssen deshalb empirisch geprüft werden.
- **Synthetische Daten begrenzen externe Validität:** Determinismus verbessert Reproduzierbarkeit, bildet aber reale Korrelationen, Aktualisierungsmuster und Hotspots nur soweit ab, wie sie bewusst modelliert werden.
- **Keine Konkurrenzprüfung gegen den gesamten Suchraum:** Der Direktvergleich kann Kandidaten gegeneinander bewerten, aber weder globale Optimalität noch die Unbrauchbarkeit nicht getesteter Indexsets zeigen.
- **Erste Seite statt Pagination:** `limit: 24` beantwortet nur die erste Ergebnisseite. Tiefes Paging, Cursor-Fortsetzung und Facetten bleiben außerhalb der Aussage.
- **Einzelclient statt Parallelität:** Geringerer Query-Aufwand kann Kapazitätsreserven nahelegen, ist aber kein gemessener Mehrbenutzer-Durchsatz.

## Vorab festgelegte Query-Parameter

Die folgenden Parameter passen zu den vorhandenen deterministischen Verteilungen und werden für die weitere Planung eingefroren. Änderungen nach G2 erfordern eine dokumentierte Invalidierung.

| Variante | Parameter |
| --- | --- |
| Q1-eng | `category = "Electronics"`, `isActive = true`, `100 <= price <= 150`, Sortierung `price: 1`, `limit: 24` |
| Q1-breit | `category = "Electronics"`, `isActive = true`, `100 <= price <= 900`, Sortierung `price: 1`, `limit: 24` |
| Q2-häufig | `tags = "outdoor"`, `isActive = true`, `limit: 24` |
| Q2-selten | `tags = "limited"`, `isActive = true`, `limit: 24` |
| Q3-häufig | `brand = "Northstar"`, `category = "Electronics"`, `isActive = true`, Sortierung `price: 1`, `limit: 24` |
| Q3-selten | `brand = "Summit"`, `category = "Electronics"`, `isActive = true`, Sortierung `price: 1`, `limit: 24` |
| Q4 | exakte vorhandene `productId`, `limit: 1` |

Damit Q3 tatsächlich häufige und seltene Markenvarianten besitzt, wird die derzeit uniforme Markenverteilung vor dem Pilotlauf in eine dokumentierte gewichtete Verteilung geändert. `Northstar` und `Summit` sind Arbeitswerte; die Generatorzusammenfassung muss ihre tatsächliche Trennung vor dem Referenzlauf bestätigen.

## Vorab festgelegte Indexkonfigurationen

Alle Sets enthalten `_id_` und `{productId: 1}` mit `unique: true`.

| Set | Zusätzliche Indizes | Designentscheidung |
| --- | --- | --- |
| B | keine | gemeinsame Referenzbasis |
| L | `{category: 1, isActive: 1, price: 1}`; `{tags: 1, isActive: 1}`; `{brand: 1, category: 1, isActive: 1, price: 1}` | je ein query-lokaler Index für Q1, Q2 und Q3 |
| W1 | `{category: 1, isActive: 1, price: 1}`; `{tags: 1, isActive: 1}` | Q1-Index unterstützt Q3 über Kategorie/Aktivität/Preissortierung mit residualem Markenfilter; kein Q3-Spezialindex |
| W2 | `{category: 1, price: 1}` mit `partialFilterExpression: {isActive: true}`; `{tags: 1}` | kleinere Schlüssel und Partial-Abdeckung; `isActive` wird bei Q2 residual geprüft |

Diese Sets unterscheiden sich materiell in Spezialunterstützung, Wiederverwendung und Ressourcenumfang. Im technischen Pilot wird nur geprüft, ob sie installierbar sind, korrekte Ergebnismengen liefern und unterscheidbare Pläne erzeugen. Sie werden nicht anhand günstiger Pilotlatenzen nachträglich optimiert.

## Praktische Machbarkeit

### Bereits vorhanden

- deterministische Python-Datengenerierung und Scale-Registry;
- MongoDB-Container mit 2 CPU und 2 GiB Benchmark-Limit;
- Registry für Query-Szenarien und Einzelindex-Strategien;
- wiederholte Runner-Schleife, Plan-Cache-Clear, Explain-Parser und Rohartefakte;
- Indexgrößenmessung, Git-/Versionsmetadaten und Ergebnis-Signaturen;
- Unit-, Integrations- und End-to-End-Teststruktur.

### Erforderliche Änderungen vor dem Pilot

1. **Version einfrieren:** `mongo:8` ist ein veränderlicher Tag. Der vorhandene lokale Image-Digest ist `mongo@sha256:49f1d7b87c2ddf918372be5defe7edff8c46703d0b2a56023a3f825e32e1250c`; vor dem Referenzlauf müssen zusätzlich `db.version()`, Image-Digest und Serverparameter im Manifest stehen.
2. **Datenmodell angleichen:** `brand` liegt derzeit unter `specifications.brand`; `rating`, `stock` und `updatedAt` fehlen. Für den freigegebenen Brief wird `brand` als Top-Level-Feld materialisiert und mindestens `stock` für W2a ergänzt. Nicht tatsächlich verwendete Felder sollen nicht nur wegen des alten Plans aufgenommen werden.
3. **Szenarien ersetzen/vereinheitlichen:** Q1 bis Q4 und ihre Varianten müssen die freigegebenen Filter, Sortierungen und Limits exakt abbilden; die bisherige Markenquery fehlt.
4. **vollständige Sets:** Die Registry und der Manager müssen mehrere gemeinsame Indizes pro B/L/W1/W2 inklusive `unique` verwalten und den tatsächlichen Indexzustand vollständig verifizieren.
5. **Latenz von Explain trennen:** Normale `find`-Aufrufe liefern End-to-End-Latenzen; `explain("executionStats")` wird separat und nicht pro Latenzwiederholung ausgeführt. Die aktuellen Explain-Warmups werden durch normale Query-Warmups ersetzt.
6. **Rotation:** Die feste Schleifenreihenfolge Szenario → Strategie wird durch eine vorab deterministische Rotation pro Messblock ersetzt.
7. **Write-Batches:** Insert-, nicht indexierte und potenziell indexierte Updates benötigen eigene Messpfade sowie identische Ausgangszustände pro Konfiguration und Wiederholung.
8. **Auswertung ersetzen:** Die alten vier Einzelindex-Hypothesen und ihre Auswertungslogik werden nicht auf die neuen Sets umgedeutet. Benötigt werden scenario-lokale Kennzahlen, Speicher-/Write-Vergleich und bedingte Empfehlungen ohne Gesamtscore.

### Laufzeitrisiko

Mit 3 Größen, 4 Sets, 6 Lesevarianten, 30 Wiederholungen sowie Write-Resets ist der Referenzlauf grundsätzlich machbar, aber deutlich umfangreicher als der aktuelle `full`-Lauf. Ein technischer Pilot muss Laufzeit und Artefaktvolumen schätzen. Die Zahl der Wiederholungen wird nur vor G2 oder nach dokumentierter Invalidierung geändert; ein p95 bleibt ausgeschlossen.

## Konsequenzen für Forschungsfrage und Gliederung

- Die Forschungsfrage bleibt unverändert und empirisch beantwortbar.
- Theorie wird auf Query Shape/Planwahl, Compound-Präfixe/ESR, Multikey/Partial sowie Explain- und Write-Kosten begrenzt.
- Ein kurzer B-Baum-Abschnitt erläutert die gemeinsame Ursache für geordneten Lesezugriff und Indexpflege: Schlüsselzugriffe können gegenüber vollständigem Scannen begrenzt werden, während Inserts und betroffene Updates Indexeinträge zusätzlich pflegen müssen.
- Methodik muss die Trennung von normaler Latenzmessung und Explain explizit erklären.
- Ergebnisse werden zuerst pro Query-Variante dargestellt; erst danach folgen Speicher, Writes, Dominanz und bedingte Empfehlung.
- Der Unterschied zwischen eigener empirischer Evidenz und Literaturbelegen wird in Kapitelplanung und Entwurf sichtbar gehalten.
- Limitationen zu synthetischen Daten, erster Ergebnisseite, Einzelclient, Kandidatenraum und Serverversion gehören in die Diskussion, nicht nur in den Ausblick.

## Noch ungeklärte Punkte

- Exakte MongoDB-Patchversion und Serverparameter können erst bei laufendem Container erfasst werden; der Digest ist bereits bekannt.
- Die gewichtete Markenverteilung und die gewählten Q1-Preisbereiche müssen im technischen Pilot nur auf ausreichende Ergebniszahlen und klare Häufigkeitsunterschiede geprüft werden.
- Ob 30 Query-Wiederholungen und zehn Write-Wiederholungen innerhalb des verfügbaren Zeitfensters liegen, muss der Pilot zeigen. Eine Änderung danach invalidiert mindestens Methodik und nachgelagerte Artefakte.
- Die gelöschte lokale LaTeX-Arbeitskopie und der Vorlagen-Snapshot blockieren nicht Scoping oder Outline, müssen aber spätestens vor Materialisierung nach G2 konsistent wiederhergestellt sein.
