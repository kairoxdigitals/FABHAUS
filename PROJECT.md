# FAB HAUS — Homepage

## Overview
Single-page, full-screen hero homepage for **FAB HAUS**, an architecture and prefabrication firm (Architecture, Interiors, Pre-Fab / LGSF, Modular).
The page is a full-bleed auto-rotating project slider with a fixed top bar (navigation plus a looped text band).

- **Status:** Homepage hero complete. Inner pages not built yet.
- **Entry file:** `index.html` (single file, no build step)

## Tech Stack
- Plain HTML + vanilla JavaScript (no framework)
- Tailwind CSS via CDN: `https://cdn.tailwindcss.com/3.4.17` (pinned version)
- Google Fonts: Cormorant Garamond (serif), Inter (sans)
- Custom CSS inside `<style>` for slider transitions, hover underline and marquee

## File Structure
```
/
├── index.html     # entire page: markup, styles, scripts
└── PROJECT.md     # this file
```

## Design System

**Colors** (defined in `tailwind.config`):
| Token      | Hex       | Use                          |
|------------|-----------|------------------------------|
| `ivory`    | `#F5F3EE` | Primary text                 |
| `charcoal` | `#252724` | Dark text / hover fills      |
| `muted`    | `#85877C` | Secondary text               |
| `bronze`   | `#A68B69` | Accent (labels, diamonds, progress) |
| `concrete` | `#DAD8D0` | Light neutral                |
| Page bg    | `#141513` | Body background              |

**Typography**
- Headings / wordmark: `font-serif` (Cormorant Garamond), light weight, wide tracking
- Nav, labels, body: `font-sans` (Inter), uppercase, wide letter-spacing (`tracking-[0.18em]`)

**Mood:** minimal, architectural, editorial. Dark photography-led, generous spacing, thin lines, no heavy boxes or buttons.

## Page Structure (top to bottom)

### 1. Fixed top bar — `#top-bar`
A fixed wrapper (`z-50`) holding two stacked parts:
1. **Header / nav** (`<header>`): wordmark "FAB HAUS" on the left, nav on the right.
2. **Looped text band** directly under the nav.

**Desktop nav order** (all links share one style and the `hover-line` underline interaction):
Home · Projects (`#projects`) · Architecture (`#architecture`) · Interior (`#interior`) · Fab Haus Modular · Pre-Fab · About Us · Contact Us (bronze accent)

- Projects is a plain link (no dropdown).
- **Mobile:** burger button `#mobile-toggle` opens full-screen overlay `#mobile-menu` with the same links.

**Hover interaction (`.hover-line`):** text turns white and a 1px ivory underline grows left to right in 0.35s.

### 2. Looped text band (marquee)
- Thin strip (`h-8`) under the nav, dark translucent background, 10px uppercase tracked text, bronze diamond separators.
- CSS-only infinite loop: `.marquee-track` holds two identical `.marquee-group` blocks and animates `translateX(0 → -50%)` over **40s**, linear, infinite.
- Both groups must stay identical for a seamless loop. The second group is `aria-hidden`.
- Current text is mock content about the website (welcome, services, enquiries).

### 3. Hero slider — `<main>`
Four full-bleed slides (`.slide`), one `.active` at a time, the rest `.inactive` (crossfade plus slight zoom).

| # | Title                     | Category                | Location                     | Year |
|---|---------------------------|-------------------------|------------------------------|------|
| 1 | The Monolith Pavilion     | Architecture & Interiors| Engadin Valley, Switzerland  | 2024 |
| 2 | Atelier Noire Residence   | Architecture & Interiors| Milan, Italy                 | 2023 |
| 3 | Komorebi Forest Sanctuary | Modular Solutions       | Kyoto Prefecture, Japan      | 2024 |
| 4 | Aeroform Kinetic Pavilion | Pre-Fab & LGSF          | Basel, Switzerland           | 2023 |

Slide metadata lives in `data-*` attributes (`data-title`, `data-category`, `data-location`, `data-year`) and is read by JS to update the overlay text.

**Overlays on the hero:**
- Bottom-left: category, year, project title (`#project-title`), location (`#location-text`)
- Bottom-center: "VIEW PROJECTS →" link (`#projects`)
- Bottom-center (below): clickable progress dots (`#loading-dots`, built in JS) with a circular timer ring on the active dot
- Bottom-right: counter (`#current-slide` / 04) and prev/next buttons (`#prev-btn`, `#next-btn`)
- Bottom edge: thin progress line (`#progress-indicator`)

## JavaScript Behavior
All logic is in one IIFE at the bottom of `index.html`.

- **Autoplay:** `const SLIDE_DURATION = 4000;` (4 seconds per slide, applies to all slides). A `requestAnimationFrame` loop tracks elapsed time and calls `nextSlide()` at the limit.
- **Looping:** `nextIndex = (currentIndex + 1) % totalSlides`, so slide 4 returns to slide 1.
- **`updateSlide(index)`:** swaps `active`/`inactive` classes, fades and updates overlay text, updates counter, progress line and dots, resets the timer. Locked during the ~1.2s transition (`isTransitioning`).
- **Manual controls:** prev/next buttons, dots, keyboard arrows (left/right), touch swipe (50px threshold).
- **Pause:** while hovering `<main>`, and while the browser tab is hidden.
- **Mobile menu:** toggles `#mobile-menu` and swaps burger/close icons.

## Conventions for AI Agents
- Keep it a single-file static page unless asked otherwise. No frameworks or build tools.
- Use Tailwind utility classes and the color tokens above. Do not add new colors without asking.
- New nav links must reuse: `relative hover-line hover:text-white transition-colors py-1`. No borders, boxes or filled buttons in the nav.
- Keep the top bar structure: `#top-bar` (fixed) contains header, then the marquee band.
- Keep both marquee groups identical when editing band text.
- To change slide speed, edit only `SLIDE_DURATION`.
- When adding or removing slides, also update: the hardcoded `/ 04` in the counter, and the initial `w-1/4` on `#progress-indicator`.
- Preserve the minimal, editorial feel: thin lines, wide tracking, lots of space.

## Known Placeholders / To Do
- [ ] Most nav links point to `#` (Home, Fab Haus Modular, Pre-Fab, About Us, Contact Us). Wire to real pages or sections.
- [ ] `#projects`, `#architecture`, `#interior` anchors do not exist yet. Build those sections or pages.
- [ ] Marquee band text is mock content. Replace with real announcements or messaging.
- [ ] Hero images are remote placeholder URLs (`lh3.googleusercontent.com`). Replace with real project photos, hosted locally or on the final host.
- [ ] Project titles, locations and years are sample data.
- [ ] Add a page `<meta name="description">`, favicon and Open Graph tags.
- [ ] For production, replace the Tailwind CDN with a compiled build.
- [ ] Add accessibility pass: marquee pause option and reduced-motion support.

## Suggested Next Steps
1. Build the Architecture and Interior pages or sections the new nav links point to.
2. Build the Projects gallery (`#projects`).
3. Add About Us and Contact Us pages (enquiry form).
4. Swap placeholder images and copy for real content.
