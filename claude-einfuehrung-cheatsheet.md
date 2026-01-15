# Claude Einführung & Cheatsheet

> Eine praxisorientierte Anleitung für die effektive Nutzung von Claude

---

## Kapitel 1: Was ist Claude?

### 1.1 Überblick

Claude ist ein KI-Assistent von Anthropic, entwickelt mit einem besonderen Fokus auf Sicherheit, Ehrlichkeit und Nützlichkeit. Im Gegensatz zu anderen Large Language Models (LLMs) wurde Claude mit der "Constitutional AI"-Methode trainiert, die auf definierten Prinzipien basiert.

### 1.2 Die Claude-Modellfamilie

Die aktuelle Generation umfasst drei Modelle mit unterschiedlichen Stärken:

| Modell | Stärken | Typische Anwendungen |
|--------|---------|---------------------|
| **Claude Opus 4.5** | Höchste Intelligenz, komplexes Reasoning | Strategische Analysen, Forschung, anspruchsvolle Aufgaben |
| **Claude Sonnet 4.5** | Ausgewogenes Verhältnis von Leistung und Geschwindigkeit | Alltägliche Aufgaben, Content-Erstellung, Coding |
| **Claude Haiku 4.5** | Schnell und kosteneffizient | Einfache Anfragen, hohe Volumen, Echtzeit-Anwendungen |

### 1.3 Zugangsoptionen

Claude ist über verschiedene Kanäle verfügbar:

- **claude.ai**: Web-Interface für direkte Interaktion
- **Claude App**: Mobile und Desktop-Anwendungen
- **API**: Programmatischer Zugang für Entwickler und Integrationen
- **Claude Code**: Command-Line-Tool für Entwickler (agentic coding)
- **Claude for Chrome**: Browser-Agent (Beta)
- **Claude for Excel**: Spreadsheet-Agent (Beta)

### 1.4 Kernprinzipien

Claude folgt drei fundamentalen Prinzipien:

1. **Helpful** – Claude strebt danach, genuinen Mehrwert zu liefern
2. **Harmless** – Vermeidung von schädlichen Outputs
3. **Honest** – Transparenz über Fähigkeiten und Limitationen

---

## Kapitel 2: Grundlagen der Nutzung

### 2.1 Der richtige Prompt

Die Qualität der Antwort hängt massgeblich von der Qualität des Prompts ab. Ein guter Prompt enthält:

**Kontext**: Was ist der Hintergrund? Wer ist die Zielgruppe?

**Aufgabe**: Was soll Claude konkret tun?

**Format**: Wie soll das Ergebnis aussehen?

**Einschränkungen**: Was soll vermieden werden?

### 2.2 Beispiel für einen strukturierten Prompt

```
Du bist ein erfahrener Business-Analyst.

KONTEXT:
Ich arbeite an einer Marktanalyse für ein Fintech-Startup im Schweizer Markt.

AUFGABE:
Erstelle eine SWOT-Analyse für den Eintritt in den B2B-Zahlungsverkehr.

FORMAT:
- Strukturierte Tabelle
- Je 3-5 Punkte pro Kategorie
- Priorisiert nach Relevanz

EINSCHRÄNKUNGEN:
- Fokus auf Schweizer Markt
- Keine allgemeinen Aussagen
```

### 2.3 Conversation Management

Claude merkt sich den Kontext innerhalb einer Konversation. Nutze dies strategisch:

- **Iteratives Arbeiten**: Verfeinere Ergebnisse schrittweise
- **Referenzierung**: Beziehe dich auf frühere Antworten ("Basierend auf der vorherigen Analyse...")
- **Rollenbeibehaltung**: Claude behält zugewiesene Rollen während der gesamten Konversation

### 2.4 Integrierte Funktionen

Claude bietet verschiedene eingebaute Funktionen:

| Funktion | Beschreibung | Aktivierung |
|----------|--------------|-------------|
| **Web Search** | Aktuelle Informationen aus dem Internet | Automatisch bei aktuellen Themen |
| **Deep Research** | Umfassende Recherche mit mehreren Quellen | Auf Anfrage |
| **Artifacts** | Interaktive Komponenten (Code, Visualisierungen) | Automatisch bei Code/Visualisierungen |
| **File Creation** | Erstellung von Dokumenten (DOCX, PPTX, XLSX) | Auf Anfrage |
| **Memory** | Erinnerung an frühere Gespräche | Einstellbar |
| **Past Chats** | Suche in früheren Konversationen | Auf Anfrage |

### 2.5 Dateien und Uploads

Claude kann verschiedene Dateitypen verarbeiten:

- **Dokumente**: PDF, DOCX, TXT, MD, HTML
- **Bilder**: PNG, JPG, GIF, WEBP
- **Daten**: CSV, JSON, XML
- **Code**: Praktisch alle Programmiersprachen

---

## Kapitel 3: Fortgeschrittene Techniken

### 3.1 Chain-of-Thought Prompting

Für komplexe Aufgaben kannst du Claude anweisen, schrittweise zu denken:

```
Analysiere das folgende Problem Schritt für Schritt:
1. Identifiziere zunächst die Kernfrage
2. Liste alle relevanten Faktoren auf
3. Bewerte jeden Faktor einzeln
4. Ziehe eine Schlussfolgerung
```

### 3.2 Few-Shot Learning

Zeige Claude anhand von Beispielen, was du erwartest:

```
Wandle die folgenden Produktbeschreibungen in Bullet Points um:

BEISPIEL INPUT:
"Unser CRM-System vereint Kundenmanagement mit 
intelligenter Automatisierung für bessere Conversion."

BEISPIEL OUTPUT:
• Integriertes Kundenmanagement
• Intelligente Automatisierung
• Conversion-Optimierung

JETZT DU:
[Dein Text hier]
```

### 3.3 Rollenbasiertes Prompting

Weise Claude eine spezifische Expertise zu:

```
Du bist ein erfahrener UX-Designer mit 15 Jahren Erfahrung 
in der Finanzbranche. Du hast bei führenden Schweizer Banken 
gearbeitet und kennst die regulatorischen Anforderungen.

Bewerte das folgende UI-Konzept aus deiner Perspektive.
```

### 3.4 Output-Strukturierung

Definiere präzise, wie das Ergebnis aussehen soll:

```
Erstelle die Analyse im folgenden Format:

## Executive Summary (max. 100 Wörter)
## Haupterkenntnisse
## Empfehlungen
## Nächste Schritte

Verwende:
- Aktive Sprache
- Konkrete Zahlen wo möglich
- Keine Bullet Points in Fliesstext
```

### 3.5 Claude Projects

Für projektbezogene Arbeit bietet claude.ai Projects:

- Sammle relevante Dokumente als Wissensgrundlage
- Definiere Project Instructions für konsistentes Verhalten
- Alle Chats im Project teilen den gleichen Kontext
- Ideal für wiederkehrende Aufgaben mit gleichem Hintergrund

### 3.6 Agentic Workflows

Claude kann komplexe Aufgaben autonom bearbeiten:

- **Computer Use**: Claude kann den Computer bedienen
- **Tool Use**: Integration externer APIs
- **Multi-Step Tasks**: Automatische Aufgabenzerlegung

---

## Kapitel 4: Claude Skills

### 4.1 Was sind Skills?

Skills sind wiederverwendbare Anleitungen, die Claude befähigen, spezialisierte Aufgaben konsistent und hochwertig auszuführen. Sie funktionieren wie "Rezepte" für wiederkehrende Workflows und erweitern Claudes Fähigkeiten um domänenspezifisches Wissen.

**Vorteile von Skills:**

