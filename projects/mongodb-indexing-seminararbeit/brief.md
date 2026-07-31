# Forschungsauftrag

Status: Entwurf für die erneute menschliche G1-Freigabe.

## Thema

Workload-basierte Auswahl und empirische Validierung eines geeigneten MongoDB-Indexsets aus einem endlichen Kandidatenpool. Ein synthetischer E-Commerce-Produktkatalog dient als kontrollierte Fallstudie; ein prototypischer, wiederverwendbarer Indexset-Evaluator bildet das methodische Softwareartefakt.

## Problemstellung

Indexentscheidungen betreffen nicht nur einzelne Abfragen. Ein für eine Query Shape günstiger Index kann innerhalb eines gesamten Workloads redundant sein, zusätzlichen Speicher belegen oder Schreiboperationen verteuern. Umgekehrt kann eine Konfiguration, die nur aus den jeweils einzeln schnellsten Kandidaten zusammengesetzt wird, durch Präfixüberschneidungen, Plannerentscheidungen und gemeinsame Ressourcenwirkungen ungeeignet sein.

Damit ist die Auswahl eines Indexsets ein begrenztes Mehrzielproblem: Für einen definierten Workload, einen endlichen Kandidatenpool und explizite Randbedingungen sollen Leseleistung, Indexspeicherbedarf und ausgewählter Schreibaufwand gemeinsam beurteilt werden. Einzelindexmessungen liefern dafür ein kontrolliertes Kosten- und Nutzenmodell, sind aber nicht das Endergebnis. Die tatsächliche Eignung weniger ausgewählter Sets muss anschließend mit ausschließlich diesen Indizes und ohne erzwungene Indexwahl geprüft werden.

## Forschungsfrage

**Hauptforschungsfrage**

Wie kann aus einem festgelegten MongoDB-Query-Workload und einer begrenzten Menge möglicher Indizes ein passendes Indexset ausgewählt und durch Messungen überprüft werden, wenn Leseleistung, Speicherbedarf und Schreibaufwand gemeinsam berücksichtigt werden?

**Teilfragen**

1. Wie lassen sich Workload, Indexkandidaten und Messgrößen so festlegen, dass verschiedene Indexsets nachvollziehbar und reproduzierbar verglichen werden können?
2. Wie können alle möglichen Indexsets unter Berücksichtigung von Querygewichten und festgelegten Grenzen systematisch bewertet und nicht dominierte Lösungen ermittelt werden?
3. Welche Indexsets sind im E-Commerce-Referenzworkload bei den untersuchten Datenmengen und Selektivitäten nicht dominiert, und wie stabil ist diese Auswahl?
4. Wie gut sagen Baseline- und Einzelindexmessungen die tatsächliche Leistung ausgewählter Indexsets voraus, und welche Empfehlung lässt sich daraus für den Referenzworkload ableiten?

## Ziel und erwarteter Beitrag

Die Arbeit entwickelt eine nachvollziehbare Methode, die aus einem benutzerdefinierten Workload und einem endlichen Kandidatenpool geeignete Indexsets bestimmt. Sie verbindet eine kontrollierte Kandidatenprofilierung mit vollständiger Enumeration, mehrdimensionaler Auswahl und einer separaten Finalvalidierung.

Erwartet werden zwei miteinander verbundene Ergebnisse:

1. **Methodisches Softwareartefakt:** ein prototypischer MongoDB-Indexset-Evaluator, der Query Shapes, Parameter, optionale Gewichte, Kandidaten, Pflichtindizes, Skalen und Grenzen einliest und daraus Nutzenmatrix, untersuchten Konfigurationsraum, Pareto-Front, ausgewählte Finalisten sowie einen Validierungsbericht erzeugt.
2. **Empirische Fallstudie:** nicht dominierte beziehungsweise unter explizit gesetzten Präferenzen geeignete Indexsets für den festgelegten E-Commerce-Referenzworkload einschließlich einer bedingten Empfehlung.

Die Arbeit beansprucht weder die Erzeugung aller denkbaren MongoDB-Indizes noch globale Optimalität. Aussagen gelten ausschließlich für den vorgegebenen Workload, Kandidatenraum, Datensatz, die Messumgebung und das festgelegte Kostenmodell.

## Untersuchungsgegenstand

Der allgemeine Untersuchungsgegenstand ist die Auswahl einer Teilmenge optionaler MongoDB-Indizes für einen definierten Read-Workload. Ein Konfigurationsfall besteht aus:

- Query Shapes mit konkreten Parametervarianten und optionalen Häufigkeitsgewichten;
- einem endlichen, vorab fachlich begründeten Kandidatenpool;
- gegebenenfalls nicht abwählbaren Pflichtindizes aufgrund fachlicher Integritätsanforderungen;
- Messwerten für Baseline und logisch zulässige Einzelindexnutzung;
- optionalen Grenzen für Speicher, Indexanzahl und Write-Mehraufwand.

