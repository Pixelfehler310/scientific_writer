# Scoping-Recherche

Stand: 31.07.2026. Durch G2 freigegebenes Scoping nach Freigabe des workload-basierten Forschungsauftrags. Die Notizen prüfen Plausibilität, fachliche Tragfähigkeit und praktische Machbarkeit. Sie ersetzen weder die spätere Tiefenrecherche noch die Belegprüfung einzelner Manuskriptaussagen.

## Ergebnis der Neuausrichtung

Die Arbeit vergleicht nicht länger vier Indexstrategien in weitgehend getrennten Demonstrationsszenarien. Untersuchungsgegenstand ist ein gemeinsamer Referenzworkload auf einer festen Produkt-Collection. Für jede Query Shape werden vor dem neuen Benchmark fachlich plausible Kandidaten hergeleitet. Alle Kandidaten werden als Pool installiert, query-spezifisch kontrolliert und anschließend auf eine gemeinsame Ausgangskonfiguration reduziert. Erst die reduzierte Konfiguration wird als Gesamtheit gegen die Baseline validiert und einem begrenzten Schreibkostentest unterzogen.

Diese Ausrichtung entspricht dem Grundgedanken workload-basierter physischer Datenbankgestaltung: Eine Indexkonfiguration wird aus einem festgelegten Zugriffsmuster und nicht aus einer abstrakten Rangfolge von Indexarten abgeleitet. Chaudhuri und Narasayya (1997) liefern dafür den klassischen Forschungsbezug; Kossmann et al. (2020) stützen die mehrdimensionale Bewertung und die empirische Kontrolle von Empfehlungen. Die relationale Implementierung dieser Arbeiten wird nicht auf MongoDB übertragen, wohl aber das Prinzip der workload- und kostenbezogenen Auswahl.

## Abgrenzung des Referenzworkloads

Shopifys aktuelle Storefront API stellt paginierte Produktlisten, Filter unter anderem nach Verfügbarkeit, Kategorie, Preis und Tags sowie Sortierschlüssel einschließlich Preis bereit. Dies plausibilisiert die ausgewählten Abfragebestandteile, beweist aber keine universelle Häufigkeitsverteilung. TPC-W enthält historisch Katalog-Browse- und Produktdetailzugriffe, ist seit 2005 obsolet und dient nur als zusätzliche Plausibilisierung.

Die Arbeit verwendet deshalb die begrenzte Formulierung **„ausgewählte typische Query Shapes eines E-Commerce-Produktkatalogs“**. Weder die drei Queries noch ihre Häufigkeiten bilden einen repräsentativen Gesamtshop ab. Bestellungen, Warenkörbe, Kundenkonten, Volltextsuche und administrative Analysen bleiben ausgeschlossen.

### Q1 – Aktive Produktliste einer Kategorie

```javascript
db.products.find({
  category: <commonCategory|rareCategory>,
  isActive: true
}).sort({ price: -1 }).limit(24)
```

- **Aussagefunktion:** zentrale Storefront-Listenabfrage mit zwei Equality-Feldern, Sortierung und begrenzter Ausgabe.
- **Variationen:** häufige und seltene Kategorie; 10.000, 100.000 und 500.000 Dokumente.
- **Prüfziel:** Filterabdeckung, Sortierunterstützung, Compound-Feldreihenfolge, Partial-Index-Eignung und Präfixredundanz.
- **Grenze:** `limit(24)` ist eine festgelegte experimentelle Seitengröße, keine als Branchenstandard behauptete Zahl.

### Q2 – Aktive Produktliste nach Tag

```javascript
db.products.find({
  tags: <commonTag|rareTag>,
  isActive: true
}).limit(24)
```

- **Aussagefunktion:** Arrayfilterung auf einem Produktmerkmal mit zusätzlichem Aktivitätsprädikat.
- **Variationen:** häufiges und selektiveres Tag; dieselben drei Datenmengen.
- **Prüfziel:** Wirkung eines automatisch entstehenden Multikey-Index, zusätzlicher Equality-Schlüssel und partielle Begrenzung.
- **Grenze:** Ohne explizite Sortierung sind unterschiedliche, aber logisch gültige Teilmengen möglich. Verglichen werden deshalb Ergebnisanzahl, Prädikaterfüllung und Zugriffsaufwand; Identität derselben 24 Dokumente wird nicht als Query-Semantik vorausgesetzt.

### Q3 – Aktives Produktdetail über fachliche ID

```javascript
db.products.findOne({
  productId: <existingProductId>,
  isActive: true
})
```

