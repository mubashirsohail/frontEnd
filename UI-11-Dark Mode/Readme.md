# Dark Mode
## Understand dark mode
Dark mode swaps a **light palette** for a **dark palette** — dark surfaces, light text, reduced contrast glare. Tailwind gives you a `dark:` variant that applies styles only when dark mode is active.
Two strategies:
- **`media`** (default) — follows the OS setting automatically.
- **`class`** — you control it with a `.dark` class on `<html>` (needed for a toggle button).
## Configure dark mode
**For a manual toggle (recommended):** set `darkMode: 'class'` in `tailwind.config.js`.
```js
// tailwind.config.js
module.exports = {
  darkMode: "class",
  content: ["./app/**/*.{js,ts,jsx,tsx,mdx}"],
  theme: { extend: {} },
  plugins: [],
};
```
**For OS-only (no toggle):** `darkMode: "media"` (or omit — this is default).
## Use dark:
Prefix any utility with `dark:` to apply it in dark mode.
```
<div className="bg-white dark:bg-gray-900 text-gray-900 dark:text-gray-100">
  Adapts to theme
</div>
```
**Toggle it** by adding/removing `.dark` on the `<html>` element:
```jsx
// minimal toggle
const [dark, setDark] = useState(false);
useEffect(() => {
  document.documentElement.classList.toggle("dark", dark);
}, [dark]);
```
## Dark backgrounds
Light page surface → dark page surface.
```
<div className="bg-gray-50 dark:bg-gray-950">
  <div className="bg-white dark:bg-gray-900">Card</div>
</div>
```
Rule: page `950`, card `900`, hover `800`.
## Dark text
Invert text hierarchy — dark greys become light greys.
```
<p className="text-gray-900 dark:text-gray-100">Primary</p>
<p className="text-gray-600 dark:text-gray-400">Muted</p>
<p className="text-gray-400 dark:text-gray-500">Caption</p>
```
## Dark borders
Borders flip from light greys to subtly-light-on-dark greys.
```
<div className="border border-gray-200 dark:border-gray-800">
  Divider
</div>
<hr className="border-gray-200 dark:border-gray-800" />
```
## Dark cards
Layer surfaces so cards stand out from the page background.
```
<div className="rounded-2xl border border-gray-200 bg-white p-6 shadow-sm
                dark:border-gray-800 dark:bg-gray-900 dark:shadow-none">
  <h3 className="text-gray-900 dark:text-gray-100">Title</h3>
  <p className="text-gray-500 dark:text-gray-400">Body text</p>
</div>
```
On dark, drop heavy shadows — contrast comes from surface + border, not shadow.
## Dark forms
Inputs need their own dark surface and readable placeholder.
```
<input
  className="
    w-full rounded-lg border border-gray-300 bg-white px-3 py-2 text-sm text-gray-900
    placeholder:text-gray-400
    focus:border-indigo-500 focus:ring-2 focus:ring-indigo-200 focus:outline-none
    dark:border-gray-700 dark:bg-gray-900 dark:text-gray-100
    dark:placeholder:text-gray-500
    dark:focus:border-indigo-400 dark:focus:ring-indigo-500/30
  "
/>
```
## Dark navigation
Sticky nav with theme-aware backdrop.
```
<header className="sticky top-0 z-50 border-b border-gray-200 bg-white/80 backdrop-blur
                    dark:border-gray-800 dark:bg-gray-950/80">
  <nav className="mx-auto flex max-w-6xl items-center justify-between px-6 py-4">
    <span className="font-bold text-gray-900 dark:text-gray-100">Brand</span>
    <div className="flex gap-6 text-sm text-gray-600 dark:text-gray-400">
      <span className="hover:text-gray-900 dark:hover:text-gray-100">Home</span>
      <span className="hover:text-gray-900 dark:hover:text-gray-100">About</span>
    </div>
  </nav>
</header>
```
## Dark dashboards
Stats + panels flip together.
```
<main className="min-h-screen bg-gray-50 dark:bg-gray-950 p-6">
  <div className="grid grid-cols-1 sm:grid-cols-3 gap-4">
    <div className="rounded-xl border border-gray-200 bg-white p-5
                    dark:border-gray-800 dark:bg-gray-900">
      <p className="text-xs uppercase tracking-widest text-gray-400 dark:text-gray-500">Users</p>
      <p className="mt-2 text-2xl font-bold text-gray-900 dark:text-gray-100">1,204</p>
    </div>
  </div>
</main>
```
Chart/panel backgrounds, tooltips, and axis labels all need `dark:` overrides — don't rely on default library styling.
## Light/dark color consistency
Keep the **same brand color** across both modes, but shift its shade.

| Role | Light | Dark |
|------|-------|------|
| Page bg | `bg-gray-50` | `dark:bg-gray-950` |
| Surface / card | `bg-white` | `dark:bg-gray-900` |
| Elevated / hover | `bg-gray-100` | `dark:bg-gray-800` |
| Border | `border-gray-200` | `dark:border-gray-800` |
| Primary text | `text-gray-900` | `dark:text-gray-100` |
| Muted text | `text-gray-600` | `dark:text-gray-400` |
| Brand accent | `bg-indigo-600` | `dark:bg-indigo-500` |
| Brand focus ring | `ring-indigo-200` | `dark:ring-indigo-500/30` |
Consistency rule: **same hue, shift the shade**. `indigo-600` light → `indigo-500` dark (slightly brighter so it pops on dark).
