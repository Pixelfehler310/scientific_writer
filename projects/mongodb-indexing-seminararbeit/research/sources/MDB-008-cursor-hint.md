---
source_id: MDB-008
bibliography: "MongoDB, Inc. (2026): cursor.hint() (mongosh method). MongoDB Database Manual v8.0."
identifier: "https://www.mongodb.com/docs/v8.0/reference/method/cursor.hint/"
source_type: versioned_technical_primary_documentation
quality_score: 5
edition_or_version: "MongoDB 8.0; abgerufen 2026-08-01"
locations: ["Abschnitte Definition und Behavior"]
target_chapters: [foundations, method]
status: accepted
long_excerpt_approved_by: null
long_excerpt_approval_reason: null
---

# Quellensteckbrief

## Relevanz und Qualitätsbewertung

Offizielle Quelle für die experimentelle Isolation eines benannten Einzelindex.

## Eigene Zusammenfassung

`hint()` erzwingt einen vorhandenen Index oder mit `$natural` einen Collection Scan und überschreibt die normale Indexwahl.

## Geprüfte Fundstellen

Die tatsächlich geprüften Seiten beziehungsweise Dokumentationsabschnitte sind im Frontmatter unter `locations` erfasst. Claimbezogene Auszüge stehen ausschließlich im Claim-Evidence-Ledger.

## Allgemeine Grenzen und Kontext

Query Settings können Hint-Felder übersteuern; Hidden oder fehlende Indizes führen zu Fehlern.
