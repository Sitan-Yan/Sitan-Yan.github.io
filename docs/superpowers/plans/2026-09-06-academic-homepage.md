# Academic Homepage Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Build an English, research-first academic homepage for Sitan Yan that presents his PhD profile and reflective micro-hole SFF research clearly on desktop and mobile.

**Architecture:** A framework-free GitHub Pages site uses semantic `index.html`, a single responsive stylesheet, and a small progressively enhanced navigation script. Repository-local images and the copied CV keep the published page self-contained; a standard-library Python validator checks content, anchors, assets, accessibility hooks, and design constraints.

**Tech Stack:** HTML5, CSS3, vanilla JavaScript, Python 3 standard library, GitHub Pages

**Spec:** `docs/superpowers/specs/2026-09-06-academic-homepage-design.md`

## Global Constraints

- Write only inside `E:\Abroad_Application\Sitan-Yan.github.io`.
- Treat the supplied CV, manuscript, and figure PDFs outside the repository as read-only sources.
- Use English throughout the public homepage.
- Use no framework, package manager, build step, gradients, animated backgrounds, parallax, or ornamental motion.
- Use verified facts from the supplied CV and manuscript; do not invent profile URLs, research results, or biographical claims.
- Create the homepage only; link to `/projects/sff-micropore/` without creating that project detail page.
- Keep the layout responsive, keyboard accessible, and free of horizontal overflow.

## File Map

- Modify `index.html`: all semantic homepage content, metadata, navigation, and section markup.
- Modify `style.css`: color tokens, typography, layouts, responsive behavior, and focus states.
- Modify `script.js`: mobile navigation, active navigation state, and footer year.
- Create `assets/images/micropore-hero.png`: generated decorative hero background, explicitly non-data.
- Create `assets/images/sff-micropore-results.webp`: optimized web rendering derived from the author's `ablation_qual.pdf` figure.
- Create `assets/documents/Sitan-Yan-CV.pdf`: byte-for-byte copy of the supplied CV.
- Create `tests/validate_site.py`: dependency-free structural and asset validator.

---

### Task 1: Establish the Site Contract and Prepare Research Assets

**Files:**
- Create: `tests/validate_site.py`
- Create: `assets/images/micropore-hero.png`
- Create: `assets/images/sff-micropore-results.webp`
- Create: `assets/documents/Sitan-Yan-CV.pdf`

**Interfaces:**
- Consumes: `E:\Abroad_Application\CV\SItanYan_CV.pdf` and `E:\master_grduate_material\4_3D_reconstruction\picture_process\正文\绘图\pdf\ablation_qual.pdf` as read-only inputs.
- Produces: repository-local asset paths consumed verbatim by `index.html`.

- [ ] **Step 1: Write a failing structural validator**

Create `tests/validate_site.py` with an `HTMLParser` subclass that records start tags, element IDs, links, images, and heading levels. Require these exact IDs: `about`, `research`, `publications`, `education`, and `contact`. Require local assets `assets/images/micropore-hero.png`, `assets/images/sff-micropore-results.webp`, and `assets/documents/Sitan-Yan-CV.pdf`. Require each non-decorative image to have non-empty `alt`, each navigation anchor to resolve to an existing ID, one `h1`, a `button` carrying `aria-controls="site-navigation"`, and no `target="_blank"` without `rel="noopener noreferrer"`.

The script must exit with status 1 and print every violation; otherwise print `Site validation passed.` and exit 0.

- [ ] **Step 2: Run the validator and verify the baseline fails**

Run:

```powershell
& 'C:\Users\xiaoyan\.cache\codex-runtimes\codex-primary-runtime\dependencies\python\python.exe' tests\validate_site.py
```

Expected: exit 1, reporting missing required sections and local assets.

- [ ] **Step 3: Copy the CV without altering the source**

Create `assets/documents`, copy `SItanYan_CV.pdf` to `assets/documents/Sitan-Yan-CV.pdf`, and compare SHA-256 hashes with `Get-FileHash`. The two hashes must match.

- [ ] **Step 4: Render and inspect the real SFF figure**

Render the first page of `ablation_qual.pdf` at 220 DPI into a repository-local intermediate PNG, inspect it visually, crop only surrounding whitespace if needed, and export `assets/images/sff-micropore-results.webp` at high quality. Preserve labels and scientific content; do not retouch data.

- [ ] **Step 5: Generate the decorative hero artwork**

Use the built-in image generation tool with this prompt:

```text
Use case: scientific-educational
Asset type: subtle background artwork for an academic researcher's homepage
Primary request: a restrained abstract visualization inspired by optical shape-from-focus measurement of a reflective industrial micro-hole
Scene/backdrop: white and very light cool-gray field
Subject: one precise circular micro-hole opening with faint concentric depth contours, sparse focus-plane traces, and a subtle blue measurement axis
Style/medium: clean scientific editorial illustration, realistic enough to suggest optical metrology but clearly decorative rather than experimental data
Composition/framing: wide landscape composition; keep the left-center quiet enough for dark text; concentrate detail toward the right edge
Lighting/mood: soft neutral studio light, calm and rigorous
Color palette: white, graphite gray, muted academic blue
Constraints: no text, no numbers, no logos, no watermark, no gradient background, no neon colors, no fabricated chart or claimed result
```

Inspect the result, copy the chosen output into `assets/images/micropore-hero.png`, and confirm that the webpage will identify it as decorative.

- [ ] **Step 6: Commit the site contract and assets**

```powershell
git add tests/validate_site.py assets/images/micropore-hero.png assets/images/sff-micropore-results.webp assets/documents/Sitan-Yan-CV.pdf
git commit -m "test: define homepage contract and add research assets"
```

### Task 2: Build the Semantic English Homepage

**Files:**
- Modify: `index.html`
- Test: `tests/validate_site.py`

**Interfaces:**
- Consumes: the three exact repository-local asset paths from Task 1.
- Produces: section IDs and navigation semantics consumed by `style.css`, `script.js`, and the validator.

- [ ] **Step 1: Extend the validator with content assertions**

Require these strings in `index.html`: `Sitan Yan`, `Huazhong University of Science and Technology`, `Selected Research`, `Reliability-Aware Shape-from-Focus`, `Major Revision`, `21.5`, `1.277`, `12.3`, `Publications & Manuscripts`, `Education`, and `sitan@hust.edu.cn`. Require links containing `mailto:sitan@hust.edu.cn`, `https://github.com/Sitan-Yan`, `/projects/sff-micropore/`, `assets/documents/Sitan-Yan-CV.pdf`, and `https://doi.org/10.1109/ICUAS57906.2023.10156435`.

- [ ] **Step 2: Run the validator and confirm content assertions fail**

Run the validator and expect exit 1 with missing-content messages.

- [ ] **Step 3: Replace `index.html` with semantic page content**

Implement:

- `<header>` with the name, desktop navigation, and accessible mobile menu button.
- `<main>` with `about`, `research`, `publications`, `education`, and `contact` sections.
- Hero copy describing an M.Eng. candidate working at the intersection of robotic perception, automated optical metrology, motion planning, and microscopic 3-D reconstruction.
- A text-based portrait placeholder using the initials `SY`, labeled `Portrait placeholder` so it is never mistaken for a photograph.
- A functional Google Scholar search link for `"Sitan Yan" HUST`, labeled `Google Scholar search`, rather than an invented profile identifier.
- SFF project authors, revision status, problem-method-outcome summary, the three verified metrics, real figure caption, and the future project route.
- Two publication entries from the CV, with the ICUAS DOI link.
- HUST master's and bachelor's education entries with dates, GPA, and Distinguished Graduate recognition.
- Contact invitation and footer year target `<span id="current-year">2026</span>`.

Use relative asset paths so the site works at the GitHub Pages root. Add Open Graph metadata without inventing a canonical domain beyond `https://sitan-yan.github.io/`.

- [ ] **Step 4: Run the validator and verify structural/content checks pass**

Run the validator. Expected: asset and HTML checks pass; CSS or JavaScript checks added later may still be absent.

- [ ] **Step 5: Commit the homepage content**

```powershell
git add index.html tests/validate_site.py
git commit -m "feat: add research-first academic homepage content"
```

### Task 3: Implement the Editorial Responsive Visual System

**Files:**
- Modify: `style.css`
- Test: `tests/validate_site.py`

**Interfaces:**
- Consumes: class names and IDs defined in Task 2.
- Produces: desktop and mobile layouts with CSS custom properties and a `nav-open` body state used by `script.js`.

- [ ] **Step 1: Add stylesheet constraints to the validator**

Require `--color-accent: #1f5f8b`, `max-width: 70rem`, `@media (max-width: 760px)`, `:focus-visible`, and `prefers-reduced-motion`. Reject the substrings `linear-gradient`, `radial-gradient`, `@keyframes`, and `backdrop-filter`.

- [ ] **Step 2: Run the validator and confirm stylesheet checks fail**

Expected: exit 1 with missing CSS contract messages.

- [ ] **Step 3: Implement `style.css`**

