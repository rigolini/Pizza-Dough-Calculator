# Pizza Dough Calculator — Project Guide

## What This Is

A single self-contained HTML file (`index.html`) that calculates pizza dough recipes. Built by Jamele Rigolini. It includes all CSS, JS, translations, and logic in one file — no build tools, no dependencies (except Google Fonts). The hero image is embedded as a base64 data URI for full offline support. It is also a Progressive Web App (PWA) that can be installed on phones and desktops, and is deployed on GitHub Pages at `https://rigolini.github.io/Pizza-Dough-Calculator/`.

## How the Calculation Works

### Core Formula (By # of Pizzas mode)

Given user inputs: number of pizzas, bun weight, hydration %, salt %, semolina ratio, and protein targets:

1. **Total dough** = numPizzas × pizzaWeight
2. **Salt** = saltPercent/100 × totalDough (salt is included in bun weight)
3. **Total flour** = totalDough × (1 - saltFraction) / (1 + hydrationFraction)
4. **Total water** = totalFlour × hydrationFraction
5. **Semolina** = (totalFlour - gluten) × semolinaRatio
6. **Flour** = (totalFlour - gluten) × (1 - semolinaRatio)
7. **Gluten** is auto-calculated: `G = TF × (targetProtein - mixProtein) / (glutenProtein - mixProtein)`
8. **Hydration** = total water (incl. biga 40g) ÷ total flour (incl. biga 100g)
9. **Weighted protein** = (flour×pF + semolina×pS + gluten×pG) / totalFlour

### Biga (Pre-ferment)

Fixed at 100g flour + 40g water + 1–2g dry yeast (or 3–6g fresh yeast). Prepared 12–24 hours in advance at 15–20°C. Mix until just combined — do not knead. The biga should roughly double in size and develop a pungent, slightly alcoholic aroma when ready. The biga flour and water are **included** in the flour and water totals — the recipe card shows both net amounts (what you add to the main dough) and total amounts (including biga). The biga card appears **last** in the recipe ingredients list, highlighted in accent colour.

## Pizza Style Presets

| Style | Hydration | Bun Weight | Flour | Semolina | Salt | Oil | Protein |
|-------|-----------|------------|-------|----------|------|-----|---------|
| Jamele's Recipe | 60% | 300g | — | 15% | 1% | optional | 14% |
| Neapolitan (Classic) | 60% | 250g | Tipo 00 | 0% | 2.5% | none | 12.5% |
| Canotto (Modern Neapolitan) | 70% | 250g | Tipo 00 | 0% | 2.5% | none | 12.5% |
| Roman Tonda | 55% | 200g | Tipo 00 | 0% | 2.5% | required | 12.5% |
| Roman al Taglio | 75% | 300g | Tipo 0/1 | 0% | 2.5% | required | 13% |
| Sicilian | 70% | 300g | Bread | 10% | 2% | required | 13% |
| New York | 62% | 300g | Bread | 0% | 2% | required | 13% |

Key facts:
- **Neapolitan (both classic and canotto) uses NO oil** in the dough
- **Classic Neapolitan**: AVPN-regulated, 58–62% hydration, direct dough
- **Canotto**: modern style, 65–75% hydration, often uses poolish/biga pre-ferment, taller airy crust
- Oil display is smart per preset: hidden if `none`, solid card if `required`, dashed border if `optional`
- Flour type tag shown per preset (Tipo 00, Tipo 0/1, Bread flour)
- Semolina and gluten cards hidden when quantity = 0

## UI / Design

- **Theme**: warm cream/parchment (`--bg:#faf6f1`), terracotta accent (`--accent:#c0582a`)
- **Fonts**: Playfair Display (headings), Source Sans 3 (body), Source Serif 4 (italic notes) — loaded from Google Fonts
- **Hero image**: single Italian coastal landscape, embedded as base64 data URI (no external request, works offline)
- **Title sits below the hero image** — no overlap
- **Cards**: white surface, soft shadow, rounded 14px
- **Ingredient cards**: emoji icon + large Playfair number + smaller total/biga info
- **Biga card**: highlighted in accent-pale colour, shown last in ingredient list
- **Protein content card**: has a note explaining what protein does (too little / too much)
- **Recipe summary**: shows total dough, pizzas, and hydration (no protein)

## Responsive Design (3 breakpoints)

### Phone (up to 480px)
- Single column layout
- Larger touch targets (min 44px height for inputs)
- `font-size: 16px` on inputs to prevent iOS auto-zoom
- Protein grid stacks to single column
- Recipe summary becomes horizontal row
- Recipe columns stack vertically (`flex-direction: column`) to prevent horizontal overflow
- `overflow-x: hidden` on body AND `width:100%;overflow-x:hidden` on container prevents cards exceeding screen width
- Hero image uses negative margins (`margin-left:-14px; width:calc(100%+28px)`) to be truly full-screen width
- Tooltips slide up from bottom as a sheet
- Smaller hero image (120px), tighter spacing
- Safe area padding for notched iPhones (Dynamic Island)
- **Custom +/− spinner buttons** on all number inputs (iOS hides native spinners); shown on touch/mobile, hidden on desktop

### Tablet (481–900px)
- Single column cards, wider layout
- Protein grid stays 3-column
- Recipe summary horizontal row
- Comfortable touch targets
- Guide modal with 2-column style grid

