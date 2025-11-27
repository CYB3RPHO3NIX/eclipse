---
# Fill in the fields below to create a basic custom agent for your repository.
# The Copilot CLI can be used for local testing: https://gh.io/customagents/cli
# To make this agent available, merge this file into the default repository branch.
# For format details, see: https://gh.io/customagents/config

name:
description:
---

# My Agent

# GitHub Copilot Instructions & Best Practices

> A practical, actionable guide for developing **a large, production-ready CSS framework** (components, icons, docs site, demo pages, dark theme) modeled visually after Windows XP theme. Includes dev workflow, naming conventions, accessibility, performance, packaging, and how to prompt GitHub Copilot for maximum productivity.

---

## 1. Project vision & constraints

- Build a single, cohesive CSS framework that includes:
  - Core design tokens (colors, spacing, typography, breakpoints)
  - A full component library (buttons, forms, cards, navs, tables, modals, tooltips, badges, loaders, grids, etc.)
  - A complete icon set (SVGs) so users do **not** need to import external icon frameworks.
  - Responsive support for mobile/tablet/desktop (mobile‑first).
  - Dark theme switch using the same tokens and class patterns.
  - A `docs/` folder: `docs/index.html` (landing + side menu), component pages showing live demos + code snippets for every combination.

- Important constraints and caveats:
  - **Licensing:** Font Awesome has free and paid (Pro) icons. You must **not** include any icons without verifying license. Prefer using only the Font Awesome **Free** set or other truly open icon sets (e.g., Heroicons, Feather) unless you obtain proper license for Pro icons. Document this clearly for contributors.
  - **Bundle size:** Including thousands of SVGs inline will increase repo size and delivered payload. Provide tooling to generate optimized SVG sprites and optional lazy-loading.

---

## 2. Recommended repository layout

```
my-framework/
├─ package.json
├─ README.md
├─ CONTRIBUTING.md
├─ LICENSE
├─ .github/
│  ├─ ISSUE_TEMPLATE.md
│  └─ PULL_REQUEST_TEMPLATE.md
├─ src/
│  ├─ tokens/                  # CSS variables and JSON tokens
│  │  ├─ colors.json
│  │  └─ spacing.json
│  ├─ base/                    # normalize, root variables
│  │  └─ _reset.css
│  ├─ utilities/               # small, atomic utilities (u-*)
│  ├─ layout/                  # grid, container (l-*)
│  ├─ components/              # component CSS files (c-*)
│  │  ├─ button.css
│  │  └─ card.css
│  ├─ icons/                   # source SVGs (not committed if large) and build scripts
│  │  └─ raw/*.svg
│  ├─ themes/                  # theme overrides (dark.css)
│  └─ index.css                # build entrypoint
├─ build/                      # generated assets
├─ docs/
│  ├─ index.html               # docs landing (side menu + demos)
│  ├─ components/*.html        # component single pages
│  └─ assets/                  # css/js/images used by docs
├─ scripts/                    # build scripts (node/gulp/rollup)
└─ tools/                      # tools for svg generation, tests
```

Use predictable prefixes for file/ folder purpose: `tokens`, `base`, `utilities`, `layout`, `components`, `themes`, `icons`.

---

## 3. CSS architecture & naming conventions

**Recommended architecture:** ITCSS (Inverted Triangle CSS) + BEM for components + utility classes with `u-` prefix.

- **Layering (ITCSS):**
  1. Settings (tokens) — variables, JSON
  2. Tools — mixins, functions (Sass/PostCSS)
  3. Generic — normalize.css
  4. Elements — raw element styles (a, p, h1)
  5. Objects — layout primitives (media object, grid) (use `o-` prefix if needed)
  6. Components — UI components (use `c-` prefix)
  7. Utilities — single-purpose classes (use `u-` prefix)

- **BEM + prefixes:**
  - Component classes: `c-button`, `c-card`.
  - BEM modifiers: `c-button--primary`, `c-button--large`.
  - Elements: `c-card__title`, `c-nav__item`.
  - State classes: `is-active`, `is-hidden`, `is-disabled` (avoid coupling state to descendant selectors).

- **Utility classes:**
  - `u-clearfix`, `u-hidden`, `u-mt-2`, `u-p-1`. Keep utilities atomic and predictable.

- **Why prefixes?** Keeps global namespace clean, and makes it safer for consumers to drop in the framework without name collisions.

---

## 4. Tokens & Theming

