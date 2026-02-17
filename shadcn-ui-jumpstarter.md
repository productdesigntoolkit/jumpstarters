# shadcn/ui Einführung & Cheatsheet

> Dein praxisorientierter Einstieg in shadcn/ui: von den Grundlagen des Technologie-Stacks über Installation und Komponenten bis hin zu fortgeschrittenen Patterns und dem wachsenden Ökosystem.

---

## Kapitel 1: Was ist shadcn/ui?

### 1.1 Das Konzept: Keine Library, sondern ein Distributions-System

shadcn/ui bricht mit dem klassischen Modell von UI-Bibliotheken. Statt ein npm-Paket zu installieren, das als Blackbox in deinem `node_modules` Ordner landet, kopiert shadcn/ui den Quellcode einzelner Komponenten direkt in dein Projekt. Du besitzt jede Zeile Code. Du kannst jede Komponente öffnen, lesen, anpassen und erweitern.

Das bedeutet konkret: Es gibt keine Versionskonflikte mit der Library. Es gibt keine Spezifizitätskämpfe mit vorgegebenen Styles. Es gibt keine Runtime-Dependencies, die dein Bundle aufblähen. Der Code gehört dir und verhält sich wie jeder andere Code in deinem Projekt.

shadcn/ui stellt dafür eine CLI bereit, die den Kopiervorgang automatisiert. Du wählst eine Komponente aus, die CLI legt die Datei in deinem Projekt ab und installiert allfällige Abhängigkeiten (z.B. Radix UI Primitives). Ab diesem Moment ist die Komponente Teil deines Codes.

### 1.2 Kernphilosophie: Copy & Own

Die Philosophie lässt sich in drei Prinzipien zusammenfassen:

**Open Code**: Jede Komponente ist vollständig einsehbar. Kein verstecktes Styling, keine magischen Overrides. Du siehst exakt, was passiert.

**Full Ownership**: Nach dem Hinzufügen gehört der Code dir. Du kannst ihn refactoren, erweitern oder komplett umschreiben. Es gibt keinen Vendor Lock-in.

**Composable**: Die Komponenten sind als Bausteine konzipiert. Du kombinierst sie frei und baust darauf auf, statt gegen eine API zu arbeiten.

### 1.3 Vergleich mit klassischen UI-Libraries

| Aspekt | shadcn/ui | MUI (Material UI) | Chakra UI |
|--------|-----------|-------------------|-----------|
| **Distribution** | Code wird kopiert | npm-Paket | npm-Paket |
| **Styling** | Tailwind CSS Klassen | Emotion/Styled Components | Prop-basiertes Styling |
| **Anpassung** | Direkt im Code | Theme Overrides, `sx` Prop | Theme + Props |
| **Bundle Size** | Nur was du nutzt | Gross, Tree-Shaking nötig | Mittel |
| **Updates** | Manuell (dein Code) | npm update | npm update |
| **Design System** | Du definierst es | Material Design | Eigenes System |
| **Accessibility** | Via Radix UI | Eingebaut | Eingebaut |

Der zentrale Trade-off ist klar: Du gibst automatische Updates auf und gewinnst dafür vollständige Kontrolle. Für die meisten professionellen Projekte ist das ein lohnender Tausch, weil die technischen Schulden klassischer Component Libraries über die Zeit erheblich werden können.

### 1.4 Wer steckt dahinter?

shadcn/ui wurde von shadcn (Shadid Haque) entwickelt, der bei Vercel arbeitet. Das Projekt ist Open Source unter MIT-Lizenz und hat über 100'000 GitHub-Stars. Unternehmen wie OpenAI, Sonos und Adobe setzen shadcn/ui produktiv ein. Die enge Verbindung zu Vercel zeigt sich auch in der hervorragenden Next.js-Integration und in Tools wie v0, dem AI-gestützten UI-Generator von Vercel.

---

## Kapitel 2: Der Technologie-Stack verstehen

shadcn/ui baut auf vier Kerntechnologien auf. Jede davon übernimmt eine spezifische Rolle. Dieses Kapitel erklärt dir, was jede Technologie mitbringt und wie sie im Zusammenspiel funktionieren.

### 2.1 React: Die Basis für Komponenten

React ist eine JavaScript-Bibliothek von Meta für den Aufbau von Benutzeroberflächen. Alles in React dreht sich um **Komponenten**: eigenständige, wiederverwendbare UI-Bausteine, die ihren eigenen Zustand und ihre eigene Logik kapseln.

Ein einfaches Beispiel:

```tsx
function Greeting({ name }: { name: string }) {
  return <h1>Hallo, {name}!</h1>;
}
```

Zentrale React-Konzepte, die du für shadcn/ui kennen solltest:

**Props**: Daten, die du an Komponenten übergibst. shadcn/ui-Komponenten nutzen Props für Varianten, Grössen und Verhalten.

**Hooks**: Funktionen wie `useState` und `useEffect`, mit denen Komponenten Zustand verwalten und Seiteneffekte auslösen. Viele shadcn/ui-Komponenten nutzen intern Hooks.

