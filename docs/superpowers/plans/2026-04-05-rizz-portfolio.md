# Rizz Portfolio Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Build a static personal portfolio site for Ritika "Rizz" Agrawal using Astro, with a muted rose design system, hamburger sidebar navigation, and content-collection-driven pages for experience, projects, blog, certifications, and about me.

**Architecture:** Astro static site with islands architecture. Shared layout wraps all pages (hamburger + sidebar island + footer). Content collections drive projects, blog, and certifications. Scoped CSS with CSS custom properties for the design system. One small client-side JS island for sidebar toggle and one for scroll-fade animations.

**Tech Stack:** Astro 5.x, TypeScript, CSS custom properties, Google Fonts (Nunito, Cormorant Garamond), Intersection Observer API

---

## File Structure

```
src/
├── layouts/
│   └── BaseLayout.astro          — Shared shell: <head>, fonts, blobs, hamburger, sidebar, footer
├── components/
│   ├── Sidebar.astro             — Sidebar nav panel (island, client:load)
│   ├── Hamburger.astro           — Hamburger icon button
│   ├── Footer.astro              — Contact footer strip
│   ├── Card.astro                — Reusable glassmorphism card
│   ├── InfoChip.astro            — Small pill/chip component
│   ├── ScrollFade.astro          — Wrapper that fades children in on scroll (client:visible)
│   └── TimelineEntry.astro       — Single timeline card for experience page
├── styles/
│   └── global.css                — CSS custom properties, reset, font imports, shared styles
├── content/
│   ├── config.ts                 — Content collection schemas (projects, blog, certifications)
│   ├── projects/
│   │   └── ashta-changa.md       — Sample project
│   ├── blog/
│   │   └── hello-world.md        — Sample blog post
│   └── certifications/
│       └── sample-cert.md        — Sample certification
├── pages/
│   ├── index.astro               — Landing page
│   ├── experience.astro          — Career timeline
│   ├── projects/
│   │   ├── index.astro           — Projects card grid
│   │   └── [...slug].astro       — Project detail page
│   ├── blog/
│   │   ├── index.astro           — Blog card grid
│   │   └── [...slug].astro       — Blog post page
│   ├── certifications.astro      — Certs & Awards card grid
│   └── about.astro               — About Me (snapshot + Q&A)
└── data/
    └── experience.ts             — Structured career data (companies, roles, highlights)
```

---

### Task 1: Scaffold Astro Project

**Files:**
- Create: `package.json`, `astro.config.mjs`, `tsconfig.json`, `src/pages/index.astro`

- [ ] **Step 1: Initialize Astro project**

Run from the project root `/Users/Ritika.Agrawal@gruve.ai/Desktop/rizz/rizz_info`:

```bash
npm create astro@latest . -- --template minimal --no-install --typescript strict
```

If prompted about overwriting, choose to proceed (the directory only has docs and .claude folders).

- [ ] **Step 2: Install dependencies**

```bash
npm install
```

Expected: `node_modules/` created, `package-lock.json` generated.

- [ ] **Step 3: Verify dev server starts**

```bash
npm run dev
```

Expected: Astro dev server starts on `http://localhost:4321` with the minimal template's default page.

- [ ] **Step 4: Clean the default index page**

Replace `src/pages/index.astro` with a bare placeholder:

```astro
---
// src/pages/index.astro
---
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>Rizz — Ritika Agrawal</title>
</head>
<body>
  <h1>Rizz</h1>
</body>
</html>
```

- [ ] **Step 5: Verify placeholder renders**

```bash
npm run dev
```

Open `http://localhost:4321`. Expected: page shows "Rizz" heading.

- [ ] **Step 6: Commit**

```bash
git init
echo "node_modules/\ndist/\n.astro/" > .gitignore
git add .
git commit -m "chore: scaffold Astro project with minimal template"
```

---

### Task 2: Global Styles & Design System

**Files:**
- Create: `src/styles/global.css`
- Create: `public/fonts/` (empty, fonts loaded via Google Fonts CDN)

- [ ] **Step 1: Create global CSS with custom properties and reset**

```css
/* src/styles/global.css */

/* === Reset === */
*, *::before, *::after {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

html {
  scroll-behavior: smooth;
}

/* === Design Tokens === */
:root {
  /* Colors */
  --color-bg: #faf5f7;
  --color-text: #3d2e36;
  --color-accent: #a8566e;
  --color-muted-text: #8a6a78;
  --color-muted-label: #b8a0aa;
  --color-card-bg: rgba(255, 255, 255, 0.65);
  --color-card-border: rgba(200, 160, 175, 0.15);
  --color-blob-1: #f0c0cf;
  --color-blob-2: #e4cdd6;
  --color-blob-3: #fce4ec;
  --color-divider: #e0d0d8;

  /* Typography */
  --font-body: 'Nunito', sans-serif;
  --font-accent: 'Cormorant Garamond', serif;

  /* Spacing */
  --space-xs: 4px;
  --space-sm: 8px;
  --space-md: 16px;
  --space-lg: 24px;
  --space-xl: 40px;
  --space-2xl: 80px;

  /* Card */
  --card-radius: 20px;
  --card-blur: 12px;
  --card-shadow: 0 4px 24px rgba(170, 110, 130, 0.08);
}

/* === Base === */
body {
  font-family: var(--font-body);
  background: var(--color-bg);
  color: var(--color-text);
  line-height: 1.6;
  -webkit-font-smoothing: antialiased;
}

a {
  color: var(--color-accent);
  text-decoration: none;
}

a:hover {
  text-decoration: underline;
}

/* === Utility: Section Label === */
.section-label {
  font-size: 11px;
  text-transform: uppercase;
  letter-spacing: 3px;
  color: var(--color-muted-label);
  font-weight: 400;
  margin-bottom: var(--space-lg);
}

/* === Utility: Divider === */
.divider {
  width: 40px;
  height: 1px;
  background: var(--color-divider);
  margin: 0 auto;
}

/* === Utility: Scroll Fade === */
.scroll-fade {
  opacity: 0;
  transform: translateY(20px);
  transition: opacity 0.6s ease, transform 0.6s ease;
}

.scroll-fade.visible {
  opacity: 1;
  transform: translateY(0);
}
```

- [ ] **Step 2: Verify the file was created**

```bash
cat src/styles/global.css | head -5
```

Expected: Shows the reset rules.

- [ ] **Step 3: Commit**

```bash
git add src/styles/global.css
git commit -m "feat: add global CSS with design system tokens and reset"
```

---

### Task 3: Base Layout (Shell)

**Files:**
- Create: `src/layouts/BaseLayout.astro`
- Create: `src/components/Footer.astro`
- Create: `src/components/Hamburger.astro`
- Modify: `src/pages/index.astro`

- [ ] **Step 1: Create Footer component**

