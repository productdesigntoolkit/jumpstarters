# Bilder kreieren mit AI - Flux Model Einführung & Cheatsheet

> Eine praxisorientierte Anleitung für die Bildgenerierung mit dem Flux-Modell von Black Forest Labs

---

## Kapitel 1: Was ist Flux?

### 1.1 Überblick

Flux ist ein fortschrittliches AI-Modell zur Bildgenerierung, entwickelt von Black Forest Labs – einem Team ehemaliger Stability AI-Mitarbeiter. Das Modell wurde im August 2024 veröffentlicht und hat sich schnell als eines der leistungsfähigsten Open-Source-Bildgenerierungsmodelle etabliert.

Was Flux besonders auszeichnet, ist die herausragende Qualität bei der Darstellung von Text in Bildern, die akkurate Wiedergabe menschlicher Anatomie (insbesondere Hände und Gesichter) sowie die Fähigkeit, komplexe Prompts präzise umzusetzen. Im Vergleich zu Vorgängermodellen wie Stable Diffusion erreicht Flux eine deutlich höhere Bildqualität und Prompt-Treue.

### 1.2 Black Forest Labs und die Entstehung

Black Forest Labs wurde 2024 von Robin Rombach und anderen Schlüsselfiguren der generativen AI-Szene gegründet. Das Team bringt tiefgreifende Erfahrung aus der Entwicklung von Stable Diffusion mit. Der Name "Flux" steht für "Flow" – ein Hinweis auf die zugrundeliegende Flow-Matching-Architektur, die das Modell von klassischen Diffusionsmodellen unterscheidet.

Die Firma verfolgt einen Open-Source-Ansatz, wobei verschiedene Modellvarianten mit unterschiedlichen Lizenzen angeboten werden.

### 1.3 Flux-Varianten im Überblick

Flux ist in drei Hauptvarianten verfügbar, die sich in Leistung, Geschwindigkeit und Lizenzierung unterscheiden:

| Variante | Parameter | Geschwindigkeit | Qualität | Lizenz |
|----------|-----------|-----------------|----------|--------|
| **Flux.1 Pro** | 12B | Langsam (~25s) | Höchste | Proprietär (API) |
| **Flux.1 Dev** | 12B | Mittel (~12s) | Sehr hoch | Open (nicht-kommerziell) |
| **Flux.1 Schnell** | 12B | Schnell (~2s) | Hoch | Apache 2.0 (kommerziell) |

**Flux.1 Pro** ist das Flaggschiff-Modell, nur über API verfügbar. Es liefert die beste Bildqualität und höchste Prompt-Treue.

**Flux.1 Dev** ist die destillierte Version von Pro. Ideal für Experimente und nicht-kommerzielle Projekte. Die Gewichte sind frei verfügbar.

**Flux.1 Schnell** ist optimiert für Geschwindigkeit. Benötigt nur 1-4 Steps für gute Ergebnisse. Kommerziell nutzbar.

### 1.4 Vergleich zu anderen Modellen

| Aspekt | Flux | Midjourney v6 | DALL-E 3 | SD XL |
|--------|------|---------------|----------|-------|
| **Text in Bildern** | Exzellent | Gut | Sehr gut | Schwach |
| **Anatomie/Hände** | Sehr gut | Sehr gut | Gut | Mittel |
| **Fotorealismus** | Exzellent | Exzellent | Gut | Gut |
| **Prompt-Treue** | Sehr hoch | Hoch | Sehr hoch | Mittel |
| **Open Source** | Ja (Dev/Schnell) | Nein | Nein | Ja |
| **Lokale Nutzung** | Ja | Nein | Nein | Ja |
| **Kosten** | Variabel | Abo | Pay-per-Use | Kostenlos |

---

## Kapitel 2: Zugang und Setup

### 2.1 Cloud-Optionen

Der einfachste Einstieg ist über Cloud-Plattformen, die Flux als Service anbieten:

