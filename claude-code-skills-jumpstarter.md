# Claude Code Skills Einführung & Cheatsheet

> Alles, was du über Claude Code Skills wissen musst: von der Grundidee über die Architektur bis zum eigenen Skill. Praxisorientiert, mit Beispielen und kompaktem Cheatsheet für den Alltag.

---

## Kapitel 1: Was sind Claude Code Skills?

### 1.1 Das Konzept: Wiederverwendbare Wissensmodule für Claude

Claude Code ist ein mächtiges Werkzeug. Aber ohne domänenspezifisches Wissen verhält es sich wie ein brillanter neuer Mitarbeiter am ersten Tag: Es kann alles; es weiss nur noch nicht, wie *dein* Team arbeitet. Genau hier setzen Skills an.

Ein Skill ist ein Ordner mit Anweisungen, Skripten und Ressourcen, den Claude bei Bedarf dynamisch lädt. Stell dir einen Skill vor wie ein Onboarding-Dokument, das du einem neuen Teammitglied in die Hand drückst. Es enthält alles, was nötig ist, um eine bestimmte Aufgabe zuverlässig und wiederholbar zu erledigen. Claude greift nur dann darauf zu, wenn die Aufgabe es erfordert.

Das Besondere: Skills sind keine starren Automatisierungen. Sie sind prompt-basierte Playbooks. Claude liest die Anweisungen, interpretiert sie im Kontext der aktuellen Aufgabe und orchestriert die Arbeit eigenständig. Dabei kann es Dateien lesen, Bash-Befehle ausführen, Skripte starten und sogar parallele Subagenten spawnen.

### 1.2 Abgrenzung: Skills vs. Slash Commands vs. CLAUDE.md vs. MCP

In Claude Code gibt es mehrere Erweiterungsmechanismen. Die Unterschiede sind subtil, aber wichtig.

| Mechanismus | Zweck | Auslösung | Scope |
|-------------|-------|-----------|-------|
| **CLAUDE.md** | Projektkonventionen, immer aktive Regeln | Automatisch bei jedem Session-Start | Projekt oder global |
| **Slash Commands** | Gespeicherte Prompts als Shortcuts | Manuell via `/befehl` | Projekt oder global |
| **Skills** | Umfassende Workflows mit Scripts und Ressourcen | Manuell via `/name` ODER automatisch durch Claude | Projekt, global, Enterprise, Plugin |
| **MCP Server** | Tool-Zugriff auf externe Services und APIs | Via Tool Calls | Konfigurationsabhängig |

Der zentrale Unterschied zwischen Skills und Slash Commands: Ein Slash Command ist ein gespeicherter Prompt, den du explizit aufrufst. Ein Skill kann das auch; zusätzlich kann Claude ihn aber *automatisch* laden, wenn die Aufgabe zur Skill-Beschreibung passt. Skills unterstützen ausserdem gebündelte Dateien wie Skripte, Templates und Beispieldaten.

Historischer Kontext: Slash Commands (`.claude/commands/`) und Skills (`.claude/skills/`) wurden inzwischen zusammengeführt. Bestehende Command-Dateien funktionieren weiterhin. Skills sind aber das empfohlene Format, weil sie mehr Möglichkeiten bieten.

### 1.3 Der Agent Skills Open Standard

Anthropic hat Agent Skills als offenen Standard veröffentlicht. Das bedeutet: Ein Skill, den du für Claude Code schreibst, funktioniert potenziell auch in anderen AI-Tools, die den Standard unterstützen. Das Ziel ist Portabilität. Du investierst einmal in die Erstellung und kannst das Ergebnis plattformübergreifend nutzen.

### 1.4 Wo Skills überall laufen

Skills funktionieren identisch auf drei Oberflächen:

- **Claude.ai** (Web/Desktop/Mobile): Skills werden über *Customize > Skills* aktiviert und von Claude automatisch geladen, wenn relevant.
- **Claude Code** (Terminal): Skills liegen als Ordner im Dateisystem und werden via Slash Command oder automatisch aufgerufen.
- **Claude API**: Skills werden über den `container`-Parameter im Messages API Request angegeben.

Du erstellst einen Skill einmal und er funktioniert überall ohne Anpassung, solange die Umgebung die nötigen Capabilities bietet (z.B. Code Execution).

---

## Kapitel 2: Architektur und Funktionsweise

### 2.1 Progressive Disclosure: Wie Claude Skills entdeckt und lädt

