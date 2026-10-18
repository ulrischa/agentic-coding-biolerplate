# Anleitung zur Coding-Vorlage

Ein schlanker, sprachneutraler Ausgangspunkt für GitHub Copilot in VS Code,
OpenAI Codex und Claude Code. Enthalten sind gemeinsame Projektregeln, zwei
Review-Skills und Rumpfdateien für die Projektdokumentation. Eine Anwendung,
Laufzeitumgebung oder Testinfrastruktur ist noch nicht enthalten.

## Schnellstart

1. Repository klonen, als ZIP herunterladen oder auf GitHub als Vorlage verwenden,
   falls dort „Use this template“ angeboten wird. Beim Übernehmen in ein bestehendes
   Projekt vorhandene Dateien und Regeln vorher abgleichen, nicht überschreiben.
2. Den Repository-Hauptordner in VS Code öffnen. Die Vorlage selbst braucht weder
   WSL noch Administratorrechte, Node.js oder zusätzliche Pakete. Programmiersprache
   und Coding-Assistent müssen separat verfügbar sein; dafür kann ein kostenpflichtiger
   Zugang erforderlich sein. Erweiterungsempfehlungen installieren nichts automatisch.
3. In `README.md` Projektziel, unterstützte Versionen, Einrichtung und echte
   Prüfkommandos eintragen. Nicht benötigte Prüfungen als „Not applicable“ markieren.
4. `AGENTS.md` um konkrete Projektkonventionen ergänzen. Bekannten Aufbau und feste
   Grenzen in `docs/ARCHITECTURE.md` festhalten. `TODO` bedeutet offen, nicht beschlossen.
5. Eine neue Agentensitzung im Hauptordner starten und die Erkennung wie unten
   beschrieben prüfen. Danach mit einer kleinen, klar begrenzten Aufgabe beginnen.

Die `.gitkeep`-Dateien halten `src/` und `tests/` im Git-Repository und können
entfallen, sobald dort echte Dateien liegen. Diese Ordner sind Vorschläge: Ein
kleines PHP-Projekt darf seine Dateien beispielsweise im Hauptordner ablegen.

## Zuständigkeit der Dateien

| Datei / Ordner | Inhalt | Pflege |
| --- | --- | --- |
| `README.md` | Ziel, Voraussetzungen, Einrichtung, Nutzung und Prüfkommandos der späteren Anwendung | Beim Projektstart und bei Änderungen |
| `AGENTS.md` | Dauerhafte Arbeitsregeln für Coding-Agenten | Bei verbindlichen Konventionen und Grenzen |
| `CLAUDE.md` | Import der gemeinsamen Regeln mit `@AGENTS.md` | Normalerweise unverändert |
| `.agents/skills/` | Vollständige Abläufe für Code- und Security-Review | Bei Änderungen der Prüfverfahren |
| `.claude/skills/` | Kurze Einstiegspunkte zu denselben Abläufen | Bei neuen oder umbenannten Skills |
| `.editorconfig` | UTF-8, LF-Zeilenenden, Einrückung und abschließender Zeilenumbruch | An den Projektstil anpassen |
| `.gitignore` | Ausschlüsse für lokale Konfiguration, Abhängigkeiten und erzeugte Dateien | An den Stack anpassen |
| `.vscode/extensions.json` | Copilot-Erweiterungsempfehlungen | Nach Bedarf |
| `docs/ARCHITECTURE.md` | Komponenten, Datenfluss, Schnittstellen und Randbedingungen | Bei relevanten Strukturänderungen |
| `docs/DECISIONS.md` | Gründe für Entscheidungen mit langfristigen Folgen | Nur bei echten Entscheidungen |
| `docs/BOILERPLATE.de.md` | Diese Anleitung zur Vorlage | Bei Änderungen an der Vorlage |

**Anwendungsdokumentation gehört in die README, Arbeitsregeln in AGENTS.md,
Erklärungen zur Vorlage in diese Datei.** Prüfkommandos nur in der README pflegen;
andere Dateien verweisen darauf. Die Beispielentscheidung in `DECISIONS.md` ist
kein beschlossener Architekturentscheid. Nicht benötigte Dokumentationsrümpfe
können entfallen, wenn auch die Verweise darauf angepasst werden.

## Unterstützung durch die Assistenten

Ein Agent ist hier ein Coding-Assistent mit Datei- und Werkzeugzugriff. Ein
anderes Modell im Copilot-Chat auszuwählen ist nicht automatisch dasselbe wie
die separate Codex- oder Claude-Code-Integration. Entscheidend sind der gestartete
Assistent, seine Version und seine Konfiguration.