### Desktop (901px+)
- Two-column grid layout
- Hover effects on buttons
- Standard tooltip positioning near the ? button

### Touch devices (any size)
- `pointer:coarse` media query enlarges all interactive elements
- `-webkit-tap-highlight-color: transparent` removes grey flash on tap

## Progressive Web App (PWA)

The file includes everything needed to work as an installable app:
- **Web App Manifest** (inline via data URI) — app name, icon, theme colour, standalone display
- **Service Worker** (inline blob) — caches the page for offline use after first visit
- **Install prompt** — floating "📲 Install App" button appears on Android/Chrome for 15 seconds
- **Apple meta tags** — `apple-mobile-web-app-capable`, status bar style, touch icon (`icon.png`)
- **Open Graph tags** — `og:title`, `og:description`, `og:image` for WhatsApp/iMessage link previews (uses `icon.png`)
- **Requirements**: must be served over HTTPS for PWA features to work (deployed on GitHub Pages)

## Internationalization (i18n)

Full translation in 5 languages: 🇬🇧 English, 🇫🇷 French, 🇪🇸 Spanish, 🇩🇪 German, 🇮🇹 Italian

Everything is translated: UI labels, tooltips, preset names, preset descriptions, guide content, protein notes, biga recipe notes. All translations live in the `T` object (T.en, T.fr, T.es, T.de, T.it).

**Important**: when adding or changing any text, always update ALL 5 languages.

## Guide Modal

Opened via a prominent button. Contains:
- **Style cards grid** (8 cards): Neapolitan Classic, Canotto, Roman Tonda, Roman al Taglio, Sicilian/Detroit, New York, Home Oven Tips
- **Ingredient sections**: Flour & Protein (with too-little/too-much explanation), Semolina, Vital Wheat Gluten, Hydration, Salt, Biga (with yeast amounts and timing), Olive Oil
- The Flour section explains what happens with too little protein (<10%), right range (11–13%), and too much (>14%)
- The Olive Oil section notes that both classic and canotto Neapolitan use no oil
- All content is in all 5 languages

## Tooltips

`?` buttons next to: Hydration, Flour protein, Semolina protein, Gluten protein, Biga — each shows a positioned tooltip with detailed explanation. On phones, tooltips appear as bottom sheets. Tooltip content uses innerHTML (supports `<strong>` tags). All translated.

## Interactive Tutorial

An 8-step guided tutorial with spotlight highlighting and a floating panel:
- **Step 1 (Welcome)**: larger centred panel, big emoji, no step counter — shown automatically on first visit
- **Steps 2–8**: compact panel positioned next to the highlighted element, with Back/Next/Skip buttons
- **Spotlight**: dark overlay with a transparent cutout over the target element (CSS box-shadow technique)
- **Auto-show**: opens automatically on first visit via `localStorage` (`tutorialSeen` key); never auto-shows again after that
- **🎓 Tutorial button**: always visible above the 📖 Guide button — same style
- **Panel positioning**: repositioned after scroll settles (350ms timeout) to avoid off-screen placement; fully clamped within viewport on all 4 edges; on mobile always pinned to bottom
- **All 5 languages**: 20 translation keys per language (4 UI labels + 8 × title/description pairs)
- **Survives re-renders**: panel and spotlight live outside `#app` so `render()` calls don't destroy them

## Common Pitfalls

- **Single-quoted JS strings**: the translation values use single quotes. Be careful with apostrophes in text (e.g. `won't` must be `won\\'t`). The guide sections use backtick template literals where apostrophes are safe.
- **Always validate JS** after edits: `node -e "new Function(scriptContent)"` — a broken apostrophe will silently kill the entire page with no error shown to the user.
- **Biga is fixed** at 100g flour + 40g water — these values are not user-editable, they're displayed as computed fields.
- **Salt is included in bun weight** — this is baked into the formula and affects how total flour is derived.
- **iOS auto-zoom**: input fields must have `font-size: 16px` or larger, otherwise Safari zooms in when the user taps an input.
- **iOS hides native spinners**: all `<input type="number">` are wrapped in `.num-wrap` with `.num-btn` (−/+) buttons. Use `adj(key, step, min, max)` for the button `onclick`. Buttons are hidden on desktop via CSS (`display:none` by default, shown via `@media(max-width:900px),(hover:none) and (pointer:coarse)`).
- **Update all 5 languages**: every text change must be made in EN, FR, ES, DE, and IT.

## File Structure

Single file: `index.html` (renamed from `Pizza_Dough_Calculator.html` to serve as GitHub Pages default)
- `<head>`: meta tags, Open Graph tags, PWA manifest (inline data URI), Apple touch icon (`icon.png`), Google Fonts link
- `<style>` block: all CSS including 3 responsive breakpoints + touch + safe-area rules + `overflow-x:hidden`
- `<script>` block: hero image (base64 data URI), presets, presetNames, presetDescs, translations (T object), state, compute(), render(), tooltip/modal functions, tutorial functions (startTutorial, showTutStep, tutGo, endTutorial, positionPanel, centerPanel), `upd()`, `adj()`, service worker registration, install prompt handler
- No external JS dependencies

## Additional Files
- `icon.png` — custom pizza slice icon used for home screen (apple-touch-icon) and WhatsApp/iMessage preview (og:image)
- `preview.jpg` — landscape photo extracted from hero image (kept for reference, not actively used)
