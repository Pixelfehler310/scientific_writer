# Kapitelplan: evaluation – Pareto-Ergebnisse, Finalvalidierung und Diskussion

## Funktion im Gesamtargument

Das Kapitel beantwortet die empirischen Teilfragen. Es berichtet zuerst Qualität und Ergebnisse des Screenings, danach die vorab regelbasierte Finalistenauswahl und schließlich die physisch gemessene Setleistung, Modellabweichung, Empfehlung und Grenzen.

## Teilfrage und erwartetes Ergebnis

- **Teilfragen:** Beantwortet Teilfrage 3 mit den Pareto-Fronten und ihrer Stabilität; beantwortet Teilfrage 4 mit Finalvalidierung und Vergleich von Schätzung und Messung.
- **Erwartetes Ergebnis:** Nicht dominierte Sets je Skalierung, Sensitivität der 5-/10-/20-%-Bänder, bis zu drei primäre Finalisten, tatsächliche Setkosten und eine bedingte Empfehlung für den Referenzworkload.

## Wortbudget

**1.300 Wörter**

## Voraussetzungen und Übergabe

- **Voraussetzungen:** vollständige validierte Rohdaten, unveränderte Auswahlregeln, dokumentierte Messumgebung und erfolgreiche Ergebnisvalidierung.
- **Übergabe:** Liefert die empirisch begründete Antwort und ihre Reichweite für Kapitel 5.

## Geplante Unterstruktur

| Abschnitt | Inhalt | Absatz-IDs |
| --- | --- | --- |
| 4.1 | Qualität und Plausibilität der Kandidatenprofile | EV-04-P01 bis EV-04-P02 |
| 4.2 | Pareto-Fronten und Skalenwechsel | EV-04-P03 bis EV-04-P04 |
| 4.3 | Finalistenauswahl und Sensitivität der 5-/10-/20-%-Bänder | EV-04-P05 bis EV-04-P06 |
| 4.4 | Tatsächliche Read-, Speicher- und Write-Ergebnisse | EV-04-P07 |
| 4.5 | Schätzung gegen Messung | EV-04-P08 |
| 4.6 | Empfehlung, Übertragbarkeit und Limitationen | EV-04-P09 |

## Absatzplan

| Absatz-ID | Funktion | Kernaussage | Begründung | Evidenzbedarf | Beziehung davor | Beziehung danach | Zielwörter | Medium | Offene Recherche |
| --- | --- | --- | --- | --- | --- | --- | ---: | --- | --- |
| EV-04-P01 | Auswertungsgrundlage prüfen | Nur vollständige Profile mit gültigen Ergebnissen, plausiblen Explain-Strukturen und dokumentierten Ausgangszuständen gehen in die Kostenmatrix ein. | Macht Datenqualität sichtbar, bevor Fronten oder Gewinner berichtet werden. | Eigene Validierungsberichte, Manifeste und Ausschlussprotokolle. | Übergabe aus Methode. | Voraussetzung → EV-04-P02. | 90 | keines | Kriterien und Anzahl ausgeschlossener Läufe erst aus finalen Artefakten einsetzen. |
| EV-04-P02 | Einzelprofile einordnen | Baseline-normalisierte Zeiten und Strukturmetriken zeigen je Queryvariante, welche Kandidaten das Screening tragen und wo Zeit- und Explain-Signale auseinanderfallen. | Prüft die Plausibilität des vereinfachten Kostenmodells, ohne Einzelkandidaten schon als Setempfehlung auszugeben. | Eigene Profilierungsdaten; Theoriebegriffe zu Compound, Partial und Multikey. | Anwendung. | Berechnung → EV-04-P03. | 150 | M-EV-01 | Konkrete Kandidatenunterschiede und Ausreißer bleiben bis zum validierten Lauf offen. |
| EV-04-P03 | Pareto-Fronten berichten | Für 10k, 100k und 500k werden Anzahl und Zusammensetzung der nicht dominierten Sets sowie maßgebliche Trade-offs getrennt dargestellt. | Beantwortet, welche Sets je Skalierung nicht dominiert sind, ohne Skalen zu einer Gesamtfunktion zu vermischen. | Eigene enumerierte Zielvektoren, Fronten und Dominanztests. | Folge. | Vergleich → EV-04-P04. | 150 | M-EV-02 | Darstellungsform nach tatsächlicher Frontgröße wählen; keine unlesbaren Volltabellen im Haupttext. |
| EV-04-P04 | Skalenstabilität analysieren | Übereinstimmungen und Wechsel der Pareto-Mitgliedschaft werden auf veränderte Read-Kosten und Indexgrößen zurückgeführt, soweit die Daten dies stützen. | Beantwortet den Stabilitätsaspekt von Teilfrage 3 und trennt Beobachtung von Erklärung. | Eigene Fronten und Profilwerte; ggf. Theorie zur Skalierungsabhängigkeit. | Kontrast. | Auswahl → EV-04-P05. | 130 | M-EV-02 | Keine kausale Cache- oder Plannererklärung ohne passende Messdaten. |
| EV-04-P05 | Primäre Finalisten auswählen | Read-Anker, Speicher- und Write-Kompromiss werden exakt nach dem 10-%-Band und den Tie-Breakern bestimmt; zusammenfallende Rollen führen zu weniger Sets. | Zeigt, dass Finalisten nicht nach ihren späteren Messwerten ausgewählt wurden. | Eigenes Auswahlmanifest und reproduzierbare Auswahltests. | Folge. | Sensitivität → EV-04-P06. | 150 | M-EV-03 | Rollen, Setmitglieder und Tie-Breaker erst aus dem finalen Manifest einsetzen. |
| EV-04-P06 | Band-Sensitivität bewerten | Die 5-%- und 20-%-Auswertungen zeigen, ob sich Kompromisssets, Indexanzahl, Speicher oder Write-Proxy bei veränderter Read-Toleranz ändern. | Prüft die Abhängigkeit der Auswahl von der gesetzten 10-%-Präferenz, ohne zusätzliche Finalisten nachzunominieren. | Eigene Sensitivitätsausgabe; keine zusätzliche physische Evidenz für nur dort auftretende Sets. | Kontrast und Einschränkung. | Prüfung → EV-04-P07. | 160 | M-EV-03 | Stabilität oder Wechsel nicht vorwegnehmen; alternative Sets ausdrücklich als Screening-Ergebnis kennzeichnen. |
| EV-04-P07 | Tatsächliche Setleistung berichten | Für `B` und die primären Finalisten werden unhinted Read-Kosten, Plannerwahl, gesamte Indexgröße sowie Insert-/Update-Messungen gegenübergestellt. | Liefert die belastbare physische Evidenz für die realen Trade-offs der ausgewählten Konfigurationen. | Eigene Finalistenläufe, Explain-Daten, Laufzeiten, Größen- und Write-Manifeste. | Prüfung. | Vergleich → EV-04-P08. | 160 | M-EV-04 | Nur validierte Wiederholungen und äquivalente Ausgangszustände verwenden. |
| EV-04-P08 | Schätzung gegen Messung prüfen | Abweichungen zwischen Screening und materialisierter Leistung werden je Zielgröße quantifiziert und mit beobachteter Plannerwahl oder Interaktion vorsichtig eingeordnet. | Beantwortet, wie gut Baseline-/Einzelprofile die Setleistung vorhersagen, und markiert Grenzen des Evaluators. | Eigene Schätz- und Messwerte; technische Dokumentation zur Planinterpretation; ggf. Literatur zu Kostenschätzungsgrenzen. | Vergleich. | Synthese → EV-04-P09. | 170 | M-EV-04 | Fehlermaß und Vorzeichenkonvention vor Auswertung festlegen; keine unbeobachtete Ursache behaupten. |
| EV-04-P09 | Empfehlung und Grenzen synthetisieren | Die Empfehlung nennt das unter den gesetzten Präferenzen geeignete Set beziehungsweise den verbleibenden Trade-off und begrenzt ihn auf Workload, Kandidatenraum, Daten, Umgebung und Kostenmodell. | Verbindet alle Teilantworten, diskutiert Übertragbarkeit und verhindert universelle Optimalitätsbehauptungen. | Eigene validierte Ergebnisse; bereits eingeführte Literatur zu Grenzen; dokumentierte Limitationen. | Synthese und Einschränkung. | Übergabe → Kapitel 5. | 140 | keines | Empfehlung erst nach Evidenzsynthese formulieren; Konkurrenzlast, andere Workloads und größere Skalen als Grenzen prüfen. |