| Assistent | Projektregeln | Skills dieser Vorlage |
| --- | --- | --- |
| GitHub Copilot in VS Code | `AGENTS.md` | Erkennt `.agents/skills/` und auch `.claude/skills/` |
| OpenAI Codex, CLI oder Erweiterung | `AGENTS.md` | `.agents/skills/` |
| Claude Code, CLI oder Erweiterung | `CLAUDE.md` importiert `AGENTS.md` | `.claude/skills/` verweist auf `.agents/skills/` |

Neuere Claude-Code-Versionen können `AGENTS.md` unter bestimmten Bedingungen auch
direkt laden. Der kleine Import bleibt für unterschiedliche Versionen sinnvoll.
Gemeint sind Coding-Werkzeuge mit Repository-Zugriff, nicht gewöhnliche
Chat-Weboberflächen ohne angebundenes Projekt.

Die Claude-Einstiegspunkte enthalten keinen zweiten Prüfablauf und keine
symbolischen Links. Damit benötigt ein normaler Windows-Checkout keine besonderen
Symlink-Rechte. Ein Einstiegspunkt fordert den Agenten auf, die zentrale Datei zu
öffnen: Das ist ein Arbeitsauftrag, kein technischer Import. Bei der ersten
Verwendung kontrollieren, ob die zentrale Datei wirklich gelesen wird.

Copilot kann denselben Skill aus beiden Verzeichnissen entdecken. Falls doppelte
Einträge erscheinen, führen beide zum gleichen Ablauf; verwende den unter
`.agents/skills/`. Eine bestimmte Erkennungsreihenfolge wird nicht vorausgesetzt.
Die Namen beginnen mit `project-`, um Konflikte mit eingebauten Review-Befehlen
zu vermeiden.

## Erkennung prüfen und Skills verwenden

Projektregeln werden entsprechend der Erkennung des Assistenten eingebunden.
Skills sind aufgabenbezogene Anleitungen. Der Assistent kann sie anhand ihrer
Beschreibung auswählen; diese automatische Auswahl ist nicht garantiert.

Starte nach dem Einrichten mit diesem Auftrag:

> Lies die geltenden Projektregeln. Nenne offene Projektangaben und die beiden
> Review-Skills einschließlich ihrer Dateipfade. Ändere keine Dateien.

Eine Antwort allein beweist keine automatische Erkennung. Prüfe zusätzlich:

- **Copilot:** In der Befehlspalette `Chat: Open Customizations` öffnen und die
  Anweisungen und Skills für den ausgewählten Assistenten ansehen. Beim älteren
  „Local“-Modus kann `chat.useAgentsMdFile` die Erkennung abschalten.
- **Claude Code:** Mit `/context` die Projektregeln unter den Speicherdateien
  kontrollieren und im `/`-Menü nach `project-` suchen.
- **Codex:** Im Skill-Menü beziehungsweise über `/skills`, soweit in der verwendeten
  Oberfläche verfügbar, die beiden Projektskills suchen.

| Aufgabe | Copilot | Codex | Claude Code |
| --- | --- | --- | --- |
| Code-Review | „Nutze den Skill project-code-review für meine Änderungen.“ | `$project-code-review` | `/project-code-review` |
| Sicherheitsprüfung | „Nutze den Skill project-security-review für meine Änderungen.“ | `$project-security-review` | `/project-security-review` |

Gib den Umfang an, etwa „nur die uncommitteten Änderungen“ oder „den letzten
Commit“. Reviews liefern Befunde und Vorschläge; Korrekturen werden umgesetzt,
wenn dein Auftrag sie umfasst. Ein Review beweist keine Fehlerfreiheit und
ersetzt keine passenden Tests.

Falls die Erkennung fehlt: Hauptordner, ausgewählten Assistenten, Großschreibung
der Dateinamen und Skill-Header prüfen. Nach Änderungen eine neue Sitzung starten.
Persönliche oder organisatorische Einstellungen können Projektanpassungen
beschränken. Vollständige interaktive Sitzungen aller drei Produkte wurden im
Rahmen dieses Vorlagen-Reviews nicht ausgeführt.

## Im Alltag

Gute Aufträge enthalten Ziel, Grenzen und erwartetes Ergebnis, zum Beispiel:

> Ergänze im bestehenden Formular die Prüfung der Kennung. Erhalte Fehleranzeige
> und Eingabeübernahme. Halte die dokumentierte PHP-Version ein. Prüfe gültige und
> ungültige Eingaben und erkläre kurz die Änderung.

