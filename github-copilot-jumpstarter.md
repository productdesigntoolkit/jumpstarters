# GitHub Copilot Jumpstarter

## Schnellstart

### Installation & Setup
1. **VS Code Extension installieren**
   - Extension "GitHub Copilot" im VS Code Marketplace suchen
   - Installieren und mit GitHub-Account anmelden
   - Subscription erforderlich (kostenlos für Studenten/Open Source)

2. **Erste Aktivierung**
   - `Ctrl/Cmd + I` öffnet Copilot Chat
   - Tab-Taste akzeptiert Vorschläge im Code-Editor
   - `Alt + ]` / `Alt + [` wechselt zwischen Vorschlägen

## Die wichtigsten Features

### 1. Inline Code-Completion
**So funktioniert's:**
- Beginne mit einem Kommentar oder Funktionsnamen
- Copilot schlägt automatisch Code vor
- Tab drücken zum Akzeptieren, Esc zum Ablehnen

**Beispiel:**
```javascript
// Funktion zum Sortieren eines Arrays nach Namen
// Copilot vervollständigt automatisch
```

### 2. Copilot Chat (`Ctrl/Cmd + I`)
**Anwendungsfälle:**
- Code erklären lassen: Markiere Code → "Erkläre diesen Code"
- Refactoring: "Refactor this to use async/await"
- Tests schreiben: "Write unit tests for this function"
- Bugs fixen: "Why isn't this working?"

### 3. Slash Commands
Direktbefehle im Chat:
- `/explain` – Code erklären
- `/fix` – Fehler beheben
- `/tests` – Tests generieren
- `/doc` – Dokumentation erstellen
- `/simplify` – Code vereinfachen

## Best Practices

### Context ist King
**Gib Copilot Kontext durch:**
- Aussagekräftige Dateinamen
- Klare Kommentare vor Code-Blöcken
- Offene verwandte Dateien im Editor
- Präzise Funktions- und Variablennamen

**Beispiel für guten Context:**
```python
# API Client für Finnova Bankware REST API
# Authentifizierung via OAuth2
# Basis-URL: https://api.finnova.ch/v2

class FinnovaAPIClient:
    # Copilot versteht nun den gesamten Kontext
```

### Iteratives Arbeiten
1. Lass Copilot einen ersten Entwurf erstellen
2. Reviewe und verfeinere den Code
3. Nutze Chat für spezifische Anpassungen
4. Teste gründlich (Copilot ist nicht fehlerfrei!)

### Produktivitäts-Hacks

**Multi-Line Completion:**
- Schreibe den ersten Teil einer Funktion
- Drücke Enter mehrmals
- Copilot vervollständigt die gesamte Logik

**Comment-Driven Development:**
```javascript
// Erstelle eine Funktion die:
// 1. User-Daten von der API lädt
// 2. Nach aktiven Usern filtert
// 3. Nach Namen alphabetisch sortiert
// 4. Als JSON zurückgibt

// Copilot generiert die komplette Funktion
```

**Test-Driven mit Copilot:**
```python
def test_user_authentication_with_valid_credentials():
    # Arrange
    user = User(email="test@test.ch", password="secret123")
    
    # Act & Assert
    # Copilot vervollständigt den Test
```

## Für deine Use Cases

### Curriculum-Entwicklung (HWZ)
```markdown
<!-- Prompt im Chat -->
"Erstelle eine Übung für Studenten zum Thema Agentic AI mit Python.
Die Übung soll ein Multi-Agent System mit CrewAI demonstrieren.
Schwierigkeitsgrad: Master-Level.
Inkl. Lösung und Bewertungskriterien."
```

### Product Design Toolkit
```javascript
// Erstelle React-Komponente für Canvas-Tool
// Requirements:
// - Drag & Drop für Elemente
// - Zoom & Pan Funktionalität
// - Export als PNG/PDF
// - Mobile-responsive

// Copilot generiert Basis-Struktur
```

### Recipe Entwicklung (foodies.pixelfreund.ch)
```python
# Python Script für Recipe-Formatter
# Input: Rohtext-Rezept
# Output: Strukturiertes Markdown im Format:
# - Titel
# - Zutaten (mit Mengen)
# - Schritte (nummeriert)
# - Nährwerte

# Copilot erstellt Parser & Formatter
```

## Keyboard Shortcuts (Essential)

| Aktion | macOS | Windows/Linux |
|--------|-------|---------------|
| Chat öffnen | `Cmd + I` | `Ctrl + I` |
| Vorschlag akzeptieren | `Tab` | `Tab` |
| Nächster Vorschlag | `Option + ]` | `Alt + ]` |
| Vorheriger Vorschlag | `Option + [` | `Alt + [` |
| Inline Chat | `Cmd + K` | `Ctrl + K` |
| Vorschlag ablehnen | `Esc` | `Esc` |

