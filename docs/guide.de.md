# AI Worker: Wie ein Mitarbeiter arbeitet und ein Team Ergebnisse liefert

[Languages / 语言](languages.md)

**Max Luo und Morgan Chen, Mitgründer von Teloa** · 18. Oktober 2026

Diese Übersetzung der Version 1.0 folgt dem [maßgeblichen vereinfachten chinesischen Text](guide.zh-CN.md). Sie wurde noch nicht vollständig von muttersprachlichen Branchenfachleuten geprüft; bitte melden Sie Abweichungen. JSON- und YAML-Beispielwerte bleiben zum sprachübergreifenden Vergleich unverändert. Die Abbildungen sind auf Englisch und werden vollständig auf Deutsch erläutert.

In Teloa heißen die Arbeitseinheiten, die Benutzer erstellen und mit denen sie zusammenarbeiten, **KI-Mitarbeiter**. **AI Worker ist der zugrunde liegende allgemeine Standardbegriff:** Dauerhafte Identität, Aufgaben, Arbeitsverträge und Kollaborationsgrenzen beschreiben, wie ein KI-Mitarbeiter arbeitet. Zusammensetzung und Lieferanforderungen von KI-Mitarbeitern und ihren KI-Teams lassen sich mit diesem Standard darstellen.

Ein Name und ein Rollenprompt reichen bereits für einen ersten Versuch. Beim zweiten Auftrag werden die Fragen konkreter: Ist es noch derselbe Mitarbeiter? Welche Materialien wurden diesmal genutzt? Was wurde an das nächste Mitglied übergeben? Wer entscheidet nach der Prüfung eines Berichts, ob er verwendet wird?

Der AI-Worker-Standard macht diese Vereinbarungen ausdrücklich. Benutzer können nachvollziehen, was ein KI-Mitarbeiter übernommen und geliefert hat; verschiedene Produkte erhalten eine gemeinsame technische Ausdrucksform.

Dieser Leitfaden stellt **AI Worker Standard 1.0** mit Kernspezifikation und Arbeits- und Kollaborationsprofil vor. Die Beispiele sind Lehrmaterial, keine Kundenfälle oder Betriebsnachweise. Der [Spezifikationstext](../spec/core.de.md) legt die technischen Anforderungen fest.

Die drei Kernbegriffe lassen sich zunächst so unterscheiden:

| Begriff | Definition | Beantwortete Frage |
| --- | --- | --- |
| **AI Worker** | KI-Arbeitseinheit mit dauerhafter Identität, die innerhalb ausdrücklicher Autorisierungsgrenzen Aufgaben übernimmt, Arbeit annimmt und abnahmefähige Ergebnisse liefert | Wer ist dieser KI-Mitarbeiter, wofür ist er zuständig und wie wird der Abschluss belegt? |
| **AI Team** | Auf gemeinsame Ergebnisse ausgerichteter Verbund mit expliziter Rollenverteilung, Materialübergabe und Abnahmeanforderungen | Wie erledigen mehrere KI-Mitarbeiter gemeinsam eine Arbeit? |
| **AI Organization (AI Org)** | Organisation, die einem Verbund aus Einheiten und Teams organisatorische Verantwortlichkeit, Ressourcen- und Informationsgrenzen sowie Änderungsgovernance hinzufügt | Wer verantwortet mehrere Teams, wie werden Ressourcen verteilt und Änderungen gesteuert? |

„Dauerhafte Identität“ bedeutet, denselben Worker über Aufträge hinweg wiederzuerkennen. Ständige Erreichbarkeit, eine exklusive Maschine oder Langzeitgedächtnis sind nicht erforderlich. „Verantwortung“ meint Arbeitszuordnung und Nachvollziehbarkeit, keine Rechtspersönlichkeit der KI. Text, Tabellen und Schritte erläutern im Folgenden diese Beziehungen; die Abbildungen zeigen dieselben Inhalte im Überblick.

