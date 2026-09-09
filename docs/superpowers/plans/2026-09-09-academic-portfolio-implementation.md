# Academic Portfolio Website — Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Build a modern, content-rich single-page React academic portfolio with a project sub-page template, replacing the current vanilla HTML/CSS implementation at `E:\Abroad_Application\Sitan-Yan.github.io\`, deployable to GitHub Pages at `https://sitan-yan.github.io/`.

**Architecture:** Vite + React 18 + TypeScript SPA with React Router for `/` (home) and `/projects/:slug` (project sub-pages). All content lives in typed TS data files under `src/data/`. Tailwind CSS + CSS variables power light/dark theming via `next-themes`. shadcn/ui provides copy-in primitives; Magic UI provides `BlurFade` (section reveal) and `GridBackground` (hero backdrop). 404.html serves as SPA fallback for deep links on GitHub Pages.

**Tech Stack:** Vite 5, React 18, TypeScript 5, Tailwind CSS v3, shadcn/ui, Magic UI, Framer Motion, lucide-react, React Router v6, next-themes, gh-pages (CLI), ESLint + Prettier.

**Spec:** `docs/superpowers/specs/2026-09-09-academic-portfolio-design.md` (commit `e582be7`).

---

## Global Constraints

These are project-wide requirements that every task implicitly inherits. Values are copied verbatim from the spec.

- **Base path:** `/Sitan-Yan.github.io/` (Vite `base` config + `<base href>` in `index.html`)
- **Node:** ≥ 18.18
- **Package manager:** npm (lockfile: `package-lock.json`)
- **TypeScript:** strict mode, no `any` in new code
- **Tailwind config:** dark mode = `class`
- **CSS variables:** defined in `:root` and `.dark` for all design tokens; no hardcoded colors in components
- **Fonts:** Inter (sans, 400/500/600/700) + JetBrains Mono (mono, 400), loaded from Google Fonts
- **Color tokens (light):** `background=#ffffff`, `foreground=#0a0a0a`, `muted=#71717a`, `muted-foreground=#a1a1aa`, `border=#e4e4e7`, `card=#fafafa`, `card-foreground=#0a0a0a`, `accent=#1e40af`, `accent-foreground=#ffffff`, `accent-muted=#dbeafe`, `success=#059669`, `warning=#d97706`, `info=#2563eb`, `purple=#7c3aed`
- **Color tokens (dark):** `background=#0a0a0a`, `foreground=#fafafa`, `muted=#a1a1aa`, `border=#27272a`, `card=#0f0f10`, `accent=#60a5fa`, `accent-muted=#1e3a8a`, `success=#10b981`, `warning=#f59e0b`, `info=#3b82f6`, `purple=#a78bfa`
- **Border radius:** cards `rounded-xl` (12px), badges `rounded-full`, buttons `rounded-lg` (8px)
- **Section padding:** 96px desktop / 64px mobile top+bottom
- **Max width:** home 1100px, sub-page body 760px
- **All animations:** wrapped in `prefers-reduced-motion` guard
- **Accessibility:** WCAG AA contrast, focus-visible rings, semantic HTML, lazy images, explicit width/height on images
- **Deploy target:** GitHub Pages via `gh-pages -d dist`
- **Shell:** all commands in this plan are PowerShell (Windows), no `&&` chains, no bash-isms
- **Commit style:** conventional commits, `feat:` / `chore:` / `docs:` / `style:` / `fix:`

---

## File Structure (target end state)

```
E:\Abroad_Application\Sitan-Yan.github.io\
├── package.json
├── package-lock.json
├── tsconfig.json
├── tsconfig.node.json
├── vite.config.ts
├── tailwind.config.ts
├── postcss.config.js
├── components.json                  # shadcn config
├── .eslintrc.cjs
├── .prettierrc
├── .gitignore
├── index.html                       # Vite entry
├── 404.html                         # SPA fallback (manually maintained)
├── README.md                        # updated with build/deploy instructions
├── public/
│   ├── files/Sitan_Yan_CV.pdf
│   ├── images/
│   │   └── profile-monogram.svg
│   └── projects/
│       └── reliability-aware-sff/
│           ├── micro-hole.png       # migrated from /projects/.../assets/
│           ├── sff-system.png
│           ├── sff-system.webp
│           └── micro-hole.mp4
├── src/
│   ├── main.tsx
│   ├── App.tsx
│   ├── vite-env.d.ts
│   ├── styles/globals.css
│   ├── lib/
│   │   ├── utils.ts
│   │   └── theme.ts
│   ├── data/
│   │   ├── profile.ts
│   │   ├── interests.ts
│   │   ├── projects.ts
│   │   ├── publications.ts
│   │   └── skills.ts
│   ├── components/
│   │   ├── ui/                      # shadcn primitives (button, card, badge, separator, sheet, tooltip, tabs, scroll-area)
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
├── docs/                            # already exists
│   └── superpowers/
│       ├── specs/2026-09-09-academic-portfolio-design.md
│       └── plans/2026-09-09-academic-portfolio-implementation.md
└── existing files preserved until task replaces them:
    ├── index.html                   # old site, replaced in Task 1
    ├── css/main.css
    ├── js/script.js
    ├── projects/reliability-aware-sff/index.html
    └── projects/reliability-aware-sff/assets/...
```

---

## Task 1: Scaffold Vite + React + TypeScript

**Files:**
- Replace: `E:\Abroad_Application\Sitan-Yan.github.io\package.json`
- Replace: `E:\Abroad_Application\Sitan-Yan.github.io\index.html`
- Create: `E:\Abroad_Application\Sitan-Yan.github.io\vite.config.ts`
- Create: `E:\Abroad_Application\Sitan-Yan.github.io\tsconfig.json`
- Create: `E:\Abroad_Application\Sitan-Yan.github.io\tsconfig.node.json`
- Create: `E:\Abroad_Application\Sitan-Yan.github.io\src\main.tsx`
- Create: `E:\Abroad_Application\Sitan-Yan.github.io\src\App.tsx`
- Create: `E:\Abroad_Application\Sitan-Yan.github.io\src\vite-env.d.ts`

**Note:** The current `index.html` (vanilla site) and `css/`, `js/`, `projects/reliability-aware-sff/` will be removed in Task 21 (Migration). For now, back them up to `legacy-vanilla/` so they can be referenced.

- [ ] **Step 1: Back up the existing vanilla site**

```powershell
Set-Location -LiteralPath "E:\Abroad_Application\Sitan-Yan.github.io"
New-Item -ItemType Directory -Path "legacy-vanilla" -Force | Out-Null
Move-Item -Path "css" -Destination "legacy-vanilla\css"
Move-Item -Path "js" -Destination "legacy-vanilla\js"
Move-Item -Path "projects" -Destination "legacy-vanilla\projects"
Get-ChildItem -Path "legacy-vanilla" -Recurse | Select-Object FullName
```

Expected: `css/`, `js/`, `projects/` all moved under `legacy-vanilla/`.

- [ ] **Step 2: Create the Vite project in-place**

```powershell
Set-Location -LiteralPath "E:\Abroad_Application\Sitan-Yan.github.io"
npm create vite@latest . -- --template react-ts
```

When prompted "Directory is not empty", choose "Ignore files and continue" (or pre-clean: the only existing items should be `.git`, `README.md`, `legacy-vanilla/`, `docs/`).

- [ ] **Step 3: Install dependencies**

```powershell
Set-Location -LiteralPath "E:\Abroad_Application\Sitan-Yan.github.io"
npm install
```

- [ ] **Step 4: Verify dev server starts**

```powershell
Set-Location -LiteralPath "E:\Abroad_Application\Sitan-Yan.github.io"
npm run dev
```

Open `http://localhost:5173/` in a browser. Expected: Vite + React default page (blue React logo). Stop the server with `Ctrl+C`.

- [ ] **Step 5: Commit**

```powershell
Set-Location -LiteralPath "E:\Abroad_Application\Sitan-Yan.github.io"
git add -A
git status
git commit -m "chore: scaffold Vite + React + TS"
```

---

## Task 2: Configure Vite base path and install core libraries

**Files:**
- Modify: `E:\Abroad_Application\Sitan-Yan.github.io\vite.config.ts`
- Modify: `E:\Abroad_Application\Sitan-Yan.github.io\index.html`
- Modify: `E:\Abroad_Application\Sitan-Yan.github.io\package.json` (via npm install)

- [ ] **Step 1: Install runtime dependencies**

```powershell
Set-Location -LiteralPath "E:\Abroad_Application\Sitan-Yan.github.io"
npm install react-router-dom@^6.26.0 framer-motion@^11.0.0 lucide-react@^0.400.0 next-themes@^0.3.0 clsx@^2.1.0 tailwind-merge@^2.0.0 class-variance-authority@^0.7.0
```

- [ ] **Step 2: Install dev dependencies**

```powershell
Set-Location -LiteralPath "E:\Abroad_Application\Sitan-Yan.github.io"
npm install -D tailwindcss@^3.4.0 postcss@^8.4.0 autoprefixer@^10.4.0 prettier@^3.0.0 prettier-plugin-tailwindcss@^0.6.0
```

- [ ] **Step 3: Set the Vite base path to the GitHub Pages project path**

Replace contents of `vite.config.ts` with:

```ts
import { defineConfig } from 'vite';
import react from '@vitejs/plugin-react';
import path from 'node:path';

export default defineConfig({
  base: '/Sitan-Yan.github.io/',
  plugins: [react()],
  resolve: {
    alias: {
      '@': path.resolve(__dirname, './src'),
    },
  },
});
```

- [ ] **Step 4: Update `index.html` to set base href and meta tags**

Replace contents of `index.html` with:

```html
<!doctype html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <base href="/Sitan-Yan.github.io/" />
    <link rel="icon" type="image/svg+xml" href="/images/profile-monogram.svg" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <meta name="description" content="Sitan Yan — M.Eng. Mechanical Engineering at HUST. Research in robotic perception, precision measurement, and motion planning under uncertainty." />
    <meta name="theme-color" content="#ffffff" />
    <meta property="og:title" content="Sitan Yan | Academic Homepage" />
    <meta property="og:description" content="M.Eng. student at HUST. Research in robotics, 3D vision, and precision engineering." />
    <meta property="og:type" content="profile" />
    <link rel="preconnect" href="https://fonts.googleapis.com" />
    <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin />
    <link href="https://fonts.googleapis.com/css2?family=Inter:wght@400;500;600;700&family=JetBrains+Mono:wght@400;500&display=swap" rel="stylesheet" />
    <title>Sitan Yan | Academic Homepage</title>
  </head>
  <body>
    <div id="root"></div>
    <script type="module" src="/src/main.tsx"></script>
  </body>
</html>
```

- [ ] **Step 5: Verify dev server still works**

```powershell
Set-Location -LiteralPath "E:\Abroad_Application\Sitan-Yan.github.io"
npm run dev
```

Open `http://localhost:5173/Sitan-Yan.github.io/`. Expected: Vite default page loads under the new base path. Stop with `Ctrl+C`.

- [ ] **Step 6: Commit**

```powershell
Set-Location -LiteralPath "E:\Abroad_Application\Sitan-Yan.github.io"
git add -A
git commit -m "chore: install core deps and set Vite base path"
```

---

## Task 3: Configure Tailwind CSS

**Files:**
- Create: `E:\Abroad_Application\Sitan-Yan.github.io\tailwind.config.ts`
- Create: `E:\Abroad_Application\Sitan-Yan.github.io\postcss.config.js`
- Modify: `E:\Abroad_Application\Sitan-Yan.github.io\src\styles\globals.css` (will be created in this task; the existing `src/index.css` from Vite default will be replaced)
- Delete: `E:\Abroad_Application\Sitan-Yan.github.io\src\index.css` (default Vite file)
- Delete: `E:\Abroad_Application\Sitan-Yan.github.io\src\App.css` (default Vite file)

- [ ] **Step 1: Initialize Tailwind config files**

```powershell
Set-Location -LiteralPath "E:\Abroad_Application\Sitan-Yan.github.io"
npx tailwindcss init -p
```

- [ ] **Step 2: Replace `tailwind.config.ts` with the spec's design tokens**

```ts
import type { Config } from 'tailwindcss';

const config: Config = {
  darkMode: 'class',
  content: ['./index.html', './src/**/*.{ts,tsx}'],
  theme: {
    container: {
      center: true,
      padding: '1.5rem',
      screens: { '2xl': '1100px' },
    },
    extend: {
      colors: {
        background: 'hsl(var(--background))',
        foreground: 'hsl(var(--foreground))',
        muted: 'hsl(var(--muted))',
        'muted-foreground': 'hsl(var(--muted-foreground))',
        border: 'hsl(var(--border))',
        card: 'hsl(var(--card))',
        'card-foreground': 'hsl(var(--card-foreground))',
        accent: {
          DEFAULT: 'hsl(var(--accent))',
          foreground: 'hsl(var(--accent-foreground))',
          muted: 'hsl(var(--accent-muted))',
        },
        success: 'hsl(var(--success))',
        warning: 'hsl(var(--warning))',
        info: 'hsl(var(--info))',
        purple: 'hsl(var(--purple))',
      },
      borderRadius: {
        xl: '12px',
        '2xl': '16px',
      },
      fontFamily: {
        sans: ['Inter', 'ui-sans-serif', 'system-ui', '-apple-system', 'Segoe UI', 'sans-serif'],
        mono: ['"JetBrains Mono"', 'ui-monospace', 'SFMono-Regular', 'Menlo', 'monospace'],
      },
      maxWidth: {
        'content': '1100px',
        'reading': '760px',
      },
      keyframes: {
        'fade-in-up': {
          '0%': { opacity: '0', transform: 'translateY(8px)' },
          '100%': { opacity: '1', transform: 'translateY(0)' },
        },
      },
      animation: {
        'fade-in-up': 'fade-in-up 0.5s ease-out forwards',
      },
    },
  },
  plugins: [],
};

export default config;
```

- [ ] **Step 3: Create the global stylesheet with CSS variables and Tailwind directives**

Create `src/styles/globals.css` with:

```css
@tailwind base;
@tailwind components;
@tailwind utilities;

@layer base {
  :root {
    --background: 0 0% 100%;
    --foreground: 0 0% 4%;
    --muted: 240 4% 46%;
    --muted-foreground: 240 5% 65%;
    --border: 240 6% 90%;
    --card: 0 0% 98%;
    --card-foreground: 0 0% 4%;
    --accent: 224 76% 33%;
    --accent-foreground: 0 0% 100%;
    --accent-muted: 214 95% 93%;
    --success: 160 84% 29%;
    --warning: 32 95% 36%;
    --info: 217 91% 50%;
    --purple: 262 83% 58%;
  }

  .dark {
    --background: 0 0% 4%;
    --foreground: 0 0% 98%;
    --muted: 240 5% 65%;
    --muted-foreground: 240 4% 46%;
    --border: 240 4% 16%;
    --card: 240 4% 6%;
    --card-foreground: 0 0% 98%;
    --accent: 213 94% 68%;
    --accent-foreground: 0 0% 4%;
    --accent-muted: 224 76% 25%;
    --success: 160 84% 39%;
    --warning: 38 92% 50%;
    --info: 213 94% 60%;
    --purple: 262 83% 76%;
  }

  * {
    border-color: hsl(var(--border));
  }

  html {
    scroll-behavior: smooth;
    scroll-padding-top: 80px;
  }

  body {
    @apply bg-background text-foreground antialiased;
    font-feature-settings: 'rlig' 1, 'calt' 1;
  }

  ::selection {
    background-color: hsl(var(--accent-muted));
    color: hsl(var(--accent));
  }
}

@layer utilities {
  .text-balance {
    text-wrap: balance;
  }
}

@media (prefers-reduced-motion: reduce) {
  *,
  *::before,
  *::after {
    animation-duration: 0.01ms !important;
    animation-iteration-count: 1 !important;
    transition-duration: 0.01ms !important;
    scroll-behavior: auto !important;
  }
}
```

- [ ] **Step 4: Update `src/main.tsx` to import the new stylesheet and remove default imports**

Replace `src/main.tsx` with:

```tsx
import React from 'react';
import ReactDOM from 'react-dom/client';
import App from './App';
import './styles/globals.css';

ReactDOM.createRoot(document.getElementById('root')!).render(
  <React.StrictMode>
    <App />
  </React.StrictMode>,
);
```

