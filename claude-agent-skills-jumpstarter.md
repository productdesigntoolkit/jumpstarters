# Claude Agent Skills Einführung & Cheatsheet

> Alles was du brauchst, um eigene Skills für Claude zu bauen, zu testen und zu verteilen. Von den Grundlagen über bewährte Patterns bis zum Marketplace.

---

## Kapitel 1: Was sind Claude Agent Skills?

### 1.1 Das Konzept

Ein Skill ist ein Ordner mit Instruktionen, der Claude beibringt, wie er bestimmte Aufgaben oder Workflows erledigen soll. Statt Claude in jedem Gespräch erneut deine Vorlieben, Prozesse und Fachkenntnisse zu erklären, lehrst du ihn einmal und profitierst immer wieder davon.

Das klingt simpel. Ist es auch. Im Kern ist ein Skill nichts anderes als eine `SKILL.md`-Datei in einem Ordner. Doch diese Einfachheit ist bewusst gewählt: Skills sind leichtgewichtig, portabel und komponierbar. Du kannst mehrere Skills gleichzeitig aktivieren und sie ergänzen sich gegenseitig.

Skills entfalten ihre Stärke bei wiederkehrenden Workflows: Frontend-Designs aus Specs generieren, Dokumente nach deinem Styleguide erstellen, Research mit konsistenter Methodik durchführen oder mehrstufige Prozesse orchestrieren. Sie funktionieren nahtlos mit Claudes eingebauten Fähigkeiten wie Code-Ausführung und Dokumenterstellung.

### 1.2 Die drei Design-Prinzipien

Skills basieren auf drei Grundprinzipien, die ihre Architektur prägen:

| Prinzip | Bedeutung | Praxiseffekt |
|---------|-----------|--------------|
| **Progressive Disclosure** | Informationen werden stufenweise geladen | Nur was Claude gerade braucht, landet im Kontext. Das spart Tokens und hält die Qualität hoch. |
| **Composability** | Skills arbeiten nebeneinander | Dein Skill kann zusammen mit anderen aktiv sein. Er sollte nie davon ausgehen, allein geladen zu sein. |
| **Portability** | Ein Skill, alle Plattformen | Derselbe Skill funktioniert in Claude.ai, Claude Code und via API ohne Anpassung. |

Progressive Disclosure verdient besondere Aufmerksamkeit. Es ist ein Drei-Ebenen-System:

**Ebene 1 (YAML Frontmatter):** Wird immer im System Prompt geladen. Enthält nur genug Information, damit Claude entscheiden kann, ob der Skill relevant ist. Hier stehen Name und Beschreibung.

**Ebene 2 (SKILL.md Body):** Wird geladen, wenn Claude denkt, der Skill passt zur aktuellen Aufgabe. Enthält die vollständigen Instruktionen.

**Ebene 3 (Verlinkte Dateien):** Zusätzliche Dateien im Skill-Ordner, die Claude nur bei Bedarf öffnet. Hier liegen Referenzdokumente, Templates und Scripts.

### 1.3 Die Küchen-Analogie

Falls du auch mit MCP (Model Context Protocol) arbeitest, hilft diese Analogie:

**MCP** ist die professionelle Küche. Es gibt dir Zugang zu Werkzeugen, Zutaten und Geräten. MCP verbindet Claude mit externen Services wie Notion, Linear, Figma oder Slack.

**Skills** sind die Rezepte. Sie beschreiben Schritt für Schritt, wie man mit den verfügbaren Werkzeugen etwas Wertvolles erstellt.

Zusammen ermöglichen sie es, komplexe Aufgaben zu erledigen, ohne jeden Schritt selbst herausfinden zu müssen. Aber Skills funktionieren auch eigenständig ohne MCP; viele nutzen nur Claudes eingebaute Fähigkeiten wie Code-Ausführung und Dateierstellung.

---

## Kapitel 2: Skills vs. CLAUDE.md vs. System Prompt

### 2.1 Drei Wege, Claude zu instruieren

Es gibt mehrere Möglichkeiten, Claudes Verhalten zu steuern. Die Unterschiede sind zentral für die Frage, wann du welchen Mechanismus nutzen solltest.

**CLAUDE.md** ist eine Datei, die im Root eines Projekts liegt (oder in `~/.claude/` als globale Variante). Sie wird immer geladen, wenn Claude in diesem Kontext arbeitet. CLAUDE.md ist monolithisch: alles steht in einer Datei, alles wird immer gelesen. Sie eignet sich für projektspezifische Konventionen, Coding-Standards und Kontextinformationen, die in jeder Interaktion relevant sind.

