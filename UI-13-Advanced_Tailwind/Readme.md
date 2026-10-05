# Advanced Tailwind
## Arbitrary Properties
Write raw CSS inside a class with `[property:value]` — no config needed.
```
<div className="[mask-image:linear-gradient(to_bottom,black,transparent)]">
<div className="[text-wrap:balance]">
<div className="[scrollbar-width:none]">
```
Use spaces as underscores: `[mask-image:linear-gradient(...)]`.
## Arbitrary Values
Any Tailwind utility accepts a bracket value.
```
<div className="w-[427px] top-[117px] bg-[#1da1f2] text-[13px]">
<div className="grid-cols-[200px_minmax(900px,_1fr)_100px]">
<div className="bg-[url('/hero.jpg')] shadow-[0_35px_60px_-15px_rgba(0,0,0,0.3)]">
```
## Arbitrary Variants
Prefix with `[&...]:` to target any selector/state — no config needed.
```
<ul className="[&>li]:mt-2 [&>li]:border-b">
<div className="[&::-webkit-scrollbar]:hidden">
<div className="[&:nth-child(2)]:bg-red-500">
<div className="[&_p]:leading-relaxed">
<div className="[@supports(display:grid)]:grid">
```
`&` = the element itself. Selector is scoped to that one element's subtree.
## Custom Utilities
Define reusable classes in CSS using `@utility` (v4) — or `@layer utilities` (v3).
**v4:**
```css
/* app/globals.css */
@utility btn {
  display: inline-flex;
  align-items: center;
  gap: 0.5rem;
  padding: 0.625rem 1rem;
  border-radius: 0.75rem;
  font-weight: 600;
}
```
```
<button className="btn bg-indigo-600 text-white">Save</button>
```
**v3:**
```css
@layer utilities {
  .btn { /* same */ }
}
```
## Custom Theme Values
Extend the design system with your own tokens.

**v4 — in CSS:**
```css
@theme {
  --color-brand-50:  #eef2ff;
  --color-brand-500: #6366f1;
  --color-brand-600: #4f46e5;
  --color-brand-700: #4338ca;
  --font-display: "Playfair Display", serif;
  --radius-card: 1.25rem;
  --shadow-glow: 0 10px 40px -10px rgb(99 102 241 / 0.5);
  --breakpoint-3xl: 120rem;
}
```
```
<div className="bg-brand-600 font-display rounded-card shadow-glow">
<div className="hidden 3xl:block">
```

**v3 — in `tailwind.config.js`:**
```js
theme: {
  extend: {
    colors: { brand: { 500: "#6366f1", 600: "#4f46e5" } },
    fontFamily: { display: ["Playfair Display", "serif"] },
  },
}
```
## CSS Variables
Use theme tokens anywhere with `var(--token)`.
```
<div className="bg-[var(--color-brand-600)] text-white">
<div style={{ "--w": "240px" }} className="w-[var(--w)]">
```
Inline style + arbitrary value = per-instance theming.
## Container Queries
Style children based on **parent container width** — not viewport. Enabled by default in v4.
```
<div className="@container">
  <div className="grid grid-cols-1 @md:grid-cols-2 @lg:grid-cols-3 gap-4">
    <div>Card</div>
  </div>
</div>
```
Mark the container with `@container`, then use `@sm:`, `@md:`, `@lg:` inside — they respond to the container's size, not the screen.
## Advanced Responsive Layouts
Combine arbitrary grids + container queries + breakpoints for real-world layouts.
```
<div className="grid grid-cols-[1fr_2fr_1fr] md:grid-cols-[240px_1fr] lg:grid-cols-[280px_1fr_320px] gap-6">
  <aside>Sidebar</aside>
  <main>Content</main>
  <aside className="hidden lg:block">Right rail</aside>
</div>
```
```
<div className="
  grid
  grid-cols-1
  sm:grid-cols-2
  lg:grid-cols-3
  [grid-template-areas:'hero''sidebar''main']
  lg:[grid-template-areas:'hero_hero_hero''sidebar_main_aside']
">
```
## Complex Selectors
Arbitrary variants target any relationship.
```
<ul className="
  [&>li]:border-b [&>li]:py-2
  [&>li:last-child]:border-0
  [&>li:hover]:bg-gray-50
">
```
```
<div className="[&_a]:underline [&_a:hover]:text-indigo-600">
```
```
<form className="[&_input:invalid]:border-red-500 [&_input:focus]:ring-2">
```
## Data Attributes
Style based on `data-*` attributes with `data-*` variants.
```
<div data-active="true" className="data-[active=true]:bg-indigo-600 data-[active=true]:text-white">
```
```
<button data-state="open" className="data-[state=open]:rotate-180 transition-transform">
```
Great for integrating with headless UI libraries (Radix, Headless UI) that emit `data-state`.
## Accessibility Variants
Target users by their interaction mode.
**`aria-*`** — style based on ARIA state.
```
<button aria-pressed="true" className="aria-pressed:bg-indigo-600 aria-pressed:text-white">
<div aria-hidden="true" className="aria-hidden:hidden">
<input aria-invalid="true" className="aria-invalid:border-red-500">
```
**`motion-*`** — reduce motion for users who prefer it.
```
<div className="transition-transform motion-safe:hover:scale-110">
<div className="animate-bounce motion-reduce:animate-none">
```
- `motion-safe:` — only when user **accepts** motion.
- `motion-reduce:` — only when user **requests reduced** motion.
**`rtl:` / `ltr:`** — direction-aware styling.
```
<div className="ml-4 rtl:ml-0 rtl:mr-4">
```
**`forced-colors:`** — Windows high-contrast mode.
```
<button className="forced-colors:border">
```
## Reduced-Motion Variants
Never assume animations are welcome.
```
<div className="
  transition-all duration-300
  hover:-translate-y-1 hover:shadow-xl
  motion-reduce:transition-none motion-reduce:hover:translate-y-0 motion-reduce:hover:shadow-sm
">
```
Rule: if you animate transforms, always add a `motion-reduce:` opt-out.
## Print Styles
`print:` variants apply only when the page is printed or saved as PDF.
```
<header className="print:hidden">
<nav className="print:hidden">
<aside className="print:hidden">
```
```
<article className="text-gray-900 dark:text-gray-900 print:text-black">
<a className="text-indigo-600 print:text-black print:underline">
<div className="shadow-lg print:shadow-none print:border print:border-black">
```
Common print pattern: hide chrome (nav, footer, sidebar), force black text, remove shadows and gradients.
