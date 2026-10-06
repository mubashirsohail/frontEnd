# Practice Project: Complete Next.js SaaS Dashboard
**Goal:** A real Next.js App Router project with a dashboard shell, sidebar, responsive navbar, stats, charts, tables, auth UI, loading/error/not-found pages, and full dark mode — all with Tailwind v4.
## Project Structure
```
saas-dashboard/
├── app/
│   ├── globals.css
│   ├── layout.tsx              ← root shell + metadata + theme script
│   ├── page.tsx                ← marketing / redirect to dashboard
│   ├── loading.tsx             ← root loading skeleton
│   ├── error.tsx               ← root error boundary
│   ├── not-found.tsx           ← 404
│   ├── login/
│   │   └── page.tsx            ← auth UI
│   └── dashboard/
│       ├── layout.tsx          ← sidebar + topbar shell
│       ├── page.tsx            ← overview (stats + chart + activity)
│       ├── loading.tsx
│       ├── error.tsx
│       ├── customers/
│       │   └── page.tsx        ← table view
│       ├── reports/
│       │   └── page.tsx
│       └── settings/
│           └── page.tsx
├── components/
│   ├── ThemeToggle.tsx         ← client
│   ├── Navbar.tsx              ← client (mobile menu)
│   ├── Sidebar.tsx             ← server
│   ├── StatCard.tsx            ← server
│   └── LoginForm.tsx           ← client
└── lib/
    ├── cn.ts
    └── nav.ts                  ← shared nav items
```
## 1. `app/globals.css`
```css
@import "tailwindcss";
@custom-variant dark (&:where(.dark, .dark *));
@theme {
  --color-brand-50:  #eef2ff;
  --color-brand-100: #e0e7ff;
  --color-brand-500: #6366f1;
  --color-brand-600: #4f46e5;
  --color-brand-700: #4338ca;
}
```
## 2. `lib/nav.ts`
```ts
export interface NavItem {
  href: string;
  label: string;
  icon: string;
}
export const navItems: NavItem[] = [
  { href: "/dashboard", label: "Overview", icon: "▦" },
  { href: "/dashboard/customers", label: "Customers", icon: "☺" },
  { href: "/dashboard/reports", label: "Reports", icon: "▤" },
  { href: "/dashboard/settings", label: "Settings", icon: "⚙" },
];
```
## 3. `lib/cn.ts`
```ts
import clsx, { type ClassValue } from "clsx";
import { twMerge } from "tailwind-merge";
export function cn(...args: ClassValue[]) {
  return twMerge(clsx(args));
}
```
Install if you don't have it:
```bash
npm install clsx tailwind-merge
```
## 4. `app/layout.tsx`
```tsx
import type { Metadata } from "next";
import "./globals.css";
export const metadata: Metadata = {
  title: { default: "Dashly", template: "%s · Dashly" },
  description: "A modern SaaS dashboard built with Next.js + Tailwind v4.",
};
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
      <body className="bg-gray-50 text-gray-900 antialiased dark:bg-gray-950 dark:text-gray-100">
        {children}
      </body>
    </html>
  );
}
```
## 5. `components/ThemeToggle.tsx`
```tsx
"use client";
import { useEffect, useState } from "react";
export function ThemeToggle() {
  const [dark, setDark] = useState(false);
  const [ready, setReady] = useState(false);

  useEffect(() => {
    setDark(document.documentElement.classList.contains("dark"));
    setReady(true);
  }, []);

  function toggle() {
    const next = !dark;
    setDark(next);
    document.documentElement.classList.toggle("dark", next);
    localStorage.setItem("theme", next ? "dark" : "light");
  }

  return (
    <button
      onClick={toggle}
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
  );
}
```
## 6. `components/Navbar.tsx` (dashboard topbar)
```tsx
"use client";
import Link from "next/link";
import { useState } from "react";
import { ThemeToggle } from "./ThemeToggle";
import { navItems } from "@/lib/nav";

export function Navbar() {
  const [open, setOpen] = useState(false);

  return (
    <header className="sticky top-0 z-40 border-b border-gray-200 bg-white/80 backdrop-blur dark:border-gray-800 dark:bg-gray-950/80 md:pl-64">
      <div className="mx-auto flex max-w-7xl items-center justify-between gap-4 px-6 py-4">

        {/* Mobile brand */}
        <Link href="/dashboard" className="font-bold tracking-tight md:hidden">
          Dash<span className="text-brand-600 dark:text-brand-500">ly</span>
        </Link>

        {/* Search */}
        <div className="hidden flex-1 md:flex">
          <input
            type="text"
            placeholder="Search…"
            className="w-full max-w-md rounded-lg border border-gray-300 bg-white px-3.5 py-2 text-sm text-gray-900 placeholder:text-gray-400 focus:border-brand-500 focus:ring-2 focus:ring-brand-100 focus:outline-none dark:border-gray-700 dark:bg-gray-900 dark:text-gray-100 dark:placeholder:text-gray-500 dark:focus:border-brand-500 dark:focus:ring-brand-500/30"
          />
        </div>

        <div className="ml-auto flex items-center gap-3">
          <button
            type="button"
            aria-label="Notifications"
            className="relative rounded-lg border border-gray-200 bg-white px-3 py-2 text-sm transition hover:bg-gray-50 dark:border-gray-700 dark:bg-gray-900 dark:hover:bg-gray-800 focus:outline-none focus-visible:ring-2 focus-visible:ring-brand-500"
          >
            🔔
            <span className="absolute -right-1 -top-1 flex h-4 w-4 items-center justify-center rounded-full bg-red-500 text-[10px] font-bold text-white ring-2 ring-white dark:ring-gray-950">
              3
            </span>
          </button>

          <ThemeToggle />

          <button
            onClick={() => setOpen((v) => !v)}
            aria-label="Menu"
            className="rounded-lg p-2 hover:bg-gray-100 md:hidden dark:hover:bg-gray-800 focus:outline-none focus-visible:ring-2 focus-visible:ring-brand-500"
          >
            {open ? "✕" : "☰"}
          </button>
        </div>
      </div>

      {/* Mobile menu */}
      {open && (
        <div className="border-t border-gray-200 px-6 py-4 md:hidden dark:border-gray-800">
          <nav className="flex flex-col gap-3 text-sm">
            {navItems.map((it) => (
              <Link
                key={it.href}
                href={it.href}
                onClick={() => setOpen(false)}
                className="flex items-center gap-3 rounded-lg px-3 py-2 text-gray-700 hover:bg-gray-100 dark:text-gray-300 dark:hover:bg-gray-800"
              >
                <span>{it.icon}</span>
                {it.label}
              </Link>
            ))}
          </nav>
        </div>
      )}
    </header>
  );
}
```
## 7. `components/Sidebar.tsx`
```tsx
import Link from "next/link";
import { navItems } from "@/lib/nav";
export function Sidebar({ active = "/dashboard" }: { active?: string }) {
  return (
    <aside className="hidden md:flex fixed left-0 top-0 h-screen w-64 flex-col border-r border-gray-200 bg-white dark:border-gray-800 dark:bg-gray-900">
      <div className="flex h-16 items-center border-b border-gray-200 px-6 text-lg font-bold tracking-tight dark:border-gray-800">
        Dash<span className="text-brand-600 dark:text-brand-500">ly</span>
      </div>

      <nav className="flex-1 space-y-1 p-3">
        {navItems.map((it) => {
          const isActive = active === it.href;
          return (
            <Link
              key={it.href}
              href={it.href}
              className={`flex items-center gap-3 rounded-lg px-3 py-2 text-sm font-medium transition ${
                isActive
                  ? "bg-brand-50 text-brand-700 dark:bg-brand-500/10 dark:text-brand-500"
                  : "text-gray-600 hover:bg-gray-100 dark:text-gray-400 dark:hover:bg-gray-800"
              }`}
            >
              <span className="text-base">{it.icon}</span>
              {it.label}
            </Link>
          );
        })}
      </nav>

      <div className="border-t border-gray-200 p-4 dark:border-gray-800">
        <div className="flex items-center gap-3">
          <span className="flex h-9 w-9 items-center justify-center rounded-full bg-brand-100 text-sm font-semibold text-brand-700 dark:bg-brand-500/20 dark:text-brand-500">
            A
          </span>
          <div className="min-w-0">
            <p className="truncate text-sm font-medium">Ali Raza</p>
            <p className="truncate text-xs text-gray-500 dark:text-gray-400">ali@example.com</p>
          </div>
        </div>
      </div>
    </aside>
  );
}
```
## 8. `components/StatCard.tsx`
```tsx
interface StatCardProps {
  label: string;
  value: string;
  delta: string;
  up: boolean;
}

export function StatCard({ label, value, delta, up }: StatCardProps) {
  return (
    <div className="rounded-2xl border border-gray-200 bg-white p-5 shadow-sm transition hover:-translate-y-0.5 hover:shadow-md dark:border-gray-800 dark:bg-gray-900 dark:shadow-none dark:hover:border-gray-700 motion-reduce:transition-none motion-reduce:hover:translate-y-0">
      <p className="text-xs uppercase tracking-widest text-gray-400 dark:text-gray-500">{label}</p>
      <p className="mt-2 text-2xl font-bold">{value}</p>
      <p
        className={`mt-1 text-xs font-medium ${
          up ? "text-emerald-600 dark:text-emerald-400" : "text-red-600 dark:text-red-400"
        }`}
      >
        {delta} <span className="text-gray-400 dark:text-gray-500">vs last week</span>
      </p>
    </div>
  );
}
```
## 9. `app/dashboard/layout.tsx`
```tsx
import { Navbar } from "@/components/Navbar";
import { Sidebar } from "@/components/Sidebar";
import { navItems } from "@/lib/nav";

export default function DashboardLayout({ children }: { children: React.ReactNode }) {
  return (
    <div className="min-h-screen">
      <Sidebar active="/dashboard" />
      <Navbar />
      <main className="md:pl-64">
        <div className="mx-auto max-w-7xl px-6 py-10">{children}</div>
      </main>
    </div>
  );
}
```