**Summe Zielwörter: 90 + 150 + 150 + 130 + 150 + 160 + 160 + 170 + 140 = 1.300.**

## Medienplan

| Medium-ID | Typ | Aussagefunktion | Herkunft/Erzeugung | Beschriftung | Einführung und Interpretation |
| --- | --- | --- | --- | --- | --- |
| M-EV-01 | kompakte Profiltabelle | Zeigt pro Queryvariante die Baseline und die für das Kostenminimum maßgeblichen Einzelprofile einschließlich zentraler Strukturmetriken. | Eigene validierte Kandidatenprofile. | „Grundlage der geschätzten Read-Kosten“ | EV-04-P02 führt Auswahl und Lesart ein; vollständige Profile verbleiben im Anhang/Artefaktarchiv. |
| M-EV-02 | Pareto-Darstellung oder verdichtete Tabelle | Vergleicht Frontmitgliedschaft und Zielwerte über die drei Skalierungen. | Eigene Enumeration und Pareto-Ausgabe. | „Nicht dominierte Indexsets nach Datenskalierung“ | EV-04-P03 erläutert die Fronten; EV-04-P04 interpretiert stabile und wechselnde Sets. |
| M-EV-03 | Tabelle | Stellt Read-Anker, Speicher- und Write-Kompromiss bei 10 % sowie Abweichungen bei 5 % und 20 % gegenüber. | Eigenes Auswahl- und Sensitivitätsmanifest. | „Finalistenauswahl und Sensitivität des Read-Bands“ | EV-04-P05 führt die primären Rollen ein; EV-04-P06 interpretiert Bandwechsel. |
| M-EV-04 | Tabelle oder kombinierte Grafik | Vergleicht für Baseline und Finalisten geschätzte mit tatsächlichen Read-Kosten, Plannerwahl, Speicher und Write-Kosten. | Eigene Finalvalidierung. | „Schätzung und physisch gemessene Setleistung“ | EV-04-P07 führt Messwerte ein; EV-04-P08 interpretiert Abweichungen. |

## Offene Entscheidungen

- Die endgültige Medienform hängt von Frontgröße und Datenlesbarkeit ab; jede Darstellung benötigt eine eigenständige Aussage.
- Ergebnisdarstellung, Interpretation und Limitationen bleiben unterscheidbar, auch wenn sie im selben Kapitel stehen.
- Keine Messzahl, Setzusammensetzung oder Empfehlung wird vor Abschluss und Validierung des neuen Laufs eingesetzt.