**Replicate**
- URL: replicate.com
- Alle Flux-Varianten verfügbar
- Pay-per-Use (~$0.003-0.05 pro Bild)
- API und Web-Interface

**fal.ai**
- URL: fal.ai
- Sehr schnelle Inferenz
- Günstige Preise
- Einfache API

**Together AI**
- URL: together.ai
- Flux Pro und Dev
- Competitive Pricing
- Gute Dokumentation

**Freepik Flux**
- URL: freepik.com/ai/flux
- Benutzerfreundliches Interface
- Keine technischen Kenntnisse nötig
- Freemium-Modell

### 2.2 Hardware-Anforderungen für lokale Nutzung

Für die lokale Ausführung von Flux benötigst du eine leistungsstarke GPU:

| Variante | Minimaler VRAM | Empfohlen | Generierungszeit |
|----------|---------------|-----------|------------------|
| Flux Schnell | 12 GB | 16+ GB | 2-5 Sekunden |
| Flux Dev | 16 GB | 24+ GB | 10-20 Sekunden |
| Flux Dev (quantisiert) | 8 GB | 12+ GB | 15-30 Sekunden |

**Empfohlene GPUs:**
- NVIDIA RTX 4090 (24 GB) – Optimal
- NVIDIA RTX 4080 (16 GB) – Gut für Schnell
- NVIDIA RTX 3090 (24 GB) – Gut
- Apple M2/M3 Ultra – Funktioniert, aber langsamer

### 2.3 Lokale Installation mit ComfyUI

ComfyUI ist das empfohlene Interface für die lokale Flux-Nutzung:

```bash
# 1. ComfyUI klonen
git clone https://github.com/comfyanonymous/ComfyUI.git
cd ComfyUI

# 2. Abhängigkeiten installieren
pip install -r requirements.txt

# 3. Flux-Modell herunterladen
# Platziere die Dateien in: ComfyUI/models/unet/
# - flux1-dev.safetensors oder
# - flux1-schnell.safetensors

# 4. VAE herunterladen (in models/vae/)
# - ae.safetensors

# 5. CLIP-Modelle (in models/clip/)
# - clip_l.safetensors
# - t5xxl_fp16.safetensors (oder fp8 für weniger VRAM)

# 6. ComfyUI starten
python main.py
```

### 2.4 API-Zugang einrichten

Für die programmatische Nutzung über Replicate:

```python
import replicate

# API-Key als Umgebungsvariable setzen
# export REPLICATE_API_TOKEN="r8_..."

output = replicate.run(
    "black-forest-labs/flux-dev",
    input={
        "prompt": "A serene mountain landscape at sunset",
        "aspect_ratio": "16:9",
        "num_outputs": 1,
        "guidance": 3.5,
        "num_inference_steps": 28
    }
)

print(output)  # URL zum generierten Bild
```

---

## Kapitel 3: Prompting für Flux

### 3.1 Grundlagen des Prompt-Aufbaus

Flux versteht natürliche Sprache sehr gut. Im Gegensatz zu älteren Modellen musst du keine kryptischen Keyword-Listen verwenden. Beschreibe einfach, was du sehen möchtest – klar und detailliert.

**Grundstruktur eines guten Prompts:**

```
[Subjekt] + [Aktion/Pose] + [Umgebung] + [Stil] + [Lichtstimmung] + [Details]
```

**Beispiel:**
```
A young woman with curly red hair reading a book in a cozy cafe, 
warm afternoon light streaming through large windows, 
photorealistic style, shallow depth of field, 
wearing a cream-colored sweater
```

### 3.2 Flux-spezifische Stärken nutzen

**Text in Bildern:**
Flux kann Text akkurat darstellen – eine Seltenheit bei AI-Modellen.

```
A vintage coffee shop sign that reads "BREW & BOOKS" in 
elegant serif typography, hanging above a wooden door, 
golden hour lighting
```