## 01 Was für einen KI-Mitarbeiter geklärt sein muss

Als Beispiel dient ein KI-Mitarbeiter für Materialzusammenfassungen. Er erstellt Zusammenfassungen anhand bereitgestellter Materialien und hält offene Fragen fest, wenn Fakten darin fehlen. Er kann diese Rolle wiederholt ausfüllen und für einzelne Aufträge unterschiedliche Modelle verwenden. Jedes Ergebnis bleibt demselben KI-Mitarbeiter zugeordnet. Technisch entspricht er einer festen Worker-Instanz, deren verwendete Definitionsversion aufgezeichnet wird.

![Zuordnung von Teloa-KI-Mitarbeiter, Grok Bot und AI Worker sowie fünf Gruppen von Bestandteilen](../diagrams/en/worker.png)

*Zuordnung von Teloa-KI-Mitarbeiter, Grok Bot und AI Worker sowie fünf Gruppen von Bestandteilen.*

Die Pfeile zeigen begriffliche Zuordnungen: Teloa behält den Produktbegriff KI-Mitarbeiter, Grok den Begriff Bot. Beide können über die gemeinsame AI-Worker-Definition beschrieben werden; die Abbildung belegt weder eine abgeschlossene Anpassung noch eine Herstellerzertifizierung.

**Die fünf Gruppen von Bestandteilen eines KI-Mitarbeiters** zeigen, was eine Rolle vereinbaren muss. Ressourcen werden nach tatsächlicher Arbeit gewählt; nicht jeder Mitarbeiter benötigt sämtliche Werkzeuge.

| Bestandteil | Allgemeine Bedeutung | Konfiguration des KI-Mitarbeiters für Zusammenfassungen |
| --- | --- | --- |
| **Rolle und Verantwortung** | Dauerhafte Identität, Rollenauftrag, Aufgabengrenzen sowie Regeln für eigene Entscheidungen, Bestätigung und Eskalation | Vorhandenes Material zusammenfassen; Widersprüche auflisten, keine Fakten für den Autor erfinden |
| **Methoden und Grundlagen** | Skills liefern Methoden, Wissen liefert Urteilsgrundlagen, SOPs beschreiben Schritte, Prüfpunkte und Ausnahmebehandlung; Herkunft und Version werden jeweils erhalten | Materialien des aktuellen Auftrags verwenden; bei komplexeren Arbeiten Skills, Wissen oder SOPs ergänzen |
| **Arbeit und Planung** | Arbeitsverträge definieren Ziele, Ein- und Ausgaben, Abhängigkeiten, verantwortliche Position und Abnahme; Routinen legen fest, wann welche Arbeit aufgerufen wird | Arbeitseinstieg `summarize` bereitstellen; bei Auftragseingang ausführen, zunächst keine Routine |
| **Ausführungsbedingungen** | Anwendungs- und Werkzeugbedarf, Informationsumfang, gültige Autorisierung sowie Modell, Budget und Arbeitsraum | Nur autorisiert bereitgestellte Materialien lesen; externe Veröffentlichung gesondert bestätigen |
| **Erfahrung und Ergebnisse** | Gedächtnisregeln legen Speicherung, Nutzung und Rücknahme von Erfahrung fest; Ergebnisse und Nachweise werden versioniert und wie vereinbart abgenommen | Zusammenfassung und offene Fragen liefern; Auftragsverantwortlicher prüft Übereinstimmung mit dem Original |

Ein Gedächtnisspeicher, Anwendungsverbindungen oder Zeitpläne sind hier nicht vorgeschrieben. Ob eine Ressource notwendig ist, hängt von der zugesagten Arbeit ab. Wer externe Informationen abfragen soll, muss Verbindung und Autorisierung als Bedingungen ausweisen. Wer nur Eingabetext zusammenfasst, benötigt keine Werkzeuge allein zur Vervollständigung einer Modulliste.

