# Rizz Portfolio — Design Spec

**Date:** 2026-04-05
**Author:** Ritika "Rizz" Agrawal
**Status:** Approved

---

## 1. Overview

A personal portfolio website for Ritika Agrawal, branded around the identity "Rizz." The site serves three purposes: job-seeking tool, professional showcase, and personal brand hub. Target audience is HRs and tech-savvy professionals.

**Framework:** Astro (static output, islands architecture)
**Deployment:** Vercel or Netlify, connected to a custom domain
**Phase 2 (deferred):** Chatbot feature as an Astro island

---

## 2. Design System

### Typography

- **Body:** Nunito (weights 200–700, Google Fonts)
- **Tagline accent:** Cormorant Garamond italic (optional, for landing tagline)

### Color Palette

| Token | Value | Usage |
|---|---|---|
| bg | #faf5f7 | Page background |
| text | #3d2e36 | Primary text |
| accent | #a8566e | Rizz rose — brand accent, active states, highlights |
| muted-text | #8a6a78 | Taglines, secondary text |
| muted-label | #b8a0aa | Section labels, icons, hamburger lines |
| card-bg | rgba(255,255,255, 0.6–0.7) | Card backgrounds |
| card-border | rgba(200,160,175, 0.15) | Card borders |
| blob-1 | #f0c0cf | Background blob |
| blob-2 | #e4cdd6 | Background blob |
| blob-3 | #fce4ec | Background blob |
| divider | #e0d0d8 | Thin divider lines |

### Card Style (Global)

All cards across Projects, Blog, Certs & Awards, and About Me Q&A share the same glassmorphism treatment:

- Background: rgba(255,255,255, 0.6–0.7)
- Backdrop-filter: blur(12px)
- Border-radius: 20px
- Border: 1px solid rgba(200,160,175, 0.15)
- Subtle box-shadow for depth

### Background Blobs

Soft circular gradients, absolutely positioned, with `filter: blur(100px)` and `opacity: 0.4`. Present on the landing hero; used sparingly on inner pages to maintain visual continuity without distraction.

### Animations

- Scroll-triggered fade-ins via Intersection Observer (cards, timeline entries)
- Sidebar slide-in: 300ms ease
- Landing scroll-hint: vertical bob animation (2.5s ease-in-out infinite)

### Responsive Behavior

- Card grids: 2–3 columns on desktop, 1 column on mobile
- Sidebar: narrow slide-in on desktop, full-width overlay on mobile
- Typography scales down gracefully on smaller viewports

---

## 3. Site Architecture

```
/
├── index.astro              — Landing page
├── experience.astro         — Career timeline
├── projects/
│   ├── index.astro          — Projects card grid
│   └── [slug].astro         — Individual project detail
├── blog/
│   ├── index.astro          — Blog card grid
│   └── [slug].astro         — Individual blog post
├── certifications.astro     — Certs & Awards card grid
├── about.astro              — About Me (snapshot + Q&A)
└── [Layout: hamburger + sidebar + footer on every page]
```

### Content Collections

- `/src/content/projects/` — Markdown files for each project
- `/src/content/blog/` — Markdown/MDX files for blog posts
- `/src/content/certifications/` — Markdown files for each certification or award (consistent with other collections)

### Shared Layout

An Astro layout component wraps every page, providing:
- Hamburger icon (fixed top-right)
- Sidebar navigation (island)
- Footer with contact links
- Background blobs (on landing; optional subtle version on inner pages)

---

## 4. Navigation

### Hamburger