```astro
---
// src/components/Footer.astro
---
<footer class="footer">
  <span class="footer-text">Let's connect</span>
  <nav class="footer-links">
    <a href="mailto:ritika@example.com" aria-label="Email" class="footer-icon">
      <svg width="20" height="20" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.5" stroke-linecap="round" stroke-linejoin="round">
        <rect x="2" y="4" width="20" height="16" rx="2"/>
        <path d="M22 4L12 13L2 4"/>
      </svg>
    </a>
    <a href="https://linkedin.com/in/" target="_blank" rel="noopener" aria-label="LinkedIn" class="footer-icon">
      <svg width="20" height="20" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.5" stroke-linecap="round" stroke-linejoin="round">
        <path d="M16 8a6 6 0 0 1 6 6v7h-4v-7a2 2 0 0 0-4 0v7h-4v-7a6 6 0 0 1 6-6z"/>
        <rect x="2" y="9" width="4" height="12"/>
        <circle cx="4" cy="4" r="2"/>
      </svg>
    </a>
    <a href="https://github.com/" target="_blank" rel="noopener" aria-label="GitHub" class="footer-icon">
      <svg width="20" height="20" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.5" stroke-linecap="round" stroke-linejoin="round">
        <path d="M9 19c-5 1.5-5-2.5-7-3m14 6v-3.87a3.37 3.37 0 0 0-.94-2.61c3.14-.35 6.44-1.54 6.44-7A5.44 5.44 0 0 0 20 4.77 5.07 5.07 0 0 0 19.91 1S18.73.65 16 2.48a13.38 13.38 0 0 0-7 0C6.27.65 5.09 1 5.09 1A5.07 5.07 0 0 0 5 4.77a5.44 5.44 0 0 0-1.5 3.78c0 5.42 3.3 6.61 6.44 7A3.37 3.37 0 0 0 9 18.13V22"/>
      </svg>
    </a>
  </nav>
</footer>

<style>
  .footer {
    display: flex;
    align-items: center;
    justify-content: center;
    gap: 20px;
    padding: 24px 40px;
    border-top: 1px solid var(--color-divider);
  }

  .footer-text {
    font-size: 13px;
    font-weight: 300;
    color: var(--color-muted-label);
  }

  .footer-links {
    display: flex;
    gap: 16px;
  }

  .footer-icon {
    color: var(--color-muted-label);
    transition: color 0.2s ease;
  }

  .footer-icon:hover {
    color: var(--color-accent);
    text-decoration: none;
  }
</style>
```

- [ ] **Step 2: Create Hamburger component**

```astro
---
// src/components/Hamburger.astro
---
<button class="hamburger" aria-label="Open menu" data-hamburger>
  <span></span>
  <span></span>
  <span></span>
</button>

<style>
  .hamburger {
    position: fixed;
    top: 28px;
    right: 32px;
    z-index: 100;
    display: flex;
    flex-direction: column;
    gap: 5px;
    cursor: pointer;
    background: none;
    border: none;
    padding: 4px;
  }

  .hamburger span {
    width: 26px;
    height: 2px;
    background: var(--color-muted-label);
    border-radius: 2px;
    transition: transform 0.3s ease, opacity 0.3s ease;
  }

  .hamburger[aria-expanded="true"] span:nth-child(1) {
    transform: rotate(45deg) translate(5px, 5px);
  }
  .hamburger[aria-expanded="true"] span:nth-child(2) {
    opacity: 0;
  }
  .hamburger[aria-expanded="true"] span:nth-child(3) {
    transform: rotate(-45deg) translate(5px, -5px);
  }
</style>
```

- [ ] **Step 3: Create BaseLayout**

```astro
---
// src/layouts/BaseLayout.astro
import Footer from '../components/Footer.astro';
import Hamburger from '../components/Hamburger.astro';
import '../styles/global.css';

interface Props {
  title: string;
  showBlobs?: boolean;
}

const { title, showBlobs = false } = Astro.props;
const currentPath = Astro.url.pathname;
---
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <link rel="preconnect" href="https://fonts.googleapis.com" />
  <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin />
  <link href="https://fonts.googleapis.com/css2?family=Nunito:ital,wght@0,200;0,300;0,400;0,500;0,600;0,700;1,300&family=Cormorant+Garamond:ital,wght@1,300;1,400&display=swap" rel="stylesheet" />
  <title>{title}</title>
</head>
<body>
  {showBlobs && (
    <div class="blob blob-1"></div>
    <div class="blob blob-2"></div>
    <div class="blob blob-3"></div>
  )}

  <Hamburger />

  <!-- Sidebar overlay -->
  <div class="sidebar-overlay" data-sidebar-overlay></div>

  <!-- Sidebar -->
  <nav class="sidebar" data-sidebar>
    <a href="/" class:list={["sidebar-link", { active: currentPath === "/" }]}>
      <svg width="16" height="16" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.5"><path d="M3 9l9-7 9 7v11a2 2 0 0 1-2 2H5a2 2 0 0 1-2-2z"/></svg>
      Home
    </a>
    <a href="/experience" class:list={["sidebar-link", { active: currentPath === "/experience" || currentPath === "/experience/" }]}>
      <svg width="16" height="16" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.5"><rect x="2" y="7" width="20" height="14" rx="2"/><path d="M16 7V5a2 2 0 0 0-2-2h-4a2 2 0 0 0-2 2v2"/></svg>
      Experience
    </a>
    <a href="/projects" class:list={["sidebar-link", { active: currentPath.startsWith("/projects") }]}>
      <svg width="16" height="16" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.5"><rect x="3" y="3" width="7" height="7" rx="1"/><rect x="14" y="3" width="7" height="7" rx="1"/><rect x="3" y="14" width="7" height="7" rx="1"/><rect x="14" y="14" width="7" height="7" rx="1"/></svg>
      Projects
    </a>
    <a href="/blog" class:list={["sidebar-link", { active: currentPath.startsWith("/blog") }]}>
      <svg width="16" height="16" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.5"><path d="M12 20h9"/><path d="M16.5 3.5a2.121 2.121 0 0 1 3 3L7 19l-4 1 1-4L16.5 3.5z"/></svg>
      Blog
    </a>
    <a href="/certifications" class:list={["sidebar-link", { active: currentPath === "/certifications" || currentPath === "/certifications/" }]}>
      <svg width="16" height="16" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.5"><circle cx="12" cy="8" r="6"/><path d="M8.21 13.89L7 23l5-3 5 3-1.21-9.12"/></svg>
      Certs & Awards
    </a>
    <a href="/about" class:list={["sidebar-link", { active: currentPath === "/about" || currentPath === "/about/" }]}>
      <svg width="16" height="16" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.5"><path d="M20 21v-2a4 4 0 0 0-4-4H8a4 4 0 0 0-4 4v2"/><circle cx="12" cy="7" r="4"/></svg>
      About Me
    </a>
  </nav>

  <main>
    <slot />
  </main>

  <Footer />

  <script>
    const hamburger = document.querySelector('[data-hamburger]');
    const sidebar = document.querySelector('[data-sidebar]');
    const overlay = document.querySelector('[data-sidebar-overlay]');

    function toggleSidebar() {
      const isOpen = sidebar?.classList.toggle('open');
      overlay?.classList.toggle('open', isOpen);
      hamburger?.setAttribute('aria-expanded', String(isOpen));
    }

    function closeSidebar() {
      sidebar?.classList.remove('open');
      overlay?.classList.remove('open');
      hamburger?.setAttribute('aria-expanded', 'false');
    }

    hamburger?.addEventListener('click', toggleSidebar);
    overlay?.addEventListener('click', closeSidebar);
  </script>
</body>
</html>

<style>
  /* Blobs */
  .blob {
    position: fixed;
    border-radius: 50%;
    filter: blur(100px);
    opacity: 0.4;
    pointer-events: none;
    z-index: 0;
  }
  .blob-1 { width: 420px; height: 420px; background: var(--color-blob-1); top: -130px; right: -100px; }
  .blob-2 { width: 340px; height: 340px; background: var(--color-blob-2); bottom: -110px; left: -70px; }
  .blob-3 { width: 240px; height: 240px; background: var(--color-blob-3); top: 50%; left: 55%; }

  /* Sidebar overlay */
  .sidebar-overlay {
    position: fixed;
    inset: 0;
    background: rgba(61, 46, 54, 0.15);
    z-index: 90;
    opacity: 0;
    pointer-events: none;
    transition: opacity 0.3s ease;
  }
  .sidebar-overlay.open {
    opacity: 1;
    pointer-events: auto;
  }

  /* Sidebar panel */
  .sidebar {
    position: fixed;
    top: 0;
    right: 0;
    width: 220px;
    height: 100vh;
    background: rgba(250, 245, 247, 0.85);
    backdrop-filter: blur(20px);
    -webkit-backdrop-filter: blur(20px);
    z-index: 95;
    transform: translateX(100%);
    transition: transform 0.3s ease;
    display: flex;
    flex-direction: column;
    padding: 80px 24px 40px;
    gap: 8px;
  }
  .sidebar.open {
    transform: translateX(0);
  }

  .sidebar-link {
    display: flex;
    align-items: center;
    gap: 10px;
    padding: 10px 12px;
    border-radius: 10px;
    font-size: 14px;
    font-weight: 400;
    color: var(--color-muted-text);
    text-decoration: none;
    transition: background 0.2s ease, color 0.2s ease;
  }
  .sidebar-link:hover {
    background: rgba(200, 160, 175, 0.1);
    color: var(--color-text);
    text-decoration: none;
  }
  .sidebar-link.active {
    color: var(--color-accent);
    font-weight: 600;
  }
  .sidebar-link svg {
    flex-shrink: 0;
  }

  main {
    position: relative;
    z-index: 1;
  }

  @media (max-width: 640px) {
    .sidebar {
      width: 100%;
    }
  }
</style>
```

