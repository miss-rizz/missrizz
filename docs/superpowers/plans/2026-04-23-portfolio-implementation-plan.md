# missrizz.info — Implementation Plan
**Date:** 2026-04-23
**Spec:** `docs/superpowers/specs/2026-04-23-portfolio-design.md`

---

## Prerequisites (from Ritika before building)

- [ ] Profile photo (high-res, portrait)
- [ ] Tagline text
- [ ] Job title / current role label
- [ ] Experience: title, company, location, dates, 1–2 sentence description, tech tags — for all 4 roles
- [ ] Projects: name, year, description, tech stack, link — for all projects
- [ ] Email address for "Hire me" + footer
- [ ] LinkedIn URL
- [ ] GitHub URL

---

## Phase 1 — Project scaffold

**Goal:** Bare-bones file structure, CDN links wired up, shared CSS variables defined.

### Steps
1. Create project root with these files:
   ```
   index.html
   experience.html
   projects.html
   assets/
     css/
       style.css
     img/
       photo.jpg        ← placeholder until real photo provided
   ```
2. In each HTML file, add `<head>` with:
   - Tailwind CSS CDN (`<script src="https://cdn.tailwindcss.com">`)
   - AOS CDN (CSS + JS)
   - Google Fonts: Inter
   - Link to `assets/css/style.css`
3. In `style.css`, define CSS custom properties:
   ```css
   :root {
     --indigo:   #4f46e5;
     --coral:    #fb7185;
     --indigo-dark: #1e1b4b;
     --indigo-light: #e0e7ff;
     --coral-light: #ffe4e6;
     --bg:       #f8f7ff;
   }
   ```
4. Add `AOS.init()` call in a shared `<script>` block at the bottom of each page.
5. Initialize a git repo, add `.gitignore` (ignore `.superpowers/`, `node_modules/`).

---

## Phase 2 — Shared components (nav + footer)

**Goal:** Build nav and footer once as HTML snippets, paste into all three pages.

### Nav
- Fixed/sticky, white background, `border-bottom: 1px solid var(--indigo-light)`
- Left: "Miss Rizz" bold link → `index.html`
- Right: Experience link · Projects link · "Hire me" pill button (→ `mailto:EMAIL`)
- Active page link gets coral underline (`border-bottom: 2px solid var(--coral)`)

### Footer
- Background `var(--indigo-dark)`
- Left: `© 2026 Ritika Agrawal` in muted lavender
- Right: LinkedIn · GitHub · Email — coral for email link, lavender for others

---

## Phase 3 — Home page (`index.html`)

**Goal:** Hero split layout, photo panel, text panel, entrance animations.

### Steps
1. Full-viewport two-column grid (`grid-cols-2`, height `100vh` minus nav)
2. **Left panel:** gradient background (`from-indigo-600 via-indigo-400 to-rose-400`). Photo anchored to bottom center.
3. **Right panel:** white background, vertically centered content:
   - Pill badge (role label)
   - Name (h1, `font-black`, last name in indigo)
   - Tagline paragraph
   - Two CTA buttons (filled + outlined)
   - Coral accent bar
4. Add AOS attributes: right panel `data-aos="fade-left"`, nav `data-aos="fade-down"`
5. Responsive: on mobile, stack columns vertically (photo panel shrinks, text panel full width)

---

## Phase 4 — Experience page (`experience.html`)

**Goal:** Vertical timeline with animated card entries.

### Steps
1. Page header section (white background, coral label, h2 heading, subtitle)
2. Timeline wrapper: relative positioned container, left border as the gradient line
3. For each experience entry, build a card component:
   - Dot on the timeline line (indigo or coral, alternating)
   - White card: year range, job title, company/location, description, skill tags, logo square
   - Most recent: full opacity. Each subsequent entry: reduce opacity by ~0.2
4. Add `data-aos="fade-up"` to each card with staggered `data-aos-delay`
5. Fill in real content once provided

---

## Phase 5 — Projects page (`projects.html`)

**Goal:** Same timeline layout as Experience, project-specific card content.

### Steps
1. Reuse page header pattern (coral label "WORK", h2 "Projects")
2. Reuse timeline + dot structure from Phase 4
3. Project card differences vs experience card:
   - No company/location — just year
   - External link button (↗) top-right of card
   - Link targets: GitHub repo, live site, or demo URL
4. Fill in real content once provided

---

## Phase 6 — Polish & responsiveness

**Goal:** Mobile-friendly, smooth animations, no rough edges.

### Steps
1. Test all three pages at 375px, 768px, 1280px widths
2. On mobile (`< 768px`): hero stacks vertically, nav collapses to hamburger or simplified links
3. Verify AOS animations don't cause layout shift
4. Check font loading (Inter), fallback stack set correctly
5. Validate all links (nav, footer, project ↗ buttons, "Hire me")
6. Lighthouse check: aim for 90+ performance, 100 accessibility

---

## Phase 7 — Deploy to Vercel + custom domain

**Goal:** Live at missrizz.info over HTTPS.

### Steps
1. Push repo to GitHub (public or private)
2. Go to vercel.com → "Add New Project" → import GitHub repo
3. No build command needed (static site) — set output directory to `/` (root)
4. Deploy → confirm it loads on the Vercel preview URL
5. In Vercel dashboard → Settings → Domains → add `missrizz.info`
6. At domain registrar (wherever missrizz.info is registered):
   - Add `A` record: `@` → `76.76.21.21` (Vercel IP)
   - Add `CNAME` record: `www` → `cname.vercel-dns.com`
7. Wait for DNS propagation (usually < 30 min)
8. Confirm `https://missrizz.info` loads with valid SSL

---

## Build order summary

| Phase | Deliverable | Blocked on |
|---|---|---|
| 1 | Scaffold + CSS vars | Nothing |
| 2 | Nav + footer | Phase 1 |
| 3 | Home page | Phase 2 + photo + content |
| 4 | Experience page | Phase 2 + experience content |
| 5 | Projects page | Phase 2 + projects content |
| 6 | Polish + mobile | Phases 3–5 |
| 7 | Deploy | Phase 6 + domain access |

Phases 1–2 can start immediately. Phases 3–5 need content from Ritika.
