# Practice Project: Complete Professional SaaS Website
**Goal:** A full multi-section SaaS marketing site with navbar (incl. mega menu), hero, features, pricing, testimonials, FAQ, CTA, footer, plus a login page — all in Next.js App Router + Tailwind v4 with dark mode.
## Project Structure
```
saas-site/
├── app/
│   ├── globals.css
│   ├── layout.tsx
│   ├── page.tsx                 ← landing page
│   ├── not-found.tsx
│   └── login/
│       └── page.tsx
├── components/
│   ├── ThemeToggle.tsx
│   ├── Navbar.tsx
│   ├── MegaMenu.tsx
│   ├── Hero.tsx
│   ├── LogoStrip.tsx
│   ├── Features.tsx
│   ├── HowItWorks.tsx
│   ├── Pricing.tsx
│   ├── Testimonials.tsx
│   ├── FAQ.tsx
│   ├── CTASection.tsx
│   ├── Footer.tsx
│   └── LoginForm.tsx
└── lib/
    └── cn.ts
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

  --breakpoint-3xl: 120rem;
}
```

---

## 2. `lib/cn.ts`

```ts
import clsx, { type ClassValue } from "clsx";
import { twMerge } from "tailwind-merge";

export function cn(...args: ClassValue[]) {
  return twMerge(clsx(args));
}
```

Install once:
```bash
npm install clsx tailwind-merge
```

---

## 3. `app/layout.tsx`

```tsx
import type { Metadata } from "next";
import "./globals.css";

export const metadata: Metadata = {
  title: { default: "Neuron — AI for modern teams", template: "%s · Neuron" },
  description:
    "Deploy production-grade AI models in minutes. Fine-tune, scale, and ship — all from one unified platform.",
  openGraph: {
    type: "website",
    locale: "en_US",
    siteName: "Neuron",
    title: "Neuron — AI for modern teams",
    description: "Ship production AI faster.",
  },
  twitter: { card: "summary_large_image" },
  robots: { index: true, follow: true },
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
      <body className="bg-white text-gray-900 antialiased selection:bg-brand-100 selection:text-brand-700 dark:bg-gray-950 dark:text-gray-100 dark:selection:bg-brand-500/30 dark:selection:text-brand-300">
        {children}
      </body>
    </html>
  );
}
```

---

## 4. `components/ThemeToggle.tsx`

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

---

## 5. `components/MegaMenu.tsx`

```tsx
const groups = [
  {
    title: "Platform",
    items: [
      { name: "Models", desc: "Fine-tune in minutes.", icon: "⚡" },
      { name: "Deployments", desc: "Ship to production.", icon: "🚀" },
      { name: "Monitoring", desc: "Track quality live.", icon: "📊" },
    ],
  },
  {
    title: "Developers",
    items: [
      { name: "API", desc: "REST + SDKs.", icon: "⌘" },
      { name: "Docs", desc: "Guides and references.", icon: "📖" },
      { name: "Changelog", desc: "What shipped this week.", icon: "🗒" },
    ],
  },
];

export function MegaMenu() {
  return (
    <div className="group relative">
      <button
        type="button"
        className="flex items-center gap-1 rounded-lg px-3 py-2 text-sm font-medium text-gray-700 transition hover:bg-gray-100 dark:text-gray-300 dark:hover:bg-gray-800 focus:outline-none focus-visible:ring-2 focus-visible:ring-brand-500"
      >
        Product
        <span className="text-xs transition-transform group-hover:rotate-180">▾</span>
      </button>

      <div
        className="
          invisible absolute left-1/2 top-full z-50 mt-2 w-[640px] -translate-x-1/2
          rounded-2xl border border-gray-200 bg-white p-6 opacity-0 shadow-2xl
          transition-all duration-200
          group-hover:visible group-hover:opacity-100
          dark:border-gray-800 dark:bg-gray-900
        "
      >
        <div className="grid grid-cols-2 gap-6">
          {groups.map((g) => (
            <div key={g.title}>
              <p className="text-xs font-semibold uppercase tracking-widest text-gray-400 dark:text-gray-500">
                {g.title}
              </p>
              <div className="mt-3 space-y-1">
                {g.items.map((it) => (
                  <a
                    key={it.name}
                    href="#"
                    className="flex items-start gap-3 rounded-lg p-3 transition hover:bg-gray-50 dark:hover:bg-gray-800"
                  >
                    <span className="flex h-9 w-9 shrink-0 items-center justify-center rounded-lg bg-brand-50 text-brand-600 dark:bg-brand-500/10 dark:text-brand-400">
                      {it.icon}
                    </span>
                    <span>
                      <span className="block text-sm font-semibold">{it.name}</span>
                      <span className="block text-xs text-gray-500 dark:text-gray-400">
                        {it.desc}
                      </span>
                    </span>
                  </a>
                ))}
              </div>
            </div>
          ))}
        </div>
      </div>
    </div>
  );
}
```

