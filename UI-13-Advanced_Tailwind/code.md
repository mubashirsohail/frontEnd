# Practice Project: Advanced Tailwind Component Library
**Goal:** A showcase page of reusable components built with **container queries, custom utilities, theme tokens, arbitrary variants, data attributes, ARIA, motion-safe, and print styles** — all in one file + one CSS file.
## 1. `app/globals.css` — Theme tokens + custom utilities
```css
@import "tailwindcss";
/* Enable class-based dark mode */
@custom-variant dark (&:where(.dark, .dark *));
/* ============ Custom theme tokens ============ */
@theme {
  /* Brand palette */
  --color-brand-50:  #eef2ff;
  --color-brand-100: #e0e7ff;
  --color-brand-500: #6366f1;
  --color-brand-600: #4f46e5;
  --color-brand-700: #4338ca;
  /* Surface tokens (theme-aware via CSS var) */
  --color-surface:      #ffffff;
  --color-surface-alt:  #f9fafb;
  --color-surface-dark: #0f172a;
  --color-surface-dark-alt: #1e293b;
  /* Radius + shadow tokens */
  --radius-card: 1rem;
  --radius-pill: 9999px;
  --shadow-soft: 0 2px 8px rgb(0 0 0 / 0.06);
  --shadow-glow: 0 10px 40px -10px rgb(99 102 241 / 0.5);
  /* Extra breakpoint */
  --breakpoint-3xl: 120rem;
}
/* ============ Custom utilities ============ */
@utility btn-base {
  display: inline-flex;
  align-items: center;
  justify-content: center;
  gap: 0.5rem;
  padding: 0.625rem 1rem;
  font-size: 0.875rem;
  font-weight: 600;
  border-radius: var(--radius-card);
  transition: all 200ms ease-out;
}

@utility card-base {
  border-radius: var(--radius-card);
  border: 1px solid rgb(229 231 235);
  background-color: var(--color-surface);
  box-shadow: var(--shadow-soft);
}

@utility scrollbar-none {
  scrollbar-width: none;
  &::-webkit-scrollbar {
    display: none;
  }
}

/* Make dark mode swap surface tokens */
.dark {
  --color-surface: var(--color-surface-dark);
  --color-surface-alt: var(--color-surface-dark-alt);
}
```

---

## 2. `app/layout.tsx` — Same as before (theme init script)

```tsx
import "./globals.css";

export const metadata = { title: "Component Library" };

const themeInit = `
(function () {
  try {
    var t = localStorage.getItem('theme');
    var prefers = window.matchMedia('(prefers-color-scheme: dark)').matches;
    if (t === 'dark' || (!t && prefers)) document.documentElement.classList.add('dark');
    else document.documentElement.classList.remove('dark');
  } catch (e) {}
})();
`;

export default function RootLayout({ children }: { children: React.ReactNode }) {
  return (
    <html lang="en" suppressHydrationWarning>
      <head>
        <script dangerouslySetInnerHTML={{ __html: themeInit }} />
      </head>
      <body className="bg-gray-50 dark:bg-gray-950">{children}</body>
    </html>
  );
}
```

---

## 3. `app/page.jsx` — Component Library Showcase

