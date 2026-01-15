# ChatGPT Einführung & Cheatsheet

> Eine praxisorientierte Anleitung für die effektive Nutzung von ChatGPT

---

## Kapitel 1: Was ist ChatGPT?

### 1.1 Überblick

ChatGPT ist ein KI-Assistent von OpenAI, basierend auf der GPT-Architektur (Generative Pre-trained Transformer). Seit dem Launch im November 2022 hat sich ChatGPT zum meistgenutzten KI-Assistenten weltweit entwickelt und setzt kontinuierlich neue Standards in der Mensch-KI-Interaktion.

### 1.2 Die GPT-Modellfamilie

OpenAI bietet verschiedene Modelle mit unterschiedlichen Fähigkeiten:

| Modell | Stärken | Typische Anwendungen |
|--------|---------|---------------------|
| **GPT-4o** | Multimodal, schnell, vielseitig | Alltägliche Aufgaben, Vision, Audio |
| **GPT-4o mini** | Kosteneffizient, schnell | Einfache Aufgaben, hohe Volumen |
| **o1** | Tiefes Reasoning, komplexe Logik | Mathematik, Wissenschaft, Strategie |
| **o1-mini** | Reasoning mit Effizienz | Coding, technische Probleme |
| **o3** | Neueste Generation, höchste Intelligenz | Forschung, komplexe Analysen |

### 1.3 Zugangsoptionen

ChatGPT ist über verschiedene Kanäle verfügbar:

- **chat.openai.com**: Web-Interface für direkte Interaktion
- **ChatGPT App**: Mobile Apps für iOS und Android, Desktop-App
- **API**: Programmatischer Zugang über OpenAI Platform
- **ChatGPT Enterprise/Team**: Business-Lösungen mit erweiterten Features
- **Copilot**: Integration in Microsoft-Produkte (basiert auf GPT)

### 1.4 Abo-Modelle

| Plan | Preis | Kernfeatures |
|------|-------|--------------|
| **Free** | Kostenlos | GPT-4o mini, begrenzte GPT-4o Nutzung |
| **Plus** | $20/Monat | Voller GPT-4o Zugang, DALL-E, erweiterte Tools |
| **Pro** | $200/Monat | o1 pro mode, unbegrenzte Nutzung, höchste Priorität |
| **Team** | $25-30/User | Workspace, Admin-Controls, längerer Kontext |
| **Enterprise** | Custom | SSO, erweiterte Sicherheit, unbegrenzt |

---

## Kapitel 2: Grundlagen der Nutzung

### 2.1 Der richtige Prompt

Die Qualität der Antwort hängt massgeblich von der Qualität des Prompts ab. Ein guter Prompt enthält:

**Rolle**: Wer soll ChatGPT sein?

**Kontext**: Was ist der Hintergrund?

**Aufgabe**: Was soll konkret getan werden?

**Format**: Wie soll das Ergebnis aussehen?

### 2.2 Beispiel für einen strukturierten Prompt

```
Du bist ein erfahrener Marketing-Stratege für B2B-SaaS-Unternehmen.

KONTEXT:
Wir launchen ein neues Projektmanagement-Tool für 
mittelständische Unternehmen in der DACH-Region.

AUFGABE:
Entwickle eine Go-to-Market-Strategie für die ersten 90 Tage.

FORMAT:
- Phasenweise Struktur (Tag 1-30, 31-60, 61-90)
- Konkrete Massnahmen mit KPIs
- Budget-Empfehlungen pro Phase

EINSCHRÄNKUNGEN:
- Gesamtbudget: CHF 50'000
- Fokus auf digitale Kanäle
```

### 2.3 Conversation Management

ChatGPT merkt sich den Kontext innerhalb einer Konversation. Nutze dies strategisch:

- **Iteratives Arbeiten**: Verfeinere Ergebnisse schrittweise
- **Referenzierung**: Beziehe dich auf frühere Antworten
- **Korrektur**: Weise auf Fehler hin, ChatGPT lernt im Kontext
- **Richtungswechsel**: "Lass uns einen anderen Ansatz versuchen..."

### 2.4 Integrierte Funktionen

ChatGPT bietet verschiedene eingebaute Tools:

| Funktion | Beschreibung | Verfügbarkeit |
|----------|--------------|---------------|
| **Web Browsing** | Aktuelle Informationen aus dem Internet | Plus/Pro |
| **DALL-E** | Bildgenerierung aus Text | Plus/Pro |
| **Code Interpreter** | Python-Code ausführen, Datenanalyse | Plus/Pro |
| **Advanced Data Analysis** | Komplexe Datenverarbeitung | Plus/Pro |
| **Canvas** | Kollaboratives Schreiben und Coden | Plus/Pro |
| **Voice Mode** | Sprachbasierte Interaktion | Alle (erweitert in Plus) |
| **Memory** | Erinnerung über Chats hinweg | Plus/Pro |
| **Custom GPTs** | Spezialisierte Assistenten erstellen | Plus/Pro |

### 2.5 Dateien und Uploads

ChatGPT kann verschiedene Dateitypen verarbeiten:

- **Dokumente**: PDF, DOCX, TXT, MD, PPTX
- **Bilder**: PNG, JPG, GIF, WEBP (mit Vision)
- **Daten**: CSV, XLSX, JSON
- **Code**: Praktisch alle Programmiersprachen
- **Audio**: MP3, WAV (für Transkription)

---

## Kapitel 3: Fortgeschrittene Techniken

### 3.1 Custom GPTs erstellen

Custom GPTs sind spezialisierte Versionen von ChatGPT für wiederkehrende Aufgaben:

**Aufbau eines Custom GPT:**

1. **Name & Beschreibung**: Klare Positionierung
2. **Instructions**: Detaillierte Verhaltensanweisungen
3. **Conversation Starters**: Vorgeschlagene Einstiegsfragen
4. **Knowledge**: Hochgeladene Referenzdokumente
5. **Capabilities**: Aktivierte Tools (Web, DALL-E, Code)
6. **Actions**: API-Integrationen (optional)

**Beispiel Instructions für einen Custom GPT:**

```
Du bist ein Experte für Schweizer Arbeitsrecht.

VERHALTEN:
- Antworte präzise und faktenbasiert
- Verweise auf relevante Gesetzesartikel (OR, ArG)
- Weise bei komplexen Fällen auf Anwaltskonsultation hin

EINSCHRÄNKUNGEN:
- Keine Rechtsberatung im juristischen Sinne
- Aktualität: Weise auf mögliche Gesetzesänderungen hin

FORMAT:
- Strukturierte Antworten
- Praktische Beispiele wo möglich
```

### 3.2 Chain-of-Thought Prompting

Für komplexe Aufgaben kannst du ChatGPT anweisen, schrittweise zu denken:

```
Löse dieses Problem Schritt für Schritt:
1. Analysiere zunächst die Ausgangslage
2. Identifiziere die Kernvariablen
3. Entwickle mögliche Lösungsansätze
4. Bewerte jeden Ansatz
5. Empfehle die beste Lösung mit Begründung
```

### 3.3 Few-Shot Learning

Zeige ChatGPT anhand von Beispielen, was du erwartest:

```
Wandle Kundenfeedback in strukturierte Insights um:

BEISPIEL INPUT:
"Das Produkt ist super, aber die Lieferung hat ewig gedauert 
und der Support war nicht erreichbar."

BEISPIEL OUTPUT:
{
  "sentiment": "gemischt",
  "positiv": ["Produktqualität"],
  "negativ": ["Lieferzeit", "Support-Erreichbarkeit"],
  "priorität": "Support verbessern"
}

JETZT ANALYSIERE:
[Dein Kundenfeedback hier]
```

### 3.4 Canvas nutzen