Die Projektregeln verlangen fokussierte Änderungen, den Erhalt bestehender
Funktionen und ehrliche Angaben zu ausgeführten Prüfungen. Vor einem Commit Diff
und Prüfergebnisse ansehen. Commit, Push und Deployment müssen vom Auftrag gedeckt
sein. Bei längeren Aufgaben neue Erkenntnisse in den vorhandenen Projektdokumenten
festhalten, damit die nächste Sitzung darauf aufbauen kann.

## Einen eigenen Skill ergänzen

1. `.agents/skills/project-mein-ablauf/SKILL.md` anlegen:

   ```markdown
   ---
   name: project-mein-ablauf
   description: Explain what the workflow does and when to use it.
   ---

   # My workflow

   1. Identify the requested scope.
   2. Perform the documented steps and verify the result.
   ```

2. `.claude/skills/project-mein-ablauf/SKILL.md` mit demselben Header anlegen.
   Darunter diesen Leseauftrag einfügen:

   ```markdown
   Read and follow [the canonical workflow](../../../.agents/skills/project-mein-ablauf/SKILL.md).
   Resolve this link relative to this file, not the terminal working directory.
   ```

3. Namen, Beschreibungen und Verweise konsistent halten. Die eigentlichen Schritte
   ausschließlich in `.agents/skills/` pflegen.

Der relative Pfad benötigt **drei** Schritte nach oben vom Claude-Skill-Ordner
zum Repository-Hauptordner. Ergänzende Skripte und Referenzen erst hinzufügen,
wenn benötigt. Produktspezifische Header für Werkzeugfreigaben oder
Aufrufbeschränkungen nicht ungeprüft auf andere Assistenten übertragen.

## Was noch fehlt und was später kommen kann

Vor produktiver Entwicklung fehlen vor allem **echte Projektangaben und
funktionierende Prüfkommandos**. Mehr Agentenkonfiguration ersetzt diese nicht.
Eine passende Lizenz muss der Eigentümer wählen, wenn andere den Code unter
explizit geregelten Bedingungen verwenden sollen; diese Entscheidung wird nicht
vorweggenommen.

Weitere Dateien nur bei konkretem Bedarf ergänzen:

- CI, sobald tatsächliche Tests oder andere reproduzierbare Prüfungen existieren.
- VS-Code-Tasks und Debug-Konfiguration, wenn die Befehle feststehen.
- Dateispezifische Regeln, wenn Teile des Projekts eigene Konventionen brauchen.
- MCP, Hooks oder spezialisierte Agenten für einen passenden Arbeitsablauf.

Zusätzliche Prompt-Dateien und Kopien derselben Regeln sind für den Einstieg
nicht erforderlich. `.editorconfig` wirkt nur in unterstützenden Editoren oder
Erweiterungen; die Datei allein formatiert keinen Code.

`.gitignore`, Suchausschlüsse und Markdown-Anweisungen sind **keine
Zugriffssperren**. Geheimnisse möglichst außerhalb des Arbeitsbereichs halten.
Werkzeugfreigaben und Betriebssystemrechte bestimmen den tatsächlichen Zugriff.
Die `.env.example`-Ausnahme ist ausschließlich für Platzhalter gedacht.

## Ergebnis des Reviews der Ausgangsvorlage

Die ursprüngliche Struktur und die relativen Claude-Verweise waren korrekt.
Die Skill-Namen wurden gegen Konflikte mit eingebauten Review-Befehlen präzisiert.
Die gemischte README wurde in Projekt-Rumpf und diese Anleitung getrennt.
Ergänzt wurden klare Regeln zum Erhalt vorhandener Änderungen, zu offenen
Anforderungen und zu fehlender Testinfrastruktur. Dafür sind weder zusätzliche
Laufzeitabhängigkeiten noch automatische Befehlsausführungen erforderlich.

## Offizielle Referenzen

Die Unterstützung kann sich ändern. Bei Abweichungen die Dokumentation der
verwendeten Version prüfen.

- [VS Code: Projektanweisungen](https://code.visualstudio.com/docs/agent-customization/custom-instructions)
- [VS Code: Agent Skills](https://code.visualstudio.com/docs/agent-customization/agent-skills)
- [Codex: Skills](https://developers.openai.com/codex/skills/)
- [Claude Code: Skills](https://code.claude.com/docs/en/skills)
- [Claude Code: Projektregeln und Imports](https://code.claude.com/docs/en/memory)
- [Agent Skills: offenes Format](https://agentskills.io/specification)
