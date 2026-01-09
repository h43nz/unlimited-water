# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Projekt-Übersicht

Unlimited Water ist eine statische Marketing-Website für professionellen Brunnenbau und Tiefbohrungen. Die Seite ist mehrsprachig (DE/EN/PL) und richtet sich an Kunden in Deutschland und Nachbarstaaten.

**Live URL:** https://unlimited-water.eu
**Status:** Production-ready (DE), Übersetzungen ausstehend (EN, PL)

## Tech Stack

- **HTML + Tailwind CSS (CDN)** - Single-File Architektur, kein Build-Prozess
- **Nginx Alpine** - Static file serving in Container
- **GitHub Container Registry** - Automatisches Image-Building via GitHub Actions
- **Deployment:** Dockge (LXC auf Proxmox) + Pangolin Reverse Proxy

## Architektur

### Single-File Pattern
Jede Sprachversion ist eine standalone HTML-Datei mit:
- Inline Tailwind CSS (CDN)
- Inline JavaScript für Interaktivität
- Google Fonts (Inter) via CDN
- Keine externen CSS/JS Dependencies

**Vorteil:** Keine Build-Pipeline, sofortige Preview, einfaches Deployment

### Multi-Language Struktur

```
/                    → index.html (Deutsch, x-default)
/impressum.html      → Impressum (DE)
/datenschutz.html    → Datenschutz (DE)
/en/                 → Englische Version (vorbereitet)
/en/imprint.html     → Imprint (EN, ausstehend)
/en/privacy.html     → Privacy (EN, ausstehend)
/pl/                 → Polnische Version (vorbereitet)
/pl/impressum.html   → Impressum (PL, ausstehend)
/pl/polityka-prywatnosci.html → Datenschutz (PL, ausstehend)
```

**SEO:** Jede Seite hat `<link rel="alternate" hreflang="xx">` Tags für alle Sprachen

### Dateistruktur

```
unlimited-water/
├── index.html              # Hauptseite (Deutsch)
├── impressum.html          # Impressum mit Firmendaten
├── datenschutz.html        # Datenschutzerklärung
├── en/                     # Englische Übersetzung (leer)
├── pl/                     # Polnische Übersetzung (leer)
├── assets/
│   ├── logo_transparent.png
│   ├── logo_mit_name_transparent.png
│   ├── logo-icon.svg       # Nicht verwendet (SVG-Konvertierung fehlgeschlagen)
│   └── favicon.svg         # SVG Wassertropfen
├── .github/workflows/
│   └── docker-build.yml    # Auto-build bei push zu main
├── Dockerfile              # Nginx Alpine
├── docker-compose.yml      # Port 8090
└── CLAUDE.md
```

## Design-System