## Erweiterte Techniken

### 1. Context Files
Erstelle `.github/copilot-instructions.md`:
```markdown
# Project-Specific Instructions

- Nutze TypeScript strict mode
- Verwende Supabase für Backend
- Style mit Tailwind CSS
- Testing mit Vitest
- Dokumentation in Deutsch (Schweizer Rechtschreibung)
```

### 2. Workspace Instructions
In VS Code Settings (`.vscode/settings.json`):
```json
{
  "github.copilot.advanced": {
    "inlineSuggest.enable": true,
    "contextSize": "large"
  }
}
```

### 3. Template-Driven Development
```typescript
// Template für API Routes (Next.js)
// POST /api/users
// - Validiere Input mit Zod
// - Speichere in Supabase
// - Return User-Objekt oder Error
// - TypeScript types aus @/types/user

export async function POST(request: Request) {
  // Copilot füllt Template aus
}
```

## Häufige Fallstricke

### 1. Blind Copy-Paste vermeiden
- **Problem:** Copilot generiert manchmal veralteten oder unsicheren Code
- **Lösung:** Immer reviewen, testen, Security prüfen

### 2. Over-Reliance
- **Problem:** Zu stark auf Copilot verlassen
- **Lösung:** Verstehe den generierten Code, lerne weiter

### 3. Context Overflow
- **Problem:** Zu viele offene Dateien verwirren Copilot
- **Lösung:** Schliesse irrelevante Tabs

### 4. Unspezifische Prompts
- **Problem:** "Erstelle eine Funktion" → generischer Code
- **Lösung:** "Erstelle eine async Funktion die User von Supabase lädt, Fehlerhandling mit try-catch, TypeScript types"

## Quick Wins für heute

1. **Boilerplate eliminieren:** Lass Copilot Config-Files, Tests, Types generieren
2. **Documentation:** Markiere Funktionen → `/doc` → Instant-Doku
3. **Refactoring:** Legacy Code → Chat → "Modernize this to use current best practices"
4. **Learning:** Unbekannter Code → `/explain` → Sofortige Erklärung

## Integration in deinen Workflow

### n8n Automation
Copilot hilft bei:
- Workflow-Logic in JavaScript
- Webhook-Handler
- Daten-Transformationen
- Error-Handling

### Dify Integration
```python
# Erstelle Python-Script für Dify API
# - Authentication
# - Chat Completion
# - Stream Response
# - Error Handling

# Copilot generiert vollständigen Client
```

### GitBook Dokumentation
Copilot im Markdown-Editor:
- Strukturierung
- Code-Beispiele
- Diagramme (Mermaid)
- Cross-References

## Pro-Tipps

1. **Language-Specific Settings:** In VS Code kannst du Copilot pro Sprache konfigurieren
2. **Disable für sensible Files:** `.env`, `secrets.json` von Copilot ausschliessen
3. **Copilot Labs:** Teste experimentelle Features (Brushes, Test Generation)
4. **GitHub Copilot CLI:** `gh copilot suggest "git command to..."` im Terminal

## Ressourcen

- **Offizielle Docs:** https://docs.github.com/copilot
- **VS Code Extension:** https://marketplace.visualstudio.com/items?itemName=GitHub.copilot
- **Pricing:** https://github.com/features/copilot#pricing
- **Copilot Chat Docs:** https://docs.github.com/en/copilot/using-github-copilot/asking-github-copilot-questions-in-your-ide

## Dein Action Plan

**Heute:**
- [ ] Extension installieren & aktivieren
- [ ] Erstes Projekt öffnen und mit Tab-Completion experimentieren
- [ ] Chat-Feature mit `/explain` und `/tests` ausprobieren

**Diese Woche:**
- [ ] Context Files erstellen für deine Hauptprojekte
- [ ] 3 häufige Coding-Tasks an Copilot delegieren
- [ ] Workflow-Integration testen (n8n, Dify)

**Diesen Monat:**
- [ ] Eigene Copilot-Instructions verfeinern
- [ ] Team-Best-Practices dokumentieren
- [ ] Produktivitätsgewinn messen

---

**Remember:** GitHub Copilot ist ein Werkzeug, kein Ersatz für dein Können. Nutze es, um schneller zu sein, aber bleib kritisch und verstehe, was es tut.

*Viel Erfolg mit GitHub Copilot! 🚀*