**Skills** sind modular und trigger-basiert. Claude lädt einen Skill nur, wenn die Aufgabe dazu passt. Das bedeutet weniger Token-Verbrauch und gezielteres Wissen. Skills sind ausserdem wiederverwendbar; derselbe Skill kann in verschiedenen Projekten und über verschiedene Plattformen hinweg eingesetzt werden.

**System Prompt** wird auf Plattform-Level gesetzt und ist für Endnutzer nicht direkt editierbar. Er definiert das Grundverhalten von Claude.

### 2.2 Vergleich

| Eigenschaft | CLAUDE.md | Skills | System Prompt |
|-------------|-----------|--------|---------------|
| **Laden** | Immer (ganzes Dokument) | Nur bei Relevanz (trigger-basiert) | Immer |
| **Struktur** | Eine Datei, monolithisch | Ordner mit mehreren Dateien | Plattform-definiert |
| **Portabilität** | Projektgebunden | Plattformübergreifend | Nicht portabel |
| **Wiederverwendung** | Copy-Paste zwischen Projekten | Ein Skill, überall einsetzbar | Nicht möglich |
| **Token-Effizienz** | Alles wird immer geladen | Progressives Laden | Immer geladen |
| **Bearbeitung** | Direkt in der Datei | SKILL.md + Ressourcen | Nur über API/Plattform |
| **Scope** | Ein Projekt | Global oder projektspezifisch | Global |

### 2.3 Wann was verwenden?

**Nutze CLAUDE.md wenn:**
Du projektspezifische Regeln hast, die in jeder Interaktion gelten sollen. Beispiel: "In diesem Projekt verwenden wir TypeScript mit Strict Mode und testen mit Vitest."

**Nutze Skills wenn:**
Du einen wiederverwendbaren Workflow hast, der nicht immer relevant ist, aber wenn er relevant ist, präzise ausgeführt werden soll. Beispiel: Ein Skill, der Rezepte immer im gleichen Format erstellt und auf WordPress pusht.

**Kombiniere beides:**
CLAUDE.md definiert den Projektkontext ("Wir arbeiten mit Next.js und Supabase"). Skills stellen spezialisierte Workflows bereit ("Erstelle eine neue API-Route nach unserem Standard"). Die CLAUDE.md kann sogar auf Skills verweisen und erklären, wann sie zum Einsatz kommen sollen.

---

## Kapitel 3: Standard-Skills vs. eigene Skills

### 3.1 Was Claude mitbringt

Claude kommt mit einer Reihe vorinstallierter Skills. Diese decken häufige Anwendungsfälle ab und sind sofort einsetzbar. Du findest sie unter `/mnt/skills/public/` (Kern-Skills), `/mnt/skills/examples/` (Beispiel-Skills) und `/mnt/skills/user/` (deine eigenen).

**Public Skills (Kern-Skills von Anthropic):**

| Skill | Funktion |
|-------|----------|
| `docx` | Word-Dokumente erstellen, lesen, bearbeiten |
| `pptx` | PowerPoint-Präsentationen erstellen und bearbeiten |
| `xlsx` | Excel-Dateien erstellen, lesen, bearbeiten |
| `pdf` | PDFs lesen, erstellen, zusammenführen, ausfüllen |
| `frontend-design` | Production-grade Web-Interfaces erstellen |
| `product-self-knowledge` | Korrekte Fakten über Anthropics Produkte |

**Example Skills (Vorlagen und Inspiration):**

| Skill | Funktion |
|-------|----------|
| `skill-creator` | Neue Skills erstellen und bestehende verbessern |
| `doc-coauthoring` | Strukturierter Workflow für Co-Authoring |
| `web-artifacts-builder` | Komplexe Web-Artifacts mit React und shadcn/ui |
| `canvas-design` | Visuelle Designs und Poster als PNG/PDF |
| `algorithmic-art` | Generative Kunst mit p5.js |
| `theme-factory` | Themes für Artifacts und Dokumente |
| `mcp-builder` | MCP-Server erstellen |
| `slack-gif-creator` | Animierte GIFs für Slack |
| `internal-comms` | Interne Kommunikation (Reports, Updates, Newsletter) |

### 3.2 Grenzen der Standard-Skills

Standard-Skills sind generisch gehalten. Sie kennen weder deinen Markenstil noch deine spezifischen Workflows. Ein paar Beispiele, wo sie an Grenzen stossen:

Der `docx`-Skill erstellt Word-Dokumente. Aber er weiss nicht, dass dein Unternehmen ein bestimmtes Letterhead-Template verwendet. Der `pptx`-Skill baut Präsentationen. Aber er kennt deine Markenfarben nicht. Der `frontend-design`-Skill erstellt Web-Interfaces. Aber er weiss nicht, dass dein Team ausschliesslich mit Tailwind und shadcn/ui arbeitet.

### 3.3 Eigene Skills: Warum und wann

