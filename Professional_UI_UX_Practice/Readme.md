# Professional UI/UX Practice
A walkthrough of 16 production-grade UI patterns. Each entry: **purpose**, **key Tailwind techniques**, and a compact code sample you can paste.
## 1. Design a navbar
Sticky top bar, backdrop blur, logo left, links center, actions right, mobile drawer.
```tsx
<header className="sticky top-0 z-40 border-b border-gray-200 bg-white/80 backdrop-blur dark:border-gray-800 dark:bg-gray-950/80">
  <nav className="mx-auto flex max-w-6xl items-center justify-between px-6 py-4">
    <a href="/" className="text-lg font-bold tracking-tight">Brand</a>
    <div className="hidden md:flex gap-8 text-sm text-gray-600 dark:text-gray-400">
      <a className="hover:text-gray-900 dark:hover:text-gray-100 transition-colors">Product</a>
      <a className="hover:text-gray-900 dark:hover:text-gray-100 transition-colors">Pricing</a>
      <a className="hover:text-gray-900 dark:hover:text-gray-100 transition-colors">Docs</a>
    </div>
    <div className="flex items-center gap-3">
      <a className="hidden md:inline text-sm text-gray-600 hover:text-gray-900">Sign in</a>
      <a className="rounded-lg bg-indigo-600 px-4 py-2 text-sm font-semibold text-white hover:bg-indigo-500 transition">Get started</a>
    </div>
  </nav>
</header>
```
**UX rules:** sticky but slim (≤ 72px), never more than 5 primary links, always show a clear primary action.
## 2. Design a mega menu
Multi-column dropdown for large sites. Use `group` + `absolute` panel.
```tsx
<div className="group relative">
  <button className="flex items-center gap-1 text-sm font-medium text-gray-700 dark:text-gray-300">
    Products <span className="transition-transform group-hover:rotate-180">▾</span>
  </button>

  <div className="invisible absolute left-0 top-full z-50 mt-2 w-[640px] rounded-2xl border border-gray-200 bg-white p-6 opacity-0 shadow-xl transition-all group-hover:visible group-hover:opacity-100 dark:border-gray-800 dark:bg-gray-900">
    <div className="grid grid-cols-2 gap-6">
      {[
        { title: "Analytics", desc: "Track usage in real time." },
        { title: "Automation", desc: "Ship workflows without code." },
        { title: "Security", desc: "SOC2-ready infrastructure." },
        { title: "Integrations", desc: "Connect 100+ tools." },
      ].map((i) => (
        <a key={i.title} className="rounded-lg p-3 hover:bg-gray-50 dark:hover:bg-gray-800 transition">
          <p className="font-semibold text-sm">{i.title}</p>
          <p className="mt-1 text-xs text-gray-500 dark:text-gray-400">{i.desc}</p>
        </a>
      ))}
    </div>
  </div>
</div>
```
**UX rules:** max 2–3 columns, group by purpose, add icons + short descriptions.
## 3. Design a hero section
Background image + gradient overlay + layered content.
```tsx
<section className="relative min-h-[80vh] flex items-center overflow-hidden">
  <div className="absolute inset-0 bg-cover bg-center bg-[url('/hero.jpg')]" />
  <div className="absolute inset-0 bg-gradient-to-b from-black/70 via-black/50 to-black/90" />

  <div className="relative z-10 mx-auto max-w-4xl px-6 text-center text-white">
    <span className="inline-flex items-center gap-2 rounded-full border border-white/20 bg-white/10 px-4 py-1.5 text-xs uppercase tracking-widest backdrop-blur">
      <span className="h-1.5 w-1.5 rounded-full bg-emerald-400 animate-pulse" /> Now live
    </span>
    <h1 className="mt-6 text-4xl md:text-6xl font-extrabold tracking-tight leading-[1.05]">
      Ship faster with{" "}
      <span className="bg-gradient-to-r from-indigo-400 to-purple-400 bg-clip-text text-transparent">confidence</span>
    </h1>
    <p className="mt-6 max-w-2xl mx-auto text-base md:text-lg text-gray-300">
      Everything your team needs to build, test, and deploy — in one platform.
    </p>
    <div className="mt-10 flex flex-col sm:flex-row justify-center gap-4">
      <button className="rounded-xl bg-white px-7 py-3.5 text-sm font-semibold text-gray-900 hover:bg-gray-100 transition">Start free</button>
      <button className="rounded-xl border border-white/20 bg-white/5 px-7 py-3.5 text-sm font-semibold text-white backdrop-blur hover:bg-white/10 transition">Watch demo</button>
    </div>
  </div>
</section>
```
**UX rules:** one headline, one subhead, two CTAs (primary + secondary), social proof below.
## 4. Design feature cards
Icon top, title, description, optional link. Hover lift + shadow.
```tsx
<div className="grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-3 gap-6">
  {features.map((f) => (
    <div key={f.title} className="group rounded-2xl border border-gray-200 bg-white p-6 shadow-sm transition hover:-translate-y-1 hover:shadow-lg hover:border-indigo-200 dark:border-gray-800 dark:bg-gray-900 dark:shadow-none dark:hover:border-indigo-500/30 motion-reduce:transition-none motion-reduce:hover:translate-y-0">
      <div className="flex h-10 w-10 items-center justify-center rounded-lg bg-indigo-50 text-indigo-600 dark:bg-indigo-500/10 dark:text-indigo-400">
        {f.icon}
      </div>
      <h3 className="mt-4 font-semibold transition-colors group-hover:text-indigo-600 dark:group-hover:text-indigo-400">{f.title}</h3>
      <p className="mt-2 text-sm text-gray-500 dark:text-gray-400">{f.desc}</p>
    </div>
  ))}
</div>
```
**UX rules:** 3 or 6 cards max per row, icon + 3-word title + one-line description.
## 5. Design pricing cards
3 tiers, middle highlighted, badge on top, feature list, CTA.
```tsx
<div className="grid grid-cols-1 md:grid-cols-3 gap-6 items-stretch">
  {plans.map((p) => (
    <div key={p.name} className={`relative flex flex-col rounded-2xl p-8 ${
      p.highlighted
        ? "bg-gray-900 text-gray-100 ring-2 ring-indigo-500 shadow-2xl md:-mt-4 md:mb-4"
        : "border border-gray-200 bg-white dark:border-gray-800 dark:bg-gray-900"
    }`}>
      {p.highlighted && (
        <span className="absolute -top-3 left-1/2 -translate-x-1/2 rounded-full bg-gradient-to-r from-indigo-500 to-purple-500 px-3 py-1 text-xs font-semibold uppercase tracking-widest text-white ring-2 ring-white">
          Most Popular
        </span>
      )}
      <h3 className="text-lg font-semibold">{p.name}</h3>
      <p className={`mt-1 text-sm ${p.highlighted ? "text-gray-400" : "text-gray-500 dark:text-gray-400"}`}>{p.tagline}</p>
      <p className="mt-6 text-5xl font-extrabold">{p.price}<span className="text-sm font-medium opacity-60">/mo</span></p>
      <ul className="mt-6 space-y-3 text-sm flex-1">
        {p.features.map((f) => (
          <li key={f} className="flex items-center gap-3">
            <span className={`flex h-5 w-5 items-center justify-center rounded-full text-[10px] font-bold ${p.highlighted ? "bg-indigo-500 text-white" : "bg-indigo-50 text-indigo-600 dark:bg-indigo-500/10 dark:text-indigo-400"}`}>✓</span>
            {f}
          </li>
        ))}
      </ul>
      <button className={`mt-8 w-full rounded-xl px-5 py-3 text-sm font-semibold transition ${
        p.highlighted ? "bg-indigo-600 text-white hover:bg-indigo-500" : "bg-gray-900 text-white hover:bg-gray-800 dark:bg-gray-100 dark:text-gray-900"
      }`}>{p.cta}</button>
    </div>
  ))}
</div>
```
**UX rules:** anchor the middle tier, always show a "most popular" badge, 4–6 features per plan.
## 6. Design testimonials
Quote cards with avatar, name, role, optional company logo.
```tsx
<div className="grid grid-cols-1 md:grid-cols-3 gap-6">
  {testimonials.map((t) => (
    <figure key={t.name} className="rounded-2xl border border-gray-200 bg-white p-6 shadow-sm dark:border-gray-800 dark:bg-gray-900 dark:shadow-none">
      <div className="flex gap-1 text-amber-500 text-sm">{"★★★★★"}</div>
      <blockquote className="mt-4 text-sm text-gray-700 dark:text-gray-300 leading-relaxed">"{t.quote}"</blockquote>
      <figcaption className="mt-6 flex items-center gap-3">
        <img src={t.avatar} className="h-10 w-10 rounded-full object-cover" alt="" />
        <div>
          <p className="text-sm font-semibold">{t.name}</p>
          <p className="text-xs text-gray-500 dark:text-gray-400">{t.role}</p>
        </div>
      </figcaption>
    </figure>
  ))}
</div>
```
**UX rules:** real names + photos, one meaningful quote, no more than 3 lines.
## 7. Design FAQ
Accordion with `data-state` variant. Uses `<details>` or React state.
```tsx
<div className="divide-y divide-gray-200 dark:divide-gray-800 rounded-2xl border border-gray-200 bg-white dark:border-gray-800 dark:bg-gray-900">
  {faqs.map((f) => (
    <details key={f.q} className="group">
      <summary className="flex cursor-pointer items-center justify-between p-5 text-sm font-medium">
        {f.q}
        <span className="transition-transform group-open:rotate-180">▾</span>
      </summary>
      <p className="px-5 pb-5 text-sm text-gray-600 dark:text-gray-400">{f.a}</p>
    </details>
  ))}
</div>
```
**UX rules:** 6–10 questions, answer in 2–3 sentences, use native `<details>` for zero-JS.
## 8. Design CTA section
Bold band, gradient, single action. Last nudge before footer.