- [ ] **Step 4: Update index.astro to use BaseLayout**

```astro
---
// src/pages/index.astro
import BaseLayout from '../layouts/BaseLayout.astro';
---
<BaseLayout title="Rizz — Ritika Agrawal" showBlobs={true}>
  <h1>Rizz</h1>
</BaseLayout>
```

- [ ] **Step 5: Verify layout renders with hamburger + sidebar + footer**

```bash
npm run dev
```

Open `http://localhost:4321`. Expected: "Rizz" heading visible, hamburger top-right, clicking it slides in the sidebar from the right with nav links, footer at bottom with icons.

- [ ] **Step 6: Commit**

```bash
git add src/layouts/ src/components/Footer.astro src/components/Hamburger.astro src/pages/index.astro src/styles/global.css
git commit -m "feat: add base layout with hamburger sidebar navigation and footer"
```

---

### Task 4: Reusable Card & InfoChip Components

**Files:**
- Create: `src/components/Card.astro`
- Create: `src/components/InfoChip.astro`

- [ ] **Step 1: Create Card component**

```astro
---
// src/components/Card.astro
interface Props {
  href?: string;
  class?: string;
}

const { href, class: className } = Astro.props;
const Tag = href ? 'a' : 'div';
---
<Tag href={href} class:list={["card", className]} data-card>
  <slot />
</Tag>

<style>
  .card {
    background: var(--color-card-bg);
    backdrop-filter: blur(var(--card-blur));
    -webkit-backdrop-filter: blur(var(--card-blur));
    border-radius: var(--card-radius);
    border: 1px solid var(--color-card-border);
    box-shadow: var(--card-shadow);
    padding: var(--space-lg);
    text-decoration: none;
    color: inherit;
    display: block;
    transition: transform 0.2s ease, box-shadow 0.2s ease;
  }

  a.card:hover {
    transform: translateY(-2px);
    box-shadow: 0 8px 32px rgba(170, 110, 130, 0.12);
    text-decoration: none;
  }
</style>
```

- [ ] **Step 2: Create InfoChip component**

```astro
---
// src/components/InfoChip.astro
interface Props {
  class?: string;
}

const { class: className } = Astro.props;
---
<span class:list={["info-chip", className]}>
  <slot />
</span>

<style>
  .info-chip {
    padding: 6px 18px;
    background: rgba(255, 255, 255, 0.6);
    backdrop-filter: blur(12px);
    -webkit-backdrop-filter: blur(12px);
    border-radius: 18px;
    border: 1px solid var(--color-card-border);
    font-size: 13px;
    color: var(--color-muted-text);
    font-weight: 400;
    white-space: nowrap;
  }
</style>
```

- [ ] **Step 3: Commit**

```bash
git add src/components/Card.astro src/components/InfoChip.astro
git commit -m "feat: add reusable Card and InfoChip components"
```

---

### Task 5: Landing Page

**Files:**
- Modify: `src/pages/index.astro`

- [ ] **Step 1: Build the full landing page**