Canvas ist ein kollaborativer Arbeitsbereich für längere Texte und Code:

**Für Texte:**
- Gemeinsames Editieren
- Abschnitte markieren und überarbeiten
- Länge anpassen, Ton ändern
- Lesbarkeit verbessern

**Für Code:**
- Syntax-Highlighting
- Bugs fixen lassen
- Code kommentieren
- In andere Sprachen portieren

**Canvas aktivieren:**
```
"Öffne Canvas für diesen Text"
"Lass uns das im Canvas bearbeiten"
"Wechsle zu Canvas"
```

### 3.5 Advanced Data Analysis

Mit dem Code Interpreter kannst du komplexe Datenanalysen durchführen:

```
Analysiere die hochgeladene CSV-Datei:
1. Zeige eine statistische Zusammenfassung
2. Identifiziere Ausreisser
3. Erstelle relevante Visualisierungen
4. Finde Korrelationen zwischen den Variablen
5. Exportiere die Ergebnisse als Excel-Datei
```

### 3.6 API und Automation

Für fortgeschrittene Nutzer bietet die OpenAI API:

- **Assistants API**: Persistente Assistenten mit Tools
- **Function Calling**: Strukturierte Outputs und Tool-Integration
- **Fine-Tuning**: Modell auf eigene Daten trainieren
- **Batch API**: Kosteneffiziente Massenverarbeitung

---

# ChatGPT Cheatsheet

## Seite 1: Prompt-Grundlagen

### Prompt-Struktur (RICE-Framework)

```
R - Role       → Rolle/Expertise definieren
I - Instructions → Klare Anweisungen
C - Context    → Hintergrund und Situation  
E - Examples   → Beispiele für gewünschtes Ergebnis
```

### Quick Templates

**Analyse-Prompt:**
```
Agiere als [EXPERTE].
Analysiere [THEMA] hinsichtlich:
- [Aspekt 1]
- [Aspekt 2]
- [Aspekt 3]
Präsentiere die Ergebnisse als [FORMAT].
```

**Content-Erstellung:**
```
Schreibe einen [CONTENT-TYP] über [THEMA].
Zielgruppe: [BESCHREIBUNG]
Ton: [formell/informell/inspirierend/etc.]
Länge: [WORTANZAHL]
Inkludiere: [ELEMENTE]
```

**Problemlösung:**
```
Ich habe folgendes Problem: [BESCHREIBUNG]

Kontext: [HINTERGRUND]
Bereits versucht: [BISHERIGE ANSÄTZE]
Ziel: [GEWÜNSCHTES ERGEBNIS]

Entwickle 3 Lösungsansätze mit Vor- und Nachteilen.
```

**Code-Erstellung:**
```
Schreibe [SPRACHE]-Code für [FUNKTION].

Anforderungen:
- [Anforderung 1]
- [Anforderung 2]

Input: [BESCHREIBUNG]
Output: [BESCHREIBUNG]

Füge Kommentare hinzu und erkläre die Logik.
```

### Modifier-Phrasen

| Phrase | Wirkung |
|--------|---------|
| "Denke Schritt für Schritt" | Aktiviert strukturiertes Reasoning |
| "Agiere als..." | Aktiviert Rollenexpertise |
| "Sei kritisch und hinterfrage" | Fördert kritische Analyse |
| "Vergleiche Pro und Contra" | Ausgewogene Betrachtung |
| "Fasse dich kurz" | Kompakte Antworten |
| "Gehe ins Detail" | Ausführliche Erklärungen |
| "Für einen Anfänger erklärt" | Vereinfachte Sprache |
| "Auf Experten-Niveau" | Technische Tiefe |

### Output-Kontrolle

