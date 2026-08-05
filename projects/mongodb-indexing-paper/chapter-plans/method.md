# Kapitelplan: method — Vorab festgelegter, reproduzierbarer Setvergleich

## Funktion im Gesamtargument

Das Kapitel operationalisiert die Forschungsfrage vollständig, bevor Ergebnisse erscheinen. Es trennt fachliche Setdefinition, technische Umsetzung und Messprotokoll so, dass der Referenzlauf reproduzierbar und gegen nachträgliche Kandidatenanpassung geschützt ist.

## Teilfrage und erwartetes Ergebnis

Wie werden Datenmodell, Workload, B/L/W1/W2 und Messdimensionen kontrolliert verglichen? Ergebnis ist ein prüfbares Versuchsprotokoll mit klaren internen Artefakten.

## Wortbudget

850 Wörter.

## Voraussetzungen und Übergabe

Voraussetzungen sind die in `foundations` erklärten Mechanismen und ein technisch erfolgreicher Pilot. Das Kapitel übergibt an `evaluation` exakt die zulässigen Beobachtungen, Kennzahlen und Aussagegrenzen.

## Absatzplan

### ME-03-P01 — Untersuchungslogik und Vorabfestlegung

- **Funktion:** Designprinzip und Schutz vor Ergebnisanpassung erklären.
- **Kernaussage:** Vier vollständige Sets werden aus denselben Ausgangsdaten gemessen; Kandidaten und Parameter stehen vor dem Referenzlauf fest.
- **Begründung:** Der Direktvergleich ersetzt Enumeration und iterative Ergebnisoptimierung.
- **Evidenzbedarf:** Benchmarkmethodik und freigegebene Projektentscheidung.
- **Evidenztyp:** `external`
- **Evidenzstatus:** `planned`
- **Beziehung davor:** Übernahme der theoretischen Synthese.
- **Beziehung danach:** Konkretisierung durch Datenmodell und Verteilungen.
- **Zielwörter:** 105.
- **Medium:** Ablaufübersicht optional.
- **Offene Recherche:** Benchmarkfallen präzise zuordnen.

### ME-03-P02 — Produktmodell und Verteilungen

- **Funktion:** Datengrundlage reproduzierbar definieren.
- **Kernaussage:** Deterministischer Seed erzeugt 1.000/10.000/100.000 Dokumente mit dokumentierten Kategorie-, Marken-, Tag-, Aktivitäts- und Preisverteilungen sowie benötigten Write-Feldern.
- **Begründung:** Selektivitätsvarianten und Skalen benötigen nachvollziehbare Verteilungen.
- **Evidenzbedarf:** interne Generator- und Seed-Artefakte.
- **Evidenztyp:** `internal`
- **Evidenzstatus:** `planned`
- **Beziehung davor:** Konkretisierung.
- **Beziehung danach:** Folge — Workloadparameter greifen genau auf diese Verteilungen zu.
- **Zielwörter:** 115.
- **Medium:** kompakte Verteilungstabelle.
- **Offene Recherche:** keine; technische Implementierung und Pilotvalidierung offen.

### ME-03-P03 — Q1–Q4 und Write-Batches

- **Funktion:** Operationen und Varianten vollständig festlegen.
- **Kernaussage:** Sechs Lesevarianten plus Q4 sowie W1/W2a/W2b verwenden feste Filter, Sortierung, `limit: 24`, Produktmengen und gleiche Ziel-IDs.
- **Begründung:** Ohne feste Parameter wären Setvergleiche nachträglich steuerbar.
- **Evidenzbedarf:** interne Scenario-Registry und Manifest.
- **Evidenztyp:** `internal`
- **Evidenzstatus:** `planned`
- **Beziehung davor:** Folge.
- **Beziehung danach:** Fortführung — Sets bilden die unabhängige Variable.
- **Zielwörter:** 125.
- **Medium:** Query-Varianten-Tabelle.
- **Offene Recherche:** keine.

### ME-03-P04 — Vollständige B/L/W1/W2-Sets