**JSX/TSX**: Die Syntax, mit der du HTML-ähnlichen Code in JavaScript/TypeScript schreibst. Jede shadcn/ui-Komponente ist eine `.tsx`-Datei.

**Composition**: React fördert das Zusammensetzen von Komponenten. shadcn/ui nutzt dieses Pattern intensiv. Ein Dialog besteht z.B. aus `Dialog`, `DialogTrigger`, `DialogContent`, `DialogHeader` und `DialogFooter`.

```tsx
<Dialog>
  <DialogTrigger asChild>
    <Button>Öffnen</Button>
  </DialogTrigger>
  <DialogContent>
    <DialogHeader>
      <DialogTitle>Titel</DialogTitle>
    </DialogHeader>
    <p>Inhalt des Dialogs</p>
  </DialogContent>
</Dialog>
```

### 2.2 TypeScript: Typsicherheit und Entwickler-Erfahrung

TypeScript ist eine Erweiterung von JavaScript, die statische Typisierung hinzufügt. Das bedeutet: Der Editor weiss, welche Props eine Komponente akzeptiert, und warnt dich, bevor du einen Fehler machst.

Für shadcn/ui ist TypeScript aus mehreren Gründen zentral:

**Autocompletion**: Dein Editor schlägt dir automatisch alle verfügbaren Props, Varianten und Optionen vor. Du musst nicht in die Dokumentation schauen.

**Fehlererkennung**: Tippfehler in Prop-Namen oder falsche Typen werden sofort markiert. Statt eines Runtime-Fehlers siehst du den Fehler direkt im Editor.

**Interfaces und Types**: shadcn/ui definiert klare Interfaces für jede Komponente. Du siehst exakt, was erwartet wird.

```tsx
// Beispiel: Button-Varianten sind typisiert
interface ButtonProps {
  variant?: "default" | "destructive" | "outline" | "secondary" | "ghost" | "link";
  size?: "default" | "sm" | "lg" | "icon";
}
```

Du musst kein TypeScript-Experte sein. Die Grundlagen reichen: Verstehe, dass nach dem Doppelpunkt ein Typ steht, dass `interface` eine Struktur beschreibt und dass `?` ein optionales Feld markiert. Der Rest ergibt sich durch die Autocompletion deines Editors.

### 2.3 Tailwind CSS: Utility-First Styling

Tailwind CSS ist ein Utility-First CSS-Framework. Statt eigene CSS-Klassen zu schreiben, kombinierst du vordefinierte Utility-Klassen direkt im HTML/JSX.

Klassisches CSS:
```css
.button {
  background-color: blue;
  color: white;
  padding: 8px 16px;
  border-radius: 4px;
}
```

Tailwind CSS:
```tsx
<button className="bg-blue-500 text-white px-4 py-2 rounded">
  Klick mich
</button>
```

Das fühlt sich anfangs ungewohnt an, hat aber grosse Vorteile: Kein Kontextwechsel zwischen HTML und CSS-Dateien. Kein Erfinden von Klassennamen. Kein ungenutztes CSS, das dein Bundle aufbläht.

Für shadcn/ui relevante Tailwind-Konzepte:

**Responsive Design**: Prefixe wie `sm:`, `md:`, `lg:` steuern Breakpoints. `md:flex` bedeutet "ab mittlerer Bildschirmbreite Flexbox verwenden".

**Dark Mode**: Der `dark:`-Prefix aktiviert Styles im Dark Mode. `dark:bg-gray-800` setzt einen dunklen Hintergrund nur im Dark Mode.

**CSS-Variablen**: shadcn/ui nutzt Tailwind in Kombination mit CSS-Variablen für Theming. Farben wie `bg-primary` oder `text-muted-foreground` verweisen auf Variablen, die du zentral anpassen kannst.

**CVA (Class Variance Authority)**: shadcn/ui verwendet CVA, um Varianten zu definieren. Das ist eine elegante Art, verschiedene Styles einer Komponente (z.B. `variant="destructive"`) auf Tailwind-Klassen abzubilden.

```tsx
const buttonVariants = cva(
  "inline-flex items-center justify-center rounded-md text-sm font-medium",
  {
    variants: {
      variant: {
        default: "bg-primary text-primary-foreground",
        destructive: "bg-destructive text-destructive-foreground",
        outline: "border border-input bg-background",
      },
      size: {
        default: "h-10 px-4 py-2",
        sm: "h-9 rounded-md px-3",
        lg: "h-11 rounded-md px-8",
      },
    },
  }
);
```

### 2.4 Radix UI: Headless Primitives für Accessibility

Radix UI ist eine Bibliothek von "headless" UI-Primitives. Headless bedeutet: Radix liefert das Verhalten und die Accessibility, aber kein visuelles Styling. Die Komponenten wissen, wie ein Dialog, ein Dropdown oder ein Tooltip funktionieren sollte (Fokus-Management, Keyboard-Navigation, ARIA-Attribute), aber sie sehen "nackt" aus.