Eine vollständige Worker-Definition enthält mindestens Identität und Revision, Aufgaben- und Entscheidungsgrenzen sowie eine Arbeit mit Eingaben, Ausgaben, verantwortlicher Position und Abnahmeanforderungen. Für tatsächliche Nutzung muss der Host außerdem konkrete Instanz, Ausführungsbedingungen und vertrauenswürdige Autorisierung binden. Die fünf Gruppen dienen der Lektüre und decken Identität, Verantwortung, Skills, Wissen, SOPs, Arbeitsvorlagen, Routinen, Anwendungen, Berechtigungen, Laufzeitbedingungen, Erfahrung, Ergebnisse und Qualität ab. Die konkreten Felder stehen im [Arbeits- und Kollaborationsprofil](../spec/work-profile.de.md).

## 02 Teamlieferungen müssen jede Übergabe nachvollziehbar machen

Angenommen, eine Gruppe möchte KI mit „Material ordnen und Schlussfolgerungen prüfen“ beauftragen. Ein Analyse-KI-Mitarbeiter liest die Materialien, ein Prüf-KI-Mitarbeiter prüft die Nachweise, und das Team liefert Bericht und Prüfkommentar. Ein Gruppenchat erleichtert den Austausch; der Teamvertrag muss zusätzlich gemeinsames Ergebnis, Rollenverteilung, Übergabebedingungen und Stoppregeln festhalten.

**Ein AI Team besteht aus: gemeinsamem Ziel + Mitgliedern und Rollenverteilung + Grenzen des gemeinsamen Kontexts + kollaborativen Übergaben + Arbeitsplanung + Ressourcenbeschränkungen + Ergebnisabnahme.**

| Teamelement | Zu klärende Vereinbarung |
| --- | --- |
| Gemeinsames Ziel | Welche Arbeit nimmt das Team von außen an und welches Ergebnis liefert es? |
| Mitglieder und Rollenverteilung | Wofür ist jedes Mitglied zuständig; wie beteiligen sich dauerhafte Mitglieder und auftragsbezogene Berater? |
| Gemeinsamer Kontext | Welche Materialien dürfen in welcher Version an welche Mitglieder gehen; kein standardmäßiges Teilen sämtlicher Sitzungen oder privaten Gedächtnisses |
| Kollaborative Übergaben | Wer übergibt welches feste Ergebnis an wen, wie werden Eingaben zugeordnet, wie nimmt der Empfänger an oder lehnt ab? |
| Arbeitsplanung | Wer übernimmt Eingabeverteilung und Ausgabezusammenführung; der Host oder ein ausdrücklich beauftragter Koordinator? |
| Ressourcenbeschränkungen | Autorisierung, Anwendungen, Budget und Stoppbedingungen je Arbeit; Eskalationsziel bei fehlendem Fortschritt |
| Ergebnisabnahme | Welche Ausgaben werden geprüft, wer nimmt welche Version ab, ist unabhängige Prüfung erforderlich? |

Ein Koordinator ist optional; nicht alle Teams brauchen ein Vorgesetztenmodell. Mitgliederliste, Gruppenchat und Organigramm ersetzen diese Arbeitsvereinbarungen ebenfalls nicht.

Auch der Verbund selbst besitzt eine Arbeitsschnittstelle. Beispielsweise empfängt `deliver` des Teams `evidence` und muss `report`, `evidence` und `review` liefern. Koordinator oder Host müssen festlegen, wer die Eingaben erhält, aus welchen festen Ergebnissen die drei Ausgaben stammen und wer deren Vollständigkeit prüft. Ohne diese Zuordnungen ist die Teamarbeit noch nicht bereit.

![Teloa-KI-Mitarbeiter arbeiten nach einem Teamvertrag zusammen; Menschen verantworten Freigabe und Abnahme](../diagrams/en/delivery.png)

*Teloa-KI-Mitarbeiter arbeiten nach einem Teamvertrag zusammen; Menschen verantworten Freigabe und Abnahme.*