Du brauchst einen eigenen Skill, sobald du merkst, dass du Claude wiederholt dasselbe erklärst. Typische Signale:

Du kopierst immer wieder dieselben Instruktionen in den Chat. Du korrigierst Claude regelmässig bei Format oder Stil. Du hast einen mehrstufigen Workflow, den du jedes Mal von vorne beschreibst. Dein Team nutzt Claude unterschiedlich für dieselbe Aufgabe und bekommt inkonsistente Ergebnisse.

### 3.4 Von Standard zu Custom

Der eleganteste Weg: Du baust auf Standard-Skills auf und erweiterst sie. Dein eigener Skill kann auf einen Standard-Skill verweisen oder dessen Logik ergänzen.

Beispiel: Du erstellst einen `pdt-pptx`-Skill, der den Standard-`pptx`-Skill nutzt, aber dein eigenes Branding (Farben, Fonts, Master-Slides) und deine spezifischen Folientypen ergänzt. Oder du baust einen `recipe-format`-Skill, der Rezepte in einem exakten Format für deinen Food-Blog erstellt, und kombinierst ihn mit einem `wp-publish-recipe`-Skill für die WordPress-Veröffentlichung.

---

## Kapitel 4: Speicherorte und Umgebungen

### 4.1 Claude.ai

In Claude.ai lädst du Skills über die Oberfläche hoch:

```
Settings > Capabilities > Skills > Upload Skill
```

Du kannst den Skill-Ordner als ZIP-Datei hochladen. Nach dem Upload erscheint der Skill in deiner Skill-Liste und kann per Toggle aktiviert oder deaktiviert werden. Skills in Claude.ai werden vom System unter `/mnt/skills/user/` abgelegt und sind dann in Gesprächen verfügbar.

### 4.2 Claude Code

In Claude Code gibt es zwei Speicherorte für eigene Skills:

**User-Level Skills** liegen in `~/.claude/skills/`. Diese sind global für alle Projekte verfügbar. Hier platzierst du Skills, die du projektübergreifend brauchst.

**Projekt-Level Skills** liegen in `.claude/skills/` im Projekt-Root. Diese gelten nur für das aktuelle Projekt. Ideal für teamspezifische Skills, die ins Repository eingecheckt werden.

```
# User-Level (global)
~/.claude/skills/
└── recipe-format/
    └── SKILL.md

# Projekt-Level (projektspezifisch)
./mein-projekt/.claude/skills/
└── api-route-generator/
    └── SKILL.md
```

### 4.3 API

Für programmatische Anwendungen bietet die API direkte Kontrolle:

Der `/v1/skills`-Endpoint ermöglicht das Auflisten und Verwalten von Skills. Über den `container.skills`-Parameter kannst du Skills in Messages-API-Requests einbinden. Versionskontrolle und Verwaltung laufen über die Claude Console. Die API erfordert das Code Execution Tool Beta, das die sichere Umgebung für Skills bereitstellt.

### 4.4 Organisation-Level

Seit Dezember 2025 können Admins Skills workspace-weit deployen. Das bedeutet: automatische Updates, zentralisierte Verwaltung und einheitliche Workflows für das ganze Team. Wenn ein Admin einen Skill deployed, steht er allen Nutzern der Organisation zur Verfügung.

### 4.5 Pfade im System

Wenn du in Claude.ai oder über die API arbeitest, liegen Skills unter diesen Pfaden:

| Pfad | Inhalt | Zugriff |
|------|--------|---------|
| `/mnt/skills/public/` | Anthropics Kern-Skills (docx, pptx, xlsx etc.) | Read-only |
| `/mnt/skills/examples/` | Beispiel-Skills (skill-creator, canvas-design etc.) | Read-only |
| `/mnt/skills/user/` | Deine eigenen hochgeladenen Skills | Read-only (Claude liest sie) |

Alle drei Pfade sind read-only. Claude kann sie lesen und ihre Instruktionen befolgen, aber nicht direkt bearbeiten. Wenn du einen Skill modifizieren willst, kopierst du ihn in dein Arbeitsverzeichnis, bearbeitest ihn dort und lädst die neue Version hoch.

---

## Kapitel 5: Der Skills Marketplace

### 5.1 Skills als offener Standard

Anthropic hat Agent Skills als offenen Standard veröffentlicht. Genau wie MCP sollen Skills portabel sein. Derselbe Skill soll funktionieren, egal ob du Claude oder eine andere AI-Plattform nutzt. In der Praxis sind manche Skills auf Claudes spezifische Fähigkeiten optimiert; das kann im `compatibility`-Feld des Frontmatters dokumentiert werden.

### 5.2 Das Public Skills Repository

Auf GitHub unter `anthropics/skills` findest du von Anthropic erstellte Skills, die du anpassen und als Vorlage nutzen kannst. Das Repository enthält die Document-Skills (PDF, DOCX, PPTX, XLSX), verschiedene Workflow-Patterns und wird regelmässig aktualisiert.

