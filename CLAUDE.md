# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

Unlimited Water is a static marketing website for a professional well-drilling and water supply business. The site uses an "Industrial Tech" design aesthetic with a dark theme, monospace fonts (JetBrains Mono), and orange (#F7931A) accents. Bitcoin references have been made subtle - the design aesthetic remains but explicit crypto terminology has been removed. The project is in German and targets customers in Germany and neighboring states (DK, PL, NL).

## Architecture

**Tech Stack:**
- Pure static HTML/CSS/JavaScript (no build system or framework)
- Nginx Alpine container for serving
- Docker Compose for deployment
- Deployed via Dockge (Docker stack manager) on a VM
- Proxied through Pangolin for domain routing

**File Structure:**
- `index.html` - Single-page application with sections: hero, manifest, methodik (technical), vorteile (benefits), contact
- `css/style.css` - Custom CSS with design system defined in CSS variables
- `js/main.js` - Vanilla JavaScript for smooth scrolling, navbar effects, form handling
- `assets/` - Logo files (transparent PNGs preferred: `logo_mit_name_transparent.png`, `logo_transparent.png`)
- `kleinanzeigen/` - Marketing materials and screenshots (not part of web deployment)
- `Dockerfile` - Nginx Alpine container configuration
- `docker-compose.yml` - Deployment configuration for Dockge

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
- Industrial tech aesthetic (subtle technical references like "SYSTEM:", "METHODIK")
- Professional B2B presentation with trust signals
- Conversion-optimized with clear CTAs and benefits
- Mobile responsive with clamp() for fluid typography
- SEO-optimized meta tags and content structure

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

**Update Workflow in Dockge:**
1. Click "Update" (pulls latest from Git)
2. Stop stack
3. Delete stack (only removes container, repo stays)
4. Click "Deploy" (rebuilds with new files)

## Content Guidelines

**Language & Tone:**
- All content in German
- Professional B2B tone with technical credibility
- Subtle tech references (SYSTEM labels, monospace styling) without explicit crypto terminology
- Emphasizes: independence, long-term value, quality, transparency, trust

**Sections:**
1. **Hero** - Main value prop: independent water supply, crisis-proof infrastructure
2. **Manifest (#manifest)** - "LANGFRISTIG GEDACHT" - generational thinking, quality focus
3. **Methodik (#methodik)** - Technical capabilities (drilling equipment, rotary method, deep water access)
4. **Vorteile (#vorteile)** - 6 key benefits with trust signals (experience, service, quality, cost transparency, speed, sustainability)
5. **Contact (#signal)** - Enhanced contact section with phone number, business hours, improved form with placeholders and privacy notice

**Marketing & SEO Guidelines:**
- Keep Bitcoin aesthetic subtle (orange color, industrial design, technical labels)
- Use professional service terminology, not crypto slang
- SEO meta description includes: "Tiefbrunnen", "Deutschland", country codes, "Bohrtechnik"
- Trust signals: years of experience, certifications (DIN), transparency, testimonials
- CTAs emphasize value: "Kostenlose Beratung anfragen" not generic "Submit"

## JavaScript Behavior

`main.js` implements:
- Smooth scroll for anchor links (works with updated navigation: #manifest, #methodik, #vorteile, #signal)
- Intersection Observer for `.fade-up` elements (available but not currently used in HTML)
- Navbar shrink effect on scroll (padding reduces from 1.5rem to 0.5rem)
- Form submission preventDefault with German alert message (no backend integration yet)

**Form Integration TODO:** Replace alert in `js/main.js` with backend (FormSpree, EmailJS, or custom endpoint)

## Common Modifications

**Changing Colors:**
Edit CSS variables in `css/style.css:1-14`

**Adding Sections:**
Follow the pattern: section with `id`, container div, mono label, h2 heading, content grid

**Form Integration:**
Replace alert in `js/main.js:53` with actual backend submission (e.g., FormSpree, EmailJS)

**Mobile Responsiveness:**
Media query at `768px` - includes navigation font-size reduction, benefit spacing, footer stacking, reduced section padding

**New CSS Components:**
- `.benefit-list` and `.benefit-item` - Benefits section with checkmark icons
- `.benefit-icon` - Circular orange checkmarks
- `.footer-content` and `.footer-section` - 3-column footer layout
- Form placeholder styling with opacity changes on focus

**Key Files to Update Together:**
- When changing navigation: update both `index.html` nav-links AND section `id` attributes
- When adding sections: add CSS in `style.css`, update navigation, add scroll handling in `main.js`
- Phone number placeholder: Currently `+49 (0) 123 456 789 00` - update in contact section when real number available
