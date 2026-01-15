# Claude Code Einführung & Cheatsheet

> Eine praxisorientierte Anleitung für agentic coding mit Claude Code

---

## Kapitel 1: Was ist Claude Code?

### 1.1 Überblick

Claude Code ist ein agentic coding tool von Anthropic, das direkt im Terminal läuft. Es ermöglicht Entwicklern, Coding-Aufgaben an Claude zu delegieren – von einfachen Edits bis hin zu komplexen, mehrstufigen Projekten. Claude Code versteht den Projektkontext, kann Dateien lesen und schreiben, Befehle ausführen und autonom iterieren.

### 1.2 Kernkonzept: Agentic Coding

Im Unterschied zu klassischen Code-Assistenten arbeitet Claude Code **autonom**:

| Klassischer Assistent | Claude Code (Agentic) |
|----------------------|----------------------|
| Beantwortet Fragen | Führt Aufgaben aus |
| Zeigt Code-Snippets | Schreibt und editiert Dateien |
| Erklärt Lösungen | Implementiert Lösungen |
| Wartet auf jeden Schritt | Plant und iteriert selbstständig |
| Isolierte Antworten | Versteht Projektkontext |

### 1.3 Hauptfähigkeiten

Claude Code kann:

- **Codebase verstehen**: Analysiert Projektstruktur, Dependencies, Patterns
- **Code schreiben**: Erstellt neue Dateien, Funktionen, Module
- **Code editieren**: Refactoring, Bug-Fixes, Feature-Erweiterungen
- **Commands ausführen**: Git, npm, Tests, Build-Prozesse
- **Debugging**: Fehler identifizieren, analysieren und beheben
- **Dokumentation**: README, Docstrings, API-Docs generieren

### 1.4 Systemanforderungen

```
Betriebssystem: macOS 10.15+, Ubuntu 20.04+, Windows (via WSL)
Node.js: Version 18+
RAM: Mindestens 4 GB empfohlen
Terminal: Bash, Zsh, oder kompatible Shell
```

### 1.5 Installation

```bash
# Installation via npm (empfohlen)
npm install -g @anthropic-ai/claude-code

# Oder via Homebrew (macOS)
brew install claude-code

# API-Key konfigurieren
export ANTHROPIC_API_KEY="sk-ant-..."

# Oder interaktiv einrichten
claude config set api_key
```

### 1.6 Kostenmodell

Claude Code nutzt die Anthropic API und wird nach Token-Verbrauch abgerechnet:

| Modell | Input | Output |
|--------|-------|--------|
| Claude Sonnet 4 | $3 / 1M Tokens | $15 / 1M Tokens |
| Claude Opus 4 | $15 / 1M Tokens | $75 / 1M Tokens |

Typische Coding-Session: $0.50 - $5.00 je nach Komplexität

---

## Kapitel 2: Grundlagen der Nutzung

### 2.1 Erste Schritte

```bash
# Claude Code im aktuellen Verzeichnis starten
claude

# Mit spezifischer Aufgabe starten
claude "Erkläre mir die Struktur dieses Projekts"

# In einem bestimmten Verzeichnis starten
claude --cwd /path/to/project
```

### 2.2 Interaktionsmodi

Claude Code bietet verschiedene Interaktionsmodi:

**Interactive Mode (Standard):**
```bash
claude
> Analysiere die Codebase und finde potenzielle Bugs
```

**One-Shot Mode:**
```bash
claude "Füge TypeScript-Typen zur utils.js hinzu"
```

**Pipe Mode:**
```bash
cat error.log | claude "Analysiere diesen Error und schlage Fixes vor"
```

**Headless Mode (für Automation):**
```bash
claude --headless "Führe alle Tests aus und fixe Fehler"
```

### 2.3 Die Benutzeroberfläche

