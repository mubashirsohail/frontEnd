# Complete Light / Dark Dashboard (Tailwind v4)
## 1. `app/globals.css` (must already contain these two lines)
```css
@import "tailwindcss";
@custom-variant dark (&:where(.dark, .dark *));
```
## 2. `app/layout.tsx`
```tsx
import "./globals.css";
export const metadata = { title: "Dashly" };
const themeInit = `
(function () {
  try {
    var t = localStorage.getItem('theme');
    var prefers = window.matchMedia('(prefers-color-scheme: dark)').matches;
    if (t === 'dark' || (!t && prefers)) {
      document.documentElement.classList.add('dark');
    } else {
      document.documentElement.classList.remove('dark');
    }
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
## 3. `app/page.jsx` — Full Dashboard
```jsx
"use client";
import { useEffect, useState } from "react";
export default function Dashboard() {
  const [dark, setDark] = useState(false);
  const [ready, setReady] = useState(false);

  // Read theme class once on mount
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
    <div className="min-h-screen bg-gray-50 text-gray-900 dark:bg-gray-950 dark:text-gray-100">

      <Sidebar />
      <TopNav dark={dark} ready={ready} onToggle={toggleTheme} />

      <main className="md:pl-64">
        <div className="mx-auto max-w-7xl px-6 py-10 space-y-8">
          <PageHeader />
          <StatsRow />
          <ChartAndActivity />
          <InviteForm />
        </div>
      </main>

    </div>
  );
}

/* ================= Sidebar ================= */
function Sidebar() {
  const items = [
    { label: "Overview", icon: "▦", active: true },
    { label: "Reports", icon: "▤" },
    { label: "Customers", icon: "☺" },
    { label: "Products", icon: "◫" },
    { label: "Settings", icon: "⚙" },
  ];

  return (
    <aside className="hidden md:flex fixed left-0 top-0 h-screen w-64 flex-col border-r border-gray-200 bg-white dark:border-gray-800 dark:bg-gray-900">
      <div className="flex h-16 items-center px-6 border-b border-gray-200 dark:border-gray-800">
        <span className="text-lg font-bold tracking-tight">
          Dash<span className="text-indigo-600 dark:text-indigo-400">ly</span>
        </span>
      </div>

      <nav className="flex-1 px-3 py-4 space-y-1">
        {items.map((item) => (
          <button
            key={item.label}
            className={`flex w-full items-center gap-3 rounded-lg px-3 py-2 text-sm font-medium transition ${
              item.active
                ? "bg-indigo-50 text-indigo-700 dark:bg-indigo-500/10 dark:text-indigo-400"
                : "text-gray-600 hover:bg-gray-100 hover:text-gray-900 dark:text-gray-400 dark:hover:bg-gray-800 dark:hover:text-gray-100"
            }`}
          >
            <span className="text-base">{item.icon}</span>
            {item.label}
          </button>
        ))}
      </nav>

      <div className="border-t border-gray-200 p-4 dark:border-gray-800">
        <div className="flex items-center gap-3">
          <span className="flex h-9 w-9 items-center justify-center rounded-full bg-indigo-100 text-sm font-semibold text-indigo-700 dark:bg-indigo-500/20 dark:text-indigo-300">
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

/* ================= Top Nav ================= */
function TopNav({ dark, ready, onToggle }) {
  return (
    <header className="sticky top-0 z-40 border-b border-gray-200 bg-white/80 backdrop-blur dark:border-gray-800 dark:bg-gray-950/80 md:pl-64">
      <div className="mx-auto flex max-w-7xl items-center justify-between px-6 py-4">

        {/* Mobile brand (sidebar is hidden) */}
        <span className="md:hidden font-bold tracking-tight">
          Dash<span className="text-indigo-600 dark:text-indigo-400">ly</span>
        </span>

        {/* Search */}
        <div className="hidden md:flex flex-1 max-w-md">
          <input
            type="text"
            placeholder="Search…"
            className="w-full rounded-lg border border-gray-300 bg-white px-3.5 py-2 text-sm text-gray-900 placeholder:text-gray-400 focus:border-indigo-500 focus:ring-2 focus:ring-indigo-200 focus:outline-none dark:border-gray-700 dark:bg-gray-900 dark:text-gray-100 dark:placeholder:text-gray-500 dark:focus:border-indigo-400 dark:focus:ring-indigo-500/30"
          />
        </div>

        <div className="flex items-center gap-3 ml-auto">
          {/* Notifications */}
          <button
            type="button"
            aria-label="Notifications"
            className="relative rounded-lg border border-gray-200 bg-white px-3 py-2 text-sm hover:bg-gray-50 transition dark:border-gray-700 dark:bg-gray-900 dark:hover:bg-gray-800 focus:outline-none focus-visible:ring-2 focus-visible:ring-indigo-400"
          >
            🔔
            <span className="absolute -top-1 -right-1 flex h-4 w-4 items-center justify-center rounded-full bg-red-500 text-[10px] font-bold text-white ring-2 ring-white dark:ring-gray-950">
              3
            </span>
          </button>

          {/* Theme toggle */}
          <button
            type="button"
            onClick={onToggle}
            aria-label="Toggle theme"
            className="relative flex h-9 w-16 items-center rounded-full border border-gray-300 bg-gray-100 px-1 transition-colors dark:border-gray-700 dark:bg-gray-800 focus:outline-none focus-visible:ring-2 focus-visible:ring-indigo-400"
          >
            <span
              className={`flex h-7 w-7 items-center justify-center rounded-full bg-white text-xs shadow transition-transform duration-300 dark:bg-gray-950 ${
                ready && dark ? "translate-x-7" : "translate-x-0"
              }`}
            >
              {ready && dark ? "🌙" : "☀️"}
            </span>
          </button>
        </div>
      </div>
    </header>
  );
}

/* ================= Page Header ================= */
function PageHeader() {
  return (
    <div className="flex flex-col sm:flex-row sm:items-center sm:justify-between gap-4">
      <div>
        <h1 className="text-2xl md:text-3xl font-bold tracking-tight">Overview</h1>
        <p className="mt-1 text-sm text-gray-500 dark:text-gray-400">
          Welcome back, Ali — here's what's happening today.
        </p>
      </div>
      <button className="w-full sm:w-auto rounded-lg bg-indigo-600 px-4 py-2.5 text-sm font-semibold text-white transition hover:bg-indigo-500 focus:outline-none focus-visible:ring-2 focus-visible:ring-indigo-400">
        + New report
      </button>
    </div>
  );
}

/* ================= Stats ================= */
function StatsRow() {
  const stats = [
    { label: "Users", value: "12,480", delta: "+8.2%", up: true },
    { label: "Revenue", value: "$42.1k", delta: "+12.5%", up: true },
    { label: "Churn", value: "2.4%", delta: "-0.6%", up: false },
    { label: "Sessions", value: "87,203", delta: "+3.1%", up: true },
  ];

  return (
    <section className="grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-4 gap-4">
      {stats.map((s) => (
        <div
          key={s.label}
          className="rounded-2xl border border-gray-200 bg-white p-5 shadow-sm transition hover:-translate-y-0.5 hover:shadow-md dark:border-gray-800 dark:bg-gray-900 dark:shadow-none dark:hover:border-gray-700"
        >
          <p className="text-xs uppercase tracking-widest text-gray-400 dark:text-gray-500">
            {s.label}
          </p>
          <p className="mt-2 text-2xl font-bold">{s.value}</p>
          <p
            className={`mt-1 text-xs font-medium ${
              s.up
                ? "text-emerald-600 dark:text-emerald-400"
                : "text-red-600 dark:text-red-400"
            }`}
          >
            {s.delta}{" "}
            <span className="text-gray-400 dark:text-gray-500">vs last week</span>
          </p>
        </div>
      ))}
    </section>
  );
}

/* ================= Chart + Activity ================= */
function ChartAndActivity() {
  return (
    <section className="grid grid-cols-1 lg:grid-cols-3 gap-6">
      {/* Chart */}
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
                className="w-full rounded-t-md bg-gradient-to-t from-indigo-500 to-purple-500"
              />
              <span className="text-[10px] text-gray-400 dark:text-gray-500">
                {["M", "T", "W", "T", "F", "S", "S"][i]}
              </span>
            </div>
          ))}
        </div>
      </div>

      {/* Activity */}
      <div className="rounded-2xl border border-gray-200 bg-white p-6 shadow-sm dark:border-gray-800 dark:bg-gray-900 dark:shadow-none">
        <h2 className="font-semibold">Recent activity</h2>
        <ul className="mt-4 divide-y divide-gray-100 dark:divide-gray-800">
          {[
            ["Deployed v2.4", "2m ago", "bg-emerald-500"],
            ["New signup", "1h ago", "bg-indigo-500"],
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
  );
}

/* ================= Invite Form ================= */
function InviteForm() {
  return (
    <section className="rounded-2xl border border-gray-200 bg-white p-6 shadow-sm dark:border-gray-800 dark:bg-gray-900 dark:shadow-none">
      <h2 className="font-semibold">Invite a teammate</h2>
      <p className="mt-1 text-sm text-gray-500 dark:text-gray-400">
        They'll get an email with a link to join.
      </p>

      <form onSubmit={(e) => e.preventDefault()} className="mt-4 flex flex-col sm:flex-row gap-3">
        <input
          type="email"
          placeholder="teammate@example.com"
          className="flex-1 rounded-lg border border-gray-300 bg-white px-3.5 py-2.5 text-sm text-gray-900 placeholder:text-gray-400 focus:border-indigo-500 focus:ring-2 focus:ring-indigo-200 focus:outline-none dark:border-gray-700 dark:bg-gray-950 dark:text-gray-100 dark:placeholder:text-gray-500 dark:focus:border-indigo-400 dark:focus:ring-indigo-500/30"
        />
        <button className="rounded-lg bg-gray-900 px-5 py-2.5 text-sm font-semibold text-white transition hover:bg-gray-800 focus:outline-none focus-visible:ring-2 focus-visible:ring-indigo-400 dark:bg-indigo-600 dark:hover:bg-indigo-500">
          Send invite
        </button>
      </form>
    </section>
  );
}
```