- **Aussagefunktion:** hochselektiver Detailabruf eines konkreten Produkts.
- **Variationen:** Datenmenge; keine künstliche Selektivitätsvariation der eindeutigen ID.
- **Prüfziel:** Nutzen eines vollständigen Unique Index auf `productId` gegenüber der Baseline und Einordnung der Alternative, `productId` direkt als `_id` zu modellieren.
- **Grenze:** Die `_id`-Alternative wird konzeptionell diskutiert, aber nicht als zweite Dokumentstruktur benchmarked. Ein Partial Index auf `productId` wird nicht mehr als Standardkandidat empfohlen, weil er globale Eindeutigkeit und Abrufe inaktiver Produkte nicht allgemein absichert.

## Kandidatenpool

Alle benannten Kandidaten sollen innerhalb einer Skalierungsstufe gleichzeitig installiert werden. Kandidaten mit identischem Key Pattern werden über eindeutige Indexnamen gehintet. Die Kandidaten sind Untersuchungshypothesen, keine vorweggenommenen Empfehlungen.

| ID | Query | Indexdefinition | Begründete Erwartung |
| --- | --- | --- | --- |
| I1 | Q1 | `{category: 1}` | unterstützt den führenden Filter, aber weder Aktivstatus noch Preissortierung vollständig |
| I2 | Q1 | `{category: 1, price: -1}` | unterstützt Kategorie und Preisreihenfolge; `isActive` bleibt Fetch-Prädikat |
| I3 | Q1 | `{price: -1, category: 1, isActive: 1}` | priorisiert die Sortierreihenfolge und prüft eine plausible Sort-first-Alternative |
| I4 | Q1 | `{category: 1, isActive: 1, price: -1}` | folgt für die konkrete Query der Equality-Sort-Logik und kann Sortierarbeit vermeiden |
| I5 | Q1 | `{category: 1, price: -1}`, `partialFilterExpression: {isActive: true}` | unterstützt nur aktive Listen, reduziert dafür potenziell Größe und zu prüfende Einträge |
| I6 | Q2 | `{tags: 1}` | direkter Multikey-Zugriff; Aktivstatus wird nachgelagert geprüft |
| I7 | Q2 | `{tags: 1, isActive: 1}` | bindet beide Equality-Prädikate in einen Compound-Multikey-Index ein |
| I8 | Q2 | `{tags: 1}`, `partialFilterExpression: {isActive: true}` | begrenzt den Multikey-Index auf öffentlich aktive Produkte |
| I9 | Q3 | `{productId: 1}`, `unique: true` | unterstützt den punktuellen Zugriff und erzwingt die fachlich erwartete globale Eindeutigkeit |

Die Baseline besitzt nur den automatisch erzeugten `_id`-Index. Im vollständigen Kandidatenpool werden für Q1 die Kandidaten I1 bis I5, für Q2 I6 bis I8 und für Q3 I9 kontrolliert. Eine identische Kandidatenzahl je Query wäre methodisch künstlich und ist nicht vorgesehen.

## Fachliche Tragfähigkeit der Kandidaten

- **Compound-Feldreihenfolge:** MongoDB dokumentiert, dass die Reihenfolge der Schlüssel für Filter- und Sortierunterstützung entscheidend ist. Die ESR-Guideline ist eine Heuristik, kein universelles Gesetz. Q1 vergleicht deshalb Filter-, Sort-first-, ESR-orientierte und partielle Kandidaten unter identischen Queryparametern.
- **Compound-Präfixe und Redundanz:** Ein späterer Compound-Index kann einen separaten führenden Präfixindex funktional abdecken. Der kleinere Einzelindex I1 wird trotzdem zunächst gemessen, weil funktionale Abdeckung nicht automatisch gleiche Größe, Cachewirkung oder Laufzeit bedeutet. Er wird nur behalten, wenn sein zusätzlicher Nutzen die Speicher- und Schreibkosten rechtfertigt.
- **Multikey:** Ein Index auf `tags` wird automatisch zum Multikey-Index. In einem Compound-Multikey-Index darf pro Dokument höchstens ein indexiertes Feld ein Array sein; das Referenzschema erfüllt dies für I7. Q2 enthält keine Sortierung auf dem Arrayfeld und vermeidet damit eine zusätzliche, für die Forschungsfrage unnötige Sortierkomplexität.
- **Partial Index:** Ein Partial Index darf nur verwendet werden, wenn die Querybedingung seine Filterbedingung einschließt. Q1 und Q2 enthalten deshalb explizit `isActive: true`. Full- und Partial-Kandidaten mit gleichem Key Pattern werden über ihre Namen unterschieden; ihre gemeinsame technische Anlage wird vor dem finalen Lauf durch einen Smoke-Test abgesichert.
- **Unique `productId`:** MongoDB erzeugt bereits einen Unique Index auf `_id`. Da die Referenzstruktur zusätzlich `productId` verwendet, ist ein vollständiger Unique Index die fachlich konsistente Ausgangshypothese. Die Arbeit weist darauf hin, dass ein reales Neuprojekt die fachliche ID alternativ direkt als `_id` modellieren könnte und dadurch einen zusätzlichen Index vermeidet.
- **Unnötige Indizes:** MongoDB warnt, dass ein Index pro Query zu redundanten oder ungenutzten Indizes führen kann. Die Reduktionsphase ist daher kein nachträgliches Aufräumen, sondern Teil der Forschungslogik.