---

## 6. `components/Navbar.tsx`

```tsx
"use client";
import Link from "next/link";
import { useState } from "react";
import { ThemeToggle } from "./ThemeToggle";
import { MegaMenu } from "./MegaMenu";

const links = [
  { href: "#features", label: "Features" },
  { href: "#how", label: "How it works" },
  { href: "#pricing", label: "Pricing" },
  { href: "#faq", label: "FAQ" },
];

export function Navbar() {
  const [open, setOpen] = useState(false);

  return (
    <header className="sticky top-0 z-50 border-b border-gray-200 bg-white/80 backdrop-blur dark:border-gray-800 dark:bg-gray-950/80">
      <nav className="mx-auto flex max-w-7xl items-center justify-between px-6 py-4">
        <Link href="/" className="text-lg font-bold tracking-tight">
          Neur<span className="text-brand-600 dark:text-brand-500">o</span>n
        </Link>

        <div className="hidden items-center gap-1 md:flex">
          <MegaMenu />
          {links.map((l) => (
            <a
              key={l.href}
              href={l.href}
              className="rounded-lg px-3 py-2 text-sm font-medium text-gray-700 transition hover:bg-gray-100 dark:text-gray-300 dark:hover:bg-gray-800"
            >
              {l.label}
            </a>
          ))}
        </div>

        <div className="flex items-center gap-3">
          <div className="hidden md:block">
            <ThemeToggle />
          </div>
          <Link
            href="/login"
            className="hidden rounded-lg px-4 py-2 text-sm font-medium text-gray-700 transition hover:bg-gray-100 md:inline-block dark:text-gray-300 dark:hover:bg-gray-800"
          >
            Sign in
          </Link>
          <Link
            href="/login"
            className="hidden rounded-lg bg-brand-600 px-4 py-2 text-sm font-semibold text-white transition hover:bg-brand-500 md:inline-block focus:outline-none focus-visible:ring-2 focus-visible:ring-brand-500 focus-visible:ring-offset-2 dark:focus-visible:ring-offset-gray-950"
          >
            Get started
          </Link>

          <button
            onClick={() => setOpen((v) => !v)}
            aria-label="Menu"
            className="rounded-lg p-2 transition hover:bg-gray-100 md:hidden dark:hover:bg-gray-800 focus:outline-none focus-visible:ring-2 focus-visible:ring-brand-500"
          >
            {open ? "✕" : "☰"}
          </button>
        </div>
      </nav>

      {/* Mobile menu */}
      {open && (
        <div className="border-t border-gray-200 px-6 py-4 md:hidden dark:border-gray-800">
          <div className="flex flex-col gap-1 text-sm">
            {links.map((l) => (
              <a
                key={l.href}
                href={l.href}
                onClick={() => setOpen(false)}
                className="rounded-lg px-3 py-2 text-gray-700 hover:bg-gray-100 dark:text-gray-300 dark:hover:bg-gray-800"
              >
                {l.label}
              </a>
            ))}
            <div className="mt-3 flex items-center justify-between rounded-lg border border-gray-200 p-2 dark:border-gray-800">
              <span className="text-xs uppercase tracking-widest text-gray-400">Theme</span>
              <ThemeToggle />
            </div>
            <Link
              href="/login"
              onClick={() => setOpen(false)}
              className="mt-2 rounded-lg bg-brand-600 px-4 py-2.5 text-center text-sm font-semibold text-white transition hover:bg-brand-500"
            >
              Get started
            </Link>
          </div>
        </div>
      )}
    </header>
  );
}
```

---

## 7. `components/Hero.tsx`