- Fixed position, top-right corner (top: 28px, right: 32px)
- Three thin horizontal lines (#b8a0aa, 26px wide, 2px tall, 5px gap)
- Click toggles sidebar open/closed

### Sidebar (Compact Slide-in)

- Narrow panel (~220px) slides in from the right
- Semi-transparent blurred background
- Stacked vertical links with small icons:
  - Home
  - Experience
  - Projects
  - Blog
  - Certs & Awards
  - About Me
- Active page highlighted with accent color (#a8566e)
- Close: X icon replaces hamburger, or click outside to dismiss
- Transition: 300ms ease slide + subtle overlay on page content

---

## 5. Pages

### 5.1 Landing Page

**Hero section (full viewport):**

- Circular photo: 300px diameter, gradient placeholder (linear-gradient 135deg, #e4bcc9 → #cfa4b4), centered
- "RIZZ" label: 14px, weight 400, uppercase, letter-spacing 5px, color #a8566e, 12px below photo
- "RITIKA AGRAWAL": 42px, weight 300, uppercase, letter-spacing 6px, 40px below RIZZ. "Agrawal" in weight 600, color #a8566e
- Tagline: Nunito italic, 16px, weight 300, color #8a6a78, 28px below name. Text: "turning ideas into systems, fueled by tea"
- Scroll-hint chevron: animated bob at bottom of viewport
- Background: three blobs (pink, mauve, light pink) with 100px blur

**Below fold (scroll down):**

- "At a glance" section: chips for "5+ years experience", "IIT BHU ECE '21", "Backend / AI / Infra"
- "Worked with" section: company names as styled text — Mastercard, High Insight, Atomic Ads, Gruve
- "Skills" section: pill tags — Java, Python, Kubernetes, LangGraph, Ansible, DevOps, AI/ML, Distributed Systems

### 5.2 Experience Page

- Page title in section-label style (uppercase, letter-spaced, muted)
- Vertical timeline: thin line (#e0d0d8) running down the left side
- Stacked cards, one per company, connected to the timeline via dot nodes
- Each card (glassmorphism style):
  - Company name (bold)
  - Role title
  - Date range
  - 3–4 bullet highlights
- Order: most recent first (Gruve → Atomic Ads → High Insight → Mastercard)
- Cards fade in on scroll

### 5.3 Projects Page

**Grid view (`/projects`):**

- Responsive card grid (2–3 columns desktop, 1 mobile)
- Each card:
  - Thumbnail/preview area (gradient placeholder if no image)
  - Project title
  - One-line description
  - Tech tags as small pills
  - Entire card is clickable → navigates to detail page

**Detail view (`/projects/[slug]`):**

- Hero with project title + tech tags
- Narrative sections: Problem → Approach → Outcome
- Screenshots/diagrams if available
- Back link to grid

### 5.4 Blog/Writing Page

**Grid view (`/blog`):**

- Card grid matching Projects layout
- Each card:
  - Category tag at top (e.g., "DevOps", "AI", "Thoughts")
  - Post title
  - Date
  - Short excerpt (~100 chars or custom summary)
  - Click → full post

**Post view (`/blog/[slug]`):**

- Title + date + category tag
- Markdown-rendered body (Nunito, comfortable line-height)
- Back link to grid

**Content:** `.md` or `.mdx` files in `/src/content/blog/`, newest first

### 5.5 Certifications & Awards Page

- Card grid matching Projects and Blog
- Each card:
  - Issuer name/logo area
  - Certification or award title
  - Date earned
  - Verify link button (subtle) if applicable
  - Label tag distinguishing "Certification" vs "Award"
- No detail pages — cards are self-contained

### 5.6 About Me Page

**Top section (Snapshot):**

- Larger photo or circular placeholder
- Quick-fact chips (location, current role, education, tea enthusiast, etc.)
- Fun stats row (e.g., "cups of tea: infinity", "deployments survived: 100+", "companies: 4")

**Below fold (Q&A):**

- Casual FAQ-style cards (glassmorphism)
- Example questions: "What do you do?", "Why Rizz?", "Tea or coffee?", "What's your superpower?"
- Short, personality-forward answers
- Each Q&A in its own card

---

## 6. Footer (Contact)

- Appears on every page
- Compact strip: one-line message ("Let's connect") + icon links (Email, LinkedIn, GitHub)
- Icons in #b8a0aa, tint to #a8566e on hover
- Minimal height — one row of icons + text

---

## 7. Content Needs (To Be Collected)

Before implementation, the following content is needed from Ritika:

- [ ] Profile photo (or confirm using initial "R" placeholder)
- [ ] Project details (titles, descriptions, tech stacks, outcomes) — e.g., Ashta Changa game and others
- [ ] Certifications and awards list (issuer, title, date, verify links)
- [ ] Blog posts or initial drafts (or confirm starting empty)
- [ ] About Me Q&A answers
- [ ] Fun stats for the snapshot section
- [ ] Social links (email, LinkedIn, GitHub URLs)
- [ ] Gruve project details fact-check (model names, exact parameters)

---

## 8. Phase 2 (Deferred)

- **Chatbot:** Interactive assistant embedded as an Astro island, potentially powered by an LLM, answering questions about Ritika's experience and projects. Design and implementation deferred to a separate spec.