> **Note:** The `active` prop here is hardcoded for simplicity. In production, use `usePathname()` inside a small Client wrapper around `<Sidebar>` to compute the active route dynamically.
## 10. `app/dashboard/page.tsx`
```tsx
import Link from "next/link";
import { StatCard } from "@/components/StatCard";

export const metadata = { title: "Overview" };

export default function OverviewPage() {
  const stats = [
    { label: "Users", value: "12,480", delta: "+8.2%", up: true },
    { label: "Revenue", value: "$42.1k", delta: "+12.5%", up: true },
    { label: "Churn", value: "2.4%", delta: "-0.6%", up: false },
    { label: "Sessions", value: "87,203", delta: "+3.1%", up: true },
  ];

  return (
    <div className="space-y-8">
      {/* Header */}
      <div className="flex flex-col gap-4 sm:flex-row sm:items-center sm:justify-between">
        <div>
          <h1 className="text-2xl font-bold tracking-tight md:text-3xl">Overview</h1>
          <p className="mt-1 text-sm text-gray-500 dark:text-gray-400">
            Welcome back — here's what's happening today.
          </p>
        </div>
        <Link
          href="/dashboard/reports"
          className="w-full rounded-lg bg-brand-600 px-4 py-2.5 text-center text-sm font-semibold text-white transition hover:bg-brand-500 focus:outline-none focus-visible:ring-2 focus-visible:ring-brand-500 focus-visible:ring-offset-2 sm:w-auto dark:focus-visible:ring-offset-gray-950"
        >
          + New report
        </Link>
      </div>

      {/* Stats */}
      <section className="grid grid-cols-1 gap-4 sm:grid-cols-2 lg:grid-cols-4">
        {stats.map((s) => (
          <StatCard key={s.label} {...s} />
        ))}
      </section>

      {/* Chart + Activity */}
      <section className="grid grid-cols-1 gap-6 lg:grid-cols-3">
        <div className="lg:col-span-2 rounded-2xl border border-gray-200 bg-white p-6 shadow-sm dark:border-gray-800 dark:bg-gray-900 dark:shadow-none">
          <div className="flex items-center justify-between">
            <div>
              <h2 className="font-semibold">Traffic</h2>
              <p className="text-xs text-gray-500 dark:text-gray-400">Last 7 days</p>
            </div>
            <span className="rounded-full bg-emerald-50 px-2.5 py-1 text-xs font-semibold text-emerald-700 dark:bg-emerald-500/10 dark:text-emerald-400">
              Live
            </span>
          </div>

          <div className="mt-6 flex h-40 items-end gap-2">
            {[40, 65, 30, 80, 55, 90, 70].map((h, i) => (
              <div key={i} className="flex flex-1 flex-col items-center gap-2">
                <div
                  style={{ height: `${h}%` }}
                  className="w-full rounded-t-md bg-gradient-to-t from-brand-500 to-purple-500"
                />
                <span className="text-[10px] text-gray-400 dark:text-gray-500">
                  {["M", "T", "W", "T", "F", "S", "S"][i]}
                </span>
              </div>
            ))}
          </div>
        </div>

        <div className="rounded-2xl border border-gray-200 bg-white p-6 shadow-sm dark:border-gray-800 dark:bg-gray-900 dark:shadow-none">
          <h2 className="font-semibold">Recent activity</h2>
          <ul className="mt-4 divide-y divide-gray-100 dark:divide-gray-800">
            {[
              ["Deployed v2.4", "2m ago", "bg-emerald-500"],
              ["New signup", "1h ago", "bg-brand-500"],
              ["Payment failed", "3h ago", "bg-red-500"],
              ["Report exported", "5h ago", "bg-amber-500"],
            ].map(([text, time, dot]) => (
              <li key={text} className="flex items-center gap-3 py-3">
                <span className={`h-2 w-2 rounded-full ${dot}`} />
                <span className="text-sm text-gray-700 dark:text-gray-300">{text}</span>
                <span className="ml-auto text-xs text-gray-400 dark:text-gray-500">{time}</span>
              </li>
            ))}
          </ul>
        </div>
      </section>
    </div>
  );
}
```
## 11. `app/dashboard/loading.tsx`
```tsx
export default function Loading() {
  return (
    <div className="space-y-8">
      <div className="h-8 w-48 animate-pulse rounded-md bg-gray-200 dark:bg-gray-800" />
      <div className="grid grid-cols-1 gap-4 sm:grid-cols-2 lg:grid-cols-4">
        {[1, 2, 3, 4].map((i) => (
          <div key={i} className="h-28 animate-pulse rounded-2xl bg-gray-200 dark:bg-gray-800" />
        ))}
      </div>
      <div className="grid grid-cols-1 gap-6 lg:grid-cols-3">
        <div className="h-64 animate-pulse rounded-2xl bg-gray-200 lg:col-span-2 dark:bg-gray-800" />
        <div className="h-64 animate-pulse rounded-2xl bg-gray-200 dark:bg-gray-800" />
      </div>
    </div>
  );
}
```
## 12. `app/dashboard/error.tsx`
```tsx
"use client";

export default function Error({
  error,
  reset,
}: {
  error: Error & { digest?: string };
  reset: () => void;
}) {
  return (
    <div className="flex min-h-[60vh] items-center justify-center">
      <div className="max-w-md text-center">
        <div className="mx-auto flex h-14 w-14 items-center justify-center rounded-full bg-red-50 text-2xl text-red-600 dark:bg-red-500/10 dark:text-red-400">
          !
        </div>
        <h2 className="mt-4 text-xl font-bold tracking-tight">Something went wrong</h2>
        <p className="mt-2 text-sm text-gray-500 dark:text-gray-400">
          {error.message || "An unexpected error occurred."}
        </p>
        <button
          onClick={reset}
          className="mt-6 rounded-lg bg-brand-600 px-5 py-2.5 text-sm font-semibold text-white transition hover:bg-brand-500 focus:outline-none focus-visible:ring-2 focus-visible:ring-brand-500 focus-visible:ring-offset-2 dark:focus-visible:ring-offset-gray-950"
        >
          Try again
        </button>
      </div>
    </div>
  );
}
```
## 13. `app/dashboard/customers/page.tsx`
```tsx
export const metadata = { title: "Customers" };
const customers = [
  { name: "Ali Raza", email: "ali@example.com", plan: "Pro", status: "Active" },
  { name: "Sarah Khan", email: "sarah@example.com", plan: "Starter", status: "Active" },
  { name: "Bilal Ahmed", email: "bilal@example.com", plan: "Pro", status: "Paused" },
  { name: "Hina Malik", email: "hina@example.com", plan: "Enterprise", status: "Active" },
];
export default function CustomersPage() {
  return (
    <div className="space-y-6">
      <div>
        <h1 className="text-2xl font-bold tracking-tight md:text-3xl">Customers</h1>
        <p className="mt-1 text-sm text-gray-500 dark:text-gray-400">
          Manage your customer accounts and subscriptions.
        </p>
      </div>

      {/* Scrollable table on mobile */}
      <div className="overflow-hidden rounded-2xl border border-gray-200 bg-white shadow-sm dark:border-gray-800 dark:bg-gray-900 dark:shadow-none">
        <div className="overflow-x-auto">
          <table className="min-w-full divide-y divide-gray-200 text-sm dark:divide-gray-800">
            <thead className="bg-gray-50 dark:bg-gray-950">
              <tr>
                <th className="px-6 py-3 text-left font-medium text-gray-500 dark:text-gray-400">Name</th>
                <th className="px-6 py-3 text-left font-medium text-gray-500 dark:text-gray-400">Email</th>
                <th className="px-6 py-3 text-left font-medium text-gray-500 dark:text-gray-400">Plan</th>
                <th className="px-6 py-3 text-left font-medium text-gray-500 dark:text-gray-400">Status</th>
              </tr>
            </thead>
            <tbody className="divide-y divide-gray-100 dark:divide-gray-800">
              {customers.map((c) => (
                <tr key={c.email} className="transition hover:bg-gray-50 dark:hover:bg-gray-950">
                  <td className="whitespace-nowrap px-6 py-4 font-medium">{c.name}</td>
                  <td className="whitespace-nowrap px-6 py-4 text-gray-600 dark:text-gray-400">{c.email}</td>
                  <td className="whitespace-nowrap px-6 py-4 text-gray-600 dark:text-gray-400">{c.plan}</td>
                  <td className="whitespace-nowrap px-6 py-4">
                    <span
                      className={`inline-flex items-center rounded-full px-2.5 py-1 text-xs font-semibold ${
                        c.status === "Active"
                          ? "bg-emerald-50 text-emerald-700 dark:bg-emerald-500/10 dark:text-emerald-400"
                          : "bg-amber-50 text-amber-700 dark:bg-amber-500/10 dark:text-amber-400"
                      }`}
                    >
                      {c.status}
                    </span>
                  </td>
                </tr>
              ))}
            </tbody>
          </table>
        </div>
      </div>
    </div>
  );
}
```
## 14. `app/dashboard/reports/page.tsx` and `settings/page.tsx`
```tsx
// app/dashboard/reports/page.tsx
export const metadata = { title: "Reports" };

export default function ReportsPage() {
  return (
    <div className="space-y-6">
      <div>
        <h1 className="text-2xl font-bold tracking-tight md:text-3xl">Reports</h1>
        <p className="mt-1 text-sm text-gray-500 dark:text-gray-400">
          Export and analyze your data.
        </p>
      </div>
      <div className="rounded-2xl border-2 border-dashed border-gray-300 p-12 text-center text-sm text-gray-400 dark:border-gray-700 dark:text-gray-500">
        No reports yet — create your first one.
      </div>
    </div>
  );
}
```
```tsx
// app/dashboard/settings/page.tsx
export const metadata = { title: "Settings" };
export default function SettingsPage() {
  return (
    <div className="space-y-6">
      <div>
        <h1 className="text-2xl font-bold tracking-tight md:text-3xl">Settings</h1>
        <p className="mt-1 text-sm text-gray-500 dark:text-gray-400">
          Manage your workspace preferences.
        </p>
      </div>
      <div className="rounded-2xl border border-gray-200 bg-white p-6 shadow-sm dark:border-gray-800 dark:bg-gray-900 dark:shadow-none">
        <h2 className="font-semibold">Profile</h2>
        <p className="mt-1 text-sm text-gray-500 dark:text-gray-400">
          Update your personal information.
        </p>
        <div className="mt-4 grid grid-cols-1 gap-4 md:grid-cols-2">
          <div>
            <label className="block text-sm font-medium text-gray-700 dark:text-gray-300">Full name</label>
            <input
              defaultValue="Ali Raza"
              className="mt-1.5 w-full rounded-lg border border-gray-300 bg-white px-3.5 py-2.5 text-sm focus:border-brand-500 focus:ring-2 focus:ring-brand-100 focus:outline-none dark:border-gray-700 dark:bg-gray-950 dark:text-gray-100 dark:focus:border-brand-500 dark:focus:ring-brand-500/30"
            />
          </div>
          <div>
            <label className="block text-sm font-medium text-gray-700 dark:text-gray-300">Email</label>
            <input
              defaultValue="ali@example.com"
              className="mt-1.5 w-full rounded-lg border border-gray-300 bg-white px-3.5 py-2.5 text-sm focus:border-brand-500 focus:ring-2 focus:ring-brand-100 focus:outline-none dark:border-gray-700 dark:bg-gray-950 dark:text-gray-100 dark:focus:border-brand-500 dark:focus:ring-brand-500/30"
            />
          </div>
        </div>
      </div>
    </div>
  );
}
```
## 15. `app/login/page.tsx` + `components/LoginForm.tsx`
```tsx
// app/login/page.tsx — Server Component
import { LoginForm } from "@/components/LoginForm";
export const metadata = { title: "Sign in" };
export default function LoginPage() {
  return (
    <main className="flex min-h-screen items-center justify-center bg-gradient-to-br from-gray-50 to-gray-100 px-6 dark:from-gray-950 dark:to-gray-900">
      <div className="w-full max-w-md rounded-2xl border border-gray-200 bg-white p-8 shadow-sm dark:border-gray-800 dark:bg-gray-900">
        <div className="text-center">
          <span className="text-2xl font-extrabold tracking-tight">
            Dash<span className="text-brand-600 dark:text-brand-500">ly</span>
          </span>
          <h1 className="mt-4 text-2xl font-bold tracking-tight">Welcome back</h1>
          <p className="mt-2 text-sm text-gray-500 dark:text-gray-400">
            Sign in to continue to your dashboard.
          </p>
        </div>

        <LoginForm />
      </div>
    </main>
  );
}
```
```tsx
// components/LoginForm.tsx — Client Component
"use client";
import { useState } from "react";
import { useRouter } from "next/navigation";
export function LoginForm() {
  const router = useRouter();
  const [loading, setLoading] = useState(false);
  const [email, setEmail] = useState("");
  const [password, setPassword] = useState("");
  function onSubmit(e: React.FormEvent) {
    e.preventDefault();
    setLoading(true);
    setTimeout(() => {
      setLoading(false);
      router.push("/dashboard");
    }, 1200);
  }

  return (
    <form onSubmit={onSubmit} className="mt-6 space-y-4">
      <div>
        <label htmlFor="email" className="block text-sm font-medium text-gray-700 dark:text-gray-300">
          Email
        </label>
        <input
          id="email"
          type="email"
          value={email}
          onChange={(e) => setEmail(e.target.value)}
          placeholder="you@example.com"
          autoComplete="email"
          className="mt-1.5 w-full rounded-lg border border-gray-300 bg-white px-3.5 py-2.5 text-sm text-gray-900 placeholder:text-gray-400 focus:border-brand-500 focus:ring-2 focus:ring-brand-100 focus:outline-none dark:border-gray-700 dark:bg-gray-950 dark:text-gray-100 dark:placeholder:text-gray-500 dark:focus:border-brand-500 dark:focus:ring-brand-500/30"
        />
      </div>

      <div>
        <label htmlFor="password" className="block text-sm font-medium text-gray-700 dark:text-gray-300">
          Password
        </label>
        <input
          id="password"
          type="password"
          value={password}
          onChange={(e) => setPassword(e.target.value)}
          placeholder="••••••••"
          autoComplete="current-password"
          className="mt-1.5 w-full rounded-lg border border-gray-300 bg-white px-3.5 py-2.5 text-sm text-gray-900 placeholder:text-gray-400 focus:border-brand-500 focus:ring-2 focus:ring-brand-100 focus:outline-none dark:border-gray-700 dark:bg-gray-950 dark:text-gray-100 dark:placeholder:text-gray-500 dark:focus:border-brand-500 dark:focus:ring-brand-500/30"
        />
      </div>

      <button
        type="submit"
        disabled={loading}
        className="w-full rounded-lg bg-brand-600 px-5 py-3 text-sm font-semibold text-white shadow-sm transition hover:bg-brand-500 hover:shadow-lg disabled:cursor-not-allowed disabled:opacity-50 disabled:hover:bg-brand-600 disabled:hover:shadow-sm focus:outline-none focus-visible:ring-2 focus-visible:ring-brand-500 focus-visible:ring-offset-2 dark:focus-visible:ring-offset-gray-900 motion-reduce:transition-none"
      >
        {loading ? (
          <span className="inline-flex items-center justify-center gap-2">
            <span className="h-4 w-4 animate-spin rounded-full border-2 border-white/40 border-t-white motion-reduce:animate-none" />
            Signing in…
          </span>
        ) : (
          "Sign in"
        )}
      </button>
    </form>
  );
}
```
## 16. `app/page.tsx` — Root (redirect to dashboard)
```tsx
import { redirect } from "next/navigation";
export default function Home() {
  redirect("/dashboard");
}
```
## 17. `app/not-found.tsx`
```tsx
import Link from "next/link";

export default function NotFound() {
  return (
    <div className="flex min-h-screen items-center justify-center px-6">
      <div className="max-w-md text-center">
        <p className="bg-gradient-to-r from-brand-500 to-purple-500 bg-clip-text text-7xl font-extrabold tracking-tight text-transparent">
          404
        </p>
        <h1 className="mt-4 text-xl font-bold tracking-tight">Page not found</h1>
        <p className="mt-2 text-sm text-gray-500 dark:text-gray-400">
          The page you're looking for doesn't exist or has been moved.
        </p>
        <Link
          href="/dashboard"
          className="mt-6 inline-block rounded-lg bg-brand-600 px-5 py-2.5 text-sm font-semibold text-white transition hover:bg-brand-500 focus:outline-none focus-visible:ring-2 focus-visible:ring-brand-500 focus-visible:ring-offset-2 dark:focus-visible:ring-offset-gray-950"
        >
          Back to dashboard
        </Link>
      </div>
    </div>
  );
}
```
## 18. `app/loading.tsx` (root)
```tsx
export default function RootLoading() {
  return (
    <div className="flex min-h-screen items-center justify-center">
      <span className="h-10 w-10 animate-spin rounded-full border-2 border-gray-300 border-t-brand-600 motion-reduce:animate-none" />
    </div>
  );
}
```
## 19. `app/error.tsx` (root)
```tsx
"use client";
export default function RootError({
  error,
  reset,
}: {
  error: Error & { digest?: string };
  reset: () => void;
}) {
  return (
    <div className="flex min-h-screen items-center justify-center px-6">
      <div className="max-w-md text-center">
        <h1 className="text-xl font-bold tracking-tight">Something went wrong</h1>
        <p className="mt-2 text-sm text-gray-500 dark:text-gray-400">{error.message}</p>
        <button
          onClick={reset}
          className="mt-6 rounded-lg bg-brand-600 px-5 py-2.5 text-sm font-semibold text-white transition hover:bg-brand-500"
        >
          Reload
        </button>
      </div>
    </div>
  );
}
```
## Everything Demonstrated
| Requirement | Where |
|---|---|
| **Tailwind with Next.js** | `globals.css` imported once in `app/layout.tsx` |
| **App Router styling** | Per-segment `layout.tsx` + `page.tsx` |
| **Server Component** | `Sidebar`, `StatCard`, page shell |
| **Client Component** | `Navbar`, `ThemeToggle`, `LoginForm`, error pages |
| **Layout styling** | `app/dashboard/layout.tsx` — sidebar + navbar + main offset |
| **Page styling** | Each route's `page.tsx` — metadata + Tailwind |
| **Responsive navbar** | Sticky, backdrop blur, mobile menu via `useState` |
| **Dashboard layout** | `md:pl-64` main + fixed sidebar |
| **Auth UI** | `/login` with Server shell + Client form |
| **Loading UI** | `loading.tsx` skeletons at root + dashboard |
| **Error UI** | `error.tsx` with `reset()` at root + dashboard |
| **Not-found UI** | `not-found.tsx` with gradient 404 |
| **SEO-friendly** | `metadata` in layouts/pages, semantic HTML |
| **Dark mode** | v4 `@custom-variant` + `.dark` class + theme script |
| **Theme tokens** | `@theme { --color-brand-* }` |
| **Motion reduce** | `motion-reduce:` on animations throughout |
## Test Checklist
- Visit `/` → redirects to `/dashboard`
- Sidebar visible at `md:+`, hidden on mobile
- Click "☰" on mobile → nav links appear
- Toggle theme → every page flips instantly
- Visit `/dashboard/customers` → table scrolls horizontally on narrow screens
- Slow down network → dashboard `loading.tsx` skeleton shows
- Throw an error in a page → `error.tsx` shows with "Try again"
- Visit `/does-not-exist` → `not-found.tsx` renders
- Visit `/login` → form works, redirects to `/dashboard` after 1.2s
- Lighthouse mobile → SEO score high