```tsx
import Link from "next/link";

export function Hero() {
  return (
    <section className="relative overflow-hidden">
      {/* Background gradient */}
      <div className="pointer-events-none absolute inset-0 -z-10">
        <div className="absolute left-1/2 top-0 h-[600px] w-[900px] -translate-x-1/2 rounded-full bg-brand-500/20 blur-3xl dark:bg-brand-500/10" />
        <div className="absolute right-0 top-40 h-[400px] w-[400px] rounded-full bg-purple-500/20 blur-3xl dark:bg-purple-500/10" />
      </div>

      <div className="mx-auto max-w-7xl px-6 py-20 lg:py-32 text-center">
        {/* Badge */}
        <span className="inline-flex items-center gap-2 rounded-full border border-brand-100 bg-brand-50 px-4 py-1.5 text-xs font-medium uppercase tracking-widest text-brand-700 dark:border-brand-500/30 dark:bg-brand-500/10 dark:text-brand-400">
          <span className="h-1.5 w-1.5 animate-pulse rounded-full bg-brand-500 motion-reduce:animate-none" />
          Now in public beta
        </span>

        {/* Headline */}
        <h1 className="mx-auto mt-6 max-w-4xl text-4xl font-extrabold leading-[1.05] tracking-tight sm:text-5xl md:text-6xl lg:text-7xl">
          Build with{" "}
          <span className="bg-gradient-to-r from-brand-500 via-purple-500 to-pink-500 bg-clip-text text-transparent">
            AI that thinks
          </span>{" "}
          like your team
        </h1>

        {/* Subhead */}
        <p className="mx-auto mt-6 max-w-2xl text-base text-gray-600 md:text-lg dark:text-gray-400">
          Deploy production-grade models in minutes. Fine-tune, scale, and ship — all from one
          unified platform built for modern teams.
        </p>

        {/* CTAs */}
        <div className="mt-10 flex flex-col items-center justify-center gap-3 sm:flex-row">
          <Link
            href="/login"
            className="group inline-flex w-full items-center justify-center rounded-xl bg-brand-600 px-7 py-3.5 text-sm font-semibold text-white shadow-lg shadow-brand-500/20 transition-all hover:bg-brand-500 hover:-translate-y-0.5 hover:shadow-xl hover:shadow-brand-500/40 focus:outline-none focus-visible:ring-2 focus-visible:ring-brand-500 focus-visible:ring-offset-2 sm:w-auto dark:focus-visible:ring-offset-gray-950 motion-reduce:transition-none motion-reduce:hover:translate-y-0"
          >
            Start building free
            <span className="ml-2 transition-transform group-hover:translate-x-1 motion-reduce:transition-none">→</span>
          </Link>
          <a
            href="#how"
            className="inline-flex w-full items-center justify-center rounded-xl border border-gray-300 bg-white px-7 py-3.5 text-sm font-semibold text-gray-800 transition hover:bg-gray-50 sm:w-auto dark:border-gray-700 dark:bg-gray-900 dark:text-gray-100 dark:hover:bg-gray-800 focus:outline-none focus-visible:ring-2 focus-visible:ring-brand-500 focus-visible:ring-offset-2 dark:focus-visible:ring-offset-gray-950"
          >
            Watch demo
          </a>
        </div>

        <p className="mt-8 text-xs uppercase tracking-widest text-gray-400 dark:text-gray-500">
          Trusted by 2,000+ engineering teams
        </p>
      </div>
    </section>
  );
}
```

---

## 8. `components/LogoStrip.tsx`

```tsx
const logos = ["Acme", "Nimbus", "Vertex", "Orbit", "Helix", "Lattice"];

export function LogoStrip() {
  return (
    <section className="border-y border-gray-200 bg-gray-50 py-8 dark:border-gray-800 dark:bg-gray-900/50">
      <div className="mx-auto max-w-7xl px-6">
        <div className="flex flex-wrap items-center justify-center gap-x-12 gap-y-4 text-xs font-semibold uppercase tracking-widest text-gray-400 dark:text-gray-500">
          {logos.map((l) => (
            <span key={l}>{l}</span>
          ))}
        </div>
      </div>
    </section>
  );
}
```

---

## 9. `components/Features.tsx`

```tsx
const features = [
  { icon: "⚡", title: "Instant deploys",   desc: "Ship models in seconds with a single command." },
  { icon: "🔒", title: "Secure by default", desc: "SOC2-ready infrastructure and end-to-end encryption." },
  { icon: "📈", title: "Scales to millions", desc: "From prototype to production — no rewrites needed." },
  { icon: "🧠", title: "Fine-tune easily",  desc: "Bring your data, get a custom model in minutes." },
  { icon: "🔍", title: "Live monitoring",   desc: "Track quality, latency, and drift in real time." },
  { icon: "🔌", title: "100+ integrations", desc: "Connect to your stack with a single click." },
];

export function Features() {
  return (
    <section id="features" className="scroll-mt-24 px-6 py-20 lg:py-28">
      <div className="mx-auto max-w-7xl">
        <div className="mx-auto max-w-2xl text-center">
          <p className="text-xs font-semibold uppercase tracking-widest text-brand-600 dark:text-brand-400">
            Features
          </p>
          <h2 className="mt-3 text-3xl font-extrabold tracking-tight md:text-4xl">
            Everything you need to ship AI
          </h2>
          <p className="mt-4 text-base text-gray-600 dark:text-gray-400">
            A complete platform that removes the friction between idea and production.
          </p>
        </div>

        <div className="mt-14 grid grid-cols-1 gap-6 sm:grid-cols-2 lg:grid-cols-3">
          {features.map((f) => (
            <div
              key={f.title}
              className="group rounded-2xl border border-gray-200 bg-white p-6 shadow-sm transition-all hover:-translate-y-1 hover:border-brand-200 hover:shadow-lg dark:border-gray-800 dark:bg-gray-900 dark:shadow-none dark:hover:border-brand-500/30 motion-reduce:transition-none motion-reduce:hover:translate-y-0"
            >
              <div className="flex h-10 w-10 items-center justify-center rounded-lg bg-brand-50 text-lg text-brand-600 dark:bg-brand-500/10 dark:text-brand-400">
                {f.icon}
              </div>
              <h3 className="mt-4 font-semibold transition-colors group-hover:text-brand-600 dark:group-hover:text-brand-400">
                {f.title}
              </h3>
              <p className="mt-2 text-sm text-gray-500 dark:text-gray-400">{f.desc}</p>
            </div>
          ))}
        </div>
      </div>
    </section>
  );
}
```

