# AI Worker Standard 1.0

**Ein offener Definitionsstandard für AI Worker, AI Teams und AI Organizations.**

Von **Max Luo und Morgan Chen, Mitgründer von Teloa** · 18. Oktober 2026

[English](README.md) · [简体中文](README.zh-CN.md) · [繁體中文](README.zh-Hant.md) · [日本語](README.ja.md) · [한국어](README.ko.md) · [Deutsch](README.de.md) · [Français](README.fr.md) · [Español](README.es.md)

Ein **AI Worker** ist eine KI-Arbeitseinheit mit einer aufgabenübergreifenden Identität, klaren Verantwortlichkeiten, einer Berechtigungsgrenze und überprüfbaren Arbeitsergebnissen. Bei Teloa lautet der Produktbegriff **KI-Mitarbeiter (AI employee)**; AI Worker ist das zugrunde liegende Standardkonzept. Auch Grok Bot lässt sich nach den unten beschriebenen Zuordnungsprüfungen damit beschreiben.

## 1. Grundbegriffe

| Begriff | Beschreibung | Zweck |
| --- | --- | --- |
| AI Worker | Identifizierbare Arbeitseinheit mit Verantwortlichkeiten, Arbeitsverträgen und Ergebnisprüfungen | Arbeit bleibt über Aufgaben, Modelle und temporäre Agents hinweg zuordenbar |
| AI Team | Zusammenschluss für ein gemeinsames Ergebnis mit Rollen, Übergaben und Abnahme | Zusammenarbeit erhält einen überprüfbaren Liefervertrag |
| AI Organization | Zusammensetzung aus Workern und Teams mit organisatorischer Verantwortung, Ressourcengrenzen und Änderungsregeln | Teams arbeiten unter einer ausdrücklich festgelegten Governance zusammen |

Eine dauerhafte Identität erfordert weder einen eigenen Rechner noch einen permanenten Prozess oder Langzeitgedächtnis. Verantwortung bezeichnet hier Zuordnung und Nachverfolgbarkeit von Arbeit, keine Rechtspersönlichkeit. Ein Gruppenchat oder eine Mitgliederliste allein bildet keinen Team-Vertrag.

## 2. Umfang und Format

| Gegenstand | Version 1.0 |
| --- | --- |
| Kernobjekte | `agent` und `organization` |
| Erforderliches Arbeitsprofil | `urn:teloa:profile:ai-work`, Version `1.0` |
| Worker / Team / Organization | Worker: Kern + Arbeitsprofil; Team: zusätzlich Zusammenarbeit; Organization: zusätzlich Governance |
| Austausch | JSON; optional YAML zur Erstellung nach eingeschränkten Konvertierungsregeln |
| Kernprüfungen | Acht gemeinsame Regeln R01–R08; sechs Kernaustauschfälle T01–T06 |
| Maßgebliche Fassung | Vereinfachtes Chinesisch für Kernspezifikation und Arbeitsprofil; vollständige Lesefassungen in sieben weiteren Sprachen |

Der Standard definiert Identitäten, unveränderliche Revisionen, Referenzen, Verantwortlichkeiten, Arbeitsschnittstellen, Zusammenarbeit und Governance. Hosts implementieren Modellausführung, Planung, Verbindungen und Autorisierung. Der Import einer Definition installiert keine Werkzeuge, verbindet keine Konten, startet keine Arbeit und aktiviert keine Routinen. Ein `agent`-Dokument mit nur sechs Kernfeldern ist keine vollständige AI-Worker-Definition.

## 3. Dokumentation

| Dokument | Inhalt |
| --- | --- |
| [Leitfaden mit Diagrammen und durchsuchbarem Text](docs/guide.de.md) | Zusammensetzung, Architektur, Lieferablauf und ein konkretes Beispiel |
| [Vollständige Kernspezifikation](spec/core.de.md) | Felder, feste Revisionen, Referenzen, gemeinsame Regeln und Austauschfälle |
| [Vollständiges Arbeits- und Zusammenarbeitsprofil](spec/work-profile.de.md) | Arbeitsverträge, Ressourcen, Übergaben, Routinen, Instanzbindungen und Governance |
| [Beispiele](examples/README.md) | Vollständiger Worker in JSON/YAML, Kompositionsbeispiel und Ein-/Ausgaben |
| [Produkt- und Spezifikationszuordnungen](docs/mappings.md) | Teloa, Grok Bot, OASF und Oracle Agent Spec |
| [Konformität](CONFORMANCE.md) · [Governance](GOVERNANCE.md) | Nachweisgrenzen, Beiträge und Pflege |
| [Alle acht Sprachen](docs/languages.md) | Vollständige Lesefassungen und Übersetzungsregeln |