| Aspekt | Ohne Skill | Mit Skill |
|--------|-----------|-----------|
| Konsistenz | Variierende Ergebnisse | Standardisierte Outputs |
| Effizienz | Wiederholung von Instruktionen | Einmalige Definition |
| Qualität | Generische Antworten | Domänenspezifische Expertise |
| Skalierbarkeit | Manueller Aufwand | Automatisierte Workflows |

### 4.2 Skill-Architektur

Ein Skill besteht aus mehreren Komponenten:

```
/mnt/skills/user/mein-skill/
├── SKILL.md           # Hauptdatei mit Instruktionen
├── templates/         # Vorlagen und Beispiele
│   ├── template.md
│   └── example.md
├── resources/         # Zusätzliche Ressourcen
│   ├── style-guide.md
│   └── checklist.md
└── assets/           # Bilder, Logos etc.
    └── logo.png
```

**Die SKILL.md ist das Herzstück** – sie enthält alle Anweisungen, Regeln und Workflows.

### 4.3 Anatomie einer SKILL.md

Eine gut strukturierte SKILL.md enthält folgende Elemente:

```markdown
# Skill Name

## Beschreibung
Kurze Erklärung, was dieser Skill tut und wann er verwendet wird.

## Trigger-Phrasen
Wann soll Claude diesen Skill aktivieren?
- "Erstelle einen Blogpost für..."
- "Schreibe einen Artikel über..."

## Workflow
1. Schritt 1: Was zuerst tun
2. Schritt 2: Was dann tun
3. Schritt 3: Wie abschliessen

## Output-Format
Wie soll das Ergebnis strukturiert sein?

## Regeln und Einschränkungen
- Was beachten
- Was vermeiden

## Beispiele
Konkrete Beispiele für Input und erwarteten Output.

## Ressourcen
Verweis auf Templates und zusätzliche Dateien.
```

### 4.4 Skill-Kategorien

Skills können in verschiedene Kategorien eingeteilt werden:

**Öffentliche Skills** (`/mnt/skills/public/`)
- Von Anthropic bereitgestellt
- Dokumentenerstellung (DOCX, PPTX, XLSX, PDF)
- Immer verfügbar

**Beispiel-Skills** (`/mnt/skills/examples/`)
- Referenzimplementierungen
- Inspiration für eigene Skills

**Benutzerdefinierte Skills** (`/mnt/skills/user/`)
- Selbst erstellte Skills
- Projekt- oder unternehmensspezifisch
- Höchste Relevanz für individuelle Workflows

### 4.5 Skill erstellen: Schritt für Schritt

**1. Anwendungsfall definieren:**
```
Was ist die wiederkehrende Aufgabe?
→ Beispiel: Blogposts für Tool-Reviews schreiben
```

**2. Workflow dokumentieren:**
```
Welche Schritte sind nötig?
1. Tool recherchieren
2. Kernfeatures identifizieren
3. Zielgruppe definieren
4. Struktur erstellen
5. Content schreiben
6. SEO optimieren
```

**3. SKILL.md schreiben:**
```markdown
# Tool-Review Blogpost

## Beschreibung
Erstellt strukturierte Blogposts für Software-Tool-Reviews.

## Trigger-Phrasen
- "Schreibe einen Blogpost über [Tool]"
- "Tool-Review für [Tool]"
- "Erstelle einen Artikel über [Tool] für den Blog"

## Workflow
1. **Recherche**: Lies die SKILL.md und verstehe das Format
2. **Analyse**: Identifiziere die Kernfeatures des Tools
3. **Struktur**: Folge dem Template in /templates/
4. **Schreiben**: Erstelle den Content nach Style-Guide
5. **Prüfung**: Checke gegen die Checkliste

## Output-Format
- Titel: Catchy, mit Keyword
- Intro: 2-3 Sätze, Hook + Relevanz
- Hauptteil: Features, Vorteile, Anwendung
- Fazit: Empfehlung + CTA
- Metadaten: SEO-Title, Description, Tags

## Regeln
- Du-Ansprache verwenden
- Konkrete Beispiele einbauen
- Screenshots erwähnen (Platzhalter)
- Vergleiche zu Alternativen ziehen
- Max. 1500 Wörter

## Template
Siehe: /templates/blogpost-template.md
```