---

## 10. `components/HowItWorks.tsx`

```tsx
const steps = [
  { n: "01", title: "Connect your data", desc: "Upload datasets or connect to your warehouse." },
  { n: "02", title: "Fine-tune a model", desc: "Pick a base model and let us handle training." },
  { n: "03", title: "Deploy globally",    desc: "One-click deploy to our edge network." },
];

export function HowItWorks() {
  return (
    <section
      id="how"
      className="scroll-mt-24 border-y border-gray-200 bg-gray-50 px-6 py-20 lg:py-28 dark:border-gray-800 dark:bg-gray-900/50"
    >
      <div className="mx-auto max-w-7xl">
        <div className="mx-auto max-w-2xl text-center">
          <p className="text-xs font-semibold uppercase tracking-widest text-brand-600 dark:text-brand-400">
            How it works
          </p>
          <h2 className="mt-3 text-3xl font-extrabold tracking-tight md:text-4xl">
            From idea to production in 3 steps
          </h2>
        </div>

        <div className="mt-14 grid grid-cols-1 gap-6 md:grid-cols-3">
          {steps.map((s, i) => (
            <div key={s.n} className="relative">
              <div className="rounded-2xl border border-gray-200 bg-white p-6 shadow-sm dark:border-gray-800 dark:bg-gray-900 dark:shadow-none">
                <span className="text-2xl font-extrabold text-brand-600 dark:text-brand-400">{s.n}</span>
                <h3 className="mt-3 font-semibold">{s.title}</h3>
                <p className="mt-2 text-sm text-gray-500 dark:text-gray-400">{s.desc}</p>
              </div>
              {i < steps.length - 1 && (
                <span className="pointer-events-none absolute -right-3 top-1/2 hidden -translate-y-1/2 text-gray-300 md:block dark:text-gray-700">
                  →
                </span>
              )}
            </div>
          ))}
        </div>
      </div>
    </section>
  );
}
```

---

## 11. `components/Pricing.tsx`

```tsx
import Link from "next/link";

const plans = [
  {
    name: "Starter",
    tagline: "For individuals getting started.",
    price: "$9",
    features: ["1 project", "5 GB storage", "Community support", "Basic analytics"],
    highlighted: false,
    cta: "Start free",
  },
  {
    name: "Pro",
    tagline: "For growing teams that need more.",
    price: "$29",
    features: ["Unlimited projects", "100 GB storage", "Priority support", "Advanced analytics", "Custom domains"],
    highlighted: true,
    cta: "Get Pro",
  },
  {
    name: "Enterprise",
    tagline: "For organizations at scale.",
    price: "$99",
    features: ["Everything in Pro", "Unlimited storage", "Dedicated manager", "SSO & audit logs"],
    highlighted: false,
    cta: "Contact sales",
  },
];

export function Pricing() {
  return (
    <section id="pricing" className="scroll-mt-24 px-6 py-20 lg:py-28">
      <div className="mx-auto max-w-7xl">
        <div className="mx-auto max-w-2xl text-center">
          <p className="text-xs font-semibold uppercase tracking-widest text-brand-600 dark:text-brand-400">
            Pricing
          </p>
          <h2 className="mt-3 text-3xl font-extrabold tracking-tight md:text-4xl">
            Plans that scale with your team
          </h2>
          <p className="mt-4 text-base text-gray-600 dark:text-gray-400">
            Simple, transparent pricing. Cancel anytime.
          </p>
        </div>

        <div className="mt-14 grid grid-cols-1 items-stretch gap-6 md:grid-cols-3">
          {plans.map((p) => (
            <div
              key={p.name}
              className={`relative flex flex-col rounded-2xl p-8 ${
                p.highlighted
                  ? "bg-gray-900 text-gray-100 ring-2 ring-brand-500 shadow-2xl md:-mt-4 md:mb-4"
                  : "border border-gray-200 bg-white shadow-sm dark:border-gray-800 dark:bg-gray-900 dark:shadow-none"
              }`}
            >
              {p.highlighted && (
                <span className="absolute -top-3 left-1/2 -translate-x-1/2 rounded-full bg-gradient-to-r from-brand-500 to-purple-500 px-3 py-1 text-xs font-semibold uppercase tracking-widest text-white ring-2 ring-white dark:ring-gray-900">
                  Most Popular
                </span>
              )}

              <h3 className="text-lg font-semibold">{p.name}</h3>
              <p className={`mt-1 text-sm ${p.highlighted ? "text-gray-400" : "text-gray-500 dark:text-gray-400"}`}>
                {p.tagline}
              </p>

              <div className="mt-6 flex items-end gap-1">
                <span className="text-5xl font-extrabold tracking-tight">{p.price}</span>
                <span className={`pb-1.5 text-sm ${p.highlighted ? "text-gray-400" : "text-gray-500 dark:text-gray-400"}`}>
                  /month
                </span>
              </div>

              <hr className={`my-6 ${p.highlighted ? "border-gray-700" : "border-gray-200 dark:border-gray-800"}`} />

              <ul className="flex-1 space-y-3 text-sm">
                {p.features.map((f) => (
                  <li key={f} className="flex items-start gap-3">
                    <span
                      className={`mt-0.5 flex h-5 w-5 shrink-0 items-center justify-center rounded-full text-[10px] font-bold ${
                        p.highlighted
                          ? "bg-brand-500 text-white"
                          : "bg-brand-50 text-brand-600 dark:bg-brand-500/10 dark:text-brand-400"
                      }`}
                    >
                      ✓
                    </span>
                    <span className={p.highlighted ? "text-gray-300" : "text-gray-700 dark:text-gray-300"}>{f}</span>
                  </li>
                ))}
              </ul>

              <Link
                href="/login"
                className={`mt-8 inline-flex w-full items-center justify-center rounded-xl px-5 py-3 text-sm font-semibold transition focus:outline-none focus-visible:ring-2 focus-visible:ring-brand-500 focus-visible:ring-offset-2 dark:focus-visible:ring-offset-gray-950 ${
                  p.highlighted
                    ? "bg-brand-600 text-white hover:bg-brand-500"
                    : "bg-gray-900 text-white hover:bg-gray-800 dark:bg-gray-100 dark:text-gray-900 dark:hover:bg-white"
                }`}
              >
                {p.cta}
              </Link>
            </div>
          ))}
        </div>
      </div>
    </section>
  );
}
```