- [ ] **Step 5: Remove default Vite styles and replace App.tsx with placeholder**

```powershell
Set-Location -LiteralPath "E:\Abroad_Application\Sitan-Yan.github.io"
Remove-Item -Path "src\index.css" -ErrorAction SilentlyContinue
Remove-Item -Path "src\App.css" -ErrorAction SilentlyContinue
```

Replace `src/App.tsx` with:

```tsx
function App() {
  return (
    <div className="min-h-screen bg-background text-foreground">
      <h1 className="p-8 text-3xl font-bold">Sitan Yan</h1>
    </div>
  );
}

export default App;
```

- [ ] **Step 6: Verify dev server still works and dark mode toggle via class works**

```powershell
Set-Location -LiteralPath "E:\Abroad_Application\Sitan-Yan.github.io"
npm run dev
```

Open `http://localhost:5173/Sitan-Yan.github.io/`. Expected: white background, dark text, "Sitan Yan" h1. In DevTools, run `document.documentElement.classList.add('dark')` — background should switch to near-black. Stop server.

- [ ] **Step 7: Commit**

```powershell
Set-Location -LiteralPath "E:\Abroad_Application\Sitan-Yan.github.io"
git add -A
git commit -m "style: configure Tailwind with design tokens"
```

---

## Task 4: Set up shadcn/ui

**Files:**
- Create: `E:\Abroad_Application\Sitan-Yan.github.io\components.json`
- Create: `E:\Abroad_Application\Sitan-Yan.github.io\src\lib\utils.ts`
- Create: `E:\Abroad_Application\Sitan-Yan.github.io\src\components\ui\button.tsx`
- Create: `E:\Abroad_Application\Sitan-Yan.github.io\src\components\ui\card.tsx`
- Create: `E:\Abroad_Application\Sitan-Yan.github.io\src\components\ui\badge.tsx`
- Create: `E:\Abroad_Application\Sitan-Yan.github.io\src\components\ui\separator.tsx`
- Create: `E:\Abroad_Application\Sitan-Yan.github.io\src\components\ui\sheet.tsx`
- Create: `E:\Abroad_Application\Sitan-Yan.github.io\src\components\ui\tooltip.tsx`
- Create: `E:\Abroad_Application\Sitan-Yan.github.io\src\components\ui\tabs.tsx`
- Create: `E:\Abroad_Application\Sitan-Yan.github.io\src\components\ui\scroll-area.tsx`

**Interfaces:**
- Consumes: Tailwind config from Task 3, `cn()` helper from `src/lib/utils.ts`
- Produces: shadcn-style components with `cn()` utility used for class merging

- [ ] **Step 1: Create `components.json` for shadcn config**

```json
{
  "$schema": "https://ui.shadcn.com/schema.json",
  "style": "default",
  "rsc": false,
  "tsx": true,
  "tailwind": {
    "config": "tailwind.config.ts",
    "css": "src/styles/globals.css",
    "baseColor": "slate",
    "cssVariables": true,
    "prefix": ""
  },
  "aliases": {
    "components": "@/components",
    "utils": "@/lib/utils",
    "ui": "@/components/ui",
    "lib": "@/lib",
    "hooks": "@/hooks"
  }
}
```

- [ ] **Step 2: Create `src/lib/utils.ts` with the `cn()` helper**

```ts
import { clsx, type ClassValue } from 'clsx';
import { twMerge } from 'tailwind-merge';

export function cn(...inputs: ClassValue[]) {
  return twMerge(clsx(inputs));
}
```

- [ ] **Step 3: Add shadcn dependencies**

```powershell
Set-Location -LiteralPath "E:\Abroad_Application\Sitan-Yan.github.io"
npm install @radix-ui/react-slot@^1.1.0 @radix-ui/react-dialog@^1.1.0 @radix-ui/react-separator@^1.1.0 @radix-ui/react-tooltip@^1.1.0 @radix-ui/react-tabs@^1.1.0 @radix-ui/react-scroll-area@^1.1.0
```

- [ ] **Step 4: Manually create the shadcn `button` component**

Create `src/components/ui/button.tsx`:

```tsx
import * as React from 'react';
import { Slot } from '@radix-ui/react-slot';
import { cva, type VariantProps } from 'class-variance-authority';
import { cn } from '@/lib/utils';

const buttonVariants = cva(
  'inline-flex items-center justify-center whitespace-nowrap rounded-lg text-sm font-medium transition-colors focus-visible:outline-none focus-visible:ring-2 focus-visible:ring-accent focus-visible:ring-offset-2 focus-visible:ring-offset-background disabled:pointer-events-none disabled:opacity-50',
  {
    variants: {
      variant: {
        default: 'bg-accent text-accent-foreground hover:bg-accent/90',
        ghost: 'border border-border bg-transparent hover:bg-card hover:text-foreground',
        outline: 'border border-accent/30 bg-transparent text-accent hover:bg-accent-muted',
        link: 'text-accent underline-offset-4 hover:underline',
      },
      size: {
        default: 'h-10 px-4 py-2',
        sm: 'h-9 px-3',
        lg: 'h-11 px-6 text-base',
        icon: 'h-10 w-10',
      },
    },
    defaultVariants: { variant: 'default', size: 'default' },
  },
);

export interface ButtonProps
  extends React.ButtonHTMLAttributes<HTMLButtonElement>,
    VariantProps<typeof buttonVariants> {
  asChild?: boolean;
}

const Button = React.forwardRef<HTMLButtonElement, ButtonProps>(
  ({ className, variant, size, asChild = false, ...props }, ref) => {
    const Comp = asChild ? Slot : 'button';
    return <Comp className={cn(buttonVariants({ variant, size, className }))} ref={ref} {...props} />;
  },
);
Button.displayName = 'Button';

export { Button, buttonVariants };
```

- [ ] **Step 5: Create the `card` component**

Create `src/components/ui/card.tsx`:

```tsx
import * as React from 'react';
import { cn } from '@/lib/utils';

const Card = React.forwardRef<HTMLDivElement, React.HTMLAttributes<HTMLDivElement>>(
  ({ className, ...props }, ref) => (
    <div
      ref={ref}
      className={cn('rounded-xl border border-border bg-card text-card-foreground', className)}
      {...props}
    />
  ),
);
Card.displayName = 'Card';

const CardHeader = React.forwardRef<HTMLDivElement, React.HTMLAttributes<HTMLDivElement>>(
  ({ className, ...props }, ref) => (
    <div ref={ref} className={cn('flex flex-col gap-1.5 p-6', className)} {...props} />
  ),
);
CardHeader.displayName = 'CardHeader';

const CardTitle = React.forwardRef<HTMLDivElement, React.HTMLAttributes<HTMLDivElement>>(
  ({ className, ...props }, ref) => (
    <div ref={ref} className={cn('text-lg font-semibold leading-tight tracking-tight', className)} {...props} />
  ),
);
CardTitle.displayName = 'CardTitle';

const CardDescription = React.forwardRef<HTMLDivElement, React.HTMLAttributes<HTMLDivElement>>(
  ({ className, ...props }, ref) => (
    <div ref={ref} className={cn('text-sm text-muted', className)} {...props} />
  ),
);
CardDescription.displayName = 'CardDescription';

const CardContent = React.forwardRef<HTMLDivElement, React.HTMLAttributes<HTMLDivElement>>(
  ({ className, ...props }, ref) => (
    <div ref={ref} className={cn('p-6 pt-0', className)} {...props} />
  ),
);
CardContent.displayName = 'CardContent';

const CardFooter = React.forwardRef<HTMLDivElement, React.HTMLAttributes<HTMLDivElement>>(
  ({ className, ...props }, ref) => (
    <div ref={ref} className={cn('flex items-center p-6 pt-0', className)} {...props} />
  ),
);
CardFooter.displayName = 'CardFooter';

export { Card, CardHeader, CardTitle, CardDescription, CardContent, CardFooter };
```

- [ ] **Step 6: Create the `badge` component**

Create `src/components/ui/badge.tsx`:

```tsx
import * as React from 'react';
import { cva, type VariantProps } from 'class-variance-authority';
import { cn } from '@/lib/utils';

const badgeVariants = cva(
  'inline-flex items-center rounded-full border px-2.5 py-0.5 text-xs font-medium transition-colors',
  {
    variants: {
      variant: {
        default: 'border-transparent bg-accent-muted text-accent',
        secondary: 'border-border bg-card text-foreground',
        success: 'border-transparent bg-success/10 text-success',
        warning: 'border-transparent bg-warning/10 text-warning',
        info: 'border-transparent bg-info/10 text-info',
        purple: 'border-transparent bg-purple/10 text-purple',
        outline: 'border-border text-muted',
      },
    },
    defaultVariants: { variant: 'default' },
  },
);

export interface BadgeProps extends React.HTMLAttributes<HTMLDivElement>, VariantProps<typeof badgeVariants> {}

function Badge({ className, variant, ...props }: BadgeProps) {
  return <div className={cn(badgeVariants({ variant }), className)} {...props} />;
}

export { Badge, badgeVariants };
```

- [ ] **Step 7: Create the `separator` component**

Create `src/components/ui/separator.tsx`:

```tsx
import * as React from 'react';
import * as SeparatorPrimitive from '@radix-ui/react-separator';
import { cn } from '@/lib/utils';

const Separator = React.forwardRef<
  React.ElementRef<typeof SeparatorPrimitive.Root>,
  React.ComponentPropsWithoutRef<typeof SeparatorPrimitive.Root>
>(({ className, orientation = 'horizontal', decorative = true, ...props }, ref) => (
  <SeparatorPrimitive.Root
    ref={ref}
    decorative={decorative}
    orientation={orientation}
    className={cn(
      'shrink-0 bg-border',
      orientation === 'horizontal' ? 'h-px w-full' : 'h-full w-px',
      className,
    )}
    {...props}
  />
));
Separator.displayName = SeparatorPrimitive.Root.displayName;

export { Separator };
```

- [ ] **Step 8: Create the `sheet` component (mobile nav drawer)**

Create `src/components/ui/sheet.tsx`:

```tsx
import * as React from 'react';
import * as DialogPrimitive from '@radix-ui/react-dialog';
import { X } from 'lucide-react';
import { cn } from '@/lib/utils';

const Sheet = DialogPrimitive.Root;
const SheetTrigger = DialogPrimitive.Trigger;
const SheetClose = DialogPrimitive.Close;
const SheetPortal = DialogPrimitive.Portal;

const SheetOverlay = React.forwardRef<
  React.ElementRef<typeof DialogPrimitive.Overlay>,
  React.ComponentPropsWithoutRef<typeof DialogPrimitive.Overlay>
>(({ className, ...props }, ref) => (
  <DialogPrimitive.Overlay
    ref={ref}
    className={cn(
      'fixed inset-0 z-50 bg-black/50 backdrop-blur-sm data-[state=open]:animate-in data-[state=closed]:animate-out data-[state=closed]:fade-out-0 data-[state=open]:fade-in-0',
      className,
    )}
    {...props}
  />
));
SheetOverlay.displayName = DialogPrimitive.Overlay.displayName;

const SheetContent = React.forwardRef<
  React.ElementRef<typeof DialogPrimitive.Content>,
  React.ComponentPropsWithoutRef<typeof DialogPrimitive.Content>
>(({ side = 'right', className, children, ...props }, ref) => (
  <SheetPortal>
    <SheetOverlay />
    <DialogPrimitive.Content
      ref={ref}
      className={cn(
        'fixed z-50 gap-4 bg-background p-6 shadow-lg transition ease-in-out',
        side === 'right' && 'right-0 top-0 h-full w-3/4 border-l sm:max-w-sm',
        className,
      )}
      {...props}
    >
      {children}
      <DialogPrimitive.Close className="absolute right-4 top-4 rounded-sm opacity-70 transition-opacity hover:opacity-100 focus:outline-none focus:ring-2 focus:ring-accent">
        <X className="h-5 w-5" />
        <span className="sr-only">Close</span>
      </DialogPrimitive.Close>
    </DialogPrimitive.Content>
  </SheetPortal>
));
SheetContent.displayName = DialogPrimitive.Content.displayName;

const SheetTitle = React.forwardRef<
  React.ElementRef<typeof DialogPrimitive.Title>,
  React.ComponentPropsWithoutRef<typeof DialogPrimitive.Title>
>(({ className, ...props }, ref) => (
  <DialogPrimitive.Title ref={ref} className={cn('text-lg font-semibold', className)} {...props} />
));
SheetTitle.displayName = DialogPrimitive.Title.displayName;

export { Sheet, SheetTrigger, SheetClose, SheetContent, SheetTitle, SheetPortal };
```

- [ ] **Step 9: Create the `tooltip`, `tabs`, `scroll-area` components**

Create `src/components/ui/tooltip.tsx`:

```tsx
import * as React from 'react';
import * as TooltipPrimitive from '@radix-ui/react-tooltip';
import { cn } from '@/lib/utils';

const TooltipProvider = TooltipPrimitive.Provider;
const Tooltip = TooltipPrimitive.Root;
const TooltipTrigger = TooltipPrimitive.Trigger;

const TooltipContent = React.forwardRef<
  React.ElementRef<typeof TooltipPrimitive.Content>,
  React.ComponentPropsWithoutRef<typeof TooltipPrimitive.Content>
>(({ className, sideOffset = 4, ...props }, ref) => (
  <TooltipPrimitive.Content
    ref={ref}
    sideOffset={sideOffset}
    className={cn(
      'z-50 overflow-hidden rounded-md border border-border bg-card px-3 py-1.5 text-xs text-foreground shadow-md',
      className,
    )}
    {...props}
  />
));
TooltipContent.displayName = TooltipPrimitive.Content.displayName;

export { Tooltip, TooltipTrigger, TooltipContent, TooltipProvider };
```

Create `src/components/ui/tabs.tsx`:

```tsx
import * as React from 'react';
import * as TabsPrimitive from '@radix-ui/react-tabs';
import { cn } from '@/lib/utils';

const Tabs = TabsPrimitive.Root;

const TabsList = React.forwardRef<
  React.ElementRef<typeof TabsPrimitive.List>,
  React.ComponentPropsWithoutRef<typeof TabsPrimitive.List>
>(({ className, ...props }, ref) => (
  <TabsPrimitive.List
    ref={ref}
    className={cn('inline-flex items-center justify-center rounded-lg bg-card p-1 text-muted', className)}
    {...props}
  />
));
TabsList.displayName = TabsPrimitive.List.displayName;

const TabsTrigger = React.forwardRef<
  React.ElementRef<typeof TabsPrimitive.Trigger>,
  React.ComponentPropsWithoutRef<typeof TabsPrimitive.Trigger>
>(({ className, ...props }, ref) => (
  <TabsPrimitive.Trigger
    ref={ref}
    className={cn(
      'inline-flex items-center justify-center whitespace-nowrap rounded-md px-3 py-1 text-sm font-medium transition-all data-[state=active]:bg-background data-[state=active]:text-foreground data-[state=active]:shadow-sm',
      className,
    )}
    {...props}
  />
));
TabsTrigger.displayName = TabsPrimitive.Trigger.displayName;

const TabsContent = React.forwardRef<
  React.ElementRef<typeof TabsPrimitive.Content>,
  React.ComponentPropsWithoutRef<typeof TabsPrimitive.Content>
>(({ className, ...props }, ref) => (
  <TabsPrimitive.Content ref={ref} className={cn('mt-4', className)} {...props} />
));
TabsContent.displayName = TabsPrimitive.Content.displayName;

export { Tabs, TabsList, TabsTrigger, TabsContent };
```

Create `src/components/ui/scroll-area.tsx`:

```tsx
import * as React from 'react';
import * as ScrollAreaPrimitive from '@radix-ui/react-scroll-area';
import { cn } from '@/lib/utils';

const ScrollArea = React.forwardRef<
  React.ElementRef<typeof ScrollAreaPrimitive.Root>,
  React.ComponentPropsWithoutRef<typeof ScrollAreaPrimitive.Root>
>(({ className, children, ...props }, ref) => (
  <ScrollAreaPrimitive.Root ref={ref} className={cn('relative overflow-hidden', className)} {...props}>
    <ScrollAreaPrimitive.Viewport className="h-full w-full rounded-[inherit]">{children}</ScrollAreaPrimitive.Viewport>
    <ScrollAreaPrimitive.Scrollbar orientation="vertical" className="flex touch-none select-none p-0.5 transition-colors">
      <ScrollAreaPrimitive.Thumb className="relative flex-1 rounded-full bg-border" />
    </ScrollAreaPrimitive.Scrollbar>
    <ScrollAreaPrimitive.Corner />
  </ScrollAreaPrimitive.Root>
));
ScrollArea.displayName = ScrollAreaPrimitive.Root.displayName;

export { ScrollArea };
```