**Komplexe Szenen:**
Flux versteht räumliche Beziehungen und kann mehrere Elemente korrekt platzieren.

```
A scientist in a white lab coat standing next to a 
large telescope, with star charts on the wall behind her 
and a cup of coffee on the desk to her left
```

**Fotorealismus:**
Für fotorealistische Ergebnisse beschreibe Kamera-Eigenschaften.

```
Portrait photograph of an elderly man with weathered skin, 
shot on Canon 5D Mark IV, 85mm f/1.4 lens, 
natural window light, high detail skin texture
```

### 3.3 Prompt-Struktur im Detail

| Element | Beschreibung | Beispiele |
|---------|--------------|-----------|
| **Subjekt** | Hauptmotiv | "A golden retriever", "An ancient temple" |
| **Aktion** | Was passiert | "running through", "illuminated by" |
| **Umgebung** | Wo/Setting | "in a misty forest", "on a busy street" |
| **Stil** | Visueller Stil | "photorealistic", "oil painting style" |
| **Licht** | Beleuchtung | "golden hour", "dramatic shadows" |
| **Kamera** | Technische Details | "wide angle", "macro shot", "bokeh" |
| **Qualität** | Qualitätsmarker | "highly detailed", "8K resolution" |

### 3.4 Stil-Keywords für verschiedene Looks

**Fotografie:**
```
photorealistic, photograph, DSLR, mirrorless, 
film photography, Kodak Portra 400, Fujifilm, 
professional photography, editorial photo
```

**Illustration/Kunst:**
```
digital illustration, concept art, matte painting,
oil painting, watercolor, pencil sketch,
anime style, Studio Ghibli, Pixar style
```

**3D/Rendering:**
```
3D render, Octane render, Unreal Engine 5,
CGI, architectural visualization, 
product rendering, isometric view
```

### 3.5 Negative Prompts

Bei Flux sind Negative Prompts weniger kritisch als bei älteren Modellen, können aber helfen:

```
Negative: blurry, low quality, distorted, 
watermark, text overlay, cropped, 
bad anatomy, extra limbs
```

**Tipp:** Flux Dev und Pro benötigen oft keine Negative Prompts. Bei Schnell können sie die Qualität verbessern.

### 3.6 Praktische Prompt-Beispiele

**Portrait:**
```
Professional headshot of a confident businesswoman 
in her 40s, wearing a navy blue blazer, 
neutral gray background, soft studio lighting, 
sharp focus on eyes, Canon EOS R5, 85mm lens
```

**Landschaft:**
```
Breathtaking view of the Swiss Alps at sunrise, 
snow-capped peaks reflecting pink and orange light, 
a small wooden cabin in the foreground, 
mist in the valley below, 
landscape photography, high dynamic range
```

**Produkt:**
```
Minimalist product photo of a matte black 
wireless headphone on a white marble surface, 
soft shadows, studio lighting, 
commercial photography style, 8K detail
```

**Fantasy:**
```
An ancient library with impossibly tall bookshelves, 
floating candles providing warm light, 
a wise owl perched on a reading stand, 
magical atmosphere, dust particles in light beams,
detailed digital art style
```

---

## Kapitel 4: Fortgeschrittene Techniken

### 4.1 Parameter verstehen

Die wichtigsten Parameter bei Flux:

**Steps (Inference Steps)**
- Anzahl der Verarbeitungsschritte
- Mehr Steps = höhere Qualität, aber langsamer
- Flux Schnell: 1-4 Steps optimal
- Flux Dev: 20-30 Steps empfohlen
- Flux Pro: 25-50 Steps

**Guidance Scale (CFG)**
- Wie stark der Prompt befolgt wird
- Flux arbeitet mit niedrigeren Werten als SD
- Empfohlen: 2.5-4.0 für Flux Dev
- Zu hoch (>5): Übergesättigte, unnatürliche Bilder

**Seed**
- Zufallszahl für Reproduzierbarkeit
- Gleicher Seed + gleicher Prompt = gleiches Bild
- Nützlich für Variationen und Iteration

