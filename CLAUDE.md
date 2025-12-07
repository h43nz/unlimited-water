# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

Unlimited Water is a static marketing website for a sovereign water supply business. The site uses a Bitcoin-aligned "Sovereign Industrial" design aesthetic with a dark theme, monospace fonts (JetBrains Mono), and Bitcoin orange (#F7931A) accents. The project is in German and targets customers in Germany and neighboring states (DK, PL, NL).

## Architecture

**Tech Stack:**
- Pure static HTML/CSS/JavaScript (no build system or framework)
- Nginx Alpine container for serving
- Docker Compose for deployment
- Deployed via Dockge (Docker stack manager) on a VM
- Proxied through Pangolin for domain routing

**File Structure:**
- `index.html` - Single-page application with sections: hero, manifest, proof-of-work, contact
- `css/style.css` - Custom CSS with design system defined in CSS variables
- `js/main.js` - Vanilla JavaScript for smooth scrolling, intersection observer animations, navbar effects
- `assets/` - Logo files (transparent PNGs preferred: `logo_mit_name_transparent.png`, `logo_transparent.png`)
- `kleinanzeigen/` - Marketing materials and screenshots (not part of web deployment)

## Design System

**Color Palette (defined in `:root`):**
- Primary: `#051e3e` (Deep Navy)
- Secondary: `#64748b` (Steel Grey)
- Signal: `#F7931A` (Bitcoin Orange) - used for CTAs and highlights
- Background Dark: `#0b1120`
- Background Card: `#0f172a`
- Border: `#1e293b`

**Typography:**
- Headers: Space Grotesk (geometric, bold, uppercase for h1)
- Technical/Code elements: JetBrains Mono (monospace)
- `.mono` class for technical labels and accent text

**Design Principles:**
- Bitcoin-aligned aesthetic (references to "proof of work", "hash", "low time preference")
- Industrial, no-nonsense presentation
- Minimal animations (intersection observer fade-ups)
- Mobile responsive with clamp() for fluid typography

## Development Commands

**Local Development:**
```bash
# Serve locally (requires Python or any HTTP server)
python3 -m http.server 8090
# Then visit http://localhost:8090
```

**Docker Build & Run:**
```bash
# Build and run with Docker Compose
docker-compose up --build

# Access at http://localhost:8090
```

**Docker Direct:**
```bash
docker build -t unlimited-water .
docker run -p 8090:80 unlimited-water
```

## Deployment

The site deploys to a Dockge VM and is proxied through Pangolin:

1. **Via Git (Recommended):** Push to GitHub repo `h43nz/unlimited-water`, then deploy in Dockge using the Git URL
2. **Via SCP:** Copy files to `/opt/stacks/unlimited-water` on Dockge VM
3. **Pangolin Proxy:** Points domain to Dockge VM IP on port 8090

**Port Configuration:**
- Container exposes port 80 internally
- Docker Compose maps to port 8090 externally
- Pangolin upstream points to port 8090

## Content Guidelines

**Language & Tone:**
- All content in German
- Technical, sovereign, no-nonsense tone
- Bitcoin terminology used metaphorically (HASH, Proof of Work, Low Time Preference)
- Emphasizes: independence, generational thinking, crisis resistance, decentralization

**Sections:**
1. Hero - Main value prop: decentralized water infrastructure
2. Manifest (#manifest) - "Low Time Preference" - durability focus
3. Proof of Work (#pow) - Technical capabilities (drilling equipment, rotary method, deep layer access)
4. Contact (#signal) - Contact form with Bitcoin-themed labels

## JavaScript Behavior

`main.js` implements:
- Smooth scroll for anchor links
- Intersection Observer for `.fade-up` elements (currently not actively used in HTML)
- Navbar shrink effect on scroll (padding reduces from 1rem to 0.5rem)
- Form submission preventDefault with alert (no backend integration)

## Common Modifications

**Changing Colors:**
Edit CSS variables in `css/style.css:1-14`

**Adding Sections:**
Follow the pattern: section with `id`, container div, mono label, h2 heading, content grid

**Form Integration:**
Replace alert in `js/main.js:53` with actual backend submission (e.g., FormSpree, EmailJS)

**Mobile Responsiveness:**
Media query at `768px` in `css/style.css:216-225` - adjust as needed for new content
