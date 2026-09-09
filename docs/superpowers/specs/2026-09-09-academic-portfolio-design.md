# Academic Portfolio Website — Design Spec

**Date:** 2026-09-09
**Status:** Draft — pending user review
**Owner:** Sitan Yan (M.Eng. Mech Eng, HUST)
**Goal:** Rebuild the personal academic portfolio as a modern, content-rich single-page React site with a dedicated project sub-page template, replacing the current vanilla HTML/CSS implementation.

---

## 1. Problem & Goals

### 1.1 Current state
- Vanilla HTML/CSS/JS at `E:\Abroad_Application\Sitan-Yan.github.io\`
- Hosted on GitHub Pages at `https://sitan-yan.github.io/`
- 6 main content sections (About, Interests, Education, Research, Publications, Awards) plus nav and footer
- 1 static sub-page at `projects/reliability-aware-sff/index.html` with image/video assets
- Visual style is restrained and functional but reads as a generic template; layout density is decent but the design language doesn't differentiate from many peer sites

### 1.2 Goals
1. **Differentiate** — produce a portfolio that reads as a serious researcher's site, not a template, while remaining academically tasteful (no marketing flash, no cyberpunk).
2. **Compress signal** — give a PhD advisor or admissions committee a clear picture in 1–2 minutes: who you are, what you research, what you've shipped, what you're looking for.
3. **Showcase depth** — three featured research projects rendered with a consistent, scannable Problem / Method / Contribution / Result / Technology structure, each with a full sub-page.
4. **Future-proof content** — content lives in typed TS data files; adding a project or publication should be a one-line change.
5. **Stay portable** — the site must remain hostable on GitHub Pages with zero server-side requirements.

### 1.3 Non-goals (out of scope for v1)
- Blog, notes, or writing section
- Contact form (mailto only)
- i18n / bilingual content
- Analytics or tracking
- CMS
- Comments / social embed

### 1.4 Target audience
Primary: PhD advisors and admissions committees evaluating applications. Secondary: peer researchers, lab mates, prospective collaborators. They scan, then drill into specific projects.

---

## 2. Information Architecture

The site is a single-page React app with one route template for project sub-pages. All eight (actually nine) sections live on the home route. Project sub-pages are reached via `/projects/:slug`.

### 2.1 Home page section order

| # | Section ID | Section name | Purpose |
|---|---|---|---|
| 1 | `#hero` | Hero | Identity, position, advisor, contact CTAs |
| 2 | `#about` | About Me | Academic background in 2–3 sentences |
| 3 | `#interests` | Research Interests | 3 large interest cards with icon + description |
| 4 | `#projects` | Selected Research Projects | 3 featured projects, full structured blocks |
| 5 | `#publications` | Publications & Research Experience | Publication list + lab affiliation block |
| 6 | `#skills` | Skills | Programming / Frameworks / Research Tools |
| 7 | `#education` | Education | Degree timeline |
| 8 | `#contact` | Contact | Email + social + location + opportunity status |
| 9 | `#awards` | Honors & Awards | Year-grouped recognition list (positioned last per user request) |

### 2.2 Section content (source = current site unless noted)

**Hero**
- Name: `Sitan Yan`
- Position: `M.Eng. Mechanical Engineering`
- Institution: `Huazhong University of Science and Technology (HUST)`
- Lab: `State Key Laboratory of Digital Manufacturing Equipment and Technology`
- Advisor: `Prof. Wenlong Li` (with link to faculty page)
- Research one-liner (new, derived from existing bio): `Robotic perception, precision measurement, and motion planning under uncertainty.`
- Status line (new): `Open to PhD opportunities starting Fall 2027.`
- CTAs: `CV` (PDF), `GitHub`, `Email`, `LinkedIn`
- Right side: SY monogram placeholder (square card with subtle gradient border)

**About Me**
- 2–3 sentence paragraph derived from the existing `bio` paragraph in the current site, plus a one-line note about PhD application intent.

**Research Interests** (3 cards; user said 3–4, we use 3 to keep the section tight)
1. **Robotic Perception & Control** — Visual servoing, focus-based control, sensorimotor coupling for precision tasks.
2. **Automated Optical Metrology** — Microscopic 3D reconstruction, shape-from-focus, reliability under reflection and noise.
3. **Motion Planning & Optimization** — Time-optimal trajectory planning, MPC, planning under uncertainty.