Define a restrained palette, serif heading stack, system sans-serif body stack, 70rem content width, thin rules, and consistent spacing. Implement:

- Sticky white header without blur.
- Two-column hero with the generated artwork as a low-contrast background layer and a high-contrast text panel.
- Circular `SY` portrait placeholder with a solid border and no shadow-heavy treatment.
- Research media/text grid, compact metric row, numbered publication list, and aligned education entries.
- Clear link underlines and visible blue keyboard focus outlines.
- A 760px breakpoint that stacks hero and research blocks, converts navigation to a menu, keeps 44px touch targets, and prevents horizontal overflow.
- Reduced-motion behavior that disables smooth scrolling.

- [ ] **Step 4: Run the validator and verify CSS checks pass**

Expected: `Site validation passed.` once the script task is also complete, or only the pending JavaScript checks remain.

- [ ] **Step 5: Commit the visual system**

```powershell
git add style.css tests/validate_site.py
git commit -m "feat: add responsive academic editorial styling"
```

### Task 4: Add Progressive Navigation Behavior

**Files:**
- Modify: `script.js`
- Test: `tests/validate_site.py`

**Interfaces:**
- Consumes: `#menu-toggle`, `#site-navigation`, section IDs, navigation anchors, and `#current-year` from Task 2; toggles `body.nav-open` from Task 3.
- Produces: correct `aria-expanded` state, mobile-menu closing behavior, active link state, and current year text.

- [ ] **Step 1: Add JavaScript contract assertions**

Require the strings `aria-expanded`, `nav-open`, `IntersectionObserver`, `current-year`, and `DOMContentLoaded` in `script.js`. Reject `document.write` and inline timer loops.

- [ ] **Step 2: Run the validator and confirm JavaScript checks fail**

Expected: exit 1 with missing script contract messages.

- [ ] **Step 3: Implement the minimal script**

On `DOMContentLoaded`:

- Toggle `body.nav-open` from `#menu-toggle` and synchronize `aria-expanded`.
- Close the mobile menu after selecting a navigation link and when Escape is pressed.
- Use one `IntersectionObserver` to apply `aria-current="location"` to the active section link.
- Set `#current-year` from `new Date().getFullYear()`.
- Exit safely if optional elements are unavailable.

- [ ] **Step 4: Run the validator and verify all automated checks pass**

Expected output: `Site validation passed.`

- [ ] **Step 5: Commit the progressive enhancement**

```powershell
git add script.js tests/validate_site.py
git commit -m "feat: add accessible homepage navigation"
```

### Task 5: Browser QA and Final Repository Audit

**Files:**
- Modify if defects are found: `index.html`, `style.css`, `script.js`, or repository-local assets
- Test: `tests/validate_site.py`

**Interfaces:**
- Consumes: the complete homepage from Tasks 1-4.
- Produces: a visually verified GitHub Pages-ready repository.

- [ ] **Step 1: Start a local static server**

Run from the repository root:

```powershell
& 'C:\Users\xiaoyan\.cache\codex-runtimes\codex-primary-runtime\dependencies\python\python.exe' -m http.server 4173
```

- [ ] **Step 2: Inspect desktop presentation in the browser**

Open `http://127.0.0.1:4173/` at a desktop viewport. Confirm the first viewport communicates identity and research direction, the real SFF figure is sharp and correctly captioned, text remains readable over the decorative background, all sections align, and external links have clear labels.

- [ ] **Step 3: Inspect phone presentation and navigation**

Use a viewport near 390 by 844 pixels. Confirm no horizontal overflow, hero and research content stack in the intended order, the menu opens and closes, Escape closes it, touch targets remain usable, and the metrics wrap without clipping.

- [ ] **Step 4: Perform keyboard and content QA**

Tab through the page, verify focus visibility and sensible order, check every anchor and document link, and confirm the future `/projects/sff-micropore/` link is clearly marked as the forthcoming detail page. Confirm that no searched third-party scientific image is published; the search results are used only as visual research for the original generated decorative artwork.

- [ ] **Step 5: Re-run automated validation and audit repository scope**

Run:

```powershell
& 'C:\Users\xiaoyan\.cache\codex-runtimes\codex-primary-runtime\dependencies\python\python.exe' tests\validate_site.py
git status --short
git diff --check
```

Expected: validator passes, `git diff --check` has no output, and every changed path is inside the homepage repository.

- [ ] **Step 6: Commit any QA corrections**

If corrections were required, commit only the affected homepage files:

```powershell
git add index.html style.css script.js assets tests/validate_site.py
git commit -m "fix: polish academic homepage presentation"
```