---

## 12. `components/Testimonials.tsx`

```tsx
const testimonials = [
  {
    quote: "We shipped our first AI feature in three days. Unreal.",
    name: "Sarah Khan",
    role: "CTO, Vertex",
  },
  {
    quote: "The fastest deploy pipeline we've ever used. Period.",
    name: "Bilal Ahmed",
    role: "Lead Engineer, Orbit",
  },
  {
    quote: "Our team stopped fighting infra and started shipping.",
    name: "Hina Malik",
    role: "Product Lead, Helix",
  },
];

export function Testimonials() {
  return (
    <section className="border-y border-gray-200 bg-gray-50 px-6 py-20 lg:py-28 dark:border-gray-800 dark:bg-gray-900/50">
      <div className="mx-auto max-w-7xl">
        <div className="mx-auto max-w-2xl text-center">
          <p className="text-xs font-semibold uppercase tracking-widest text-brand-600 dark:text-brand-400">
            Loved by teams
          </p>
          <h2 className="mt-3 text-3xl font-extrabold tracking-tight md:text-4xl">
            What our customers say
          </h2>
        </div>

        <div className="mt-14 grid grid-cols-1 gap-6 md:grid-cols-3">
          {testimonials.map((t) => (
            <figure
              key={t.name}
              className="rounded-2xl border border-gray-200 bg-white p-6 shadow-sm dark:border-gray-800 dark:bg-gray-900 dark:shadow-none"
            >
              <div className="flex gap-0.5 text-sm text-amber-500">{"★★★★★"}</div>
              <blockquote className="mt-4 text-sm leading-relaxed text-gray-700 dark:text-gray-300">
                "{t.quote}"
              </blockquote>
              <figcaption className="mt-6 flex items-center gap-3">
                <span className="flex h-10 w-10 items-center justify-center rounded-full bg-brand-100 text-sm font-semibold text-brand-700 dark:bg-brand-500/20 dark:text-brand-400">
                  {t.name.charAt(0)}
                </span>
                <div>
                  <p className="text-sm font-semibold">{t.name}</p>
                  <p className="text-xs text-gray-500 dark:text-gray-400">{t.role}</p>
                </div>
              </figcaption>
            </figure>
          ))}
        </div>
      </div>
    </section>
  );
}
```

---

## 13. `components/FAQ.tsx`

