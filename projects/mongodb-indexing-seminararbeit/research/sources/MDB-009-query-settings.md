---
source_id: MDB-009
bibliography: "MongoDB, Inc. (2026): setQuerySettings (database command). MongoDB Database Manual v8.0."
identifier: "https://www.mongodb.com/docs/v8.0/reference/command/setquerysettings/"
source_type: versioned_technical_primary_documentation
quality_score: 5
edition_or_version: "MongoDB 8.0; abgerufen 2026-08-01"
locations: ["Abschnitt Definition; Hinweis zu indexHints.allowedIndexes"]
target_chapters: [foundations]
status: accepted
long_excerpt_approved_by: null
long_excerpt_approval_reason: null
---

# Quellensteckbrief

## Relevanz und Qualitätsbewertung

Offizielle Quelle zur Reichweite von Query Settings als möglicher Screeningmechanismus.

## Eigene Zusammenfassung

`allowedIndexes` begrenzt die Plannerkandidaten für eine Query Shape, garantiert jedoch keinen Indexzugriff; ein Collection Scan kann gewinnen.

## Geprüfte Fundstellen

Die tatsächlich geprüften Seiten beziehungsweise Dokumentationsabschnitte sind im Frontmatter unter `locations` erfasst. Claimbezogene Auszüge stehen ausschließlich im Claim-Evidence-Ledger.

## Allgemeine Grenzen und Kontext

Seit MongoDB 8.0; Clusterkonfiguration und Priorität gegenüber Command-Hints sind zu beachten.
