# missrizz.info

Personal portfolio for Ritika Agrawal — live at [missrizz.info](https://missrizz.info)

---

## Stack

- Plain HTML / CSS / JavaScript — no build step
- [Tailwind CSS](https://tailwindcss.com/) via CDN
- [AOS](https://michalsnik.github.io/aos/) (Animate On Scroll) via CDN
- [Inter](https://fonts.google.com/specimen/Inter) via Google Fonts
- Hosted on [Vercel](https://vercel.com)

---

## Structure

```
rizz_info/
├── index.html          # Home / Hero (photo slideshow + name + tagline)
├── experience.html     # Career timeline (AtomicAds, High Insight, Mastercard)
├── projects.html       # Projects (coming soon)
└── assets/
    ├── css/
    │   └── style.css   # All shared styles + CSS variables
    └── img/
        ├── photo-1.jpeg
        ├── photo-2.jpeg
        ├── photo-3.jpeg
        ├── photo-4.jpeg
        ├── photo-5.jpeg
        └── photo-6.jpeg
```

---

## Running Locally

No install needed. Open any HTML file directly in your browser:

```bash
open index.html
```

Or use a local server for accurate relative path resolution:

```bash
npx serve .
```

---

## Deploying to Vercel

1. Push this repo to your GitHub account
2. Go to [vercel.com](https://vercel.com) → **Add New Project** → import the repo
3. No build command needed — set **Output Directory** to `/` (root)
4. Click **Deploy**

### Connecting missrizz.info

1. In Vercel dashboard → **Settings** → **Domains** → add `missrizz.info`
2. At your domain registrar, update DNS:
   - `A` record: `@` → `76.76.21.21`
   - `CNAME` record: `www` → `cname.vercel-dns.com`
3. SSL is handled automatically by Vercel

---

## Updating Content

| What to change | File | What to edit |
|---|---|---|
| Photo slideshow | `index.html` | Swap `assets/img/photo-*.jpeg` files |
| Tagline | `index.html` | `.tagline-bar p` text |
| Experience cards | `experience.html` | `.card-title`, `.card-desc`, `.tags` per entry |
| Projects | `projects.html` | Replace coming-soon block with timeline entries |
| Colors / fonts | `assets/css/style.css` | CSS variables in `:root` |
| Email / social links | All 3 HTML files | `mailto:` and `href` in nav + footer |

---

## Phase 2 (Planned)

- [ ] Gaming project on Projects page
- [ ] AI chatbot that answers questions about Ritika
- [ ] Contact form