### 5.3 Partner-Skills

Zahlreiche Partner haben eigene Skills entwickelt, die ihre MCP-Server ergänzen:

| Partner | Skill-Fokus |
|---------|------------|
| **Asana** | Projekt- und Task-Management-Workflows |
| **Atlassian** | Jira-/Confluence-Workflows |
| **Canva** | Design-Erstellung und Asset-Management |
| **Figma** | Design-to-Code Handoff |
| **Sentry** | Automated Bug Analysis und Code Review |
| **Zapier** | Workflow-Automation über Services hinweg |

Diese Partner-Skills zeigen das Zusammenspiel von MCP (Zugang zum Service) und Skills (Wissen über optimale Nutzung). Ein Sentry-Skill analysiert beispielsweise automatisch Bugs in GitHub Pull Requests anhand von Sentrys Error-Monitoring-Daten.

### 5.4 Skills entdecken und installieren

Der aktuelle Workflow: Du findest Skills auf GitHub oder in der Partner-Dokumentation, lädst den Skill-Ordner herunter (Clone oder ZIP), und installierst ihn in Claude.ai über Settings oder legst ihn in den Claude Code Skills-Ordner. Der Prozess ist bewusst einfach gehalten, wird aber weiterentwickelt.

### 5.5 Eigene Skills veröffentlichen

Wenn du einen Skill für andere bereitstellen willst:

Hoste den Skill auf GitHub in einem öffentlichen Repository. Erstelle eine README (für menschliche Besucher; sie liegt auf Repo-Ebene, nicht im Skill-Ordner selbst). Dokumentiere Beispiel-Nutzung mit Screenshots. Falls dein Skill einen MCP-Server ergänzt, verlinke ihn aus der MCP-Dokumentation heraus und erkläre den Mehrwert.

### 5.6 Wohin sich der Marketplace entwickelt

Der Marketplace ist noch jung. Aktuell ist die Distribution dezentral über GitHub. Anthropic arbeitet an besseren Discovery-Mechanismen und einfacherer Installation. Die Organisation-Level-Distribution (Admin-Deployment) ist ein erster Schritt in Richtung zentralisierter Verwaltung. Es ist absehbar, dass ein integrierter Marketplace in Claude.ai kommt, ähnlich wie App Stores für andere Plattformen.

---

## Kapitel 6: Anatomie, Planung und Patterns

### 6.1 Dateistruktur

Jeder Skill hat eine klare Ordnerstruktur:

```
dein-skill-name/
├── SKILL.md              # Pflicht: Hauptdatei mit Instruktionen
├── scripts/              # Optional: Ausführbarer Code
│   ├── process_data.py
│   └── validate.sh
├── references/           # Optional: Zusätzliche Dokumentation
│   ├── api-guide.md
│   └── examples/
└── assets/               # Optional: Templates, Fonts, Icons
    └── report-template.md
```

**Kritische Regeln:**

Die Hauptdatei muss exakt `SKILL.md` heissen. Gross/Kleinschreibung zählt. Keine Variationen wie `SKILL.MD`, `skill.md` oder `Skill.md`.

Der Ordnername muss in kebab-case sein: `notion-project-setup` ist korrekt. Leerzeichen, Underscores und Grossbuchstaben sind nicht erlaubt.

Kein `README.md` im Skill-Ordner. Alle Dokumentation gehört in `SKILL.md` oder den `references/`-Ordner. Eine README für GitHub-Besucher liegt auf Repo-Ebene, nicht im Skill-Ordner.

### 6.2 YAML Frontmatter

Das Frontmatter ist der wichtigste Teil. Darüber entscheidet Claude, ob er den Skill laden soll.

**Minimales Format:**

```yaml
---
name: dein-skill-name
description: Was er tut. Nutze ihn wenn der User [spezifische Phrasen] sagt.
---
```

**Alle Felder:**

| Feld | Pflicht | Max. Zeichen | Beschreibung |
|------|---------|-------------|--------------|
| `name` | Ja | — | kebab-case, keine Leerzeichen, keine Grossbuchstaben |
| `description` | Ja | 1024 | WAS der Skill tut + WANN er triggern soll |
| `license` | Nein | — | z.B. MIT, Apache-2.0 |
| `compatibility` | Nein | 500 | Umgebungsanforderungen |
| `metadata` | Nein | — | Eigene Key-Value-Paare (author, version, mcp-server etc.) |

**Sicherheitsregeln:** Keine XML-Tags (`<` oder `>`) im Frontmatter. Keine Skills mit "claude" oder "anthropic" im Namen (reserviert). Das Frontmatter erscheint im System Prompt; bösartiger Inhalt könnte Instruktionen injizieren.