Das ist die perfekte Ergänzung zu Tailwind CSS. Radix kümmert sich um das komplexe Verhalten; Tailwind und shadcn/ui liefern das visuelle Design.

Was Radix UI mitbringt:

**Accessibility out of the box**: Korrekte ARIA-Rollen, Keyboard-Navigation, Focus Trapping. Du musst dich nicht selbst um WAI-ARIA kümmern.

**Composition Pattern**: Radix-Komponenten sind zerlegbar. Ein `Select` besteht aus `Select.Root`, `Select.Trigger`, `Select.Content`, `Select.Item` etc. Du entscheidest, wie du sie zusammensetzt.

**Controlled & Uncontrolled**: Jede Komponente funktioniert sowohl gesteuert (du verwaltest den State) als auch ungesteuert (die Komponente verwaltet sich selbst).

**Portal-Rendering**: Overlays wie Dialoge und Tooltips werden automatisch am Ende des DOM gerendert, um Stacking-Probleme zu vermeiden.

Seit Juni 2025 importiert shadcn/ui aus dem vereinheitlichten `radix-ui` Paket statt aus einzelnen `@radix-ui/react-*` Paketen. Das vereinfacht die `package.json` erheblich.

### 2.5 Wie alles zusammenspielt

Das Zusammenspiel der vier Technologien lässt sich so visualisieren:

```
┌─────────────────────────────────────────────┐
│              Deine Anwendung                │
│                                             │
│  ┌─────────────────────────────────────┐    │
│  │         shadcn/ui Komponente        │    │
│  │                                     │    │
│  │  ┌──────────┐    ┌──────────────┐   │    │
│  │  │ Radix UI │    │ Tailwind CSS │   │    │
│  │  │ Verhalten│    │   Styling    │   │    │
│  │  │ A11y     │    │   Theming    │   │    │
│  │  └──────────┘    └──────────────┘   │    │
│  │                                     │    │
│  │  ┌──────────────────────────────┐   │    │
│  │  │     React + TypeScript       │   │    │
│  │  │  Komponenten-Architektur     │   │    │
│  │  │  Typsicherheit               │   │    │
│  │  └──────────────────────────────┘   │    │
│  └─────────────────────────────────────┘    │
└─────────────────────────────────────────────┘
```

React und TypeScript bilden das Fundament: die Komponenten-Architektur und die Typsicherheit. Radix UI sitzt darauf und liefert das zugängliche, barrierefreie Verhalten. Tailwind CSS liefert das visuelle Styling. shadcn/ui verbindet alle drei Schichten zu einer fertigen, aber vollständig anpassbaren Komponente.

---

## Kapitel 3: Installation & Setup

### 3.1 Voraussetzungen

Bevor du loslegst, brauchst du:

| Voraussetzung | Minimum | Empfohlen |
|---------------|---------|-----------|
| Node.js | v18+ | v20+ (LTS) |
| Package Manager | npm | pnpm oder bun |
| Framework | React 18+ | Next.js 14+ oder Vite |
| Tailwind CSS | v3.4+ | v4 |
| TypeScript | v5+ | Aktuellste Version |

### 3.2 Neues Projekt mit shadcn/create

Der schnellste Weg zu einem neuen Projekt ist `shadcn/create`. Dieses Tool scaffoldet ein komplettes Projekt mit Theme, Komponenten und Konfiguration.

```bash
npx shadcn@latest create
```

Der interaktive Wizard fragt dich nach Framework (Next.js, Vite, TanStack Start), Icon Library, Theme und Base Color. Danach hast du ein lauffähiges Projekt mit shadcn/ui-Konfiguration.

### 3.3 Bestehendes Projekt initialisieren

Wenn du ein bestehendes React-Projekt hast, nutzt du `init`:

**Next.js:**
```bash
npx create-next-app@latest my-app --typescript --tailwind --eslint --app
cd my-app
npx shadcn@latest init
```

**Vite:**
```bash
npm create vite@latest my-app -- --template react-ts
cd my-app
npm install
npx shadcn@latest init
```

Der `init`-Befehl macht drei Dinge: Er erstellt die Konfigurationsdatei `components.json`, aktualisiert deine `globals.css` mit CSS-Variablen für das Theming und erstellt die Hilfsfunktion `cn()` unter `lib/utils.ts`.

### 3.4 Die Konfiguration: components.json

Nach dem Init findest du im Projektstamm eine `components.json`. Diese Datei steuert, wie die CLI Komponenten in dein Projekt einfügt.

```json
{
  "$schema": "https://ui.shadcn.com/schema.json",
  "style": "new-york",
  "rsc": false,
  "tsx": true,
  "tailwind": {
    "config": "",
    "css": "src/styles/globals.css",
    "baseColor": "neutral",
    "cssVariables": true,
    "prefix": ""
  },
  "aliases": {
    "components": "@/components",
    "utils": "@/lib/utils",
    "ui": "@/components/ui",
    "lib": "@/lib",
    "hooks": "@/hooks"
  },
  "iconLibrary": "lucide"
}
```

