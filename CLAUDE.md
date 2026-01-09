# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Projekt-Übersicht

Unlimited Water ist eine statische Marketing-Website für professionellen Brunnenbau und Tiefbohrungen. Die Seite ist mehrsprachig (DE/EN/PL) und richtet sich an Kunden in Deutschland und Nachbarstaaten.

**Live URL:** https://unlimited-water.eu
**Status:** Production-ready (DE), Übersetzungen ausstehend (EN, PL)

**Kontakt:**
- Telefon (Deutschland): +49 156 79682655
- Telefon (Polen): +48 690 365 073
- WhatsApp: +48 690 365 073
- E-Mail: info@unlimited-water.eu

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
2. **Hero** - Headline "Nie wieder Wasserkosten" + Preis-Box (2.499€ bis 15m, bis 50m möglich) + Urgency-Element
3. **USP Features** - 3 Icon-Cards: International tätig, Kompaktes Bohrgerät (1,1m), DIN-gerecht
4. **Einsatzgebiet-Banner** - 4 Länder + USP "1,1m breit, 2,4m hoch"
5. **Über uns** - Professioneller Brunnenbau mit Qualitätsgarantie (keine falschen Zeitangaben!)
6. **Leistungen** - 3 Feature-Cards (Tiefenbohrung bis 50m, Brunnenausbau, Brunnenfilter DIN 4924)
7. **Vorteile** - 7 Benefit-Cards (Trockenheitssicher, Fördermenge, Wasserqualität, Kosten, Unabhängigkeit, Lebensdauer, Schmale Einfahrten)
8. **Ablauf** - 4-Schritte-Prozess (Anfrage → Beratung → Bohrung → Fertig)
9. **FAQ** - 10 ausklappbare Fragen mit detaillierten Antworten (Accordion-Pattern)
10. **Kontakt** - Formular mit DSGVO-Checkbox + ausklappbaren optionalen Services (5 Checkboxen) + beide Telefonnummern + WhatsApp
11. **Footer** - Links, Language Switcher (DE/EN/PL), beide Telefonnummern, WhatsApp-Link

**Rechtliche Seiten:**
- impressum.html - Vollständige Firmendaten (inkl. beide Telefonnummern), dezenter "Zurück"-Link (Icon only mobile)
- datenschutz.html - Minimal-Datenschutzerklärung (nur technische Logs, keine Cookies/Tracking) + WhatsApp-Hinweis mit DSGVO-Rechtsgrundlage

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
- Bauwasser: ca. 1.000 Liter (IBC-Container, wird je nach Bedarf nachgefüllt)
- Bohrschlamm: ca. 1 m³ (verbleibt auf Grundstück, optional Entsorgung)
- Bohrgerät: 1,1m breit, 2,4m hoch (70m Schlauch für Wasseranschluss)
- Standards: DIN 4924, DVGW-geprüfte Materialien
- Wartung: Nahezu wartungsfrei (nur bei Problemen Inspektion nötig)
- Lebensdauer: 30-50 Jahre bei guter Wasserqualität

## Firmendaten

**Unlimited Water sp. z o.o.**
Jana Heweliusza 11/811, PL-80-890 Gdańsk, Polen
Geschäftsführer: Marcin Stanisław Klein
KRS: 0001179873, EUID: PLKRS.0001179873
Telefon (DE): +49 156 79682655
Telefon (PL): +48 690 365 073 (auch WhatsApp)
E-Mail: info@unlimited-water.eu

*sp. z o.o. = polnische GmbH (Gesellschaft mit beschränkter Haftung)*

**WICHTIG:** Das Unternehmen existiert seit letztem Jahr - KEINE falschen "15 Jahre Erfahrung" Claims verwenden!

## FAQ-Inhalte (10 Fragen)

Die FAQ-Section ist kritisch für Conversion - beantwortet Kundenbedenken proaktiv:

1. **Wie tief wird gebohrt?** - Bis Grundwasser (8-25m), Festpreis bis 15m, max. 50m
2. **Passt das durch mein Gartentor?** - Ja, 1,1m breit, 2,4m hoch
3. **Wie läuft die Bohrung ab?** - 4 Phasen (Vorbereitung, Bohrung, Ausbau, Fertigstellung) + Hinweis: Bohrschlamm verbleibt
4. **Brauche ich eine Genehmigung?** - Meist nur Anzeige (keine Genehmigung), optional 200€ Service
5. **Brauche ich einen Wasseranschluss?** - Ideal: Gartenschlauch max. 50m, IBC wird nachgefüllt. Ohne: +500€
6. **Wie lange dauert die Bohrung?** - 1 Tag (6-8h), bei hartem Boden 1-2 Tage
7. **Funktioniert bei Trockenheit?** - Ja! Tiefbrunnen erreichen stabile Grundwasserleiter
8. **Welche Wassermenge?** - 2.000-5.000 L/h (Sand), weniger bei Ton, min. 1.500 L/h für Garten
9. **Muss ich warten?** - Nahezu wartungsfrei, 30-50 Jahre Lebensdauer
10. **Unterschied Tiefbrunnen vs. Spülbrunnen/Schlagbrunnen?** - Wettbewerbsabgrenzung (siehe unten)

## Wettbewerbsabgrenzung

**Wichtig:** FAQ 10 positioniert Tiefbrunnen gegen Konkurrenz:

**Schlagbrunnen & Spülbrunnen (negativ):**
- Nur bis 8m Tiefe
- Versiegt bei Trockenheit
- Geringere Lebensdauer
- Geringere Fördermenge
- Schlechtere Wasserqualität

**Tiefbrunnen (positive USPs als Spiegelung):**
- Bis 50m Tiefe
- Trockenheitssicher auch bei Dürre
- Hohe Fördermenge für große Gärten
- Ausgezeichnete Wasserqualität aus tiefen Schichten
- Lange Lebensdauer (30-50 Jahre)

