# Tailwind + Next.js
## Tailwind with Next.js
Tailwind is a first-class citizen in Next.js. `create-next-app` scaffolds it. In **Tailwind v4**, styling lives in `app/globals.css` — no `tailwind.config.js` needed unless you want custom tokens.
```
npx create-next-app@latest my-app --typescript --tailwind --app
```
Key rules:
- Styling works in both Server and Client Components.
- Static classes are compiled at build time. Dynamic string concatenation (`bg-${color}-500`) will not work.
- Import `globals.css` **once**, in `app/layout.tsx`.
## App Router styling
Everything under `app/` follows the same styling rules. Each route segment can add its own `layout.tsx`, `page.tsx`, `loading.tsx`, `error.tsx`, `not-found.tsx`.

```
app/
├── layout.tsx         ← root layout (import globals.css here)
├── page.tsx           ← home page
├── globals.css
├── dashboard/
│   ├── layout.tsx     ← nested layout (sidebar wraps children)
│   ├── page.tsx
│   ├── loading.tsx
│   └── error.tsx
```
## Server Component styling
Default in App Router. No `"use client"`, no state. Tailwind classes work exactly the same.
```tsx
// app/page.tsx — Server Component
export default function Home() {
  return (
    <main className="mx-auto max-w-4xl px-6 py-16">
      <h1 className="text-4xl font-extrabold tracking-tight">Hello</h1>
      <p className="mt-4 text-gray-600 dark:text-gray-400">Rendered on the server.</p>
    </main>
  );
}
```
Use Server Components for anything static: marketing pages, docs, blog posts, list displays. Zero JS sent to the client.
## Client Component styling
Add `"use client"` at the top. Needed for `useState`, `useEffect`, event handlers, browser APIs.
```tsx
"use client";
import { useState } from "react";

export function Counter() {
  const [n, setN] = useState(0);
  return (
    <button
      onClick={() => setN(n + 1)}
      className="rounded-lg bg-indigo-600 px-4 py-2 text-white hover:bg-indigo-500 transition"
    >
      Clicked {n} times
    </button>
  );
}
```
Rule: keep Client Components **small** and at the leaves of your tree. Fetch data in Server Components, pass it down as props.
## Layout styling
A layout wraps every page in its segment. Styling here persists across navigation — the layout **does not re-render** when moving between pages.
```tsx
// app/dashboard/layout.tsx
import { Sidebar } from "@/components/Sidebar";
export default function DashboardLayout({ children }: { children: React.ReactNode }) {
  return (
    <div className="min-h-screen bg-gray-50 dark:bg-gray-950">
      <Sidebar />
      <main className="md:pl-64">{children}</main>
    </div>
  );
}
```
Use layouts for: navbars, sidebars, page shells, background colors, fonts.
## Page styling
A page is the leaf of a route. Style it freely — it renders inside its parent layout.
```tsx
// app/dashboard/page.tsx
export default function DashboardPage() {
  return (
    <section className="mx-auto max-w-6xl px-6 py-10 space-y-6">
      <h1 className="text-2xl font-bold tracking-tight">Overview</h1>
      <div className="grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-4 gap-4">
        {/* stat cards */}
      </div>
    </section>
  );
}
```
## Responsive Next.js navbar
Sticky nav with mobile menu (Client Component) and static links (Server Component).
```tsx
// components/Navbar.tsx
"use client";
import { useState } from "react";
import Link from "next/link";

export function Navbar({ links }: { links: { href: string; label: string }[] }) {
  const [open, setOpen] = useState(false);

  return (
    <header className="sticky top-0 z-40 border-b border-gray-200 bg-white/80 backdrop-blur dark:border-gray-800 dark:bg-gray-950/80">
      <nav className="mx-auto flex max-w-6xl items-center justify-between px-6 py-4">
        <Link href="/" className="font-bold tracking-tight">Brand</Link>

        <div className="hidden md:flex gap-6 text-sm">
          {links.map((l) => (
            <Link key={l.href} href={l.href} className="text-gray-600 hover:text-gray-900 dark:text-gray-400 dark:hover:text-gray-100 transition-colors">
              {l.label}
            </Link>
          ))}
        </div>

        <button className="md:hidden text-2xl" onClick={() => setOpen(!open)} aria-label="Menu">
          {open ? "✕" : "☰"}
        </button>
      </nav>

      {open && (
        <div className="md:hidden flex flex-col gap-3 border-t border-gray-200 px-6 py-4 text-sm dark:border-gray-800">
          {links.map((l) => (
            <Link key={l.href} href={l.href}>{l.label}</Link>
          ))}
        </div>
      )}
    </header>
  );
}
```
Use **`<Link>` from `next/link`** — it prefetches routes. Plain `<a>` reloads the whole page.
## Dashboard layout
Sidebar + main. Sidebar is static (Server Component); the mobile toggle lives in a small Client child.
```tsx
// components/Sidebar.tsx — Server Component
import Link from "next/link";
export function Sidebar({ active = "" }: { active?: string }) {
  const items = [
    { href: "/dashboard", label: "Overview" },
    { href: "/dashboard/reports", label: "Reports" },
    { href: "/dashboard/settings", label: "Settings" },
  ];
  return (
    <aside className="hidden md:flex fixed left-0 top-0 h-screen w-64 flex-col border-r border-gray-200 bg-white dark:border-gray-800 dark:bg-gray-900">
      <div className="h-16 flex items-center px-6 font-bold border-b border-gray-200 dark:border-gray-800">Brand</div>
      <nav className="flex-1 p-3 space-y-1">
        {items.map((it) => (
          <Link
            key={it.href}
            href={it.href}
            className={`block rounded-lg px-3 py-2 text-sm font-medium transition ${
              active === it.href
                ? "bg-indigo-50 text-indigo-700 dark:bg-indigo-500/10 dark:text-indigo-400"
                : "text-gray-600 hover:bg-gray-100 dark:text-gray-400 dark:hover:bg-gray-800"
            }`}
          >
            {it.label}
          </Link>
        ))}
      </nav>
    </aside>
  );
}
```
## Authentication UI
Login page — Server Component shell + Client form.
```tsx
// app/login/page.tsx
import { LoginForm } from "@/components/LoginForm";
export default function LoginPage() {
  return (
    <main className="min-h-screen flex items-center justify-center bg-gradient-to-br from-gray-50 to-gray-100 px-6 dark:from-gray-950 dark:to-gray-900">
      <div className="w-full max-w-md rounded-2xl border border-gray-200 bg-white p-8 shadow-sm dark:border-gray-800 dark:bg-gray-900">
        <h1 className="text-2xl font-bold tracking-tight">Welcome back</h1>
        <p className="mt-1 text-sm text-gray-500 dark:text-gray-400">Sign in to continue.</p>
        <LoginForm />
      </div>
    </main>
  );
}
```
`LoginForm` is a Client Component handling `useState` + `fetch`.
## Loading UI
`loading.tsx` shows automatically while a route segment's data is loading (Suspense boundary).
```tsx
// app/dashboard/loading.tsx
export default function Loading() {
  return (
    <div className="mx-auto max-w-6xl px-6 py-10 space-y-4">
      <div className="h-8 w-48 animate-pulse rounded-md bg-gray-200 dark:bg-gray-800" />
      <div className="grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-4 gap-4">
        {[1, 2, 3, 4].map((i) => (
          <div key={i} className="h-28 animate-pulse rounded-2xl bg-gray-200 dark:bg-gray-800" />
        ))}
      </div>
    </div>
  );
}
```
Skeletons over spinners — they match the final layout and reduce perceived wait.
## Error UI
`error.tsx` catches runtime errors in a route segment. **Must** be a Client Component.
```tsx
// app/dashboard/error.tsx
"use client";
export default function Error({ error, reset }: { error: Error & { digest?: string }; reset: () => void }) {
  return (
    <div className="flex min-h-[60vh] items-center justify-center px-6">
      <div className="max-w-md text-center">
        <div className="mx-auto flex h-14 w-14 items-center justify-center rounded-full bg-red-50 text-2xl text-red-600 dark:bg-red-500/10 dark:text-red-400">
          !
        </div>
        <h2 className="mt-4 text-xl font-bold tracking-tight">Something went wrong</h2>
        <p className="mt-2 text-sm text-gray-500 dark:text-gray-400">{error.message}</p>
        <button
          onClick={reset}
          className="mt-6 rounded-lg bg-indigo-600 px-5 py-2.5 text-sm font-semibold text-white hover:bg-indigo-500 transition"
        >
          Try again
        </button>
      </div>
    </div>
  );
}
```
## Not-found UI
`not-found.tsx` renders when `notFound()` is called or a route doesn't exist.
```tsx
// app/not-found.tsx
import Link from "next/link";
export default function NotFound() {
  return (
    <div className="flex min-h-screen items-center justify-center px-6">
      <div className="max-w-md text-center">
        <p className="text-7xl font-extrabold tracking-tight bg-gradient-to-r from-indigo-500 to-purple-500 bg-clip-text text-transparent">
          404
        </p>
        <h1 className="mt-4 text-xl font-bold tracking-tight">Page not found</h1>
        <p className="mt-2 text-sm text-gray-500 dark:text-gray-400">
          The page you're looking for doesn't exist or has been moved.
        </p>
        <Link
          href="/"
          className="mt-6 inline-block rounded-lg bg-indigo-600 px-5 py-2.5 text-sm font-semibold text-white hover:bg-indigo-500 transition"
        >
          Back home
        </Link>
      </div>
    </div>
  );
}
```
Trigger it manually from any Server Component:
```tsx
import { notFound } from "next/navigation";
if (!post) notFound();
```
## SEO-friendly responsive layouts
Combine Next.js `metadata` with responsive Tailwind layouts.
```tsx
// app/layout.tsx
import type { Metadata } from "next";
import "./globals.css";
export const metadata: Metadata = {
  title: { default: "My App", template: "%s · My App" },
  description: "A modern Next.js + Tailwind app.",
  openGraph: { type: "website", locale: "en_US", siteName: "My App" },
  twitter: { card: "summary_large_image" },
  robots: { index: true, follow: true },
};
export default function RootLayout({ children }: { children: React.ReactNode }) {
  return (
    <html lang="en" suppressHydrationWarning>
      <body className="bg-gray-50 text-gray-900 dark:bg-gray-950 dark:text-gray-100">
        {children}
      </body>
    </html>
  );
}
```
Per-page metadata:
```tsx
// app/dashboard/page.tsx
export const metadata = { title: "Dashboard" };
```
**SEO + responsive checklist:**
- Semantic HTML: `<header>`, `<nav>`, `<main>`, `<article>`, `<footer>`.
- One `<h1>` per page; correct heading hierarchy (`h1 → h2 → h3`).
- `<Image>` from `next/image` for automatic sizing, lazy loading, and `alt` enforcement.
- `<Link>` for internal nav (prefetch + client-side routing).
- `metadata` for title/description/OG per route.
- Tailwind mobile-first: base styles for the smallest screen, layer up with `sm:` `md:` `lg:`.
- Test with Lighthouse on **mobile** — SEO score reflects mobile layout and metadata.