## Selektivität und Datenmengen

Die drei Skalierungsstufen bleiben 10.000, 100.000 und 500.000 Dokumente. Sie sind experimentelle Größen und keine Behauptung über einen durchschnittlichen Shop.

Für die neue Datengenerierung werden folgende Zielbereiche angestrebt:

- häufige Kategorie: ungefähr 30–40 % der Dokumente;
- seltene Kategorie: ungefähr 3–5 %;
- häufiges Tag: ungefähr 40–60 % der Dokumente;
- selektiveres Tag: ungefähr 5–10 %;
- aktive Produkte: ungefähr 80 %;
- global eindeutige `productId`-Werte.

Nicht die Generatorgewichte, sondern die tatsächlich beobachteten Trefferzahlen und Prävalenzen werden je Skalierungsstufe im Manifest gespeichert. Ein fester Seed sichert Reproduzierbarkeit. Der Kontrast ist bewusst kontrolliert und wird nicht als empirische Verteilung realer Shops ausgegeben.

## Mess- und Auswahlverfahren

### Baseline

Alle Queryvarianten werden zunächst ohne benutzerdefinierte Indizes ausgeführt. Für eine explizite Kontrollmessung kann der Collection Scan über `hint({$natural: 1})` erzwungen werden.

### Kontrollierte Kandidatenmessung

- Alle Kandidaten sind gleichzeitig installiert.
- Nur für die jeweilige Query fachlich geeignete Kandidaten werden über den Indexnamen mit `hint()` erzwungen.
- Pro Skalierung und Queryvariante sind drei Warmups und zehn gemessene Wiederholungen vorgesehen.
- Die Kandidatenreihenfolge wird je Wiederholung deterministisch rotiert.
- `explain("executionStats")` liefert Plan und Strukturmetriken.
- Die reale Query wird zusätzlich vollständig bis zum festgelegten Limit konsumiert und separat zeitlich gemessen.
- Partial Indizes werden nur bei erfüllter Filterbedingung gehintet.
- Ergebnisanzahl, Filterkorrektheit und – wo die Query eine deterministische Ordnung definiert – Ergebnisidentität werden geprüft.

### Natürliche Planner-Auswahl

Mit installiertem Kandidatenpool wird jede Query ohne Hint ausgeführt. Explain dokumentiert den gewählten Index und mögliche Sortier-/Fetch-Stufen. Da Explain bestehende Plan-Cache-Einträge ignoriert und `executionTimeMillis` nicht zwingend die steady-state-Laufzeit repräsentiert, werden Plannerentscheidung, Strukturmetriken und separat gemessene Queryzeit nicht gleichgesetzt.

### Reduktion und finale Validierung

Ein Kandidat wird entfernt, wenn er für den Referenzworkload strukturell dominiert, nicht gewählt, durch einen anderen Index hinreichend abgedeckt oder nur durch einen unverhältnismäßigen Speicherzuwachs gerechtfertigt wäre. Gibt es keinen eindeutigen Sieger, bleibt die Empfehlung bedingt.

Nach der Reduktion werden alle Query Shapes mit der finalen Konfiguration ohne Hint erneut gemessen. Erst dieser Lauf stützt die gemeinsame Indexaufstellung. Ausgewiesen werden die exakten `createIndex`-Definitionen, verwendete Indizes, Gesamtindexgröße und Bedingungen der Empfehlung.

## Begrenzte Schreibkostenprüfung

MongoDB aktualisiert bei Inserts die relevanten Einträge aller Indizes und bei Updates die von den geänderten Schlüsseln beziehungsweise Partial-Bedingungen betroffenen Indizes. Deshalb wäre eine uneingeschränkte Aussage über „Anwendungseffizienz“ bei reiner Lesemessung unangemessen.

Die Arbeit ergänzt bei 500.000 Ausgangsdokumenten eine begrenzte Gegenprüfung:

1. Einfügen eines deterministisch erzeugten Batches von 1.000 neuen Produkten;
2. Umschalten von `isActive` für 1.000 vorab festgelegte Produkte, sodass Ein- und Austritte aus möglichen Partial Indizes auftreten. Die Zieldokumente werden über ihre in beiden Konfigurationen indexierten `_id`-Werte adressiert, damit die Messung nicht durch einen zusätzlichen Lookup-Vorteil der finalen `productId`-Konfiguration verzerrt wird.