**4. Templates hinzufügen:**
```markdown
# [Tool-Name]: [Catchy Subtitle]

## Auf einen Blick
- **Kategorie**: [z.B. Projektmanagement]
- **Preis**: [Kostenlos / Freemium / Ab CHF X]
- **Plattform**: [Web / Desktop / Mobile]

## Was ist [Tool]?
[2-3 Sätze Einführung]

## Die wichtigsten Features
### Feature 1
[Beschreibung + Nutzen]

### Feature 2
[Beschreibung + Nutzen]

## Für wen eignet sich [Tool]?
[Zielgruppen-Definition]

## Vorteile und Nachteile
| Vorteile | Nachteile |
|----------|-----------|
| ... | ... |

## Fazit
[Empfehlung + Call-to-Action]

---
*Zuletzt aktualisiert: [Datum]*
```

### 4.6 Best Practices für Skills

**Klarheit über Komplexität:**
- Einfache, verständliche Anweisungen
- Lieber mehr Beispiele als abstrakte Regeln

**Modularität:**
- Ein Skill = Eine Aufgabe
- Kombinierbare Teilskills für komplexe Workflows

**Iterative Verbesserung:**
- Mit einfacher Version starten
- Basierend auf Ergebnissen verfeinern
- Edge Cases dokumentieren

**Versionierung:**
- Änderungen dokumentieren
- Alte Versionen aufbewahren

### 4.7 Skills in der Praxis

**Anwendungsbeispiele:**

| Use Case | Skill-Funktion |
|----------|---------------|
| Content Marketing | Blogposts, Social Media, Newsletter |
| Dokumentation | README, API-Docs, Handbücher |
| Bildung | Kursunterlagen, Curricula, Prüfungen |
| Business | Reports, Analysen, Präsentationen |
| Entwicklung | Code-Reviews, Refactoring-Guides |

**Workflow mit Skills:**

```
1. User: "Erstelle einen Blogpost über Notion"

2. Claude erkennt Trigger → Liest SKILL.md

3. Claude folgt dem definierten Workflow:
   - Recherchiert Notion (falls Web-Suche aktiv)
   - Wendet Template an
   - Befolgt Style-Guide
   - Prüft gegen Checkliste

4. Output: Konsistenter, hochwertiger Blogpost
```

### 4.8 Skill-Debugging

Wenn ein Skill nicht wie erwartet funktioniert:

**Häufige Probleme:**

| Problem | Lösung |
|---------|--------|
| Skill wird nicht erkannt | Trigger-Phrasen prüfen/erweitern |
| Inkonsistente Outputs | Regeln präzisieren, Beispiele hinzufügen |
| Wichtige Schritte fehlen | Workflow detaillierter dokumentieren |
| Falsches Format | Template expliziter gestalten |

**Debugging-Prompt:**
```
Zeige mir, wie du den Skill [Name] interpretierst.
Welche Schritte würdest du ausführen?
Welche Regeln wendest du an?
```

---

# Claude Cheatsheet

## Seite 1: Prompt-Grundlagen

### Prompt-Struktur (CIAO-Framework)

```
C - Context    → Hintergrund und Situation
I - Instruction → Klare Aufgabenstellung  
A - Audience   → Zielgruppe definieren
O - Output     → Gewünschtes Format
```

### Quick Templates

**Analyse-Prompt:**
```
Analysiere [THEMA] unter folgenden Aspekten:
- [Aspekt 1]
- [Aspekt 2]
- [Aspekt 3]
Fasse die Ergebnisse in einer Tabelle zusammen.
```

**Vergleichs-Prompt:**
```
Vergleiche [A] und [B] hinsichtlich:
1. [Kriterium 1]
2. [Kriterium 2]
3. [Kriterium 3]
Erstelle eine Entscheidungsmatrix.
```