```tsx
const faqs = [
  {
    q: "Do I need a credit card to start?",
    a: "No — the Starter plan is free forever, no card required.",
  },
  {
    q: "Can I switch plans later?",
    a: "Yes. Upgrade or downgrade at any time. Changes prorate automatically.",
  },
  {
    q: "How does billing work?",
    a: "We bill monthly by default, or annually at a 20% discount.",
  },
  {
    q: "Is my data secure?",
    a: "Yes — SOC2 Type II, end-to-end encryption, and regional data residency.",
  },
  {
    q: "Do you offer a free trial for Pro?",
    a: "Yes — 14 days, full access, no card required.",
  },
  {
    q: "What happens if I cancel?",
    a: "Your account stays active until the end of the billing period, then downgrades to Starter.",
  },
];

export function FAQ() {
  return (
    <section id="faq" className="scroll-mt-24 px-6 py-20 lg:py-28">
      <div className="mx-auto max-w-3xl">
        <div className="text-center">
          <p className="text-xs font-semibold uppercase tracking-widest text-brand-600 dark:text-brand-400">
            FAQ
          </p>
          <h2 className="mt-3 text-3xl font-extrabold tracking-tight md:text-4xl">
            Frequently asked questions
          </h2>
        </div>

        <div className="mt-12 divide-y divide-gray-200 overflow-hidden rounded-2xl border border-gray-200 bg-white dark:divide-gray-800 dark:border-gray-800 dark:bg-gray-900">
          {faqs.map((f) => (
            <details key={f.q} className="group">
              <summary className="flex cursor-pointer items-center justify-between gap-4 p-5 text-sm font-medium transition hover:bg-gray-50 dark:hover:bg-gray-800/50">
                {f.q}
                <span className="shrink-0 text-gray-400 transition-transform group-open:rotate-180">
                  ▾
                </span>
              </summary>
              <p className="px-5 pb-5 text-sm text-gray-600 dark:text-gray-400">{f.a}</p>
            </details>
          ))}
        </div>
      </div>
    </section>
  );
}
```

---

## 14. `components/CTASection.tsx`

```tsx
import Link from "next/link";

export function CTASection() {
  return (
    <section className="px-6 py-20 lg:py-28">
      <div className="mx-auto max-w-5xl overflow-hidden rounded-3xl bg-gradient-to-br from-brand-600 to-purple-600 p-10 text-center text-white shadow-2xl md:p-16">
        <h2 className="text-3xl font-extrabold tracking-tight md:text-4xl">
          Ready to ship faster?
        </h2>
        <p className="mx-auto mt-4 max-w-xl text-sm opacity-90 md:text-base">
          Join thousands of teams building with Neuron. Free to start, no credit card required.
        </p>
        <div className="mt-8 flex flex-col justify-center gap-3 sm:flex-row">
          <Link
            href="/login"
            className="rounded-xl bg-white px-7 py-3.5 text-sm font-semibold text-brand-700 transition hover:bg-gray-100 focus:outline-none focus-visible:ring-2 focus-visible:ring-white focus-visible:ring-offset-2 focus-visible:ring-offset-brand-600"
          >
            Start building free
          </Link>
          <Link
            href="/login"
            className="rounded-xl border border-white/30 px-7 py-3.5 text-sm font-semibold text-white transition hover:bg-white/10 focus:outline-none focus-visible:ring-2 focus-visible:ring-white focus-visible:ring-offset-2 focus-visible:ring-offset-brand-600"
          >
            Talk to sales
          </Link>
        </div>
      </div>
    </section>
  );
}
```

---

## 15. `components/Footer.tsx`

```tsx
import Link from "next/link";

const groups = [
  { title: "Product", links: ["Features", "Pricing", "Docs", "Changelog"] },
  { title: "Company", links: ["About", "Blog", "Careers", "Contact"] },
  { title: "Legal",   links: ["Privacy", "Terms", "Security", "DPA"] },
];

export function Footer() {
  return (
    <footer className="border-t border-gray-200 bg-white dark:border-gray-800 dark:bg-gray-950">
      <div className="mx-auto grid max-w-7xl grid-cols-2 gap-8 px-6 py-12 md:grid-cols-5">
        <div className="col-span-2">
          <p className="text-lg font-bold tracking-tight">
            Neur<span className="text-brand-600 dark:text-brand-500">o</span>n
          </p>
          <p className="mt-2 max-w-xs text-sm text-gray-500 dark:text-gray-400">
            Build, ship, and scale AI — all in one platform.
          </p>
          <div className="mt-4 flex gap-4 text-sm text-gray-400 dark:text-gray-500">
            <a href="#" className="transition hover:text-gray-900 dark:hover:text-gray-100">Twitter</a>
            <a href="#" className="transition hover:text-gray-900 dark:hover:text-gray-100">GitHub</a>
            <a href="#" className="transition hover:text-gray-900 dark:hover:text-gray-100">LinkedIn</a>
          </div>
        </div>

        {groups.map((g) => (
          <div key={g.title}>
            <p className="text-xs font-semibold uppercase tracking-widest text-gray-400 dark:text-gray-500">
              {g.title}
            </p>
            <ul className="mt-3 space-y-2 text-sm text-gray-600 dark:text-gray-400">
              {g.links.map((l) => (
                <li key={l}>
                  <Link href="#" className="transition hover:text-gray-900 dark:hover:text-gray-100">
                    {l}
                  </Link>
                </li>
              ))}
            </ul>
          </div>
        ))}
      </div>

      <div className="border-t border-gray-200 py-6 dark:border-gray-800">
        <div className="mx-auto flex max-w-7xl flex-col items-center justify-between gap-3 px-6 text-xs text-gray-400 dark:text-gray-500 sm:flex-row">
          <p>© 2025 Neuron Inc. All rights reserved.</p>
          <p>Built with Next.js + Tailwind v4</p>
        </div>
      </div>
    </footer>
  );
}
```