```
┌─────────────────────────────────────────────────────────────┐
│ Claude Code v1.x                                            │
├─────────────────────────────────────────────────────────────┤
│                                                             │
│ Claude: Ich habe das Projekt analysiert. Es ist eine        │
│         React-App mit TypeScript. Wie kann ich helfen?      │
│                                                             │
│ ─────────────────────────────────────────────────────────── │
│                                                             │
│ > [Deine Eingabe hier]                                      │
│                                                             │
├─────────────────────────────────────────────────────────────┤
│ [Esc] Menu  [Tab] Autocomplete  [↑↓] History  [Ctrl+C] Stop │
└─────────────────────────────────────────────────────────────┘
```

### 2.4 Kern-Workflows

**Code Review:**
```
> Überprüfe die letzten 5 Commits auf Code-Qualität und Sicherheit
```

**Feature Implementation:**
```
> Implementiere eine User-Authentifizierung mit JWT
```

**Bug Fixing:**
```
> Der Test in user.test.js schlägt fehl. Finde und behebe den Bug.
```

**Refactoring:**
```
> Refaktoriere die API-Calls in einen zentralen Service
```

### 2.5 Berechtigungen und Sicherheit

Claude Code fragt vor kritischen Aktionen um Erlaubnis:

```
Claude möchte folgende Aktion ausführen:
  → Datei schreiben: src/components/Auth.tsx
  
[y] Erlauben  [n] Ablehnen  [a] Immer erlauben  [v] Vorschau
```

**Permission-Level konfigurieren:**
```bash
# Strikt: Fragt bei jeder Aktion
claude --permission-mode strict

# Standard: Fragt bei kritischen Aktionen
claude --permission-mode normal

# Permissiv: Minimale Nachfragen (für erfahrene User)
claude --permission-mode permissive
```

### 2.6 Kontext-Management

Claude Code analysiert automatisch:
- Projektstruktur und Dateien
- package.json / requirements.txt / Cargo.toml etc.
- Git-History und aktuelle Changes
- Vorhandene Tests und Dokumentation

**Kontext manuell erweitern:**
```
> @file:src/config.ts Erkläre diese Konfiguration
> @folder:src/components Liste alle Komponenten auf
> @git:HEAD~5 Was wurde in den letzten 5 Commits geändert?
```

---

## Kapitel 3: Fortgeschrittene Techniken

### 3.1 Projekt-Konfiguration

Erstelle eine `.claude/config.yaml` im Projektroot:

```yaml
# .claude/config.yaml
project:
  name: "My Awesome App"
  description: "Eine React-App mit Node.js Backend"
  
context:
  include:
    - "src/**/*.ts"
    - "src/**/*.tsx"
    - "package.json"
    - "README.md"
  exclude:
    - "node_modules/**"
    - "dist/**"
    - "*.log"

preferences:
  language: "de"
  code_style: "prettier"
  test_framework: "jest"
  
custom_instructions: |
  - Verwende TypeScript strict mode
  - Schreibe Tests für alle neuen Funktionen
  - Folge dem bestehenden Code-Style
  - Kommentare auf Englisch
```

### 3.2 Slash-Commands

| Command | Funktion |
|---------|----------|
| `/help` | Zeigt verfügbare Befehle |
| `/clear` | Löscht Konversations-History |
| `/context` | Zeigt aktuellen Kontext |
| `/add <file>` | Fügt Datei zum Kontext hinzu |
| `/remove <file>` | Entfernt Datei aus Kontext |
| `/run <cmd>` | Führt Shell-Befehl aus |
| `/git <cmd>` | Git-Operationen |
| `/test` | Führt Tests aus |
| `/build` | Startet Build-Prozess |
| `/diff` | Zeigt ausstehende Änderungen |
| `/commit` | Erstellt Commit mit Message |
| `/undo` | Macht letzte Änderung rückgängig |
| `/model <name>` | Wechselt das Modell |
| `/cost` | Zeigt bisherige Kosten |
| `/exit` | Beendet Claude Code |

### 3.3 Git-Integration

Claude Code arbeitet nahtlos mit Git:

```
# Automatische Commits
> Implementiere Feature X und committe mit sinnvoller Message

# Branch-Management
> Erstelle einen Feature-Branch für die neue Auth-Logik

# Merge-Konflikte lösen
> Löse die Merge-Konflikte in favor der incoming changes

# History analysieren
> Wann wurde die Funktion calculateTotal zuletzt geändert und warum?
```

