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

- **Funktion:** Dokumentmodell und Aggregatbezug knapp einführen.
- **Kernaussage:** Produktattribute können als gemeinsam gelesener Dokumentverbund modelliert werden; diese Aggregation schafft die Datenbasis, legt aber noch keinen effizienten Zugriffspfad fest.
- **Begründung:** Die spätere Indexdiskussion braucht nur den Zusammenhang zwischen Produktdokument, Arrays/eingebetteten Feldern und Zugriffsmustern.
- **Evidenzbedarf:** Dokumentdatenbank- und Aggregatgrundlage.
- **Evidenztyp:** `external`
- **Evidenzstatus:** `planned`
- **Beziehung davor:** Übernahme aus der Fallankündigung der Einleitung.
- **Beziehung danach:** Folge — die späteren Zugriffsmuster bestimmen, welche Felder und Kombinationen relevant werden.
- **Zielwörter:** 140.
- **Medium:** keines.
- **Offene Recherche:** genaue Ausgabe-/Seitenangaben vorhandener Fachbücher prüfen.
- **Bestandsübernahme:** bevorzugt; geeignete vorhandene Absatzblöcke wortgleich samt später zuzuordnender Prüfhistorie.

### TH-02-P02 — Query Shapes und Selektivität

- **Funktion:** Query Shape als Ausgangspunkt des Indexentwurfs und Selektivität als Interpretationsgröße einführen.
- **Kernaussage:** Filter, Sortierung und Projektion beschreiben, welche Daten eine Query benötigt; die Selektivität ihrer Bedingungen bestimmt, wie stark sie den Produktbestand eingrenzen. Deshalb werden enge und breite Preisbereiche sowie häufige und seltene Tags beziehungsweise Marken getrennt untersucht.
- **Begründung:** Bevor ein Zugriffspfad oder eine Feldreihenfolge erklärt wird, muss klar sein, was Q1 bis Q3 fachlich anfordern und warum Varianten nicht zu einem Durchschnitt verschmolzen werden.
- **Evidenzbedarf:** MongoDB-Dokumentation zu Query Shapes und Selektivität; begriffliche Einordnung.
- **Evidenztyp:** `external`
- **Evidenzstatus:** `planned`
- **Beziehung davor:** Folge.
- **Beziehung danach:** Ursache — geordnete Indizes können nur für einen durch den Shape und seine Grenzen bestimmten Schlüsselbereich arbeiten.
- **Zielwörter:** 150.
- **Medium:** optionaler Verweis auf die kompakte Query-Shape-zu-Indexset-Tabelle in Kapitel 3, keine Doppelung.
- **Offene Recherche:** Abgrenzung von Query Shape und zusätzlich festgelegten Workloadparametern wie Limit präzise formulieren.
- **Bestandsübernahme:** teilweise; vorhandene produktkatalogspezifische Absätze nur bei Übereinstimmung mit Q1–Q4 übernehmen, Selektivitätsbezug ergänzen und erneut prüfen.

### TH-02-P03 — Geordnete Indexzugriffe und ihr Pflegeaufwand

- **Funktion:** Kurz erklären, wie geordnete Baumstrukturen Suchräume begrenzen können, und den Zielkonflikt zu Speicher und Writes begründen.
- **Kernaussage:** MongoDB-Indizes organisieren Schlüssel geordnet in baumartigen Zugriffspfaden. Bei Equality- oder Range-Bedingungen kann die Suche an einem passenden Schlüsselbereich beginnen und Teilbereiche außerhalb der Grenzen auslassen; bei breiten oder wenig selektiven Bedingungen bleibt der relevante Bereich dennoch groß. Zusätzliche Indizes benötigen Speicher und ihre Einträge müssen bei Inserts, Deletes sowie Änderungen indexierter Werte gepflegt werden.
- **Begründung:** Dieser Absatz erklärt zugleich, warum Indexe Q1-eng, Q1-breit und die Häufigkeitsvarianten unterschiedlich unterstützen können und warum zusätzliche Indizes Write-Aufwand verursachen. Er behauptet keine nicht überprüfte MongoDB-interne Rebalancing-Logik.
- **Evidenzbedarf:** B-Baum-Grundlage, MongoDB-Indexbeschreibung und Write-Performance-Dokumentation.
- **Evidenztyp:** `external`
- **Evidenzstatus:** `planned`
- **Beziehung davor:** Ursache.
- **Beziehung danach:** Folge — Indexfelder und -eigenschaften müssen passend zu den Shapes gewählt werden.
- **Zielwörter:** 180.
- **Medium:** keines.
- **Offene Recherche:** MongoDB-spezifische Terminologie am Original verifizieren; Pflegekosten präzise von allgemeinem Baum-Rebalancing trennen.
- **Bestandsübernahme:** bevorzugt aus den vorhandenen Absätzen zu Indexzugriff, Balance/Bereich und Write-Kosten, aber als zusammenhängender neuer Absatz erst nach Evidenzprüfung.

### TH-02-P04 — Indexentwurf: Compound-Indizes, Präfixe und ESR