**Erklärungs-Prompt:**
```
Erkläre [KONZEPT] so, dass [ZIELGRUPPE] es versteht.
Verwende [ANZAHL] praktische Beispiele.
Vermeide Fachjargon / Verwende Fachbegriffe.
```

**Review-Prompt:**
```
Überprüfe folgenden [TEXT/CODE/DOKUMENT]:
- Auf [Kriterium 1]
- Auf [Kriterium 2]
Schlage konkrete Verbesserungen vor.
```

### Modifier-Wörter

| Modifier | Wirkung |
|----------|---------|
| "Schritt für Schritt" | Aktiviert strukturiertes Reasoning |
| "Sei kritisch" | Fördert kritische Analyse |
| "Denke wie ein..." | Aktiviert Rollenexpertise |
| "Berücksichtige..." | Lenkt Fokus auf spezifische Aspekte |
| "Vermeide..." | Schliesst unerwünschte Elemente aus |
| "Maximal X Wörter" | Kontrolliert Ausgabelänge |
| "Im Stil von..." | Definiert Tonalität |

### Formatierungs-Anweisungen

```
# Für strukturierte Outputs:
- "Erstelle eine Tabelle mit..."
- "Gliedere in Abschnitte..."
- "Nummeriere die Schritte..."
- "Verwende Markdown-Formatierung"

# Für Fliesstext:
- "Schreibe in Prosa ohne Bullet Points"
- "Formuliere als zusammenhängenden Text"
- "Verwende Absätze statt Listen"
```

---

## Seite 2: Funktionen & Befehle

### Eingebaute Tools

| Tool | Trigger | Beispiel |
|------|---------|----------|
| **Web Search** | Aktuelle Infos, News | "Was sind die neuesten Entwicklungen bei..." |
| **Deep Research** | Umfassende Analyse | "Recherchiere umfassend zum Thema..." |
| **Artifacts** | Code, Visualisierungen | "Erstelle ein React-Component für..." |
| **File Creation** | Dokumente | "Erstelle ein Word-Dokument mit..." |
| **Memory** | Personalisierung | "Merke dir, dass ich..." |
| **Past Chats** | Historie | "Was haben wir letzte Woche zu X besprochen?" |

### Datei-Befehle

```
# Dokumente erstellen
"Erstelle ein Word-Dokument..."
"Erstelle eine PowerPoint-Präsentation..."
"Erstelle eine Excel-Tabelle..."
"Erstelle ein PDF..."

# Dateien analysieren
"Analysiere diese Datei..."
"Fasse dieses Dokument zusammen..."
"Extrahiere die wichtigsten Punkte..."
```

### Memory-Befehle

```
# Hinzufügen
"Merke dir: [Information]"
"Speichere: [Präferenz]"

# Abrufen
"Was weisst du über mich?"
"Was hast du dir gemerkt?"

# Löschen
"Vergiss [Information]"
"Lösche aus deinem Gedächtnis..."
```

### Conversation-Befehle

```
# Navigation
"Fasse unsere Konversation zusammen"
"Was haben wir bisher besprochen?"
"Zurück zum ursprünglichen Thema"

# Iteration
"Überarbeite die letzte Antwort"
"Mach es kürzer/länger"
"Mehr Details zu Punkt 3"
"Anderer Ansatz bitte"
```

### Code & Technical

```
# Code-Erstellung
"Schreibe [Sprache]-Code für..."
"Erstelle eine Funktion die..."
"Implementiere [Algorithmus]..."

# Code-Review
"Überprüfe diesen Code auf Fehler"
"Optimiere für Performance"
"Erkläre was dieser Code macht"

# Debugging
"Finde den Bug in..."
"Warum funktioniert X nicht?"
```

---

## Seite 3: Best Practices & Troubleshooting

### Do's ✓

