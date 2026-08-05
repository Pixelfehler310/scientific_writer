# Argumentationslinie und Gliederung

**Status:** Entwurf zur menschlichen Prüfung für G2  
**Grundlage:** freigegebener Brief (G1) und Scoping-Recherche vom 04.08.2026

## Forschungsfrage

Welche MongoDB-Indexsets eignen sich für einen definierten E-Commerce-Produktkatalog-Workload unter Berücksichtigung von Query-Latenz, strukturellen Explain-Metriken, Indexspeicher und Write-Aufwand, und welche bedingte Empfehlung lässt sich daraus ableiten?

## Argumentationslinie

1. Ein einzelner passender Index beantwortet noch nicht, welches vollständige Set für mehrere Lese- und Schreiboperationen geeignet ist.
2. B-Bäume erklären die Grundlage geordneter Indexzugriffe; Query Shapes, Indexpräfixe, ESR, Multikey-/Partial-Eigenschaften und MongoDBs Planwahl erklären, warum konkrete Indexsets unterschiedlich wirken und kosten.
3. Der Produktkatalog, die Query-Varianten und die Sets B/L/W1/W2 werden vor der Ergebnisauswertung vollständig operationalisiert und unter gleichen Bedingungen gemessen.
4. Latenz und strukturelle Explain-Metriken zeigen die Wirksamkeit pro Lesevariante; Speicher und Write-Batches zeigen die Gegenkosten.
5. Die gemeinsame Betrachtung macht Dominanz, Zielkonflikte und Grenzen des synthetischen Einzelclient-Falls sichtbar.
6. Daraus folgt eine auf den dokumentierten Versuchsraum begrenzte Empfehlung für Lese- beziehungsweise Ressourcenpriorität.

## Wortbudget

| Block | Kapitel | Zielwörter |
| --- | --- | ---: |
| Einleitung | 1 | 400 |
| Theorie/Grundlagen | 2 | 1.100 |
| Hauptteil | 3 | 850 |
| Hauptteil | 4 | 1.250 |
| Fazit | 5 | 400 |
| **Gesamt** |  | **4.000** |

Literaturverzeichnis, Verzeichnisse, Tabellen-/Abbildungsbeschriftungen, Anhänge und interne Absatz-ID-Kommentare zählen vorbehaltlich späterer institutioneller Klärung nicht zum Fließtextbudget.

## Kapitel

### intro — Einleitung

- **Funktion:** Vom praktischen Mehrindex-Problem zur präzisen Forschungsfrage führen und den begrenzten Erkenntnisanspruch festlegen.
- **Teilfrage:** Warum ist der Vergleich vollständiger Indexsets für einen definierten Workload ein relevantes und abgrenzbares Problem?
- **Erwartetes Ergebnis:** Forschungsfrage, Beitrag, Methode in einem Satz, Scope und Aufbau sind konsistent angekündigt.
- **Voraussetzungen:** freigegebener Brief.
- **Übergabe an Folgekapitel:** benennt die Begriffe und Wirkmechanismen, die zur späteren Ergebnisinterpretation benötigt werden.
- **Zielwörter:** 400.
- **Evidenzbedarf:** knapper Beleg für Lesevorteile und Write-/Speicherkosten von Indizes; keine Messbefunde vorwegnehmen.
- **Praktische Artefakte:** keine.
- **Mögliche Medien:** keine; die Forschungsfrage benötigt keine Abbildung.

### foundations — Warum Indexsets unterschiedlich wirken

- **Funktion:** Nur die theoretischen Mechanismen erklären, die später Pläne, Scanaufwand und Zielkonflikte interpretierbar machen.
- **Teilfrage:** Welche Eigenschaften von MongoDB-Indizes und Planwahl erklären Unterschiede zwischen B, L, W1 und W2?
- **Erwartetes Ergebnis:** Ein begrifflicher Interpretationsrahmen aus B-Baum-Grundlage und Dokumentmodell/Query Shape, Compound-Präfixen und ESR, Multikey/Partial, Planwahl/Explain sowie Speicher-/Write-Kosten. Der B-Baum-Teil erklärt knapp, wie geordnete Schlüsselzugriffe Bereichssuche und passende Sortierung unterstützen und warum jedes zusätzliche Indexpflegearbeit bei Writes erzeugt.
- **Voraussetzungen:** Query- und Setdefinitionen aus Brief und Scoping als Auswahlfilter für den Theorieumfang.
- **Übergabe an Folgekapitel:** liefert begründete Designkriterien für die vorab festgelegten Kandidaten und Messgrößen.
- **Zielwörter:** 1.100.
- **Evidenzbedarf:** Bayer/McCreight (1972) und Comer (1979) für die allgemeine B-Baum-Grundlage; MongoDB-Primärdokumentation zur konkreten Semantik und zu Write-Kosten; Fachliteratur zu Query-Verarbeitung; Tao et al. als Begrenzung der Planwahlinterpretation; keine ungesicherte Behauptung über interne MongoDB-Baumvarianten.
- **Praktische Artefakte:** keine Messergebnisse; optional schematische Query-Shape-zu-Indexset-Tabelle.
- **Mögliche Medien:** eine kompakte Tabelle, die Mechanismus, beobachtbare Explain-Größe und spätere Interpretationsfunktion verbindet.

### method — Vorab festgelegter, reproduzierbarer Setvergleich