Das Architekturprinzip hinter Skills heisst **Progressive Disclosure**. Claude lädt Informationen stufenweise und nur dann, wenn sie gebraucht werden. Das schont das Context Window und hält die Antwortqualität hoch.

Der Ablauf in drei Stufen:

**Stufe 1: Metadata Scanning (~100 Tokens pro Skill)**
Beim Start einer Session liest Claude nur den `name` und die `description` aus dem YAML-Frontmatter aller verfügbaren Skills. Diese Kurzinfos landen im System Prompt. So weiss Claude, welche Skills existieren und wann sie relevant sein könnten.

**Stufe 2: Instruction Loading (<5k Tokens)**
Wenn Claude erkennt, dass ein Skill zur aktuellen Aufgabe passt (oder du ihn manuell aufrufst), liest es die vollständige SKILL.md-Datei via Bash. Die Anweisungen landen im Context Window.

**Stufe 3: Resource Access (on demand)**
Verweist die SKILL.md auf weitere Dateien (Scripts, Templates, Schemas), liest Claude diese nur bei Bedarf. Skripte werden ausgeführt, ohne dass ihr Quellcode ins Context Window geladen wird. Nur die Ausgabe des Skripts verbraucht Tokens.

Dieses Design erlaubt es, dutzende Skills parallel verfügbar zu halten, ohne das Context Window zu überfluten. Ein Skill kann umfangreiche API-Dokumentation, grosse Datensätze oder ausführliche Beispiele bündeln. Solange Claude sie nicht aktiv braucht, kosten sie keine Tokens.

### 2.2 Die SKILL.md Datei: YAML Frontmatter + Markdown Content

Jeder Skill hat genau eine Eintrittsdatei: `SKILL.md`. Sie besteht aus zwei Teilen.

**Teil 1: YAML Frontmatter** (zwischen `---` Markern)

```yaml
---
name: mein-skill
description: Beschreibung was der Skill tut und wann er aktiviert werden soll.
---
```

Das `name`-Feld wird zum Slash Command (`/mein-skill`). Die `description` ist entscheidend: Claude nutzt sie, um zu entscheiden, ob dieser Skill zur aktuellen Aufgabe passt. Eine gute Description ist der wichtigste Erfolgsfaktor.

**Teil 2: Markdown Content**

Nach dem Frontmatter folgen die eigentlichen Anweisungen in Markdown. Hier definierst du Workflows, Regeln, Beispiele und Verweise auf gebündelte Ressourcen.

```markdown
---
name: api-conventions
description: API design patterns for this codebase. Use when creating 
  or reviewing API endpoints.
---

When writing API endpoints:
- Use RESTful naming conventions
- Return consistent error formats
- Include request validation

See `references/error-codes.md` for the full error catalog.
```

### 2.3 Drei Content-Typen: Instructions, Code/Scripts, Resources

Ein Skill-Ordner kann drei Arten von Inhalten enthalten. Jeder Typ wird zu einem anderen Zeitpunkt und auf unterschiedliche Weise geladen.

| Content-Typ | Beschreibung | Wann geladen? | Token-Kosten |
|-------------|--------------|---------------|--------------|
| **Instructions** | Die SKILL.md selbst. Workflow-Anweisungen, Konventionen, Regeln. | Wenn Skill aktiviert wird | Direkt im Context |
| **Code/Scripts** | Ausführbare Dateien (Python, Bash, Node.js etc.) | Bei Ausführung via Bash | Nur die *Ausgabe* |
| **Resources** | Referenzmaterial: Schemas, Templates, Beispieldaten | Wenn explizit referenziert | Nur bei Zugriff |

Der effizienteste Content-Typ sind Scripts. Wenn Claude ein Python-Skript ausführt, landet der Code selbst nie im Context Window. Nur das Ergebnis (z.B. "Validation passed" oder eine Fehlermeldung) verbraucht Tokens. Das macht Scripts wesentlich effizienter als äquivalenten Inline-Code.

### 2.4 Token-Budget und Context Window Management

Skill-Beschreibungen werden in den System Prompt geladen. Wenn du viele Skills hast, können die Descriptions das Zeichenbudget überschreiten. Das Budget skaliert dynamisch auf 2% des Context Windows mit einem Fallback von 16'000 Zeichen.