### 3.4 Test-Driven Development

```
# Tests zuerst schreiben lassen
> Schreibe Tests für eine Funktion die E-Mail-Adressen validiert

# Implementation nach Tests
> Implementiere die Funktion so, dass alle Tests bestehen

# Test Coverage verbessern
> Erhöhe die Test-Coverage in src/utils auf mindestens 80%
```

### 3.5 Multi-File Operations

Claude Code kann über mehrere Dateien hinweg arbeiten:

```
> Refaktoriere alle API-Calls in der App zu einem 
  zentralen ApiService mit Interceptors für Auth und Error-Handling.
  Aktualisiere alle betroffenen Komponenten.
```

Claude wird:
1. Bestehende API-Calls analysieren
2. Neuen ApiService erstellen
3. Alle Komponenten aktualisieren
4. Imports anpassen
5. Tests aktualisieren

### 3.6 CI/CD Integration

```bash
# In GitHub Actions
- name: Code Review mit Claude
  run: |
    claude --headless "Review die Änderungen in diesem PR 
    und erstelle einen Report" > review.md

# In Pre-Commit Hook
#!/bin/bash
claude --headless "Prüfe staged files auf offensichtliche Fehler"
```

### 3.7 Custom Tools und MCP

Claude Code unterstützt MCP (Model Context Protocol) für erweiterte Integrationen:

```yaml
# .claude/mcp.yaml
servers:
  - name: database
    command: "mcp-server-postgres"
    args: ["--connection-string", "$DATABASE_URL"]
    
  - name: jira
    command: "mcp-server-jira"
    args: ["--api-token", "$JIRA_TOKEN"]
```

Mit konfigurierten MCP-Servern:
```
> Erstelle ein Jira-Ticket für diesen Bug und verlinke es im Code
> Zeige mir das Schema der users-Tabelle
```

---

# Claude Code Cheatsheet

## Seite 1: Befehle & Navigation

### Startup-Optionen

```bash
# Basis-Start
claude                          # Interaktiver Modus
claude "Aufgabe"               # One-Shot Modus
claude --headless "Aufgabe"    # Ohne Interaktion

# Konfiguration
claude --cwd <path>            # Arbeitsverzeichnis setzen
claude --model sonnet          # Modell wählen (sonnet/opus)
claude --permission-mode <x>   # strict/normal/permissive
claude --max-tokens <n>        # Token-Limit setzen
claude --verbose               # Debug-Output

# Projekt-spezifisch
claude --config <file>         # Custom Config laden
claude --no-git                # Git-Integration deaktivieren
claude --include "*.ts"        # Dateien inkludieren
claude --exclude "test/**"     # Dateien exkludieren
```

### Slash-Commands

```
/help                  Hilfe anzeigen
/clear                 History löschen
/context               Aktuellen Kontext zeigen
/cost                  Token-Verbrauch und Kosten

/add <file|folder>     Zum Kontext hinzufügen
/remove <file>         Aus Kontext entfernen
/tree                  Projektstruktur anzeigen

/run <command>         Shell-Befehl ausführen
/git <command>         Git-Operation
/test [pattern]        Tests ausführen
/build                 Build starten
/lint                  Linter ausführen

/diff                  Änderungen anzeigen
/undo                  Letzte Änderung rückgängig
/commit [message]      Änderungen committen
/push                  Änderungen pushen

/model <name>          Modell wechseln
/config <key> <value>  Einstellung ändern
/exit                  Beenden
```

### Keyboard Shortcuts

| Shortcut | Funktion |
|----------|----------|
| `Enter` | Nachricht senden |
| `Shift+Enter` | Mehrzeilige Eingabe |
| `Tab` | Autocomplete |
| `↑` / `↓` | Command History |
| `Ctrl+C` | Aktuelle Aktion stoppen |
| `Ctrl+D` | Beenden |
| `Ctrl+L` | Bildschirm löschen |
| `Ctrl+R` | History durchsuchen |
| `Esc` | Menü öffnen |