**Aspect Ratio / Auflösung**
- Flux unterstützt verschiedene Formate
- Native Auflösung: 1024x1024
- Weitere: 1024x576 (16:9), 576x1024 (9:16), etc.

### 4.2 Image-to-Image mit Flux

Du kannst ein Ausgangsbild als Referenz verwenden:

```python
# Replicate API Beispiel
output = replicate.run(
    "black-forest-labs/flux-dev",
    input={
        "prompt": "Transform into a watercolor painting style",
        "image": "https://example.com/input-image.jpg",
        "prompt_strength": 0.75,  # 0.0-1.0, höher = mehr Veränderung
        "num_inference_steps": 28
    }
)
```

**Prompt Strength:**
- 0.0-0.3: Subtile Änderungen, Original bleibt erkennbar
- 0.4-0.6: Moderate Transformation
- 0.7-1.0: Starke Änderungen, nur Grundstruktur bleibt

### 4.3 ControlNet für Flux

ControlNet ermöglicht präzise Kontrolle über Komposition:

**Verfügbare ControlNet-Typen:**
- **Canny**: Kantenerkennung für Strukturen
- **Depth**: Tiefenkarten für räumliche Kontrolle
- **Pose**: Menschliche Posen übertragen
- **Line Art**: Zeichnungen als Vorlage

```
Workflow in ComfyUI:
1. Lade ControlNet-Modell für Flux
2. Verbinde Preprocessing-Node (z.B. Canny Edge)
3. Input-Bild laden
4. ControlNet-Stärke anpassen (0.5-0.8 empfohlen)
5. Prompt hinzufügen und generieren
```

### 4.4 Upscaling und Nachbearbeitung

**Integriertes Upscaling:**
Viele Flux-Implementierungen bieten 2x oder 4x Upscaling.

**Externe Upscaler:**
- **Real-ESRGAN**: Beste Qualität für Fotos
- **4x-UltraSharp**: Gut für Illustrationen
- **Topaz Gigapixel AI**: Kommerziell, sehr gut

**Workflow für maximale Qualität:**
```
1. Generiere bei nativer Auflösung (1024x1024)
2. Wähle bestes Ergebnis aus
3. Upscale mit Real-ESRGAN auf 4K
4. Optional: Nachschärfen in Photoshop/GIMP
```

### 4.5 Batch-Generierung und Workflows

Für effizientes Arbeiten:

**Seed-Variation:**
```python
base_seed = 12345
for i in range(10):
    generate(seed=base_seed + i, prompt="...")
```

**Prompt-Variation:**
```python
styles = ["photorealistic", "oil painting", "watercolor", "pencil sketch"]
for style in styles:
    generate(prompt=f"A mountain landscape, {style} style")
```

**ComfyUI Batch-Workflow:**
- Nutze den "Queue Prompt" für mehrere Generierungen
- Speichere Workflows als JSON für Wiederverwendung
- Verwende Wildcards für automatische Variationen

### 4.6 Tipps für konsistente Ergebnisse

**Für Charakterkonsistenz:**
- Detaillierte Beschreibung der Person festhalten
- Gleichen Seed-Bereich verwenden
- Reference-Image mit niedrigem Prompt-Strength nutzen

**Für Stilkonsistenz:**
- Stil-Keywords als Template speichern
- Gleiche Parameter für eine Serie verwenden
- LoRA-Modelle für spezifische Stile trainieren (fortgeschritten)

---

# Flux Cheatsheet

## Seite 1: Flux-Modelle & Zugang

### Modellvergleich

| Modell | Speed | Qualität | Steps | Guidance | Lizenz |
|--------|-------|----------|-------|----------|--------|
| **Pro** | ~25s | ★★★★★ | 25-50 | 2.5-4.0 | API only |
| **Dev** | ~12s | ★★★★☆ | 20-30 | 2.5-4.0 | Non-commercial |
| **Schnell** | ~2s | ★★★☆☆ | 1-4 | 0-2.0 | Apache 2.0 |

