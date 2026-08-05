---
source_id: SCI-001
bibliography: "Chaudhuri, Surajit; Narasayya, Vivek R. (1997): An Efficient, Cost-Driven Index Selection Tool for Microsoft SQL Server. In: VLDB 1997, S. 146–155."
identifier: "https://www.vldb.org/conf/1997/P146.PDF"
source_type: peer_reviewed_conference_paper
quality_score: 5
edition_or_version: "VLDB 1997"
locations:
  - "S. 147, Abschnitt 2.1 Problem Statement"
  - "S. 147–148, Abschnitt 2.2 Architecture of the Index Selection Tool"
target_chapters: [intro, foundations]
status: accepted
long_excerpt_approved_by: null
long_excerpt_approval_reason: null
---

# Quellensteckbrief

## Relevanz und Qualitätsbewertung

Peer-reviewte Primärpublikation und grundlegender Forschungsanschluss für workload- und kostengestützte Indexauswahl. Die konkrete Technik ist relational und SQL-Server-spezifisch; übertragbar sind Problemstruktur, Konfigurationsbegriff und Trennung von Kandidatenauswahl, Enumeration und Kostenauswertung.

## Eigene Zusammenfassung

Die Autoren definieren eine Konfiguration als Indexmenge, bewerten sie workloadbezogen und berücksichtigen Grenzen wie Indexanzahl oder Speicher. Die Architektur trennt Kandidatenauswahl, Konfigurationssuche und Kostenbewertung.

## Geprüfte Fundstellen

Die tatsächlich geprüften Seiten beziehungsweise Dokumentationsabschnitte sind im Frontmatter unter `locations` erfasst. Claimbezogene Auszüge stehen ausschließlich im Claim-Evidence-Ledger.

## Allgemeine Grenzen und Kontext

Microsoft SQL Server 7.0, relationales Modell, optimizerbasierte Kostenschätzung; keine MongoDB- oder Pareto-Mehrzielaussage.