### Kontext-Referenzen

```
@file:path/to/file.ts      Spezifische Datei referenzieren
@folder:src/components     Ganzen Ordner einbeziehen
@git:HEAD~3                Letzte 3 Commits
@git:branch-name           Spezifischen Branch
@url:https://...           Externe Dokumentation
@clipboard                 Inhalt der Zwischenablage
```

### Permission-Responses

```
[y] Yes        Einmalig erlauben
[n] No         Ablehnen
[a] Always     Für Session immer erlauben
[v] View       Vorschau der Änderung
[e] Edit       Änderung manuell anpassen
[s] Skip       Überspringen, fortfahren
```

---

## Seite 2: Workflows & Patterns

### Standard-Workflows

**Neues Feature implementieren:**
```
> Ich brauche eine User-Registration mit:
  - E-Mail-Validierung
  - Passwort-Stärke-Check
  - Bestätigungs-E-Mail
  Nutze das bestehende User-Model und Mail-Service.
```

**Bug fixen:**
```
> Der folgende Test schlägt fehl:
  npm test -- --grep "calculateDiscount"
  
  Analysiere den Fehler und behebe ihn.
```

**Code Review:**
```
> Führe ein Code Review durch für:
  @git:HEAD~1
  
  Prüfe auf:
  - Security Issues
  - Performance Probleme
  - Code Style Violations
  - Fehlende Tests
```

**Refactoring:**
```
> Refaktoriere src/legacy/ zu modernem TypeScript:
  - Konvertiere alle .js zu .ts
  - Füge Typen hinzu
  - Ersetze var durch const/let
  - Nutze async/await statt Callbacks
```

### Prompt-Patterns

**Kontext-First:**
```
> Das Projekt nutzt:
  - React 18 mit TypeScript
  - Redux Toolkit für State
  - RTK Query für API-Calls
  - Jest für Tests
  
  Implementiere [Aufgabe] entsprechend dieser Architektur.
```

**Constraint-Driven:**
```
> Implementiere [Feature] mit folgenden Einschränkungen:
  - Keine neuen Dependencies
  - Maximale Funktionslänge: 30 Zeilen
  - 100% Test-Coverage
  - Dokumentation für alle public APIs
```

**Iterative Refinement:**
```
> Erstelle einen ersten Entwurf für [Feature]
[Claude erstellt Code]
> Gut, aber verbessere die Error-Handling-Logik
[Claude verfeinert]
> Füge jetzt Logging hinzu
```

### Projekt-Konfiguration Template

```yaml
# .claude/config.yaml

project:
  name: "Projektname"
  description: "Kurze Beschreibung"
  tech_stack:
    - TypeScript
    - React
    - Node.js

context:
  include:
    - "src/**/*"
    - "package.json"
    - "tsconfig.json"
  exclude:
    - "node_modules/**"
    - "dist/**"
    - "coverage/**"
    - "*.log"
    - ".env*"

preferences:
  model: "sonnet"
  language: "de"
  permission_mode: "normal"
  auto_commit: false
  
coding_standards:
  style_guide: "prettier + eslint"
  test_framework: "jest"
  doc_style: "jsdoc"
  
custom_instructions: |
  - Befolge bestehenden Code-Style
  - Schreibe Tests für neue Funktionen
  - Verwende aussagekräftige Variablennamen
  - Dokumentiere komplexe Logik
  - Keine console.log in Production-Code
```

### Git-Workflow

```
# Feature-Branch erstellen und implementieren
> Erstelle Branch feature/user-auth, implementiere 
  die Authentifizierung und committe mit sinnvollen Messages

# Merge Request vorbereiten
> Bereite diesen Branch für einen Merge Request vor:
  - Squash kleine Commits
  - Aktualisiere CHANGELOG
  - Stelle sicher, dass alle Tests bestehen

# Konflikt-Resolution
> Merge main in diesen Branch und löse Konflikte.
  Bevorzuge bei Konflikten die main-Version für Config-Dateien.
```

