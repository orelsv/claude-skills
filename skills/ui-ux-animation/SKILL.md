---
name: ui-ux-animation
description: Design polished UI/UX with animations, scroll effects, and micro-interactions to make interfaces feel alive. Use when the user asks to improve design, add scroll animations, parallax, hover effects, transitions, make a site/app more lively, or build a landing page with modern visual polish.
---

# UI/UX Design & Animation

When the user asks for UI/UX work or wants an interface to feel "more alive", follow these rules:

## 1. Design foundation first
Before adding animation, verify the basics:
- **Typography**: max 2 font families; clear size scale (e.g., 16/20/28/40px); line-height 1.5–1.7 for body text
- **Spacing**: consistent scale (4/8/16/24/32/64px); generous whitespace
- **Color**: one accent color + neutrals; check WCAG AA contrast (4.5:1 for text)
- **Hierarchy**: the eye must know where to look first
Avoid generic "AI look": no default purple gradients on white cards everywhere; pick a distinctive direction (editorial, brutalist, soft-depth, dark-technical, etc.) and commit.

## 2. Animation principles
- Animations must have PURPOSE: guide attention, show state change, create spatial continuity.
- Durations: micro-interactions 150–250ms, transitions 300–500ms, scroll reveals 400–700ms.
- Easing: use `cubic-bezier(0.22, 1, 0.36, 1)` (ease-out-quint feel) for entrances; never linear.
- Animate only `transform` and `opacity` (GPU-friendly); never animate width/height/top/left.
- ALWAYS respect `prefers-reduced-motion` — wrap animations in the media query.

## 3. Scroll animations toolkit (vanilla first, library if needed)
**Simple (no library)** — IntersectionObserver reveal pattern:
```js
const io = new IntersectionObserver((entries) => {
  entries.forEach(e => e.target.classList.toggle('in-view', e.isIntersecting));
}, { threshold: 0.15 });
document.querySelectorAll('[data-reveal]').forEach(el => io.observe(el));
```
```css
[data-reveal] { opacity: 0; transform: translateY(24px); transition: opacity .6s, transform .6s cubic-bezier(.22,1,.36,1); }
[data-reveal].in-view { opacity: 1; transform: none; }
@media (prefers-reduced-motion: reduce) { [data-reveal] { transition: none; opacity: 1; transform: none; } }
```
**Advanced**: CSS scroll-driven animations (`animation-timeline: view()`), GSAP + ScrollTrigger for pinning/scrubbing, Framer Motion for React, Lenis for smooth scrolling. Recommend a library only when vanilla becomes painful.

## 4. Micro-interactions catalog
Buttons (scale 0.97 on press + subtle shadow lift on hover), inputs (label float, focus ring), cards (translateY(-4px) + shadow on hover), page transitions (fade+slide), skeleton loaders instead of spinners, staggered list entrances (60–90ms delay between items).

## 5. Output rules
- Deliver working code, not just advice: HTML+CSS+JS or React component, commented.
- Show the simple version first, then offer the advanced/library version.
- Mention performance: test on mobile, avoid layout thrash, lazy-load heavy assets.