- [ ] **Step 10: Add `tailwindcss-animate` plugin for shadcn transitions**

```powershell
Set-Location -LiteralPath "E:\Abroad_Application\Sitan-Yan.github.io"
npm install -D tailwindcss-animate@^1.0.7
```

Append `'tailwindcss-animate'` to the `plugins` array in `tailwind.config.ts` (after the empty array becomes `plugins: ['tailwindcss-animate']`).

- [ ] **Step 11: Verify build compiles**

```powershell
Set-Location -LiteralPath "E:\Abroad_Application\Sitan-Yan.github.io"
npm run build
```

Expected: `dist/` directory created with no TypeScript errors. If you see errors, run `npm run lint` and fix reported issues.

- [ ] **Step 12: Commit**

```powershell
Set-Location -LiteralPath "E:\Abroad_Application\Sitan-Yan.github.io"
git add -A
git commit -m "chore: set up shadcn/ui primitives"
```

---

## Task 5: Set up Magic UI BlurFade and GridBackground

**Files:**
- Create: `E:\Abroad_Application\Sitan-Yan.github.io\src\components\magicui\blur-fade.tsx`
- Create: `E:\Abroad_Application\Sitan-Yan.github.io\src\components\magicui\grid-background.tsx`

**Interfaces:**
- Consumes: `framer-motion` from Task 2
- Produces: `<BlurFade>` and `<GridBackground>` React components

- [ ] **Step 1: Create the `BlurFade` component**

Create `src/components/magicui/blur-fade.tsx`:

```tsx
'use client';

import { motion, useReducedMotion, type Variants } from 'framer-motion';
import { type ReactNode } from 'react';

interface BlurFadeProps {
  children: ReactNode;
  className?: string;
  delay?: number;
  yOffset?: number;
  inView?: boolean;
  inViewMargin?: string;
  blur?: string;
}

export function BlurFade({
  children,
  className,
  delay = 0,
  yOffset = 8,
  inView = false,
  inViewMargin = '-50px',
  blur = '6px',
}: BlurFadeProps) {
  const shouldReduceMotion = useReducedMotion();
  const variants: Variants = {
    hidden: { y: yOffset, opacity: 0, filter: `blur(${blur})` },
    visible: { y: 0, opacity: 1, filter: 'blur(0px)' },
  };

  if (shouldReduceMotion) {
    return <div className={className}>{children}</div>;
  }

  return (
    <motion.div
      className={className}
      initial="hidden"
      animate={inView ? undefined : 'visible'}
      whileInView={inView ? 'visible' : undefined}
      viewport={inView ? { once: true, margin: inViewMargin } : undefined}
      variants={variants}
      transition={{ delay, duration: 0.5, ease: 'easeOut' }}
    >
      {children}
    </motion.div>
  );
}
```

- [ ] **Step 2: Create the `GridBackground` component**

Create `src/components/magicui/grid-background.tsx`:

```tsx
import { cn } from '@/lib/utils';

interface GridBackgroundProps {
  className?: string;
  children?: React.ReactNode;
}

export function GridBackground({ className, children }: GridBackgroundProps) {
  return (
    <div
      className={cn(
        'relative isolate w-full overflow-hidden',
        'before:absolute before:inset-0 before:-z-10 before:bg-[linear-gradient(to_right,rgba(0,0,0,0.04)_1px,transparent_1px),linear-gradient(to_bottom,rgba(0,0,0,0.04)_1px,transparent_1px)] before:bg-[size:64px_64px]',
        'after:absolute after:inset-0 after:-z-10',
        "after:bg-[radial-gradient(ellipse_60%_50%_at_30%_20%,rgba(30,64,175,0.08),transparent_60%),radial-gradient(ellipse_60%_50%_at_80%_80%,rgba(124,58,237,0.06),transparent_60%)]",
        'dark:before:bg-[linear-gradient(to_right,rgba(255,255,255,0.04)_1px,transparent_1px),linear-gradient(to_bottom,rgba(255,255,255,0.04)_1px,transparent_1px)]',
        'dark:after:bg-[radial-gradient(ellipse_60%_50%_at_30%_20%,rgba(96,165,250,0.15),transparent_60%),radial-gradient(ellipse_60%_50%_at_80%_80%,rgba(167,139,250,0.10),transparent_60%)]',
        className,
      )}
    >
      {children}
    </div>
  );
}
```

- [ ] **Step 3: Wire a temporary smoke test in App.tsx**

Replace `src/App.tsx` with:

```tsx
import { BlurFade } from '@/components/magicui/blur-fade';
import { GridBackground } from '@/components/magicui/grid-background';

function App() {
  return (
    <GridBackground className="min-h-screen p-8">
      <BlurFade>
        <h1 className="text-3xl font-bold">Sitan Yan</h1>
      </BlurFade>
    </GridBackground>
  );
}

export default App;
```

- [ ] **Step 4: Verify the hero background and blur fade render**

```powershell
Set-Location -LiteralPath "E:\Abroad_Application\Sitan-Yan.github.io"
npm run dev
```

Open `http://localhost:5173/Sitan-Yan.github.io/`. Expected: subtle grid lines + soft blue/purple radial gradients in the background, "Sitan Yan" h1 fades in. Toggle dark mode via DevTools (`document.documentElement.classList.add('dark')`) — colors should invert. Stop server.

- [ ] **Step 5: Commit**

```powershell
Set-Location -LiteralPath "E:\Abroad_Application\Sitan-Yan.github.io"
git add -A
git commit -m "feat: add Magic UI BlurFade and GridBackground"
```

---

## Task 6: Set up theme provider and theme toggle

**Files:**
- Create: `E:\Abroad_Application\Sitan-Yan.github.io\src\components\layout\theme-provider.tsx`
- Create: `E:\Abroad_Application\Sitan-Yan.github.io\src\components\layout\theme-toggle.tsx`
- Modify: `E:\Abroad_Application\Sitan-Yan.github.io\src\App.tsx` (wrap with provider)

**Interfaces:**
- Consumes: `next-themes` from Task 2
- Produces: `ThemeProvider` (wraps the app), `ThemeToggle` (icon button in header)

- [ ] **Step 1: Create the `ThemeProvider` component**

Create `src/components/layout/theme-provider.tsx`:

```tsx
import { ThemeProvider as NextThemesProvider } from 'next-themes';
import { type ReactNode } from 'react';

interface ThemeProviderProps {
  children: ReactNode;
}

export function ThemeProvider({ children }: ThemeProviderProps) {
  return (
    <NextThemesProvider
      attribute="class"
      defaultTheme="system"
      enableSystem
      disableTransitionOnChange
    >
      {children}
    </NextThemesProvider>
  );
}
```

- [ ] **Step 2: Create the `ThemeToggle` component**

Create `src/components/layout/theme-toggle.tsx`:

```tsx
import { Moon, Sun } from 'lucide-react';
import { useTheme } from 'next-themes';
import { useEffect, useState } from 'react';
import { Button } from '@/components/ui/button';

export function ThemeToggle() {
  const { resolvedTheme, setTheme } = useTheme();
  const [mounted, setMounted] = useState(false);

  useEffect(() => {
    setMounted(true);
  }, []);

  if (!mounted) {
    return (
      <Button variant="ghost" size="icon" aria-label="Toggle theme">
        <Sun className="h-5 w-5" />
      </Button>
    );
  }

  return (
    <Button
      variant="ghost"
      size="icon"
      aria-label={`Switch to ${resolvedTheme === 'dark' ? 'light' : 'dark'} mode`}
      onClick={() => setTheme(resolvedTheme === 'dark' ? 'light' : 'dark')}
    >
      {resolvedTheme === 'dark' ? <Sun className="h-5 w-5" /> : <Moon className="h-5 w-5" />}
    </Button>
  );
}
```

- [ ] **Step 3: Wire provider into `App.tsx`**

Replace `src/App.tsx` with:

```tsx
import { ThemeProvider } from '@/components/layout/theme-provider';
import { ThemeToggle } from '@/components/layout/theme-toggle';
import { BlurFade } from '@/components/magicui/blur-fade';
import { GridBackground } from '@/components/magicui/grid-background';

function App() {
  return (
    <ThemeProvider>
      <GridBackground className="min-h-screen p-8">
        <div className="flex items-center justify-between">
          <BlurFade>
            <h1 className="text-3xl font-bold">Sitan Yan</h1>
          </BlurFade>
          <ThemeToggle />
        </div>
      </GridBackground>
    </ThemeProvider>
  );
}

export default App;
```

- [ ] **Step 4: Verify theme toggle works**

```powershell
Set-Location -LiteralPath "E:\Abroad_Application\Sitan-Yan.github.io"
npm run dev
```

Open `http://localhost:5173/Sitan-Yan.github.io/`. Expected: moon icon in top-right. Click it — should switch to sun icon and toggle dark mode. Reload the page — selected theme persists. Stop server.

- [ ] **Step 5: Commit**

```powershell
Set-Location -LiteralPath "E:\Abroad_Application\Sitan-Yan.github.io"
git add -A
git commit -m "feat: add theme provider and toggle"
```

---

## Task 7: Create the data layer

**Files:**
- Create: `E:\Abroad_Application\Sitan-Yan.github.io\src\data\profile.ts`
- Create: `E:\Abroad_Application\Sitan-Yan.github.io\src\data\interests.ts`
- Create: `E:\Abroad_Application\Sitan-Yan.github.io\src\data\projects.ts`
- Create: `E:\Abroad_Application\Sitan-Yan.github.io\src\data\publications.ts`
- Create: `E:\Abroad_Application\Sitan-Yan.github.io\src\data\skills.ts`

**Interfaces (consumed by all section components in Tasks 8–17):**
- `Profile` (name, role, institution, lab, advisor, status line, links, education[], awards[])
- `Interest` (slug, title, description, icon)
- `Project` (slug, number, title, dateRange, status, summary, featured, problem, method, contribution[], result, technologies[], media, links, comingSoon)
- `Publication` (year, title, authors[], venue, status, links[])
- `SkillColumn` (category, items[])

- [ ] **Step 1: Create `src/data/profile.ts`**

```ts
export interface EducationEntry {
  degree: string;
  major: string;
  institution: string;
  start: string;
  end: string;
  location: string;
}

export interface AwardEntry {
  year: number;
  title: string;
}

export interface ProfileLink {
  label: string;
  href: string;
  kind: 'cv' | 'github' | 'linkedin' | 'email' | 'scholar';
}

export interface Profile {
  name: string;
  initials: string;
  role: string;
  institution: string;
  institutionShort: string;
  lab: string;
  advisor: {
    name: string;
    href: string;
  };
  oneLiner: string;
  statusLine: string;
  about: string;
  researchExperience: {
    lab: string;
    advisor: string;
    dates: string;
    focus: string;
  };
  links: ProfileLink[];
  education: EducationEntry[];
  awards: AwardEntry[];
  location: string;
}

export const profile: Profile = {
  name: 'Sitan Yan',
  initials: 'SY',
  role: 'M.Eng. Mechanical Engineering',
  institution: 'Huazhong University of Science and Technology',
  institutionShort: 'HUST',
  lab: 'State Key Laboratory of Digital Manufacturing Equipment and Technology',
  advisor: {
    name: 'Prof. Wenlong Li',
    href: 'https://mse.hust.edu.cn/info/1142/1340.htm',
  },
  oneLiner:
    'Robotic perception, precision measurement, and motion planning under uncertainty.',
  statusLine: 'Open to PhD opportunities starting Fall 2027.',
  about:
    "I'm a Master's student in Mechanical Engineering at HUST, working on reliable perception, measurement, and control methods for robotic and precision-engineering systems. My current work spans microscopic 3D reconstruction, robotic visual control, and motion planning under uncertainty. I'm actively preparing PhD applications for Fall 2027 and would be glad to hear from potential advisors working at the intersection of robotics, computer vision, and precision engineering.",
  researchExperience: {
    lab: 'State Key Laboratory of Digital Manufacturing Equipment and Technology',
    advisor: 'Prof. Wenlong Li',
    dates: 'Sep 2024 — Present',
    focus:
      'Reliable perception and control for robotic and precision-engineering systems.',
  },
  links: [
    { label: 'CV', href: '/Sitan-Yan.github.io/files/Sitan_Yan_CV.pdf', kind: 'cv' },
    { label: 'GitHub', href: 'https://github.com/Sitan-Yan', kind: 'github' },
    { label: 'Email', href: 'mailto:sitan@hust.edu.cn', kind: 'email' },
    { label: 'LinkedIn', href: 'https://www.linkedin.com/in/sitan-yan-ab3509434/', kind: 'linkedin' },
  ],
  education: [
    {
      degree: 'M.Eng.',
      major: 'Mechanical Engineering',
      institution: 'Huazhong University of Science and Technology (HUST)',
      start: 'Sep 2024',
      end: 'Jun 2027',
      location: 'Wuhan, China',
    },
    {
      degree: 'B.Eng.',
      major: 'Mechanical Design, Manufacturing and Automation',
      institution: 'Huazhong University of Science and Technology (HUST)',
      start: 'Sep 2020',
      end: 'Jun 2024',
      location: 'Wuhan, China',
    },
  ],
  awards: [
    { year: 2024, title: 'Distinguished Graduate of HUST' },
    { year: 2022, title: 'First Prize, Chinese Mathematics Competitions (CMC)' },
    { year: 2022, title: 'Third Prize, China Collegiate Intelligent Robotics Creative Competition' },
    { year: 2021, title: 'Academic Excellence Scholarship' },
  ],
  location: 'Wuhan, China',
};
```

- [ ] **Step 2: Create `src/data/interests.ts`**

```ts
import { Eye, ScanSearch, Route } from 'lucide-react';
import { type LucideIcon } from 'lucide-react';

export interface Interest {
  slug: string;
  title: string;
  description: string;
  icon: LucideIcon;
}

export const interests: Interest[] = [
  {
    slug: 'perception-control',
    title: 'Robotic Perception & Control',
    description:
      'Visual servoing, focus-based control, and sensorimotor coupling for precision tasks under limited field-of-view.',
    icon: Eye,
  },
  {
    slug: 'optical-metrology',
    title: 'Automated Optical Metrology',
    description:
      'Microscopic 3D reconstruction, shape-from-focus, and reliability under reflection and depth noise.',
    icon: ScanSearch,
  },
  {
    slug: 'motion-planning',
    title: 'Motion Planning & Optimization',
    description:
      'Time-optimal trajectory planning, model predictive control, and planning under partial observation.',
    icon: Route,
  },
];
```

- [ ] **Step 3: Create `src/data/projects.ts`**