Jeder Leitfaden erklärt die Diagramme auch im Text. Übersetzungen behalten Feldnamen, Regelkennungen und JSON-Beispiele bei. Eine vollständige Prüfung durch muttersprachliche Fachleute steht aus. Bei Auslegungsunterschieden gelten die beiden chinesischen normativen Dokumente. Gemeinsame Zuordnungs-, Beispiel- und Governance-Dokumente liegen gemäß Sprachindex auf Englisch und/oder Chinesisch vor.

## 4. Mit einem vollständigen Worker beginnen

1. Lesen Sie den [Leitfaden zu Zusammensetzung und Lieferung](docs/guide.de.md).
2. Prüfen Sie [document-worker.json](examples/document-worker.json) oder das gleichwertige [YAML](examples/document-worker.yaml). Der Worker erhält `material` und liefert `summary` sowie `openQuestions`.
3. Prüfen Sie Verantwortlichkeiten, Ein-/Ausgabeschemas, benötigte Ressourcen und Abnahmevertrag anhand der [Beispieleingabe](examples/document-input.json) und [Beispielausgabe](examples/document-output.json).
4. Zur Ausführung binden Sie die Definition in einem Host an eine tatsächliche Instanz, unterstützte Ressourcen und wirksame Berechtigungen. Ergebnis, Abnahme und geschäftliche Verwendung werden getrennt dokumentiert.

Die Beispiele erläutern Verträge; sie sind weder ein Bereitstellungspaket noch ein Konformitätsprüfer. Das Kompositionsbeispiel enthält nicht aufgelöste Bindungen und ist nicht für die reale Ausführung bereit.

## 5. Produktnamen und externe Spezifikationen

Teloas Produktbegriff für Nutzer bleibt **KI-Mitarbeiter**. Die Worker-Definition beschreibt Rolle und Arbeitsvertrag; eine Worker-Instanz identifiziert einen konkreten KI-Mitarbeiter. Ein KI-Team organisiert Zusammenarbeit und Lieferung über einen Team-Vertrag.

Groks **Bot** lässt sich einem AI Worker zuordnen. Ein von mehreren Menschen genutzter **Team Bot** ist zunächst ein gemeinsam genutzter Worker und nicht automatisch ein Team aus mehreren Workern. Konformität hängt von Definitionen und Hostverhalten ab, nicht vom Namen. Quellen und Prüfungen stehen im [Zuordnungsleitfaden](docs/mappings.md).

OASF liefert Anregungen für Auffindbarkeitsmetadaten und Taxonomien. Oracle Agent Spec liefert Anregungen für Komponentenreferenzen, Schnittstellen und Komposition. Daraus folgt keine vollständige Format- oder Laufzeitkompatibilität.

## 6. Status, Beiträge und Zitierung

Dies ist eine **von Teloa gepflegte offene Spezifikation**, kein verabschiedeter internationaler Standard und kein Zertifizierungsprogramm. Eine Bestätigung durch xAI, AGNTCY, Oracle oder eine Normungsorganisation wird nicht behauptet. Unabhängige Interoperabilität erfordert reproduzierbare Nachweise unabhängiger Implementierungen.

Melden Sie konkrete Unklarheiten, Gegenbeispiele und Implementierungsberichte mit Versionsangaben über [Issues](https://github.com/teloa-ai/ai-worker-standard/issues). Für Änderungen gilt [GOVERNANCE.md](GOVERNANCE.md), für Aussagen zur Konformität [CONFORMANCE.md](CONFORMANCE.md).

Zitiervorschlag: **Max Luo and Morgan Chen. AI Worker Standard 1.0. Teloa, 2026.** Maschinenlesbare Angaben: [CITATION.cff](CITATION.cff).

## 7. Lizenz

Originalinhalte stehen unter [Apache-2.0](LICENSE). Produktnamen und verlinkte Spezifikationen Dritter behalten ihre jeweiligen Rechte. Lesen und Implementieren erfordern weder ein Teloa-Konto noch einen Teloa-Dienst.