**Spülbrunnen ist Konkurrenzprodukt** - negative Aspekte klar benennen, aber Tiefbrunnen-Vorteile als positive Spiegelung darstellen.

## Nächste Schritte

- [ ] Englische Übersetzung: `/en/index.html`, `/en/imprint.html`, `/en/privacy.html`
- [ ] Polnische Übersetzung: `/pl/index.html`, `/pl/impressum.html`, `/pl/polityka-prywatnosci.html`
- [ ] Kontaktformular-Backend einrichten (Formspree/Web3Forms)
- [ ] SVG-Logo professionell konvertieren (aktuell PNG)
- [ ] Technische Bohrskizze für FAQ 3 erstellen (Spülbohrverfahren-Illustration)

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

**Kritische Regeln (rechtlich/ethisch):**
- **KEINE erfundenen Zahlen/Stats/Zeitangaben** (Abmahnung-Risiko!)
- **KEINE "15 Jahre Erfahrung"** - Firma existiert seit letztem Jahr
- **KEINE Testimonials ohne echte Kunden** (Verbrauchertäuschung)
- Fokus auf echte, beweisbare USPs: 1,1m Bohrgerät, DIN 4924, 4 Länder, 70m Schlauch

**Transparenz & Vertrauen:**
- Zusatzkosten klar kommuniziert (optional, aber sichtbar)
- Technische Details im FAQ (Vertrauensbildung durch Fachwissen)
- DSGVO-konforme Datenschutzerklärung (WhatsApp-Hinweis)
- Ehrliche Angaben (IBC wird nachgefüllt, Bohrschlamm verbleibt)

**Conversion-Optimierung:**
- Headline: Benefit-fokussiert ("Nie wieder Wasserkosten" statt "Ihr eigener Tiefbrunnen")
- Urgency ohne konkrete Termine ("Frühjahr und Sommer" statt "März/April 2026")
- Problem-Lösung-Fokus (Trockenheit → Fördermenge → Kosten)
- FAQ beantwortet alle Kundenbedenken proaktiv (10 Fragen)
- Progressive Disclosure: Komplexe Optionen ausklappbar (Service-Checkboxen)
- Sticky Mobile CTA (erscheint beim Scrollen)
- Dezente rechtliche Seiten (Icon-only "Zurück" auf Mobile)
- WhatsApp prominent für niedrigschwellige Kontaktaufnahme

**Wettbewerbsstrategie:**
- Spülbrunnen als minderwertige Alternative darstellen
- Negative Aspekte der Konkurrenz → Positive USPs des Tiefbrunnens
- Technische Überlegenheit klar kommunizieren (DIN 4924, DN 100, 30-50 Jahre)

## Assets & Bilder

**Vorhandene Assets:**
- `/assets/logo_transparent.png` - Hauptlogo (PNG, da SVG-Konvertierung fehlgeschlagen)
- `/assets/logo_mit_name_transparent.png` - Logo mit Firmennamen
- `/assets/favicon.svg` - SVG Wassertropfen-Icon
- `/assets/logo-icon.svg` - Nicht verwendet (SVG-Konvertierung problematisch)

**Geplant:**
- `/assets/bohrung-ablauf.svg` (oder `.png`) - Technische Illustration für FAQ 3 "Wie läuft die Bohrung ab?"
  - Stil: Modern, clean, Tailwind-kompatibel (rounded-2xl, shadow-sm)
  - Farben: `#0284c7` (water-blue), `#f8fafc` (slate-50), `#475569` (slate-600)
  - Inhalt: Spülbohrverfahren-Schema (Spülpumpe, Saugbecken, Bohrloch, Rücklaufgraben, Wasserkreislauf)
  - Labels: "Spülpumpe", "Saugbecken min 1000L", "Bohrloch" (keine Beschriftung für Rücklaufgraben)
  - Pfeile zeigen Wasserfluss: Saugbecken → Pumpe → Lafette, innen runter, außen hoch, Graben zurück

**Design-Guidelines für neue Bilder:**
- Modern, nicht industriell (Apple/Stripe-Ästhetik)
- Soft shadows, rounded corners
- Farbpalette aus Tailwind-Config verwenden
- Glassmorphism-Effekte für Wasser
- SVG bevorzugt (skalierbar, web-optimiert)
- Falls PNG: min. 1200px Breite, retina-ready

## Kontaktformular

**Aktueller Status:** Mock-Implementation (zeigt nur Success-Modal, sendet keine Daten)

**Formular-Felder:**
- Name (required)
- E-Mail (required)
- Telefon (optional)
- Nachricht (required)
- DSGVO-Checkbox mit Link zu datenschutz.html (required)
- Ausklappbare optionale Services (5 Checkboxen):
  - Pumpeninstallation (ab 800€)
  - Hauswasserwerk (ab 1.400€)
  - Anzeige-Service (200€)
  - Bauwasser-Service (500€)
  - Entsorgung Bohrschlamm

**TODO: FormSpree Integration**
- Aktuell: Zeile ~1290-1320 in index.html enthält Mock-Code
- Zu ersetzen durch echten FormSpree API-Call
- FormSpree ID muss generiert werden
- Success-Modal beibehalten (gute UX)
- Error-Handling hinzufügen

**Code-Kommentar im index.html (Zeile ~1290):**
```javascript
// TODO: FormSpree Integration
// Ersetze den Mock-Code unten durch echten FormSpree API-Call:
//
// const formData = new FormData(form);
// const response = await fetch('https://formspree.io/f/YOUR_FORM_ID', {
//     method: 'POST',
//     body: formData,
//     headers: { 'Accept': 'application/json' }
// });
```