Die wichtigsten Einstellungen:

**style**: `"new-york"` ist der aktuelle Standard-Stil.

**rsc**: Ob React Server Components verwendet werden (relevant für Next.js App Router).

**aliases**: Wo die Komponenten abgelegt werden. `@/components/ui` ist der Standard-Ordner für UI-Komponenten.

**baseColor**: Die Grundfarbe deines Themes (neutral, gray, zinc, stone, slate).

**iconLibrary**: Standard ist Lucide React, eine schlanke Icon-Bibliothek.

### 3.5 Erste Komponente hinzufügen

Jetzt kannst du Komponenten hinzufügen:

```bash
npx shadcn@latest add button
```

Dieser Befehl erstellt die Datei `components/ui/button.tsx` in deinem Projekt und installiert die nötigen Abhängigkeiten. Du kannst die Datei sofort öffnen und den Code inspizieren.

Verwendung in deiner App:

```tsx
import { Button } from "@/components/ui/button";

export default function MyPage() {
  return (
    <div className="p-8">
      <Button variant="default">Primär</Button>
      <Button variant="outline">Outline</Button>
      <Button variant="destructive">Löschen</Button>
    </div>
  );
}
```

Mehrere Komponenten auf einmal:

```bash
npx shadcn@latest add button card dialog input label
```

Alle Komponenten auf einmal:

```bash
npx shadcn@latest add --all
```

### 3.6 Die cn() Hilfsfunktion

Die Datei `lib/utils.ts` enthält die zentrale Hilfsfunktion `cn()`:

```tsx
import { clsx, type ClassValue } from "clsx";
import { twMerge } from "tailwind-merge";

export function cn(...inputs: ClassValue[]) {
  return twMerge(clsx(inputs));
}
```

`cn()` kombiniert zwei Libraries: `clsx` für das bedingte Zusammensetzen von Klassennamen und `tailwind-merge` für das intelligente Zusammenführen von Tailwind-Klassen (damit `px-4` und `px-8` nicht beide im Output landen, sondern nur die letzte gewinnt).

```tsx
// Beispiel: Bedingte Klassen
<div className={cn(
  "rounded-lg border p-4",
  isActive && "border-primary bg-primary/10",
  isDisabled && "opacity-50 cursor-not-allowed"
)} />
```

---

## Kapitel 4: Komponenten im Alltag

### 4.1 Übersicht der wichtigsten Komponenten

shadcn/ui bietet über 40 Kern-Komponenten. Hier die wichtigsten nach Kategorie:

| Kategorie | Komponenten | Typischer Einsatz |
|-----------|-------------|-------------------|
| **Layout** | Card, Separator, Resizable, Aspect Ratio | Inhalte strukturieren, Bereiche trennen |
| **Navigation** | Navigation Menu, Menubar, Breadcrumb, Tabs, Sidebar, Pagination | Seitennavigation, Tab-Interfaces |
| **Eingabe** | Input, Textarea, Checkbox, Radio Group, Select, Switch, Slider, Calendar, Date Picker | Formulare, Einstellungen |
| **Feedback** | Alert, Alert Dialog, Dialog, Drawer, Sheet, Sonner (Toast), Progress, Skeleton | Benachrichtigungen, Ladezustände |
| **Overlay** | Popover, Tooltip, Hover Card, Context Menu, Dropdown Menu, Command | Kontextuelle Information, Menüs |
| **Daten** | Table, Data Table, Badge, Avatar, Scroll Area | Daten darstellen, Listen |
| **Formular** | Form, Label, Input OTP | Formulare mit Validierung |

### 4.2 Komponenten hinzufügen und anpassen

Das Hinzufügen ist immer gleich:

```bash
npx shadcn@latest add [komponente]
```

Danach findest du die Datei unter `components/ui/[komponente].tsx`. Du kannst sie direkt bearbeiten. Ein Beispiel: Du möchtest eine neue Button-Variante "success" hinzufügen.

Öffne `components/ui/button.tsx` und ergänze die Varianten:

```tsx
const buttonVariants = cva(
  "inline-flex items-center justify-center ...",
  {
    variants: {
      variant: {
        default: "bg-primary text-primary-foreground ...",
        destructive: "bg-destructive text-destructive-foreground ...",
        // Neue Variante:
        success: "bg-green-600 text-white hover:bg-green-700",
      },
      // ...
    },
  }
);
```

Fertig. Keine Pull Requests an eine Library, kein Warten auf Releases.

### 4.3 Theming mit CSS-Variablen

shadcn/ui nutzt CSS-Variablen für das gesamte Farbsystem. Diese findest du in deiner `globals.css`:

```css
@layer base {
  :root {
    --background: 0 0% 100%;
    --foreground: 0 0% 3.9%;
    --primary: 0 0% 9%;
    --primary-foreground: 0 0% 98%;
    --secondary: 0 0% 96.1%;
    --muted: 0 0% 96.1%;
    --muted-foreground: 0 0% 45.1%;
    --accent: 0 0% 96.1%;
    --destructive: 0 84.2% 60.2%;
    --border: 0 0% 89.8%;
    --radius: 0.5rem;
  }

  .dark {
    --background: 0 0% 3.9%;
    --foreground: 0 0% 98%;
    /* ... weitere Dark-Mode-Werte */
  }
}
```

Die Werte sind im HSL-Format (Hue, Saturation, Lightness) ohne die `hsl()`-Funktion. Du änderst dein komplettes Farbschema, indem du diese Variablen anpasst. Light Mode und Dark Mode sind über `:root` und `.dark` getrennt.

In Tailwind-Klassen werden diese Variablen dann so verwendet: `bg-primary`, `text-muted-foreground`, `border-border`. Das Theming ist dadurch zentral und konsistent.

### 4.4 Blocks: Vorgefertigte UI-Abschnitte

Neben einzelnen Komponenten bietet shadcn/ui auch "Blocks". Das sind grössere, zusammengesetzte UI-Abschnitte wie Dashboards, Login-Seiten, Einstellungsseiten oder Daten-Tabellen. Du findest sie unter `https://ui.shadcn.com/blocks`.

Blocks sind keine Komponenten im klassischen Sinn, sondern Vorlagen. Du kopierst sie als Ausgangspunkt und passt sie an deine Bedürfnisse an.

### 4.5 Integration mit React Hook Form und Zod

Für Formulare bietet shadcn/ui eine nahtlose Integration mit React Hook Form (Formular-Management) und Zod (Schema-Validierung).

```bash
npx shadcn@latest add form input label
```

Beispiel eines validierten Formulars:

```tsx
import { z } from "zod";
import { useForm } from "react-hook-form";
import { zodResolver } from "@hookform/resolvers/zod";
import { Form, FormControl, FormField, FormItem, FormLabel, FormMessage } from "@/components/ui/form";
import { Input } from "@/components/ui/input";
import { Button } from "@/components/ui/button";

const schema = z.object({
  email: z.string().email("Ungültige E-Mail-Adresse"),
  name: z.string().min(2, "Mindestens 2 Zeichen"),
});

export function ContactForm() {
  const form = useForm<z.infer<typeof schema>>({
    resolver: zodResolver(schema),
    defaultValues: { email: "", name: "" },
  });

  function onSubmit(values: z.infer<typeof schema>) {
    console.log(values);
  }

  return (
    <Form {...form}>
      <form onSubmit={form.handleSubmit(onSubmit)} className="space-y-4">
        <FormField
          control={form.control}
          name="name"
          render={({ field }) => (
            <FormItem>
              <FormLabel>Name</FormLabel>
              <FormControl>
                <Input placeholder="Dein Name" {...field} />
              </FormControl>
              <FormMessage />
            </FormItem>
          )}
        />
        <FormField
          control={form.control}
          name="email"
          render={({ field }) => (
            <FormItem>
              <FormLabel>E-Mail</FormLabel>
              <FormControl>
                <Input placeholder="name@beispiel.ch" {...field} />
              </FormControl>
              <FormMessage />
            </FormItem>
          )}
        />
        <Button type="submit">Absenden</Button>
      </form>
    </Form>
  );
}
```

Das `Form`-Komponentensystem von shadcn/ui kümmert sich um Labels, Fehlermeldungen, Accessibility und die Verknüpfung zwischen Zod-Schema und Formularfeldern.

---

## Kapitel 5: Fortgeschrittene Nutzung & Ökosystem

### 5.1 CLI-Befehle im Detail

Die shadcn CLI bietet mehrere Befehle:

| Befehl | Zweck |
|--------|-------|
| `shadcn init` | Projekt initialisieren, Konfiguration erstellen |
| `shadcn add [name]` | Komponente zum Projekt hinzufügen |
| `shadcn add --all` | Alle Komponenten hinzufügen |
| `shadcn create` | Neues Projekt mit Wizard erstellen |
| `shadcn build` | Registry aus deinen Komponenten bauen |
| `shadcn migrate [migration]` | Migrationen ausführen (z.B. Radix, RTL) |

Nützliche Flags für `add`:

```bash
# Ohne Bestätigungsdialog
npx shadcn@latest add button -y

# Bestehende Dateien überschreiben
npx shadcn@latest add button --overwrite

# In einen bestimmten Pfad installieren
npx shadcn@latest add button --path src/components/custom
```

### 5.2 Eigene Registry erstellen

Seit 2025 kannst du deine eigenen Komponenten als Registry veröffentlichen. Das ist ideal für Design-Systeme in Unternehmen oder für Open-Source-Projekte.

Du erstellst eine `registry.json` in deinem Projekt, definierst deine Komponenten und baust sie mit `shadcn build`. Das Ergebnis ist eine JSON-Datei, die andere via CLI installieren können:

```bash
npx shadcn@latest add https://deine-domain.ch/r/custom-button.json
```