- **Funktion:** Die allgemeine Entwurfslogik für die Q1-/Q3-Unterstützung und die Unterschiede zwischen L und W1 erklären.
- **Kernaussage:** Compound-Indizes ordnen mehrere Felder in einer festen Reihenfolge; geeignete Präfixe können weitere Zugriffsmuster unterstützen. Die ESR-Regel ordnet Equality-, Sort- und Range-Anteile als Entwurfsleitlinie, ersetzt aber keine Prüfung gegen den konkreten Shape, seine Selektivität und die Datenmenge.
- **Begründung:** Der Spezialindex von L und die bewusste Wiederverwendung eines Q1-orientierten Indexes durch W1 lassen sich nur über Feldreihenfolge, Präfixe und die Grenze von ESR verständlich vergleichen.
- **Evidenzbedarf:** MongoDB-Primärdokumentation zu Compound-Präfixen, Sortierung und ESR.
- **Evidenztyp:** `external`
- **Evidenzstatus:** `planned`
- **Beziehung davor:** Folge.
- **Beziehung danach:** Fortführung — Multikey-, Partial- und Unique-Eigenschaften ergänzen diese Entwurfsentscheidung für Q2, W2 und die gemeinsame Basis.
- **Zielwörter:** 210.
- **Medium:** TH-M01.
- **Offene Recherche:** Range-und-Sort-Grenze für die konkrete Q1-Feldreihenfolge präzise prüfen.
- **Bestandsübernahme:** bevorzugt; Sortierrichtung und projektspezifische Beispiele fachlich angleichen und danach neu prüfen.

### TH-02-P05 — Indexentwurf: Multikey, Partial und Unique

- **Funktion:** Die besonderen Indexeigenschaften in denselben Entwurfsblock einordnen.
- **Kernaussage:** Das Arrayfeld `tags` erzeugt Multikey-Verhalten; ein Partial Index ist nur für Queries mit passender Filterbedingung vollständig nutzbar; der gemeinsame Unique-Index auf `productId` verbindet Detailzugriff und Integritätsregel. Diese Eigenschaften unterscheiden insbesondere Q2, W2 und die gemeinsame Basis aller Sets.
- **Begründung:** Multikey, Partial und Unique sind keine isolierten Indextypen im Kapitel, sondern konkrete Gestaltungsoptionen desselben vollständigen Indexsets.
- **Evidenzbedarf:** MongoDB-Primärdokumentation zu Multikey-, Partial- und Unique-Regeln sowie Write-Pflege bei Partial-Indizes.
- **Evidenztyp:** `external`
- **Evidenzstatus:** `planned`
- **Beziehung davor:** Fortführung.
- **Beziehung danach:** Folge — erst der Planner macht sichtbar, welcher dieser Zugriffspfade für eine konkrete Query gewählt wird.
- **Zielwörter:** 210.
- **Medium:** TH-M01.
- **Offene Recherche:** bisher ungeprüften Unique- und Partial-Write-Teil erstmals am Original prüfen.
- **Bestandsübernahme:** Multikey und Partial bevorzugt; Unique- und Partial-Write-Aussagen nur nach neuer Prüfung.

### TH-02-P06 — Planwahl, Planstufen und Explain-Analyse

- **Funktion:** Planner, sichtbare Planstufen und Explain-Metriken als zusammenhängende Beobachtungsebene erklären.
- **Kernaussage:** Auf Basis von Query Shape und verfügbaren Indizes kann MongoDB unterschiedliche Pläne wählen. `COLLSCAN`, `IXSCAN`, `FETCH` und `SORT` machen Collection-, Index-, Dokument- und Sortierarbeit sichtbar; `nReturned`, `totalDocsExamined` und `totalKeysExamined` quantifizieren den ausgeführten Plan. Ein `IXSCAN` allein beweist jedoch keine Eignung, und Explain-Zeit ist wegen abweichenden Plan-Cache-Verhaltens nicht mit wiederholter End-to-End-Latenz gleichzusetzen.
- **Begründung:** Die Evaluation benötigt eine gemeinsame Sprache, um die Setwirkung technisch zu erklären, ohne Planstruktur oder Explain-Zeit als Ersatz für die Latenzmessung zu behandeln.
- **Evidenzbedarf:** versionspassende MongoDB-Dokumentation zu Planwahl, Explain-Ausgabe und Kennzahlen; Forschung zur Begrenzung der Plannerinterpretation.
- **Evidenztyp:** `external`
- **Evidenzstatus:** `planned`
- **Beziehung davor:** Folge.
- **Beziehung danach:** Übergabe an `method` — der kontrollierte Setvergleich misst deshalb normale Latenz und Explain getrennt.
- **Zielwörter:** 270.
- **Medium:** optionaler schematischer Planbaum nur, falls er die gemeinsame Lesart der Stufen besser als Prosa trägt.
- **Offene Recherche:** konkrete MongoDB-Version und Planrepräsentation in Kapitel 3 dokumentieren; keine Versionsgrenze als eigenen Grundlagenabsatz schreiben.
- **Bestandsübernahme:** bevorzugt für Planstufen und Metriken; Plannerbeschreibung nur nach Versionsabgleich; ungeprüften Hint-/Rejected-Plans-Exkurs weglassen.

## Medienplan

| Medium-ID | Typ | Aussagefunktion | Herkunft/Erzeugung | Beschriftung | Einführung und Interpretation |
| --- | --- | --- | --- | --- | --- |
| TH-M01 | Tabelle | Zeigt für Q1–Q3, welche Indexeigenschaften und Designrollen jeweils relevant sind | eigene Synthese nach geprüften Grundlagen | „Query Shapes, Indexentwurf und beobachtbare Größen“ | Nach TH-02-P05 einführen; TH-02-P06 erklärt, wie die Wirkung dieser Entwürfe beobachtet wird |

## Offene Entscheidungen

- Nach G3 für jeden Bestandsabsatz entscheiden: `exact reuse`, `editorial`, `structural` oder `substantive`; im Zweifel `substantive`.
- Zielumfang nach evidenzbasierter Auswahl ganzer Absätze erneut schätzen; keine Sätze nur zur Budgeterfüllung zerschneiden.