| Praxis | Beispiel |
|--------|----------|
| Kontext geben | "Als Produktmanager im Fintech-Bereich brauche ich..." |
| Spezifisch sein | "Erstelle 5 Headlines mit max. 60 Zeichen" |
| Beispiele zeigen | "Hier ist ein Beispiel wie es aussehen soll: ..." |
| Format definieren | "Antworte in einer Tabelle mit 3 Spalten" |
| Iterieren | "Gut, jetzt verfeinere Punkt 2..." |
| Feedback geben | "Das ist zu lang, kürze auf die Hälfte" |

### Don'ts ✗

| Vermeiden | Stattdessen |
|-----------|-------------|
| Vage Anfragen | Präzise Aufgabenstellung |
| Mehrere Aufgaben gleichzeitig | Eine Aufgabe pro Prompt |
| Annahmen machen | Kontext explizit nennen |
| Zu lange Prompts | Fokussiert und strukturiert |
| Ungeduld bei komplexen Aufgaben | Schrittweise vorgehen |

### Troubleshooting

**Problem: Antwort zu allgemein**
```
→ Mehr Kontext geben
→ Spezifischere Frage stellen
→ Beispiel für gewünschtes Ergebnis zeigen
```

**Problem: Antwort zu lang**
```
→ "Maximal X Wörter/Sätze"
→ "Nur die wichtigsten 3 Punkte"
→ "Executive Summary Format"
```

**Problem: Falsches Format**
```
→ Format explizit definieren
→ Beispiel-Output zeigen
→ "Kein Markdown" / "Als Fliesstext"
```

**Problem: Claude versteht nicht**
```
→ Aufgabe umformulieren
→ In Teilschritte zerlegen
→ Kontext erweitern
```

**Problem: Halluzinationen**
```
→ "Suche aktuelle Informationen"
→ "Sage wenn du unsicher bist"
→ "Nenne deine Quellen"
```

### Qualitäts-Checkliste

```
□ Ist der Kontext klar?
□ Ist die Aufgabe eindeutig?
□ Ist das Format definiert?
□ Sind Einschränkungen genannt?
□ Ist die Zielgruppe bekannt?
□ Sind Beispiele hilfreich?
```

### Keyboard Shortcuts (claude.ai)

| Shortcut | Funktion |
|----------|----------|
| `Enter` | Nachricht senden |
| `Shift + Enter` | Zeilenumbruch |
| `Cmd/Ctrl + K` | Neuer Chat |
| `Cmd/Ctrl + Shift + O` | Upload-Dialog |
| `/` | Befehls-Menü |

### Nützliche Ressourcen

| Ressource | URL |
|-----------|-----|
| Dokumentation | docs.claude.com |
| Support | support.claude.com |
| API Reference | docs.claude.com/api |
| Prompt Library | anthropic.com/prompts |

---

## Seite 4: Skills – Grundlagen & Struktur

### Skill-Konzept auf einen Blick

```
┌─────────────────────────────────────────────────────────┐
│                    SKILL = REZEPT                       │
├─────────────────────────────────────────────────────────┤
│  Input (Trigger)    →  Workflow    →  Output (Format)   │
│  "Erstelle Blog..."    1. Analyse      Strukturierter   │
│                        2. Struktur     Blogpost nach    │
│                        3. Schreiben    Template         │
└─────────────────────────────────────────────────────────┘
```

### Skill-Verzeichnisse

| Pfad | Typ | Beschreibung |
|------|-----|--------------|
| `/mnt/skills/public/` | System | DOCX, PPTX, XLSX, PDF |
| `/mnt/skills/examples/` | Referenz | Beispiel-Implementierungen |
| `/mnt/skills/user/` | Custom | Eigene Skills |

### SKILL.md Grundstruktur

```markdown
# [Skill-Name]

## Beschreibung
[Was macht dieser Skill?]

## Trigger-Phrasen
- "[Phrase 1]"
- "[Phrase 2]"

## Workflow
1. [Schritt 1]
2. [Schritt 2]
3. [Schritt 3]

## Output-Format
[Struktur des Ergebnisses]

## Regeln
- [Regel 1]
- [Regel 2]

## Beispiele
[Input → Output Beispiele]
```