```ts
export type ProjectStatus = 'completed' | 'ongoing' | 'in-preparation' | 'under-revision' | 'planned';

export interface ProjectMedia {
  images?: { src: string; alt: string; caption?: string }[];
  videos?: { src: string; poster?: string; caption?: string }[];
}

export interface Project {
  slug: string;
  number: string;
  title: string;
  dateRange: string;
  status: ProjectStatus;
  summary: string;
  featured?: boolean;
  problem: string;
  method: string;
  contribution: string[];
  result: string;
  technologies: string[];
  media?: ProjectMedia;
  links?: { label: string; href: string }[];
  comingSoon?: boolean;
}

export const projects: Project[] = [
  {
    slug: 'reliability-aware-sff',
    number: '01',
    title: 'Reliability-Aware Microscopic 3D Reconstruction for Reflective Micro-Holes',
    dateRange: 'Jun 2025 — Present',
    status: 'under-revision',
    summary:
      'Developed a reliability-aware shape-from-focus framework for accurate 3D measurement of reflective micro-holes, explicitly modeling reflection-induced depth uncertainty.',
    featured: true,
    problem:
      'Microscopic 3D reconstruction of reflective micro-holes fails under specular highlights, where conventional focus measures become unreliable and propagate error into the recovered surface.',
    method:
      'A reliability-aware shape-from-focus pipeline that couples a learned reliability model with focus-volume fusion, then uses a Gaussian-process-style surface fit to obtain dense, uncertainty-quantified depth maps.',
    contribution: [
      'Designed the overall pipeline from focus stack acquisition to dense surface reconstruction.',
      'Built the reliability model on top of Tenengrad focus measures and trained it on a labelled micro-hole dataset.',
      'Implemented the surface fitting and uncertainty propagation module.',
      'Wrote the manuscript and revision response for IEEE TIM.',
    ],
    result:
      'Reduced reconstruction error on a held-out reflective micro-hole set by 38% relative to baseline shape-from-focus, with sub-micron agreement against confocal ground truth on selected samples.',
    technologies: ['Python', 'PyTorch', 'OpenCV', 'NumPy', 'SciPy', 'LaTeX'],
    media: {
      images: [
        { src: '/Sitan-Yan.github.io/projects/reliability-aware-sff/sff-system.png', alt: 'Shape-from-focus imaging system' },
        { src: '/Sitan-Yan.github.io/projects/reliability-aware-sff/sff-system.webp', alt: 'Shape-from-focus imaging system (webp)' },
        { src: '/Sitan-Yan.github.io/projects/reliability-aware-sff/micro-hole.png', alt: 'Microscopic image of a micro-hole' },
      ],
      videos: [
        { src: '/Sitan-Yan.github.io/projects/reliability-aware-sff/micro-hole.mp4', caption: 'Focus stack sweep of a reflective micro-hole' },
      ],
    },
    links: [],
  },
  {
    slug: 'focus-constrained-vpc',
    number: '02',
    title: 'Focus-Constrained Visual Predictive Control for Robotic Micro-Hole Localization',
    dateRange: 'Jun 2026 — Aug 2026',
    status: 'in-preparation',
    summary:
      'Macro–micro visual control framework for autonomous micro-hole localization under limited field-of-view and focus constraints.',
    problem:
      'Localizing micro-holes autonomously under a microscope is hard: the camera sees only a small patch of the workpiece at high magnification, and motion is constrained by focus and limited depth of field.',
    method:
      'A two-timescale visual control stack: a 6-DOF arm performs a bounded spiral search using a Tenengrad autofocus signal, while a 1D linear stage adjusts focus. The lateral search runs at 10 Hz NMPC with a local task-frame formulation that decouples lateral search from axial focus.',
    contribution: [
      'Modeled the 6-DOF robot + 1D linear stage in an orthogonal local task frame.',
      'Integrated Tenengrad autofocus, bounded spiral search, and NMPC at 10 Hz.',
      'Tuned the search and focus controllers and ran the lab validation experiments.',
      'Drafted the manuscript for IEEE/ASME T-Mech.',
    ],
    result:
      '85% search success rate for initial lateral offsets within 1.0 mm; final localization error below 10 μm.',
    technologies: ['Python', 'C++', 'ROS 2', 'Gazebo', 'OpenCV', 'CasADi'],
    comingSoon: true,
  },
  {
    slug: 'dynamic-obstacle-avoidance',
    number: '03',
    title: 'Robotic Dynamic Obstacle Avoidance Control based on Partial RGB-D Observations',
    dateRange: 'Jan 2026 — Present',
    status: 'ongoing',
    summary:
      'Closed-loop robotic obstacle avoidance using partial RGB-D observations, learned obstacle representations, and MPC under occlusion and depth noise.',
    problem:
      'Avoiding dynamic obstacles with a moving manipulator is hard when only partial RGB-D observations are available — occlusions, depth noise, and unknown obstacle shape all conspire to break naive reactive planners.',
    method:
      'A perception-control stack that fuses RGB-D reconstruction with PCA/MVEE-initialised obstacle shapes, refined by a residual neural network under occlusion, and feeds spatial constraints into an MPC that also uses a Kalman-filter motion prediction.',
    contribution: [
      'Built the ROS 2 / Gazebo framework for RGB-D reconstruction and local point-cloud processing.',
      'Implemented PCA/MVEE initialization plus residual NN refinement of obstacle occupancy.',
      'Wired Kalman-filter motion prediction into the MPC spatial constraints.',
    ],
    result:
      'Ongoing — current results show robust obstacle avoidance in simulation for up to 4 dynamic obstacles, with real-robot validation in progress.',
    technologies: ['Python', 'PyTorch', 'ROS 2', 'Gazebo', 'Open3D', 'CasADi'],
    comingSoon: true,
  },
];
```

- [ ] **Step 4: Create `src/data/publications.ts`**

```ts
export type PublicationStatus = 'published' | 'under-revision' | 'in-preparation';

export interface Publication {
  year: number;
  title: string;
  authors: string[];
  venue: string;
  status: PublicationStatus;
  links?: { label: string; href: string }[];
  note?: string;
}

export const publications: Publication[] = [
  {
    year: 2026,
    title: 'Reliability-aware shape-from-focus for reflective 3-D micro-hole measurement',
    authors: ['S. Yan', 'W. Xu', 'L. Zeng', 'W. Li'],
    venue: 'IEEE Transactions on Instrumentation and Measurement',
    status: 'under-revision',
    note: 'Manuscript under major revision.',
  },
  {
    year: 2026,
    title: 'Focus-Constrained Visual Predictive Control for Robotic Micro-Hole Localization',
    authors: ['S. Yan et al.'],
    venue: 'IEEE/ASME Transactions on Mechatronics',
    status: 'in-preparation',
    note: 'Planned submission.',
  },
  {
    year: 2023,
    title: 'Preliminary Design and Prototype Development of an Air-ground Carrier Platform',
    authors: ['T. Chen', 'J. Han', 'J. Wang', 'S. Yan', 'C. Wan', 'F. Tian'],
    venue: '2023 International Conference on Unmanned Aircraft Systems (ICUAS), Warsaw, Poland, pp. 235–240',
    status: 'published',
    links: [{ label: 'IEEE Xplore', href: 'https://ieeexplore.ieee.org/abstract/document/10156435' }],
  },
];
```

- [ ] **Step 5: Create `src/data/skills.ts`**

```ts
export interface SkillColumn {
  category: string;
  items: string[];
}

export const skills: SkillColumn[] = [
  {
    category: 'Programming',
    items: ['Python', 'C++', 'MATLAB'],
  },
  {
    category: 'Frameworks & Libraries',
    items: ['OpenCV', 'PyTorch', 'ROS / ROS 2', 'NumPy', 'SciPy', 'CasADi'],
  },
  {
    category: 'Research Tools',
    items: ['Gazebo', 'RobotStudio', 'Git', 'LaTeX'],
  },
];
```

- [ ] **Step 6: Verify TypeScript compiles cleanly**

```powershell
Set-Location -LiteralPath "E:\Abroad_Application\Sitan-Yan.github.io"
npx tsc --noEmit
```

Expected: no errors. If errors appear, fix type mismatches before continuing.

- [ ] **Step 7: Commit**

```powershell
Set-Location -LiteralPath "E:\Abroad_Application\Sitan-Yan.github.io"
git add -A
git commit -m "feat: add typed data layer for site content"
```

---

## Task 8: Create the Container, Header, and Footer layout components

**Files:**
- Create: `E:\Abroad_Application\Sitan-Yan.github.io\src\components\layout\container.tsx`
- Create: `E:\Abroad_Application\Sitan-Yan.github.io\src\components\layout\header.tsx`
- Create: `E:\Abroad_Application\Sitan-Yan.github.io\src\components\layout\footer.tsx`
- Create: `E:\Abroad_Application\Sitan-Yan.github.io\src\components\layout\section-header.tsx`
- Modify: `E:\Abroad_Application\Sitan-Yan.github.io\src\App.tsx` (use new layout)

**Interfaces:**
- Consumes: ThemeToggle (from Task 6), Sheet (from Task 4), Separator (from Task 4)
- Produces: `<Container>`, `<SiteHeader>`, `<SiteFooter>`, `<SectionHeader>`

- [ ] **Step 1: Create `Container` component**

Create `src/components/layout/container.tsx`:

```tsx
import { type ReactNode } from 'react';
import { cn } from '@/lib/utils';

interface ContainerProps {
  children: ReactNode;
  className?: string;
  as?: 'div' | 'section' | 'article' | 'header' | 'footer';
}

export function Container({ children, className, as: As = 'div' }: ContainerProps) {
  return <As className={cn('mx-auto w-full max-w-content px-6 sm:px-8', className)}>{children}</As>;
}
```

- [ ] **Step 2: Create `SectionHeader` component**

Create `src/components/layout/section-header.tsx`:

```tsx
import { type ReactNode } from 'react';
import { cn } from '@/lib/utils';

interface SectionHeaderProps {
  eyebrow?: string;
  title: string;
  description?: ReactNode;
  className?: string;
}

export function SectionHeader({ eyebrow, title, description, className }: SectionHeaderProps) {
  return (
    <div className={cn('mb-10 max-w-3xl', className)}>
      {eyebrow && (
        <p className="mb-3 text-xs font-medium uppercase tracking-[0.08em] text-muted">{eyebrow}</p>
      )}
      <h2 className="text-balance text-3xl font-bold tracking-tight sm:text-4xl md:text-[2.5rem]">
        {title}
      </h2>
      {description && <p className="mt-3 text-balance text-base text-muted sm:text-lg">{description}</p>}
    </div>
  );
}
```

- [ ] **Step 3: Create `SiteHeader` component**

Create `src/components/layout/header.tsx`:

```tsx
import { Menu } from 'lucide-react';
import { useEffect, useState } from 'react';
import { Link, useLocation } from 'react-router-dom';
import { Button } from '@/components/ui/button';
import { Sheet, SheetContent, SheetTitle, SheetTrigger } from '@/components/ui/sheet';
import { ThemeToggle } from '@/components/layout/theme-toggle';
import { profile } from '@/data/profile';
import { cn } from '@/lib/utils';

const NAV_SECTIONS = [
  { id: 'about', label: 'About' },
  { id: 'interests', label: 'Research' },
  { id: 'projects', label: 'Projects' },
  { id: 'publications', label: 'Publications' },
  { id: 'skills', label: 'Skills' },
  { id: 'education', label: 'Education' },
  { id: 'contact', label: 'Contact' },
];

export function SiteHeader() {
  const [scrolled, setScrolled] = useState(false);
  const [open, setOpen] = useState(false);
  const { pathname } = useLocation();

  useEffect(() => {
    const onScroll = () => setScrolled(window.scrollY > 32);
    onScroll();
    window.addEventListener('scroll', onScroll, { passive: true });
    return () => window.removeEventListener('scroll', onScroll);
  }, []);

  const isHome = pathname === '/' || pathname === '/Sitan-Yan.github.io/' || pathname === '/Sitan-Yan.github.io';

  return (
    <header
      className={cn(
        'fixed inset-x-0 top-0 z-40 transition-all',
        scrolled
          ? 'border-b border-border bg-background/80 backdrop-blur-md'
          : 'border-b border-transparent bg-transparent',
      )}
    >
      <div className="mx-auto flex h-16 w-full max-w-content items-center justify-between px-6 sm:px-8">
        <Link
          to={isHome ? '#top' : '/'}
          className="text-base font-semibold tracking-tight"
          onClick={(e) => {
            if (isHome) {
              e.preventDefault();
              window.scrollTo({ top: 0, behavior: 'smooth' });
            }
          }}
        >
          {profile.name}
        </Link>

        <nav className="hidden items-center gap-1 md:flex" aria-label="Main navigation">
          {NAV_SECTIONS.map((item) => (
            <a
              key={item.id}
              href={isHome ? `#${item.id}` : `/#${item.id}`}
              className="rounded-md px-3 py-1.5 text-sm text-muted transition-colors hover:bg-card hover:text-foreground"
            >
              {item.label}
            </a>
          ))}
        </nav>

        <div className="flex items-center gap-2">
          <ThemeToggle />
          <Sheet open={open} onOpenChange={setOpen}>
            <SheetTrigger asChild>
              <Button variant="ghost" size="icon" className="md:hidden" aria-label="Open menu">
                <Menu className="h-5 w-5" />
              </Button>
            </SheetTrigger>
            <SheetContent>
              <SheetTitle className="mb-6">Menu</SheetTitle>
              <nav className="flex flex-col gap-2" aria-label="Mobile navigation">
                {NAV_SECTIONS.map((item) => (
                  <a
                    key={item.id}
                    href={isHome ? `#${item.id}` : `/#${item.id}`}
                    onClick={() => setOpen(false)}
                    className="rounded-md px-3 py-2 text-base text-foreground hover:bg-card"
                  >
                    {item.label}
                  </a>
                ))}
              </nav>
            </SheetContent>
          </Sheet>
        </div>
      </div>
    </header>
  );
}
```

- [ ] **Step 4: Create `SiteFooter` component**

Create `src/components/layout/footer.tsx`:

```tsx
import { profile } from '@/data/profile';

export function SiteFooter() {
  const year = new Date().getFullYear();
  return (
    <footer className="border-t border-border py-10 text-sm text-muted">
      <div className="mx-auto flex w-full max-w-content flex-col items-center justify-between gap-4 px-6 sm:flex-row sm:px-8">
        <p>© {year} {profile.name}</p>
        <p>Last updated {new Date().toLocaleString('en-US', { month: 'long', year: 'numeric' })}</p>
      </div>
    </footer>
  );
}
```

- [ ] **Step 5: Wire the layout into `App.tsx`**

Replace `src/App.tsx` with:

```tsx
import { SiteFooter } from '@/components/layout/footer';
import { SiteHeader } from '@/components/layout/header';
import { ThemeProvider } from '@/components/layout/theme-provider';
import { profile } from '@/data/profile';

function App() {
  return (
    <ThemeProvider>
      <div id="top" className="min-h-screen bg-background text-foreground">
        <SiteHeader />
        <main className="pt-16">
          <section className="mx-auto max-w-content px-6 py-20 sm:px-8">
            <h1 className="text-4xl font-bold">{profile.name}</h1>
            <p className="mt-2 text-muted">{profile.role}</p>
          </section>
        </main>
        <SiteFooter />
      </div>
    </ThemeProvider>
  );
}

export default App;
```

- [ ] **Step 6: Verify header, footer, and theme toggle render correctly**

```powershell
Set-Location -LiteralPath "E:\Abroad_Application\Sitan-Yan.github.io"
npm run dev
```

Open `http://localhost:5173/Sitan-Yan.github.io/`. Expected: sticky transparent header with "Sitan Yan" left, nav links center, theme toggle + mobile menu right; main content below; footer at bottom. Scroll down — header gains a border and backdrop-blur. Resize to <768px — nav becomes a hamburger menu. Toggle theme — colors swap. Stop server.

- [ ] **Step 7: Commit**

```powershell
Set-Location -LiteralPath "E:\Abroad_Application\Sitan-Yan.github.io"
git add -A
git commit -m "feat: add header, footer, container, section-header"
```

---

## Task 9: Build the Hero section

**Files:**
- Create: `E:\Abroad_Application\Sitan-Yan.github.io\src\components\sections\hero.tsx`
- Modify: `E:\Abroad_Application\Sitan-Yan.github.io\src\App.tsx` (use `<Hero />`)

- [ ] **Step 1: Create the `Hero` component**

Create `src/components/sections/hero.tsx`:

```tsx
import { ArrowUpRight, FileText, Github, Linkedin, Mail } from 'lucide-react';
import { BlurFade } from '@/components/magicui/blur-fade';
import { GridBackground } from '@/components/magicui/grid-background';
import { Button } from '@/components/ui/button';
import { profile } from '@/data/profile';
import { cn } from '@/lib/utils';

const ICON_BY_KIND: Record<string, typeof FileText> = {
  cv: FileText,
  github: Github,
  linkedin: Linkedin,
  email: Mail,
};

export function Hero() {
  return (
    <section id="about" aria-label="Hero">
      <GridBackground className="relative">
        <div className="mx-auto grid w-full max-w-content gap-12 px-6 pb-24 pt-20 sm:px-8 md:grid-cols-[1fr_auto] md:items-center md:pb-32 md:pt-28">
          <div className="flex flex-col gap-6">
            <BlurFade inView>
              <p className="text-sm font-medium uppercase tracking-[0.08em] text-muted">
                {profile.role} · {profile.institutionShort}
              </p>
            </BlurFade>

            <BlurFade delay={0.05} inView>
              <h1 className="text-balance text-5xl font-bold leading-[1.05] tracking-[-0.03em] sm:text-6xl md:text-[5rem]">
                {profile.name}
              </h1>
            </BlurFade>

            <BlurFade delay={0.1} inView>
              <p className="max-w-2xl text-balance text-lg text-muted sm:text-xl">
                {profile.oneLiner}
              </p>
            </BlurFade>

            <BlurFade delay={0.15} inView>
              <p className="max-w-2xl text-sm text-muted-foreground">
                {profile.role} at{' '}
                <span className="text-foreground">{profile.institution}</span>, advised by{' '}
                <a
                  href={profile.advisor.href}
                  target="_blank"
                  rel="noopener noreferrer"
                  className="text-foreground underline-offset-4 hover:underline"
                >
                  {profile.advisor.name}
                </a>{' '}
                at the {profile.lab}.
              </p>
            </BlurFade>

            <BlurFade delay={0.2} inView>
              <div className="mt-2 flex flex-wrap items-center gap-3">
                {profile.links.map((link) => {
                  const Icon = ICON_BY_KIND[link.kind] ?? ArrowUpRight;
                  return (
                    <Button key={link.label} asChild variant={link.kind === 'cv' ? 'default' : 'ghost'} size="default">
                      <a
                        href={link.href}
                        target={link.href.startsWith('http') ? '_blank' : undefined}
                        rel={link.href.startsWith('http') ? 'noopener noreferrer' : undefined}
                      >
                        <Icon className="mr-2 h-4 w-4" />
                        {link.label}
                      </a>
                    </Button>
                  );
                })}
              </div>
            </BlurFade>

            <BlurFade delay={0.25} inView>
              <p className="mt-2 inline-flex items-center gap-2 text-sm font-medium text-success">
                <span className="relative flex h-2 w-2">
                  <span className="absolute inline-flex h-full w-full animate-ping rounded-full bg-success opacity-60" />
                  <span className="relative inline-flex h-2 w-2 rounded-full bg-success" />
                </span>
                {profile.statusLine}
              </p>
            </BlurFade>
          </div>

          <BlurFade delay={0.2} inView className="hidden md:block">
            <div
              className={cn(
                'relative flex h-44 w-44 items-center justify-center rounded-2xl',
                'border border-border bg-card text-foreground shadow-sm',
                'bg-[linear-gradient(135deg,rgba(30,64,175,0.08),rgba(124,58,237,0.08))]',
                'dark:bg-[linear-gradient(135deg,rgba(96,165,250,0.12),rgba(167,139,250,0.12))]',
              )}
              aria-label="Profile monogram"
            >
              <span className="text-6xl font-semibold tracking-tight">{profile.initials}</span>
            </div>
          </BlurFade>
        </div>
      </GridBackground>
    </section>
  );
}
```

- [ ] **Step 2: Wire `<Hero />` into `App.tsx`**

Replace `src/App.tsx` with:

```tsx
import { SiteFooter } from '@/components/layout/footer';
import { SiteHeader } from '@/components/layout/header';
import { ThemeProvider } from '@/components/layout/theme-provider';
import { Hero } from '@/components/sections/hero';

function App() {
  return (
    <ThemeProvider>
      <div id="top" className="min-h-screen bg-background text-foreground">
        <SiteHeader />
        <main>
          <Hero />
        </main>
        <SiteFooter />
      </div>
    </ThemeProvider>
  );
}

export default App;
```

- [ ] **Step 3: Verify Hero renders with all elements**

```powershell
Set-Location -LiteralPath "E:\Abroad_Application\Sitan-Yan.github.io"
npm run dev
```

Open `http://localhost:5173/Sitan-Yan.github.io/`. Expected: subtle grid background, large "Sitan Yan" h1, role + institution + advisor line, 4 CTA buttons, green status pulse with "Open to PhD opportunities starting Fall 2027.", SY monogram card on the right. Scroll — elements fade in on enter. Stop server.

- [ ] **Step 4: Commit**

```powershell
Set-Location -LiteralPath "E:\Abroad_Application\Sitan-Yan.github.io"
git add -A
git commit -m "feat: add Hero section"
```

---

## Task 10: Build the About, Research Interests, and Education sections

**Files:**
- Create: `E:\Abroad_Application\Sitan-Yan.github.io\src\components\sections\about.tsx`
- Create: `E:\Abroad_Application\Sitan-Yan.github.io\src\components\sections\research-interests.tsx`
- Create: `E:\Abroad_Application\Sitan-Yan.github.io\src\components\sections\education.tsx`

- [ ] **Step 1: Create the `About` component**

Create `src/components/sections/about.tsx`:

```tsx
import { BlurFade } from '@/components/magicui/blur-fade';
import { Container } from '@/components/layout/container';
import { SectionHeader } from '@/components/layout/section-header';
import { profile } from '@/data/profile';

export function About() {
  return (
    <section id="about-bio" aria-label="About" className="py-20 md:py-24">
      <Container>
        <BlurFade inView>
          <SectionHeader
            eyebrow="About me"
            title="Background"
            description="A short version of who I am and what I'm looking for."
          />
        </BlurFade>
        <BlurFade inView delay={0.05}>
          <p className="max-w-3xl text-balance text-base leading-relaxed text-foreground/90 sm:text-lg">
            {profile.about}
          </p>
        </BlurFade>
      </Container>
    </section>
  );
}
```

- [ ] **Step 2: Create the `ResearchInterests` component**

Create `src/components/sections/research-interests.tsx`:

```tsx
import { BlurFade } from '@/components/magicui/blur-fade';
import { Container } from '@/components/layout/container';
import { SectionHeader } from '@/components/layout/section-header';
import { Card, CardContent, CardDescription, CardHeader, CardTitle } from '@/components/ui/card';
import { interests } from '@/data/interests';

export function ResearchInterests() {
  return (
    <section id="interests" aria-label="Research interests" className="py-20 md:py-24">
      <Container>
        <BlurFade inView>
          <SectionHeader
            eyebrow="Research directions"
            title="Research Interests"
            description="Three threads I keep coming back to, with adjacent problems I'm happy to detour into."
          />
        </BlurFade>
        <div className="grid gap-5 md:grid-cols-3">
          {interests.map((interest, i) => {
            const Icon = interest.icon;
            return (
              <BlurFade key={interest.slug} inView delay={0.05 + i * 0.05}>
                <Card className="h-full transition-colors hover:border-accent/40">
                  <CardHeader>
                    <div className="mb-2 flex h-10 w-10 items-center justify-center rounded-lg bg-accent-muted text-accent">
                      <Icon className="h-5 w-5" aria-hidden />
                    </div>
                    <CardTitle>{interest.title}</CardTitle>
                  </CardHeader>
                  <CardContent>
                    <CardDescription className="text-sm leading-relaxed">{interest.description}</CardDescription>
                  </CardContent>
                </Card>
              </BlurFade>
            );
          })}
        </div>
      </Container>
    </section>
  );
}
```

- [ ] **Step 3: Create the `Education` component**

Create `src/components/sections/education.tsx`:

```tsx
import { GraduationCap } from 'lucide-react';
import { BlurFade } from '@/components/magicui/blur-fade';
import { Container } from '@/components/layout/container';
import { SectionHeader } from '@/components/layout/section-header';
import { Separator } from '@/components/ui/separator';
import { profile } from '@/data/profile';

export function Education() {
  return (
    <section id="education" aria-label="Education" className="py-20 md:py-24">
      <Container>
        <BlurFade inView>
          <SectionHeader
            eyebrow="Academic background"
            title="Education"
            description="Where I trained, and what I focused on at each stage."
          />
        </BlurFade>
        <div className="relative">
          <ol className="relative space-y-8 border-l border-border pl-6">
            {profile.education.map((entry, i) => (
              <BlurFade key={`${entry.degree}-${entry.start}`} inView delay={0.05 + i * 0.05}>
                <li className="relative">
                  <span className="absolute -left-[31px] top-1 flex h-6 w-6 items-center justify-center rounded-full border border-border bg-background">
                    <GraduationCap className="h-3 w-3 text-accent" />
                  </span>
                  <p className="font-mono text-xs uppercase tracking-wider text-muted">{entry.start} — {entry.end}</p>
                  <h3 className="mt-1 text-lg font-semibold tracking-tight">{entry.degree} · {entry.major}</h3>
                  <p className="text-sm text-foreground/80">{entry.institution}</p>
                  <p className="text-sm text-muted">{entry.location}</p>
                </li>
              </BlurFade>
            ))}
          </ol>
          <Separator className="mt-12" />
        </div>
      </Container>
    </section>
  );
}
```

- [ ] **Step 4: Wire new sections into `App.tsx`**

Replace `src/App.tsx` with:

```tsx
import { SiteFooter } from '@/components/layout/footer';
import { SiteHeader } from '@/components/layout/header';
import { ThemeProvider } from '@/components/layout/theme-provider';
import { About } from '@/components/sections/about';
import { Education } from '@/components/sections/education';
import { Hero } from '@/components/sections/hero';
import { ResearchInterests } from '@/components/sections/research-interests';

function App() {
  return (
    <ThemeProvider>
      <div id="top" className="min-h-screen bg-background text-foreground">
        <SiteHeader />
        <main>
          <Hero />
          <About />
          <ResearchInterests />
          <Education />
        </main>
        <SiteFooter />
      </div>
    </ThemeProvider>
  );
}

export default App;
```

- [ ] **Step 5: Verify the three sections render with proper layout**

```powershell
Set-Location -LiteralPath "E:\Abroad_Application\Sitan-Yan.github.io"
npm run dev
```

Open `http://localhost:5173/Sitan-Yan.github.io/`. Expected: Hero → About paragraph → 3 interest cards in a row (collapse to 1 column <768px) → Education timeline with circular dots. All sections fade in on scroll. Stop server.

- [ ] **Step 6: Commit**

```powershell
Set-Location -LiteralPath "E:\Abroad_Application\Sitan-Yan.github.io"
git add -A
git commit -m "feat: add About, Research Interests, Education sections"
```

---

## Task 11: Build the Research Projects section

**Files:**
- Create: `E:\Abroad_Application\Sitan-Yan.github.io\src\components\sections\research-projects.tsx`

**Interfaces:**
- Consumes: `projects` from `@/data/projects`, `Badge` variants for status

- [ ] **Step 1: Create the `ResearchProjects` component**

Create `src/components/sections/research-projects.tsx`:

```tsx
import { ArrowRight } from 'lucide-react';
import { Link } from 'react-router-dom';
import { BlurFade } from '@/components/magicui/blur-fade';
import { Container } from '@/components/layout/container';
import { SectionHeader } from '@/components/layout/section-header';
import { Badge, type BadgeProps } from '@/components/ui/badge';
import { projects } from '@/data/projects';
import type { ProjectStatus } from '@/data/projects';
import { cn } from '@/lib/utils';

const STATUS_BADGE: Record<ProjectStatus, { label: string; variant: BadgeProps['variant'] }> = {
  completed: { label: 'Completed', variant: 'success' },
  ongoing: { label: 'Ongoing', variant: 'info' },
  'in-preparation': { label: 'In Preparation', variant: 'warning' },
  'under-revision': { label: 'Under Revision', variant: 'purple' },
  planned: { label: 'Planned', variant: 'outline' },
};

export function ResearchProjects() {
  return (
    <section id="projects" aria-label="Selected research projects" className="py-20 md:py-24">
      <Container>
        <BlurFade inView>
          <SectionHeader
            eyebrow="Selected work"
            title="Research Projects"
            description="Three projects where I drove the core technical contribution. Each links to a full write-up."
          />
        </BlurFade>

        <div className="space-y-6">
          {projects.map((project, i) => {
            const status = STATUS_BADGE[project.status];
            return (
              <BlurFade key={project.slug} inView delay={0.05 + i * 0.05}>
                <article
                  className={cn(
                    'group relative grid gap-6 rounded-xl border border-border p-6 sm:p-8 md:grid-cols-[auto_1fr] md:gap-8',
                    project.featured &&
                      'bg-[linear-gradient(135deg,rgba(30,64,175,0.04),rgba(124,58,237,0.04))] dark:bg-[linear-gradient(135deg,rgba(96,165,250,0.06),rgba(167,139,250,0.06))]',
                  )}
                >
                  <div className="flex flex-col items-start gap-3 md:items-center">
                    <span className="font-mono text-xs font-medium text-muted">{project.number}</span>
                    <Badge variant={status.variant}>{status.label}</Badge>
                    <span className="font-mono text-xs text-muted-foreground">{project.dateRange}</span>
                  </div>

                  <div className="space-y-5">
                    <div>
                      <h3 className="text-balance text-xl font-semibold tracking-tight sm:text-2xl">
                        {project.title}
                      </h3>
                      <p className="mt-2 text-sm text-muted sm:text-base">{project.summary}</p>
                    </div>

                    <dl className="grid gap-4 sm:grid-cols-2">
                      <div>
                        <dt className="font-mono text-xs uppercase tracking-wider text-muted">Problem</dt>
                        <dd className="mt-1 text-sm leading-relaxed text-foreground/90">{project.problem}</dd>
                      </div>
                      <div>
                        <dt className="font-mono text-xs uppercase tracking-wider text-muted">Method</dt>
                        <dd className="mt-1 text-sm leading-relaxed text-foreground/90">{project.method}</dd>
                      </div>
                      <div>
                        <dt className="font-mono text-xs uppercase tracking-wider text-muted">My contribution</dt>
                        <dd className="mt-1 text-sm leading-relaxed text-foreground/90">
                          <ul className="list-disc space-y-1 pl-4">
                            {project.contribution.map((c, j) => (
                              <li key={j}>{c}</li>
                            ))}
                          </ul>
                        </dd>
                      </div>
                      <div>
                        <dt className="font-mono text-xs uppercase tracking-wider text-muted">Result</dt>
                        <dd className="mt-1 text-sm leading-relaxed text-foreground/90">{project.result}</dd>
                      </div>
                    </dl>

                    <div className="flex flex-wrap items-center gap-2">
                      <span className="font-mono text-xs uppercase tracking-wider text-muted">Technologies</span>
                      {project.technologies.map((tech) => (
                        <Badge key={tech} variant="secondary">
                          {tech}
                        </Badge>
                      ))}
                    </div>

                    {!project.comingSoon ? (
                      <Link
                        to={`/projects/${project.slug}`}
                        className="inline-flex items-center gap-1.5 text-sm font-medium text-accent transition-colors hover:text-accent/80"
                      >
                        View project
                        <ArrowRight className="h-4 w-4 transition-transform group-hover:translate-x-0.5" />
                      </Link>
                    ) : (
                      <span className="inline-flex items-center gap-1.5 text-sm text-muted">
                        Full write-up coming soon
                      </span>
                    )}
                  </div>
                </article>
              </BlurFade>
            );
          })}
        </div>
      </Container>
    </section>
  );
}
```

- [ ] **Step 2: Wire the section into `App.tsx`**

Replace `src/App.tsx` with:

```tsx
import { SiteFooter } from '@/components/layout/footer';
import { SiteHeader } from '@/components/layout/header';
import { ThemeProvider } from '@/components/layout/theme-provider';
import { About } from '@/components/sections/about';
import { Education } from '@/components/sections/education';
import { Hero } from '@/components/sections/hero';
import { ResearchInterests } from '@/components/sections/research-interests';
import { ResearchProjects } from '@/components/sections/research-projects';

function App() {
  return (
    <ThemeProvider>
      <div id="top" className="min-h-screen bg-background text-foreground">
        <SiteHeader />
        <main>
          <Hero />
          <About />
          <ResearchInterests />
          <ResearchProjects />
          <Education />
        </main>
        <SiteFooter />
      </div>
    </ThemeProvider>
  );
}

export default App;
```

- [ ] **Step 3: Verify the project cards render with status badges and structured fields**

```powershell
Set-Location -LiteralPath "E:\Abroad_Application\Sitan-Yan.github.io"
npm run dev
```

Open `http://localhost:5173/Sitan-Yan.github.io/`. Expected: 3 project cards, each with number + status badge + date range, title + summary, 4 labeled fields (Problem/Method/My Contribution/Result), tech pills, and either "View project →" or "Full write-up coming soon". First card has a subtle gradient bg. Stop server.

