# CLAUDE.md

Anweisungen für Claude Code bei der Arbeit mit diesem Repository.

## Projekt-Übersicht

Unlimited Water ist eine statische Marketing-Website für einen professionellen Brunnenbau- und Wasserversorgungsbetrieb. Die Seite ist in Deutsch und richtet sich an Kunden in Deutschland und Nachbarstaaten (DK, PL, NL).

## Tech Stack

- **HTML + Tailwind CSS (CDN)** - Alles in einer einzigen `index.html`
- **Kein Build-Prozess** - Direkt hostbar
- **Nginx Alpine Container** für Deployment
- **Docker Compose** für einfaches Hosting

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

## Lokale Entwicklung

```bash
# Einfacher HTTP-Server
python3 -m http.server 8090
# oder
npx serve -p 8090
```

## Docker Deployment

```bash
# Build und Start
docker-compose up --build

# Läuft auf http://localhost:8090
```

## Anpassungen

**Farben ändern:** Tailwind-Config im `<script>` Tag der index.html

**Inhalte ändern:** Direkt in der index.html

**Formular-Backend:** Aktuell nur Alert - für echte Funktion FormSpree/EmailJS einbinden

## Kontaktdaten (Platzhalter)

- Telefon: `+49 (0) 123 456 789 00`
- E-Mail: `info@unlimited-water.com`

Diese müssen noch durch echte Daten ersetzt werden.