```astro
---
// src/pages/index.astro
import BaseLayout from '../layouts/BaseLayout.astro';
import InfoChip from '../components/InfoChip.astro';
---
<BaseLayout title="Rizz — Ritika Agrawal" showBlobs={true}>

  <section class="hero">
    <div class="hero-content">
      <div class="photo">R</div>
      <div class="rizz">Rizz</div>
      <div class="full-name">Ritika <span>Agrawal</span></div>
      <div class="tagline">turning ideas into systems, fueled by tea</div>
    </div>
    <div class="scroll-hint">
      <div class="scroll-arrow"></div>
    </div>
  </section>

  <div class="divider"></div>

  <section class="below-fold">
    <div class="section-label">At a glance</div>
    <div class="info-strip">
      <InfoChip>5+ years experience</InfoChip>
      <span class="dot"></span>
      <InfoChip>IIT BHU · ECE '21</InfoChip>
      <span class="dot"></span>
      <InfoChip>Backend · AI · Infra</InfoChip>
    </div>

    <div class="section-label">Worked with</div>
    <div class="companies">
      <span class="co">Mastercard</span><span class="sep"></span>
      <span class="co">High Insight</span><span class="sep"></span>
      <span class="co">Atomic Ads</span><span class="sep"></span>
      <span class="co">Gruve</span>
    </div>

    <div class="section-label">Skills</div>
    <div class="skills">
      <InfoChip>Java</InfoChip>
      <InfoChip>Python</InfoChip>
      <InfoChip>Kubernetes</InfoChip>
      <InfoChip>LangGraph</InfoChip>
      <InfoChip>Ansible</InfoChip>
      <InfoChip>DevOps</InfoChip>
      <InfoChip>AI / ML</InfoChip>
      <InfoChip>Distributed Systems</InfoChip>
    </div>
  </section>

</BaseLayout>

<style>
  .hero {
    min-height: 100vh;
    position: relative;
    display: flex;
    align-items: center;
    justify-content: center;
    flex-direction: column;
  }

  .hero-content {
    position: relative;
    z-index: 1;
    display: flex;
    flex-direction: column;
    align-items: center;
  }

  .photo {
    width: 300px;
    height: 300px;
    border-radius: 50%;
    background: linear-gradient(135deg, #e4bcc9, #cfa4b4);
    display: flex;
    align-items: center;
    justify-content: center;
    font-size: 90px;
    color: white;
    font-weight: 200;
    box-shadow: 0 18px 60px rgba(170, 110, 130, 0.2);
  }

  .rizz {
    font-size: 14px;
    font-weight: 400;
    color: var(--color-accent);
    letter-spacing: 5px;
    text-transform: uppercase;
    text-align: center;
    margin-top: 12px;
  }

  .full-name {
    font-size: 42px;
    font-weight: 300;
    color: var(--color-text);
    letter-spacing: 6px;
    text-transform: uppercase;
    text-align: center;
    line-height: 1.1;
    margin-top: 40px;
  }

  .full-name span {
    font-weight: 600;
    color: var(--color-accent);
  }

  .tagline {
    font-family: var(--font-body);
    font-weight: 300;
    font-size: 16px;
    text-align: center;
    color: var(--color-muted-text);
    font-style: italic;
    margin-top: 28px;
  }

  .scroll-hint {
    position: absolute;
    bottom: 36px;
    left: 50%;
    transform: translateX(-50%);
    z-index: 1;
    display: flex;
    flex-direction: column;
    align-items: center;
    gap: 6px;
    color: var(--color-muted-label);
    animation: bob 2.5s ease-in-out infinite;
  }

  .scroll-arrow {
    width: 18px;
    height: 18px;
    border-right: 1.5px solid var(--color-muted-label);
    border-bottom: 1.5px solid var(--color-muted-label);
    transform: rotate(45deg);
  }

  @keyframes bob {
    0%, 100% { transform: translateX(-50%) translateY(0); }
    50% { transform: translateX(-50%) translateY(8px); }
  }

  .below-fold {
    padding: var(--space-2xl) var(--space-xl);
    max-width: 800px;
    margin: 0 auto;
  }

  .info-strip {
    display: flex;
    align-items: center;
    gap: 10px;
    flex-wrap: wrap;
    margin-bottom: var(--space-xl);
  }

  .dot {
    width: 3px;
    height: 3px;
    border-radius: 50%;
    background: #c8a8b4;
  }

  .companies {
    display: flex;
    align-items: center;
    gap: 20px;
    font-size: 15px;
    font-weight: 300;
    color: var(--color-muted-label);
    margin-bottom: 48px;
  }
  .co { color: var(--color-muted-text); font-weight: 400; }
  .sep { width: 6px; height: 1px; background: #d4bec8; display: inline-block; }

  .skills {
    display: flex;
    gap: 10px;
    flex-wrap: wrap;
  }

  @media (max-width: 640px) {
    .photo {
      width: 200px;
      height: 200px;
      font-size: 60px;
    }
    .full-name {
      font-size: 28px;
      letter-spacing: 3px;
    }
    .below-fold {
      padding: var(--space-xl) var(--space-md);
    }
  }
</style>
```

- [ ] **Step 2: Verify landing page matches approved mockup**

```bash
npm run dev
```

Open `http://localhost:4321`. Expected: Full-viewport hero with 300px circle, RIZZ label, RITIKA AGRAWAL name, tagline. Scroll down for chips, companies, skills. Hamburger → sidebar works. Footer at bottom.

- [ ] **Step 3: Commit**

```bash
git add src/pages/index.astro
git commit -m "feat: build landing page with hero, below-fold sections"
```

---

### Task 6: Experience Data & Page

**Files:**
- Create: `src/data/experience.ts`
- Create: `src/components/TimelineEntry.astro`
- Create: `src/pages/experience.astro`

- [ ] **Step 1: Create experience data**

```typescript
// src/data/experience.ts
export interface Experience {
  company: string;
  role: string;
  period: string;
  highlights: string[];
}

export const experiences: Experience[] = [
  {
    company: "Gruve",
    role: "Software Developer",
    period: "Jan 2026 – Present",
    highlights: [
      "Building GPU inferencing infrastructure on Kubernetes clusters",
      "Designing end-to-end logging, alerting, and observability architecture",
      "Deploying open-source LLMs with model sharding across GPU nodes",
    ],
  },
  {
    company: "Atomic Ads",
    role: "Full-Stack Developer",
    period: "2025 – 2026",
    highlights: [
      "Built SaaS foundation — UI, backend services, and data pipelines",
      "Created AI agentic system using LangGraph with swarm architecture",
      "Integrated ad platforms (DV360, TTD, Vistar) for automated campaign management",
    ],
  },
  {
    company: "High Insight",
    role: "ML Analyst & JS Automator",
    period: "2024 – 2025",
    highlights: [
      "Data analysis and cycle predictions for ERP modules (OPEX, HR, COGS)",
      "Client-facing requirements gathering and dashboarding on SAP SAC",
    ],
  },
  {
    company: "Mastercard",
    role: "Software Engineer",
    period: "Jul 2021 – 2024",
    highlights: [
      "DevOps and engineering on legacy multi-service Java architecture",
      "Created Ansible deployment pipeline for frontend + backend services",
      "Reduced vulnerability backlog from ~150 to 20-30 across services",
      "Won 3rd prize leading team in company-wide competition",
    ],
  },
];
```

- [ ] **Step 2: Create TimelineEntry component**