### 5.3 Das Ökosystem

Rund um shadcn/ui ist ein grosses Ökosystem entstanden:

| Projekt | Beschreibung |
|---------|-------------|
| **v0 (Vercel)** | AI-gestützter UI-Generator, der shadcn/ui-Komponenten erzeugt |
| **shadcn Studio** | Erweiterte Komponenten, Blocks, Templates und Theme-Generator |
| **Motion Primitives** | Animationskomponenten speziell für shadcn/ui |
| **shadcn/ui Figma Kit** | Figma-Design-System mit Figma-to-Code Plugin |
| **awesome-shadcn-ui** | Kuratierte Liste mit 170+ Erweiterungen und Tools |
| **FormCN** | Spezialisierte Formular-Komponenten |
| **Cult UI** | Templates mit AI-Code-Generierung |

### 5.4 RTL-Support und Migrationen

Seit Januar 2026 unterstützt shadcn/ui Right-to-Left Layouts für Sprachen wie Arabisch und Hebräisch. Die CLI transformiert dabei physische CSS-Klassen (z.B. `ml-4`) automatisch in logische Äquivalente (`ms-4`).

Neues Projekt mit RTL:
```bash
npx shadcn@latest init --rtl
```

Bestehendes Projekt migrieren:
```bash
npx shadcn@latest migrate rtl
```

Die Radix-Migration (Umstellung auf das vereinheitlichte `radix-ui` Paket) funktioniert ähnlich:
```bash
npx shadcn@latest migrate radix
```

### 5.5 MCP-Server und AI-Integration

shadcn/ui bietet seit 2025 einen MCP-Server (Model Context Protocol). Damit können AI-Tools wie Claude, Cursor oder andere MCP-fähige Clients direkt auf die shadcn/ui-Komponentenbibliothek zugreifen. Das ermöglicht es, Komponenten per natürlichsprachlicher Beschreibung zu generieren und direkt ins Projekt einzufügen.

Auch v0 von Vercel nutzt shadcn/ui als Grundlage. Du beschreibst ein UI-Element, und v0 generiert den Code mit shadcn/ui-Komponenten.

### 5.6 Best Practices

**Komponentenstruktur**: Halte die `components/ui/`-Dateien als "Primitives". Baue darauf eigene, anwendungsspezifische Komponenten in `components/`:

```
components/
├── ui/           ← shadcn/ui Primitives (Button, Dialog, etc.)
│   ├── button.tsx
│   ├── dialog.tsx
│   └── ...
├── user-card.tsx  ← Deine zusammengesetzten Komponenten
├── nav-bar.tsx
└── settings-form.tsx
```

**Theming zentral halten**: Alle Farbanpassungen gehören in die CSS-Variablen deiner `globals.css`. Vermeide es, Farben direkt in Komponenten hart zu codieren.

**Nicht alle Komponenten installieren**: Füge nur hinzu, was du brauchst. Jede Komponente, die du nicht nutzt, ist toter Code.

**Git Tracking**: Da die Komponenten Teil deines Codes sind, werden sie von Git versioniert. Änderungen an Komponenten sind im Diff sichtbar und reviewbar.

---

# shadcn/ui Cheatsheet

## Seite 1: CLI-Befehle & Setup

### Projekt-Setup

```bash
# Neues Projekt (interaktiver Wizard)
npx shadcn@latest create

# Bestehendes Projekt initialisieren
npx shadcn@latest init

# Init mit RTL-Support
npx shadcn@latest init --rtl

# Init mit spezifischer Base Color
npx shadcn@latest init -b zinc
```

### Komponenten verwalten

```bash
# Einzelne Komponente hinzufügen
npx shadcn@latest add button

# Mehrere Komponenten
npx shadcn@latest add button card dialog input

# Alle Komponenten
npx shadcn@latest add --all

# Ohne Bestätigung
npx shadcn@latest add button -y

# Überschreiben erzwingen
npx shadcn@latest add button --overwrite

# Von externer Registry
npx shadcn@latest add https://example.com/r/component.json
```

### Migrationen

```bash
# Radix UI Migration (einzelne → unified)
npx shadcn@latest migrate radix

# RTL Migration
npx shadcn@latest migrate rtl

# Migration auf bestimmten Pfad
npx shadcn@latest migrate radix "src/components/ui/**"

# Alle Migrationen auflisten
npx shadcn@latest migrate --list
```

### components.json Kurzreferenz

| Feld | Werte | Beschreibung |
|------|-------|-------------|
| `style` | `"new-york"` | Visueller Stil |
| `rsc` | `true/false` | React Server Components |
| `tsx` | `true/false` | TypeScript verwenden |
| `tailwind.baseColor` | `neutral, gray, zinc, stone, slate` | Grundfarbe |
| `tailwind.cssVariables` | `true/false` | CSS-Variablen nutzen |
| `tailwind.prefix` | `""` | Tailwind-Klassen-Prefix |
| `aliases.ui` | `"@/components/ui"` | Ablageort der UI-Komponenten |
| `iconLibrary` | `"lucide"` | Icon-Bibliothek |