```
# Länge steuern
- "In maximal 100 Wörtern"
- "Als Tweet (280 Zeichen)"
- "Ausführlich in 1000+ Wörtern"

# Format definieren
- "Als nummerierte Liste"
- "In Tabellenform"
- "Als JSON/XML/Markdown"
- "Als Fliesstext ohne Aufzählungen"

# Stil anpassen
- "Im Ton von [Publikation/Person]"
- "Formell für Geschäftskontext"
- "Locker und umgangssprachlich"
```

---

## Seite 2: Funktionen & Befehle

### Tool-Aktivierung

| Tool | Aktivierungs-Phrase | Beispiel |
|------|---------------------|----------|
| **Browse** | "Suche im Internet nach..." | "Suche aktuelle News zu KI-Regulierung" |
| **DALL-E** | "Erstelle ein Bild von..." | "Erstelle ein Bild von einem futuristischen Büro" |
| **Code Interpreter** | "Analysiere diese Daten..." | "Analysiere die CSV und erstelle Grafiken" |
| **Canvas** | "Öffne Canvas..." | "Öffne Canvas für diesen Artikel" |
| **Memory** | "Merke dir..." | "Merke dir: Ich arbeite im Fintech-Bereich" |

### DALL-E Prompts

```
# Struktur für Bildgenerierung
[Stil] [Subjekt] [Aktion] [Umgebung] [Beleuchtung] [Zusätze]

# Beispiele
"Fotorealistisches Portrait eines Geschäftsmannes 
in modernem Büro, natürliches Licht, 4K"

"Minimalistisches Logo-Design für Tech-Startup, 
blau und weiss, auf transparentem Hintergrund"

"Isometrische Illustration eines Smart Home, 
flaches Design, lebhafte Farben"
```

### Code Interpreter Befehle

```
# Datenanalyse
"Lade die Datei und zeige die ersten 10 Zeilen"
"Erstelle eine Zusammenfassung der Statistiken"
"Visualisiere die Verteilung von [Spalte]"
"Finde Korrelationen zwischen [A] und [B]"

# Dateitransformation
"Konvertiere von CSV zu Excel"
"Bereinige die Daten (entferne Duplikate, fülle Lücken)"
"Erstelle einen PDF-Report aus den Ergebnissen"

# Berechnungen
"Berechne [komplexe Formel]"
"Simuliere [Szenario] mit diesen Parametern"
```

### Memory-Befehle

```
# Speichern
"Merke dir, dass ich [Information]"
"Speichere für zukünftige Gespräche: [Präferenz]"

# Abrufen
"Was weisst du über mich?"
"Welche Präferenzen hast du gespeichert?"

# Löschen
"Vergiss [spezifische Information]"
"Lösche alle gespeicherten Informationen über mich"

# In Einstellungen
Settings → Personalization → Memory → Manage
```

### Canvas-Befehle

```
# Text-Bearbeitung
"Kürze diesen Abschnitt"
"Erweitere diesen Teil"
"Mach den Ton formeller"
"Verbessere die Lesbarkeit"
"Füge einen Abschnitt über [X] hinzu"

# Code-Bearbeitung
"Füge Kommentare hinzu"
"Optimiere die Performance"
"Konvertiere zu [andere Sprache]"
"Schreibe Unit Tests"
"Finde und behebe Bugs"
```

### Custom GPT Grundstruktur

```
NAME: [Beschreibender Name]

DESCRIPTION: 
[Was der GPT tut in 1-2 Sätzen]

INSTRUCTIONS:
Du bist [ROLLE] mit Expertise in [BEREICH].

VERHALTEN:
- [Regel 1]
- [Regel 2]
- [Regel 3]

EINSCHRÄNKUNGEN:
- [Was nicht tun]

AUSGABEFORMAT:
- [Wie Antworten strukturiert sein sollen]

CONVERSATION STARTERS:
- "[Beispielfrage 1]"
- "[Beispielfrage 2]"
```

---

## Seite 3: Best Practices & Troubleshooting

### Do's ✓