```jsx
// app/page.jsx
"use client";
import { useState, useEffect } from "react";

export default function ComponentLibrary() {
  const [dark, setDark] = useState(false);
  const [ready, setReady] = useState(false);

  useEffect(() => {
    setDark(document.documentElement.classList.contains("dark"));
    setReady(true);
  }, []);

  function toggleTheme() {
    const next = !dark;
    setDark(next);
    document.documentElement.classList.toggle("dark", next);
    localStorage.setItem("theme", next ? "dark" : "light");
  }

  return (
    <div className="min-h-screen bg-gray-50 text-gray-900 dark:bg-gray-950 dark:text-gray-100 print:bg-white print:text-black">

      {/* ===== Sticky Nav (hidden in print) ===== */}
      <header className="sticky top-0 z-40 border-b border-gray-200 bg-white/80 backdrop-blur dark:border-gray-800 dark:bg-gray-950/80 print:hidden">
        <nav className="mx-auto flex max-w-6xl items-center justify-between px-6 py-4">
          <span className="font-bold tracking-tight">
            UI<span className="text-brand-600 dark:text-brand-500">Kit</span>
          </span>

          <div className="hidden md:flex gap-6 text-sm text-gray-600 dark:text-gray-400">
            <a href="#buttons" className="hover:text-gray-900 dark:hover:text-gray-100 transition-colors">Buttons</a>
            <a href="#cards" className="hover:text-gray-900 dark:hover:text-gray-100 transition-colors">Cards</a>
            <a href="#alerts" className="hover:text-gray-900 dark:hover:text-gray-100 transition-colors">Alerts</a>
            <a href="#forms" className="hover:text-gray-900 dark:hover:text-gray-100 transition-colors">Forms</a>
          </div>

          <button
            onClick={toggleTheme}
            aria-label="Toggle theme"
            className="relative flex h-9 w-16 items-center rounded-full border border-gray-300 bg-gray-100 px-1 transition-colors dark:border-gray-700 dark:bg-gray-800 focus:outline-none focus-visible:ring-2 focus-visible:ring-brand-500"
          >
            <span
              className={`flex h-7 w-7 items-center justify-center rounded-full bg-white text-xs shadow transition-transform duration-300 dark:bg-gray-950 ${
                ready && dark ? "translate-x-7" : "translate-x-0"
              }`}
            >
              {ready && dark ? "🌙" : "☀️"}
            </span>
          </button>
        </nav>
      </header>

      <main className="mx-auto max-w-6xl px-6 py-12 space-y-16 print:py-4 print:space-y-8">

        {/* ============ Header ============ */}
        <section className="text-center">
          <span className="inline-flex items-center gap-2 rounded-full border border-brand-100 bg-brand-50 px-3 py-1 text-xs font-semibold uppercase tracking-widest text-brand-700 dark:border-brand-500/30 dark:bg-brand-500/10 dark:text-brand-500">
            Advanced Tailwind
          </span>
          <h1 className="mt-4 text-3xl md:text-5xl font-extrabold tracking-tight">
            Component{" "}
            <span className="bg-gradient-to-r from-brand-500 to-purple-500 bg-clip-text text-transparent">
              Library
            </span>
          </h1>
          <p className="mt-4 text-sm md:text-base text-gray-600 dark:text-gray-400 max-w-xl mx-auto">
            Built with container queries, custom utilities, theme tokens, data attributes, and motion-safe variants.
          </p>
        </section>

        {/* ============ Buttons ============ */}
        <Section id="buttons" title="Buttons" note="Uses @utility btn-base + arbitrary variants">
          <div className="flex flex-wrap gap-3">
            <button className="btn-base bg-brand-600 text-white hover:bg-brand-500 focus-visible:ring-2 focus-visible:ring-brand-500 focus-visible:ring-offset-2 focus:outline-none">
              Primary
            </button>
            <button className="btn-base border border-gray-300 bg-white text-gray-800 hover:bg-gray-50 dark:border-gray-700 dark:bg-gray-900 dark:text-gray-100 dark:hover:bg-gray-800">
              Secondary
            </button>
            <button className="btn-base bg-gray-900 text-white hover:bg-gray-800 dark:bg-gray-100 dark:text-gray-900 dark:hover:bg-white">
              Inverse
            </button>
            <button
              disabled
              className="btn-base cursor-not-allowed bg-gray-200 text-gray-400 dark:bg-gray-800 dark:text-gray-600"
            >
              Disabled
            </button>
            <button className="btn-base bg-gradient-to-r from-brand-500 to-purple-500 text-white shadow-glow hover:-translate-y-0.5 motion-reduce:hover:translate-y-0">
              Gradient
            </button>
          </div>
        </Section>

        {/* ============ Container-Query Cards ============ */}
        <Section id="cards" title="Container-Query Cards" note="@container + @md: + @lg: — cards adapt to their parent, not the viewport">
          <div className="grid grid-cols-1 lg:grid-cols-2 gap-6">

            {/* Wide container */}
            <div className="@container card-base p-6">
              <p className="text-xs uppercase tracking-widest text-gray-400 dark:text-gray-500 mb-3">
                Wide parent
              </p>
              <div className="grid grid-cols-1 @md:grid-cols-2 @lg:grid-cols-3 gap-4">
                {["Design", "Build", "Ship"].map((t) => (
                  <div key={t} className="rounded-lg border border-gray-200 bg-gray-50 p-4 dark:border-gray-800 dark:bg-gray-950">
                    <p className="font-semibold text-sm">{t}</p>
                    <p className="text-xs text-gray-500 dark:text-gray-400 mt-1">A compact tile.</p>
                  </div>
                ))}
              </div>
            </div>

            {/* Narrow container */}
            <div className="@container card-base p-6">
              <p className="text-xs uppercase tracking-widest text-gray-400 dark:text-gray-500 mb-3">
                Narrow parent — same markup, different layout
              </p>
              <div className="grid grid-cols-1 @md:grid-cols-2 @lg:grid-cols-3 gap-4">
                {["Design", "Build", "Ship"].map((t) => (
                  <div key={t} className="rounded-lg border border-gray-200 bg-gray-50 p-4 dark:border-gray-800 dark:bg-gray-950">
                    <p className="font-semibold text-sm">{t}</p>
                    <p className="text-xs text-gray-500 dark:text-gray-400 mt-1">A compact tile.</p>
                  </div>
                ))}
              </div>
            </div>

          </div>
        </Section>

        {/* ============ Feature Card ============ */}
        <Section id="feature" title="Feature Card" note="group-hover + motion-safe + arbitrary variants">
          <div className="grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-3 gap-6">
            {[
              { icon: "⚡", title: "Fast", desc: "Ship in seconds." },
              { icon: "🔒", title: "Secure", desc: "SOC2-ready." },
              { icon: "📈", title: "Scalable", desc: "Grows with you." },
            ].map((f) => (
              <div
                key={f.title}
                className="
                  group card-base p-6
                  transition-all duration-300
                  hover:-translate-y-1 hover:shadow-glow hover:border-brand-500
                  motion-reduce:transition-none motion-reduce:hover:translate-y-0
                  [&>h3]:transition-colors group-hover:[&>h3]:text-brand-600
                  dark:group-hover:[&>h3]:text-brand-500
                "
              >
                <div className="text-2xl">{f.icon}</div>
                <h3 className="mt-3 font-semibold">{f.title}</h3>
                <p className="mt-1 text-sm text-gray-500 dark:text-gray-400">{f.desc}</p>
              </div>
            ))}
          </div>
        </Section>

        {/* ============ Alerts ============ */}
        <Section id="alerts" title="Alerts" note="Role + aria-live, data-variant driven styling">
          <div className="space-y-3">
            <Alert variant="info"    title="Heads up">New version available.</Alert>
            <Alert variant="success" title="All good">Your changes are saved.</Alert>
            <Alert variant="warning" title="Careful">This action can't be undone.</Alert>
            <Alert variant="error"   title="Error">Something went wrong.</Alert>
          </div>
        </Section>

        {/* ============ Data-Attribute Accordion ============ */}
        <Section id="accordion" title="Accordion" note="data-[state=open] variant drives styling">
          <div className="card-base divide-y divide-gray-200 dark:divide-gray-800">
            <Accordion title="What is Tailwind?">A utility-first CSS framework.</Accordion>
            <Accordion title="Why container queries?">Because components should adapt to their parent, not just the viewport.</Accordion>
            <Accordion title="How do tokens work?">@theme in CSS defines named design tokens you can use anywhere.</Accordion>
          </div>
        </Section>

        {/* ============ Forms ============ */}
        <Section id="forms" title="Form Controls" note="aria-invalid, disabled, dark:, focus-visible">
          <div className="grid grid-cols-1 @container md:grid-cols-2 gap-6">
            <div className="card-base p-6 space-y-4">
              <div>
                <label className="block text-sm font-medium text-gray-700 dark:text-gray-300">Email</label>
                <input
                  type="email"
                  defaultValue="you@example.com"
                  className="mt-1.5 w-full rounded-lg border border-gray-300 bg-white px-3.5 py-2.5 text-sm text-gray-900 placeholder:text-gray-400 focus:border-brand-500 focus:ring-2 focus:ring-brand-100 focus:outline-none dark:border-gray-700 dark:bg-gray-950 dark:text-gray-100 dark:focus:border-brand-500 dark:focus:ring-brand-500/30"
                />
              </div>
              <div>
                <label className="block text-sm font-medium text-gray-700 dark:text-gray-300">Invalid example</label>
                <input
                  type="text"
                  aria-invalid="true"
                  defaultValue="not-an-email"
                  className="mt-1.5 w-full rounded-lg border border-red-500 bg-white px-3.5 py-2.5 text-sm text-gray-900 focus:border-red-500 focus:ring-2 focus:ring-red-200 focus:outline-none dark:border-red-500 dark:bg-gray-950 dark:text-gray-100"
                />
                <p className="mt-1.5 text-xs text-red-600 dark:text-red-400">Please enter a valid email.</p>
              </div>
            </div>

            <div className="card-base p-6 space-y-4">
              <div>
                <label className="block text-sm font-medium text-gray-700 dark:text-gray-300">Disabled</label>
                <input
                  disabled
                  defaultValue="Can't edit"
                  className="mt-1.5 w-full cursor-not-allowed rounded-lg border border-gray-300 bg-gray-100 px-3.5 py-2.5 text-sm text-gray-400 dark:border-gray-700 dark:bg-gray-800 dark:text-gray-500"
                />
              </div>
              <div>
                <label className="block text-sm font-medium text-gray-700 dark:text-gray-300">Custom checkbox</label>
                <label className="mt-1.5 inline-flex items-center gap-3 cursor-pointer">
                  <input type="checkbox" defaultChecked className="peer sr-only" />
                  <span className="flex h-4 w-4 items-center justify-center rounded border border-gray-300 bg-white text-[10px] text-white transition peer-checked:border-brand-600 peer-checked:bg-brand-600 peer-focus-visible:ring-2 peer-focus-visible:ring-brand-500 dark:border-gray-600 dark:bg-gray-950">
                    ✓
                  </span>
                  <span className="text-sm text-gray-700 dark:text-gray-300">Enable notifications</span>
                </label>
              </div>
            </div>
          </div>
        </Section>

        {/* ============ CSS Variable Theming ============ */}
        <Section id="cssvars" title="CSS Variable Theming" note="Inline style sets --accent; component reads var(--accent)">
          <div className="grid grid-cols-1 sm:grid-cols-3 gap-4">
            {[
              { name: "Indigo",  color: "#6366f1" },
              { name: "Emerald", color: "#10b981" },
              { name: "Rose",    color: "#f43f5e" },
            ].map((c) => (
              <div
                key={c.name}
                style={{ "--accent": c.color }}
                className="card-base p-6 [background:linear-gradient(135deg,var(--accent),transparent_70%)]"
              >
                <p className="text-sm font-semibold text-white drop-shadow">{c.name}</p>
                <p className="mt-1 text-xs text-white/80">var(--accent) via inline style</p>
              </div>
            ))}
          </div>
        </Section>

        {/* ============ Advanced Responsive Grid ============ */}
        <Section id="grid" title="Advanced Responsive Layout" note="Arbitrary grid-template-areas + custom 3xl breakpoint">
          <div className="
            grid gap-4
            grid-cols-1
            sm:grid-cols-2
            lg:grid-cols-[240px_1fr]
            [grid-template-areas:'sidebar''main']
            lg:[grid-template-areas:'sidebar_main']
          ">
            <aside className="[grid-area:sidebar] card-base p-6 text-sm">
              Sidebar
            </aside>
            <main className="[grid-area:main] card-base p-6 text-sm">
              Main content — layout switches with named grid areas.
            </main>
          </div>
        </Section>

      </main>

      {/* ===== Footer (hidden in print) ===== */}
      <footer className="border-t border-gray-200 py-8 print:hidden dark:border-gray-800">
        <p className="mx-auto max-w-6xl px-6 text-xs uppercase tracking-widest text-gray-400 dark:text-gray-500">
          UIKit · Built with Tailwind v4
        </p>
      </footer>

    </div>
  );
}

/* ================= Reusable primitives ================= */

function Section({ id, title, note, children }) {
  return (
    <section id={id} className="scroll-mt-24">
      <div className="flex flex-col sm:flex-row sm:items-end sm:justify-between gap-2 mb-6 print:mb-3">
        <h2 className="text-xl md:text-2xl font-bold tracking-tight">{title}</h2>
        {note && (
          <p className="text-xs text-gray-500 dark:text-gray-400 max-w-md">
            {note}
          </p>
        )}
      </div>
      {children}
    </section>
  );
}

/* ---- Alert (data-variant driven) ---- */
function Alert({ variant = "info", title, children }) {
  const styles = {
    info:    "border-brand-100 bg-brand-50 text-brand-700 dark:border-brand-500/30 dark:bg-brand-500/10 dark:text-brand-500",
    success: "border-emerald-200 bg-emerald-50 text-emerald-700 dark:border-emerald-500/30 dark:bg-emerald-500/10 dark:text-emerald-400",
    warning: "border-amber-200 bg-amber-50 text-amber-700 dark:border-amber-500/30 dark:bg-amber-500/10 dark:text-amber-400",
    error:   "border-red-200 bg-red-50 text-red-700 dark:border-red-500/30 dark:bg-red-500/10 dark:text-red-400",
  };

  return (
    <div
      data-variant={variant}
      role="alert"
      className={`rounded-lg border p-4 ${styles[variant]}`}
    >
      <p className="text-sm font-semibold">{title}</p>
      <p className="mt-0.5 text-sm opacity-90">{children}</p>
    </div>
  );
}

/* ---- Accordion (data-state driven) ---- */
function Accordion({ title, children }) {
  const [open, setOpen] = useState(false);

  return (
    <div>
      <button
        type="button"
        onClick={() => setOpen(!open)}
        aria-expanded={open}
        data-state={open ? "open" : "closed"}
        className="
          group flex w-full items-center justify-between px-5 py-4 text-left text-sm font-medium
          transition-colors
          hover:bg-gray-50 dark:hover:bg-gray-950
          focus:outline-none focus-visible:ring-2 focus-visible:ring-brand-500
        "
      >
        {title}
        <span
          className="
            text-gray-400 transition-transform duration-200
            data-[state=open]:rotate-180
            group-data-[state=open]:text-brand-600
            motion-reduce:transition-none
          "
        >
          ▾
        </span>
      </button>
      <div
        data-state={open ? "open" : "closed"}
        className="
          grid transition-all duration-300
          data-[state=open]:grid-rows-[1fr]
          data-[state=closed]:grid-rows-[0fr]
          motion-reduce:transition-none
        "
      >
        <div className="overflow-hidden">
          <p className="px-5 pb-4 text-sm text-gray-600 dark:text-gray-400">
            {children}
          </p>
        </div>
      </div>
    </div>
  );
}
```
## Advanced Features Used — Cheat Sheet
| Feature | Where |
|---|---|
| **`@theme` tokens** | `--color-brand-*`, `--radius-card`, `--shadow-glow`, `--breakpoint-3xl` |
| **Custom utilities** | `@utility btn-base`, `@utility card-base`, `@utility scrollbar-none` |
| **CSS variable theming** | Surface tokens swap when `.dark` is on root |
| **Inline CSS vars** | `<div style={{ "--accent": color }}>` → used in `[background:...]` |
| **Container queries** | `@container` + `@md:` + `@lg:` — same markup, two parent widths |
| **Arbitrary values** | `grid-cols-[240px_1fr]`, `bg-[url(...)]` |
| **Arbitrary properties** | `[background:linear-gradient(...)]`, `[grid-area:sidebar]` |
| **Arbitrary variants** | `[&>h3]:transition-colors`, `group-hover:[&>h3]:text-brand-600` |
| **Data attributes** | `data-[state=open]:rotate-180`, `data-[state=closed]:grid-rows-[0fr]` |
| **Named groups** | `group-data-[state=open]:text-brand-600` |
| **ARIA variants** | `aria-expanded`, `aria-invalid`, `role="alert"` |
| **Motion-safe / reduce** | `motion-reduce:transition-none`, `motion-reduce:hover:translate-y-0` |
| **Print styles** | `print:hidden` on nav/footer, `print:bg-white print:text-black` |
| **Custom breakpoint** | `3xl:` available via `--breakpoint-3xl` |
| **Dark mode** | `.dark` class + surface token override |
## Test Checklist
- **Buttons** — hover primary/secondary, disable, focus-ring via keyboard
- **Container queries** — the two card sections show the SAME markup but **different column counts** (wide parent → 3 cols, narrow parent → 2 or 1 col)
- **Feature cards** — hover lifts, title turns brand-colored, motion-reduce disables the lift (test with system setting)
- **Alerts** — each variant uses its own palette, `role="alert"` present
- **Accordion** — click → chevron rotates, panel expands. `data-state` attribute toggles on both button and panel
- **Forms** — invalid field red, disabled field muted, custom checkbox works via keyboard
- **CSS var theming** — three gradient cards each in a different color, same markup
- **Advanced grid** — sidebar/main rearrange at `lg:` via named grid areas
- **Dark mode toggle** — whole page flips
- **Print preview** (Ctrl+P) — nav and footer hidden, only content visible
