---
source_id: MDB-011
bibliography: "MongoDB, Inc. (2026): Write Operation Performance. MongoDB Database Manual."
identifier: "https://www.mongodb.com/docs/manual/core/write-performance/"
source_type: technical_primary_documentation
quality_score: 5
edition_or_version: "aktuelle Manual-Seite; abgerufen 2026-08-01"
locations: ["Abschnitt Indexes"]
target_chapters: [intro, foundations, method]
status: accepted
long_excerpt_approved_by: null
long_excerpt_approval_reason: null
---

# Quellensteckbrief

## Relevanz und Qualitätsbewertung

Offizielle Quelle zum allgemeinen Write-Trade-off zusätzlicher Indizes.

## Eigene Zusammenfassung

Inserts und Deletes ändern Schlüssel in allen betroffenen Indizes; Updates verändern die Indizes, deren Schlüssel betroffen sind. Partial-Indizes werden nur für enthaltene Dokumente aktualisiert.

## Geprüfte Fundstellen

Die tatsächlich geprüften Seiten beziehungsweise Dokumentationsabschnitte sind im Frontmatter unter `locations` erfasst. Claimbezogene Auszüge stehen ausschließlich im Claim-Evidence-Ledger.

## Allgemeine Grenzen und Kontext

Keine quantitative Vorhersage für Kandidat oder Set; Write Concern und Ausgangszustand beeinflussen Messwerte.