- Keep all design tokens in a single source (JSON + CSS variables). Example `tokens/colors.json` and `tokens/spacing.json`.
- Generate CSS variables from tokens: `:root { --color-bg: #fff; --color-text: #24292e; }`
- For dark theme use `[data-theme="dark"] { --color-bg: #0d1117; ... }` or `html[data-theme='dark']`.
- Keep tokens semantic (do not tie to components): `--color-bg`, `--color-surface`, `--color-primary`, `--radius-base`.
- Provide a theming guide and a `theme-generator` script to create alternate palettes.

---

## 5. Components: structure, variants, and combinations

- Each component must have:
  - A single `.css` (or `.scss`) file named after the component (e.g., `c-button.css`).
  - A demo HTML showing every state and variant.
  - Clear API: documented classes, modifiers, and expected markup.
- Follow *one markup* pattern per component (a single recommended markup), and then document modifier classes for alternate styles. E.g. `c-button` with `c-button--outline`, `c-button--danger`.
- For composition/combinations, create a `combinations/` page that shows common pairings (button group + icon buttons, form + validation states, etc.).

---

## 6. Icons: inclusion strategy & tooling

**⚠️ Licensing & size warning (must include in README and CONTRIBUTING):**
- Do **not** include any Font Awesome Pro icons without license. Confirm and record which icon set is being included.
- Including *literally all* icons will bloat the repo. Offer an opt-in build pipeline that creates a minimized sprite and a split package (core + icons).

**Recommended approach:**
1. **Source the icons** legally (Font Awesome Free, or other OSS icon packs) and keep sources in `icons/raw/` (or instruct contributors to fetch them via npm and build).
2. **Build pipeline**: use `svgo` to optimize, then `svg-sprite` or `svgstore` to produce a single `<symbol>` sprite and a mapping JSON.
3. **Runtime usage**:
   - Provide an `<svg>` sprite loader (inject sprite into DOM once) or an `use` helper that references `#icon-name`.
   - Provide icon component wrappers (`<span class="c-icon" data-icon="github">`), and a small JS helper to resolve and insert the SVG inline for accessibility (add `aria-hidden` or proper `role` and `title` attributes when used decoratively or as meaningful content).
4. **Alternative distribution**: ship `core.css` and a separate `icons.svg` (or `icons.zip`) so consumers who don't want the big icon payload can skip it.
5. **Performance**: compress sprite, offer gzipped assets, use caching headers and `Cache-Control`.

**File naming:** `icons/raw/fa-free/fa-github.svg` and mapping `icons/map.json`.

---

## 7. Docs site (docs/index.html) & component demos

- **Landing page layout**:
  - Left vertical side menu (sticky) with search, sections, and anchors.
  - Main content area that shows component demos, code snippet (HTML) and a "copy" button.
  - Theme toggle (light / dark) in top-right.
  - Responsive: side menu collapses to a hamburger on mobile.
- **For each demo:** show
  - Live demo (interactive)
  - Full markup (syntax-highlighted)
  - Notes: accessibility concerns, props/classes used
  - Variant matrix (table/grid) showing permutations & their classes
- **UX features:** code copy, expand/collapse for large combos, keyboard accessible menu, persistent theme selection (localStorage).
- **Docs generation:** prefer hand-written simple HTML + partial templates or a static site generator (11ty) if you want automation. Keep a minimal JS footprint.

---

## 8. Accessibility (A11y)

- Components must be keyboard operable and follow WAI-ARIA practices.
- Provide ARIA roles for interactive components (e.g., `role="dialog"` with focus trap for modals).
- Use semantic HTML whenever possible (buttons `<button>`, links `<a>`).
- Color contrast: validate that text and UI elements meet WCAG AA (4.5:1 for normal text; 3:1 for large text).
- Test with `axe-core`, Lighthouse, and manual keyboard testing.

---

## 9. Responsiveness & layout

- Adopt **mobile-first** CSS: styles for narrow widths first, then media queries for `min-width` breakpoints.
- Suggested breakpoints (semantic):
  - small (mobile): 0–599px
  - medium (tablet): 600–899px
  - large (desktop): 900px+
- Use relative units (rem, em, %). Keep containers fluid and test common device widths.
- Document responsive variants (e.g., `c-button--block@sm`). Consider utility helpers: `@media` utility modifiers or responsive utility classes like `u-hidden@md`.

---

## 10. Build tooling & automation

