# Tailwind + React
## Conditional classes
Toggle classes based on state. Cleanest with template literals or a small helper.
```
<button className={`px-4 py-2 rounded-lg ${active ? "bg-indigo-600 text-white" : "bg-gray-100 text-gray-700"}`}>
  Toggle
</button>
```
For strings with spaces, prefer `clsx` or `cn` (clsx + tailwind-merge) to avoid duplicate classes winning by accident.
## Dynamic classes
**Bad:** string concatenation of class fragments breaks Tailwind's scanner.
```
// ❌ Tailwind can't detect these at build time
<div className={`bg-${color}-500`} />
```
**Good:** map to complete class strings.
```
const toneMap = {
  info: "bg-blue-50 text-blue-700 border-blue-200",
  success: "bg-emerald-50 text-emerald-700 border-emerald-200",
  error: "bg-red-50 text-red-700 border-red-200",
};
<div className={toneMap[tone]} />
```
## Reusable components
One component = one file. Accept `className` so callers can extend styles.
```
export function Card({ className = "", children, ...rest }) {
  return (
    <div className={`rounded-2xl border border-gray-200 bg-white p-6 shadow-sm ${className}`} {...rest}>
      {children}
    </div>
  );
}
```
## Button component
Variants + sizes + states, all mapped to static class strings.
```jsx
export function Button({
  variant = "primary",
  size = "md",
  loading = false,
  className = "",
  children,
  ...rest
}) {
  const variants = {
    primary:   "bg-indigo-600 text-white hover:bg-indigo-500",
    secondary: "border border-gray-300 bg-white text-gray-800 hover:bg-gray-50",
    ghost:     "text-gray-700 hover:bg-gray-100",
    danger:    "bg-red-600 text-white hover:bg-red-500",
  };
  const sizes = {
    sm: "px-3 py-1.5 text-xs",
    md: "px-4 py-2.5 text-sm",
    lg: "px-5 py-3 text-base",
  };
  return (
    <button
      disabled={loading || rest.disabled}
      className={`
        inline-flex items-center justify-center gap-2 rounded-lg font-semibold
        transition-all focus:outline-none focus-visible:ring-2 focus-visible:ring-indigo-400 focus-visible:ring-offset-2
        disabled:opacity-50 disabled:cursor-not-allowed
        ${variants[variant]} ${sizes[size]} ${className}
      `}
      {...rest}
    >
      {loading && <span className="h-4 w-4 animate-spin rounded-full border-2 border-white/40 border-t-white" />}
      {children}
    </button>
  );
}
```
## Card component
Simple shell + subcomponents for header/body/footer.
```jsx
export function Card({ className = "", children, ...rest }) {
  return <div className={`rounded-2xl border border-gray-200 bg-white shadow-sm ${className}`} {...rest}>{children}</div>;
}
Card.Header = ({ children }) => <div className="border-b border-gray-200 p-5 font-semibold">{children}</div>;
Card.Body   = ({ children }) => <div className="p-5">{children}</div>;
Card.Footer = ({ children }) => <div className="border-t border-gray-200 p-5">{children}</div>;
```
## Modal component
Fixed inset-0, centered, backdrop click to close, Escape key, body scroll lock.
```jsx
"use client";
import { useEffect } from "react";

export function Modal({ open, onClose, title, children }) {
  useEffect(() => {
    function onKey(e) { if (e.key === "Escape") onClose(); }
    if (open) document.addEventListener("keydown", onKey);
    return () => document.removeEventListener("keydown", onKey);
  }, [open, onClose]);
  if (!open) return null;
  return (
    <div className="fixed inset-0 z-50 flex items-center justify-center p-4">
      <div onClick={onClose} className="absolute inset-0 bg-black/50 backdrop-blur-sm" />
      <div role="dialog" aria-modal="true" className="relative z-10 w-full max-w-md rounded-2xl bg-white p-6 shadow-2xl dark:bg-gray-900">
        <div className="flex items-start justify-between">
          <h2 className="text-lg font-bold">{title}</h2>
          <button onClick={onClose} aria-label="Close" className="rounded-full p-1 text-gray-400 hover:bg-gray-100 hover:text-gray-700">✕</button>
        </div>
        <div className="mt-4">{children}</div>
      </div>
    </div>
  );
}
```
## Navbar component
Sticky, backdrop-blur, mobile menu toggle, responsive links.
```jsx
"use client";
import { useState } from "react";
export function Navbar({ links = [] }) {
  const [open, setOpen] = useState(false);
  return (
    <header className="sticky top-0 z-40 border-b border-gray-200 bg-white/80 backdrop-blur dark:border-gray-800 dark:bg-gray-950/80">
      <nav className="mx-auto flex max-w-6xl items-center justify-between px-6 py-4">
        <span className="font-bold tracking-tight">Brand</span>
        <div className="hidden md:flex gap-6 text-sm">
          {links.map((l) => (
            <a key={l.href} href={l.href} className="text-gray-600 hover:text-gray-900 dark:text-gray-400 dark:hover:text-gray-100 transition-colors">
              {l.label}
            </a>
          ))}
        </div>
        <button className="md:hidden text-2xl" onClick={() => setOpen(!open)} aria-label="Menu">
          {open ? "✕" : "☰"}
        </button>
      </nav>
      {open && (
        <div className="md:hidden border-t border-gray-200 px-6 py-4 flex flex-col gap-3 text-sm dark:border-gray-800">
          {links.map((l) => <a key={l.href} href={l.href}>{l.label}</a>)}
        </div>
      )}
    </header>
  );
}
```
## Sidebar component
Fixed on desktop, toggled drawer on mobile.
```jsx
export function Sidebar({ items = [], active = "" }) {
  return (
    <aside className="hidden md:flex fixed left-0 top-0 h-screen w-64 flex-col border-r border-gray-200 bg-white dark:border-gray-800 dark:bg-gray-900">
      <div className="h-16 flex items-center px-6 font-bold border-b border-gray-200 dark:border-gray-800">
        Brand
      </div>
      <nav className="flex-1 p-3 space-y-1">
        {items.map((it) => (
          <a
            key={it.href}
            href={it.href}
            className={`flex items-center gap-3 rounded-lg px-3 py-2 text-sm font-medium transition
              ${active === it.href
                ? "bg-indigo-50 text-indigo-700 dark:bg-indigo-500/10 dark:text-indigo-400"
                : "text-gray-600 hover:bg-gray-100 dark:text-gray-400 dark:hover:bg-gray-800"}`}
          >
            {it.label}
          </a>
        ))}
      </nav>
    </aside>
  );
}
```
## Form components
Wrap label + input + helper into one `Field`.
```jsx
export function Field({ id, label, error, children }) {
  return (
    <div>
      <label htmlFor={id} className="block text-sm font-medium text-gray-700 dark:text-gray-300">{label}</label>
      <div className="mt-1.5">{children}</div>
      {error && <p className="mt-1.5 text-xs text-red-600 dark:text-red-400">{error}</p>}
    </div>
  );
}
export function Input({ invalid, className = "", ...rest }) {
  const base = "w-full rounded-lg border bg-white px-3.5 py-2.5 text-sm focus:outline-none focus:ring-2 dark:bg-gray-900 dark:text-gray-100";
  const tone = invalid
    ? "border-red-500 focus:border-red-500 focus:ring-red-200"
    : "border-gray-300 focus:border-indigo-500 focus:ring-indigo-200 dark:border-gray-700";
  return <input className={`${base} ${tone} ${className}`} aria-invalid={invalid} {...rest} />;
}
```
## Badge component
Small pill, tones + optional dot.
```jsx
export function Badge({ tone = "neutral", dot = false, children }) {
  const tones = {
    neutral: "bg-gray-100 text-gray-700 dark:bg-gray-800 dark:text-gray-300",
    brand:   "bg-indigo-50 text-indigo-700 dark:bg-indigo-500/10 dark:text-indigo-400",
    success: "bg-emerald-50 text-emerald-700 dark:bg-emerald-500/10 dark:text-emerald-400",
    warning: "bg-amber-50 text-amber-700 dark:bg-amber-500/10 dark:text-amber-400",
    error:   "bg-red-50 text-red-700 dark:bg-red-500/10 dark:text-red-400",
  };
  return (
    <span className={`inline-flex items-center gap-1.5 rounded-full px-2.5 py-1 text-xs font-semibold ${tones[tone]}`}>
      {dot && <span className="h-1.5 w-1.5 rounded-full bg-current" />}
      {children}
    </span>
  );
}
```
## Alert component
Banner with icon + title + body, `role="alert"`.
```jsx
export function Alert({ tone = "info", title, children }) {
  const tones = {
    info:    "border-blue-200 bg-blue-50 text-blue-800 dark:border-blue-500/30 dark:bg-blue-500/10 dark:text-blue-300",
    success: "border-emerald-200 bg-emerald-50 text-emerald-800 dark:border-emerald-500/30 dark:bg-emerald-500/10 dark:text-emerald-300",
    warning: "border-amber-200 bg-amber-50 text-amber-800 dark:border-amber-500/30 dark:bg-amber-500/10 dark:text-amber-300",
    error:   "border-red-200 bg-red-50 text-red-800 dark:border-red-500/30 dark:bg-red-500/10 dark:text-red-300",
  };
  return (
    <div role="alert" className={`rounded-lg border p-4 ${tones[tone]}`}>
      {title && <p className="text-sm font-semibold">{title}</p>}
      <p className="mt-0.5 text-sm opacity-90">{children}</p>
    </div>
  );
}
```
## Loading component
Skeleton block, spinner, or full-page loader.
```jsx
export function Skeleton({ className = "" }) {
  return <div className={`animate-pulse rounded-md bg-gray-200 dark:bg-gray-800 ${className}`} />;
}
export function Spinner({ size = "md" }) {
  const sizes = { sm: "h-4 w-4", md: "h-6 w-6", lg: "h-10 w-10" };
  return <span className={`inline-block ${sizes[size]} animate-spin rounded-full border-2 border-gray-300 border-t-indigo-600`} />;
}
export function PageLoader() {
  return (
    <div className="fixed inset-0 z-50 flex items-center justify-center bg-white/60 backdrop-blur-sm dark:bg-gray-950/60">
      <Spinner size="lg" />
    </div>
  );
}
```
## Responsive component architecture
Patterns that keep components predictable at any size:
**1. Mobile-first props.** Accept a `size` or `direction` prop; default to mobile-safe values.
```jsx
<Stack direction={{ base: "col", md: "row" }} gap="4">
```
Implement with a small map from prop → class.
**2. `className` escape hatch.** Every component accepts `className` and appends it last so callers can override.
**3. Slot subcomponents** (`Card.Header`, `Card.Body`) instead of 10 boolean props.
**4. Compound variants via maps, never string concatenation.**
```
const variants = { primary: "...", secondary: "..." };
```
**5. Use `clsx` + `tailwind-merge` in a `cn()` helper** so overrides don't fight.
```js
// lib/cn.js
import clsx from "clsx";
import { twMerge } from "tailwind-merge";
export const cn = (...args) => twMerge(clsx(args));
```
Then:
```jsx
<button className={cn("px-4 py-2 rounded-lg", variants[variant], className)} />
```
**6. Respect accessibility:** every interactive component ships with `focus-visible:ring-*`, `aria-*`, and `disabled:*` states by default.
**7. Respect motion:** every animated component includes a `motion-reduce:` opt-out.