### 6.3 Die Description richtig schreiben

Die Description ist deine Visitenkarte. Sie folgt der Formel:

```
[Was der Skill tut] + [Wann er triggern soll] + [Schlüsselfähigkeiten]
```

**Gute Beispiele:**

```yaml
# Spezifisch und handlungsorientiert
description: Analysiert Figma-Designdateien und generiert Developer-Handoff-Dokumentation.
  Nutze ihn wenn der User .fig-Dateien hochlädt, nach "Design Specs",
  "Component Documentation" oder "Design-to-Code Handoff" fragt.

# Mit klaren Trigger-Phrasen
description: Verwaltet Linear-Projekt-Workflows inklusive Sprint-Planung,
  Task-Erstellung und Status-Tracking. Nutze ihn wenn der User "Sprint",
  "Linear Tasks", "Projektplanung" erwähnt oder "Tickets erstellen" will.
```

**Schlechte Beispiele:**

```yaml
# Zu vage
description: Hilft bei Projekten.

# Keine Triggers
description: Erstellt ausgefeilte mehrseitige Dokumentationssysteme.

# Zu technisch, keine User-Triggers
description: Implementiert das Project-Entity-Modell mit hierarchischen Beziehungen.
```

**Negative Triggers** helfen gegen Overtriggering:

```yaml
description: Fortgeschrittene Datenanalyse für CSV-Dateien. Nutze ihn für
  statistisches Modelling, Regression, Clustering. NICHT verwenden für
  einfache Datenexploration (dafür data-viz Skill).
```

### 6.4 Die fünf bewährten Patterns

Diese Patterns haben sich in der Praxis bewährt. Die meisten Skills lassen sich einem dieser Muster zuordnen.

**Pattern 1: Sequential Workflow Orchestration**
Für mehrstufige Prozesse in fester Reihenfolge. Jeder Schritt hat klare Abhängigkeiten vom vorherigen. Beispiel: Kunden-Onboarding (Account erstellen → Payment Setup → Subscription erstellen → Welcome-Mail senden). Schlüsseltechniken: Explizite Reihenfolge, Validierung pro Schritt, Rollback-Instruktionen bei Fehlern.

**Pattern 2: Multi-MCP Coordination**
Für Workflows, die über mehrere Services laufen. Beispiel: Design-to-Development Handoff (Figma → Drive → Linear → Slack). Schlüsseltechniken: Klare Phasentrennung, Datenübergabe zwischen MCPs, Validierung vor dem nächsten Schritt.

**Pattern 3: Iterative Refinement**
Wenn die Ausgabequalität durch Iteration steigt. Beispiel: Report-Generierung (Entwurf → Validierung → Verfeinerung → Finalisierung). Schlüsseltechniken: Explizite Qualitätskriterien, Validierungsscripts, klare Abbruchbedingungen.

**Pattern 4: Context-aware Tool Selection**
Wenn dasselbe Ziel je nach Kontext unterschiedliche Werkzeuge erfordert. Beispiel: Datei-Speicherung (grosse Dateien → Cloud; kollaborative Docs → Notion; Code → GitHub). Schlüsseltechniken: Klarer Entscheidungsbaum, Fallback-Optionen, Transparenz gegenüber dem User.

**Pattern 5: Domain-specific Intelligence**
Wenn der Skill spezialisiertes Fachwissen über den Tool-Zugang hinaus einbringt. Beispiel: Payment Processing mit Compliance-Prüfung (Compliance-Check → Verarbeitung → Audit Trail). Schlüsseltechniken: Fachlogik in Instruktionen eingebettet, Compliance vor Aktion, lückenlose Dokumentation.

### 6.5 Instruktionen schreiben

**Dos:**

Sei spezifisch und handlungsorientiert. Statt "Validiere die Daten" schreibe "Führe `python scripts/validate.py --input {filename}` aus. Bei Fehler prüfe: fehlende Pflichtfelder, ungültige Datumsformate (nutze YYYY-MM-DD)."

Baue Error Handling ein. Beschreibe häufige Fehler und ihre Lösungen direkt in den Instruktionen.

Referenziere gebündelte Ressourcen klar: "Konsultiere `references/api-patterns.md` für Rate-Limiting und Paginierung."

Nutze Progressive Disclosure. Halte die SKILL.md unter 5000 Wörtern. Detaillierte Referenzen gehören in `references/`.

**Don'ts:**

Vermeide vage Anweisungen wie "Stelle sicher, dass alles korrekt ist." Vermeide zu lange SKILL.md-Dateien; verschiebe Details in `references/`. Vermeide mehrdeutige Sprache. Wenn etwas kritisch ist, markiere es als `CRITICAL:` oder `WICHTIG:`.

