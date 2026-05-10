# SuperVolt EV Service Center — Complete Project Documentation

## Project Overview

**SuperVolt EV Service Center** is a static HTML/CSS/JS website for a premium EV service center in Pune, India. The site is fully bilingual (English/Marathi), mobile responsive, and connected to a Strapi CMS backend for managing popup ad campaigns remotely.

**Stack:**
- Frontend: Pure HTML + CSS + Vanilla JS (no build process, no framework)
- Backend: Strapi v5 (Node.js headless CMS)
- Fonts: Bebas Neue (headings) + Inter (body) via Google Fonts CDN
- Contact form: FormSubmit.co (no backend needed)
- Popup ads: Strapi REST API

---

## File Structure

```
supervolt-deploy/
├── index.html                  — Main public website
├── admin.html                  — Popup ad management dashboard
├── strapi-config.js            — Strapi URL config (edit this before deploy)
├── firebase-config.js          — Deprecated, ignore
├── .gitignore                  — Git exclusions
├── package.json                — Minimal, only used for local serve
├── images/
│   ├── IMG_8673.PNG            — Gallery photo 1
│   ├── IMG_8672.PNG            — Gallery photo 2
│   ├── IMG_8400.JPG.jpeg       — Gallery photo 3
│   └── IMG_8552.JPG.jpeg       — Gallery photo 4
├── ads/
│   └── WhatsApp Image *.jpeg   — Fallback ad image (local only)
└── *.mp4 (5 files)             — Hero background videos
    ├── Video_Project_Headlight_compressed.mp4
    ├── Video_Project_Light_bulb_compressed.mp4
    ├── Video_Project_plugs_compressed.mp4
    ├── Video_Project_wheel_compressed.mp4
    └── Wire_works_compressed.mp4
```

---

## index.html — Section by Section

### CSS Design Tokens (`:root`)
All colors and sizes are defined as CSS variables at the top:
- `--bg: #080808` — main background (near black)
- `--accent: #00b848` — green brand color used everywhere
- `--header-h: 78px` — header height, used by hero padding and mobile nav positioning
- `--radius: 14px` — card border radius

### 1. Header (`<header id="site-header">`)
**What it does:** Fixed top navigation bar that stays visible while scrolling.

- Layout: CSS Grid `1fr auto 1fr` — Logo (left), Nav (center, truly centered), Actions (right)
- Becomes opaque (`class="scrolled"`) when user scrolls past 20px — triggered by the scroll event listener in JS
- Contains: Logo + tagline, desktop nav links, language toggle (EN/Marathi), "Get Service" CTA button
- On mobile (≤768px): nav and actions collapse, hamburger button appears

**JS functions:**
- `scrollToContact()` — smoothly scrolls to the contact section
- `scrollToHome(e)` — scrolls to top of page
- Scroll listener adds `.scrolled` class and updates `.active` class on nav links based on which section is in viewport

### 2. Mobile Navigation (`<div id="mobile-nav">`)
**What it does:** Slide-down drawer that appears when hamburger is tapped on mobile.