```astro
---
// src/components/TimelineEntry.astro
import Card from './Card.astro';

interface Props {
  company: string;
  role: string;
  period: string;
  highlights: string[];
}

const { company, role, period, highlights } = Astro.props;
---
<div class="timeline-entry scroll-fade">
  <div class="timeline-dot"></div>
  <Card>
    <h3 class="entry-company">{company}</h3>
    <p class="entry-role">{role}</p>
    <p class="entry-period">{period}</p>
    <ul class="entry-highlights">
      {highlights.map((h) => <li>{h}</li>)}
    </ul>
  </Card>
</div>

<style>
  .timeline-entry {
    position: relative;
    padding-left: 36px;
    margin-bottom: var(--space-lg);
  }

  .timeline-dot {
    position: absolute;
    left: 0;
    top: 28px;
    width: 10px;
    height: 10px;
    border-radius: 50%;
    background: var(--color-accent);
    border: 2px solid var(--color-bg);
    box-shadow: 0 0 0 2px var(--color-divider);
  }

  .entry-company {
    font-size: 18px;
    font-weight: 600;
    color: var(--color-text);
    margin-bottom: 4px;
  }

  .entry-role {
    font-size: 14px;
    font-weight: 400;
    color: var(--color-accent);
    margin-bottom: 2px;
  }

  .entry-period {
    font-size: 12px;
    color: var(--color-muted-label);
    margin-bottom: 12px;
  }

  .entry-highlights {
    list-style: none;
    display: flex;
    flex-direction: column;
    gap: 6px;
  }

  .entry-highlights li {
    font-size: 14px;
    font-weight: 300;
    color: var(--color-muted-text);
    padding-left: 14px;
    position: relative;
  }

  .entry-highlights li::before {
    content: '';
    position: absolute;
    left: 0;
    top: 8px;
    width: 4px;
    height: 4px;
    border-radius: 50%;
    background: var(--color-divider);
  }
</style>
```

- [ ] **Step 3: Create Experience page**

```astro
---
// src/pages/experience.astro
import BaseLayout from '../layouts/BaseLayout.astro';
import TimelineEntry from '../components/TimelineEntry.astro';
import { experiences } from '../data/experience';
---
<BaseLayout title="Experience — Rizz">

  <section class="page-section">
    <div class="section-label">Experience</div>

    <div class="timeline">
      {experiences.map((exp) => (
        <TimelineEntry
          company={exp.company}
          role={exp.role}
          period={exp.period}
          highlights={exp.highlights}
        />
      ))}
    </div>
  </section>

</BaseLayout>

<style>
  .page-section {
    padding: var(--space-2xl) var(--space-xl);
    max-width: 800px;
    margin: 0 auto;
  }

  .timeline {
    position: relative;
    padding-left: 5px;
  }

  .timeline::before {
    content: '';
    position: absolute;
    left: 4px;
    top: 0;
    bottom: 0;
    width: 1px;
    background: var(--color-divider);
  }
</style>
```

- [ ] **Step 4: Verify experience page**

```bash
npm run dev
```

Navigate to `http://localhost:4321/experience`. Expected: Page title "EXPERIENCE", vertical timeline line on left, four stacked cards (Gruve, Atomic Ads, High Insight, Mastercard) each with company name, role, period, and bullet highlights.

- [ ] **Step 5: Commit**

```bash
git add src/data/experience.ts src/components/TimelineEntry.astro src/pages/experience.astro
git commit -m "feat: add experience page with vertical timeline and career data"
```

---

### Task 7: Content Collections Setup

**Files:**
- Create: `src/content.config.ts`
- Create: `src/content/projects/ashta-changa.md`
- Create: `src/content/blog/hello-world.md`
- Create: `src/content/certifications/sample-cert.md`

- [ ] **Step 1: Create content collection config**

```typescript
// src/content.config.ts
import { defineCollection, z } from 'astro:content';
import { glob } from 'astro/loaders';

const projects = defineCollection({
  loader: glob({ pattern: '**/*.md', base: './src/content/projects' }),
  schema: z.object({
    title: z.string(),
    description: z.string(),
    tags: z.array(z.string()),
    image: z.string().optional(),
    order: z.number().optional().default(0),
  }),
});

const blog = defineCollection({
  loader: glob({ pattern: '**/*.{md,mdx}', base: './src/content/blog' }),
  schema: z.object({
    title: z.string(),
    date: z.coerce.date(),
    category: z.string(),
    excerpt: z.string(),
  }),
});

const certifications = defineCollection({
  loader: glob({ pattern: '**/*.md', base: './src/content/certifications' }),
  schema: z.object({
    title: z.string(),
    issuer: z.string(),
    date: z.coerce.date(),
    type: z.enum(['Certification', 'Award']),
    verifyUrl: z.string().optional(),
  }),
});

export const collections = { projects, blog, certifications };
```

- [ ] **Step 2: Create sample project**

```markdown
---
title: Ashta Changa
description: A digital adaptation of the classic Indian board game
tags: ["Python", "Game Dev"]
order: 1
---

## Problem

Traditional board games are being forgotten in the digital age.

## Approach

Built a digital version of Ashta Changa, a classic Indian board game, preserving the authentic rules and feel.

## Outcome

A playable digital game that brings a traditional experience to modern screens.
```

Save to `src/content/projects/ashta-changa.md`.

- [ ] **Step 3: Create sample blog post**

```markdown
---
title: "Hello World"
date: 2026-04-05
category: "Thoughts"
excerpt: "First post on the new site — why I built this and what's coming next."
---

Welcome to my corner of the internet. This is where I'll share thoughts on backend engineering, AI systems, DevOps, and the occasional tea recommendation.

Stay tuned.
```

Save to `src/content/blog/hello-world.md`.

- [ ] **Step 4: Create sample certification**

```markdown
---
title: "Sample Certification"
issuer: "Example Issuer"
date: 2025-01-15
type: "Certification"
verifyUrl: "https://example.com/verify"
---

Placeholder certification entry. Replace with real certifications.
```

Save to `src/content/certifications/sample-cert.md`.

- [ ] **Step 5: Verify collections load**

```bash
npm run dev
```

Expected: No errors on startup. Astro picks up the content collections from `src/content.config.ts`.

- [ ] **Step 6: Commit**

```bash
git add src/content.config.ts src/content/
git commit -m "feat: add content collections for projects, blog, and certifications"
```

---

### Task 8: Projects Pages (Grid + Detail)

**Files:**
- Create: `src/pages/projects/index.astro`
- Create: `src/pages/projects/[...slug].astro`

- [ ] **Step 1: Create projects grid page**