---

## 16. `app/page.tsx` — Landing Page

```tsx
import { Navbar } from "@/components/Navbar";
import { Hero } from "@/components/Hero";
import { LogoStrip } from "@/components/LogoStrip";
import { Features } from "@/components/Features";
import { HowItWorks } from "@/components/HowItWorks";
import { Pricing } from "@/components/Pricing";
import { Testimonials } from "@/components/Testimonials";
import { FAQ } from "@/components/FAQ";
import { CTASection } from "@/components/CTASection";
import { Footer } from "@/components/Footer";

export default function HomePage() {
  return (
    <>
      <Navbar />
      <main>
        <Hero />
        <LogoStrip />
        <Features />
        <HowItWorks />
        <Pricing />
        <Testimonials />
        <FAQ />
        <CTASection />
      </main>
      <Footer />
    </>
  );
}
```

---

## 17. `components/LoginForm.tsx`

```tsx
"use client";
import { useState } from "react";
import { useRouter } from "next/navigation";
import Link from "next/link";

export function LoginForm() {
  const router = useRouter();
  const [loading, setLoading] = useState(false);

  function onSubmit(e: React.FormEvent) {
    e.preventDefault();
    setLoading(true);
    setTimeout(() => {
      setLoading(false);
      router.push("/");
    }, 1200);
  }

  return (
    <form onSubmit={onSubmit} className="mt-8 space-y-4" noValidate>
      <div>
        <label htmlFor="email" className="block text-sm font-medium text-gray-700 dark:text-gray-300">
          Email
        </label>
        <input
          id="email"
          type="email"
          placeholder="you@example.com"
          autoComplete="email"
          className="mt-1.5 w-full rounded-lg border border-gray-300 bg-white px-3.5 py-2.5 text-sm text-gray-900 placeholder:text-gray-400 focus:border-brand-500 focus:ring-2 focus:ring-brand-100 focus:outline-none dark:border-gray-700 dark:bg-gray-950 dark:text-gray-100 dark:placeholder:text-gray-500 dark:focus:border-brand-500 dark:focus:ring-brand-500/30"
        />
      </div>

      <div>
        <div className="flex items-center justify-between">
          <label htmlFor="password" className="block text-sm font-medium text-gray-700 dark:text-gray-300">
            Password
          </label>
          <Link href="#" className="text-xs font-medium text-brand-600 hover:text-brand-700 dark:text-brand-400">
            Forgot?
          </Link>
        </div>
        <input
          id="password"
          type="password"
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

---

## 18. `app/login/page.tsx`

```tsx
import Link from "next/link";
import { LoginForm } from "@/components/LoginForm";

export const metadata = { title: "Sign in" };

export default function LoginPage() {
  return (
    <main className="grid min-h-screen grid-cols-1 md:grid-cols-2">
      {/* Brand panel (hidden on mobile) */}
      <div className="relative hidden flex-col justify-between overflow-hidden bg-gradient-to-br from-brand-600 to-purple-600 p-12 text-white md:flex">
        <Link href="/" className="text-xl font-bold tracking-tight">
          Neur<span className="text-white/70">o</span>n
        </Link>

        <blockquote className="max-w-md text-lg leading-relaxed">
          "The fastest way we've ever shipped a product."
          <footer className="mt-4 text-sm opacity-80">— Sarah, CTO at Vertex</footer>
        </blockquote>

        <p className="text-xs opacity-70">© 2025 Neuron Inc.</p>

        {/* Decorative blobs */}
        <div className="pointer-events-none absolute -right-20 -top-20 h-64 w-64 rounded-full bg-white/10 blur-3xl" />
        <div className="pointer-events-none absolute -bottom-20 -left-20 h-72 w-72 rounded-full bg-purple-300/20 blur-3xl" />
      </div>

      {/* Form panel */}
      <div className="flex items-center justify-center bg-gray-50 px-6 py-12 dark:bg-gray-950">
        <div className="w-full max-w-md">
          <div className="text-center md:hidden">
            <Link href="/" className="text-2xl font-extrabold tracking-tight">
              Neur<span className="text-brand-600 dark:text-brand-500">o</span>n
            </Link>
          </div>

          <h1 className="mt-6 text-center text-2xl font-bold tracking-tight md:mt-0 md:text-left">
            Welcome back
          </h1>
          <p className="mt-1 text-center text-sm text-gray-500 md:text-left dark:text-gray-400">
            Sign in to continue to your dashboard.
          </p>

          <LoginForm />

          <p className="mt-6 text-center text-sm text-gray-500 dark:text-gray-400">
            Don't have an account?{" "}
            <Link href="#" className="font-medium text-brand-600 hover:text-brand-700 dark:text-brand-400">
              Sign up
            </Link>
          </p>
        </div>
      </div>
    </main>
  );
}
```

---

## 19. `app/not-found.tsx`

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
          href="/"
          className="mt-6 inline-block rounded-lg bg-brand-600 px-5 py-2.5 text-sm font-semibold text-white transition hover:bg-brand-500"
        >
          Back home
        </Link>
      </div>
    </div>
  );
}
```