Die Fallstudie verwendet weiterhin eine gemeinsame Collection `products` mit den drei Query Shapes Q1 bis Q3:

1. aktive Produkte einer häufigen oder seltenen Kategorie, absteigend nach Preis, `limit(24)`;
2. aktive Produkte mit einem häufigen oder seltenen Tag, ohne explizite Sortierung, `limit(24)`;
3. Abruf eines bestehenden Produkts über die fachliche `productId`.

Untersucht werden 10.000, 100.000 und 500.000 synthetische Dokumente. Die geplanten Zielverteilungen bleiben kontrollierte Versuchsfaktoren; die tatsächlich beobachteten Verteilungen werden je Lauf im Manifest festgehalten.

Die bisher definierten Kandidaten I1 bis I8 für Kategorie-, Preis-, Aktivstatus- und Tagzugriffe bilden den optionalen Ausgangspool. Der eindeutige Index I9 auf `productId` wird als fachlicher Pflichtindex modelliert: Konfigurationen ohne garantierte Produkt-ID-Eindeutigkeit sind keine zulässigen Alternativen. Dadurch umfasst der optionale Suchraum zunächst `2^8 = 256` Sets; jedes ausgewertete Set enthält zusätzlich I9. Die fachliche Zulässigkeit und mögliche Redundanz der einzelnen Kandidaten werden in G2 nochmals geprüft.

## Methode

Die Untersuchung ist zweistufig angelegt und trennt Schätzung von tatsächlicher Setleistung.

### 1. Kandidatenprofilierung und Set-Screening

- Baseline und jeder für eine Query logisch zulässige optionale Kandidat werden unter kontrollierten Bedingungen einzeln profiliert; `hint()` dient hier ausschließlich der experimentellen Isolation.
- Erfasst werden insbesondere die mediane reale Queryzeit, `totalDocsExamined`, `totalKeysExamined`, `nReturned`, Fetch-/Sort-Stufen und Indexgröße. Strukturmetriken dienen als Plausibilitäts- und Stabilitätskontrolle der Zeitmessung.
- Für die Fallstudie werden die drei Query Shapes gleich gewichtet; Parametervarianten innerhalb einer Shape erhalten gleiche Anteile. Das ist eine Referenzannahme und keine Aussage über reale Shop-Häufigkeiten. Das Werkzeug akzeptiert optional andere Nutzergewichte.
- Für ein Set (S) wird die geschätzte Read-Kostenfunktion zunächst aus der günstigsten gemessenen, in (S) verfügbaren Zugriffsmöglichkeit je Query gebildet:

  `C_read_hat(S) = Summe_q w_q * min(Baselinekosten(q), Kandidatenkosten(q, i in S))`

- Alle zulässigen Teilmengen des optionalen Pools werden vollständig enumeriert. Ein heuristisches oder lernendes Suchverfahren ist für den kleinen Fallstudienraum nicht erforderlich.
- Jedes Set wird als Vektor aus geschätzten Read-Kosten, geschätztem Write-Aufwand und Indexspeicher bewertet. Dominierte Sets werden entfernt. Harte Grenzen können den zulässigen Raum zusätzlich einschränken. Ein eindeutiges „bestes“ Set wird nur bei Dominanz oder aufgrund einer ausdrücklich dokumentierten Präferenz beziehungsweise Nebenbedingung benannt.

Die Schätzung abstrahiert von Indexinteraktionen, Cacheeffekten und abweichenden Plannerentscheidungen. Sie dient deshalb nur zur systematischen Reduktion des Suchraums. Eine additive oder aus Einzelmessungen abgeleitete Write-Schätzung ist ebenfalls nur ein Screening-Proxy.

### 2. Materialisierte Finalvalidierung

Höchstens drei fachlich unterschiedliche Pareto-Finalisten werden transparent ausgewählt, etwa eine read-orientierte, eine ressourcenarme und eine unter expliziten Grenzen ausgewählte Konfiguration. Für jeden Finalisten gilt:

- sauberer, äquivalenter Ausgangszustand;
- ausschließlich Pflichtindizes und die optionalen Indizes dieses Sets;
- Queryausführung ohne `hint()`;
- getrennte Erfassung von Explain-Struktur und realer Laufzeit;
- gesamte Indexgröße;
- identischer Batch-Insert und identisches `isActive`-Update bei der repräsentativen Skalierung von 500.000 Ausgangsdokumenten;
- Vergleich der geschätzten mit den tatsächlich gemessenen Setkosten.

Vorgesehen bleiben 1.000 Inserts und 1.000 vorab festgelegte Aktivstatusänderungen je Finalist. Die exakten Wiederholungszahlen, Messreihenfolgen, Grenzen und Finalistenauswahlregeln werden vor der Umsetzung in G2 festgeschrieben.

## Scope