```astro
---
// src/pages/projects/index.astro
import BaseLayout from '../../layouts/BaseLayout.astro';
import Card from '../../components/Card.astro';
import InfoChip from '../../components/InfoChip.astro';
import { getCollection } from 'astro:content';

const projects = (await getCollection('projects')).sort((a, b) => (a.data.order ?? 0) - (b.data.order ?? 0));
---
<BaseLayout title="Projects — Rizz">

  <section class="page-section">
    <div class="section-label">Projects</div>

    <div class="card-grid">
      {projects.map((project) => (
        <Card href={`/projects/${project.id}`} class="project-card">
          <div class="card-thumb" />
          <h3 class="card-title">{project.data.title}</h3>
          <p class="card-desc">{project.data.description}</p>
          <div class="card-tags">
            {project.data.tags.map((tag) => <InfoChip>{tag}</InfoChip>)}
          </div>
        </Card>
      ))}
    </div>
  </section>

</BaseLayout>

<style>
  .page-section {
    padding: var(--space-2xl) var(--space-xl);
    max-width: 960px;
    margin: 0 auto;
  }

  .card-grid {
    display: grid;
    grid-template-columns: repeat(auto-fill, minmax(280px, 1fr));
    gap: var(--space-lg);
  }

  .card-thumb {
    width: 100%;
    height: 140px;
    border-radius: 12px;
    background: linear-gradient(135deg, var(--color-blob-1), var(--color-blob-3));
    margin-bottom: var(--space-md);
  }

  .card-title {
    font-size: 18px;
    font-weight: 600;
    color: var(--color-text);
    margin-bottom: var(--space-xs);
  }

  .card-desc {
    font-size: 14px;
    font-weight: 300;
    color: var(--color-muted-text);
    margin-bottom: var(--space-sm);
  }

  .card-tags {
    display: flex;
    gap: 6px;
    flex-wrap: wrap;
  }
</style>
```

- [ ] **Step 2: Create project detail page**

```astro
---
// src/pages/projects/[...slug].astro
import BaseLayout from '../../layouts/BaseLayout.astro';
import InfoChip from '../../components/InfoChip.astro';
import { getCollection, render } from 'astro:content';

export async function getStaticPaths() {
  const projects = await getCollection('projects');
  return projects.map((project) => ({
    params: { slug: project.id },
    props: { project },
  }));
}

const { project } = Astro.props;
const { Content } = await render(project);
---
<BaseLayout title={`${project.data.title} — Rizz`}>

  <section class="project-detail">
    <a href="/projects" class="back-link">← Back to Projects</a>
    <h1 class="project-title">{project.data.title}</h1>
    <div class="project-tags">
      {project.data.tags.map((tag) => <InfoChip>{tag}</InfoChip>)}
    </div>
    <div class="project-body">
      <Content />
    </div>
  </section>

</BaseLayout>

<style>
  .project-detail {
    padding: var(--space-2xl) var(--space-xl);
    max-width: 800px;
    margin: 0 auto;
  }

  .back-link {
    font-size: 13px;
    color: var(--color-muted-label);
    display: inline-block;
    margin-bottom: var(--space-lg);
  }

  .project-title {
    font-size: 32px;
    font-weight: 600;
    color: var(--color-text);
    margin-bottom: var(--space-md);
  }

  .project-tags {
    display: flex;
    gap: 8px;
    flex-wrap: wrap;
    margin-bottom: var(--space-xl);
  }

  .project-body {
    font-size: 15px;
    font-weight: 300;
    line-height: 1.8;
    color: var(--color-text);
  }

  .project-body :global(h2) {
    font-size: 18px;
    font-weight: 600;
    color: var(--color-accent);
    margin-top: var(--space-xl);
    margin-bottom: var(--space-sm);
  }

  .project-body :global(p) {
    margin-bottom: var(--space-md);
  }
</style>
```

- [ ] **Step 3: Verify projects pages**

```bash
npm run dev
```

Navigate to `http://localhost:4321/projects`. Expected: Grid with one card (Ashta Changa). Click it → detail page with title, tags, and markdown body. Back link returns to grid.

- [ ] **Step 4: Commit**

```bash
git add src/pages/projects/
git commit -m "feat: add projects grid and detail pages with content collection"
```

---

### Task 9: Blog Pages (Grid + Post)

**Files:**
- Create: `src/pages/blog/index.astro`
- Create: `src/pages/blog/[...slug].astro`

- [ ] **Step 1: Create blog grid page**

```astro
---
// src/pages/blog/index.astro
import BaseLayout from '../../layouts/BaseLayout.astro';
import Card from '../../components/Card.astro';
import { getCollection } from 'astro:content';

const posts = (await getCollection('blog')).sort(
  (a, b) => b.data.date.getTime() - a.data.date.getTime()
);
---
<BaseLayout title="Blog — Rizz">

  <section class="page-section">
    <div class="section-label">Blog</div>

    <div class="card-grid">
      {posts.map((post) => (
        <Card href={`/blog/${post.id}`} class="blog-card">
          <span class="card-category">{post.data.category}</span>
          <h3 class="card-title">{post.data.title}</h3>
          <p class="card-date">{post.data.date.toLocaleDateString('en-US', { year: 'numeric', month: 'short', day: 'numeric' })}</p>
          <p class="card-excerpt">{post.data.excerpt}</p>
        </Card>
      ))}
    </div>
  </section>

</BaseLayout>

<style>
  .page-section {
    padding: var(--space-2xl) var(--space-xl);
    max-width: 960px;
    margin: 0 auto;
  }

  .card-grid {
    display: grid;
    grid-template-columns: repeat(auto-fill, minmax(280px, 1fr));
    gap: var(--space-lg);
  }

  .card-category {
    font-size: 11px;
    text-transform: uppercase;
    letter-spacing: 2px;
    color: var(--color-accent);
    font-weight: 500;
    margin-bottom: var(--space-sm);
    display: block;
  }

  .card-title {
    font-size: 18px;
    font-weight: 600;
    color: var(--color-text);
    margin-bottom: var(--space-xs);
  }

  .card-date {
    font-size: 12px;
    color: var(--color-muted-label);
    margin-bottom: var(--space-sm);
  }

  .card-excerpt {
    font-size: 14px;
    font-weight: 300;
    color: var(--color-muted-text);
    line-height: 1.5;
  }
</style>
```

- [ ] **Step 2: Create blog post page**