Für kritische Validierungen: Erwäge, ein Script zu bündeln, das die Prüfung programmatisch durchführt. Code ist deterministisch; Sprachinterpretation nicht.

---

## Kapitel 7: Testen, Iterieren und Verteilen

### 7.1 Drei Test-Ebenen

Je nach Sichtbarkeit und Wichtigkeit deines Skills wählst du die passende Test-Tiefe:

**Manuelles Testen in Claude.ai:** Queries direkt eingeben und das Verhalten beobachten. Schnelle Iteration, kein Setup nötig. Ideal für den Start und für persönliche Skills.

**Scripted Testing in Claude Code:** Testfälle automatisieren für wiederholbare Validierung. Sinnvoll, wenn du den Skill weiterentwickelst und Regressionen vermeiden willst.

**Programmatisches Testen via Skills API:** Evaluations-Suites, die systematisch gegen definierte Test-Sets laufen. Für Skills, die an tausende Enterprise-Nutzer gehen.

### 7.2 Was du testen solltest

**Triggering-Tests:** Wird der Skill bei relevanten Anfragen geladen?

```
Soll triggern:
- "Hilf mir ein neues Projekt in ProjectHub aufzusetzen"
- "Ich muss ein ProjectHub-Projekt erstellen"
- "Initialisiere ein ProjectHub-Projekt für Q4-Planung"

Soll NICHT triggern:
- "Wie wird das Wetter morgen?"
- "Hilf mir Python-Code zu schreiben"
- "Erstelle eine Tabelle"
```

**Funktionale Tests:** Produziert der Skill korrekte Outputs? API-Calls erfolgreich? Error Handling funktioniert? Edge Cases abgedeckt?

**Performance-Vergleich:** Beweise, dass der Skill die Ergebnisse verbessert. Vergleiche dieselbe Aufgabe mit und ohne Skill: Anzahl Nachrichten, fehlgeschlagene API-Calls, Token-Verbrauch.

### 7.3 Der skill-creator Skill

Der `skill-creator`-Skill ist dein bester Freund beim Erstellen und Verbessern von Skills. Er kann Skills aus natürlichsprachigen Beschreibungen generieren, korrekt formatiertes YAML-Frontmatter erzeugen, Trigger-Phrasen vorschlagen, bestehende Skills reviewen und Verbesserungen empfehlen, und Test-Cases basierend auf dem Skill-Zweck vorschlagen.

Nutze ihn so: "Hilf mir mit dem skill-creator einen Skill für [dein Use Case] zu bauen."

Der Tipp aus der Praxis: Iteriere zuerst an einer einzigen anspruchsvollen Aufgabe, bis Claude erfolgreich ist. Extrahiere dann den funktionierenden Ansatz in einen Skill. Das nutzt Claudes In-Context-Learning und liefert schnelleres Feedback als breites Testen.

### 7.4 Iteration basierend auf Feedback

Skills sind lebendige Dokumente. Plane ein, sie weiterzuentwickeln.

**Undertriggering erkennen:** Der Skill lädt nicht, wenn er sollte. Nutzer aktivieren ihn manuell. Support-Fragen wie "Wann soll ich den Skill nutzen?" häufen sich. Die Lösung: Mehr Detail und Nuancen in der Description. Keywords ergänzen, besonders technische Fachbegriffe.

**Overtriggering erkennen:** Der Skill lädt bei irrelevanten Anfragen. Nutzer deaktivieren ihn genervt. Verwirrung über den Zweck. Die Lösung: Negative Triggers hinzufügen, spezifischer werden, Scope klarer abgrenzen.

**Execution-Probleme:** Inkonsistente Ergebnisse, API-Fehler, häufige Korrekturen durch Nutzer. Die Lösung: Instruktionen verbessern, Error Handling ergänzen, Validierungsscripts hinzufügen.

### 7.5 Distribution

**GitHub (empfohlen für den Start):** Öffentliches Repo, klare README auf Repo-Ebene, Beispiel-Nutzung mit Screenshots. Falls dein Skill einen MCP-Server ergänzt: Verlinke aus der MCP-Dokumentation und erkläre den Mehrwert.

**Claude.ai Upload:** Skill-Ordner als ZIP hochladen über Settings > Capabilities > Skills.

**Organisation-Level:** Admins deployen Skills workspace-weit mit automatischen Updates.

**API:** Für programmatische Use Cases über den `/v1/skills`-Endpoint. Ideal für Production-Deployments, automatisierte Pipelines und Agent-Systeme.

Beim Positionieren deines Skills: Fokussiere auf Ergebnisse, nicht Features. Statt "Ein Ordner mit YAML-Frontmatter und Markdown-Instruktionen" schreibe "Ermöglicht Teams, komplette Projekt-Workspaces in Sekunden aufzusetzen statt 30 Minuten manuell."