- allgemeine Methode für einen vom Nutzer definierten MongoDB-Workload und einen endlichen, bereits vorgegebenen Kandidatenpool;
- prototypischer Evaluator für Profilierungsergebnisse, vollständige Set-Enumeration, Pareto-Analyse, Constraints, Finalistenauswahl und Validierungsvergleich;
- kontrollierte E-Commerce-Fallstudie mit Q1 bis Q3, I1 bis I8 als optionalem Ausgangspool und I9 als Pflichtindex;
- drei Datenskalierungen und kontrollierte häufige beziehungsweise seltene Filterwerte;
- gleich gewichtete Query Shapes in der Fallstudie, optionale Gewichte im Werkzeug;
- Read-Kosten primär anhand medianer realer Laufzeiten, abgesichert durch Explain-Strukturmetriken;
- Indexspeicher und begrenzter Insert-/Aktivstatus-Update-Aufwand;
- vollständige Enumeration des kleinen optionalen Fallstudienraums;
- Pareto-Front und höchstens drei sauber materialisierte Finalisten;
- Vergleich zwischen geschätzter und tatsächlich gemessener Setleistung.

## Explizite Ausschlüsse

- automatische Erzeugung eines vollständigen oder global optimalen MongoDB-Indexraums;
- Anspruch auf ein universell bestes Indexset oder repräsentative Workload-Häufigkeiten für reale Shops;
- Reinforcement Learning, genetische Algorithmen oder andere Heuristiken für die kleine Fallstudie;
- alleinige Gleichsetzung von Plannerwahl, Explain-Zeit oder Einzelindexgewinn mit Seteignung;
- Nutzung von Hidden Indexes oder Query Settings als Ersatz für die physische Finalvalidierung;
- vollständige Modellierung aller Indexinteraktionen oder des WiredTiger-Cacheverhaltens;
- repräsentative gemischte Read-/Write-Last, konkurrierende Clients und Netzwerklatenzen;
- Delete-Workloads, Replikation, Sharding und Mehrknotencluster;
- Volltextsuche, MongoDB Search, Vector Search sowie Geospatial-, Hashed-, Wildcard- und Clustered-Indizes;
- Vergleich mit relationalen oder anderen Datenbanksystemen.

## Zielgruppe

Die Arbeit richtet sich an Studierende und Softwareentwickelnde mit grundlegenden MongoDB-Kenntnissen, die aus einem bekannten Workload und einem überschaubaren Kandidatenraum eine begründete Indexkonfiguration ableiten und deren Grenzen nachvollziehen möchten.

## Praktische Artefakte

- versionierte Spezifikation von Workload, Parametervarianten, Gewichten, Pflichtindizes und optionalen Kandidaten;
- reproduzierbare Baseline- und Einzelindexprofile;
- normalisierte Nutzen- und Kostenmatrix;
- vollständiger Enumerator für zulässige Indexsets;
- Pareto- und Constraint-Auswertung;
- nachvollziehbare Finalistenauswahl;
- isolierte Finalistenläufe ohne Hint;
- Vergleich von Schätzung und Messung sowie Validierungsbericht;
- Run-Manifeste, Explain-Rohdaten, Laufzeiten, Indexgrößen und Write-Messungen;
- automatisierte Tests für Konfiguration, Enumeration, Dominanz, Constraints und Ergebnisäquivalenz.

## Bekannte Risiken und offene Entscheidungen

- Reale Laufzeitmedianen reagieren auf Cache-, Reihenfolge- und Systemeffekte. Warmups, deterministische Rotation, Wiederholungen und Strukturmetriken müssen die Interpretation absichern.
- Das Minimum aus Einzelindexkosten bildet konkurrierende beziehungsweise interagierende Indizes und das natürliche Plannerverhalten nicht vollständig ab. Genau diese Modellabweichung ist Gegenstand der Finalvalidierung.
- Die Write-Schätzung aus Einzelkandidaten ist nur ein Proxy; belastbare Aussagen werden auf die tatsächlich materialisierten Finalisten begrenzt.
- Ein Pflichtindex auf `productId` verkleinert den Suchraum und trennt Integrität von optionaler Optimierung. Falls das Referenzschema stattdessen `productId` als `_id` modellieren soll, müssten Q3, Baseline und Kandidatenraum vor G2 nochmals angepasst werden.
- Die allgemeine Wiederverwendbarkeit des Prototyps betrifft Eingabe-, Such- und Auswertungslogik, nicht die automatische fachliche Erzeugung geeigneter Kandidaten.
- In G2 festzulegen sind die exakten normalisierten Berichtseinheiten, Constraints, Wiederholungszahlen, Pareto-Finalistenregel und der Umgang mit Sets, deren Eignung zwischen Skalen oder Selektivitäten wechselt.
- Der Lauf vom 24.07.2026 bleibt Pilot und ist keine Evidenz für die neue Forschungsfrage.
