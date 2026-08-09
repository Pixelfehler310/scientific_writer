# Handoff: MongoDB-Indexing-Paper

Stand: 09.08.2026

## Projektstatus

- Das Paper befindet sich in der Phase `source_audit`.
- G4 ist freigegeben. G5 ist noch ausstehend.
- Die Kapitel `intro`, `foundations`, `method`, `evaluation` und `conclusion` sind im globalen State als `approved` geführt.
- Der Referenzlauf verwendet MongoDB~8.2.11, den Benchmark-Commit `7752eed` und die Datenstufen 1.000, 10.000 und 100.000 Dokumente.

## Zuletzt bearbeitete Dokumente

- `seminararbeit/content/chapters/method.tex`: Die Begrenzung der sechs Listenqueries Q1 bis Q3 auf 24 Dokumente ist nun ausdrücklich als Simulation einer paginierten oder per Lazy Loading geladenen Listenansicht beschrieben.
- `seminararbeit/content/chapters/evaluation.tex`: Leseergebnisse, Kostenabwägung und Reichweitenbegrenzung wurden sprachlich überarbeitet. Die bedingte Empfehlung trennt nun klar zwischen den Befunden zu B, L, W1 und W2.

## Wichtige empirische Einordnung

- B enthält neben dem automatisch vorhandenen `_id`-Index nur den gemeinsamen eindeutigen Index auf `productId`. L ergänzt spezialisierte Indizes für Q1, Q2 und Q3. W1 verzichtet auf den Q3-Spezialindex. W2 verwendet kompaktere Q1- und Q2-Indizes.
- Für die auf die erste Ergebnisseite begrenzten Zugriffe weisen L, W1 und W2 in mehreren Szenarien ähnliche Clientlatenzen auf.
- Gegenüber B sind die relativen Unterschiede bei Q1 und Q3 deutlich. Bei 100.000 Dokumenten beträgt der absolute Vorteil indexgestützter Sets jedoch nur etwa 16 bis 19 ms.
- L zeigt bei Q3-selten den klaren strukturellen und zeitlichen Vorteil. W2 benötigt gegenüber L und W1 weniger Indexspeicher und Insert-Zeit, ist aber nicht als schlankstmögliche Lösung belegt.
- Die Messung umfasst einen warmen Einzelclient-Zugriff. Parallele Last, Tail-Latenzen, tiefere Seiten und weitere Kandidatensets wurden nicht untersucht.

## Offene Frage für den nächsten Chat

Prüfen, ob die Schlussfolgerung für Workloads mit häufigen Writes und paginierten Erstseitenzugriffen weiter geschärft werden sollte. Denkbar ist eine bedingte Empfehlung, auf zusätzliche workload-spezifische Indizes zu verzichten, wenn die wesentlichen Reads bereits innerhalb eines akzeptierten Latenzbudgets liegen und Speicher- oder Write-Kosten Vorrang haben.

Diese Empfehlung darf nicht als bereits bestätigt gelten. Der aktuelle Referenzlauf zeigt zugleich, dass B bei den sortierten Q1- und Q3-Szenarien alle 100.000 Dokumente scannt und dabei etwa 16 bis 19 ms höhere Clientlatenzen erreicht. Zu bewerten ist daher, unter welchen Workloadgewichten und Latenzbudgets ein Verzicht auf zusätzliche Indizes vertretbar wäre. Weitere, noch schlankere Kandidatensets als W2 wurden nicht gemessen.

## Nächste Schritte

1. G5 als Quellen- und Integritätsprüfung abschließen.
2. Die bedingte Empfehlung im Fazit gegen die Forschungsfrage prüfen.
3. Den möglichen Verzicht auf zusätzliche Indizes nur als abgegrenzte Empfehlung oder als weiterführenden Prüfauftrag formulieren.