### Plattform-Übersicht

| Plattform | Modelle | Preis/Bild | Besonderheit |
|-----------|---------|------------|--------------|
| Replicate | Alle | $0.003-0.05 | Flexibel, API |
| fal.ai | Alle | $0.002-0.04 | Sehr schnell |
| Together AI | Pro, Dev | $0.003-0.03 | Gute Doku |
| Freepik | Pro | Freemium | Einfaches UI |
| ComfyUI (lokal) | Dev, Schnell | Kostenlos | Volle Kontrolle |

### Hardware-Anforderungen (Lokal)

```
Flux Schnell:
├── Minimum: 12 GB VRAM
├── Empfohlen: RTX 4070 Ti / 3080
└── Zeit: 2-5 Sekunden

Flux Dev:
├── Minimum: 16 GB VRAM (24 GB optimal)
├── Empfohlen: RTX 4090 / 3090
└── Zeit: 10-20 Sekunden

Flux Dev (FP8 quantisiert):
├── Minimum: 8 GB VRAM
├── Empfohlen: RTX 4060 Ti 16GB
└── Zeit: 15-30 Sekunden
```

---

## Seite 2: Prompt-Bausteine

### Prompt-Struktur Template

```
[SUBJEKT], [AKTION/POSE], [UMGEBUNG/SETTING], 
[STIL], [BELEUCHTUNG], [KAMERA/TECHNISCH], [DETAILS]
```

### Stil-Keywords

| Kategorie | Keywords |
|-----------|----------|
| **Foto** | photorealistic, photograph, DSLR, film photo, editorial |
| **Digital Art** | digital illustration, concept art, matte painting |
| **Traditional** | oil painting, watercolor, pencil sketch, charcoal |
| **3D** | 3D render, Octane, Unreal Engine 5, CGI, isometric |
| **Anime/Comic** | anime style, manga, comic book art, cel shaded |

### Qualitäts-Modifier

```
Positiv:
highly detailed, 8K resolution, masterpiece, 
professional, sharp focus, intricate details,
award-winning, stunning

Vermeiden (Negative):
blurry, low quality, distorted, watermark,
bad anatomy, extra limbs, disfigured, cropped
```

### Beleuchtungs-Keywords

| Typ | Keywords |
|-----|----------|
| **Natürlich** | golden hour, blue hour, overcast, harsh sunlight |
| **Studio** | softbox, rim light, three-point lighting |
| **Dramatisch** | chiaroscuro, dramatic shadows, backlit |
| **Stimmung** | moody, ethereal, warm, cool tones |

### Kamera/Technisch

```
Objektive:        Perspektiven:       Effekte:
85mm f/1.4        wide angle          bokeh
50mm              telephoto           motion blur
macro lens        bird's eye view     shallow DOF
fisheye           low angle           tilt-shift
```

### Kompositions-Begriffe

```
rule of thirds, centered composition, 
symmetrical, asymmetrical balance,
leading lines, frame within frame,
negative space, foreground interest
```

---

## Seite 3: Parameter-Referenz

### Empfohlene Einstellungen

| Parameter | Schnell | Dev | Pro |
|-----------|---------|-----|-----|
| **Steps** | 1-4 | 20-30 | 25-50 |
| **Guidance** | 0-2.0 | 2.5-4.0 | 2.5-4.0 |
| **Sampler** | Euler | Euler | Euler |

### Guidance Scale Effekte

```
1.0-2.0  → Kreativ, lockere Interpretation
2.5-3.5  → Ausgewogen (empfohlen)
4.0-5.0  → Strenge Prompt-Befolgung
>5.0     → Übergesättigt, Artefakte möglich
```

### Aspect Ratios