| Praxis | Beispiel |
|--------|----------|
| Rolle zuweisen | "Du bist ein erfahrener UX-Designer..." |
| Kontext liefern | "Für ein Startup mit 10 Mitarbeitern..." |
| Beispiele geben | "Hier ist ein Beispiel für den gewünschten Stil:..." |
| Iterativ arbeiten | "Gut, jetzt passe den zweiten Absatz an..." |
| Spezifisch sein | "Genau 5 Punkte, je maximal 2 Sätze" |
| Feedback nutzen | "Das ist zu technisch, vereinfache es" |

### Don'ts ✗

| Vermeiden | Stattdessen |
|-----------|-------------|
| "Schreib was über Marketing" | Zielgruppe, Kanal, Ton definieren |
| Alles in einem Prompt | Komplexe Aufgaben aufteilen |
| Unklare Anweisungen | Präzise Erwartungen formulieren |
| Ergebnisse akzeptieren | Iterieren und verfeinern |
| Ohne Kontext fragen | Hintergrund erklären |

### Troubleshooting

**Problem: Zu generische Antworten**
```
→ Mehr Kontext hinzufügen
→ Spezifische Beispiele geben
→ "Sei spezifisch und konkret"
→ Zielgruppe definieren
```

**Problem: Halluzinationen / Falsche Fakten**
```
→ "Suche im Internet nach aktuellen Informationen"
→ "Wenn du unsicher bist, sage es"
→ "Nenne Quellen für deine Aussagen"
→ Fakten selbst verifizieren
```

**Problem: Unerwünschtes Format**
```
→ Format explizit vorgeben
→ Beispiel-Output zeigen
→ "Keine Bullet Points, nur Fliesstext"
→ "Antworte nur mit JSON, kein Text davor"
```

**Problem: Zu lang / Zu kurz**
```
→ Wortanzahl vorgeben
→ "Maximal 3 Sätze pro Punkt"
→ "Ausführliche Erklärung mit Beispielen"
→ "Executive Summary in 50 Wörtern"
```

**Problem: Code funktioniert nicht**
```
→ Fehlermeldung teilen
→ Kontext erklären (Framework, Version)
→ "Teste den Code und korrigiere Fehler"
→ Code Interpreter für Ausführung nutzen
```

**Problem: Vergisst Kontext**
```
→ Wichtiges wiederholen
→ Memory-Funktion nutzen
→ Custom GPT mit Instructions erstellen
→ Kontext am Anfang zusammenfassen
```

### Qualitäts-Checkliste

```
□ Ist eine klare Rolle definiert?
□ Ist genügend Kontext vorhanden?
□ Sind die Anweisungen eindeutig?
□ Ist das gewünschte Format klar?
□ Sind Beispiele hilfreich?
□ Wurde das Ergebnis iteriert?
```

### Keyboard Shortcuts

| Shortcut | Funktion |
|----------|----------|
| `Enter` | Nachricht senden |
| `Shift + Enter` | Zeilenumbruch |
| `Ctrl/Cmd + Shift + ;` | Letzten Chat kopieren |
| `Ctrl/Cmd + Shift + C` | Code-Block kopieren |
| `Ctrl/Cmd + /` | Shortcuts anzeigen |

### Nützliche Ressourcen

| Ressource | URL |
|-----------|-----|
| ChatGPT | chat.openai.com |
| OpenAI Platform | platform.openai.com |
| Dokumentation | platform.openai.com/docs |
| GPT Store | chat.openai.com/gpts |
| Status | status.openai.com |
| Community | community.openai.com |

### Vergleich: Wann welches Modell?

| Aufgabe | Empfohlenes Modell |
|---------|-------------------|
| Schnelle Alltagsfragen | GPT-4o mini |
| Kreative Texte, Bilder | GPT-4o |
| Komplexe Logik, Mathe | o1 / o3 |
| Code-Probleme | o1-mini |
| Datenanalyse | GPT-4o + Code Interpreter |

---

*Erstellt für den praktischen Einsatz | Version 1.0 | Januar 2025*