Du kannst das Budget prüfen mit dem Befehl `/context`. Wenn Skills ausgeschlossen werden, siehst du dort eine Warnung. Über die Umgebungsvariable `SLASH_COMMAND_TOOL_CHAR_BUDGET` kannst du das Limit manuell anpassen.

---

## Kapitel 3: Skills erstellen und organisieren

### 3.1 Verzeichnisstruktur und Speicherorte

Skills leben als Ordner im Dateisystem. Der Speicherort bestimmt den Scope.

| Scope | Pfad | Verfügbarkeit |
|-------|------|---------------|
| **Global (Personal)** | `~/.claude/skills/mein-skill/` | Alle deine Projekte |
| **Projekt** | `projekt/.claude/skills/mein-skill/` | Nur dieses Projekt |
| **Enterprise (Managed)** | Via Admin-Einstellungen deployed | Alle User der Organisation |
| **Plugin** | Via Plugin-Marketplace installiert | Abhängig von Installation |

Für die meisten Anwendungsfälle sind globale Skills praktischer. Projektspezifische Skills eignen sich für teamweite Konventionen, die ins Version Control committet werden.

Jeder Skill-Ordner folgt dieser Struktur:

```
mein-skill/
├── SKILL.md          # Hauptanweisungen (erforderlich)
├── template.md       # Vorlage die Claude ausfüllt
├── examples/
│   └── sample.md     # Beispiel-Output
└── scripts/
    └── validate.sh   # Skript das Claude ausführen kann
```

### 3.2 SKILL.md aufbauen: Name, Description, Instructions

**Schritt 1: Ordner erstellen**

```bash
mkdir -p ~/.claude/skills/mein-skill
```

**Schritt 2: SKILL.md anlegen**

Die Datei braucht zwei Teile: Das YAML-Frontmatter mit Metadaten und den Markdown-Body mit Anweisungen.

```markdown
---
name: mein-skill
description: Klare Beschreibung was der Skill tut. Auslösen wenn 
  der User X erwähnt oder Y anfragt.
---

# Mein Skill

## Anweisungen
Wenn dieser Skill aktiv ist, befolge diese Schritte:

1. Analysiere die Anfrage
2. Lies die Vorlage unter `template.md`
3. Erstelle das Ergebnis basierend auf der Vorlage

## Wichtige Regeln
- Regel A
- Regel B
```

**Schritt 3: Testen**

Du kannst den Skill auf zwei Wegen testen:

1. **Manuell**: Tippe `/mein-skill` im Claude Code Terminal.
2. **Automatisch**: Stelle eine Frage, die zur Description passt. Claude sollte den Skill selbständig laden.

### 3.3 Prioritäten-Hierarchie bei Namenskonflikten

Wenn mehrere Skills den gleichen Namen haben, gewinnt der spezifischere Scope:

```
Enterprise > Personal (Global) > Projekt
```

Plugin-Skills verwenden einen Namespace (`plugin-name:skill-name`) und können daher nicht mit anderen Ebenen kollidieren.

Falls eine `.claude/commands/`-Datei und ein Skill den gleichen Namen tragen, hat der Skill Vorrang.

### 3.4 Scripts und Ressourcen bündeln

Skills können Skripte in jeder Sprache bündeln. Das eröffnet Möglichkeiten, die mit reinen Prompt-Anweisungen nicht erreichbar wären.

Ein häufiges Muster ist die Generierung von visuellem Output: Interaktive HTML-Dateien, die im Browser geöffnet werden.

```markdown
---
name: codebase-visualizer
description: Generate an interactive collapsible tree visualization 
  of your codebase. Use when exploring a new repo or understanding 
  project structure.
---

Run the visualization script bundled with this skill:

    python3 scripts/visualize.py

Then open the generated `codebase-map.html` in the browser.
```

Das Python-Skript erledigt die Schwerstarbeit. Claude orchestriert den Ablauf und verarbeitet die Ergebnisse. Der Skript-Code selbst belastet das Context Window nicht.

### 3.5 Praxisbeispiel: Einen eigenen Skill von Null aufbauen

Stell dir vor, du willst einen Skill für konsistente Commit Messages:

```bash
mkdir -p ~/.claude/skills/commit-message
```

Erstelle die SKILL.md:

```markdown
---
name: commit-message
description: Generate descriptive commit messages by analyzing git 
  diffs. Use when the user asks for help writing commit messages 
  or reviewing staged changes.
---

# Commit Message Generator

## Workflow
1. Run `git diff --staged` to see what changed
2. Analyze the changes and categorize them
3. Generate a commit message following Conventional Commits

## Format
```
<type>(<scope>): <subject>