---

# Claude Agent Skills Cheatsheet

## Seite 1: Dateistruktur & YAML Frontmatter

### Ordnerstruktur

```
dein-skill-name/              # kebab-case, keine Leerzeichen
├── SKILL.md                  # PFLICHT, exakte Schreibweise
├── scripts/                  # Optional: Python, Bash etc.
├── references/               # Optional: Zusätzliche Docs
└── assets/                   # Optional: Templates, Fonts
```

### YAML Frontmatter

```yaml
---
name: dein-skill-name                    # Pflicht, kebab-case
description: Was er tut. Nutze ihn       # Pflicht, max 1024 Zeichen
  wenn [Trigger-Phrasen].
license: MIT                             # Optional
compatibility: Benötigt Python 3.x       # Optional, max 500 Zeichen
metadata:                                # Optional
  author: Dein Name
  version: 1.0.0
  mcp-server: server-name
---
```

### Namensregeln

| Regel | Korrekt | Falsch |
|-------|---------|--------|
| Ordnername | `notion-project-setup` | `Notion Project Setup`, `notion_setup` |
| SKILL.md | `SKILL.md` | `SKILL.MD`, `skill.md`, `Skill.md` |
| Kein README.md | Nur SKILL.md im Ordner | README.md im Skill-Ordner |
| Kein "claude"/"anthropic" | `my-assistant-skill` | `claude-helper-skill` |

### Sicherheit

Kein `<` oder `>` im Frontmatter (XML-Injection-Risiko). Keine Skills mit "claude" oder "anthropic" im Namen (reserviert).

---

## Seite 2: Skills vs. CLAUDE.md + Speicherorte

### Vergleich

| | Skills | CLAUDE.md | System Prompt |
|-|--------|-----------|---------------|
| **Laden** | Trigger-basiert | Immer komplett | Immer |
| **Scope** | Global oder Projekt | Projekt | Plattform |
| **Portabel** | Ja | Nein | Nein |
| **Modular** | Ja (Ordner) | Nein (1 Datei) | Nein |
| **Token-effizient** | Ja (3 Ebenen) | Nein | Nein |

### Entscheidungsbaum

```
Wiederkehrender Workflow?
├── Ja → Immer relevant im Projekt?
│   ├── Ja → CLAUDE.md
│   └── Nein → Skill
└── Nein → Einmalig im Chat erklären
```

### Speicherorte

| Umgebung | Pfad / Methode | Scope |
|----------|---------------|-------|
| Claude.ai (Upload) | Settings > Capabilities > Skills | User |
| Claude.ai (System) | `/mnt/skills/public/` | Alle |
| Claude.ai (Beispiele) | `/mnt/skills/examples/` | Alle |
| Claude.ai (Eigene) | `/mnt/skills/user/` | User |
| Claude Code (Global) | `~/.claude/skills/` | User |
| Claude Code (Projekt) | `.claude/skills/` | Projekt |
| API | `/v1/skills` Endpoint | Programmatisch |
| Organisation | Admin-Deployment | Workspace |

---

## Seite 3: Standard-Skills & Marketplace

### Anthropic Kern-Skills

| Skill | Trigger-Phrasen |
|-------|----------------|
| `docx` | "Word doc", ".docx", "Report erstellen" |
| `pptx` | "Präsentation", "Slides", "Deck" |
| `xlsx` | "Spreadsheet", ".xlsx", "Tabelle erstellen" |
| `pdf` | "PDF lesen", "PDF erstellen", "PDFs mergen" |
| `frontend-design` | "Webseite", "Landing Page", "Dashboard" |
| `skill-creator` | "Skill erstellen", "Skill verbessern" |

### Beispiel-Skills

| Skill | Einsatzgebiet |
|-------|--------------|
| `doc-coauthoring` | Strukturiertes Co-Authoring |
| `web-artifacts-builder` | Komplexe React-Artifacts |
| `canvas-design` | Visuelle Designs, Poster |
| `theme-factory` | Styling und Themes |
| `mcp-builder` | MCP-Server erstellen |

### Partner-Skills

| Partner | Fokus |
|---------|-------|
| Asana | Task-/Projekt-Management |
| Atlassian | Jira-/Confluence-Workflows |
| Canva | Design-Erstellung |
| Figma | Design-to-Code |
| Sentry | Bug Analysis |
| Zapier | Cross-Service Automation |

### Skills installieren

```bash
# Von GitHub
git clone https://github.com/company/skills

# Dann: ZIP erstellen und hochladen
# Settings > Capabilities > Skills > Upload
```

---

## Seite 4: Description, Triggers & Patterns

### Description-Formel

```
[WAS der Skill tut] + [WANN er triggern soll] + [Schlüsselfähigkeiten]
```

### Pattern-Übersicht

