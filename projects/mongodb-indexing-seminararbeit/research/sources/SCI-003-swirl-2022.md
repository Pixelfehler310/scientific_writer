---
source_id: SCI-003
bibliography: "Kossmann, Jan; Kastius, Alexander; Schlosser, Rainer (2022): SWIRL: Selection of Workload-aware Indexes using Reinforcement Learning. EDBT 2022, S. 155–168."
identifier: "https://doi.org/10.48786/EDBT.2022.06"
source_type: peer_reviewed_conference_paper
quality_score: 5
edition_or_version: "EDBT 2022"
locations:
  - "S. 156, Abschnitt 2.1 The Index Selection Problem"
  - "S. 156, Abschnitt 2.2 Problem Formalization"
target_chapters: [intro, foundations]
status: accepted
long_excerpt_approved_by: null
long_excerpt_approval_reason: null
---

# Quellensteckbrief

## Relevanz und Qualitätsbewertung

Peer-reviewte aktuelle Primärquelle, die das Index Selection Problem formal mit Workloadfrequenzen, Kandidatenmenge, Speicher und Kardinalitätsgrenzen beschreibt. Sie benennt außerdem Interaktionen und mögliche Abweichungen zwischen Kostenschätzung und realer Ausführung.

## Eigene Zusammenfassung

SWIRL beschreibt Indexauswahl als Bestimmung einer Kandidatenteilmenge für einen gewichteten Workload unter Grenzen. Große Räume erfordern Suche; Indexinteraktionen erschweren eine unabhängige Bewertung, und geschätzte Kosten können deutlich von realen Ausführungskosten abweichen.

## Geprüfte Fundstellen

Die tatsächlich geprüften Seiten beziehungsweise Dokumentationsabschnitte sind im Frontmatter unter `locations` erfasst. Claimbezogene Auszüge stehen ausschließlich im Claim-Evidence-Ledger.

## Allgemeine Grenzen und Kontext

PostgreSQL und analytische Benchmarks; RL-Verfahren und What-if-Mechanismus werden nicht auf MongoDB übertragen.