**Selected Research Projects** (3 featured, structured)
- Source: existing 4 projects, we promote the 3 strongest:
  1. **Reliability-Aware Microscopic 3D Reconstruction for Reflective Micro-Holes** — `Completed`, Jun 2025 – Present (will be the sub-page migrated from current `projects/reliability-aware-sff/`)
  2. **Focus-Constrained Visual Predictive Control for Robotic Micro-Hole Localization** — `Manuscript in Preparation`, Jun 2026 – Aug 2026
  3. **Robotic Dynamic Obstacle Avoidance Control based on Partial RGB-D Observations** — `Ongoing`, Jan 2026 – Present
- The 4th project (CFRP Trajectory Planning) is **not** a featured project on the home page (kept as research-experience data but not a top-3 slot).
- Each project renders 5 fields: `Problem`, `Method`, `My Contribution`, `Result`, `Technologies` (the last as a row of pill badges).

**Publications & Research Experience**
- Publications (3, year-grouped):
  1. 2026 — *Reliability-aware shape-from-focus for reflective 3-D micro-hole measurement* — **S. Yan**, W. Xu, L. Zeng, W. Li — IEEE TIM (Under Revision)
  2. 2026 — *Focus-Constrained Visual Predictive Control for Robotic Micro-Hole Localization* — **S. Yan** et al. — IEEE/ASME T-Mech (In Preparation)
  3. 2023 — *Preliminary Design and Prototype Development of an Air-ground Carrier Platform* — T. Chen, J. Han, J. Wang, **S. Yan**, C. Wan, F. Tian — ICUAS 2023 — [IEEE Xplore link]
- Research Experience block: lab name, advisor, dates, current research focus (1 sentence).

**Skills** (3 columns)
- **Programming:** Python, C++, MATLAB
- **Frameworks & Libraries:** OpenCV, PyTorch, ROS / ROS 2, NumPy, SciPy
- **Research Tools:** Gazebo, RobotStudio, Git, LaTeX
- *(Content above is inferred from current projects; user to verify/adjust during content pass.)*

**Education**
- Sep 2024 – Jun 2027 — M.Eng. Mechanical Engineering, HUST
- Sep 2020 – Jun 2024 — B.Eng. Mechanical Design, Manufacturing and Automation, HUST

**Contact**
- Email: `sitan@hust.edu.cn`
- GitHub: `https://github.com/Sitan-Yan`
- LinkedIn: `https://www.linkedin.com/in/sitan-yan-ab3509434/`
- CV: `files/Sitan_Yan_CV.pdf`
- Location: Wuhan, China
- Status: `Open to research opportunities`

**Honors & Awards** (positioned last per user)
- 2024 — Distinguished Graduate of HUST
- 2022 — First Prize, Chinese Mathematics Competitions (CMC)
- 2022 — Third Prize, China Collegiate Intelligent Robotics Creative Competition
- 2021 — Academic Excellence Scholarship

### 2.3 Project sub-page route (`/projects/:slug`)

**Header**
- Status badge
- Title (h1, large)
- Date range (monospace)
- Tech stack pill row

**Body sections** (in order):
1. **Overview / Problem** — 2–3 sentence problem statement
2. **Method** — 1 paragraph + supporting figure
3. **My Contribution** — bullet list
4. **Results** — paragraph + key quantitative outcomes + supporting figure
5. **Media Gallery** — images and video, lazy-loaded, click to expand
6. **Footer** — back link to home, prev/next project navigation

**Initial sub-page data:** `reliability-aware-sff` (migrated from current `projects/reliability-aware-sff/`). The other two featured projects will have placeholder sub-pages that say "Full write-up coming soon — see publication for details." This keeps all 3 links on the home page working.

---

## 3. Visual System