### Skill-Komponenten

```
mein-skill/
├── SKILL.md          # Pflicht: Hauptanleitung
├── templates/        # Optional: Vorlagen
├── resources/        # Optional: Zusatzmaterial
├── examples/         # Optional: Beispiele
└── assets/          # Optional: Medien
```

### Trigger-Phrasen definieren

**Gute Trigger:**
```markdown
## Trigger-Phrasen
- "Erstelle einen Blogpost über [Thema]"
- "Schreibe einen Artikel zu [Thema]"
- "Blog-Artikel für [Thema]"
- "Tool-Review für [Software]"
```

**Trigger-Typen:**

| Typ | Beispiel | Wann nutzen |
|-----|----------|-------------|
| Direkt | "Erstelle Blogpost" | Eindeutige Aufgaben |
| Variabel | "[Thema] Blogpost" | Flexible Inputs |
| Kontext | "für den PDT Blog" | Projektspezifisch |

### Workflow dokumentieren

```markdown
## Workflow

### Phase 1: Vorbereitung
1. Lies diese SKILL.md vollständig
2. Prüfe verfügbare Templates in /templates/
3. Identifiziere relevante Ressourcen

### Phase 2: Analyse
4. Analysiere das Thema/Input
5. Definiere Zielgruppe
6. Recherchiere falls nötig (Web Search)

### Phase 3: Erstellung
7. Wende Template an
8. Fülle alle Sektionen aus
9. Prüfe gegen Regeln

### Phase 4: Qualitätssicherung
10. Checke Checkliste in /resources/
11. Validiere Format
12. Finalisiere Output
```

### Output-Formate spezifizieren

```markdown
## Output-Format

### Struktur
- **Titel**: Max. 60 Zeichen, Keyword enthalten
- **Intro**: 2-3 Sätze, Hook + Relevanz
- **Hauptteil**: 3-5 Abschnitte mit H2/H3
- **Fazit**: Zusammenfassung + CTA
- **Meta**: SEO-Title, Description

### Formatierung
- Markdown verwenden
- Tabellen für Vergleiche
- Code-Blöcke für technische Inhalte
- Keine übermässigen Bullet Points

### Länge
- Minimum: 800 Wörter
- Maximum: 1500 Wörter
- Ideal: 1000-1200 Wörter
```

---

## Seite 5: Skills – Praxis & Beispiele

### Skill-Beispiel: Rezept-Formatter

```markdown
# Rezept-Formatter

## Beschreibung
Formatiert Rezepte in ein einheitliches Format für 
den Food-Blog foodies.pixelfreund.ch.

## Trigger-Phrasen
- "Formatiere dieses Rezept"
- "Erstelle ein Rezept für [Gericht]"
- "Rezept für den Food-Blog"

## Workflow
1. Lies das Template in /templates/rezept.md
2. Extrahiere: Titel, Zutaten, Schritte, Zeiten
3. Strukturiere nach Template
4. Ergänze: Portionen, Schwierigkeit, Tags
5. Prüfe Vollständigkeit

## Output-Format
---
title: "[Rezeptname]"
date: YYYY-MM-DD
categories: [Kategorie]
tags: [tag1, tag2]
prepTime: "X Min"
cookTime: "X Min"
servings: X
difficulty: [einfach/mittel/anspruchsvoll]
---

# [Rezeptname]

## Zutaten
- [ ] Zutat 1
- [ ] Zutat 2

## Zubereitung
1. Schritt 1
2. Schritt 2

## Tipps
[Optionale Tipps]

## Regeln
- Schweizer Rechtschreibung (kein ß)
- Metrische Einheiten
- Zutaten als Checkliste
- Klare, nummerierte Schritte
```

### Skill-Beispiel: Kurs-Curriculum

