# Claude Code Brief: Convert Scott Beaulier HTML Site to WordPress Theme

## Project Overview

Convert a 5-page static HTML/CSS website into a custom WordPress theme. The site is a personal/professional website for Scott Beaulier, Dean of Business at the University of Wyoming, optimized for media pickup and journalist usability.

The design has an **editorial/newspaper aesthetic** with a **navy and gold color palette** reflecting Scott's existing brand colors.

---

## Color Palette (CRITICAL — use these exact values)

```css
:root {
  --ink: #1a1a1a;          /* Primary text */
  --cream: #f0f0ea;        /* Card/highlight backgrounds */
  --navy: #354F64;         /* Primary accent — borders, headings, labels, CTA backgrounds */
  --navy-dark: #2a3f51;    /* Hover states on navy elements */
  --gold: #b08a3a;         /* Secondary accent — card borders, topic numbers, primary buttons, kicker lines */
  --gold-light: #c9a24e;   /* Button hover states */
  --warm-white: #f8f8f5;   /* Page background */
  --mid-gray: #6b6b6b;     /* Secondary text */
  --light-rule: rgba(26, 26, 26, 0.12); /* Dividers */
}
```

**How colors are applied:**
- **Navy** = nav double border, site name, hero kicker text, section labels, expertise grid outer border, commentary source links, CTA section backgrounds, form submit buttons, coverage outlet labels, affiliations grid border
- **Gold** = hero kicker line (::before), media card left borders, topic numbers, Quick Facts/education card top borders, hero image bottom border, speaking card left borders, primary CTA buttons, nav hover underlines, copy button hover, download buttons
- **Gold-light** = pullquote banner bold text, primary button hover state, quick-grab label text

---

## Source Files

The following HTML files make up the complete site. Each page shares a common `styles.css` and the same nav/footer structure.

**Typography stack (Google Fonts):**
- Playfair Display (400, 700, 900, italic) — headlines, display
- Source Sans 3 (300, 400, 500, 600, 700) — body text
- JetBrains Mono (400, 500) — labels, kickers, metadata

### Pages:
1. **index.html** — Home (hero with "cowboys in business" headline + portrait photo, For Media cards, Ask Me About expertise grid, Commentary list, Bio preview with Quick Facts sidebar)
2. **philosophy.html** — Philosophy (Four Pillars grid, long-form editorial prose with drop cap, sidebar with Core Beliefs list, photo break placeholders)
3. **cv.html** — CV (photo + contact header, timeline-based career history, education cards, affiliations grid, accomplishments block)
4. **press.html** — Press & Media (quick-grab download bar, short + long copy-paste bios with clipboard buttons, headshot download cards, story angle cards with sample reporter questions, recent coverage grid, speaking topics)
5. **contact.html** — Contact (response time strip, direct contact channels, inquiry form with dropdown, social links, map placeholder)

---

## WordPress Theme Requirements

### Theme Structure

Create a theme called `beaulier` with this file structure:

```
beaulier/
├── style.css              (theme metadata + all custom CSS)
├── functions.php           (enqueues, menus, CPTs, theme support)
├── header.php              (shared top-bar nav)
├── footer.php              (shared footer + media CTA)
├── index.php               (fallback)
├── front-page.php          (Home page template)
├── page-philosophy.php     (Philosophy page template)
├── page-cv.php             (CV page template)
├── page-press.php          (Press & Media page template)
├── page-contact.php        (Contact page template)
├── template-parts/
│   ├── media-cta.php       (reusable dark CTA block at bottom of pages)
│   ├── expertise-grid.php  (Ask Me About topic grid)
│   └── pullquote-banner.php
├── inc/
│   ├── cpt-commentary.php  (Custom Post Type: Commentary/Op-Eds)
│   ├── cpt-media-hits.php  (Custom Post Type: Media Coverage)
│   ├── cpt-speaking.php    (Custom Post Type: Speaking Topics)
│   └── customizer.php      (Theme Customizer settings)
├── js/
│   └── clipboard.js        (copy-to-clipboard for press page bios)
├── screenshot.png          (theme screenshot, can be placeholder)
└── assets/
    └── images/             (placeholder images if needed)
```

