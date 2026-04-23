# missrizz.info — Portfolio Design Spec
**Date:** 2026-04-23
**Phase:** 1 (no chatbot)

---

## Overview

A personal portfolio for Ritika Agrawal ("Miss Rizz"), a full-stack/backend engineer with experience in AI agents, ML, and DevOps. The site is a job-seeking and personal brand showcase, hosted on a custom domain.

**Domain:** missrizz.info
**Stack:** Plain HTML + CSS + JavaScript (no build step)
**Dependencies:** Tailwind CSS (CDN) · AOS — Animate On Scroll (CDN)
**Hosting:** Vercel (static site, free tier, custom domain)

---

## Visual Design

**Style:** Soft editorial — light backgrounds, clean typography, intentional whitespace
**Primary color:** Indigo (`#4f46e5`)
**Accent color:** Coral (`#fb7185`)
**Background:** Off-white (`#f8f7ff`) and white (`#ffffff`)
**Dark surface:** Deep indigo (`#1e1b4b`) — used in footer only
**Typography:** Inter (Google Fonts)

---

## Site Structure

Three HTML files, no router needed:

```
index.html        → Home / Hero
experience.html   → Experience timeline
projects.html     → Projects timeline
```

Shared across all pages:
- Sticky top nav
- Dark indigo footer with email, LinkedIn, GitHub links

---

## Page Specs

### 1. Home (`index.html`)

**Layout:** Two-column split (50/50)
- **Left column:** Full-height gradient panel (indigo → lavender → coral). Contains Ritika's photo, cropped naturally, sitting at the bottom of the panel.
- **Right column:** White background. Contains:
  - Pill badge: role/title (e.g. "FULL-STACK ENGINEER · AI")
  - Name: large, bold, first name on one line, last name in indigo on next
  - Two CTA buttons: "View Experience →" (filled indigo) + "See Projects" (outlined indigo)
- **Tagline bar:** Pinned just above the footer, full-width white strip with a top border. Tagline text centered, italic, muted gray (`#9ca3af`), font-size small. Minimal — no competing with the hero.

**Nav:** Logo left ("Miss Rizz"), links right (Experience, Projects), "Hire me" pill button (indigo)

**Animations (AOS):** Right column fades in from right on load. Nav fades in from top.

---

### 2. Experience (`experience.html`)

**Layout:** Single column, centered, max-width ~720px

**Page header:**
- Small coral label: "CAREER"
- Large bold heading: "Experience"
- Subtitle: e.g. "4 companies · 4+ years"

**Timeline:**
- Vertical line running down the left, gradient indigo → coral
- Each entry is a white card with:
  - Year range (indigo or coral, alternating)
  - Job title (large, bold)
  - Company + location (muted)
  - 1–2 sentence description
  - Tech/skill tags (indigo or coral pill tags)
  - Company logo placeholder (colored square, replaced with real logo later)
- Most recent entry: full opacity, full card
- Older entries: progressively more faded (opacity 0.65, 0.4)

**Animations (AOS):** Each card fades in and slides up as user scrolls down.

**Content needed from user:** Job title, company, location, dates, description, tech tags for each role (Gruve, Atomic Ads, High Insight, Mastercard).

---

### 3. Projects (`projects.html`)

**Layout:** Same as Experience — single column, centered, max-width ~720px

**Page header:**
- Small coral label: "WORK"
- Large bold heading: "Projects"
- Subtitle: e.g. "Things I've built"

**Timeline:**
- Same vertical line and card structure as Experience
- Each card contains:
  - Year
  - Project name (large, bold)
  - One-line description
  - Tech tags
  - External link arrow button (↗) — links to GitHub, live site, or demo

**Animations (AOS):** Same scroll-triggered fade-in per card.

**Content needed from user:** Project name, year, description, tech stack, link for each project.

---

## Navigation

Consistent across all three pages:

| Element | Detail |
|---|---|
| Logo | "Miss Rizz" — links back to `index.html` |
| Experience link | Links to `experience.html`, underlined with coral when active |
| Projects link | Links to `projects.html`, underlined with coral when active |
| Hire me button | Indigo pill — links to email (`mailto:`) |

---

## Footer

Consistent across all three pages:
- Background: `#1e1b4b`
- Left: copyright line (muted lavender)
- Right: LinkedIn · GitHub · Email (coral for email, lavender for others)

---

## Hosting & Deployment

1. All files live in the repo root (or `/public` folder if preferred)
2. Push to GitHub → connect repo to Vercel → auto-deploy on push
3. Add custom domain `missrizz.info` in Vercel dashboard → update DNS A/CNAME records at domain registrar
4. Vercel handles HTTPS automatically (Let's Encrypt)

---

## Content Checklist (needed from Ritika)

- [ ] Profile photo (high-res, portrait orientation preferred)
- [ ] Tagline (1–2 sentences)
- [ ] Job title / current role label
- [ ] Experience entries: title, company, location, dates, description, tech tags × 4 roles
- [ ] Project entries: name, year, description, tech tags, link × N projects
- [ ] Email address for "Hire me" and footer
- [ ] LinkedIn URL
- [ ] GitHub URL

---

## Out of Scope (Phase 2)

- Chatbot / AI assistant widget
- Contact form
- Blog or writing section
- Dark mode toggle
