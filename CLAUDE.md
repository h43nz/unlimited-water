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
1. **Navigation** - Fixed Header mit animiertem Mobile Menu (ARIA-konform)
2. **Hero** - Headline + Preis-Box mit "Festpreis-Garantie" Badge
3. **Stats Bar** - Social Proof (15+ Jahre, 500+ Brunnen, 4 Länder, 21M+ Liter)
4. **Über uns** - Firmenphilosophie
5. **Leistungen** - 3 Feature-Cards
6. **Vorteile** - 6 nummerierte Vorteile
7. **Kontakt** - Formular mit Loading-State & Success-Modal
8. **Footer** - Links, Language Switcher (DE/EN/PL)

**Rechtliche Seiten:**
- impressum.html - Vollständige Firmendaten (Unlimited Water sp. z o.o., Gdańsk)
- datenschutz.html - Ehrliche Minimal-Datenschutzerklärung (nur technische Logs)

**UX-Features:**
- Skip-to-Content Link (Accessibility)
- Sticky Mobile CTA Button (erscheint beim Scrollen)
- Form Success Modal mit Animation
- ARIA Labels & Keyboard Navigation
- Trust Signals (Stats, Badge, Social Proof)

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

**Sprache:** Deutsch, professioneller B2B-Ton

**Angebot:**
- Festpreis: 2.499 € inkl. MwSt. (bis 15m Tiefe)
- Aufpreis: 120 €/Meter über 15m
- Inklusive: Tiefenbohrung, Brunnenrohr DN 100mm, 2m Feinsandfilter (DIN), Brunnenkopf DN 100
- Einsatzgebiet: Deutschland, Dänemark, Polen, Niederlande

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