- **Core toolchain:** Node.js scripts + PostCSS (with `postcss-cli`), optionally Sass for variables/mixins. Use `autoprefixer`, `cssnano` for production.
- **Icon build:** `svgo` -> `svg-sprite` -> generate `icons.svg` and `icons.json` mapping.
- **Dev server:** simple static server for `docs/` (e.g., `live-server` or `vite` dev server).
- **Testing:** visual regression (Percy/Chromatic), unit tests for any JS helpers, accessibility checks with `axe`.
- **CI:** GitHub Actions pipeline for linting, build, visual tests, publish to npm and GitHub Releases.
- **Versioning & releases:** Semantic Versioning (SemVer). Tag releases and publish changelog.

---

## 11. Performance & production optimizations

- Split `core.css` vs `icons.svg` vs `docs.css` so consumers load only required parts.
- Use `purgecss` or PostCSS tool to remove unused CSS for final builds (with caution — maintain safelist for dynamic classes).
- Minify and gzip production assets; provide `prefetch`/`preload` for critical assets if bundling.
- Avoid heavy runtime JS. Keep interactive JS optional.

---

## 12. Code style, linting & review

- Use Prettier for formatting; ESLint for JS helpers.
- Use `stylelint` for CSS linting (set rules for specificity, selector length, etc.).
- Create PR and commit templates; require reviews and at least one approving review before merge.
- Automate changelog generation with `standard-version` or `semantic-release`.

---

## 13. Testing checklist (per component)

1. Browser rendering (latest Chrome, Firefox, Safari) + at least one mobile (iOS/Android) check.
2. Keyboard navigation and focus styles.
3. Screen reader check (NVDA/VoiceOver basic test).
4. Contrast & visual audit via Lighthouse/axe.
5. Visual regression screenshot.
6. Bundle size impact (icons + CSS) measured.

---

## 14. GitHub Copilot: how to prompt it effectively

- Use short, explicit prompts and include examples.
- Example prompts for Copilot when writing a component:
  - `// Create c-button with BEM classes, size and variant modifiers, accessible markup, and CSS variables`.
  - `/* Provide HTML demo showing c-button default, primary, outline, disabled */`.
  - `// Write a Node script that optimizes SVGs using svgo and generates an SVG sprite`.
- When Copilot suggests code that touches licensing or icons, always manually verify the source.
- Use Copilot for repetitive tasks: generating demo snippets, creating template HTML for components, scaffolding testcases.

---

## 15. Contributor guidelines & onboarding

- `CONTRIBUTING.md` must include:
  - How to set up the project locally
  - How to run the docs server
  - Icon contribution guidelines (naming, optimization, metadata, license proof)
  - PR checklist (tests, accessibility audit, visual snapshots)
- Add ISSUE and PR templates to guide contributors.

---

## 16. Example developer commands (single-line friendly)

- Install dependencies: `npm install`
- Run dev docs server: `npm run dev`
- Build production CSS: `npm run build:css`
- Generate icons sprite: `npm run build:icons`
- Run linters: `npm run lint`
- Run tests: `npm run test`

---

## 17. Delivery models for icons (choose one or offer multiple packages)

1. **All-in-one**: `framework-with-icons.zip` — biggest size, zero extra imports.
2. **Core-only**: `framework-core.css` + `icons-optional.zip` — smaller default footprint.
3. **On-demand**: Icon npm package with a small runtime loader to fetch single icons as needed.

Document tradeoffs and provide automation for each.

---

## 18. Final notes & checklist before first release

- [ ] Confirm legal status of every icon included.
- [ ] Ensure docs show every component + variant + code snippet.
- [ ] Implement theme toggle and persistent preference.
- [ ] Add performance budgets and automated checks in CI.
- [ ] Provide UI for copying markup and example usage in multiple frameworks (HTML, React example optionally).

---

## Appendix: Quick prompts for GitHub Copilot

- `"Create a responsive c-card component using BEM naming; include title, subtitle, image, and action area; include accessible markup and keyboard support."`
- `"Generate a demo page that shows all c-button variants and includes copy-to-clipboard for markup."`
- `"Write a node script: read icons from icons/raw, optimize with svgo, create SVG sprite and JSON mapping file"`

---

If you want, I can now:
- generate a starting repository scaffold (file tree + sample files) as a zip,
- create the `docs/index.html` skeleton with side menu and demo placeholders,
- create a `c-button.css` and `c-button.html` demo example,
- or produce build scripts (node) for the icon sprite.

Tell me which of those you'd like me to create next and I'll generate it.