**Farbpalette:**
- Primary: `water-600` (#0284c7) - Wasserblau für CTAs und Akzente
- Background: Weiß und `slate-50` für Sections
- Text: `slate-800` (Haupttext), `slate-600` (Subtext)
- Dark: `slate-900` für Footer

**Typografie:**
- Font: Inter (via Google Fonts)
- Clean, modern, professionell

**Tailwind-Konfiguration:** Inline im `<script>` Tag mit Custom Colors (`water`, `earth`)

## Page Sections

**Hauptseite (index.html):**
1. **Navigation** - Fixed Header mit animiertem Mobile Menu (ARIA-konform), FAQ-Link
2. **Hero** - Headline "Tiefbrunnen" + Preis-Box (2.499€ bis 15m, bis 50m möglich)
3. **USP Features** - 3 Icon-Cards: International tätig, Kompaktes Bohrgerät (1,1m), DIN-gerecht
4. **Einsatzgebiet-Banner** - 4 Länder + USP "1,1m breit"
5. **Über uns** - Firmenphilosophie
6. **Leistungen** - 3 Feature-Cards (Tiefenbohrung bis 50m, Brunnenausbau, Brunnenfilter DIN 4924)
7. **Vorteile** - 6 Benefit-Cards (Trockenheitssicher, Fördermenge, Wasserqualität, Kosten, Unabhängigkeit, Lebensdauer)
8. **Ablauf** - 4-Schritte-Prozess (Anfrage → Beratung → Bohrung → Fertig)
9. **FAQ** - 10 ausklappbare Fragen mit detaillierten Antworten (Accordion-Pattern)
10. **Kontakt** - Formular mit ausklappbaren optionalen Services (5 Checkboxen)
11. **Footer** - Links, Language Switcher (DE/EN/PL)

**Rechtliche Seiten:**
- impressum.html - Vollständige Firmendaten, dezenter "Zurück"-Link (Icon only mobile)
- datenschutz.html - Minimal-Datenschutzerklärung (nur technische Logs, keine Cookies/Tracking)

**UX-Features:**
- Skip-to-Content Link (Accessibility)
- Sticky Mobile CTA Button (erscheint beim Scrollen)
- Form Success Modal mit Animation
- FAQ Accordion mit Smooth-Toggle (JavaScript)
- Ausklappbare Service-Checkboxen im Formular (Progressive Disclosure)
- ARIA Labels & Keyboard Navigation
- Responsive Design (Mobile-First)

## Deployment Workflow

**Automatischer CI/CD Pipeline:**

```
git push main → GitHub Actions → ghcr.io/h43nz/unlimited-water:latest → Dockge → unlimited-water.eu
```

1. **Lokal ändern** und `git push` zu `main` Branch
2. **GitHub Actions** triggert automatisch (`.github/workflows/docker-build.yml`)
3. **Docker Image Build:** Nginx Alpine mit allen HTML/Assets
4. **Push zu Registry:** `ghcr.io/h43nz/unlimited-water:latest`
5. **In Dockge (CT 104):** Container manuell neu starten ("Update" Button)
6. **Pangolin Reverse Proxy** leitet `unlimited-water.eu` → `192.168.178.230:8090`

**Wichtig:**
- Änderungen sind erst nach Container-Neustart in Dockge live
- Lokale Preview via `python3 -m http.server 8090` zeigt Änderungen sofort
- GitHub Actions braucht ~2-3 Minuten für Image-Build

## Lokale Entwicklung

```bash
# Einfacher HTTP-Server für Live-Preview
python3 -m http.server 8090

# Dann http://localhost:8090 im Browser öffnen
```

**Keine Build-Steps nötig** - alle Änderungen sind sofort sichtbar.

## Content-Richtlinien

**Sprache:** Deutsch, professioneller B2B-Ton (technisch präzise, aber für Laien verständlich)

**Angebot:**
- Festpreis: 2.499 € inkl. MwSt. (bis 15m Tiefe)
- Aufpreis: 120 €/Meter über 15m (maximal bis 50m Tiefe)
- Inklusive: Tiefenbohrung, Brunnenrohr DN 100mm, Brunnenfilter DIN 4924 (2m geschlitzt 0,5mm), Brunnenkopf DN 100
- Einsatzgebiet: Deutschland, Dänemark, Polen, Niederlande

**Optionale Services:**
- Pumpeninstallation: ab 800€
- Hauswasserwerk: ab 1.400€
- Anzeige-Service (Untere Wasserbehörde): 200€
- Bauwasser-Service (IBC gefüllt mitbringen): 500€
- Entsorgung Bohrschlamm & Aushub: Preis nach Aufwand

**Technische Spezifikationen:**
- Bohrverfahren: Spülbohrverfahren (Rotationsbohren)
- Bauwasser: ca. 1.000 Liter (IBC-Container direkt an Bohrstelle)
- Bohrschlamm: ca. 1 m³ (verbleibt auf Grundstück)
- Bohrgerät: 1,1m breit, 2,4m hoch (70m Schlauch für Wasseranschluss)
- Standards: DIN 4924, DVGW-geprüfte Materialien

## Firmendaten

**Unlimited Water sp. z o.o.**
Jana Heweliusza 11/811, PL-80-890 Gdańsk, Polen
Geschäftsführer: Marcin Stanisław Klein
KRS: 0001179873, EUID: PLKRS.0001179873
E-Mail: info@unlimited-water.eu

*sp. z o.o. = polnische GmbH (Gesellschaft mit beschränkter Haftung)*

## Nächste Schritte

- [ ] Englische Übersetzung: `/en/index.html`, `/en/imprint.html`, `/en/privacy.html`
- [ ] Polnische Übersetzung: `/pl/index.html`, `/pl/impressum.html`, `/pl/polityka-prywatnosci.html`
- [ ] Kontaktformular-Backend einrichten (Formspree/Web3Forms)
- [ ] SVG-Logo professionell konvertieren (aktuell PNG)

## Tailwind Customization

**Custom Colors** (inline im `<script>` Tag jeder HTML-Datei):
- `water-*` (50-950): Blautöne für CTAs und Akzente (#0284c7 = water-600)
- `earth-*` (50-900): Warme Erdtöne (aktuell ungenutzt, für zukünftige Features)

**Font:** Inter (Google Fonts, weights: 400, 500, 600, 700)

**Wichtig bei Änderungen:**
- Tailwind Config ist inline in jeder HTML-Datei
- Änderungen müssen in allen Sprachversionen repliziert werden
- Keine separate `tailwind.config.js` Datei vorhanden

## JavaScript Patterns

**FAQ Accordion (toggleFaq):**
- Accordion-Pattern: Nur eine FAQ-Antwort gleichzeitig geöffnet
- SVG-Icon rotiert 180° beim Öffnen
- ARIA-Attribute (aria-expanded, aria-controls)
- 10 FAQ-Items mit IDs faq-1 bis faq-10

**Optional Services Toggle (toggleOptionalServices):**
- Progressive Disclosure im Kontaktformular
- SVG-Icon rotiert 90° beim Öffnen
- Reduziert Form Friction (Checkbox-Sektion ausklappbar)

**Mobile Menu Toggle:**
- Hamburger-Icon Animation
- ARIA-konform (aria-expanded, aria-controls)
- Keine CTA-Button im ausgeklappten Zustand

**Sticky Mobile CTA:**
- Erscheint beim Scrollen nach unten
- Verschwindet beim Klick
- Z-Index: 40

## Marketing & UX Prinzipien

**Wichtig:**
- **KEINE erfundenen Zahlen/Stats verwenden** (rechtliche Risiken: Abmahnung, Verbrauchertäuschung)
- Fokus auf echte USPs: 1,1m Bohrgerät, DIN 4924, 4 Länder, 70m Schlauch
- Transparenz bei Zusatzkosten (optional, aber klar kommuniziert)
- Technische Details im FAQ-Bereich (Vertrauensbildung durch Fachwissen)
- Progressive Disclosure: Komplexe Optionen ausklappbar (reduziert Überforderung)

**Conversion-Optimierung:**
- Problem-Lösung-Fokus statt Cost-First (Vorteile: Trockenheit → Kosten)
- FAQ beantwortet Kundenbedenken proaktiv
- Sticky CTA für Mobile-User
- Dezente rechtliche Seiten (Zurück-Button nicht zu prominent)