```astro
---
// src/pages/blog/[...slug].astro
import BaseLayout from '../../layouts/BaseLayout.astro';
import { getCollection, render } from 'astro:content';

export async function getStaticPaths() {
  const posts = await getCollection('blog');
  return posts.map((post) => ({
    params: { slug: post.id },
    props: { post },
  }));
}

const { post } = Astro.props;
const { Content } = await render(post);
---
<BaseLayout title={`${post.data.title} — Rizz`}>

  <article class="blog-post">
    <a href="/blog" class="back-link">← Back to Blog</a>
    <span class="post-category">{post.data.category}</span>
    <h1 class="post-title">{post.data.title}</h1>
    <p class="post-date">{post.data.date.toLocaleDateString('en-US', { year: 'numeric', month: 'long', day: 'numeric' })}</p>
    <div class="post-body">
      <Content />
    </div>
  </article>

</BaseLayout>

<style>
  .blog-post {
    padding: var(--space-2xl) var(--space-xl);
    max-width: 720px;
    margin: 0 auto;
  }

  .back-link {
    font-size: 13px;
    color: var(--color-muted-label);
    display: inline-block;
    margin-bottom: var(--space-lg);
  }

  .post-category {
    font-size: 11px;
    text-transform: uppercase;
    letter-spacing: 2px;
    color: var(--color-accent);
    font-weight: 500;
    display: block;
    margin-bottom: var(--space-sm);
  }

  .post-title {
    font-size: 32px;
    font-weight: 600;
    color: var(--color-text);
    margin-bottom: var(--space-sm);
  }

  .post-date {
    font-size: 13px;
    color: var(--color-muted-label);
    margin-bottom: var(--space-xl);
  }

  .post-body {
    font-size: 16px;
    font-weight: 300;
    line-height: 1.8;
    color: var(--color-text);
  }

  .post-body :global(p) {
    margin-bottom: var(--space-md);
  }

  .post-body :global(h2) {
    font-size: 22px;
    font-weight: 600;
    margin-top: var(--space-xl);
    margin-bottom: var(--space-sm);
  }

  .post-body :global(code) {
    background: rgba(200, 160, 175, 0.1);
    padding: 2px 6px;
    border-radius: 4px;
    font-size: 14px;
  }
</style>
```

- [ ] **Step 3: Verify blog pages**

```bash
npm run dev
```

Navigate to `http://localhost:4321/blog`. Expected: Grid with one card (Hello World). Click → full post. Back link works.

- [ ] **Step 4: Commit**

```bash
git add src/pages/blog/
git commit -m "feat: add blog grid and post pages with content collection"
```

---

### Task 10: Certifications & Awards Page

**Files:**
- Create: `src/pages/certifications.astro`

- [ ] **Step 1: Create certifications page**

```astro
---
// src/pages/certifications.astro
import BaseLayout from '../layouts/BaseLayout.astro';
import Card from '../components/Card.astro';
import { getCollection } from 'astro:content';

const certs = (await getCollection('certifications')).sort(
  (a, b) => b.data.date.getTime() - a.data.date.getTime()
);
---
<BaseLayout title="Certifications & Awards — Rizz">

  <section class="page-section">
    <div class="section-label">Certifications & Awards</div>

    <div class="card-grid">
      {certs.map((cert) => (
        <Card class="cert-card">
          <span class="cert-type">{cert.data.type}</span>
          <h3 class="cert-title">{cert.data.title}</h3>
          <p class="cert-issuer">{cert.data.issuer}</p>
          <p class="cert-date">{cert.data.date.toLocaleDateString('en-US', { year: 'numeric', month: 'short' })}</p>
          {cert.data.verifyUrl && (
            <a href={cert.data.verifyUrl} target="_blank" rel="noopener" class="cert-verify">
              Verify ↗
            </a>
          )}
        </Card>
      ))}
    </div>
  </section>

</BaseLayout>

<style>
  .page-section {
    padding: var(--space-2xl) var(--space-xl);
    max-width: 960px;
    margin: 0 auto;
  }

  .card-grid {
    display: grid;
    grid-template-columns: repeat(auto-fill, minmax(280px, 1fr));
    gap: var(--space-lg);
  }

  .cert-type {
    font-size: 10px;
    text-transform: uppercase;
    letter-spacing: 2px;
    font-weight: 500;
    color: var(--color-accent);
    background: rgba(168, 86, 110, 0.08);
    padding: 3px 10px;
    border-radius: 8px;
    display: inline-block;
    margin-bottom: var(--space-sm);
  }

  .cert-title {
    font-size: 18px;
    font-weight: 600;
    color: var(--color-text);
    margin-bottom: var(--space-xs);
  }

  .cert-issuer {
    font-size: 14px;
    font-weight: 400;
    color: var(--color-muted-text);
    margin-bottom: 2px;
  }

  .cert-date {
    font-size: 12px;
    color: var(--color-muted-label);
    margin-bottom: var(--space-sm);
  }

  .cert-verify {
    font-size: 12px;
    font-weight: 500;
    color: var(--color-accent);
    border: 1px solid var(--color-card-border);
    padding: 4px 12px;
    border-radius: 12px;
    display: inline-block;
    transition: background 0.2s ease;
  }

  .cert-verify:hover {
    background: rgba(168, 86, 110, 0.06);
    text-decoration: none;
  }
</style>
```

- [ ] **Step 2: Verify certifications page**

```bash
npm run dev
```

Navigate to `http://localhost:4321/certifications`. Expected: Grid with one sample card showing type badge, title, issuer, date, verify link.

- [ ] **Step 3: Commit**

```bash
git add src/pages/certifications.astro
git commit -m "feat: add certifications and awards grid page"
```

---

### Task 11: About Me Page

**Files:**
- Create: `src/pages/about.astro`

- [ ] **Step 1: Create About Me page**

