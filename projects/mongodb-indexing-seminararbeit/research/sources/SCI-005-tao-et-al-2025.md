---
source_id: SCI-005
bibliography: "Tao, Dawei; Liu, Enqi; Randeni Kadupitige, Sidath; Cahill, Michael; Fekete, Alan; Röhm, Uwe (2025): First Past the Post: Evaluating Query Optimization in MongoDB. In: Databases Theory and Applications, LNCS 15449, S. 99–113."
identifier: "https://doi.org/10.1007/978-981-96-1242-0_8"
source_type: peer_reviewed_conference_paper
quality_score: 5
edition_or_version: "ADC 2024 Proceedings, veröffentlicht 2025; DOI-Fassung"
locations:
  - "S. 99–103: FPTP-Planwahl, Trial-Phase und Plan Cache"
  - "S. 106–111: empirische Fälle nachteiliger Index Scans"
target_chapters: [foundations, evaluation]
status: accepted
long_excerpt_approved_by: null
long_excerpt_approval_reason: null
---

# Quellensteckbrief

## Relevanz und Qualitätsbewertung

Peer-reviewte MongoDB-spezifische Untersuchung der trial-basierten Planwahl. Sie ergänzt die relationale Index-Advisor-Literatur um einen direkten Produktbezug und begrenzt die Gleichsetzung von Indexnutzung und Laufzeitvorteil.

## Eigene Zusammenfassung

Die Studie beschreibt Kandidatenpläne, eine kurze Round-Robin-Testphase, produktivitätsbasierte Auswahl und Wiederverwendung über den Plan Cache. In den untersuchten Fällen kann ein Index Scan gegenüber einem Collection Scan nachteilig sein.

## Geprüfte Fundstellen

- S. 99–103: Untersuchungsgegenstand, FPTP-Auswahl und Plan-Cache-Bezug.
- S. 106–111: empirische Auswertung und nachteilige Index-Scan-Fälle.

## Allgemeine Grenzen und Kontext

Die Experimente verwenden MongoDB 7.0.1 und einfache Queries mit zwei Range-Prädikaten. Quantitative Befunde und konkrete Mechanismusdetails werden nicht ungeprüft auf MongoDB 8.2.11 oder den Referenzworkload übertragen. Die erweiterte arXiv-Fassung derselben Studie zählt nicht als unabhängiger zweiter Beleg.