- **Funktion:** Untersuchungsraum und Messprotokoll so beschreiben, dass Ergebnisse ohne nachträgliche Kandidatenanpassung reproduzierbar sind.
- **Teilfrage:** Wie werden Daten, Q1–Q4, W1/W2a/W2b und B/L/W1/W2 kontrolliert verglichen?
- **Erwartetes Ergebnis:** eingefrorenes Datenmodell, Verteilungen, Parameter, Indexdefinitionen, Version/Digest, Reset-/Rotationslogik, Warmups, Wiederholungen, getrennte Latenz-/Explain-Erhebung, Ergebnisvalidierung und Auswertungsregel.
- **Voraussetzungen:** Theoriebegriffe; funktionierender technischer Pilot darf Machbarkeit bestätigen, aber keine Kandidaten nach Ergebnisgüte auswählen.
- **Übergabe an Folgekapitel:** definiert, welche Beobachtungen als eigene empirische Evidenz gelten und wie sie zu lesen sind.
- **Zielwörter:** 850.
- **Evidenzbedarf:** Benchmarkmethodik, MongoDB-Explain-/Plan-Cache-Dokumentation und interne Artefaktpfade; Implementierungsdetails nur soweit für Reproduzierbarkeit nötig.
- **Praktische Artefakte:** Konfigurationsmanifest, Git-Commit, Image-Digest, Serverversion, Seeds, Registry-Snapshot, Rohdatenformat und Testnachweise.
- **Mögliche Medien:** Tabelle mit B/L/W1/W2; Ablaufdiagramm nur, falls es Reset, Rotation und getrennte Messpfade klarer als Prosa zeigt.

### evaluation — Von Szenarioergebnissen zur bedingten Empfehlung

- **Funktion:** Eigene Messbefunde darstellen, technisch erklären, über Kosten abwägen und in ihrer Reichweite begrenzen.
- **Teilfrage:** Welche Sets wirken je Lesevariante, welche Speicher-/Write-Kosten entstehen und welche bedingte Empfehlung folgt?
- **Erwartetes Ergebnis:** erstens scenario-lokale Befunde, zweitens strukturelle Erklärung über Explain, drittens Speicher-/Write-Vergleich, viertens Dominanz- und Prioritätsanalyse, fünftens Limitationen.
- **Voraussetzungen:** eingefrorener erfolgreicher Referenzlauf und validierte Ergebnisgleichheit.
- **Übergabe an Folgekapitel:** liefert die expliziten Antwortbausteine für die Forschungsfrage.
- **Zielwörter:** 1.250.
- **Evidenzbedarf:** primär eigene Rohdaten/Manifeste; Literatur nur zur Erklärung oder Begrenzung, niemals zur Ersetzung eigener Messwerte.
- **Praktische Artefakte:** Median/IQR je Q1–Q3-Variante und Set; Explain-Kennzahlen/Planstufen; Indexgrößen; W1/W2a/W2b; Validierungsstatus.
- **Mögliche Medien:** maximal drei dichte Medien: Lesevergleich, struktureller Aufwand, Speicher/Write-Trade-off. Jede Grafik muss im Text eingeführt und interpretiert werden.

### conclusion — Begrenzte Antwort und Ausblick

- **Funktion:** Forschungsfrage direkt beantworten, Empfehlung konditionieren und sinnvolle Erweiterungen vom erreichten Befund trennen.
- **Teilfrage:** Welches Set ist unter welcher Priorität im untersuchten Fall geeignet, und was folgt ausdrücklich nicht daraus?
- **Erwartetes Ergebnis:** knappe Antwort für Lese- und Ressourcenpriorität, zentrale Einschränkungen und methodisch passende nächste Untersuchungen.
- **Voraussetzungen:** abgeschlossene Evaluation und Diskussion.
- **Übergabe:** keine neue Evidenz; Abschluss der Argumentationskette.
- **Zielwörter:** 400.
- **Evidenzbedarf:** ausschließlich Rückbezug auf belegte Grundlagen und eigene validierte Ergebnisse.
- **Praktische Artefakte:** keine neuen.
- **Mögliche Medien:** keine.

## Festgelegte Designentscheidungen für die Kapitelplanung

- Q1-eng/breit, Q2-häufig/selten, Q3-häufig/selten und Q4 verwenden die im Scoping angegebenen Filter, `price: 1` und `limit: 24`.
- Bestandsgrößen sind 1.000, 10.000 und 100.000 Dokumente; 1.000 dient primär Smoke/Plausibilität.
- Alle Sets enthalten `_id_` und einen eindeutigen `{productId: 1}`-Index; zusätzliche Indizes entsprechen der B/L/W1/W2-Tabelle im Scoping.
- Normale Query-Latenz und Explain werden getrennt erhoben; Hauptkennzahlen sind Median und IQR bei 30 Wiederholungen, nicht p95.
- Writes werden zehnmal aus identischem Ausgangszustand gemessen; `stock` und `price` bleiben getrennte Updates.
- Es gibt keinen Gesamtscore und keine versteckten Query-Gewichte; Empfehlungen werden je Prioritätsprofil formuliert.
- Der technische Pilot darf Installierbarkeit, Korrektheit, Laufzeit und Artefakte prüfen, aber keine Kandidaten anhand günstiger Ergebnisse umgestalten.

## Offene Strukturentscheidungen

Keine. Änderungen an Kapitelreihenfolge, Wortbudget, Query-Parametern oder Indexsets nach G2 invalidieren mindestens die Kapitelplanung und alle nachgelagerten Artefakte; fachliche Änderungen am Untersuchungsdesign können zusätzlich Outline und praktische Evidenz invalidieren.