```astro
---
// src/pages/about.astro
import BaseLayout from '../layouts/BaseLayout.astro';
import InfoChip from '../components/InfoChip.astro';
import Card from '../components/Card.astro';

const facts = [
  "Software Developer @ Gruve",
  "IIT BHU · ECE '21",
  "Backend · AI · Infra",
  "Tea Enthusiast",
];

const stats = [
  { label: "cups of tea", value: "∞" },
  { label: "companies", value: "4" },
  { label: "deployments survived", value: "100+" },
];

const qa = [
  { q: "What do you do?", a: "I build backend systems, AI agents, and infrastructure — the stuff that runs behind the scenes." },
  { q: "Why Rizz?", a: "Short for Ritika, stuck as a vibe. It's the energy I bring to everything I build." },
  { q: "Tea or coffee?", a: "Tea. Always tea. Don't even ask." },
  { q: "What's your superpower?", a: "Turning vague requirements into working systems before the deadline." },
];
---
<BaseLayout title="About Me — Rizz">

  <section class="page-section">
    <div class="section-label">About Me</div>

    <!-- Snapshot -->
    <div class="snapshot">
      <div class="snapshot-photo">R</div>
      <div class="snapshot-chips">
        {facts.map((fact) => <InfoChip>{fact}</InfoChip>)}
      </div>
      <div class="snapshot-stats">
        {stats.map((stat) => (
          <div class="stat">
            <span class="stat-value">{stat.value}</span>
            <span class="stat-label">{stat.label}</span>
          </div>
        ))}
      </div>
    </div>

    <!-- Q&A -->
    <div class="section-label qa-label">Q&A</div>
    <div class="qa-grid">
      {qa.map(({ q, a }) => (
        <Card>
          <p class="qa-question">{q}</p>
          <p class="qa-answer">{a}</p>
        </Card>
      ))}
    </div>
  </section>

</BaseLayout>

<style>
  .page-section {
    padding: var(--space-2xl) var(--space-xl);
    max-width: 800px;
    margin: 0 auto;
  }

  /* Snapshot */
  .snapshot {
    display: flex;
    flex-direction: column;
    align-items: center;
    gap: var(--space-lg);
    margin-bottom: var(--space-2xl);
  }

  .snapshot-photo {
    width: 180px;
    height: 180px;
    border-radius: 50%;
    background: linear-gradient(135deg, #e4bcc9, #cfa4b4);
    display: flex;
    align-items: center;
    justify-content: center;
    font-size: 56px;
    color: white;
    font-weight: 200;
    box-shadow: 0 14px 48px rgba(170, 110, 130, 0.2);
  }

  .snapshot-chips {
    display: flex;
    gap: 8px;
    flex-wrap: wrap;
    justify-content: center;
  }

  .snapshot-stats {
    display: flex;
    gap: var(--space-xl);
  }

  .stat {
    display: flex;
    flex-direction: column;
    align-items: center;
    gap: 2px;
  }

  .stat-value {
    font-size: 28px;
    font-weight: 600;
    color: var(--color-accent);
  }

  .stat-label {
    font-size: 12px;
    font-weight: 300;
    color: var(--color-muted-label);
    text-transform: lowercase;
  }

  /* Q&A */
  .qa-label {
    margin-top: var(--space-xl);
  }

  .qa-grid {
    display: grid;
    grid-template-columns: repeat(auto-fill, minmax(280px, 1fr));
    gap: var(--space-md);
  }

  .qa-question {
    font-size: 15px;
    font-weight: 600;
    color: var(--color-accent);
    margin-bottom: var(--space-sm);
  }

  .qa-answer {
    font-size: 14px;
    font-weight: 300;
    color: var(--color-muted-text);
    line-height: 1.6;
  }

  @media (max-width: 640px) {
    .snapshot-stats {
      gap: var(--space-lg);
    }
    .stat-value {
      font-size: 22px;
    }
  }
</style>
```

- [ ] **Step 2: Verify About page**

```bash
npm run dev
```

Navigate to `http://localhost:4321/about`. Expected: Photo circle, fact chips, stats row, then Q&A cards below.

- [ ] **Step 3: Commit**

```bash
git add src/pages/about.astro
git commit -m "feat: add about me page with snapshot and Q&A sections"
```

---

### Task 12: Scroll Fade Animations

**Files:**
- Create: `src/scripts/scroll-fade.ts`
- Modify: `src/layouts/BaseLayout.astro` (add script import)

- [ ] **Step 1: Create scroll-fade script**

```typescript
// src/scripts/scroll-fade.ts
function initScrollFade() {
  const observer = new IntersectionObserver(
    (entries) => {
      entries.forEach((entry) => {
        if (entry.isIntersecting) {
          entry.target.classList.add('visible');
          observer.unobserve(entry.target);
        }
      });
    },
    { threshold: 0.1 }
  );

  document.querySelectorAll('.scroll-fade').forEach((el) => {
    observer.observe(el);
  });
}

// Run on initial load
initScrollFade();

// Re-run after Astro page navigations (View Transitions)
document.addEventListener('astro:page-load', initScrollFade);
```

- [ ] **Step 2: Add script to BaseLayout**

In `src/layouts/BaseLayout.astro`, add this just before the closing `</body>` tag, after the existing sidebar `<script>`:

```astro
<script src="../scripts/scroll-fade.ts"></script>
```

- [ ] **Step 3: Add scroll-fade class to landing below-fold sections**

In `src/pages/index.astro`, add `class="scroll-fade"` to each section below the fold. Wrap the info-strip, companies, and skills divs:

Replace in `index.astro`:
```html
<div class="section-label">At a glance</div>
```
with:
```html
<div class="section-label scroll-fade">At a glance</div>
```

Do the same for the other two section labels and their content containers. Wrap each group in a `<div class="scroll-fade">`:

```html
<div class="scroll-fade">
  <div class="section-label">At a glance</div>
  <div class="info-strip">...</div>
</div>

<div class="scroll-fade">
  <div class="section-label">Worked with</div>
  <div class="companies">...</div>
</div>

<div class="scroll-fade">
  <div class="section-label">Skills</div>
  <div class="skills">...</div>
</div>
```

- [ ] **Step 4: Verify scroll animations**

```bash
npm run dev
```

Scroll down on the landing page. Expected: below-fold sections fade in as they enter the viewport. Experience page timeline entries also fade in (they already have `scroll-fade` class from TimelineEntry).

- [ ] **Step 5: Commit**

```bash
git add src/scripts/scroll-fade.ts src/layouts/BaseLayout.astro src/pages/index.astro
git commit -m "feat: add scroll-triggered fade-in animations via Intersection Observer"
```

---

### Task 13: Build Verification & Production Test

**Files:**
- No new files

- [ ] **Step 1: Run production build**

```bash
npm run build
```

Expected: Build completes successfully. Static files output to `dist/`.

- [ ] **Step 2: Preview production build**

```bash
npm run preview
```

Expected: Site runs at `http://localhost:4321` from the `dist/` folder. All pages work: landing, experience, projects (grid + detail), blog (grid + post), certifications, about. Sidebar navigation works on every page. Footer appears on every page.

- [ ] **Step 3: Verify all nav links work**

Click through every sidebar link:
- Home → `/`
- Experience → `/experience`
- Projects → `/projects` → click card → `/projects/ashta-changa` → back link
- Blog → `/blog` → click card → `/blog/hello-world` → back link
- Certs & Awards → `/certifications`
- About Me → `/about`

Expected: All pages render, active sidebar link highlights correctly, no 404s.

- [ ] **Step 4: Commit any fixes if needed, then final commit**

```bash
git add -A
git commit -m "chore: verify production build, all pages functional"
```

---

## Summary

| Task | What it builds |
|------|----------------|
| 1 | Astro project scaffold |
| 2 | Global CSS design system (tokens, reset, fonts) |
| 3 | Base layout (hamburger, sidebar, footer) |
| 4 | Reusable Card & InfoChip components |
| 5 | Landing page (hero + below fold) |
| 6 | Experience page (timeline + career data) |
| 7 | Content collections (projects, blog, certs schemas + samples) |
| 8 | Projects pages (grid + detail) |
| 9 | Blog pages (grid + post) |
| 10 | Certifications & Awards page |
| 11 | About Me page (snapshot + Q&A) |
| 12 | Scroll fade animations |
| 13 | Production build verification |
