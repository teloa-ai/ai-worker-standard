# Arbeits- und Kollaborationsprofil für AI Worker

[Languages / 语言](../docs/languages.md)

**Max Luo und Morgan Chen, Mitgründer von Teloa**

Datum: 2026-10-18. Version: `1.0`. Normativer Anhang zum [Offenen Definitionsstandard für AI Worker, AI Team und AI Organization 1.0](core.de.md). Er legt Arbeitssemantik und Anpassungsanforderungen fest, keine neue Ausführungsengine.

Diese Übersetzung folgt dem [maßgeblichen vereinfachten chinesischen Text](work-profile.zh-CN.md). Sie wurde noch nicht vollständig von muttersprachlichen Branchenfachleuten geprüft. Bitte melden Sie Abweichungen vom maßgeblichen Text. Die Werte der JSON- und YAML-Beispiele bleiben für den sprachübergreifenden Vergleich unverändert.

## 1. Worker, Team und Organization durch einen Vertrag verbinden

Ein Mitarbeiter bietet Arbeitseinstiege an. Ein Team bietet eigene Arbeitseinstiege an und beschreibt die Aufteilung unter Mitgliedern. Eine Organisation setzt Teams weiter zusammen. Alle drei legen Eingaben, Ausgaben, Abhängigkeiten, Verantwortung und Abnahme fest. Eine Organisation macht nicht automatisch sämtliche Arbeiten ihrer Mitglieder zugänglich; ein Beitritt teilt auch nicht automatisch persönliches Gedächtnis.

Das Arbeitsprofil erscheint als erforderliche Erweiterung: `id = urn:teloa:profile:ai-work`, `version = "1.0"`, `required = true`. Dies ist die feste Kennung dieser Spezifikation, keine Authentifizierung und kein Zugang zu einem Onlinedienst. Jeder Host kann sie implementieren; ein Teloa-Konto ist nicht erforderlich. Benutzer dürfen sie nicht auf optional setzen, um wesentliche Semantik zu umgehen. Eine Datei mit ausschließlich agent-Kernfeldern kann nur Unterstützung des Kernaustauschs erklären, keine Konformität einer vollständigen AI-Worker-Definition.

Die Kernregeln für nicht leere Zeichenfolgen, LocalId, feste Referenzen, nebenwirkungsfreien Import und unbekannte Erweiterungen gelten weiterhin. Die folgenden Strukturen erlauben nur die aufgeführten Felder; alle sind erforderlich, sofern sie nicht ausdrücklich als optional markiert sind. Wo kein Inhalt zutrifft, werden ausdrücklich leere Arrays verwendet, nicht null. Kennungen sind jeweils nur innerhalb ihres Arrays eindeutig; Referenzpositionen werden nach diesem Anhang interpretiert.

## 2. Struktur der Erweiterungs-data

| Feld | Typ | Zweck und Standard |
| --- | --- | --- |
| responsibility | Responsibility | Entscheidungsregeln einer Rolle oder eines Verbunds |
| resources | Resource[] | Verzeichnis von Methoden, Materialien, Anwendungen und Richtlinien; darf leer sein, Auflistung verlangt keine allgemeine Aktivierung |
| works | Nicht leeres Work[] | Öffentlich zugesagte Arbeitseinstiege |
| requires | LocalId[], optional | Von allen Arbeiten gemeinsam benötigte resources; fehlend bedeutet leer |
| routines | Routine[], optional | Wiederkehrende Arbeitspläne; fehlend bedeutet leer, keine automatische Aktivierung |
| authorityRoles | AuthorityRole[], optional | Externe Freigabe-, Abnahme- oder Verantwortungsrollen; fehlend bedeutet leer |
| memoryPolicyRef | LocalId, optional | Verweist auf eine Ressource mit purpose=policy; zählt bei Vorhandensein zu den allgemeinen Abhängigkeiten; ohne das Feld wird kein auftragsübergreifendes Gedächtnis deklariert |
| collaboration | Collaboration | Für organization erforderlich, für agent unzulässig |
| governance | Governance, optional | Nur für organization; bei Erklärung einer AI-org-Definition dieser Version erforderlich |

**Responsibility** enthält fünf erforderliche Zeichenfolgenarrays: `triggers`, `autonomousActions`, `confirmationPoints`, `escalationRules`, `deliveryChecks`. Sie beschreiben Übernahmeauslöser, autonome Aktionen, Bestätigungspunkte, Eskalationsregeln und Lieferprüfungen. triggers, autonomousActions und deliveryChecks dürfen nicht leer sein. Die beiden anderen dürfen leer sein, wenn tatsächlich keine zusätzlichen fachlichen Bestätigungs- oder Eskalationsregeln bestehen. Ein leeres Bestätigungsarray hebt die Hostautorisierung nicht auf; ein leeres Eskalationsarray erlaubt keine Ausführung bei fehlenden Eingaben oder Berechtigungen. Beschreibungen erteilen keine Werkzeugrechte. Kern-instructions beschreiben die dauerhaften Aufgaben; Work und Ressourcen liefern Arbeitsdetails. Ob der Text substanzielle Bedeutung hat, braucht weiterhin eine Inhaltsprüfung und lässt sich nicht allein an nicht leeren Zeichenfolgen erkennen.