---

## Seite 2: Komponenten-Übersicht

### Layout & Struktur

| Komponente | Befehl | Einsatz |
|------------|--------|---------|
| Aspect Ratio | `add aspect-ratio` | Seitenverhältnis für Medien |
| Card | `add card` | Container für Inhaltsblöcke |
| Resizable | `add resizable` | Grössenveränderbare Panels |
| Scroll Area | `add scroll-area` | Custom Scrollbar |
| Separator | `add separator` | Visuelle Trennung |
| Collapsible | `add collapsible` | Ein-/ausklappbare Bereiche |

### Navigation

| Komponente | Befehl | Einsatz |
|------------|--------|---------|
| Breadcrumb | `add breadcrumb` | Pfadnavigation |
| Menubar | `add menubar` | Horizontale Menüleiste |
| Navigation Menu | `add navigation-menu` | Hauptnavigation |
| Pagination | `add pagination` | Seitennavigation |
| Sidebar | `add sidebar` | Seitenleiste |
| Tabs | `add tabs` | Tab-basierte Navigation |

### Eingabe & Formular

| Komponente | Befehl | Einsatz |
|------------|--------|---------|
| Button | `add button` | Aktionen auslösen |
| Calendar | `add calendar` | Datumsauswahl |
| Checkbox | `add checkbox` | Mehrfachauswahl |
| Form | `add form` | Formular mit Validierung |
| Input | `add input` | Texteingabe |
| Input OTP | `add input-otp` | Einmalpasswort-Eingabe |
| Label | `add label` | Beschriftung für Felder |
| Radio Group | `add radio-group` | Einfachauswahl |
| Select | `add select` | Dropdown-Auswahl |
| Slider | `add slider` | Wertebereich |
| Switch | `add switch` | An/Aus-Schalter |
| Textarea | `add textarea` | Mehrzeilige Texteingabe |
| Toggle | `add toggle` | Umschaltfläche |

### Feedback & Overlay

| Komponente | Befehl | Einsatz |
|------------|--------|---------|
| Alert | `add alert` | Hinweismeldung |
| Alert Dialog | `add alert-dialog` | Bestätigungsdialog |
| Dialog | `add dialog` | Modales Fenster |
| Drawer | `add drawer` | Schublade (von unten) |
| Sheet | `add sheet` | Seitliches Panel |
| Sonner | `add sonner` | Toast-Benachrichtigungen |
| Progress | `add progress` | Fortschrittsbalken |
| Skeleton | `add skeleton` | Ladezustand-Platzhalter |
| Tooltip | `add tooltip` | Kurzinfo bei Hover |

### Daten & Anzeige

| Komponente | Befehl | Einsatz |
|------------|--------|---------|
| Avatar | `add avatar` | Profilbild |
| Badge | `add badge` | Status-Label |
| Table | `add table` | Datentabelle |
| Hover Card | `add hover-card` | Vorschau bei Hover |
| Command | `add command` | Befehlspalette (⌘K) |
| Context Menu | `add context-menu` | Rechtsklick-Menü |
| Dropdown Menu | `add dropdown-menu` | Aktionsmenü |
| Popover | `add popover` | Schwebendes Panel |

---

## Seite 3: Theming & Styling

### CSS-Variablen Referenz

| Variable | Verwendung | Tailwind-Klasse |
|----------|-----------|-----------------|
| `--background` | Seitenhintergrund | `bg-background` |
| `--foreground` | Haupttextfarbe | `text-foreground` |
| `--primary` | Primärfarbe (Buttons, Links) | `bg-primary`, `text-primary` |
| `--primary-foreground` | Text auf Primärfarbe | `text-primary-foreground` |
| `--secondary` | Sekundärfarbe | `bg-secondary` |
| `--muted` | Gedämpfter Hintergrund | `bg-muted` |
| `--muted-foreground` | Gedämpfter Text | `text-muted-foreground` |
| `--accent` | Akzentfarbe (Hover) | `bg-accent` |
| `--destructive` | Fehler/Löschen | `bg-destructive` |
| `--border` | Rahmenfarbe | `border-border` |
| `--input` | Input-Rahmen | `border-input` |
| `--ring` | Fokus-Ring | `ring-ring` |
| `--radius` | Eckenradius | `rounded-[--radius]` |

### Dark Mode einrichten

```tsx
// In deinem Root Layout (z.B. mit next-themes)
import { ThemeProvider } from "next-themes";

<ThemeProvider attribute="class" defaultTheme="system" enableSystem>
  {children}
</ThemeProvider>
```

Die CSS-Variablen unter `.dark { }` werden automatisch aktiviert, wenn die Klasse `dark` auf dem `<html>`-Element gesetzt ist.

### Die cn() Funktion

