---
source_id: MDB-005
bibliography: "MongoDB, Inc. (2026): Partial Indexes. MongoDB Database Manual v8.2."
identifier: "https://www.mongodb.com/docs/v8.2/core/index-partial/"
source_type: versioned_technical_primary_documentation
quality_score: 5
edition_or_version: "MongoDB 8.2; abgerufen 2026-08-01"
locations: ["Abschnitt Behavior – Query Coverage; Abschnitt Restrictions"]
target_chapters: [foundations, method]
status: accepted
long_excerpt_approved_by: null
long_excerpt_approval_reason: null
---

# Quellensteckbrief

## Relevanz und Qualitätsbewertung

Offizielle Quelle für Eligibility und Grenzen der Partial-Kandidaten I5 und I8.

## Eigene Zusammenfassung

Ein Partial-Index enthält nur Dokumente, die seinen Filter erfüllen. MongoDB nutzt ihn nicht, wenn dadurch ein unvollständiges Resultat entstünde; die Querybedingung muss den Filter einschließen.

## Geprüfte Fundstellen

Die tatsächlich geprüften Seiten beziehungsweise Dokumentationsabschnitte sind im Frontmatter unter `locations` erfasst. Claimbezogene Auszüge stehen ausschließlich im Claim-Evidence-Ledger.

## Allgemeine Grenzen und Kontext

Die Quelle belegt keine konkrete Speicher- oder Laufzeitersparnis im Referenzdatensatz.