| Format | Pixel | Verwendung |
|--------|-------|------------|
| 1:1 | 1024×1024 | Instagram, Profilbilder |
| 16:9 | 1024×576 | Landscape, YouTube Thumbnails |
| 9:16 | 576×1024 | Stories, TikTok, Reels |
| 4:3 | 1024×768 | Klassisches Foto |
| 3:2 | 1024×683 | DSLR-Format |
| 21:9 | 1024×439 | Cinematic, Ultrawide |

### Image-to-Image Strength

```
0.1-0.3  → Minimale Änderungen, Original erkennbar
0.4-0.5  → Moderate Stilübertragung
0.6-0.7  → Deutliche Transformation
0.8-1.0  → Fast komplette Neugenierung
```

### Seed-Nutzung

```
Gleicher Seed + Gleicher Prompt = Gleiches Bild

Variationen erstellen:
seed = 12345      → Basis
seed = 12346      → Leichte Variation
seed = 12345 + n  → Serie erstellen
seed = -1         → Zufällig
```

---

## Seite 4: Best Practices & Troubleshooting

### Do's ✓

| Praxis | Warum |
|--------|-------|
| Natürliche Sprache verwenden | Flux versteht komplette Sätze besser als Keywords |
| Detailliert beschreiben | Mehr Details = präziseres Ergebnis |
| Mit niedrigem CFG starten | 2.5-3.5 ist meist optimal für Flux |
| Seeds für Iteration nutzen | Reproduzierbare Ergebnisse ermöglichen Feintuning |
| Mehrere Varianten generieren | Bestes Bild aus Serie auswählen |

### Don'ts ✗

| Vermeiden | Stattdessen |
|-----------|-------------|
| Keyword-Spam wie bei SD | Fliessende Beschreibungen |
| CFG über 5.0 | Bei 2.5-4.0 bleiben |
| Zu viele Elemente | Auf Hauptmotiv fokussieren |
| Negative Prompts überladen | Sparsam einsetzen oder weglassen |
| Maximale Auflösung erzwingen | Nativ generieren, dann upscalen |

### Troubleshooting

**Problem: Unscharfe Bilder**
```
→ Steps erhöhen (Dev: 25-30)
→ Andere Seed probieren
→ "sharp focus, highly detailed" hinzufügen
```

**Problem: Text wird falsch dargestellt**
```
→ Text in Anführungszeichen setzen
→ Schriftart spezifizieren
→ Weniger Text pro Bild verwenden
```

**Problem: Anatomie-Fehler**
```
→ Pose genauer beschreiben
→ Reference-Image mit ControlNet
→ "correct anatomy, proper proportions" hinzufügen
```

**Problem: Übergesättigte Farben**
```
→ Guidance reduzieren (2.5-3.0)
→ "natural colors, balanced exposure" hinzufügen
→ Steps leicht erhöhen
```

**Problem: Out of Memory (lokal)**
```
→ FP8-quantisierte Version nutzen
→ Auflösung reduzieren
→ Batch-Size auf 1 setzen
→ Andere Anwendungen schliessen
```

### Nützliche Ressourcen

| Ressource | URL |
|-----------|-----|
| Black Forest Labs | blackforestlabs.ai |
| ComfyUI GitHub | github.com/comfyanonymous/ComfyUI |
| Replicate Flux | replicate.com/black-forest-labs |
| Civitai (LoRAs) | civitai.com |
| r/FluxAI | reddit.com/r/FluxAI |
| ComfyUI Discord | discord.gg/comfyui |

### Quick-Start Checklist

```
□ Plattform wählen (Cloud oder lokal)
□ Account/API-Key einrichten
□ Erste Testgenerierung mit einfachem Prompt
□ Parameter für Use-Case optimieren
□ Workflow für Batch-Generierung aufsetzen
□ Ergebnisse organisieren und taggen
```

---

*Erstellt für den praktischen Einsatz | Version 1.0 | Januar 2025*

*© 2025 Ralph Hutter | Lizenziert unter CC BY-SA 4.0*