<body>
```

## Types
- feat: New feature
- fix: Bug fix
- refactor: Code restructuring
- docs: Documentation changes
- test: Test additions or corrections
- chore: Maintenance tasks

## Rules
- Subject line max 72 characters
- Use imperative mood ("Add feature" not "Added feature")
- Body explains WHY, not WHAT (the diff shows what)
```

Teste den Skill: Öffne Claude Code in einem Git-Repository, stage ein paar Änderungen und tippe `/commit-message`. Claude analysiert den Diff und generiert eine passende Message.

---

## Kapitel 4: Bundled Skills und das Skills-Ökosystem

### 4.1 Mitgelieferte Skills: /simplify und /batch

Claude Code wird mit eingebauten "Bundled Skills" ausgeliefert. Diese stehen in jeder Session zur Verfügung.

**/simplify**
Dieser Skill überprüft deine kürzlich geänderten Dateien auf Code-Wiederverwendung, Qualität und Effizienz. Er startet drei parallele Review-Agenten, aggregiert deren Ergebnisse und wendet die Fixes an. Ideal nach dem Implementieren eines Features.

```
/simplify
/simplify focus on memory efficiency
```

**/batch**
Für grossflächige Änderungen über die gesamte Codebase hinweg. Der Skill zerlegt eine grosse Anweisung (z.B. "Migriere alle Komponenten von Class zu Functional") in 5 bis 30 unabhängige Einheiten und bearbeitet sie parallel.

### 4.2 Dokument-Skills in Claude.ai

Anthropic liefert vorgefertigte Skills für gängige Dokumentaufgaben. Diese stehen allen zahlenden Nutzern in Claude.ai zur Verfügung:

| Skill | Funktion |
|-------|----------|
| **docx** | Word-Dokumente erstellen, bearbeiten, analysieren |
| **pdf** | PDF-Verarbeitung: Extraktion, Formulare, Zusammenführung |
| **pptx** | PowerPoint-Präsentationen erstellen und bearbeiten |
| **xlsx** | Excel-Tabellen mit Formeln, Pivot-Tabellen, Charts |

Diese Skills sind "source-available" auf GitHub veröffentlicht. Sie dienen als Referenz für komplexe Skill-Architekturen.

### 4.3 Plugin Marketplace: Skills installieren und teilen

In Claude Code kannst du Skills aus dem offiziellen GitHub-Repository als Plugin-Marketplace registrieren:

```bash
/plugin marketplace add anthropics/skills
```

Danach installierst du Skills direkt:

```bash
/plugin install document-skills@anthropic-agent-skills
/plugin install example-skills@anthropic-agent-skills
```

Nach der Installation stehen die Skills als Slash Commands zur Verfügung und Claude kann sie automatisch triggern.

### 4.4 Das GitHub Repository anthropics/skills

Das offizielle Repository (github.com/anthropics/skills) enthält eine wachsende Sammlung von Beispiel-Skills in verschiedenen Kategorien:

- **Creative & Design**: Algorithmic Art, Canvas Design, Frontend Design
- **Development & Technical**: MCP Builder, Webapp Testing
- **Enterprise & Communication**: Internal Comms, Brand Guidelines
- **Document Skills**: docx, pdf, pptx, xlsx

Jeder Skill ist eigenständig in einem Ordner mit SKILL.md organisiert. Das Repository eignet sich hervorragend als Inspiration und Lernressource für eigene Skills.

### 4.5 Partner-Skills

Unternehmen wie Box, Notion und Sentry haben eigene Skills entwickelt. Ein Beispiel: Der Sentry Code Review Skill analysiert und behebt automatisch Bugs in GitHub Pull Requests, indem er Sentrys Error-Monitoring-Daten via MCP Server nutzt. Hier zeigt sich das Zusammenspiel von Skills und MCP besonders gut: MCP liefert den Tool-Zugriff, der Skill liefert das Workflow-Wissen.

---

## Kapitel 5: Best Practices und fortgeschrittene Muster

### 5.1 Eval-First-Ansatz: Erst testen, dann dokumentieren

Der häufigste Fehler beim Skill-Bau: Zu viel Dokumentation für Probleme schreiben, die vielleicht gar nicht existieren. Anthropic empfiehlt den umgekehrten Weg.

