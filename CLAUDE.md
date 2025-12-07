# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

**Unlimited Water** is a static website for a sovereign/Bitcoin-aligned water well drilling service. The site promotes decentralized water infrastructure with a "Sovereign Industrial" design aesthetic.

- **Language**: German (primary content)
- **Service Area**: Germany, Denmark, Poland, Netherlands
- **Design Theme**: Bitcoin-aligned with sovereign/industrial aesthetic using Bitcoin orange (#F7931A) accents

## Tech Stack

- **Frontend**: Static HTML5, CSS3, vanilla JavaScript
- **Fonts**: Space Grotesk (headers), JetBrains Mono (code/tech elements)
- **Containerization**: Docker with Nginx Alpine Slim
- **Orchestration**: Docker Compose
- **Deployment**: Dockge VM with Pangolin reverse proxy

## Development Commands

### Local Development

```bash
# Serve locally using Python's built-in server
python3 -m http.server 8000

# Or using any other static file server
npx http-server -p 8000
```

### Docker Operations

```bash
# Build and run locally
docker-compose up --build

# Access at http://localhost:8090
# (Port 8090 maps to container port 80)

# Stop containers
docker-compose down

# Rebuild after changes
docker-compose up --build -d
```

### Deployment

The site deploys to a Dockge VM via Git. The deployment workflow is documented in `WALKTHROUGH.md`:

1. Commit and push changes to GitHub: `https://github.com/h43nz/unlimited-water.git`
2. Dockge pulls from Git repo and builds the Docker image
3. Pangolin proxy routes traffic to the Dockge VM on port 8090

## Architecture

### File Structure

```
/
├── index.html          # Single-page application (SPA) structure
├── css/
│   └── style.css      # All styles with CSS custom properties
├── js/
│   └── main.js        # Client-side interactions
├── assets/            # Logo images (transparent PNGs)
├── kleinanzeigen/     # Marketing copy for classified ads
├── Dockerfile         # Nginx Alpine container config
└── docker-compose.yml # Service definition (port 8090:80)
```

### CSS Architecture

Uses CSS custom properties in `:root` for theming:

- **Primary Colors**: Deep Navy (#051e3e), Steel Grey (#64748b)
- **Signal/Accent**: Bitcoin Orange (#F7931A)
- **Backgrounds**: Dark (#0b1120), Card (#0f172a)
- **Typography**: Space Grotesk (geometric/bold), JetBrains Mono (monospace)

Responsive grid system using CSS Grid with `auto-fit` and `minmax()` for fluid layouts.

### JavaScript Functionality

`main.js` handles:
- Smooth scrolling for anchor links
- Intersection Observer for scroll animations (`.fade-up` elements)
- Dynamic navbar styling on scroll (shadow/padding)
- Form submission handler (currently demo mode with alert)

### HTML Structure

Single-page application with sections:
1. **Hero** (`<header>`) - Value proposition
2. **Manifest** (`#manifest`) - Long-term value proposition ("Low Time Preference")
3. **Proof of Work** (`#pow`) - Technical capabilities grid
4. **Contact** (`#signal`) - Contact form and information
5. **Footer** - Copyright with "VIRES IN NUMERIS" tagline

## Design Principles

### Sovereign/Bitcoin Aesthetic
- Uppercase headings and monospace labels
- Technical/code-inspired copy (e.g., "HASH: DRILL", "/// SIGNAL")
- Bitcoin-aligned messaging (low time preference, proof of work, censorship resistance)
- Industrial color palette with orange accents

### Responsive Design
- Mobile-first with `clamp()` for fluid typography
- Breakpoint at 768px for mobile navigation
- Grid layouts automatically reflow with `auto-fit`

## Contact Form

The contact form in `#signal` section currently uses a simple `alert()` on submission. To integrate with a backend:

1. Add form action endpoint or
2. Implement AJAX submission in `main.js` (e.g., to Formspree, EmailJS, or custom backend)
3. Remove the `e.preventDefault()` and `alert()` logic

## Deployment Notes

- Container runs Nginx on port 80 internally
- Exposed as port 8090 via docker-compose
- Pangolin proxy routes domain to Dockge VM:8090
- All static assets are copied to `/usr/share/nginx/html` in container
- Container restarts automatically (`restart: always`)

## Content Updates

Marketing copy for classified ads (kleinanzeigen) is maintained in `kleinanzeigen/text_kleinanzeigen.md` for reference. This is not served on the website but used for external advertising.