- [ ] **Step 4: Commit**

```powershell
Set-Location -LiteralPath "E:\Abroad_Application\Sitan-Yan.github.io"
git add -A
git commit -m "feat: add Research Projects section"
```

---

## Task 12: Build the Publications, Skills, and Contact sections

**Files:**
- Create: `E:\Abroad_Application\Sitan-Yan.github.io\src\components\sections\publications.tsx`
- Create: `E:\Abroad_Application\Sitan-Yan.github.io\src\components\sections\skills.tsx`
- Create: `E:\Abroad_Application\Sitan-Yan.github.io\src\components\sections\contact.tsx`

- [ ] **Step 1: Create the `Publications` component**

Create `src/components/sections/publications.tsx`:

```tsx
import { ExternalLink, FlaskConical } from 'lucide-react';
import { BlurFade } from '@/components/magicui/blur-fade';
import { Container } from '@/components/layout/container';
import { SectionHeader } from '@/components/layout/section-header';
import { Badge, type BadgeProps } from '@/components/ui/badge';
import { Card, CardContent } from '@/components/ui/card';
import { Separator } from '@/components/ui/separator';
import { profile } from '@/data/profile';
import { publications } from '@/data/publications';
import type { PublicationStatus } from '@/data/publications';

const STATUS_BADGE: Record<PublicationStatus, { label: string; variant: BadgeProps['variant'] }> = {
  published: { label: 'Published', variant: 'success' },
  'under-revision': { label: 'Under Revision', variant: 'purple' },
  'in-preparation': { label: 'In Preparation', variant: 'warning' },
};

const groupedByYear = (entries = publications) => {
  const map = new Map<number, typeof publications>();
  for (const p of entries) {
    if (!map.has(p.year)) map.set(p.year, []);
    map.get(p.year)!.push(p);
  }
  return Array.from(map.entries()).sort(([a], [b]) => b - a);
};

export function Publications() {
  const grouped = groupedByYear();

  return (
    <section id="publications" aria-label="Publications and research experience" className="py-20 md:py-24">
      <Container>
        <BlurFade inView>
          <SectionHeader
            eyebrow="Research outputs"
            title="Publications & Research Experience"
            description="Peer-reviewed work, manuscripts in progress, and the lab I work in."
          />
        </BlurFade>

        <div className="space-y-12">
          <div>
            <h3 className="mb-6 font-mono text-xs uppercase tracking-[0.08em] text-muted">Publications</h3>
            <div className="space-y-8">
              {grouped.map(([year, entries]) => (
                <div key={year}>
                  <p className="mb-3 font-mono text-sm font-semibold text-foreground">{year}</p>
                  <div className="space-y-4">
                    {entries.map((pub, i) => {
                      const status = STATUS_BADGE[pub.status];
                      return (
                        <BlurFade key={`${year}-${i}`} inView delay={0.05 + i * 0.04}>
                          <Card>
                            <CardContent className="space-y-3 p-6">
                              <div className="flex flex-wrap items-start justify-between gap-3">
                                <h4 className="text-balance text-base font-semibold leading-snug">
                                  {pub.title}
                                </h4>
                                <Badge variant={status.variant}>{status.label}</Badge>
                              </div>
                              <p className="text-sm text-foreground/80">
                                {pub.authors.map((author, j) => (
                                  <span key={j} className={author.startsWith('S. Yan') ? 'font-semibold text-foreground' : ''}>
                                    {author}
                                    {j < pub.authors.length - 1 ? ', ' : ''}
                                  </span>
                                ))}
                              </p>
                              <p className="text-sm italic text-muted">{pub.venue}</p>
                              {pub.note && <p className="text-sm text-muted-foreground">{pub.note}</p>}
                              {pub.links && pub.links.length > 0 && (
                                <div className="flex flex-wrap gap-2 pt-1">
                                  {pub.links.map((link) => (
                                    <a
                                      key={link.href}
                                      href={link.href}
                                      target="_blank"
                                      rel="noopener noreferrer"
                                      className="inline-flex items-center gap-1.5 text-sm font-medium text-accent hover:underline"
                                    >
                                      {link.label}
                                      <ExternalLink className="h-3.5 w-3.5" />
                                    </a>
                                  ))}
                                </div>
                              )}
                            </CardContent>
                          </Card>
                        </BlurFade>
                      );
                    })}
                  </div>
                  <Separator className="mt-8" />
                </div>
              ))}
            </div>
          </div>

          <div>
            <h3 className="mb-6 font-mono text-xs uppercase tracking-[0.08em] text-muted">Research Experience</h3>
            <BlurFade inView>
              <Card>
                <CardContent className="flex flex-col gap-3 p-6 sm:flex-row sm:items-start sm:gap-5">
                  <div className="flex h-10 w-10 shrink-0 items-center justify-center rounded-lg bg-accent-muted text-accent">
                    <FlaskConical className="h-5 w-5" aria-hidden />
                  </div>
                  <div className="space-y-1">
                    <p className="font-mono text-xs text-muted">{profile.researchExperience.dates}</p>
                    <h4 className="text-base font-semibold">
                      {profile.researchExperience.lab}
                    </h4>
                    <p className="text-sm text-muted-foreground">Advised by {profile.researchExperience.advisor}</p>
                    <p className="pt-1 text-sm text-foreground/90">{profile.researchExperience.focus}</p>
                  </div>
                </CardContent>
              </Card>
            </BlurFade>
          </div>
        </div>
      </Container>
    </section>
  );
}
```

- [ ] **Step 2: Create the `Skills` component**

Create `src/components/sections/skills.tsx`:

```tsx
import { BlurFade } from '@/components/magicui/blur-fade';
import { Container } from '@/components/layout/container';
import { SectionHeader } from '@/components/layout/section-header';
import { Badge } from '@/components/ui/badge';
import { Card, CardContent, CardHeader, CardTitle } from '@/components/ui/card';
import { skills } from '@/data/skills';

export function Skills() {
  return (
    <section id="skills" aria-label="Skills" className="py-20 md:py-24">
      <Container>
        <BlurFade inView>
          <SectionHeader
            eyebrow="Toolbox"
            title="Skills"
            description="Languages, frameworks, and research tools I work with day to day."
          />
        </BlurFade>
        <div className="grid gap-5 md:grid-cols-3">
          {skills.map((col, i) => (
            <BlurFade key={col.category} inView delay={0.05 + i * 0.05}>
              <Card className="h-full">
                <CardHeader>
                  <CardTitle className="font-mono text-sm uppercase tracking-wider text-muted">
                    {col.category}
                  </CardTitle>
                </CardHeader>
                <CardContent>
                  <div className="flex flex-wrap gap-2">
                    {col.items.map((item) => (
                      <Badge key={item} variant="secondary">
                        {item}
                      </Badge>
                    ))}
                  </div>
                </CardContent>
              </Card>
            </BlurFade>
          ))}
        </div>
      </Container>
    </section>
  );
}
```

- [ ] **Step 3: Create the `Contact` component**

Create `src/components/sections/contact.tsx`:

```tsx
import { ArrowUpRight, FileText, Github, Linkedin, Mail, MapPin } from 'lucide-react';
import { BlurFade } from '@/components/magicui/blur-fade';
import { Container } from '@/components/layout/container';
import { SectionHeader } from '@/components/layout/section-header';
import { Card, CardContent } from '@/components/ui/card';
import { profile } from '@/data/profile';
import type { ProfileLink } from '@/data/profile';

const ICON_BY_KIND: Record<ProfileLink['kind'], typeof Mail> = {
  email: Mail,
  github: Github,
  linkedin: Linkedin,
  cv: FileText,
  scholar: ArrowUpRight,
};

export function Contact() {
  return (
    <section id="contact" aria-label="Contact" className="py-20 md:py-24">
      <Container>
        <BlurFade inView>
          <SectionHeader
            eyebrow="Get in touch"
            title="Contact"
            description="Best way to reach me, plus where I'm based."
          />
        </BlurFade>

        <BlurFade inView delay={0.05}>
          <Card>
            <CardContent className="space-y-6 p-6 sm:p-8">
              <div className="flex items-center gap-2 text-sm text-muted">
                <MapPin className="h-4 w-4" aria-hidden />
                {profile.location}
              </div>

              <div className="flex flex-wrap gap-3">
                {profile.links.map((link) => {
                  const Icon = ICON_BY_KIND[link.kind];
                  return (
                    <a
                      key={link.label}
                      href={link.href}
                      target={link.href.startsWith('http') ? '_blank' : undefined}
                      rel={link.href.startsWith('http') ? 'noopener noreferrer' : undefined}
                      className="inline-flex items-center gap-2 rounded-lg border border-border bg-card px-4 py-2.5 text-sm font-medium text-foreground transition-colors hover:border-accent/40 hover:text-accent"
                    >
                      <Icon className="h-4 w-4" aria-hidden />
                      {link.label}
                    </a>
                  );
                })}
              </div>

              <p className="text-sm text-muted">
                {profile.statusLine} — happy to hear from potential advisors, collaborators, or peer researchers.
              </p>
            </CardContent>
          </Card>
        </BlurFade>
      </Container>
    </section>
  );
}
```

- [ ] **Step 4: Wire new sections into `App.tsx`**

Replace `src/App.tsx` with:

```tsx
import { SiteFooter } from '@/components/layout/footer';
import { SiteHeader } from '@/components/layout/header';
import { ThemeProvider } from '@/components/layout/theme-provider';
import { About } from '@/components/sections/about';
import { Contact } from '@/components/sections/contact';
import { Education } from '@/components/sections/education';
import { Hero } from '@/components/sections/hero';
import { Publications } from '@/components/sections/publications';
import { ResearchInterests } from '@/components/sections/research-interests';
import { ResearchProjects } from '@/components/sections/research-projects';
import { Skills } from '@/components/sections/skills';

function App() {
  return (
    <ThemeProvider>
      <div id="top" className="min-h-screen bg-background text-foreground">
        <SiteHeader />
        <main>
          <Hero />
          <About />
          <ResearchInterests />
          <ResearchProjects />
          <Publications />
          <Skills />
          <Education />
          <Contact />
        </main>
        <SiteFooter />
      </div>
    </ThemeProvider>
  );
}

export default App;
```

- [ ] **Step 5: Verify the three sections render correctly**

```powershell
Set-Location -LiteralPath "E:\Abroad_Application\Sitan-Yan.github.io"
npm run dev
```

Open `http://localhost:5173/Sitan-Yan.github.io/`. Expected: Publications section shows year groups with status pills, italic venues, self-name bolded; Research Experience card with lab icon; Skills section with 3 columns of pills; Contact section with icon buttons and "open to" line. Stop server.

- [ ] **Step 6: Commit**

```powershell
Set-Location -LiteralPath "E:\Abroad_Application\Sitan-Yan.github.io"
git add -A
git commit -m "feat: add Publications, Skills, Contact sections"
```

---

## Task 13: Build the Awards section (last)

**Files:**
- Create: `E:\Abroad_Application\Sitan-Yan.github.io\src\components\sections\awards.tsx`

- [ ] **Step 1: Create the `Awards` component**

Create `src/components/sections/awards.tsx`:

```tsx
import { Award } from 'lucide-react';
import { BlurFade } from '@/components/magicui/blur-fade';
import { Container } from '@/components/layout/container';
import { SectionHeader } from '@/components/layout/section-header';
import { profile } from '@/data/profile';

export function Awards() {
  return (
    <section id="awards" aria-label="Honors and awards" className="py-20 md:py-24">
      <Container>
        <BlurFade inView>
          <SectionHeader
            eyebrow="Selected honors"
            title="Honors & Awards"
            description="Recognition I've been lucky to receive."
          />
        </BlurFade>

        <ul className="space-y-3">
          {profile.awards.map((award, i) => (
            <BlurFade key={`${award.year}-${i}`} inView delay={0.05 + i * 0.04}>
              <li className="flex items-center gap-4 rounded-lg border border-border bg-card px-4 py-3 sm:px-5 sm:py-4">
                <div className="flex h-8 w-8 shrink-0 items-center justify-center rounded-full bg-accent-muted text-accent">
                  <Award className="h-4 w-4" aria-hidden />
                </div>
                <span className="font-mono text-sm font-semibold text-foreground">{award.year}</span>
                <span className="text-sm text-foreground/90 sm:text-base">{award.title}</span>
              </li>
            </BlurFade>
          ))}
        </ul>
      </Container>
    </section>
  );
}
```

- [ ] **Step 2: Wire the section into `App.tsx` (after Contact)**

Replace `src/App.tsx` with:

```tsx
import { SiteFooter } from '@/components/layout/footer';
import { SiteHeader } from '@/components/layout/header';
import { ThemeProvider } from '@/components/layout/theme-provider';
import { About } from '@/components/sections/about';
import { Awards } from '@/components/sections/awards';
import { Contact } from '@/components/sections/contact';
import { Education } from '@/components/sections/education';
import { Hero } from '@/components/sections/hero';
import { Publications } from '@/components/sections/publications';
import { ResearchInterests } from '@/components/sections/research-interests';
import { ResearchProjects } from '@/components/sections/research-projects';
import { Skills } from '@/components/sections/skills';

function App() {
  return (
    <ThemeProvider>
      <div id="top" className="min-h-screen bg-background text-foreground">
        <SiteHeader />
        <main>
          <Hero />
          <About />
          <ResearchInterests />
          <ResearchProjects />
          <Publications />
          <Skills />
          <Education />
          <Contact />
          <Awards />
        </main>
        <SiteFooter />
      </div>
    </ThemeProvider>
  );
}

export default App;
```

- [ ] **Step 3: Verify the Awards section renders at the bottom**

```powershell
Set-Location -LiteralPath "E:\Abroad_Application\Sitan-Yan.github.io"
npm run dev
```

Open `http://localhost:5173/Sitan-Yan.github.io/`. Scroll to bottom. Expected: Awards section is the last content section, with 4 award items in pill-style rows. Stop server.

- [ ] **Step 4: Commit**

```powershell
Set-Location -LiteralPath "E:\Abroad_Application\Sitan-Yan.github.io"
git add -A
git commit -m "feat: add Awards section (last)"
```

---

## Task 14: Wire React Router and create the Home page wrapper

**Files:**
- Create: `E:\Abroad_Application\Sitan-Yan.github.io\src\pages\home.tsx`
- Create: `E:\Abroad_Application\Sitan-Yan.github.io\src\pages\not-found.tsx`
- Create: `E:\Abroad_Application\Sitan-Yan.github.io\src\pages\project-page.tsx` (placeholder for now, filled in Task 16)
- Modify: `E:\Abroad_Application\Sitan-Yan.github.io\src\App.tsx` (add router)

- [ ] **Step 1: Create the Home page wrapper**

Create `src/pages/home.tsx`:

```tsx
import { About } from '@/components/sections/about';
import { Awards } from '@/components/sections/awards';
import { Contact } from '@/components/sections/contact';
import { Education } from '@/components/sections/education';
import { Hero } from '@/components/sections/hero';
import { Publications } from '@/components/sections/publications';
import { ResearchInterests } from '@/components/sections/research-interests';
import { ResearchProjects } from '@/components/sections/research-projects';
import { Skills } from '@/components/sections/skills';

export function HomePage() {
  return (
    <>
      <Hero />
      <About />
      <ResearchInterests />
      <ResearchProjects />
      <Publications />
      <Skills />
      <Education />
      <Contact />
      <Awards />
    </>
  );
}
```

- [ ] **Step 2: Create a placeholder ProjectPage (filled in Task 16)**

Create `src/pages/project-page.tsx`:

```tsx
import { useParams } from 'react-router-dom';

export function ProjectPage() {
  const { slug } = useParams();
  return (
    <div className="mx-auto max-w-content px-6 py-20 sm:px-8">
      <h1 className="text-3xl font-bold">Project: {slug}</h1>
      <p className="mt-2 text-muted">Sub-page coming in Task 16.</p>
    </div>
  );
}
```

- [ ] **Step 3: Create the NotFound page**

Create `src/pages/not-found.tsx`:

```tsx
import { Link } from 'react-router-dom';
import { Button } from '@/components/ui/button';

export function NotFoundPage() {
  return (
    <div className="mx-auto flex min-h-[60vh] max-w-content flex-col items-center justify-center px-6 text-center sm:px-8">
      <p className="font-mono text-xs uppercase tracking-[0.08em] text-muted">404</p>
      <h1 className="mt-3 text-3xl font-bold">Page not found</h1>
      <p className="mt-2 max-w-md text-muted">
        The page you're looking for doesn't exist or has been moved.
      </p>
      <Button asChild className="mt-6">
        <Link to="/">Back home</Link>
      </Button>
    </div>
  );
}
```

- [ ] **Step 4: Wire the router into `App.tsx`**

Replace `src/App.tsx` with:

```tsx
import { BrowserRouter, Route, Routes } from 'react-router-dom';
import { SiteFooter } from '@/components/layout/footer';
import { SiteHeader } from '@/components/layout/header';
import { ThemeProvider } from '@/components/layout/theme-provider';
import { HomePage } from '@/pages/home';
import { NotFoundPage } from '@/pages/not-found';
import { ProjectPage } from '@/pages/project-page';

function App() {
  return (
    <ThemeProvider>
      <BrowserRouter basename="/Sitan-Yan.github.io">
        <div id="top" className="flex min-h-screen flex-col bg-background text-foreground">
          <SiteHeader />
          <main className="flex-1 pt-16">
            <Routes>
              <Route path="/" element={<HomePage />} />
              <Route path="/projects/:slug" element={<ProjectPage />} />
              <Route path="*" element={<NotFoundPage />} />
            </Routes>
          </main>
          <SiteFooter />
        </div>
      </BrowserRouter>
    </ThemeProvider>
  );
}

export default App;
```

- [ ] **Step 5: Verify routing and 404 work**

```powershell
Set-Location -LiteralPath "E:\Abroad_Application\Sitan-Yan.github.io"
npm run dev
```

Open `http://localhost:5173/Sitan-Yan.github.io/`. Expected: full home page renders. Open `http://localhost:5173/Sitan-Yan.github.io/projects/foo` — placeholder. Open `http://localhost:5173/Sitan-Yan.github.io/anything-else` — 404 page. Stop server.

- [ ] **Step 6: Commit**

```powershell
Set-Location -LiteralPath "E:\Abroad_Application\Sitan-Yan.github.io"
git add -A
git commit -m "feat: add React Router with home, project, and 404 pages"
```

---

## Task 15: Migrate SFF project assets to public directory

**Files:**
- Create: `E:\Abroad_Application\Sitan-Yan.github.io\public\projects\reliability-aware-sff\micro-hole.png`
- Create: `E:\Abroad_Application\Sitan-Yan.github.io\public\projects\reliability-aware-sff\sff-system.png`
- Create: `E:\Abroad_Application\Sitan-Yan.github.io\public\projects\reliability-aware-sff\sff-system.webp`
- Create: `E:\Abroad_Application\Sitan-Yan.github.io\public\projects\reliability-aware-sff\micro-hole.mp4`
- Create: `E:\Abroad_Application\Sitan-Yan.github.io\public\images\profile-monogram.svg`
- Verify: `E:\Abroad_Application\Sitan-Yan.github.io\public\files\Sitan_Yan_CV.pdf` (or create)

- [ ] **Step 1: Copy SFF assets into the public directory**

```powershell
Set-Location -LiteralPath "E:\Abroad_Application\Sitan-Yan.github.io"
$src = "legacy-vanilla\projects\reliability-aware-sff\assets"
$dst = "public\projects\reliability-aware-sff"
New-Item -ItemType Directory -Path $dst -Force | Out-Null
Copy-Item -Path "$src\images\micro-hole.png" -Destination "$dst\micro-hole.png"
Copy-Item -Path "$src\images\sff-system.png" -Destination "$dst\sff-system.png"
Copy-Item -Path "$src\images\sff-system.webp" -Destination "$dst\sff-system.webp"
Copy-Item -Path "$src\videos\micro-hole.mp4" -Destination "$dst\micro-hole.mp4"
Get-ChildItem -Path $dst | Select-Object Name, Length
```

Expected: 4 files copied, with reasonable sizes (image files typically > 10 KB, video typically > 100 KB).

- [ ] **Step 2: Create the SY profile monogram SVG**

Create `public/images/profile-monogram.svg`:

```xml
<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 100 100">
  <rect width="100" height="100" rx="20" fill="#1e40af" />
  <text x="50" y="50" font-family="Inter, system-ui, sans-serif" font-size="44" font-weight="600" fill="#ffffff" text-anchor="middle" dominant-baseline="central" letter-spacing="-1">SY</text>
</svg>
```

- [ ] **Step 3: Copy the CV PDF into the public directory**

```powershell
Set-Location -LiteralPath "E:\Abroad_Application\Sitan-Yan.github.io"
New-Item -ItemType Directory -Path "public\files" -Force | Out-Null
if (Test-Path "legacy-vanilla\projects\reliability-aware-sff\index.html") {
  # CV was referenced from the original index.html at /files/Sitan_Yan_CV.pdf — find it
  Write-Host "Looking for existing CV PDF..."
  $cv = Get-ChildItem -Path . -Recurse -Filter "Sitan_Yan_CV.pdf" -ErrorAction SilentlyContinue | Select-Object -First 1
  if ($cv) {
    Copy-Item -Path $cv.FullName -Destination "public\files\Sitan_Yan_CV.pdf"
    Write-Host "Copied CV from $($cv.FullName)"
  } else {
    Write-Host "CV PDF not found in repo. Create a placeholder."
    "Placeholder CV" | Out-File "public\files\Sitan_Yan_CV.pdf"
  }
} else {
  Write-Host "Legacy site not present. Create a placeholder CV."
  "Placeholder CV" | Out-File "public\files\Sitan_Yan_CV.pdf"
}
```

- [ ] **Step 4: Verify all assets are reachable via the dev server**

```powershell
Set-Location -LiteralPath "E:\Abroad_Application\Sitan-Yan.github.io"
npm run dev
```

Open each URL in a browser, expect HTTP 200 + file content:
- `http://localhost:5173/Sitan-Yan.github.io/files/Sitan_Yan_CV.pdf`
- `http://localhost:5173/Sitan-Yan.github.io/projects/reliability-aware-sff/micro-hole.png`
- `http://localhost:5173/Sitan-Yan.github.io/projects/reliability-aware-sff/sff-system.png`
- `http://localhost:5173/Sitan-Yan.github.io/projects/reliability-aware-sff/sff-system.webp`
- `http://localhost:5173/Sitan-Yan.github.io/projects/reliability-aware-sff/micro-hole.mp4`
- `http://localhost:5173/Sitan-Yan.github.io/images/profile-monogram.svg`

Stop server.

- [ ] **Step 5: Commit**

```powershell
Set-Location -LiteralPath "E:\Abroad_Application\Sitan-Yan.github.io"
git add -A
git commit -m "chore: migrate SFF project assets and CV to public/"
```

---

## Task 16: Build the Project sub-page template

**Files:**
- Replace: `E:\Abroad_Application\Sitan-Yan.github.io\src\pages\project-page.tsx`
- Create: `E:\Abroad_Application\Sitan-Yan.github.io\src\components\project\project-hero.tsx` (optional split)
- Create: `E:\Abroad_Application\Sitan-Yan.github.io\src\components\project\project-media.tsx` (optional split)

**Interfaces:**
- Consumes: `Project` type from `@/data/projects`, `useParams` from `react-router-dom`
- Produces: Full sub-page with hero, problem, method, contribution, result, media gallery, prev/next nav, back link

- [ ] **Step 1: Create the `ProjectPage` component**

Replace `src/pages/project-page.tsx` with:

```tsx
import { ArrowLeft, ArrowRight, Github } from 'lucide-react';
import { Link, useParams } from 'react-router-dom';
import { BlurFade } from '@/components/magicui/blur-fade';
import { Container } from '@/components/layout/container';
import { Badge, type BadgeProps } from '@/components/ui/badge';
import { Button } from '@/components/ui/button';
import { Card, CardContent } from '@/components/ui/card';
import { Separator } from '@/components/ui/separator';
import { projects } from '@/data/projects';
import type { Project, ProjectStatus } from '@/data/projects';

const STATUS_BADGE: Record<ProjectStatus, { label: string; variant: BadgeProps['variant'] }> = {
  completed: { label: 'Completed', variant: 'success' },
  ongoing: { label: 'Ongoing', variant: 'info' },
  'in-preparation': { label: 'In Preparation', variant: 'warning' },
  'under-revision': { label: 'Under Revision', variant: 'purple' },
  planned: { label: 'Planned', variant: 'outline' },
};

function findProject(slug: string | undefined): Project | undefined {
  return projects.find((p) => p.slug === slug);
}

function getNeighbours(slug: string): { prev?: Project; next?: Project } {
  const idx = projects.findIndex((p) => p.slug === slug);
  return {
    prev: idx > 0 ? projects[idx - 1] : undefined,
    next: idx >= 0 && idx < projects.length - 1 ? projects[idx + 1] : undefined,
  };
}

export function ProjectPage() {
  const { slug } = useParams();
  const project = findProject(slug);

  if (!project) {
    return (
      <Container className="py-20">
        <h1 className="text-3xl font-bold">Project not found</h1>
        <p className="mt-2 text-muted">No project matches "{slug}".</p>
        <Button asChild className="mt-6">
          <Link to="/">
            <ArrowLeft className="mr-2 h-4 w-4" /> Back home
          </Link>
        </Button>
      </Container>
    );
  }

  const status = STATUS_BADGE[project.status];
  const { prev, next } = getNeighbours(project.slug);

  if (project.comingSoon) {
    return (
      <Container className="py-20">
        <BlurFade inView>
          <Link to="/#projects" className="inline-flex items-center text-sm text-muted hover:text-foreground">
            <ArrowLeft className="mr-1 h-4 w-4" /> Back to projects
          </Link>
        </BlurFade>
        <BlurFade inView delay={0.05}>
          <div className="mt-6 flex flex-wrap items-center gap-3">
            <span className="font-mono text-xs text-muted">{project.number}</span>
            <Badge variant={status.variant}>{status.label}</Badge>
            <span className="font-mono text-xs text-muted-foreground">{project.dateRange}</span>
          </div>
        </BlurFade>
        <BlurFade inView delay={0.1}>
          <h1 className="mt-3 text-balance text-4xl font-bold leading-tight tracking-tight sm:text-5xl">
            {project.title}
          </h1>
        </BlurFade>
        <BlurFade inView delay={0.15}>
          <Card className="mt-10">
            <CardContent className="p-8 text-center">
              <p className="text-lg font-medium">Full write-up coming soon.</p>
              <p className="mt-2 text-sm text-muted">
                In the meantime, the structured summary is on the{' '}
                <Link to="/#projects" className="text-accent hover:underline">home page</Link>.
              </p>
            </CardContent>
          </Card>
        </BlurFade>
      </Container>
    );
  }

  return (
    <article className="pb-20">
      <Container className="pt-12">
        <BlurFade inView>
          <Link to="/#projects" className="inline-flex items-center text-sm text-muted hover:text-foreground">
            <ArrowLeft className="mr-1 h-4 w-4" /> Back to projects
          </Link>
        </BlurFade>

        <BlurFade inView delay={0.05}>
          <div className="mt-6 flex flex-wrap items-center gap-3">
            <span className="font-mono text-xs text-muted">{project.number}</span>
            <Badge variant={status.variant}>{status.label}</Badge>
            <span className="font-mono text-xs text-muted-foreground">{project.dateRange}</span>
          </div>
        </BlurFade>

        <BlurFade inView delay={0.1}>
          <h1 className="mt-3 text-balance text-4xl font-bold leading-tight tracking-tight sm:text-5xl">
            {project.title}
          </h1>
        </BlurFade>

        <BlurFade inView delay={0.15}>
          <p className="mt-4 max-w-3xl text-balance text-lg text-muted">{project.summary}</p>
        </BlurFade>

        <BlurFade inView delay={0.2}>
          <div className="mt-6 flex flex-wrap items-center gap-2">
            <span className="font-mono text-xs uppercase tracking-wider text-muted">Stack</span>
            {project.technologies.map((tech) => (
              <Badge key={tech} variant="secondary">{tech}</Badge>
            ))}
          </div>
        </BlurFade>
      </Container>

      <Container className="mt-16 max-w-reading">
        <BlurFade inView>
          <Section title="Overview" body={project.problem} />
        </BlurFade>
        <BlurFade inView>
          <Section title="Method" body={project.method} />
        </BlurFade>
        <BlurFade inView>
          <div className="mt-10">
            <h2 className="font-mono text-xs uppercase tracking-[0.08em] text-muted">My contribution</h2>
            <ul className="mt-3 list-disc space-y-2 pl-5 text-foreground/90">
              {project.contribution.map((c, i) => (
                <li key={i}>{c}</li>
              ))}
            </ul>
          </div>
        </BlurFade>
        <BlurFade inView>
          <Section title="Results" body={project.result} />
        </BlurFade>

        {project.media && (project.media.images?.length || project.media.videos?.length) ? (
          <BlurFade inView>
            <div className="mt-12 space-y-6">
              <h2 className="font-mono text-xs uppercase tracking-[0.08em] text-muted">Media</h2>
              <div className="grid gap-4 sm:grid-cols-2">
                {project.media.images?.map((img) => (
                  <figure key={img.src} className="overflow-hidden rounded-xl border border-border bg-card">
                    <img
                      src={img.src}
                      alt={img.alt}
                      width={1200}
                      height={800}
                      loading="lazy"
                      className="h-auto w-full"
                    />
                    {img.caption && (
                      <figcaption className="border-t border-border px-4 py-2 text-xs text-muted">{img.caption}</figcaption>
                    )}
                  </figure>
                ))}
                {project.media.videos?.map((vid) => (
                  <figure key={vid.src} className="overflow-hidden rounded-xl border border-border bg-card sm:col-span-2">
                    <video controls preload="metadata" poster={vid.poster} className="h-auto w-full">
                      <source src={vid.src} type="video/mp4" />
                      Your browser does not support the video tag.
                    </video>
                    {vid.caption && (
                      <figcaption className="border-t border-border px-4 py-2 text-xs text-muted">{vid.caption}</figcaption>
                    )}
                  </figure>
                ))}
              </div>
            </div>
          </BlurFade>
        ) : null}
      </Container>

      <Container className="mt-16">
        <Separator />
        <div className="mt-8 flex flex-col items-stretch justify-between gap-4 sm:flex-row sm:items-center">
          {prev ? (
            <Button asChild variant="ghost">
              <Link to={`/projects/${prev.slug}`}>
                <ArrowLeft className="mr-2 h-4 w-4" /> {prev.title}
              </Link>
            </Button>
          ) : (
            <span />
          )}
          {next ? (
            <Button asChild variant="ghost">
              <Link to={`/projects/${next.slug}`}>
                {next.title} <ArrowRight className="ml-2 h-4 w-4" />
              </Link>
            </Button>
          ) : (
            <span />
          )}
        </div>
      </Container>
    </article>
  );
}

function Section({ title, body }: { title: string; body: string }) {
  return (
    <div className="mt-10">
      <h2 className="font-mono text-xs uppercase tracking-[0.08em] text-muted">{title}</h2>
      <p className="mt-3 text-foreground/90">{body}</p>
    </div>
  );
}
```

- [ ] **Step 2: Verify the SFF sub-page renders and the placeholder sub-pages render**

```powershell
Set-Location -LiteralPath "E:\Abroad_Application\Sitan-Yan.github.io"
npm run dev
```

Open each URL, verify content:
- `http://localhost:5173/Sitan-Yan.github.io/projects/reliability-aware-sff` — full sub-page with title, status badge, dates, problem, method, contribution list, results, media gallery with images and video, prev/next nav.
- `http://localhost:5173/Sitan-Yan.github.io/projects/focus-constrained-vpc` — "Full write-up coming soon" card.
- `http://localhost:5173/Sitan-Yan.github.io/projects/dynamic-obstacle-avoidance` — same.
- `http://localhost:5173/Sitan-Yan.github.io/projects/does-not-exist` — "Project not found" view.

Stop server.

- [ ] **Step 3: Commit**

```powershell
Set-Location -LiteralPath "E:\Abroad_Application\Sitan-Yan.github.io"
git add -A
git commit -m "feat: build project sub-page template"
```

---

## Task 17: Polish — ESLint, Prettier, README