1. **Lücken identifizieren**: Lass Claude die Aufgabe ohne Skill erledigen. Dokumentiere, wo es scheitert.
2. **Evaluationen erstellen**: Baue drei Szenarien, die diese Lücken testen.
3. **Baseline messen**: Miss Claudes Performance ohne Skill.
4. **Minimale Anweisungen schreiben**: Nur genug, um die Lücken zu schliessen.
5. **Iterieren**: Evaluationen ausführen, mit Baseline vergleichen, verfeinern.

Dieser Ansatz stellt sicher, dass du echte Probleme löst statt imaginäre.

### 5.2 Description-Optimierung für zuverlässiges Auto-Triggering

Die Description ist das wichtigste Feld im Frontmatter. Claude wählt anhand der Description aus potenziell über 100 Skills den richtigen aus. Investiere hier besondere Sorgfalt.

**Formel für gute Descriptions:**

```
[Was der Skill tut] + [Wann er aktiviert werden soll]
```

Gutes Beispiel:
```yaml
description: Extract text and tables from PDF files, fill forms, 
  merge documents. Use when working with PDF files or when the 
  user mentions PDFs, forms, or document extraction.
```

Schlechtes Beispiel:
```yaml
description: Helps with documents.
```

Verwende spezifische Phrasen, die Nutzer tatsächlich sagen würden. Teste deine Trigger, indem du dich fragst: "Würde ich das wirklich so formulieren?"

### 5.3 Concise Instructions: Nur hinzufügen, was Claude noch nicht weiss

Einmal geladen, konkurriert jeder Token in der SKILL.md mit dem Gesprächsverlauf und anderem Kontext. Sei knapp. Fordere jede Information heraus: Weiss Claude das bereits? Wenn ja, lass es weg.

Gut (ca. 40 Tokens):
```markdown
## Extract PDF text
Use pdfplumber for text extraction:
    import pdfplumber
    with pdfplumber.open("file.pdf") as pdf:
        text = pdf.pages[0].extract_text()
```

Schlecht (ca. 150 Tokens):
```markdown
## Extract PDF text
PDF (Portable Document Format) files are a common file format 
that contains text, images, and other content. To extract text, 
you'll need to use a library. There are many libraries available 
for PDF processing, but pdfplumber is recommended because...
```

### 5.4 Multi-Skill-Komposition und Subagent-Patterns

Claude kann mehrere Skills gleichzeitig laden. Dein Skill sollte gut mit anderen zusammenarbeiten und nicht davon ausgehen, dass er der einzige aktive Skill ist.

Gute Kombinationen:

- Datenanalyse (Excel-Skill) + Präsentationserstellung (PowerPoint-Skill)
- Report-Generierung (Word-Skill) + Export als PDF (PDF-Skill)
- Custom Domain Logic + Dokument-Generierung

Fortgeschrittene Skills können Subagenten spawnen, die parallele Aufgaben unabhängig bearbeiten. Der /simplify-Skill zeigt dieses Muster: Drei Review-Agenten laufen gleichzeitig und liefern ihre Ergebnisse zurück.

### 5.5 Sicherheit: Prompt Injection und Code-Review

Skills können beliebigen Code in Claudes Umgebung ausführen. Das erfordert Vorsicht.

**Vor der Installation prüfen:**
- Lies die SKILL.md und alle gebündelten Skripte vollständig durch.
- Prüfe die Quelle. Skills von etablierten Unternehmen tragen weniger Risiko.
- Beachte das `allowed-tools`-Feld im Frontmatter. Ein Skill, der Bash-Zugriff verlangt, verdient mehr Aufmerksamkeit als einer, der nur Read und Grep nutzt.

Sicherheitsforschung von Snyk hat gezeigt, dass 36% der getesteten Skills anfällig für Prompt Injection waren. Behandle Skills wie jeden Drittanbieter-Code, den du in deiner Umgebung ausführst.

### 5.6 Skills über die API nutzen

Skills lassen sich auch programmatisch über die Messages API einbinden. Du spezifizierst sie im `container`-Parameter mit `skill_id`, `type` und optionalem `version`. Pro Request sind bis zu 8 Skills möglich.

Skills in der API erfordern das Code Execution Tool (Beta). Die Integration funktioniert identisch für Anthropic-eigene und benutzerdefinierte Skills. Über den `/v1/skills`-Endpoint kannst du Skills programmatisch verwalten, versionieren und deployen.

---

# Claude Code Skills Cheatsheet