Der Beispielablauf in der Abbildung hat sechs Schritte:

1. **Der menschliche Verantwortliche erteilt einen ausdrücklichen Auftrag.** Er nennt Ziel, Eingaben und Autorisierungsumfang, wählt den Team-Arbeitseinstieg und legt Lieferung und Abnahme fest.
2. **Host oder Koordinator prüfen und verteilen.** Sie prüfen Vertrag, Abhängigkeiten und Instanzbindungen, verteilen nur autorisierte Materialien und stellen tatsächliche Auftragszuordnung her.
3. **Der Analyse-KI-Mitarbeiter liefert Ergebnisse.** Er führt die zugewiesene Arbeit aus, fixiert Berichts- und Nachweisversionen und protokolliert tatsächliche Beitragende.
4. **Der Prüf-KI-Mitarbeiter bestätigt den Empfang und prüft.** Er kontrolliert die Eingaben, prüft nach Annahme und fixiert den Prüfkommentar. Bei Ablehnung hält er den Grund fest und gibt die Sache zur Koordination zurück.
5. **Host oder Koordinator prüfen die Vollständigkeit.** Sie führen feste Quellen gemäß Teamvertrag zusammen, prüfen erforderliche Ausgaben und erhalten Beitrags- und Übergabeprotokolle.
6. **Der menschliche Verantwortliche nimmt ab und entscheidet über die Verwendung.** Er nimmt eine bestimmte Ergebnisversion ab und dokumentiert getrennt deren geschäftliche Nutzung. Bestandene Abnahme genehmigt keine automatischen externen Aktionen.

Dies ist eine Beispielaufteilung in Analyse, Prüfung und menschliche Abnahme; konkrete Teams richten sich nach ihren Verträgen. Über alle Schritte bleibt dieselbe Nachweiskette erhalten: **autorisierte Eingaben → fester Bericht → Prüfkommentar → endgültiges Ergebnispaket**.

Eine Übergabe zwischen Mitgliedern kann als folgender Vertragsausschnitt aus dem [vollständigen Kompositionsbeispiel](../examples/composition-fixture.json) formuliert werden:

```json
{
  "id": "review-report",
  "from": "member/analyst/inspect",
  "to": "member/quality/review",
  "when": "已有固定报告与证据且获得本项交接授权",
  "inputs": {
    "report": "report",
    "evidence": "evidence"
  }
}
```

Links in `inputs` stehen Empfängerfelder, rechts Ausgabefelder des Senders. Die chinesische Bedingung verlangt einen festen Bericht mit Nachweisen sowie die Autorisierung dieser Übergabe. Diese Definition sendet keine Nachricht und weist keine Arbeit zu. Die tatsächliche Übergabe muss zusätzlich Sender und Empfänger, Quell-Run, Ergebnisversion, zur Weitergabe autorisierte Materialien und Annahme oder Ablehnung protokollieren. Eine Nachricht in einer Gruppe bedeutet nicht, dass der Empfänger bereits verwendbare Eingaben erhalten hat.

Der Prüf-KI-Mitarbeiter in der Abbildung liefert einen Prüfkommentar; das vollständige Ergebnispaket nimmt weiterhin der benannte Verantwortliche ab. Unabhängige Abnahme muss tatsächliche Beitragende prüfen. Schreibt der Prüfer den Bericht um, enthält die neue Fassung auch seinen Beitrag; er kann dann für diese Version nicht weiterhin als unabhängiger Abnehmer gelten.

## 03 Was eine AI Org über Teams hinaus ergänzt

Eine Organisation kann mehrere Teams und Worker verbinden und teamübergreifende Verantwortung, Ressourcenlimits und Änderungsregeln festlegen. Mehr Personen oder eine zusätzliche Gruppenebene erzeugen solche Vereinbarungen nicht automatisch.

**Eine AI Org besteht aus: dem Verbund von Einheiten und Teams + teamübergreifender Verantwortung + Ressourcen- und Informationsgrenzen + Eskalations- und Änderungsregeln.**