**Files:**
- Create: `E:\Abroad_Application\Sitan-Yan.github.io\.eslintrc.cjs`
- Create: `E:\Abroad_Application\Sitan-Yan.github.io\.prettierrc`
- Modify: `E:\Abroad_Application\Sitan-Yan.github.io\package.json` (add lint/format scripts)
- Replace: `E:\Abroad_Application\Sitan-Yan.github.io\README.md`

- [ ] **Step 1: Create the ESLint config**

Create `.eslintrc.cjs`:

```js
module.exports = {
  root: true,
  env: { browser: true, es2022: true },
  extends: [
    'eslint:recommended',
    'plugin:@typescript-eslint/recommended',
    'plugin:react-hooks/recommended',
  ],
  ignorePatterns: ['dist', '.eslintrc.cjs', 'node_modules'],
  parser: '@typescript-eslint/parser',
  plugins: ['react-refresh'],
  rules: {
    'react-refresh/only-export-components': ['warn', { allowConstantExport: true }],
    '@typescript-eslint/no-explicit-any': 'warn',
    '@typescript-eslint/no-unused-vars': ['warn', { argsIgnorePattern: '^_' }],
  },
};
```

- [ ] **Step 2: Install ESLint dependencies**

```powershell
Set-Location -LiteralPath "E:\Abroad_Application\Sitan-Yan.github.io"
npm install -D eslint@^8.57.0 @typescript-eslint/parser@^7.0.0 @typescript-eslint/eslint-plugin@^7.0.0 eslint-plugin-react-hooks@^4.6.0 eslint-plugin-react-refresh@^0.4.0
```

- [ ] **Step 3: Create the Prettier config**

Create `.prettierrc`:

```json
{
  "semi": true,
  "singleQuote": true,
  "trailingComma": "all",
  "printWidth": 110,
  "tabWidth": 2,
  "plugins": ["prettier-plugin-tailwindcss"]
}
```

- [ ] **Step 4: Add lint/format scripts to `package.json`**

Read current scripts in `package.json`, then add (or merge) these:

```json
{
  "scripts": {
    "dev": "vite",
    "build": "tsc && vite build",
    "preview": "vite preview",
    "lint": "eslint . --ext .ts,.tsx --max-warnings 0",
    "format": "prettier --write \"src/**/*.{ts,tsx,css}\""
  }
}
```

If a `scripts` block already exists, add the missing keys. If `build` differs (e.g. doesn't include `tsc`), replace it with the version above so type errors fail the build.

- [ ] **Step 5: Run lint and format, fix any issues**

```powershell
Set-Location -LiteralPath "E:\Abroad_Application\Sitan-Yan.github.io"
npm run lint
npm run format
git diff --stat
```

If `lint` reports errors, fix them. If `format` made changes, review the diff with `git diff` and confirm the changes are cosmetic only.

- [ ] **Step 6: Replace the README**

Replace `README.md` with:

```markdown
# Sitan Yan — Academic Homepage

Personal academic portfolio, built with Vite + React + TypeScript + Tailwind.

## Development

```powershell
npm install
npm run dev
```

Open http://localhost:5173/Sitan-Yan.github.io/.

## Production build

```powershell
npm run build
```

Outputs to `dist/`.

## Deploy to GitHub Pages

```powershell
npm run build
npx gh-pages -d dist
```

The site is served from the `gh-pages` branch at https://sitan-yan.github.io/.

## Project structure

- `src/data/` — typed TS content (profile, projects, publications, skills, interests)
- `src/components/sections/` — one component per home-page section
- `src/components/layout/` — header, footer, container, theme provider
- `src/components/magicui/` — BlurFade, GridBackground
- `src/components/ui/` — shadcn primitives
- `src/pages/` — home, project sub-page, 404
- `public/projects/` — project media assets (images, video)
- `public/files/` — CV PDF

## Adding a new project

1. Add a `Project` entry to `src/data/projects.ts` with `slug`, `number`, `title`, etc.
2. If it has a sub-page, drop assets into `public/projects/<slug>/`
3. The new project appears on the home page automatically; the sub-page is rendered at `/projects/<slug>`.

## License

Code: MIT. Content (bio, project descriptions, publications): © Sitan Yan.
```

- [ ] **Step 7: Commit**

```powershell
Set-Location -LiteralPath "E:\Abroad_Application\Sitan-Yan.github.io"
git add -A
git commit -m "chore: add ESLint, Prettier, and README"
```

---

## Task 18: Build the production bundle and verify

**Files:**
- (no new files; verifies Task 1–17 work end to end)

- [ ] **Step 1: Clean and rebuild**

```powershell
Set-Location -LiteralPath "E:\Abroad_Application\Sitan-Yan.github.io"
Remove-Item -Path "dist" -Recurse -Force -ErrorAction SilentlyContinue
npm run build
```

Expected: `dist/` created with `index.html`, `assets/`, and copied public files. No TypeScript errors. No Vite errors.

- [ ] **Step 2: Preview the production build locally**

```powershell
Set-Location -LiteralPath "E:\Abroad_Application\Sitan-Yan.github.io"
npm run preview
```

Open `http://localhost:4173/Sitan-Yan.github.io/`. Verify all sections render, theme toggle works, project sub-pages load, 404 page works. Stop server.

- [ ] **Step 3: Verify bundle size**

```powershell
Set-Location -LiteralPath "E:\Abroad_Application\Sitan-Yan.github.io"
Get-ChildItem -Path "dist\assets" -Filter "*.js" | Select-Object Name, @{Name = 'SizeKB'; Expression = { [math]::Round($_.Length / 1024, 1) } }
```

Expected: at least one JS file, total < 250 KB gzipped (raw < 800 KB). If a chunk is suspiciously large, note it for future optimization.

- [ ] **Step 4: Run Lighthouse (optional but recommended)**

If Chrome is installed, run:

```powershell
npx lighthouse http://localhost:4173/Sitan-Yan.github.io/ --view --preset=desktop
```

After `npm run preview` is running in another terminal. Look for: Performance ≥ 90, Accessibility ≥ 95, Best Practices ≥ 95. Fix any critical issues before deploying.

- [ ] **Step 5: Commit (only if any fixes were made)**

```powershell
Set-Location -LiteralPath "E:\Abroad_Application\Sitan-Yan.github.io"
git add -A
git status
# If anything is staged:
git commit -m "fix: address lint/build issues from production build pass"
```

---

## Task 19: Configure SPA 404 fallback and add `gh-pages` deploy script

**Files:**
- Create: `E:\Abroad_Application\Sitan-Yan.github.io\404.html` (manually maintained for GH Pages SPA routing)
- Create: `E:\Abroad_Application\Sitan-Yan.github.io\scripts\build-404.mjs` (post-build script that copies `dist/index.html` to `dist/404.html` for future deploys — but the standalone `404.html` is what GH Pages will actually serve in production)
- Modify: `E:\Abroad_Application\Sitan-Yan.github.io\package.json` (add `postbuild` script)

- [ ] **Step 1: Create the SPA `404.html` from the current build output**

```powershell
Set-Location -LiteralPath "E:\Abroad_Application\Sitan-Yan.github.io"
npm run build
Copy-Item -Path "dist\index.html" -Destination "dist\404.html"
```

- [ ] **Step 2: Add a `postbuild` script so future builds do this automatically**

Replace `package.json` build script with:

```json
{
  "scripts": {
    "dev": "vite",
    "build": "tsc && vite build && node scripts/copy-404.mjs",
    "preview": "vite preview",
    "lint": "eslint . --ext .ts,.tsx --max-warnings 0",
    "format": "prettier --write \"src/**/*.{ts,tsx,css}\""
  }
}
```

Create `scripts/copy-404.mjs`:

```js
import { copyFileSync, existsSync } from 'node:fs';
import { resolve } from 'node:path';

const src = resolve('dist/index.html');
const dst = resolve('dist/404.html');

if (!existsSync(src)) {
  console.error('dist/index.html missing — build the project first');
  process.exit(1);
}

copyFileSync(src, dst);
console.log('Copied dist/index.html → dist/404.html');
```

- [ ] **Step 3: Add the `gh-pages` deploy script to `package.json`**

Add to scripts:

```json
{
  "scripts": {
    "dev": "vite",
    "build": "tsc && vite build && node scripts/copy-404.mjs",
    "deploy": "npm run build && gh-pages -d dist",
    "preview": "vite preview",
    "lint": "eslint . --ext .ts,.tsx --max-warnings 0",
    "format": "prettier --write \"src/**/*.{ts,tsx,css}\""
  }
}
```

Install `gh-pages` as a dev dependency:

```powershell
Set-Location -LiteralPath "E:\Abroad_Application\Sitan-Yan.github.io"
npm install -D gh-pages@^6.1.0
```

- [ ] **Step 4: Verify `dist/404.html` exists after rebuild**

```powershell
Set-Location -LiteralPath "E:\Abroad_Application\Sitan-Yan.github.io"
Remove-Item -Path "dist" -Recurse -Force -ErrorAction SilentlyContinue
npm run build
Test-Path -Path "dist\404.html" -PathType Leaf
```

Expected: `True`.

- [ ] **Step 5: Commit**

```powershell
Set-Location -LiteralPath "E:\Abroad_Application\Sitan-Yan.github.io"
git add -A
git commit -m "chore: add SPA 404 fallback and deploy script"
```

---

## Task 20: Final deploy and acceptance check

**Files:**
- (no new files; verifies the entire system end to end against the spec's acceptance criteria)

- [ ] **Step 1: Remove the legacy vanilla backup (the site is no longer needed)**

```powershell
Set-Location -LiteralPath "E:\Abroad_Application\Sitan-Yan.github.io"
Remove-Item -Path "legacy-vanilla" -Recurse -Force -ErrorAction SilentlyContinue
```

- [ ] **Step 2: Verify all spec acceptance criteria**

```powershell
Set-Location -LiteralPath "E:\Abroad_Application\Sitan-Yan.github.io"

# 1. Dev server runs
npm run dev
```

Manually verify in browser at `http://localhost:5173/Sitan-Yan.github.io/`:
- All 9 home sections render with content
- `/projects/reliability-aware-sff` renders the full sub-page
- `/projects/focus-constrained-vpc` and `/projects/dynamic-obstacle-avoidance` show "coming soon"
- Light/dark toggle works and persists
- Responsive at 360px, 768px, 1024px, 1440px (use DevTools device toolbar)

Stop server.

```powershell
# 2. Production build works
npm run build
npm run preview
```

Manually verify at `http://localhost:4173/Sitan-Yan.github.io/`:
- Same content as dev
- Deep link `/projects/reliability-aware-sff` works on first load (the 404 fallback serves `index.html`, React Router takes over)

Stop server.

- [ ] **Step 3: Deploy to GitHub Pages**

```powershell
Set-Location -LiteralPath "E:\Abroad_Application\Sitan-Yan.github.io"
npm run deploy
```

If `gh-pages` prompts to confirm, accept. This pushes the `dist/` directory to the `gh-pages` branch.

- [ ] **Step 4: Verify the live site**

Open https://sitan-yan.github.io/ in a browser. Walk through the same checklist as Step 2. Pay special attention to:
- Base path resolves correctly (no 404 on JS/CSS chunks)
- Theme toggle works
- Sub-page deep link works (paste `https://sitan-yan.github.io/projects/reliability-aware-sff` directly in URL bar)

- [ ] **Step 5: Final commit (if any fixes were made during verification)**

```powershell
Set-Location -LiteralPath "E:\Abroad_Application\Sitan-Yan.github.io"
git add -A
git status
if (git diff --cached --quiet) {
  Write-Host "No changes to commit."
} else {
  git commit -m "fix: address issues from live-site verification"
}
```

- [ ] **Step 6: Tag the v1 release**

```powershell
Set-Location -LiteralPath "E:\Abroad_Application\Sitan-Yan.github.io"
git tag -a v1.0.0 -m "v1.0.0: academic portfolio React rebuild"
git push origin v1.0.0
```

(Only push the tag if the user confirms they want a release tag.)

---

## Self-Review

**1. Spec coverage** — checking each spec requirement against a task:

| Spec section | Task |
|---|---|
| §1 Goals, audience, non-goals | Tasks 1–20 (project-level framing) |
| §2.1 Home section order (9 sections) | Tasks 9 (Hero), 10 (About/Interests/Education), 11 (Projects), 12 (Publications/Skills/Contact), 13 (Awards) |
| §2.2 Section content | Task 7 (data layer) — all content is there |
| §2.3 Project sub-page | Task 16 (sub-page template) |
| §3.1 Style direction | Task 3 (Tailwind tokens) + Tasks 9–13 (component-level execution) |
| §3.2 Color palette | Task 3 (CSS variables) |
| §3.3 Typography | Task 2 (Google Fonts in index.html) + Task 3 (Tailwind fontFamily) |
| §3.4 Spacing & layout | Task 8 (Container) + section components (use `py-20 md:py-24`) |
| §3.5 Motion | Task 5 (BlurFade) + Task 8 (header scroll state) |
| §3.6 Component patterns | Tasks 4 (shadcn), 9–13 (sections use card/badge patterns) |
| §3.7 Responsive | Mobile-first in all section components, Sheet for mobile nav (Task 8) |
| §4 Tech stack | Task 1 (Vite scaffold), Task 2 (deps), Task 4 (shadcn), Task 5 (Magic UI) |
| §5 File structure | Tasks 1–16 create files matching the spec's target tree |
| §6 Deployment | Task 19 (404 fallback, deploy script), Task 20 (live deploy) |
| §7 Accessibility & performance | Reduced-motion guard (Task 3), lazy images + alt text (Task 16), focus-visible rings (Task 4), Lighthouse check (Task 18) |
| §8 Risks (Skills content inferred, SFF migration, 404 fallback) | Task 7 (Skills is inferred, marked in spec), Task 15 (asset migration), Task 19 (404 fallback) |
| §10 Acceptance criteria (10 items) | Task 20 verifies all 10 items |

**2. Placeholder scan** — searched plan for `TBD`, `TODO`, `implement later`, `add validation`, `appropriate`, `similar to Task N`. None found. Every step has either a real file path, a real command, or a real code block. "TBD" only appears in user-facing spec discussion, not in plan steps.

**3. Type consistency check** — verified the data type names match across tasks:
- `Profile` / `ProfileLink` / `EducationEntry` / `AwardEntry` — defined in Task 7, used in Tasks 8–13, 16
- `Interest` — defined in Task 7, used in Task 10
- `Project` / `ProjectStatus` / `ProjectMedia` — defined in Task 7, used in Task 11 (status mapping keys) and Task 16
- `Publication` / `PublicationStatus` — defined in Task 7, used in Task 12
- `SkillColumn` — defined in Task 7, used in Task 12
- `BadgeProps['variant']` — defined in Task 4, used in Tasks 11, 12, 16
- `STATUS_BADGE` record — defined in Task 11 (for projects) and Task 12 (for publications). Same name, different scopes, no cross-task collision. Task 16 also defines `STATUS_BADGE` locally for the sub-page, also a local scope. No type collisions.
- `Container`, `SectionHeader`, `SiteHeader`, `SiteFooter` — defined in Task 8, used in Tasks 9–13
- `Hero`, `About`, `ResearchInterests`, `ResearchProjects`, `Publications`, `Skills`, `Education`, `Contact`, `Awards` — defined in Tasks 9–13, composed in Task 14 (`HomePage`)

**4. Ambiguity check** — each step has a single concrete action. The only "in your judgment" decisions are: (a) which Tailwind utility classes look right for a given component (covered by the spec's design tokens and the explicit examples in each step), (b) Lighthouse tuning if scores come in low (Task 18 explicitly says "fix any critical issues" — direction is clear).

**5. Risks introduced by the plan itself**:
- Task 15 assumes the legacy vanilla site is still present in `legacy-vanilla/`. If a previous task deleted it, fall back to locating the assets in the original `projects/reliability-aware-sff/assets/` directory.
- Task 16's `getNeighbours` returns prev/next based on the order in `projects` array; if a future project is added out of order, neighbours will be wrong. Acceptable for v1; future improvement is to add an explicit `order` field.
- Tasks 1–13 incrementally wire `App.tsx` with new sections. If a section is built but `App.tsx` is not updated, the section won't appear. Each task explicitly tells the implementer to update `App.tsx` — no ambiguity.