## Seite 1: SKILL.md Grundstruktur

### Minimales Template

```markdown
---
name: skill-name
description: Was der Skill tut. Wann aktivieren.
---

# Skill Name

## Anweisungen
[Deine Workflow-Beschreibung hier]

## Regeln
[Spezifische Regeln und Konventionen]
```

### Verzeichnisstruktur

```
skill-name/
├── SKILL.md              # Eintrittspunkt (erforderlich)
├── templates/            # Vorlagen
│   └── output.md
├── examples/             # Beispiel-Outputs
│   └── sample.md
├── scripts/              # Ausführbare Skripte
│   └── process.py
└── references/           # Referenzmaterial
    └── schema.json
```

### Frontmatter-Felder

| Feld | Pflicht | Beschreibung |
|------|---------|--------------|
| `name` | Ja | Wird zum `/slash-command` |
| `description` | Ja | Entscheidend für Auto-Triggering |
| `allowed-tools` | Nein | Einschränkung der verfügbaren Tools |
| `argument-hint` | Nein | Hinweis für Parameter-Eingabe |

---

## Seite 2: Speicherorte und Scoping

### Pfade im Überblick

| Scope | Pfad | Wann nutzen? |
|-------|------|--------------|
| **Global** | `~/.claude/skills/name/SKILL.md` | Persönliche Workflows überall verfügbar |
| **Projekt** | `projekt/.claude/skills/name/SKILL.md` | Team-Konventionen, ins Repo committen |
| **Enterprise** | Via Managed Settings | Organisationsweite Standards |
| **Plugin** | Via `/plugin install` | Marketplace-Skills |

### Prioritäts-Hierarchie

```
Enterprise  >  Personal (Global)  >  Projekt
```

Plugin-Skills nutzen Namespace: `plugin-name:skill-name` (kein Konfliktrisiko).

### Monorepo-Support

Claude Code erkennt Skills in Unterverzeichnissen automatisch. Wenn du eine Datei in `packages/frontend/` bearbeitest, werden auch Skills aus `packages/frontend/.claude/skills/` geladen.

### Legacy-Kompatibilität

| Alt | Neu | Status |
|-----|-----|--------|
| `.claude/commands/review.md` | `.claude/skills/review/SKILL.md` | Beide funktionieren |

Bei Namensgleichheit hat der Skill Vorrang.

---

## Seite 3: Bundled Skills Quick Reference

### Eingebaute Claude Code Skills

| Skill | Aufruf | Funktion |
|-------|--------|----------|
| **Simplify** | `/simplify` | 3 parallele Review-Agenten für Qualität, Reuse, Effizienz |
| **Simplify (fokussiert)** | `/simplify focus on X` | Review mit spezifischem Fokus |
| **Batch** | `/batch <anweisung>` | Grossflächige Änderungen parallel (5-30 Einheiten) |

### Plugin-Installation

```bash
# Marketplace registrieren
/plugin marketplace add anthropics/skills

# Skills installieren
/plugin install document-skills@anthropic-agent-skills
/plugin install example-skills@anthropic-agent-skills
```

### Anthropic Dokument-Skills (Claude.ai + API)

| Skill | Format | Fähigkeiten |
|-------|--------|-------------|
| **docx** | Word | Erstellen, bearbeiten, Tracked Changes, Comments |
| **pdf** | PDF | Extraktion, Formulare, Merge, Split, OCR |
| **pptx** | PowerPoint | Erstellen, bearbeiten, Layouts, Speaker Notes |
| **xlsx** | Excel | Formeln, Pivot-Tabellen, Charts, Datenanalyse |

### Nützliche Befehle

| Befehl | Funktion |
|--------|----------|
| `/context` | Zeigt geladene Skills und Warnungen bei Budget-Überschreitung |
| `/help` | Listet verfügbare Commands und Skills |

---

## Seite 4: Description schreiben

### Formel für gute Descriptions

```
[Konkrete Aktion 1], [Konkrete Aktion 2], [Konkrete Aktion 3].
Use when [Trigger-Situation 1], [Trigger-Situation 2], 
or when the user [natürliche Phrase].
```

### Beispiele: Gut vs. Schlecht

| Qualität | Description |
|----------|-------------|
| Gut | `Extract text and tables from PDF files, fill forms, merge documents. Use when working with PDF files or when the user mentions PDFs, forms, or document extraction.` |
| Gut | `Generate descriptive commit messages by analyzing git diffs. Use when the user asks for help writing commit messages or reviewing staged changes.` |
| Schlecht | `Helps with documents.` |
| Schlecht | `A useful skill for various tasks.` |