```tsx
import { cn } from "@/lib/utils";

// Klassen zusammenführen
cn("px-4 py-2", "px-8")
// → "px-8 py-2" (tailwind-merge löst Konflikte)

// Bedingte Klassen
cn("base-class", isActive && "active-class")
// → "base-class active-class" oder "base-class"

// Objekt-Syntax via clsx
cn("base", { "font-bold": isBold, "text-red-500": hasError })
```

### Button-Varianten Kurzreferenz

```tsx
<Button variant="default">Primär</Button>
<Button variant="secondary">Sekundär</Button>
<Button variant="outline">Outline</Button>
<Button variant="ghost">Ghost</Button>
<Button variant="link">Link</Button>
<Button variant="destructive">Löschen</Button>

<Button size="sm">Klein</Button>
<Button size="default">Standard</Button>
<Button size="lg">Gross</Button>
<Button size="icon"><Icon /></Button>
```

---

## Seite 4: Best Practices & Ökosystem

### Do's ✓

| Praxis | Warum |
|--------|-------|
| Nur benötigte Komponenten installieren | Kein toter Code, übersichtliches Projekt |
| Farben über CSS-Variablen steuern | Konsistentes Theming, einfacher Dark Mode |
| Eigene Komponenten auf shadcn/ui aufbauen | Separation zwischen Primitives und App-Logik |
| `cn()` für bedingte Klassen verwenden | Saubere Tailwind-Merge-Logik |
| Komponenten-Code lesen und verstehen | Du besitzt den Code; nutze diesen Vorteil |
| Zod + React Hook Form für Formulare | Typsichere Validierung out of the box |

### Don'ts ✗

| Vermeiden | Stattdessen |
|-----------|-------------|
| Farben direkt in Komponenten hart codieren | CSS-Variablen in `globals.css` verwenden |
| shadcn/ui-Dateien in `ui/` mit App-Logik mischen | Eigene Komponenten in separatem Ordner |
| Alle Komponenten pauschal installieren | Nur installieren, was du brauchst |
| Radix-Primitive direkt verwenden | shadcn/ui-Wrapper nutzen (konsistenter) |
| CSS-Variablen-Werte mit `hsl()` wrappen | Nur die HSL-Werte ohne Funktion angeben |

### Nützliche Drittanbieter-Libraries

| Library | Zweck | Zusammenspiel |
|---------|-------|---------------|
| **next-themes** | Dark/Light Mode Toggle | Standard für Next.js + shadcn/ui |
| **react-hook-form** | Formular-Management | Direkte Integration via Form-Komponente |
| **zod** | Schema-Validierung | Typsicheres Zusammenspiel mit Forms |
| **@tanstack/react-table** | Datentabellen | Basis für die Data Table Komponente |
| **lucide-react** | Icons | Standard-Icon-Library in shadcn/ui |
| **cmdk** | Befehlspalette | Basis für die Command-Komponente |
| **vaul** | Drawer | Basis für die Drawer-Komponente |
| **sonner** | Toast-Notifications | Basis für die Sonner-Komponente |
| **embla-carousel** | Karussell | Basis für die Carousel-Komponente |

### Troubleshooting

**Problem: Komponente wird nicht gefunden nach `add`**
```
→ Prüfe den aliases.ui Pfad in components.json
→ Stelle sicher, dass der Import-Pfad stimmt: @/components/ui/button
→ Starte den Dev-Server neu
```

**Problem: Tailwind-Klassen greifen nicht**
```
→ Prüfe, ob der content-Pfad in tailwind.config.ts korrekt ist
→ Stelle sicher, dass globals.css importiert wird
→ Bei Tailwind v4: Prüfe die @import-Anweisungen
```

**Problem: Dark Mode funktioniert nicht**
```
→ Stelle sicher, dass die .dark CSS-Variablen definiert sind
→ Prüfe, ob next-themes (oder ähnliches) korrekt konfiguriert ist
→ Das class-Attribut auf <html> muss "dark" enthalten
```

**Problem: TypeScript-Fehler bei Komponenten-Props**
```
→ Prüfe die TypeScript-Version (v5+ empfohlen)
→ Stelle sicher, dass tsconfig.json die Pfad-Aliases korrekt definiert
→ Führe npx tsc --noEmit aus, um alle Fehler zu sehen
```

### Nützliche Ressourcen

| Ressource | URL |
|-----------|-----|
| Offizielle Dokumentation | https://ui.shadcn.com/docs |
| Komponenten-Übersicht | https://ui.shadcn.com/docs/components |
| Blocks (vorgefertigte Seiten) | https://ui.shadcn.com/blocks |
| Charts | https://ui.shadcn.com/charts |
| GitHub Repository | https://github.com/shadcn-ui/ui |
| awesome-shadcn-ui | https://github.com/birobirobiro/awesome-shadcn-ui |
| shadcn Studio | https://shadcnstudio.com |
| v0 (AI UI Generator) | https://v0.dev |
| Figma Design Kit | https://www.shadcndesign.com |
| Changelog | https://ui.shadcn.com/docs/changelog |

---

*Erstellt für den praktischen Einsatz | Version 1.0 | Februar 2026*
