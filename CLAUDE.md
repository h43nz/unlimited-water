# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Projekt-Übersicht

Unlimited Water ist eine statische Marketing-Website für professionellen Brunnenbau und Tiefbohrungen. Die Seite ist in Deutsch und richtet sich an Kunden in Deutschland und Nachbarstaaten (DK, PL, NL).

**Live URL:** https://uw.21b.me

## Tech Stack

- **HTML + Tailwind CSS (CDN)** - Single-File Architektur in `index.html`
- **GitHub Container Registry** - Automatisches Image-Building via GitHub Actions
- **Deployment:** Dockge (LXC auf Proxmox) + Pangolin Reverse Proxy

## Dateistruktur

```
unlimited-water/
├── index.html          # Komplette Website (HTML + Tailwind + JS inline)
├── assets/
│   ├── logo_mit_name_transparent.png
│   └── logo_transparent.png
├── Dockerfile          # Nginx Alpine
├── docker-compose.yml  # Port 8090
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

## Sections

1. **Navigation** - Fixed, mit Mobile-Menu
2. **Hero** - Headline, Subtext, CTAs
3. **Stats Bar** - Kennzahlen (15+ Jahre, 500+ Projekte, 4 Länder)
4. **Über uns** - Firmenphilosophie, 21M+ Liter Kapazität
5. **Leistungen** - 3 Karten (Bohrgerät, Spülbohrverfahren, Tiefenwasser)
6. **Vorteile** - 6 nummerierte Vorteile
7. **Kontakt** - Info + Formular
8. **Footer** - Links, Kontakt, Rechtliches

## Deployment Workflow

**Automatischer CI/CD Pipeline via GitHub Actions:**

```
git push → GitHub Actions → Container Registry → Dockge (manueller Pull)
```

1. **Lokal ändern** und `git push` zu `main` Branch
2. **GitHub Actions** baut automatisch Docker Image (`.github/workflows/docker-build.yml`)
3. **Image wird gepusht** zu `ghcr.io/h43nz/unlimited-water:latest`
4. **In Dockge:** Container manuell neu starten um neuestes Image zu pullen
5. **Live auf** https://uw.21b.me

**Wichtig:** Änderungen an `index.html` werden erst nach Container-Neustart sichtbar.

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

**Offene TODOs:**
- Kontaktformular-Backend einrichten (Formspree/Web3Forms)
- Echte Kontaktdaten eintragen (aktuell Platzhalter)
- Impressum und Datenschutz-Seiten erstellen

## Tailwind Customization

Custom Colors in `<script>` Tag:
- `water-*` (50-950): Blautöne für CTAs und Akzente
- `earth-*` (50-900): Warme Erdtöne (aktuell ungenutzt)

Font: Inter (Google Fonts)