- **Funktion:** Kandidaten und Designkontrast dokumentieren.
- **Kernaussage:** Gemeinsame `_id`-/Unique-Basis und zusätzliche Indexdefinitionen unterscheiden Spezialunterstützung, Wiederverwendung und Partial-/Schlüsselumfang.
- **Begründung:** Nur vollständige, verifizierte Zustände beantworten die Forschungsfrage.
- **Evidenzbedarf:** interne Konfigurationsregistry; theoretische Begründung aus Kapitel 2.
- **Evidenztyp:** `internal`
- **Evidenzstatus:** `planned`
- **Beziehung davor:** Fortführung.
- **Beziehung danach:** Folge — Messablauf muss jeden Zustand fair herstellen.
- **Zielwörter:** 125.
- **Medium:** B/L/W1/W2-Tabelle.
- **Offene Recherche:** keine, sofern G2-Definition unverändert bleibt.

### ME-03-P05 — Reset, Rotation und Versionskontrolle

- **Funktion:** Vergleichbarkeit und Umgebungsprotokoll erklären.
- **Kernaussage:** Jeder Block nutzt identischen Datenzustand, deterministische Rotation und dokumentierte MongoDB-/Image-/Git-/Serverparameter.
- **Begründung:** Reihenfolge, Drift und mutable Softwarestände dürfen keine Strategie systematisch bevorzugen.
- **Evidenzbedarf:** Benchmarkmethodik und interne Manifeste.
- **Evidenztyp:** `external`
- **Evidenzstatus:** `planned`
- **Beziehung davor:** Folge.
- **Beziehung danach:** Fortführung — konkrete Messpfade werden getrennt.
- **Zielwörter:** 115.
- **Medium:** kein zusätzliches Medium, falls Ablaufübersicht ME-M01 genutzt wird.
- **Offene Recherche:** laufende Server-Patchversion erfassen.

### ME-03-P06 — Getrennte Latenz-, Explain- und Write-Messung

- **Funktion:** Messprotokoll und Kennzahlen festlegen.
- **Kernaussage:** Normale Queries erhalten drei Warmups und 30 Wiederholungen; Explain wird separat erhoben; Writes werden zehnmal aus identischen Zuständen gemessen.
- **Begründung:** Explain-Zeit darf normale Latenz nicht ersetzen; Write-Kosten brauchen eigene Resetlogik.
- **Evidenzbedarf:** MongoDB-Plan-Cache-/Explain-Dokumentation und interne Runner-Artefakte.
- **Evidenztyp:** `external`
- **Evidenzstatus:** `planned`
- **Beziehung davor:** Fortführung.
- **Beziehung danach:** Folge — Validierung und Auswertung bestimmen zulässige Ergebnisse.
- **Zielwörter:** 145.
- **Medium:** Ablaufübersicht.
- **Offene Recherche:** genaue Client-seitige Zeitgrenze im Artefakt dokumentieren.

### ME-03-P07 — Korrektheit und Auswertungsregel

- **Funktion:** Ergebnisvalidierung und entscheidungslogische Grenze definieren.
- **Kernaussage:** Ergebnissignaturen müssen übereinstimmen; ausgewertet werden Median/IQR, Strukturmetriken, Speicher und Write-Kosten ohne Gesamtscore, mit bedingten Prioritätsempfehlungen.
- **Begründung:** Eine semantisch abweichende Query darf nicht als schnellere Strategie gelten.
- **Evidenzbedarf:** interne Validierungs-/Analyseartefakte; eigene vorab freigegebene Entscheidungslogik.
- **Evidenztyp:** `internal`
- **Evidenzstatus:** `planned`
- **Beziehung davor:** Folge.
- **Beziehung danach:** Übergabe an Ergebnisdarstellung.
- **Zielwörter:** 120.
- **Medium:** keines.
- **Offene Recherche:** Dominanzbegriff konsistent und ohne Pareto-Screening-Altlogik verwenden.

## Medienplan

| Medium-ID | Typ | Aussagefunktion | Herkunft/Erzeugung | Beschriftung | Einführung und Interpretation |
| --- | --- | --- | --- | --- | --- |
| ME-M01 | Ablaufdiagramm | Reset, Setinstallation, Query-Warmup/Latenz, separates Explain und Writes in korrekter Reihenfolge zeigen | eigene Darstellung aus finalem Runner | „Kontrollierter Ablauf eines Messblocks“ | In ME-03-P05 einführen und in ME-03-P06 interpretieren |
| ME-M02 | Tabelle | Query-Varianten und Sets platzsparend vollständig festhalten | Registry/Manifest | „Vorab festgelegter Workload und Indexsets“ | In ME-03-P03 und ME-03-P04 zweigeteilt erläutern |

## Offene Entscheidungen

- Medien ME-M01/ME-M02 nur verwenden, wenn sie zusammen weniger Raum als gleichwertige Prosa benötigen.