| Organisationselement | Ergänzung bestehender Teams |
| --- | --- |
| Verbund aus Einheiten und Teams | Mitglieder und feste Definitionsversionen aufführen; Unterteams wirken über ihre eigenen Arbeitsschnittstellen mit und legen nicht automatisch alle internen Fähigkeiten offen |
| Teamübergreifende Verantwortung | Vertrauenswürdigen Akteur für den Organisationsbereich sowie Eskalationsziel für teamübergreifende Probleme bestimmen |
| Ressourcen- und Informationsgrenzen | Festlegen, für welche Mitglieder Daten-, Budget- und Ausführungsgrenzen gelten; der Host prüft und setzt sie durch |
| Eskalations- und Änderungsregeln | Besetzung und Abberufung, Definitionsänderungen, Pause und Wiederaufnahme sowie Behandlung laufender Arbeit festlegen |

![Worker, AI Team und AI Org im Verbund am Beispiel von Teloa-KI-Mitarbeitern](../diagrams/en/composition.png)

*Worker, AI Team und AI Org im Verbund am Beispiel von Teloa-KI-Mitarbeitern.*

Beispielsweise kann eine Sicherheitsbetriebsorganisation aus einem Bewertung-/Untersuchungsteam und einem Detection-Engineering-Team bestehen, die einen KI-Mitarbeiter für Qualitätssicherung gemeinsam nutzen. Das erfordert die Bindung derselben tatsächlichen Instanz. Zwei Positionen mit derselben Rollenvorlage reichen nicht für die Annahme, dass es derselbe Mitarbeiter ist.

Die Organisation muss außerdem festhalten, wer ihren Bereich verantwortet, wer Besetzung oder Abberufung entscheidet, wie laufende Arbeiten nach Pausierung eines Mitarbeiters behandelt werden und für welche Mitglieder Materialien und Budgetlimits gelten. Derselbe Mitarbeiter in zwei Teams macht deren Historien nicht gegenseitig lesbar und verdoppelt kein ursprünglich einmaliges Budget.

Dies ist eine ausdrückbare Organisationsstruktur. Ob ein Produkt sie bereits ausführt, muss es durch Bindungs-, Autorisierungs- und Betriebsnachweise belegen. Die Abbildung unterscheidet sich vom herunterladbaren Strukturfixture: Dessen Gruppen A und B verwenden sowohl Analyse- als auch Qualitätsmitarbeiter gemeinsam, um Instanzbindungen zu erläutern; eine tatsächliche Detection-Engineering-Rolle ist dort nicht konfiguriert.

## 04 Definition, Instanz und einzelne Ausführung technisch trennen

Eine Worker-Definition entspricht Rollenbeschreibung und Arbeitsvertrag eines KI-Mitarbeiters; eine Worker-Instanz entspricht einem konkreten KI-Mitarbeiter in Teloa; ein Run ist eine Ausführung dieses Mitarbeiters. Bei einem Rollenvorlagenupdate behält ein alter Run seine ursprüngliche Version. Auch ein Modellwechsel muss die Mitarbeiterhistorie nicht zur Historie einer anderen Person machen.

![Teloa als Beispiel: Worker-Definition, KI-Mitarbeiterinstanz, Run und Ergebnisabnahme](../diagrams/en/architecture.png)

*Teloa als Beispiel: Worker-Definition, KI-Mitarbeiterinstanz, Run und Ergebnisabnahme.*

**Die technische Kette lautet: Standarddefinition → Vertragsprüfung → Hostbindung → ein Run → festes Ergebnis → Abnahmedatensatz → geschäftliche Verwendung.** Jeder Schritt beantwortet andere Fragen:

| Ebene | Objekt oder Schritt | Aufgezeichnete und geprüfte Inhalte |
| --- | --- | --- |
| Standarddefinition | Kerndefinition und Arbeitserweiterung | `id`, `revision`, `kind` und feste Mitgliedsreferenzen; Aufgaben, Arbeits-I/O, Abhängigkeiten, Zusammenarbeit und Abnahme |
| Hostkonfiguration | Vertrags- und Fähigkeitsprüfung | Existenz von Referenzen, vollständige Ein-/Ausgabeverträge, Unterstützung erforderlicher Fähigkeiten; bei Lücken betroffene Arbeit als nicht bereit markieren |
| Hostkonfiguration | Worker-Instanz und Bindungen | Dauerhafte Identität des konkreten KI-Mitarbeiters sowie Materialien, Modell, Anwendungsverbindungen und gültige Autorisierung dieser Arbeit |
| Tatsächliche Ausführung | Work, Auftrag und Run | Work ist ein wiederholt aufrufbarer Arbeitsvertrag; ein Auftrag eine konkrete Beauftragung; Run protokolliert Eingaben, Definitionsversion und tatsächlichen Ausführenden einer Ausführung |
| Arbeitslieferung | Festes Ergebnis | Einsehbarer Inhalt, Version, Nachweise und tatsächliche Beitragende, nicht nur „Lauf beendet“ |
| Arbeitslieferung | Abnahmedatensatz | Wer welche Ergebnisversion anhand welcher Anforderungen geprüft hat und mit welchem Ergebnis; bei geforderter Unabhängigkeit zusätzlich tatsächliche Beitragende prüfen |
| Geschäftliche Entscheidung | Verwendungsdatensatz | Ob der Verantwortliche das Ergebnis verwenden will; Verwendung und Abnahme werden getrennt aufgezeichnet und erteilen keine automatische Veröffentlichungs- oder Reaktionsbefugnis |

Beispiel: „Zusammenfassungsrolle, Revision 2“ ist eine Definition; der damit erstellte „Materialorganisator“ ist eine Instanz. Die Zusammenfassungen dieser und nächster Woche sind zwei Aufträge mit jeweils eigenem Run und Ergebnis. Ein Vorlagenupdate schreibt alte Runs nicht um und überträgt die Urheberschaft alter Ergebnisse nicht auf einen neuen Mitarbeiter.

Das Kernformat behält nur `agent` und `organization`. Worker verwenden `agent` plus erforderliche Arbeitserweiterung; Teams `organization` plus Kollaborationsvertrag; Orgs ergänzen Governance. So lassen sich alle nach denselben Referenz- und Versionsregeln zusammensetzen, ohne je Produktbezeichnung einen neuen Dateityp zu schaffen.

Der Host ist die Plattform, die diese Definitionen tatsächlich ausführt, etwa Teloa. Er bindet sie an reale Instanzen, Materialien, Werkzeugverbindungen, Modelle und autorisierende Akteure, prüft Eingaben und Abhängigkeiten und zeichnet Läufe und Ergebnisse auf. Ein Dateiimport liefert nur eine zu interpretierende Definition. Er verbindet keine Konten, erteilt keine Rechte, startet keine Ausführung und aktiviert keine Routinen. Strukturprüfung, Konfigurationsbereitschaft, Ausführungserfolg, Ergebnisabnahme und geschäftliche Verwendung brauchen jeweils eigene Nachweise und können einander nicht ersetzen.

## 05 Ein Beispiel zum Prüfen jedes Feldes

Der [Worker für Materialzusammenfassungen](../examples/document-worker.json) ist eine vollständige JSON-Definition mit allen erforderlichen Feldern. Sein `summarize` nimmt eine nicht leere Zeichenfolge `material` entgegen und liefert eine nicht leere `summary` sowie das Zeichenfolgenarray `openQuestions`. Die folgenden Ein- und Ausgaben sind von Hand verfasste Lehrbeispiele:

```json
{
  "material": "示例周报：甲项目本周完成三份资料摘要。第二份缺少来源日期，负责人要求补充。材料没有提供工时或客户反馈。"
}
```