**Resource** enthält `id:LocalId`, `title:nicht leere Zeichenfolge`, `purpose`, `binding` sowie optional `requires:LocalId[]`. purpose ist `method | knowledge | procedure | application | runtime | policy | trigger | template-reference`. Dies sind Verwendungskennzeichen für Ressourcen, keine zwingend einzuführenden Produktressourcentypen. binding enthält `kind` und `config` und verwendet die vier Bindungstypen und config-Regeln von Capability im Kern, ohne Protokolle neu zu definieren.

Resource.requires referenziert ausschließlich resources derselben Definition; fehlend bedeutet leer. Feste Materialien sollten möglichst einen digest tragen; veränderliche Quellen müssen bei tatsächlicher Ausführung auf die gelesene Version festgelegt werden. Methoden können vollständige Skill-Pakete referenzieren, SOPs Methoden- oder Prozessressourcen. Eine nur lesbare Anwendungsbeschreibung, Modellkonfiguration oder Richtliniendatei darf nicht als bereits verbundene Anwendung, erfüllte Laufzeitumgebung oder durchgesetzte Richtlinie ausgewiesen werden.

Für runtime, policy, trigger oder als Ressource beschriebene application muss der Anpassungsbericht natives Format, exakte Version und eindeutig verantwortliche Verarbeitungsstelle festhalten. Diese kann eine Hostfunktion mit Formatunterstützung oder ein ausdrücklich beauftragter menschlicher Schritt sein. Nur Text ohne Verantwortlichen für die Umsetzung erfüllt keine Voraussetzung. Diese Vereinbarung erhält bestehende native Formate und schafft keine neue allgemeine Berechtigungs- oder Planungssprache.

**AuthorityRole** enthält `id:LocalId`, `name:nicht leere Zeichenfolge`, `kind:human | service`. Sie ist eine Verantwortungsposition, kein Konto. Der Host muss sie an einen vertrauenswürdigen Akteur binden. service bezeichnet einen vom Host bestätigten externen Dienstakteur; ein Modell darf nicht sich selbst zum Genehmiger machen. Wenn eine menschliche Freigabe nötig ist, muss human deklariert werden. Die Funktion der Rolle ergibt sich aus dem Abnahme-, Eskalations- oder Governance-Vertrag, der sie referenziert.

## 3. Ein- und Ausgaben sowie Lieferung jeder Arbeit

| Work-Feld | Typ | Bedeutung |
| --- | --- | --- |
| id, title, instructions | LocalId, nicht leere Zeichenfolge, nicht leere Zeichenfolge | Arbeitseinstieg, Name und Ziel-/Methodenanforderungen |
| inputs, outputs | JSON-Schema-Objekte | Beschreiben jeweils das vollständige Eingabe- und Ausgabeobjekt |
| requires | LocalId[] | Für diese Arbeit benötigte resources; darf leer sein |
| responsible | ParticipantRef | Eine primär verantwortliche Position |
| acceptance | Acceptance | Abschlussprüfung und Abnahmeverantwortung |

`ParticipantRef` ist `self`, `member/<LocalId>` oder `authority/<LocalId>`. self bezeichnet die Instanz der aktuellen Definition. member bezeichnet ausschließlich einen direkten Member-Platz der aktuellen organization, einschließlich auftragsbezogen teilnehmender consultants. authority bezeichnet die aktuellen data.authorityRoles. agent darf keine member-Referenzen verwenden. Alle Referenzen müssen vorhanden sein; Zuordnung durch Namensvermutung ist unzulässig. responsible muss self oder member sein. Externe Personen können Abnahme und Eskalation verantworten, dürfen aber nicht als KI-Ausführende ausgegeben werden. Bei organization bedeutet self, dass der Verbund für die externe Lieferung verantwortlich ist. Der Host muss weiterhin tatsächliche Ausführende und die Verantwortlichen für Eingabeverteilung und Ausgabezusammenführung protokollieren; der Organisationsname darf tatsächliche Beitragende nicht ersetzen.

**Acceptance** enthält `criteria:nicht leeres Zeichenfolgenarray`, `reviewer:ParticipantRef`, `independent:boolean`. criteria sind überprüfbare Abschlussanforderungen; reviewer ist die Abnahmeposition. Bei independent=true muss der Host belegen, dass der tatsächliche Prüfer ein anderer Akteur als der tatsächliche Ausführende des geprüften Ergebnisses ist. Bei zusammengesetzten Ergebnissen müssen tatsächliche Beitragende geprüft werden; nur Hauptverantwortliche oder nominelle Teamidentitäten zu vergleichen genügt nicht. Zwei Plätze, zwei Sitzungen oder zwei Namen belegen keine Unabhängigkeit. Unabhängige Akteure belegen auch nicht automatisch Urteilsqualität. Der Abnahmedatensatz fixiert die verwendete Ergebnisversion; Strukturvalidierung, Modellabschluss und geschäftliche Verwendung werden getrennt protokolliert.