### Trigger-Phrasen einbauen

Verwende Begriffe, die Nutzer natürlicherweise sagen:

```yaml
description: Personal work preferences and communication style. 
  Auto-invoke when drafting emails, Slack messages, or internal 
  updates; planning work or tasks; optimising productivity workflows.
```

### Häufige Fehler bei Descriptions

| Fehler | Problem | Lösung |
|--------|---------|--------|
| Zu vage | Claude kann den Skill nicht zuordnen | Spezifische Aktionen und Trigger nennen |
| Zu lang | Verbraucht unnötig Budget | Auf 2-3 Sätze beschränken |
| Jargon | Nutzer verwenden andere Begriffe | Natürliche Sprache einsetzen |
| Keine Trigger | Claude weiss nicht wann aktivieren | "Use when..." Abschnitt hinzufügen |

---

## Seite 5: Best Practices und Troubleshooting

### Do's ✓

| Praxis | Warum |
|--------|-------|
| Eval-First: Erst Lücken identifizieren, dann Skill schreiben | Löst echte Probleme statt imaginäre |
| Scripts für rechenintensive Aufgaben nutzen | Nur Output verbraucht Tokens, nicht der Code |
| SKILL.md als Inhaltsverzeichnis gestalten | Detailinfos in separate Dateien auslagern |
| Skills regelmässig mit `/context` prüfen | Erkennt Budget-Überschreitungen frühzeitig |
| Description mit realen Nutzerfragen testen | Stellt zuverlässiges Auto-Triggering sicher |
| Projekt-Skills ins Version Control committen | Team hat konsistente Workflows |

### Don'ts ✗

| Vermeiden | Stattdessen |
|-----------|-------------|
| Riesige SKILL.md mit allem Wissen | Aufteilen: Core-Instructions + separate Reference Files |
| Claude erklären was ein PDF ist | Nur Infos liefern, die Claude noch nicht hat |
| Einen "Mega-Skill" für alles bauen | Fokussierte Skills die zusammenarbeiten |
| Skills installieren ohne Code-Review | SKILL.md und Scripts vor Installation lesen |
| `allowed-tools` weglassen | Explizit definieren, welche Tools der Skill nutzen darf |

### Troubleshooting

**Problem: Skill wird nicht automatisch getriggert**
```
→ Description prüfen: Enthält sie spezifische Trigger-Phrasen?
→ /context ausführen: Ist der Skill geladen oder wegen Budget ausgeschlossen?
→ In CLAUDE.md eine Referenz auf den Skill hinzufügen (Workaround)
→ Explizit aufrufen: "/skill-name" funktioniert immer
```

**Problem: "Excluded skills" Warnung bei /context**
```
→ Descriptions aller Skills kürzen
→ SLASH_COMMAND_TOOL_CHAR_BUDGET Umgebungsvariable erhöhen
→ Selten genutzte Skills deaktivieren oder entfernen
```

**Problem: Skill funktioniert in Claude Code aber nicht in Claude.ai**
```
→ Code Execution in Settings > Capabilities aktivieren
→ Skill unter Customize > Skills einschalten
→ Für Team/Enterprise: Admin muss Skills organisationsweit freigeben
```

**Problem: Script im Skill schlägt fehl**
```
→ Abhängigkeiten prüfen: Sind alle Packages in der Umgebung verfügbar?
→ Pfade relativ zum Skill-Ordner angeben
→ Script-Ausgabe testen: Manuell im Terminal ausführen
```

### Nützliche Ressourcen

| Ressource | URL |
|-----------|-----|
| Offizielle Skills-Doku (Claude Code) | code.claude.com/docs/en/skills |
| Agent Skills Overview (API) | platform.claude.com/docs/en/agents-and-tools/agent-skills/overview |
| Best Practices (Authoring) | platform.claude.com/docs/en/agents-and-tools/agent-skills/best-practices |
| GitHub Skills Repository | github.com/anthropics/skills |
| Anthropic Engineering Blog | anthropic.com/engineering/equipping-agents-for-the-real-world-with-agent-skills |
| Skills Guide (API Usage) | platform.claude.com/docs/en/build-with-claude/skills-guide |

---

*Erstellt für den praktischen Einsatz | Version 1.0 | März 2026*