```json
{
  "summary": "甲项目本周完成三份资料摘要，其中第二份仍需补充来源日期。",
  "openQuestions": [
    "第二份资料的来源日期是什么？",
    "现有材料未提供工时或客户反馈，无法据此评价效率或满意度。"
  ]
}
```

Die unveränderten chinesischen Beispielwerte besagen: Projekt A hat diese Woche drei Materialzusammenfassungen fertiggestellt. Bei der zweiten fehlt das Quelldatum, das der Verantwortliche nachgefordert hat. Arbeitszeit- oder Kundenfeedbackdaten liegen nicht vor. Die Ausgabe fasst den Abschluss der drei Zusammenfassungen und das fehlende Quelldatum zusammen; die offenen Fragen betreffen dieses Datum und die fehlende Grundlage, um Effizienz oder Zufriedenheit zu beurteilen.

Strukturprüfungen können fehlende Felder und falsche Typen erkennen. Der Verantwortliche muss außerdem den Inhalt prüfen: Bleibt er den Materialien treu, oder behauptet er bei fehlenden Arbeitszeitdaten eine „Effizienzsteigerung“? Abnahme bezieht sich auf eine feste Ausgabeversion; nach Änderungen ist eine erneute Prüfung gemäß Vereinbarung nötig.

Dieselbe Beziehung lässt sich für SOC-Bewertungen nutzen. Enthält das Lehrmaterial lediglich einen WAF-Datensatz zu HTTP 403, darf der Bericht nur erläutern, welche Schlussfolgerungen dieser Datensatz trägt, und fehlende Backend- und Endpunktnachweise benennen. Er darf daraus weder eine Serverkompromittierung noch vollständige Sicherheit behaupten. Für tiefere Untersuchungen benötigte Daten werden als konkrete Lücken genannt; Maßnahmen wie Sperren oder Isolieren benötigen entsprechende Werkzeuge und Autorisierung. Dieses Beispiel verbindet keine reale Umgebung und führt keinen Angriff aus.

## 06 Wie Grok Bot und Teloa zugeordnet werden