```markdown
# HWZ Curriculum Creator

## Beschreibung
Erstellt strukturierte Kursbeschreibungen für 
HWZ-Seminare im standardisierten Format.

## Trigger-Phrasen
- "Erstelle ein Curriculum für [Kurs]"
- "HWZ Kursbeschreibung für [Thema]"
- "Seminar-Konzept [Dauer] Tage"

## Workflow
1. Bestimme Kursdauer (1/2/3 Tage)
2. Berechne Module (5 pro Halbtag)
3. Lies Template für entsprechende Dauer
4. Fülle Lernziele, Inhalte, Methoden
5. Ergänze Zielgruppe und Voraussetzungen

## Output-Format
Siehe /templates/curriculum-[dauer]-tag.md

## Regeln
- 5 Module pro Halbtag
- Lernziele mit Bloom's Taxonomy
- Mix aus Theorie und Praxis
- Konkrete Methoden pro Modul
```

### Quick Reference: Skill erstellen

```bash
# 1. Verzeichnis erstellen
/mnt/skills/user/mein-skill/

# 2. SKILL.md anlegen mit:
- Beschreibung (was?)
- Trigger (wann?)
- Workflow (wie?)
- Format (welche Form?)
- Regeln (was beachten?)

# 3. Optional: Templates hinzufügen
/mnt/skills/user/mein-skill/templates/

# 4. Testen und iterieren
"Verwende den Skill [Name] für..."
```

### Skill-Debugging Checkliste

```
□ Wird der Skill erkannt?
  → Trigger-Phrasen erweitern
  
□ Falscher Output?
  → Format präziser definieren
  
□ Schritte fehlen?
  → Workflow detaillieren
  
□ Inkonsistente Qualität?
  → Mehr Beispiele hinzufügen
  
□ Template nicht genutzt?
  → Pfad explizit angeben
```

### Skill-Kombinationen

```markdown
# Komplexer Workflow mit mehreren Skills

User: "Recherchiere Notion und erstelle einen Blogpost"

Claude:
1. → Web Search Skill (Recherche)
2. → Tool-Review Skill (Blogpost)
3. → SEO Skill (Optimierung)

Ergebnis: Recherchierter, formatierter, optimierter Blogpost
```

### Fortgeschrittene Techniken

**Bedingte Logik:**
```markdown
## Workflow

### Wenn Kurs = 1 Tag:
- 10 Module (5 + 5)
- Template: curriculum-1-tag.md

### Wenn Kurs = 2 Tage:
- 20 Module
- Template: curriculum-2-tage.md
```

**Ressourcen-Referenzierung:**
```markdown
## Ressourcen
- Style Guide: /resources/style-guide.md
- Checkliste: /resources/checklist.md
- Beispiel: /examples/muster-output.md

Claude soll diese Dateien lesen bevor er beginnt.
```

**Qualitätsprüfung einbauen:**
```markdown
## Qualitätssicherung
Vor dem finalen Output prüfen:
1. [ ] Alle Pflichtfelder ausgefüllt?
2. [ ] Format entspricht Template?
3. [ ] Regeln eingehalten?
4. [ ] Länge im Rahmen?
5. [ ] Keine Rechtschreibfehler?
```

### Häufige Skill-Typen

| Kategorie | Beispiele |
|-----------|-----------|
| **Content** | Blogposts, Newsletter, Social Media |
| **Dokumente** | Reports, Proposals, Curricula |
| **Daten** | Analysen, Zusammenfassungen |
| **Code** | Reviews, Dokumentation |
| **Prozesse** | Checklisten, Workflows |

### Skill-Verwaltung

```
Tipps für die Organisation:

1. Namenskonvention
   skill-kategorie-name/
   z.B.: content-blogpost-tool-review/

2. Versionierung
   SKILL.md → SKILL-v2.md (Backup)
   Changelog in SKILL.md führen

3. Dokumentation
   README.md mit Übersicht aller Skills
   Changelog für Änderungen
```

---

*Erstellt für den praktischen Einsatz | Version 1.1 | Januar 2025*