Verglichen werden ausschließlich Baseline und finale gemeinsame Konfiguration. Die Ausgangszustände, Batches, Write Concern und Bulk-Optionen müssen identisch sein; Auswahl und Rücksetzung der Testdokumente liegen außerhalb des gemessenen Intervalls. Fünf Wiederholungen und Median beziehungsweise Durchsatz sind vorgesehen. Delete-Performance, konkurrierende Writer und ein repräsentatives Lese-/Schreibverhältnis bleiben ausgeschlossen.

## Bewertungslogik

Die Bewertung verwendet keine ungewichtete Gesamtnote und bestimmt den „besten“ Index nicht aus einem einzelnen Laufzeitwert.

1. **Korrektheit und Eignung:** vollständiges, logisch korrektes Ergebnis; Partial-Bedingung erfüllt.
2. **Struktureller Aufwand:** `totalDocsExamined`, `totalKeysExamined`, `nReturned`, `COLLSCAN`, `IXSCAN`, `FETCH`, `SORT` und Verhältnis der geprüften Einträge zum Ergebnis.
3. **Laufzeit:** Median separat ausgeführter Queries; Explain-Zeit nur ergänzend.
4. **Ressourcen:** einzelne und gesamte Indexgröße; für das finale Set zusätzlich Insert-/Update-Aufwand.
5. **Workload-Abdeckung:** natürliche Planner-Auswahl, Compound-Präfixe, bedingte Nutzbarkeit und verbleibende Redundanz.

## Praktische Machbarkeit

Der bestehende Benchmark ist weiterhin eine geeignete technische Basis: Query- und Indexregistries, reproduzierbare Datenerzeugung, Plan-Cache-Behandlung, Explain-Parsing, Artefaktarchivierung und Tests sind vorhanden. Die Neuausrichtung erfordert jedoch relevante Anpassungen:

- neue Queryparameter, Limits und Selektivitätsverteilungen;
- Kandidatenpool statt szenarioweisem Indexwechsel;
- Hint-Unterstützung und getrennte tatsächliche Query-Zeitmessung;
- generalisierte Analyse statt fest codierter H1–H4-Auswertung;
- neue Diagramme und Konfigurationsmatrix;
- finales Kombiset-Profil und begrenzter Schreibtest;
- aktualisierte Unit-, Integrations- und End-to-End-Tests.

Der Lauf vom 24.07.2026 bleibt als Pilot erhalten, ist aber für die neue Forschungsfrage keine finale Evidenz. Nach Umsetzung und Tests ist ein neuer, sauber dokumentierter Lauf erforderlich.

## Konsequenzen für Gliederung und Aussageanspruch

1. Die Theorie erklärt Entwurfsprinzipien und Messgrößen, nicht nacheinander vier Indexarten als Selbstzweck.
2. Das Methodikkapitel enthält eine sichtbare Index-Design-Phase, die Kandidaten vor den Ergebnissen begründet.
3. Die Ergebnisse werden nach Query Shape berichtet und trennen Hint-Vergleich, natürliche Planner-Auswahl und Interpretation.
4. Die gemeinsame Konfiguration entsteht erst nach Redundanzprüfung und wird anschließend ohne Hint workloadweit validiert.
5. Der kleine Schreibtest quantifiziert einen Trade-off, rechtfertigt aber keine Aussage über die Effizienz der gesamten Anwendung.
6. Das Fazit liefert eine konkrete Referenzkonfiguration und einen Übertragungsleitfaden für ähnliche Daten- und Zugriffsmuster, keine universelle Shop-Vorlage.

## Noch offene Punkte für G3/G4 und Umsetzung

- Exakte bibliografische und methodische Fundstelle für reproduzierbare Datenbank-Mikrobenchmarks auswählen.
- Gleichzeitige Anlage der Full-/Partial-Kandidaten mit identischem Key Pattern in MongoDB 8.2.11 per Smoke-Test bestätigen und stets über Namen hintbar machen.
- Queryzeitmessung einschließlich vollständiger Cursor-Konsumierung implementieren und gegen Explain-Metriken trennen.
- Äquivalenzprüfung für limitierte, aber unsortierte Q2-Ergebnisse als Prädikat- und Kardinalitätsprüfung spezifizieren.
- Identische Ausgangszustände der fünf Schreibwiederholungen technisch absichern.
- Nach dem neuen Lauf entscheiden, welche Kandidaten die finale Konfiguration tatsächlich bilden; diese Auswahl darf in G2 nicht vorweggenommen werden.
