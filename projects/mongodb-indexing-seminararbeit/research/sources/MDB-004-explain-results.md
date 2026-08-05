---
source_id: MDB-004
bibliography: "MongoDB, Inc. (2026): Explain Results. MongoDB Database Manual v8.0."
identifier: "https://www.mongodb.com/docs/v8.0/reference/explain-results/"
source_type: versioned_technical_primary_documentation
quality_score: 5
edition_or_version: "MongoDB 8.0; abgerufen 2026-08-01"
locations: ["Abschnitt executionStats: nReturned, executionTimeMillis, totalKeysExamined, totalDocsExamined"]
target_chapters: [foundations, method, evaluation]
status: accepted
long_excerpt_approved_by: null
long_excerpt_approval_reason: null
---

# Quellensteckbrief

## Relevanz und Qualitätsbewertung

Offizielle Definition der im Experiment erfassten Explain-Felder und ihrer Interpretationsgrenzen.

## Eigene Zusammenfassung

`executionStats` beschreibt den ausgeführten Gewinnerplan, untersuchte Schlüssel und Dokumente sowie zurückgegebene Ergebnisse. Die Explain-Zeit enthält Planwahlanteile und repräsentiert nicht zwingend die reale eingeschwungene Queryzeit.

## Geprüfte Fundstellen

Die tatsächlich geprüften Seiten beziehungsweise Dokumentationsabschnitte sind im Frontmatter unter `locations` erfasst. Claimbezogene Auszüge stehen ausschließlich im Claim-Evidence-Ledger.

## Allgemeine Grenzen und Kontext

MongoDB garantiert kein stabiles Explain-Ausgabeformat; Parser und Versionsbezug müssen getestet werden.