### functions.php Requirements

```php
add_theme_support('title-tag');
add_theme_support('post-thumbnails');
add_theme_support('custom-logo');
add_theme_support('html5', array('search-form', 'comment-form', 'comment-list', 'gallery', 'caption'));

// Enqueue Google Fonts
// Playfair Display (400,700,900,italic) + Source Sans 3 (300-700) + JetBrains Mono (400,500)

register_nav_menus(array('primary' => 'Primary Navigation'));
```

### Navigation

Use `wp_nav_menu()` in header.php. The nav should output clean links styled with uppercase, letter-spacing, gold hover underline via ::after pseudo-element. Support an `active` class on the current page. The site name in the nav should be colored navy (`--navy: #354F64`).

### Custom Post Types

**1. Commentary / Op-Eds (`commentary`)** — Title, Publication/Source, External URL, Year/Date, Type (Op-Ed, Feature, Interview)

**2. Media Coverage (`media_hit`)** — Title/Headline, Outlet Name, External URL, Date

**3. Speaking Topics (`speaking_topic`)** — Title, Description, Format (Keynote, Panel, Workshop, Fireside Chat), Duration

Use simple meta boxes (no ACF dependency).

### Theme Customizer Settings

Section: "Site Identity & Contact" with fields for Phone (Cell), Phone (Office), Email, Office address, Twitter/X URL, Facebook URL, LinkedIn URL, CV PDF upload. These populate throughout the site.

Add bios section: Short Bio (textarea) and Full Bio (textarea) for the press page.

### Key Design Details to Preserve

1. **3px double border in navy** on the sticky nav bar
2. **Playfair Display at 4.5rem/900 weight** for the home hero headline
3. **Navy (#354F64) accents** — section labels, kicker text, grid borders, CTA backgrounds
4. **Gold (#b08a3a) accents** — card border-left, topic numbers, hero kicker line, primary buttons, image bottom borders, top borders on highlight cards
5. **Gold (#c9a24e) light** — pullquote banner strong text, button hovers
6. **JetBrains Mono for all metadata/labels**
7. **Editorial grid on expertise section** — 3-col grid with 1.5px solid navy border
8. **Commentary list hover** — slides right with padding-left transition, border turns navy
9. **Drop cap on Philosophy page** — first letter in navy via ::first-letter
10. **Pullquote banner** — full-width navy background with Playfair italic and gold-light on strong tags
11. **Cream (#f0f0ea) backgrounds on cards** vs warm-white (#f8f8f5) page background
12. **Gold bottom border (4px)** on hero portrait frame and gold top border (4px) on Quick Facts, education cards, contact form area, and accomplishments block
13. **Fade-in and slide-in CSS keyframe animations**

### What NOT to Include

- No page builder dependency
- No ACF dependency
- No custom Gutenberg blocks
- No WooCommerce
- No jQuery dependency if avoidable

---

## Suggested Build Order

1. Theme scaffolding (style.css, functions.php, header.php, footer.php)
2. Migrate all CSS into style.css — use the navy/gold variables exactly
3. Build front-page.php with static content
4. Register CPTs and meta boxes
5. Convert commentary section to WP_Query loop
6. Build remaining page templates
7. Add Customizer settings
8. Wire up dynamic content
9. Add clipboard.js for press page
10. Test responsive breakpoints (900px and 600px)
11. Create screenshot.png

---

## Testing Checklist

- [ ] Theme passes `Theme Check` plugin validation
- [ ] All 5 pages render identically to the HTML source files
- [ ] Navy/gold color palette is consistent across all pages
- [ ] Navigation highlights current page with gold underline
- [ ] Commentary CPT entries appear on home page
- [ ] Media Coverage and Speaking CPT entries appear on press page
- [ ] Contact info from Customizer appears in all locations
- [ ] Copy-to-clipboard works on press page bios
- [ ] Responsive design works at 900px and 600px breakpoints
- [ ] Google Fonts load correctly
- [ ] CSS animations fire on page load
- [ ] No console errors