```tsx
<section className="px-6 py-16 lg:py-24">
  <div className="mx-auto max-w-3xl text-center rounded-3xl bg-gradient-to-br from-indigo-600 to-purple-600 p-12 text-white shadow-xl">
    <h2 className="text-2xl md:text-4xl font-extrabold tracking-tight">Ready to ship faster?</h2>
    <p className="mt-3 text-sm md:text-base opacity-90">Join thousands of teams building with us.</p>
    <div className="mt-8 flex flex-col sm:flex-row justify-center gap-3">
      <button className="rounded-xl bg-white px-6 py-3 text-sm font-semibold text-indigo-700 hover:bg-gray-100 transition">Start free</button>
      <button className="rounded-xl border border-white/30 px-6 py-3 text-sm font-semibold text-white hover:bg-white/10 transition">Talk to sales</button>
    </div>
  </div>
</section>
```
**UX rules:** one idea, one primary CTA, contrast against surrounding sections.
## 9. Design footer
Columns for links, newsletter, social, legal row.
```tsx
<footer className="border-t border-gray-200 bg-white dark:border-gray-800 dark:bg-gray-950">
  <div className="mx-auto max-w-6xl px-6 py-12 grid grid-cols-2 md:grid-cols-5 gap-8">
    <div className="col-span-2">
      <p className="font-bold tracking-tight">Brand</p>
      <p className="mt-2 text-sm text-gray-500 dark:text-gray-400 max-w-xs">Build, ship, and scale — all in one platform.</p>
      <div className="mt-4 flex gap-3 text-sm text-gray-400">
        <a>Twitter</a><a>GitHub</a><a>LinkedIn</a>
      </div>
    </div>
    {["Product", "Company", "Resources"].map((col) => (
      <div key={col}>
        <p className="text-xs uppercase tracking-widest text-gray-400 font-semibold">{col}</p>
        <ul className="mt-3 space-y-2 text-sm text-gray-600 dark:text-gray-400">
          <li><a className="hover:text-gray-900 dark:hover:text-gray-100 transition-colors">Link</a></li>
          <li><a className="hover:text-gray-900 dark:hover:text-gray-100 transition-colors">Link</a></li>
          <li><a className="hover:text-gray-900 dark:hover:text-gray-100 transition-colors">Link</a></li>
        </ul>
      </div>
    ))}
  </div>
  <div className="border-t border-gray-200 py-6 dark:border-gray-800">
    <p className="mx-auto max-w-6xl px-6 text-xs text-gray-400 dark:text-gray-500">
      © 2025 Brand Inc. All rights reserved.
    </p>
  </div>
</footer>
```
**UX rules:** 3–4 link columns max, group by theme, legal row separated.
## 10. Design dashboard
Sidebar + topbar + stat grid + chart + activity. Already built in earlier projects.
```tsx
<div className="grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-4 gap-4">
  {stats.map((s) => <StatCard key={s.label} {...s} />)}
</div>
<div className="grid grid-cols-1 lg:grid-cols-3 gap-6 mt-6">
  <div className="lg:col-span-2 rounded-2xl border bg-white p-6 dark:border-gray-800 dark:bg-gray-900">Chart</div>
  <div className="rounded-2xl border bg-white p-6 dark:border-gray-800 dark:bg-gray-900">Activity</div>
</div>
```
**UX rules:** KPI cards first, charts second, activity/actions third.
## 11. Design sidebar
Fixed desktop rail, active state highlight, user card at bottom.
```tsx
<aside className="hidden md:flex fixed left-0 top-0 h-screen w-64 flex-col border-r border-gray-200 bg-white dark:border-gray-800 dark:bg-gray-900">
  <div className="flex h-16 items-center border-b border-gray-200 px-6 font-bold dark:border-gray-800">Brand</div>
  <nav className="flex-1 space-y-1 p-3">
    {items.map((it) => (
      <a key={it.href} className={`flex items-center gap-3 rounded-lg px-3 py-2 text-sm font-medium transition ${
        active === it.href
          ? "bg-indigo-50 text-indigo-700 dark:bg-indigo-500/10 dark:text-indigo-400"
          : "text-gray-600 hover:bg-gray-100 dark:text-gray-400 dark:hover:bg-gray-800"
      }`}>{it.icon}<span>{it.label}</span></a>
    ))}
  </nav>
  <div className="border-t border-gray-200 p-4 dark:border-gray-800">
    {/* user card */}
  </div>
</aside>
```
**UX rules:** 5–7 top-level items max, icon + label, active state obvious, user at bottom.
## 12. Design data table
Header row, hover rows, status pills, scroll on mobile.
```tsx
<div className="overflow-hidden rounded-2xl border border-gray-200 bg-white dark:border-gray-800 dark:bg-gray-900">
  <div className="overflow-x-auto">
    <table className="min-w-full divide-y divide-gray-200 text-sm dark:divide-gray-800">
      <thead className="bg-gray-50 dark:bg-gray-950">
        <tr>
          {["Name", "Email", "Plan", "Status"].map((h) => (
            <th key={h} className="px-6 py-3 text-left font-medium text-gray-500 dark:text-gray-400">{h}</th>
          ))}
        </tr>
      </thead>
      <tbody className="divide-y divide-gray-100 dark:divide-gray-800">
        {rows.map((r) => (
          <tr key={r.id} className="hover:bg-gray-50 dark:hover:bg-gray-950 transition">
            <td className="px-6 py-4 font-medium">{r.name}</td>
            <td className="px-6 py-4 text-gray-600 dark:text-gray-400">{r.email}</td>
            <td className="px-6 py-4 text-gray-600 dark:text-gray-400">{r.plan}</td>
            <td className="px-6 py-4">
              <span className="inline-flex items-center rounded-full bg-emerald-50 px-2.5 py-1 text-xs font-semibold text-emerald-700 dark:bg-emerald-500/10 dark:text-emerald-400">
                Active
              </span>
            </td>
          </tr>
        ))}
      </tbody>
    </table>
  </div>
</div>
```
**UX rules:** left-align text, right-align numbers, hover row, status pills, always scrollable on mobile.
## 13. Design notification panel
Dropdown anchored top-right, divided list, unread indicator.
```tsx
<div className="relative">
  <button className="relative rounded-lg border border-gray-200 bg-white px-3 py-2 dark:border-gray-700 dark:bg-gray-900">
    🔔
    <span className="absolute -top-1 -right-1 flex h-4 w-4 items-center justify-center rounded-full bg-red-500 text-[10px] font-bold text-white ring-2 ring-white">3</span>
  </button>
  <div className="absolute right-0 top-full z-50 mt-2 w-80 rounded-xl border border-gray-200 bg-white shadow-2xl dark:border-gray-800 dark:bg-gray-900">
    <div className="border-b border-gray-100 p-4 text-sm font-semibold dark:border-gray-800">Notifications</div>
    <ul className="max-h-80 overflow-y-auto divide-y divide-gray-100 dark:divide-gray-800">
      {items.map((n) => (
        <li key={n.id} className="flex gap-3 p-4 hover:bg-gray-50 dark:hover:bg-gray-800/50 transition">
          <span className={`mt-1.5 h-2 w-2 shrink-0 rounded-full ${n.read ? "bg-transparent" : "bg-indigo-500"}`} />
          <div>
            <p className="text-sm">{n.text}</p>
            <p className="mt-0.5 text-xs text-gray-400">{n.time}</p>
          </div>
        </li>
      ))}
    </ul>
  </div>
</div>
```
**UX rules:** show count badge, unread dot, cap height with scroll, mark-as-read action.
## 14. Design profile page
Avatar + info card + editable fields.
```tsx
<div className="space-y-6">
  <div className="rounded-2xl border border-gray-200 bg-white p-6 dark:border-gray-800 dark:bg-gray-900">
    <div className="flex items-center gap-6">
      <img src="/avatar.jpg" className="h-20 w-20 rounded-full object-cover ring-2 ring-white dark:ring-gray-800" alt="" />
      <div>
        <h1 className="text-xl font-bold">Ali Raza</h1>
        <p className="text-sm text-gray-500 dark:text-gray-400">Product Engineer · Karachi</p>
        <div className="mt-3 flex gap-3">
          <button className="rounded-lg bg-indigo-600 px-4 py-2 text-sm font-semibold text-white hover:bg-indigo-500 transition">Edit profile</button>
          <button className="rounded-lg border border-gray-300 px-4 py-2 text-sm font-medium dark:border-gray-700">Share</button>
        </div>
      </div>
    </div>
  </div>
  <div className="rounded-2xl border border-gray-200 bg-white p-6 dark:border-gray-800 dark:bg-gray-900">
    <h2 className="font-semibold">About</h2>
    <p className="mt-2 text-sm text-gray-600 dark:text-gray-400">Short bio here…</p>
  </div>
</div>
```
**UX rules:** big avatar, name + role prominent, primary action visible, sections separated.
## 15. Design settings page
Vertical tabs or sections, one card per group, save button per card.
```tsx
<div className="space-y-6">
  <div className="grid grid-cols-1 md:grid-cols-4 gap-6">
    <aside className="md:col-span-1 space-y-1 text-sm">
      {["Profile", "Account", "Notifications", "Billing", "Security"].map((s, i) => (
        <a key={s} className={`block rounded-lg px-3 py-2 transition ${i === 0
          ? "bg-indigo-50 text-indigo-700 dark:bg-indigo-500/10 dark:text-indigo-400 font-medium"
          : "text-gray-600 hover:bg-gray-100 dark:text-gray-400 dark:hover:bg-gray-800"}`}>{s}</a>
      ))}
    </aside>
    <div className="md:col-span-3 space-y-6">
      <div className="rounded-2xl border border-gray-200 bg-white p-6 dark:border-gray-800 dark:bg-gray-900">
        <h2 className="font-semibold">Profile information</h2>
        <div className="mt-4 grid grid-cols-1 sm:grid-cols-2 gap-4">
          <input className="rounded-lg border border-gray-300 px-3.5 py-2.5 text-sm dark:border-gray-700 dark:bg-gray-950" defaultValue="Ali Raza" />
          <input className="rounded-lg border border-gray-300 px-3.5 py-2.5 text-sm dark:border-gray-700 dark:bg-gray-950" defaultValue="ali@example.com" />
        </div>
        <div className="mt-6 flex justify-end">
          <button className="rounded-lg bg-indigo-600 px-4 py-2 text-sm font-semibold text-white hover:bg-indigo-500 transition">Save changes</button>
        </div>
      </div>
    </div>
  </div>
</div>
```
**UX rules:** left-nav for settings, one form per card, save button per card, no global save-all.
## 16. Design authentication pages
Split layout: image left, form right (or centered card on mobile).
```tsx
<main className="grid min-h-screen grid-cols-1 md:grid-cols-2">
  {/* Left: brand panel (hidden on mobile) */}
  <div className="hidden md:flex flex-col justify-between bg-gradient-to-br from-indigo-600 to-purple-600 p-12 text-white">
    <p className="text-xl font-bold">Brand</p>
    <blockquote className="text-lg leading-relaxed max-w-md">
      "The fastest way we've ever shipped a product."
      <footer className="mt-4 text-sm opacity-80">— Sarah, CTO</footer>
    </blockquote>
    <p className="text-xs opacity-70">© 2025 Brand</p>
  </div>
  {/* Right: form */}
  <div className="flex items-center justify-center bg-gray-50 px-6 py-12 dark:bg-gray-950">
    <div className="w-full max-w-md">
      <h1 className="text-2xl font-bold tracking-tight">Welcome back</h1>
      <p className="mt-1 text-sm text-gray-500 dark:text-gray-400">Sign in to your account.</p>
      <form className="mt-6 space-y-4">
        <input type="email" placeholder="you@example.com" className="w-full rounded-lg border border-gray-300 px-3.5 py-2.5 text-sm dark:border-gray-700 dark:bg-gray-900" />
        <input type="password" placeholder="••••••••" className="w-full rounded-lg border border-gray-300 px-3.5 py-2.5 text-sm dark:border-gray-700 dark:bg-gray-900" />
        <button className="w-full rounded-lg bg-indigo-600 px-5 py-3 text-sm font-semibold text-white hover:bg-indigo-500 transition">Sign in</button>
      </form>
    </div>
  </div>
</main>
```
**UX rules:** brand panel builds trust, form is short, one primary action, always a link to switch (sign in ↔ sign up).
## Cross-Cutting Design Rules
| Rule | Applies to |
|---|---|
| Consistent surface system | page `950`, card `900`, hover `800` (dark) / `50`, `white`, `100` (light) |
| Border over shadow in dark | `dark:shadow-none dark:border-gray-800` |
| One primary CTA per section | navbar, hero, pricing, CTA |
| Mobile-first stacking | every grid starts at `grid-cols-1` |
| Focus-visible on all interactives | `focus-visible:ring-2 focus-visible:ring-indigo-400` |
| Reduced-motion opt-out | `motion-reduce:` on all animated UI |
| Semantic HTML | `<header>`, `<nav>`, `<main>`, `<aside>`, `<footer>`, `<figure>`, `<details>` |
| Consistent radius scale | `rounded-lg` buttons, `rounded-2xl` cards, `rounded-full` pills |
| Consistent spacing scale | 4px base, sections `py-16 lg:py-24`, cards `p-6` |
| Dark-mode tested first | if it works in dark, light is easy |
## README Section (copy-paste)
```md
## Professional UI/UX Patterns
16 production-grade UI patterns, each with Tailwind + Next.js ready code:
1. Navbar — sticky, backdrop blur, mobile drawer
2. Mega menu — 2-column dropdown with hover reveal
3. Hero — full-bleed image + gradient overlay + dual CTA
4. Feature cards — hover lift, icon tile, 3-col grid
5. Pricing cards — 3 tiers, middle highlighted, badge
6. Testimonials — quote card with avatar + stars
7. FAQ — native `<details>` accordion, group-open rotation
8. CTA section — gradient band, single primary action
9. Footer — 4-column links + legal row
10. Dashboard — sidebar + stat grid + chart + activity
11. Sidebar — fixed rail, active state, user card
12. Data table — header, hover rows, status pills, mobile scroll
13. Notification panel — dropdown, unread dot, count badge
14. Profile page — avatar hero + info card
15. Settings page — left-nav + one form per card
16. Authentication — split layout, brand panel left, form right
Common patterns:
- Consistent surface system (light/dark)
- Focus-visible rings everywhere
- motion-reduce opt-out on animations
- Semantic HTML + ARIA where needed
- Mobile-first responsive grids
```