Teloas KI-Mitarbeiter ist die konkrete Produktform eines AI Workers; auch Grok Bot kann als AI Worker beschrieben werden. KI-Mitarbeiter und Bot sind jeweilige Produktbegriffe; AI Worker liefert die gemeinsame technische Definition. Bei Anpassungen werden Aufgaben, Arbeitsverträge und Hostverhalten einzeln geprüft. Groks öffentliche Designbeschreibung erläutert dauerhaft bestehende Bots mit Bestandteilen wie Skills und Routines. Dies ermöglicht einen begrifflichen Vergleich, belegt aber keinen vorhandenen Konverter oder eine offizielle Kompatibilitätszertifizierung. [Grok-Designbeschreibung](https://x.ai/news/designing-grok-bot)

| Produktbegriff | Entsprechung in dieser Spezifikation | Bei der Anpassung zu prüfen |
| --- | --- | --- |
| Teloa-KI-Mitarbeiter; Grok Bot | Worker-Definition und tatsächliche Instanz | Vollständigkeit von dauerhafter Identität, Aufgaben, Arbeits-I/O und Abnahme |
| Rollenprompt, Bot profile | `instructions` und `responsibility` | Konkrete Arbeitsgrenzen; fehlende Angaben dürfen nicht aus dem Namen abgeleitet werden |
| Skills, Wissen, SOPs | Nach Verwendungszweck referenzierte `resources` | Tatsächliche Version, Herkunft und Arbeitsabhängigkeiten |
| Werkzeug oder Plugin | Fähigkeitsreferenz, Anwendungsressource und Hostverbindung | Tatsächliche Autorisierung; Zugangsdaten verbleiben im Host |
| Teloa-Gruppen und -Projekte | Träger von Teamkommunikation und Kontext | Zusätzliche explizite Rollenverteilung, Übergaben und Teamergebnisverträge |
| Grok Team Bot | Gemeinsam genutzter Worker | Autorisierung, private Sitzungen und private Routinen der Benutzer bleiben getrennt |

Die Dokumentation von Grok Team Bot beschreibt mehrere Personen, die denselben Bot nutzen. Dies unterscheidet sich von einem AI Team, in dem mehrere Worker arbeitsteilig ein Ergebnis liefern. Bei der Anpassung eines geteilten Workers bleiben persönliche Routinen private Hostpläne und gelangen nicht in die geteilte Definition. Nur vom Autor ausdrücklich veröffentlichte Vorlagen ohne private Inhalte dürfen geteilt werden; auch nach Import werden sie nicht automatisch aktiviert. [Team Bots](https://docs.x.ai/grok-bot/team-bots)

Genauere Teloa-Feldzuordnungen und Anpassungslücken stehen im [Mapping-Dokument](mappings.md).

## 07 Vorhandene Spezifikationen wiederverwenden und den Umfang klar halten

OASF liefert Agent-Auffindbarkeitsdatensätze sowie Domänen- und Fähigkeitsklassifikationen, die bei der Suche nach geeigneten Fähigkeiten helfen. Diese Spezifikation erhält die nativen Klassifikationen samt Versionen und beschreibt Eingaben, Ausgaben und Abnahme separat durch Arbeitsverträge. OASF-Klassifikations-Skills und ausführbare Skill-Pakete sind unterschiedliche Objekte. [OASF Record Guide](https://docs.agntcy.org/oasf/agent-record-guide/)

Komponentenreferenzen, Ein-/Ausgaben und Kompositionsdefinitionen von Oracle Agent Spec sind wiederverwendbare Ideen. Diese Spezifikation greift sie auf; bei Anpassung konkreter Versionen müssen Schnittstellen, Referenzen und Ausführungssemantik dennoch einzeln konvertiert werden. Feldumbenennungen allein sind keine Interoperabilität. [Oracle Agent Spec 26.1.2](https://oracle.github.io/agent-spec/26.1.2/agentspec/language_spec_26_1_2.html)

JSON dient zum Austausch fester Definitionen, YAML kann für deren Erstellung verwendet werden. Die YAML-Konvertierung muss unzulässige Eingaben wie doppelte Schlüssel, Anker, Aliase, explizite Tags, nicht endliche Zahlen und mehrere Dokumente ablehnen und Zahlen- sowie Zeichenfolgenbedeutung erhalten. Beispielsweise ist `revision: "1"` eine Zeichenfolge; `revision: 1` wird wegen falschen Typs abgelehnt. Beide Dateien dürfen nicht als unabhängig veränderbare Quellen derselben Revision dienen.

## 08 Mit der Anwendung beginnen

Wählen Sie eine vorhandene Arbeit und klären Sie zuerst Eingaben, Ausgaben, Abhängigkeiten, verantwortliche Position und Abnahme. Führen Sie sie mit einer tatsächlichen Instanz aus und prüfen Sie, welche Materialien der Empfänger wirklich erhalten hat und welche festen Ergebnisse vorliegen. Erweitern Sie danach auf Teams. Kann die aktuelle Plattform eine wesentliche Beschränkung nicht erfüllen, wird die betreffende Arbeit als nicht bereit markiert.

Diese Version will zunächst diese Beziehungen eindeutig definieren. Für eine branchenweit gemeinsam nutzbare Spezifikation sind zusätzlich Interoperabilitätsnachweise unabhängiger Implementierungen, Nicht-SOC-Szenarien und öffentliche Änderungsdiskussionen nötig. Das Repository verwendet Apache-2.0. Beiträge können mit einer konkreten Mehrdeutigkeit oder einem Gegenbeispiel beginnen. [Standard-Repository](https://github.com/teloa-ai/ai-worker-standard) · [Feedback](https://github.com/teloa-ai/ai-worker-standard/issues)