---

## Seite 3: Best Practices & Troubleshooting

### Do's ✓

| Praxis | Beispiel |
|--------|----------|
| Klare Aufgaben | "Implementiere X mit Y" statt "Mach was mit dem Code" |
| Kontext geben | "Das ist eine REST-API mit Express und MongoDB..." |
| Iterieren | "Gut, jetzt verbessere den Teil mit..." |
| Tests fordern | "...und schreibe Tests dafür" |
| Einschränkungen nennen | "Ohne neue Dependencies" |
| Review-Loops | "Prüfe nochmal auf Edge-Cases" |

### Don'ts ✗

| Vermeiden | Stattdessen |
|-----------|-------------|
| Vage Aufgaben | Spezifische, messbare Ziele |
| Zu grosse Schritte | In Teilaufgaben zerlegen |
| Blind vertrauen | Änderungen reviewen |
| Kontext vergessen | Stack und Constraints nennen |
| Ohne Tests | Tests immer mitfordern |

### Troubleshooting

**Problem: Claude versteht Projekt nicht**
```
→ /context prüfen
→ /add wichtige Dateien manuell
→ Projekt-Beschreibung in config.yaml
→ Stack explizit nennen im Prompt
```

**Problem: Änderungen brechen Code**
```
→ /undo für Rollback
→ Kleinere Schritte fordern
→ "Führe nach jeder Änderung Tests aus"
→ Permission-Mode auf strict
```

**Problem: Zu hohe Kosten**
```
→ Sonnet statt Opus verwenden
→ Kontext eingrenzen (--include/--exclude)
→ Spezifischere Aufgaben
→ /cost regelmässig prüfen
```

**Problem: Langsame Responses**
```
→ Kontext reduzieren
→ Aufgaben kleiner halten
→ --max-tokens begrenzen
→ Netzwerk-Verbindung prüfen
```

**Problem: Datei-Konflikte**
```
→ /diff vor Änderungen prüfen
→ Git-Status sauber halten
→ "Zeige mir zuerst was du ändern willst"
→ Branches nutzen
```

### Sicherheits-Checkliste

```
□ API-Key sicher gespeichert (nicht im Code)
□ .env in .gitignore
□ Sensitive Dateien in exclude-Liste
□ Permission-Mode angemessen
□ Änderungen vor Commit reviewen
□ Keine Secrets in Prompts
□ Regelmässige Dependency-Updates
```

### Kosten-Optimierung

```
# Modell-Wahl
- Sonnet für Standard-Tasks (~80% günstiger als Opus)
- Opus nur für sehr komplexe Aufgaben

# Kontext-Management
- Nur relevante Dateien inkludieren
- Grosse Dateien (Logs, Data) excludieren
- /context regelmässig prüfen

# Workflow
- Spezifische, fokussierte Aufgaben
- Keine unnötigen Wiederholungen
- Batch ähnliche Aufgaben
```

### Nützliche Aliases

```bash
# In .bashrc / .zshrc
alias cc="claude"
alias ccr="claude 'Führe Code Review durch für staged changes'"
alias cct="claude 'Führe Tests aus und fixe Fehler'"
alias ccb="claude 'Analysiere und behebe den letzten Build-Fehler'"
alias ccd="claude 'Dokumentiere die undokumentierten Funktionen'"
```

### Integration mit IDE

```bash
# VS Code Task
{
  "label": "Claude Code Review",
  "type": "shell",
  "command": "claude --headless 'Review ${file}' > review.md"
}

# Vim/Neovim Command
:!claude "Erkläre diese Funktion: $(cat %)"
```

### Ressourcen

| Ressource | URL |
|-----------|-----|
| Dokumentation | docs.anthropic.com/claude-code |
| GitHub | github.com/anthropics/claude-code |
| API Reference | docs.anthropic.com/api |
| Community | discord.gg/anthropic |
| Status | status.anthropic.com |

---

*Erstellt für den praktischen Einsatz | Version 1.0 | Januar 2025*