| Pattern | Wann verwenden | Schlüsseltechnik |
|---------|----------------|------------------|
| Sequential Workflow | Feste Reihenfolge, Abhängigkeiten | Schritt-Validierung, Rollback |
| Multi-MCP Coordination | Mehrere Services beteiligt | Phasentrennung, Datenübergabe |
| Iterative Refinement | Qualität durch Iteration | Quality Gates, Validierungsscripts |
| Context-aware Selection | Ziel gleich, Werkzeug variiert | Entscheidungsbaum, Fallbacks |
| Domain-specific Intelligence | Fachwissen nötig | Fachlogik eingebettet, Audit Trail |

### SKILL.md Template (Copy-Paste)

```markdown
---
name: dein-skill-name
description: Beschreibt [was]. Nutze ihn wenn der User
  [Trigger-Phrase 1], [Trigger-Phrase 2] oder [Trigger-Phrase 3]
  sagt. NICHT verwenden für [Abgrenzung].
metadata:
  author: Dein Name
  version: 1.0.0
---

# Skill-Name

## Instruktionen

### Schritt 1: [Erster Schritt]
Klare Erklärung was passiert.

Beispiel:
\```bash
python scripts/fetch_data.py --project-id PROJECT_ID
\```

Erwartetes Ergebnis: [Beschreibe Erfolg]

### Schritt 2: [Zweiter Schritt]
[...]

## Beispiele

**Beispiel 1: [Häufiges Szenario]**
User sagt: "[Typische Anfrage]"
Aktionen:
1. [Aktion 1]
2. [Aktion 2]
Ergebnis: [Was der User bekommt]

## Troubleshooting

**Fehler: [Häufiger Fehler]**
Ursache: [Warum]
Lösung: [Wie beheben]
```

---

## Seite 5: Testing & Troubleshooting

### Test-Checkliste

**Vor dem Start:**
- [ ] 2-3 konkrete Use Cases identifiziert
- [ ] Benötigte Tools identifiziert (built-in oder MCP)
- [ ] Ordnerstruktur geplant

**Während der Entwicklung:**
- [ ] Ordner in kebab-case benannt
- [ ] SKILL.md existiert (exakte Schreibweise)
- [ ] YAML Frontmatter mit `---` Delimitern
- [ ] `name`: kebab-case, keine Leerzeichen
- [ ] `description`: WAS + WANN enthalten
- [ ] Keine XML-Tags (`<` `>`)
- [ ] Instruktionen klar und handlungsorientiert
- [ ] Error Handling eingebaut
- [ ] Beispiele vorhanden

**Vor dem Upload:**
- [ ] Triggering auf offensichtlichen Tasks getestet
- [ ] Triggering auf umformulierte Anfragen getestet
- [ ] Kein Triggering auf irrelevante Topics verifiziert
- [ ] Funktionale Tests bestanden

**Nach dem Upload:**
- [ ] In echten Gesprächen getestet
- [ ] Under/Overtriggering beobachtet
- [ ] Feedback gesammelt und eingearbeitet

### Häufige Fehler

| Problem | Ursache | Lösung |
|---------|---------|--------|
| "Could not find SKILL.md" | Falscher Dateiname | Exakt `SKILL.md` (case-sensitive) |
| "Invalid frontmatter" | YAML-Fehler | `---` Delimiter prüfen, Quotes schliessen |
| "Invalid skill name" | Leerzeichen/Grossbuchstaben | kebab-case verwenden |
| Skill triggert nie | Description zu vage | Trigger-Phrasen ergänzen |
| Skill triggert zu oft | Description zu breit | Negative Triggers, Scope eingrenzen |
| Instruktionen ignoriert | Zu lang/vage | Kürzen, `CRITICAL:` markieren, Scripts nutzen |
| Langsame Responses | Zu viel Kontext | SKILL.md unter 5000 Wörter, Progressive Disclosure |

### Debugging

```
# Frage Claude direkt:
"Wann würdest du den [skill-name] Skill verwenden?"
→ Claude zitiert die Description zurück
→ Passe an, was fehlt

# Skill-Ordner prüfen:
ls -la dein-skill-name/
→ Muss SKILL.md zeigen (exakte Schreibweise)
```

### Nützliche Ressourcen

| Ressource | Link |
|-----------|------|
| Best Practices Guide | docs.anthropic.com |
| Skills Documentation | docs.anthropic.com |
| Public Skills Repo | github.com/anthropics/skills |
| MCP Documentation | modelcontextprotocol.io |
| Community | Claude Developers Discord |
| Bug Reports | github.com/anthropics/skills/issues |

---

*Erstellt für den praktischen Einsatz | Version 1.0 | März 2026*

*© 2026 Ralph Hutter | Lizenziert unter CC BY-SA 4.0*