### 3.1 Style direction
Editorial-academic with a "modern product page" overlay. Inspired by current ML PhD sites (Karpathy, Junxian He) for restraint, and Zhipu BigModel (https://open.bigmodel.cn/) for content rhythm and card treatment. The two influences combine to: editorial typography, generous whitespace, but section headers, cards, and hero all feel substantial and "designed" — not generic-template.

Reference anchors (already discussed with user):
- **Editorial restraint:** Karpathy (karpathy.ai) — no decoration, typography does the work
- **Section/list patterns:** Junxian He (jxhe.github.io) — sidebar-style sections
- **Badge / publication list:** Tri Dao (tridao.me) — Pill-style status badges on publications
- **Card rhythm & hero treatment:** Zhipu BigModel (open.bigmodel.cn) — large cards, soft gradient hero, large bold section titles

### 3.2 Color palette

**Light mode**

| Token | Value | Use |
|---|---|---|
| `background` | `#ffffff` | Page background |
| `foreground` | `#0a0a0a` | Primary text |
| `muted` | `#71717a` | Secondary text, dates, eyebrows |
| `muted-foreground` | `#a1a1aa` | Tertiary text |
| `border` | `#e4e4e7` | Hairlines, card borders |
| `card` | `#fafafa` | Card background |
| `card-foreground` | `#0a0a0a` | Card text |
| `accent` | `#1e40af` (blue-800) | Links, key emphasis |
| `accent-foreground` | `#ffffff` | Text on accent |
| `accent-muted` | `#dbeafe` (blue-100) | Accent background tint |
| `success` | `#059669` (emerald-600) | Completed status |
| `warning` | `#d97706` (amber-600) | In Preparation status |
| `info` | `#2563eb` (blue-600) | Ongoing status |
| `purple` | `#7c3aed` | Under Revision status |

**Dark mode** (paired automatically)

| Token | Value |
|---|---|
| `background` | `#0a0a0a` |
| `foreground` | `#fafafa` |
| `muted` | `#a1a1aa` |
| `border` | `#27272a` |
| `card` | `#0f0f10` |
| `accent` | `#60a5fa` (blue-400) |
| `accent-muted` | `#1e3a8a` (blue-900) |
| `success` | `#10b981` |
| `warning` | `#f59e0b` |
| `info` | `#3b82f6` |
| `purple` | `#a78bfa` |

**Hero gradient (decorative, both modes)**
- Light: `radial-gradient(at 30% 20%, #dbeafe 0%, transparent 50%), radial-gradient(at 80% 80%, #ede9fe 0%, transparent 50%)` over `#ffffff`
- Dark: `radial-gradient(at 30% 20%, rgba(30,64,175,0.2) 0%, transparent 50%), radial-gradient(at 80% 80%, rgba(124,58,237,0.15) 0%, transparent 50%)` over `#0a0a0a`

**Featured-card subtle gradient** (used on the first project card to draw the eye)
- `linear-gradient(135deg, rgba(30,64,175,0.04) 0%, rgba(124,58,237,0.04) 100%)` over `card` color

### 3.3 Typography

- **Body:** Inter 400/500, 16px base, line-height 1.6
- **Headings:** Inter 600/700
  - h1 (hero name): clamp(3rem, 6vw, 5.5rem), letter-spacing -0.03em
  - h2 (section title): clamp(1.875rem, 3.5vw, 2.75rem), letter-spacing -0.02em
  - h3 (card / project title): 1.5rem, letter-spacing -0.01em
- **Eyebrow / labels:** Inter 500, 0.75rem, uppercase, letter-spacing 0.08em, color muted
- **Mono (dates, project numbers, file sizes):** JetBrains Mono 400
- **No serif type** — keep Inter for the entire site for visual cohesion; serif would push us toward the "classic paper" direction we explicitly rejected

### 3.4 Spacing & layout

- **Max content width:** 1100px (home sections), 760px (project sub-page body text)
- **Section padding:** 96px (desktop) / 64px (mobile) top and bottom
- **Card padding:** 32px (desktop) / 24px (mobile)
- **Inter-section spacing:** 0 — sections are visually separated by their own background and section headers
- **Border radius:** cards `rounded-xl` (12px), badges `rounded-full`, buttons `rounded-lg` (8px)

### 3.5 Motion

- **Section reveal:** `BlurFade` (Magic UI) — 8px y-offset, 0.5s ease-out, 0.05s stagger between children, one-shot on scroll into view
- **Sticky header:** starts transparent; after 32px scroll, gains a 1px bottom border + `backdrop-blur-md` + 80% bg opacity
- **Card hover:** `border-accent/30` + slight color shift, 150ms ease
- **Theme toggle:** 200ms color crossfade on `background` and `foreground`
- **No parallax, no bouncing, no marquee, no autoplay video**
- All animations wrapped in `prefers-reduced-motion` check — disables motion when set

### 3.6 Component patterns

- **Cards:** `border` 1px + `rounded-xl`, no shadow. Optional subtle gradient bg on featured card.
- **Pill badges:** `rounded-full`, `px-2.5 py-0.5`, `text-xs font-medium`, color-coded per status type.
- **Section header pattern:** eyebrow + h2 + optional 1-line description, all left-aligned, max-width 720px.
- **Hairline dividers:** `<Separator />` (shadcn) between publication entries, education entries, etc. — 1px, color `border`.
- **Links:** inline text links color `accent`, underline on hover only.
- **Buttons:** two styles — `primary` (accent bg) and `ghost` (border only). No filled-shadow 3D buttons.

### 3.7 Responsive

- Mobile-first
- **Mobile (<640px):** single column, hamburger nav (shadcn `Sheet`), hero stacks vertically, project cards stack
- **Tablet (640–1024px):** interests stay single column for breathing room, projects stack, hero side-by-side
- **Desktop (≥1024px):** full layout — 3-column interests, projects stack (each is a content-heavy card, not grid), 3-column skills

---

## 4. Tech Stack

| Concern | Choice | Rationale |
|---|---|---|
| Framework | Vite + React 18 + TypeScript | Small static SPA, trivial GH Pages deploy, no SSR overhead |
| Styling | Tailwind CSS v3 + CSS variables | Utility-first, `class` strategy for dark mode, matches shadcn tokens |
| Component primitives | shadcn/ui (copy-in source) | Zero runtime weight, full control, consistent with Tailwind tokens |
| Animation | Magic UI (`BlurFade`, `GridBackground`) + Framer Motion | Restrained entrance + hero backdrop only |
| Icons | lucide-react | Tree-shakable, consistent stroke width, React-native |
| Routing | React Router v6 | Simple `/` and `/projects/:slug` |
| Theme | next-themes (works outside Next) | System default, persists to localStorage, no flash |
| Linting | ESLint + Prettier + `prettier-plugin-tailwindcss` | Standard React tooling |
| Deploy | GitHub Pages via `gh-pages` CLI | Current host, zero infra |

**Why not Next.js:** The site is fully static with no per-request data, no SEO-critical content beyond what static HTML already provides, and no need for SSR. Next.js would add Node build complexity and an extra framework layer for zero gain.

**Why not MDX for projects:** 3 projects, each ~300 words of structured content. MDX would introduce `rehype`, `remark`, MDX loader, and a content pipeline for content that fits cleanly in a typed TS object. If long-form project writeups become a thing, we can graduate to MDX later.

---

## 5. File Structure

```
E:\Abroad_Application\Sitan-Yan.github.io\
├── index.html                  # Vite entry, will be replaced during build
├── 404.html                    # SPA fallback (copy of index.html) — committed
├── package.json
├── tsconfig.json
├── vite.config.ts
├── tailwind.config.ts
├── postcss.config.js
├── .eslintrc.cjs
├── .prettierrc
├── .gitignore
├── public/
│   ├── files/
│   │   └── Sitan_Yan_CV.pdf    # keep existing
│   ├── images/
│   │   ├── profile-monogram.svg # SY monogram, generated
│   │   └── og.png              # social preview
│   └── projects/               # moved from /projects/reliability-aware-sff/assets/
│       └── reliability-aware-sff/
│           ├── micro-hole.png
│           ├── sff-system.png
│           ├── sff-system.webp
│           └── micro-hole.mp4
├── src/
│   ├── main.tsx
│   ├── App.tsx
│   ├── styles/
│   │   └── globals.css         # tailwind + CSS variables + base resets
│   ├── lib/
│   │   ├── utils.ts            # cn() helper, etc.
│   │   └── theme.ts            # next-themes config
│   ├── data/
│   │   ├── profile.ts          # name, role, advisor, links, education, awards
│   │   ├── interests.ts        # 3 interest cards
│   │   ├── projects.ts         # 3 featured projects + sub-page content
│   │   ├── publications.ts     # 3 publications
│   │   └── skills.ts           # 3 skill columns
│   ├── components/
│   │   ├── ui/                 # shadcn primitives (button, card, badge, separator, sheet, tooltip, tabs)
│   │   ├── magicui/
│   │   │   ├── blur-fade.tsx
│   │   │   └── grid-background.tsx
│   │   ├── layout/
│   │   │   ├── theme-provider.tsx
│   │   │   ├── theme-toggle.tsx
│   │   │   ├── header.tsx
│   │   │   ├── footer.tsx
│   │   │   └── container.tsx
│   │   └── sections/
│   │       ├── hero.tsx
│   │       ├── about.tsx
│   │       ├── research-interests.tsx
│   │       ├── research-projects.tsx
│   │       ├── publications.tsx
│   │       ├── skills.tsx
│   │       ├── education.tsx
│   │       ├── contact.tsx
│   │       └── awards.tsx
│   └── pages/
│       ├── home.tsx
│       ├── project-page.tsx
│       └── not-found.tsx
└── docs/
    └── superpowers/
        └── specs/
            └── 2026-09-09-academic-portfolio-design.md  # this file
```

### 5.1 Data file shape (sketch)

```ts
// src/data/projects.ts
export type ProjectStatus = 'completed' | 'ongoing' | 'in-preparation' | 'planned';

export interface Project {
  slug: string;
  number: string;          // "01", "02", "03"
  title: string;
  dateRange: string;       // "Jun 2025 — Present"
  status: ProjectStatus;
  summary: string;         // 1-line card preview
  featured?: boolean;      // true for the gradient-bg card
  problem: string;
  method: string;
  contribution: string[];  // bullet list
  result: string;
  technologies: string[];  // pill badges
  media?: {
    images?: { src: string; alt: string; caption?: string }[];
    videos?: { src: string; poster?: string; caption?: string }[];
  };
  links?: { label: string; href: string }[];
  comingSoon?: boolean;    // if true, sub-page shows placeholder
}
```

```ts
// src/data/publications.ts
export interface Publication {
  year: number;
  title: string;
  authors: string[];       // rendered with **S. Yan** bolded for self
  venue: string;
  status: 'published' | 'under-revision' | 'in-preparation';
  links?: { label: string; href: string }[];
}
```

Other data files follow the same typed pattern.

---

## 6. Deployment

### 6.1 Build output
- `npm run build` → `dist/`
- Vite config: `base: '/Sitan-Yan.github.io/'` so assets resolve correctly under the project page
- React Router uses `BrowserRouter` with `<Routes>` — works because the SPA fallback (404.html) catches deep links

### 6.2 GitHub Pages routing fallback
- Build copies `dist/index.html` to `dist/404.html` (Vite plugin or simple post-build script)
- GH Pages serves `404.html` for any unknown path; the React app then routes client-side

### 6.3 Deploy steps (manual, documented in README)
1. `npm install`
2. `npm run build`
3. `npx gh-pages -d dist`
4. (Or) push `dist/` to a `gh-pages` branch directly

### 6.4 CI (optional, not in v1)
- A GitHub Action can run `npm run build && gh-pages -d dist` on push to `main`, but this is deferred — manual deploy is fine for v1.

---

## 7. Accessibility & Performance

- **Semantic HTML** — `<header>`, `<main>`, `<section>`, `<article>`, `<footer>`, `<nav>`
- **Color contrast** — text on background meets WCAG AA in both modes (verify with axe after build)
- **Keyboard** — all interactive elements reachable, visible focus ring (`focus-visible:ring-2 ring-accent ring-offset-2`)
- **Reduced motion** — `prefers-reduced-motion: reduce` disables all transitions and BlurFade
- **Images** — explicit `width`/`height` to prevent CLS, `loading="lazy"` on below-fold images, `alt` text on every image
- **Performance budget** — initial JS bundle < 150KB gzipped, LCP < 2.5s on 3G Fast, CLS < 0.1
- **Meta** — `<title>`, `og:title`, `og:description`, `og:image`, `description`, favicon
- **No autoplay, no carousels, no scroll-jacking**

---

## 8. Risks & Open Items

| Risk | Mitigation |
|---|---|
| Skills content is inferred from project stack, not user-confirmed | Mark TODO; first content pass prompts user to verify |
| Project sub-pages for the 2 non-SFF projects have no real content | Render as "Full write-up coming soon" placeholder; link to publication for in-prep paper |
| `index.html` for Vite will overwrite the current one during build | Back up current site (git) before scaffolding; build will replace it |
| Existing sub-page `projects/reliability-aware-sff/index.html` will be removed once new build deploys | Migrate its assets into `public/projects/reliability-aware-sff/` before deleting the HTML |
| 404.html SPA fallback on GH Pages can show 404 briefly before client takes over | Acceptable; add `<noscript>` message telling users to enable JS |

---

## 9. Out-of-Scope Reminders

To prevent scope creep during implementation:
- No blog, no notes, no writing section
- No contact form (mailto only)
- No i18n (English only)
- No analytics
- No CMS
- No comments
- No autoplay media
- No custom illustrations or commissioned photography (use monogram + existing project images only)
- No new project sub-page content beyond SFF (the other two render placeholders)

---

## 10. Acceptance Criteria (for the implementation plan)

The implementation is complete when:
1. `npm run dev` starts the site locally on `localhost:5173`
2. `npm run build && npm run preview` produces a working production build
3. All 9 home sections render with content from the data files
4. `/projects/reliability-aware-sff` renders the full sub-page with images and video
5. `/projects/focus-constrained-vpc` and `/projects/dynamic-obstacle-avoidance` render "coming soon" placeholders
6. Light/dark toggle works and persists across reloads
7. The site is responsive on viewports 360px, 768px, 1024px, 1440px
8. `npx gh-pages -d dist` deploys to GitHub Pages and the site loads at the project URL
9. Deep links to `/projects/reliability-aware-sff` work (404 fallback verified)
10. Lighthouse score: Performance ≥ 90, Accessibility ≥ 95, Best Practices ≥ 95