---

## Sections Overview

| # | Section | Component | Purpose |
|---|---|---|---|
| 1 | Navbar | `Navbar` | Sticky, mega menu, mobile drawer, theme toggle |
| 2 | Hero | `Hero` | Headline, gradient text, dual CTAs, blurred bg |
| 3 | Logo strip | `LogoStrip` | Social proof of companies |
| 4 | Features | `Features` | 6 cards, hover lift, icon tiles |
| 5 | How it works | `HowItWorks` | 3 numbered steps |
| 6 | Pricing | `Pricing` | 3 tiers, middle highlighted, badge |
| 7 | Testimonials | `Testimonials` | Quotes with avatar initials + stars |
| 8 | FAQ | `FAQ` | Native `<details>` accordion |
| 9 | CTA | `CTASection` | Gradient band with dual CTAs |
| 10 | Footer | `Footer` | 3 link columns + brand + legal row |
| 11 | Login | `LoginPage` | Split layout: brand panel left, form right |
| 12 | 404 | `not-found` | Branded gradient error page |
## Cross-Cutting Techniques
- **Theme tokens** — `@theme { --color-brand-* }` in `globals.css` — no hex repeated anywhere
- **Dark mode** — v4 `@custom-variant` + flash-prevention script in root layout
- **SEO** — `metadata` in layout with title template + OG + Twitter + robots
- **Semantic HTML** — `<header>`, `<main>`, `<section>`, `<figure>`, `<blockquote>`, `<details>`, `<footer>`
- **Accessibility** — `aria-label`, `focus-visible:ring-2` everywhere, keyboard-friendly accordion
- **Reduced motion** — `motion-reduce:transition-none` + opt-out on all transforms
- **Mobile-first** — `grid-cols-1 md:grid-cols-*`, `flex-col sm:flex-row`
- **Consistent radii** — `rounded-lg` for controls, `rounded-2xl` for cards, `rounded-3xl` for hero panels
- **Consistent spacing** — sections `py-20 lg:py-28`, cards `p-6`, containers `max-w-7xl`
## Test Checklist
- **Mobile 375px** — nav collapses, mega menu replaced by list, hero stacks, all sections 1-col
- **Tablet 768px** — 2-col grids, nav links visible
- **Desktop 1440px** — 3-col grids, mega menu opens on hover
- **Theme toggle** — every section flips instantly, no flash on reload
- **Keyboard** — Tab through navbar, mega menu, CTAs, FAQ; every focus ring visible
- **Anchor links** — Features, How it works, Pricing, FAQ all scroll smoothly
- **`/login`** — split layout renders, submit shows spinner, redirects to `/`
- **404** — visit `/anything-else` → branded 404
- **Lighthouse** — mobile SEO + Accessibility green
## README Section (copy-paste)
```md
## Practice: Complete Professional SaaS Website
A full marketing site built with Next.js App Router + Tailwind v4.
Sections:
- Navbar (sticky, mega menu, mobile drawer, theme toggle)
- Hero (gradient text, blurred glow backgrounds, dual CTA)
- Logo strip (social proof)
- Features (6 cards, hover lift, icon tiles)
- How it works (3 numbered steps)
- Pricing (3 tiers, highlighted middle, badge)
- Testimonials (stars + avatar initials)
- FAQ (native <details> accordion)
- CTA section (gradient band)
- Footer (3 link columns + legal row)
Pages:
- / — landing
- /login — split-layout auth page with brand panel
- not-found.tsx — branded 404
Built with:
- Tailwind v4 @theme design tokens (brand palette + custom breakpoint)
- Class-based dark mode via @custom-variant
- Flash-prevention inline script in root layout
- SEO metadata with title template + OG + Twitter
- Mobile-first responsive layouts throughout
- focus-visible rings + motion-reduce opt-outs
- Semantic HTML + ARIA where needed
```