- Same links as desktop nav but in vertical list format
- Contains language toggle and "Get Service" CTA button (duplicated from header so they don't disappear on mobile)
- `syncMobileLang(lang)` keeps mobile and desktop language toggles in sync
- Closes when: link is clicked (`closeMenu()`), outside area is tapped (document click listener), or hamburger is tapped again
- Hamburger animates to X via `.open` class CSS transforms

### 3. Hero Section (`<section class="hero" id="home">`)
**What it does:** Full-viewport landing section with video background and main headline.

**Video background:**
- 3 video panels side by side (`grid-template-columns: 1fr 1fr 1fr`)
- 5 MP4 files cycle through each panel independently via `idx[]` array
- On video end or error → loads next video in sequence
- Dark overlay (`rgba(8,8,8,0.55)`) over each panel + directional gradient for text readability
- Videos are muted, autoplay, loop=false (cycler handles looping)

**Content:**
- Animated green dot badge: "Pune's #1 EV Service Center"
- H1 headline with explicit `<br>` to prevent word-wrap issues at all screen sizes
- Tagline with left accent border
- Description paragraph
- Two CTA buttons: "Book a Service" (scrolls to contact) + "Explore Services" (scrolls to services)
- 4 stat counters: EVs Serviced, Avg Rating, EV Brands, Turnaround time
- Scroll indicator with bounce animation at bottom

### 4. Services Section (`<section id="services">`)
**What it does:** 6-card grid showing all service offerings.

Services offered:
1. General Service — full vehicle inspection
2. Motor Service — motor diagnosis and repair
3. Battery Service — battery maintenance and replacement
4. Controller Diagnosis — advanced controller repair
5. Brake Service — safety-first brake work
6. Performance Upgrade — speed and efficiency enhancement

Each card has an icon, title, description, and "Learn More" arrow. Cards have hover lift effect.

### 5. Gallery Section (`<section id="gallery">`)
**What it does:** 4-photo grid of actual workshop images.

- Uses 4 images from the `images/` folder
- Hover reveals overlay label (e.g. "EV Workshop Bay", "Expert Technicians")
- Images: IMG_8673.PNG, IMG_8672.PNG, IMG_8400.JPG.jpeg, IMG_8552.JPG.jpeg

### 6. Why Us / About Section (`<section id="about">`)
**What it does:** Trust-building section with differentiators and a testimonials carousel.

**Feature cards** (4 cards):
- Certified Technicians
- Genuine Parts Only
- Fast Turnaround
- Transparent Pricing

**Testimonials:** Customer reviews displayed as cards with star ratings, customer name, and vehicle model.

### 7. Brands Section (`<section id="brands">`)
**What it does:** Infinite horizontal marquee of EV brand logos.

- CSS `@keyframes marquee` animation — elements scroll left infinitely
- Elements are duplicated in HTML to create seamless loop effect
- Brands shown: OLA, Ather, TVS, Bajaj, Hero, Revolt, Tork, Ampere, Pure EV, Okinawa

### 8. Contact Section (`<section id="contact">`)
**What it does:** Two-column layout with contact info + contact form.

**Left column — Contact info:**
- Phone: `+91 7972792916` (clickable tel: link)
- Email: `info@supervoltemotors.com` (clickable mailto: link)
- Hours: 10 AM – 8 PM Daily
- Instagram: `@supe_rvolt` (clickable link)
- Google Maps embed: coordinates `18.4580993, 73.8486261`

**Right column — Contact form:**
- Powered by **FormSubmit.co** — sends emails to `info@supervoltemotors.com` with zero backend
- Hidden fields: `_subject` (email subject line), `_captcha: false` (disables captcha), `_honey` (spam trap)
- Fields: Name, Email, Phone, Vehicle Model, Service Required (dropdown), Message
- Async `handleSubmit(e)` — prevents page reload, shows success/error message in-page, resets form on success
- On failure: shows error message with direct email address

### 9. Footer (`<footer>`)
**What it does:** 4-column footer with links and contact info.

Columns:
1. Logo + description + social icons (Instagram, WhatsApp, Phone)
2. Services list (links to `#services`)
3. Quick links (all section anchors)
4. Contact info (phone, email, hours)

**Hidden admin link:** Footer contains `<a href="admin.html">Admin</a>` at 30% opacity — visible but subtle, so the client can find it without it being obvious to visitors.

### 10. Popup Ad System
**What it does:** Fetches ad configuration from Strapi and shows a modal popup on page load.

**Flow:**
1. Page loads → `strapi-config.js` sets `window.STRAPI_URL`
2. `loadPopupConfig()` calls `GET {STRAPI_URL}/api/popup-ad?populate=adImage`
3. Strapi returns: enabled, delay, frequency, adImage (with URL), title, subtitle, ctaText, ctaLink
4. `shouldShow(cfg)` checks frequency logic:
   - `every_visit` → always show
   - `once_per_session` → check/set `sessionStorage['sv_popup_shown']`
   - `once_per_day` → check/set `localStorage['sv_popup_ts']` with 24hr timestamp
5. If should show: `setTimeout(() => showPopup(cfg), delay * 1000)`
6. `showPopup(cfg)` populates the modal with image, title, subtitle, CTA button
7. If no `adImageUrl` → hides the image wrapper
8. If no title/subtitle/CTA → hides the body section entirely

**Closing:** Close button, outside click, Escape key all call `closePopup()`

**Default config (fallback if Strapi is unreachable):** enabled:true, delay:3, every_visit, no image — popup will show but be empty, so it auto-hides the body section.

### 11. Language System
**What it does:** Bilingual English/Marathi toggle that changes all visible text.

- Every translatable element has `data-en="..."` and `data-mr="..."` attributes
- `setLanguage(lang)` iterates all elements with `[data-en]` and updates their text/innerHTML
- Language preference saved to `localStorage['selectedLanguage']` — persists across page refreshes
- On `DOMContentLoaded`: restores last selected language
- Mobile and desktop language buttons kept in sync via `syncMobileLang()`

---

## admin.html — Component by Component

### Login Screen (`#login-screen`)
**What it does:** Firebase-style login UI backed by Strapi JWT authentication.

- Calls `POST {STRAPI_URL}/api/auth/local` with `{ identifier, password }`
- On success: saves JWT to `sessionStorage['strapi_jwt']`, shows dashboard
- On failure: shows friendly error message
- **Auto-login:** On page load, if JWT exists in session, calls `GET /api/users/me` to verify — if valid, skips login and goes straight to dashboard
- **Setup instructions** shown below login form explaining how to configure Strapi

### Dashboard Layout
- Left sidebar: brand logo, navigation (Popup Ads, View Live Site link), user email + avatar, Sign Out button
- Main content area: scrollable, max-width 900px

### Status Tiles (3 tiles)
Real-time read-only tiles showing current popup state:
- **Status** — ON / OFF
- **Delay** — seconds before popup shows
- **Frequency** — Every / Per Session / Daily

Updated whenever config is loaded or saved.

### Popup Status Card
Toggle switch to enable/disable the popup entirely. Changing the toggle updates the status badge (green "Active" / red "Disabled") in real time.

### Timing & Frequency Card
- **Delay field** (number input, 0–60): seconds before popup appears
- **Frequency dropdown**: Every Visit / Once Per Session / Once Per Day

### Ad Image Card
**What it does:** Upload an image to Strapi Media Library and use it as the popup ad.

**Upload flow:**
1. User picks a file (click or drag-and-drop)
2. Local preview shown immediately via `URL.createObjectURL(file)`
3. XHR `POST /api/upload` with `Authorization: Bearer {JWT}`
4. Progress bar updates via `xhr.upload.onprogress`
5. On success: Strapi returns file object with `id` and `url`
6. `pendingImageId` = file ID (used when saving)
7. `pendingImageUrl` = full URL (used for preview and `index.html` display)
8. Badge changes to "New Ad — click Save to publish"

### Optional Text & CTA Card
4 fields (all optional, leave blank to hide):
- Popup Title — large heading shown below ad image
- Popup Subtitle — smaller descriptive text
- Button Text — CTA button label
- Button Link — URL the CTA button opens

### Actions Card
Three buttons:
- **Save & Publish** — `PUT /api/popup-ad` with all form data + image ID → immediately live on website
- **Preview** — opens admin preview overlay showing exactly how the popup looks to visitors
- **Disable & Reset** — `PUT /api/popup-ad` with `enabled:false, adImage:null` + clears all fields

### Preview Overlay
Floating modal matching the exact popup design on `index.html`, with a yellow "Preview — not visible to visitors" banner. Closes on X click, outside click.

### Toast Notifications
Bottom-right toast messages for: save success, upload success, load failure, save failure, reset.

---

## strapi-config.js

```js
window.STRAPI_URL = 'https://your-strapi-url.com';
```

**This is the only file that changes between local development and production.**

- Local: `http://localhost:1337`
- Production: your Strapi Cloud URL (e.g. `https://supervolt-cms.strapiapp.com`)

Both `index.html` and `admin.html` load this file via `<script src="./strapi-config.js">` before their own scripts. All `fetch()` calls prefix with `window.STRAPI_URL`.

---

## Strapi Backend — Required Setup

### Content Type: Popup Ad (Single Type)
API ID: `popup-ad`

| Field | Type | Purpose |
|---|---|---|
| `enabled` | Boolean | Master on/off switch |
| `delay` | Integer | Seconds before popup appears |
| `frequency` | Enumeration | `every_visit` / `once_per_session` / `once_per_day` |
| `title` | Short text | Optional heading in popup |
| `subtitle` | Short text | Optional subheading |
| `ctaText` | Short text | CTA button label |
| `ctaLink` | Short text | CTA button URL |
| `adImage` | Media (single) | The ad image stored in Strapi Media Library |

### Required Permissions (Settings → Users & Permissions → Roles)

| Role | Resource | Permission |
|---|---|---|
| Public | popup-ad | `find` |
| Authenticated | popup-ad | `find`, `update` |
| Authenticated | upload | `upload` |

### Admin Login User
Create under **Settings → Users & Permissions Plugin → Users**:
- Set email + password
- **Confirmed: ON** (critical — login fails if this is OFF)
- **Blocked: OFF**
- **Role: Authenticated**

This user's credentials are used to log into `admin.html`.

---

## How Data Flows

```
Visitor opens index.html
    → strapi-config.js loads STRAPI_URL
    → fetch GET /api/popup-ad?populate=adImage
    → Strapi returns popup config (public read, no auth needed)
    → shouldShow() checks frequency rules
    → setTimeout → showPopup() renders the modal

Client opens admin.html
    → Login → POST /api/auth/local → JWT returned
    → JWT stored in sessionStorage
    → fetch GET /api/popup-ad?populate=adImage (with JWT)
    → Form fields populated with current config
    → Upload image → POST /api/upload (with JWT) → get file ID
    → Save → PUT /api/popup-ad (with JWT) → config saved to Strapi DB
    → Visitor refreshes index.html → new config fetched → updated popup shown
```

---

## Production Checklist

### Before going live:
- [ ] Deploy Strapi to Strapi Cloud (run `npm run strapi deploy` inside `my-project/`)
- [ ] Update `strapi-config.js` with the live Strapi Cloud URL
- [ ] Verify Strapi permissions are set (Public: find; Authenticated: find+update+upload)
- [ ] Confirm admin user has **Confirmed: ON** in Strapi
- [ ] Test login on `admin.html` from live URL
- [ ] Upload an ad image and Save & Publish
- [ ] Open `index.html` on a separate device — verify popup appears with correct image
- [ ] Test contact form sends email to `info@supervoltemotors.com`
- [ ] Test language toggle (EN ↔ Marathi) on both desktop and mobile
- [ ] Test on mobile (iPhone + Android) for layout issues

### CORS (if popup doesn't load on live site):
In Strapi project: `config/middlewares.js`, ensure your website's domain is in the allowed origins list:
```js
{
  name: 'strapi::cors',
  config: {
    origin: ['https://your-website-domain.com', 'http://localhost:3000'],
  }
}
```

### Hosting the static site:
The `index.html` + `admin.html` + assets can be hosted on:
- **Netlify** (drag-and-drop the folder) — recommended, free
- **Vercel** — free
- **GitHub Pages** — free
- Any static host or cPanel file manager

No build step needed — just upload the files.

---

## Business Info (embedded in site)

| Field | Value |
|---|---|
| Business name | SuperVolt EV Service Center |
| Phone | +91 7972792916 |
| Email | info@supervoltemotors.com |
| Instagram | @supe_rvolt |
| Location | Lat: 18.4580993, Lng: 73.8486261 (Pune) |
| Hours | 10 AM – 8 PM Daily |
| WhatsApp | wa.me/917972792916 |