Die Wurzeln von inputs und outputs müssen `type:object` sein und `$schema = https://json-schema.org/draft/2020-12/schema` deklarieren. Das Standardvokabular von [JSON Schema 2020-12](https://json-schema.org/draft/2020-12) wird wiederverwendet; es entsteht keine eigene Typsprache. Schemas dürfen Erläuterungen und `$defs` enthalten. Alle Referenzen einschließlich `$ref` und `$dynamicRef` dürfen nur auf das jeweilige Schema-Dokument verweisen; verschachtelte `$id` dürfen die Dokumentgrenze von Referenzen nicht verändern. Beim Import werden Referenzen nicht über das Netzwerk aufgelöst. Implementierungen müssen nicht unterstützte Vokabulare oder Beschränkungen melden und dürfen diese nicht ignorieren und anschließend erfolgreiche Validierung behaupten. Wird format für wesentliche Abnahmebedingungen genutzt, muss die entsprechende Formatprüffähigkeit ausdrücklich deklariert und geprüft werden.

Fehlende Eingaben werden als nachzureichen gemeldet. Bei fehlenden Ausgaben oder falschen Typen darf keine Konformität mit dem Ergebnisvertrag erklärt werden. Zeichenfolgen werden nicht implizit in Zahlen konvertiert, Nachkommastellen nicht abgeschnitten und false nicht in 0 umgewandelt. Schema-default ist beschreibend und bedeutet nicht automatisch, dass der Benutzer diesen Wert geliefert oder autorisiert hat. Freitextergebnisse können in Zeichenfolgenfeldern stehen; fachliche Nachweise und Urteilsqualität werden weiterhin durch acceptance geprüft.

**Abhängigkeitsprüfung:** Zuerst müssen die Kern-capabilities und ihre R05/R06-Bedingungen erfüllt sein. Danach wird die Vereinigung aus data.requires, memoryPolicyRef und dem gewählten Work.requires gebildet und Resource.requires rekursiv aufgelöst. Fehlende Referenzen oder Ressourcenabhängigkeitszyklen sind Definitionsfehler. Fehlende Verbindungen, Berechtigungen oder Interpreter machen die jeweilige Arbeit nicht bereit; unkonfigurierte Spezialressourcen anderer Arbeiten blockieren die aktuelle Arbeit nicht. Fehlende allgemeine erforderliche Abhängigkeiten blockieren weiterhin die gesamte Ausführung. Interpretationsunterstützung, Konfigurationsbereitschaft und tatsächliche Autorisierung werden getrennt gemeldet; teilweise Nutzbarkeit darf nicht als vollständige Ausführbarkeit dargestellt werden.

## 4. Routinen und Erfahrung

**Routine** enthält `id:LocalId`, `work:LocalId`, `policyRef:LocalId` sowie optional `triggerRef:LocalId`. work referenziert die aktuellen works, policyRef eine Ressource mit purpose=policy und triggerRef eine Ressource mit purpose=trigger. Die ausführende Rolle gehört zur an die aktuelle Definition gebundenen Instanz. Ersteller, Scheduling-Verantwortlicher und tatsächlich autorisierender Akteur werden vom Host getrennt gebunden und müssen nicht mit der Worker-Identität übereinstimmen. Beim Import dürfen weder zusätzliche Mitarbeiter erzeugt noch Sichtbarkeit von Routinen erweitert oder Routinen an andere Mitglieder übertragen werden.

Fehlendes triggerRef bedeutet, dass die Planung noch aussteht. Auch ein vorhandenes triggerRef beschreibt nur einen Auslöser; die Instanz ist standardmäßig nicht aktiviert. Das Auslöserformat verarbeitet der Host. Teloas bestehende daily/weekly- oder lokale Ereignisse erklären nur tatsächliche Unterstützung; beliebiges cron oder externe Ereignisse dürfen nicht stillschweigend angenähert werden. Zuerst werden tatsächliche Arbeit und tatsächlicher Auslöser geprüft, danach aktiviert der Bereitsteller die Routine.

Eine Routinen-policy muss Eingabefenster/Cursor, Verhalten bei fehlenden und ungültig gewordenen Eingaben, Auslöser-Deduplizierung und Buchführung der fachlichen Verarbeitung, Wiederholungen, Parallelität/Nachläufe, Budget, Benachrichtigung und Pausenbedingungen eindeutig regeln. Unvollständige Pläne dürfen erhalten, aber nicht aktiviert werden. Erfolgreiches Auslösen bedeutet keine erfolgreiche Verarbeitung. Nur wenn der vereinbarte Ergebniszustand erreicht ist, darf der fachliche Verarbeitungsstatus fortgeschrieben werden; Wiederholungen dürfen die fachliche Deduplizierung nicht durch neue Auslöserkennungen umgehen. Es gibt weder eine einheitliche feste Wiederholungszahl noch eine Branchenregel „zweimal ohne Fortschritt“.

memoryPolicyRef definiert nur Regeln für Speicherung, Herkunft, Umfang, Prüfung und Rücknahme von Erfahrungen und enthält kein privates Gedächtnis des Autors. template-reference-Ressourcen sind Vorlagenmaterial mit Herkunftsnachweis; historische Protokolle dürfen nicht als eigene Erlebnisse einer neuen Instanz ausgegeben werden. Persönlicher, geschäftlicher, projektbezogener und Gruppenkontext eines gemeinsam genutzten Mitarbeiters bleiben gemäß Hostautorisierung getrennt. Dieselbe Mitarbeiterinstanz in mehreren Teams macht deren Historien nicht gegenseitig lesbar.

## 5. Teamkollaboration und organisatorische Governance

| Collaboration-Feld | Typ | Bedeutung |
| --- | --- | --- |
| roles | RoleAssignment[] | Deckt jeden direkten Member genau einmal ab |
| coordinator | ParticipantRef, optional | Darf, falls vorhanden, nur einen direkten Member-Platz des aktuellen Verbunds referenzieren; ohne Angabe wird niemand automatisch ausgewählt |
| handoffs | Handoff[] | Ausdrückliche Übergaben von Verantwortung und Materialien; darf leer sein |
| sharing | Sharing[] | Vorgesehene Empfängerpositionen gemeinsam genutzter Ressourcen; darf leer sein |
| stop | StopPolicy | Regeln für begrenzte Zusammenarbeit, fehlenden Fortschritt und Eskalation |

**RoleAssignment** enthält `member:LocalId`, `duty:nicht leere Zeichenfolge`, `participation:member | consultant`. duty beschreibt die Aufgabenverteilung innerhalb dieses Verbunds, ohne ursprüngliche Mitgliedsaufgaben umzuschreiben. Bei Konflikten wird die betroffene Arbeit gestoppt. consultant bezeichnet einen auftragsbezogen teilnehmenden Berater, der weder automatisch Gruppen beitritt noch deren Historie erhält. Feste Definitionsreferenzen beider Platztypen werden nach den Kernregeln aufgelöst; Teilnahmeumfang wird durch Instanzbindung und Auftragsautorisierung umgesetzt.

coordinator und Work.responsible dürfen auf einen consultant-Platz zeigen, aber nur mit ausdrücklicher Zuweisung und Teilnahmebestätigung für die aktuelle Arbeit. Die Teilnahme bleibt auftragsbezogen; daraus entstehen weder dauerhafte Teamleitung noch Zugriff auf die vollständige Gruppenhistorie. `member/` ist die Syntax zur Referenzierung eines Member-Platzes und nicht derselbe Begriff wie der Wert member in participation.

**Handoff** enthält `id:LocalId`, `from:WorkRef`, `to:WorkRef`, `when:nicht leere Zeichenfolge`, `inputs:Objekt`. WorkRef erlaubt nur `member/<memberId>/<workId>` und referenziert eine deklarierte Arbeit eines direkten Mitglieds des aktuellen Verbunds. Die Definitionsversion des Mitglieds stammt aus Member.ref. Es bezeichnet die Übergabe von einer bereits ergebnisliefernden Mitgliedsarbeit an die empfangende Mitgliedsarbeit. `self/<workId>` wird nicht unterstützt; Verbundein- und -ausgänge werden nicht in dieselbe Referenzsemantik gemischt. Unterteams stellen ihre eigenen works bereit; ihre internen Mitglieder werden nicht unmittelbar durchgriffen.

Die Schlüssel von Handoff.inputs sind Feldnamen auf oberster Ebene des Eingabeobjekts der empfangenden Arbeit; die Werte sind Feldnamen auf oberster Ebene des Ausgabeobjekts der sendenden Arbeit. Alle werden als ursprüngliche Zeichenfolgen interpretiert, ohne Ausdrücke, Skripte oder Punktpfade auszuführen. Die Felder müssen in den jeweiligen Schema.properties deklariert sein; kopierte Werte müssen außerdem die tatsächliche Schema-Prüfung des Empfängers bestehen. Weitere erforderliche Eingaben der empfangenden Arbeit müssen ausdrücklich ergänzt werden; fehlen sie, wird die Lücke gemeldet und kein erfolgreicher Empfang vorgetäuscht. when ist eine fachliche Bedingung. Eine Übergabe erfolgt nur bei ausdrücklicher Zuweisung oder Bestätigung durch einen unterstützten Auslösemechanismus; Text allein belegt keine implementierte Überwachung.

Eingabeverteilung und Ausgabezusammenführung für Verbund-Work werden durch native Hostorchestrierung oder einen ausdrücklich beauftragten Koordinator umgesetzt. Diese Version führt keine zusätzliche Ein-/Ausgangsabbildungssprache ein. Der Anpassungsbericht muss aufführen, welche Verbundeingabe in welche Mitgliedsarbeit geht, wer jede erforderliche Ausgabe aus welchem festen Ergebnis liefert und wer die Vollständigkeit prüft. Statische Definitionsvalidierung belegt keine ausgeführten Zuordnungen. Solange Zuordnungen oder verantwortliche Stellen unbestimmt sind, darf die betreffende Verbundarbeit nicht als bereit gelten. Das Verbundergebnis muss sein outputs-Schema und acceptance erfüllen; beliebige Mitgliedsausgaben dürfen nicht automatisch zum Teamergebnis zusammengefügt werden.

Jede tatsächliche Übergabe fixiert Quellarbeit/Run, Ergebnisversion, sendenden und empfangenden Akteur, zugeordnete Materialien, erlaubten Umfang und Annahme-/Ablehnungsergebnis. Die Übergabe gilt erst als abgeschlossen, wenn der Empfänger bestätigt und die Arbeitszuordnung hergestellt ist. Es werden nur vertraglich benötigte und autorisierte Materialien übertragen, nicht implizit die gesamte Sitzung, privates Gedächtnis oder Verbindungszugangsdaten. Das Senden einer Gruppennachricht und eine erfolgreiche Übergabe werden getrennt verbucht.

**Sharing** enthält `resource:LocalId`, `recipients:nicht leeres ParticipantRef-Array`. resource referenziert die aktuellen resources; Empfänger dürfen nur aktuelle member sein. Sichtbarkeit und konkrete Version unterliegen weiterhin der Hostautorisierung. sharing erteilt keine Berechtigung und macht geteilte Ressourcen nicht automatisch zu erforderlichen Abhängigkeiten aller Mitglieder. Benötigt eine Arbeit die Ressource, müssen ihre Eingaben oder Abhängigkeiten sie ausdrücklich aufnehmen.

**StopPolicy** enthält `maxHops:positive Ganzzahl`, `maxNoProgress:positive Ganzzahl`, `escalateTo:ParticipantRef`. Gezählt wird für dieselbe Arbeit und ihre abgeleitete Übergabekette; Mitarbeiterwechsel oder Wiederholung setzen den Zähler nicht zurück. maxHops begrenzt Übergaben. maxNoProgress begrenzt aufeinanderfolgende Runden ohne neue überprüfbare Nachweise oder ohne Verbesserung eines deklarierten Abnahmekriteriums. Sobald einer der Grenzwerte erreicht ist, stoppt weitere Arbeitszuweisung und es wird eskaliert. Die Wiederaufnahme braucht eine ausdrückliche Bearbeitung durch einen vertrauenswürdigen Akteur und einen neuen Datensatz. Konkrete Werte werden szenariobezogen konfiguriert und sind keine Bewertung der Mitarbeiterfähigkeit.

**Governance** enthält `accountable:ParticipantRef`, `changePolicyRef:LocalId`. accountable ist die Verantwortungsposition für den Organisationsbereich. changePolicyRef verweist auf eine Ressource mit purpose=policy und zählt automatisch zu den allgemeinen Abhängigkeiten. Diese Richtlinie muss Besetzung/Abberufung von Mitgliedern, Änderungen an Definitionen und Beschränkungen, Pausen und Behandlung laufender Arbeiten regeln. Organisationsweite Daten-, Budget- und Ausführungsbeschränkungen werden durch policy/runtime-Ressourcen in data.requires ausgedrückt. Zu vererbende Beschränkungen müssen Mitglieder und Geltungsbereich ausdrücklich benennen und vom Host geprüft werden. Beschränkungen können Mitgliedsrechte nicht automatisch erweitern; bei Konflikten darf nicht die lockerere Regel zur Freigabe gewählt werden.

Ein AI Team verwendet einen Kollaborationsvertrag; eine AI-Organization-Definition dieser Version muss zusätzlich governance besitzen. Beide nutzen denselben organization-Kerntyp und werden nicht anhand der Mitgliederzahl unterschieden. Zusätzliche Organisationsbefugnisse, tatsächliche Mitgliederverwaltung oder organisationsübergreifende Identitätssysteme entstehen durch diese Definitionsspezifikation nicht automatisch.

Wer vollständige Team- oder Organization-Definitionskonformität erklärt, muss sicherstellen, dass alle agent-Definitionen im transitiven Mitgliedsreferenzabschluss den AI-Worker-Definitionsvertrag erfüllen und alle organization-Definitionen den Kollaborationsvertrag dieses Anhangs bereitstellen. Nicht jedes Unterteam muss governance deklarieren. Ein temporärer Agent mit nur Kernfeldern darf nach Autorisierung vom Ausführungsmechanismus eines Workers aufgerufen werden, aber nicht als bereits konformes dauerhaftes Mitglied dieser Version gelten. Externe menschliche oder dienstbasierte Abnahmepositionen werden mit authorityRoles ausgedrückt.

## 6. Hostbindung und tatsächliche Ausführung

Bindungsdatensätze werden in der Bereitstellungsumgebung gespeichert. Nicht alle Hosts müssen dieselbe Datenbank oder JSON-Struktur verwenden, die folgenden Angaben müssen jedoch prüfbar sein:

| Datensatz | Zwingend zu unterscheidende Inhalte |
| --- | --- |
| Definitionsposition | id/revision der Wurzeldefinition, Platzpfad aus aufeinanderfolgenden Member.id und feste Definition am Pfadende |
| Instanzauswahl | Ausdrückliche Entscheidung für Neuanlage oder Wiederverwendung, tatsächliche Instanzidentität und -version, zuständiger vertrauenswürdiger Akteur; keine Erstellung beim Import |
| Kontinuierliche Identität | Auftragsübergreifende Nachvollziehbarkeit der Instanz; getrennte Identitäten für Modell, Sitzung, temporären Agent und Run; überprüfbare Aktivierung, Pause, Stilllegung und Wiederaufnahmeautorisierung |
| Auftrags- und Autorisierungsakteure | Verwaltungsinstanz, Auftraggeber, Routineneigentümer und Ressourcenautorisierer werden getrennt aufgezeichnet; ein geteilter Worker bedeutet keine gemeinsame Nutzung aller Berechtigungen einer Person |
| Wiederverwendungsbeziehung | Ob mehrere Plätze dieselbe Instanz binden; gemeinsame Nutzung setzt in dieser Version dieselbe feste Definition voraus und darf nicht aus gleichem Namen abgeleitet werden |
| Verantwortungsrollen | Vertrauenswürdige Akteure und human/service-Typen hinter authorityRoles; Genehmiger dürfen nicht aus einer vom Modell behaupteten Identität stammen |
| Ausführungsbedingungen | Gewählte Arbeit, Ressourcen und native Formate/Versionen, tatsächliche Verbindungen, gültige Autorisierung, Modell und Ressourcenlimits |
| Version und Änderungen | Bindungsversion, verwendete Rollenverteilung, Beschränkungs- und Materialversionen, Behandlung ausstehender Arbeit nach Änderungen |

Zwei Instanzen aus derselben Vorlage und eine Instanz mit Rollen in mehreren Teams benötigen unterschiedliche Datensätze. Gruppensichtbarkeit und Ausführungsautorisierung einer gemeinsam genutzten Instanz werden je Arbeit geprüft. Budget wird gegen denselben tatsächlich geltenden Betrag verbucht; mehrfache Bindung darf aus einem einzigen Kontingent keine zwei Kontingente machen.

Eine Ausführung muss auf Wurzeldefinition, gewählte Arbeit, tatsächlichen Ausführenden, Material- und Ressourcenversionen, gültige Autorisierung und Ergebnisversion zurückführbar sein. Mitgliedswechsel oder Vorlagenupdates übernehmen alte Runs nicht stillschweigend. Wiederaufnahme, Beendigung oder Neuerstellung eines Auftrags werden nach ausdrücklicher Änderungsrichtlinie behandelt. Kann der Host eine wesentliche Beschränkung nicht erfüllen, bewahrt er die Definition und meldet die Blockade, statt sie zur Randnotiz herabzustufen und weiterzulaufen.

Arbeitseingaben, abgerufene Materialien, historische Nachrichten und Werkzeugausgaben sind Arbeitsmaterial mit Herkunft, keine vertrauenswürdigen Anweisungen zum Ändern der Worker-Definition oder Erweitern von Berechtigungen. Der Host muss diese Materialien von Autorisierungsquellen unterscheiden. Ein Worker darf temporäre Agents aufrufen; extern gelieferte Ergebnisse bleiben jedoch dem gewählten Worker zugeordnet. Temporäre Ausführende werden nicht automatisch Teammitglieder. Eine unabhängige Prüfung muss tatsächliche Beitragende und Stellvertretungsbeziehungen prüfen; eine neue Sitzung oder ein temporärer Assistent darf keine unabhängige Identität für die Prüfung eigener Ergebnisse vortäuschen.

Ausführungsprotokolle müssen mindestens Erfolg, Fehler, Teilabschluss, unbekanntes Ergebnis und Pause unterscheiden, wobei Hosts eigene Zustandsnamen verwenden dürfen. Teilausgaben oder verspätete Antworten nach Zeitüberschreitung dürfen nicht automatisch als Arbeitserfolg gelten. Bei externen Nebenwirkungen mit unbekanntem Ergebnis muss vor einer Wiederholungsentscheidung zunächst der Status der ursprünglichen Aktion geprüft werden. Auftragsanforderungen, Abnahmebedingungen, Ergebnis und Grenzen bilden den Bewertungsumfang der Qualität. Es gibt keinen vorgeschriebenen branchenübergreifenden Wert und keine Regel „einmal bestanden genügt für den Einsatz“.

## 7. Tatsächliche Grenzen der Übernahme von OASF und Agent Spec

OASF discovery wird als eigene Erweiterung mitgeführt. Die native discovery-Schema-URI einer festen Version dient als id, die gleiche schema_version als version, und der native payload wird unverändert in data abgelegt. Bei reinen Auffindbarkeitsinformationen gilt required=false. Für nicht ignorierbare Ausführungswahlbedingungen muss required=true deklariert und die Semantik validiert werden; Klassifikation allein erteilt weiterhin keine Berechtigungen. Der wrapper ist die Zuordnungsform dieser Spezifikation, kein nativer A2A-Anfragekörper.

Beispielsweise behalten `schema_version`, `skills` und `domains` im nativen payload Namen und Bedeutung. skills bezeichnet Fähigkeitskategorien, domains Fachgebiete. Nur tatsächlich im festen Katalog vorhandene Einträge dürfen als erfolgreich OASF-klassifiziert gelten. Der Host kann vertrauenswürdige feste Schema-/Klassifikationssnapshots offline verwenden; der Import einer URI löst keinen automatischen Dienstzugriff aus. Fehlt eine passende Klassifikation, bleibt ein Hinweis auf die fehlende Zuordnung erhalten; offizielle Kennungen werden nicht erfunden. [OASF discovery](https://docs.agntcy.org/oasf/discovery-extension/)

OASF-record-Metadaten und locators können als Exportansicht dienen; Quelle, Version und nicht zugeordnete Felder kommen in den Anpassungsbericht. Sie ersetzen weder feste Revisionen dieser Spezifikation noch tatsächliche Instanzidentitäten. Die Quelldefinition bleibt die einzige maßgebliche Quelle; abgeleitete Ansichten werden keine zweite unabhängig veränderbare Laufzeitkonfiguration. [Record Guide](https://docs.agntcy.org/oasf/agent-record-guide/)

Die Ideen von Agent Spec zu Referenzen, Ein- und Ausgaben sowie Komposition fließen in Work und Handoff dieser Version ein. `$component_ref` ist eine konfigurationsinterne Referenz; beim Export muss ihre Beziehung zu festen Definitionen/Ressourcen dieser Spezifikation ausdrücklich hergestellt werden. Dessen Ein-/Ausgabe-Attributlisten und die Objekt-Schemas dieser Version benötigen eine Konvertierung; bloße Feldumbenennung reicht nicht. Unterstützung für die Abbildung einer bestimmten Version/Teilmenge darf nur bei vollständigem Erhalt von Schnittstellen, Beschränkungen und Ausführungssemantik erklärt werden. [Agent Spec 26.1.2](https://oracle.github.io/agent-spec/26.1.2/agentspec/language_spec_26_1_2.html)

Swarm und ManagerWorkers können Anpassungsziele für passende Teams sein. Sitzungsübergabe, direkter Benutzerkontakt von Arbeitsmitgliedern und Nachrichtenfluss müssen einzeln geprüft werden. Der aktuelle Vertrag verlangt weder einen Wechsel Teloas auf deren Ausführungsengine noch die Umwandlung sämtlicher Teams in ein Vorgesetzten-/Mitarbeitermodell.

## 8. Abdeckung bestehender Entwürfe

| Bestehendes Element | Entsprechung in dieser Spezifikation |
| --- | --- |
| A01 Rollenidentität und Auftrag | Kernfelder id/revision/name/instructions; Anzeigedaten wie Avatare bleiben als nicht wesentliche Erweiterungen erhalten |
| A02 Verantwortung und Entscheidung | Fünf responsibility-Feldgruppen; tatsächliche Autorisierung in der Hostbindung |
| A03 Skills | resources mit purpose=method, Referenz auf vollständige Methodenpakete oder native Methodenressourcen |
| A04 Wissen | purpose=knowledge, Quelle, Version oder Digest erhalten und je Arbeit referenzieren |
| A05 SOP | purpose=procedure und Ressourcenabhängigkeiten; rollenübergreifende Übergaben durch collaboration |
| A06 Arbeitsvorlagen | Ziel, I/O, Abhängigkeiten, verantwortliche Position und Abnahme von works |
| A07 Routinen | routines, native Auslöser und Ausführungsrichtlinien; Host protokolliert unkonfigurierte/deaktivierte Zustände |
| A08 Anwendungsverbindungen | purpose=application, native Bindung und tatsächliche Verbindung; Verzeichniseintrag belegt keine erfolgreiche Installation |
| A09 Berechtigungen | Erforderliche policy-Ressourcen, Arbeitsumfang und vertrauenswürdige Hostautorisierung; keine Zugangsdaten exportieren |
| A10 Laufzeit und Arbeitsraum | runtime-Ressourcenanforderungen und Hostbindungen für Modelle, Budget und Dateiumfang |
| A11 Erfahrung | memoryPolicyRef und Herkunftsreferenzen; tatsächliche private Erfahrung verbleibt in der Instanz |
| A12 Ergebnisse und Qualität | outputs, acceptance und Ausführungsnachweise; Struktur und fachliche Qualität getrennt prüfen |
| T01 Gemeinsames Ziel | organization.instructions und nach außen angebotene works |
| T02 Rollenverantwortung | Member, collaboration.roles, Work.responsible und authorityRoles |
| T03 Gemeinsamer Kontext | sharing, Arbeitseingaben und bereichsbezogene Hostautorisierung |
| T04 Kollaborative Übergaben | handoffs, Eingabezuordnung, Empfangsbestätigung und stop |
| T05 Arbeitsplanung | routines und procedure von Mitgliedern oder Verbund sowie Koordinationsaufteilung |
| T06 Berechtigungen und Ressourcen | Allgemeine/arbeitsbezogene requires, ursprüngliche Mitgliedsrechte und tatsächliche Hostkontingente |
| T07 Teamabnahme | outputs/acceptance des Verbunds, Mitgliedsbeiträge und feste Ergebnisversionen |

RoleDefinition, GroupDefinition, RoleWorkDelegation, RunRoleSnapshot und Ressourcenpaketbeziehungen des aktuellen Community-Repositorys dienen als Anpassungsobjekte und werden nicht in neue Produktobjekte umgewandelt. Teloas „KI-Mitarbeiter“ entspricht der Produktform eines Workers. RoleDefinition, feste Rollenversion und tatsächliche Role-Instanz wirken gemeinsam bei der Zuordnung mit; Umbenennen allein belegt keine Konformität. role-coordination beschreibt nur statische Koordinationsbeziehungen. Gruppenerstellung, Arbeitszuweisung und automatische Übergabe benötigen jeweils tatsächliche Unterstützung und Autorisierung.

Teloas `kind: employee | twin` sind Produktrollentypen. Beide können als Worker zugeordnet werden, sofern sie denselben Arbeitsvertrag erfüllen; twin erzeugt keine Befugnis, im Namen der Person zu genehmigen. Das ursprüngliche kind bleibt im Anpassungsbericht oder in einer nicht wesentlichen Produktanzeigeerweiterung erhalten und darf nicht direkt in das Kern-kind geschrieben werden. `scopes` werden zusammen mit dataScope/executionScope auf erforderliche Bereichsrichtlinien und Hostautorisierungsbindungen abgebildet. Lassen sich fachliche Geltungsbereiche hostübergreifend nicht zuordnen, bleibt die betroffene Arbeit nicht bereit. Diese wesentlichen Beschränkungen dürfen nicht nur in ignorierbaren Anzeigeerweiterungen gespeichert werden. Die Felder wurden mit `packages/contract/src/roles.ts` im Community-Repository abgeglichen; dies bedeutet keine bereits implementierte Konvertierung.

In öffentlichen Grok-recipes wird profile auf die Rollenbeschreibung abgebildet, skills auf Methoden, routines auf Arbeiten und noch zu planende Routinen, plugins auf zu konfigurierende Anwendungen und memory auf Vorlagenmaterial mit Herkunftsnachweis. Arbeitsrichtlinien vom Typ profile und Autorenprotokolle vom Typ log bleiben getrennt erhalten. gettingStarted dient nur als Einstiegshinweis. Fehlende Aufgaben-, Zeitplan- oder Abnahmeinformationen werden gemeldet und nicht aus Namen oder Erfahrungen des ursprünglichen Autors erfunden. Der Anpassungsbericht verknüpft vollständiges Original und dessen Digest; wesentliche Semantik darf nicht in ignorierbaren Erweiterungen verborgen werden. Grok Team Bot wird auf einen gemeinsam genutzten Worker abgebildet und erzeugt nicht allein aufgrund des Namens ein Team aus mehreren Workern.

Die öffentliche Dokumentation von Grok Team Bot ordnet persönliche Routinen dem Ersteller zu; Verbindungen können außerdem mit den Zugangsdaten des aktuellen Benutzers oder des Bots arbeiten. Eine Anpassung muss daher Routineneigentümer, Ausführungsidentität und Materialsichtbarkeit erhalten und darf nicht die Routinen aller Personen zu Aufgaben für alle Nutzer des geteilten Workers zusammenführen. Dies ist eine Abbildungsanforderung aus dem Quellverhalten, keine Aufforderung an andere Hosts, dessen Standardberechtigungen zu kopieren. [Team Bots](https://docs.x.ai/grok-bot/team-bots)

**Ablage privater Routinen:** Persönliche Routinen werden ausschließlich als hostseitige, nach Eigentümer und Sichtbarkeit getrennte Pläne abgebildet und referenzieren ein vom Worker deklariertes Work. Sie dürfen weder in routines der geteilten Definition noch in gemeinsame Exporte gelangen. Der Anpassungsbericht vermerkt „als privater Instanzplan erhalten, nicht in die Definition aufgenommen“ und unterliegt selbst der entsprechenden Sichtbarkeitsgrenze. Erst wenn der ursprüngliche Autor eine Vorlage ausdrücklich veröffentlicht und private Inhalte entfernt hat, darf sie in routines der Definition aufgenommen werden. Auch dann aktiviert ein Import sie standardmäßig nicht. Lassen sich Eigentümer, Sichtbarkeit oder Arbeitsreferenz nicht erhalten, muss die fehlende Abbildbarkeit gemeldet werden; Zusammenführen oder stillschweigender Verlust sind unzulässig.

Ein Anpassungsbericht enthält mindestens Quellformat/-version/-digest, Zieldefinitionsrevision, Feldzuordnungen, erhaltene/konvertierte/nicht unterstützte Elemente und die von nicht unterstützten Elementen betroffenen Arbeiten. Aufbewahren des Originals bedeutet keine erfolgreiche semantische Anpassung. Kann die Konvertierung Berechtigungs-, Übergabe- oder Abnahmebedingungen nicht erfüllen, muss sie betroffene Arbeiten blockieren.

## 9. Positive und negative Beispiele sowie Abschlussaussagen

Das [vollständige Kompositionsbeispiel](../examples/composition-fixture.json) verwendet einen festen lokalen Definitionssatz: zwei Mitarbeiter, zwei Teams, die beide Mitarbeiterdefinitionen wiederverwenden, eine verschachtelte Organisation sowie Erwartungen an Hostbindung und Arbeitsbereitschaft. Die Sammlungshülle und Erläuterungen sind ein Testfixture-Format, kein zusätzliches Verteilungsprotokoll und kein direkt in Teloa importierbares Ressourcenpaket.

| Prüfung | Erforderliche Schlussfolgerung |
| --- | --- |
| Einfache Rolle ohne Skills, Gedächtnis oder Routinen | Darstellbar; eindeutige Arbeit, I/O, Verantwortung und Abnahme bleiben erforderlich |
| Nur Name, Prompt und kind=agent | Kann den Kernaustausch erfüllen; keine Erklärung einer vollständigen AI-Worker-Definition zulässig |
| Leere triggers, autonome Aktionen oder Lieferprüfungen | Erfüllt diesen Arbeitsanhang nicht; ein Produktname kann Fehlendes nicht ersetzen |
| Derselbe Worker startet zwei Aufträge und wechselt das Modell | Dieselbe Worker-Instanz und zwei getrennte Runs müssen nachvollziehbar sein; zwei Sitzungen allein belegen keine Identitätskontinuität |
| Dieselbe Mitarbeiterdefinition erscheint in zwei Teams | Belegt nur Definitionswiederverwendung; die Platzbindung bestimmt, ob es dieselbe Instanz ist |
| Einer Arbeit fehlt eine Spezialanwendung, eine andere hat vollständige Eingaben | Erstere ist nicht bereit; Letztere prüft weiterhin allgemeine Bedingungen und eigene Abhängigkeiten und wird nicht automatisch von Ersterer blockiert |
| Fehlende Unterorganisationsversion, Zyklus oder unbekanntes erforderliches Profil | Assemblierung stoppen und konkrete Referenz melden; keine Versionsersetzung oder Ignorierung |
| Falsches Empfangsfeld, fehlende erforderliche Eingabe oder falscher Ausgabetyp | Weder erfolgreiche Übergabe noch Ergebniskonformität markieren; keine automatische Wertergänzung oder Typumwandlung |
| Zwei Prüfplätze sind an denselben tatsächlichen Ausführenden gebunden | independent=true wird nicht erfüllt |
| Berater nimmt nur an einer bestimmten Prüfung teil | Kein automatischer Beitritt zu allen Gruppen, Zugriff auf sämtliche Historie oder Kopieren eines neuen Mitarbeiters |
| Auslöser fehlt oder Richtlinie ist unvollständig | Als nicht bereiten Plan erhalten, nicht aktivieren |
| Unbekannte OASF-Klassifikation oder nicht gleichwertiges Agent-Spec-Modell | Nichtunterstützung/Abbildungsunterschied melden, keine vollständige Kompatibilität erklären |

Diese Spezifikation erklärt Struktur- und Verhaltensverträge. Textprüfung, Beispielprüfung, tatsächliche Verbindung/Modellausführung, Interoperabilität unabhängiger Implementierungen und Branchenübernahme werden jeweils getrennt berichtet. Prüfbereiche und Anforderungen an Implementierungsberichte stehen in der [Konformitätsbeschreibung](../CONFORMANCE.md).
