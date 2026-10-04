# Borders, Radius & Shadows
## Border Width
`border` (1px) + numeric widths `border-0` → `border-8`.
```
<div className="border">1px</div>
<div className="border-2">2px</div>
<div className="border-4">4px</div>
<div className="border-8">8px</div>
```
## Individual Borders
Control each side: `border-t` `border-r` `border-b` `border-l`.
```
<div className="border-t border-gray-200">Top only</div>
<div className="border-l-4 border-blue-500 pl-4">Left accent</div>
<div className="border-x border-gray-300">Left + Right</div>
<div className="border-y border-gray-300">Top + Bottom</div>
```
## Border Styles
`border-solid` (default), `border-dashed`, `border-dotted`, `border-double`, `border-none`.
```
<div className="border-2 border-dashed border-gray-400">Dashed</div>
<div className="border-2 border-dotted border-blue-500">Dotted</div>
<div className="border-4 border-double border-gray-800">Double</div>
```
## Border Radius
`rounded-*` — `none` `sm` `md` `lg` `xl` `2xl` `3xl` `full`. Side variants: `rounded-t-*`, `rounded-b-*`, `rounded-l-*`, `rounded-r-*`. Corner variants: `rounded-tl-*` etc.
```
<div className="rounded">4px</div>
<div className="rounded-lg">8px</div>
<div className="rounded-2xl">16px</div>
<div className="rounded-full">Pill / circle</div>
<div className="rounded-t-xl">Top corners only</div>
```
## Rounded Cards
```
<div className="rounded-2xl border border-gray-200 bg-white p-6 shadow-sm">
  Card content
</div>
```
## Rounded Buttons
```
<button className="rounded-lg bg-blue-600 px-4 py-2 text-white hover:bg-blue-700">
  Rounded
</button>
<button className="rounded-full bg-gray-900 px-5 py-2 text-white">
  Pill button
</button>
```
## Circular Elements
`rounded-full` + equal `w-*` and `h-*`.
```
<img className="h-12 w-12 rounded-full object-cover" src="/avatar.png" />
<span className="flex h-8 w-8 items-center justify-center rounded-full bg-indigo-100 text-indigo-600">
  A
</span>
```
## Shadows
`shadow-*` scale for elevation.
```
<div className="shadow-sm">Subtle</div>
<div className="shadow">Default</div>
<div className="shadow-md">Medium</div>
<div className="shadow-lg">Large</div>
<div className="shadow-xl">Extra large</div>
<div className="shadow-2xl">Huge</div>
<div className="shadow-inner">Inset</div>
<div className="shadow-none">No shadow</div>
```
## Custom Shadows
Override via `tailwind.config.js` `theme.extend.boxShadow`.
```js
// tailwind.config.js
theme: {
  extend: {
    boxShadow: {
      brand: "0 10px 30px -10px rgba(79,70,229,0.5)",
      soft: "0 2px 8px rgba(0,0,0,0.06)",
    },
  },
}
```
```
<div className="shadow-brand rounded-xl p-6">Brand shadow</div>
<div className="shadow-soft rounded-xl p-6">Soft shadow</div>
```
## Rings
Outline-like ring drawn **outside** the border box. Works on any element.
```
<div className="ring-2 ring-blue-500 rounded-lg p-4">Ring</div>
<div className="ring-4 ring-indigo-500/50 rounded-lg p-4">Translucent ring</div>
<div className="ring-2 ring-offset-2 ring-offset-white ring-blue-500">With offset</div>
```
## Focus Rings
Great for accessible inputs and buttons.
```
<input className="rounded-lg border border-gray-300 px-3 py-2 outline-none focus:ring-2 focus:ring-blue-500 focus:border-blue-500" />

<button className="rounded-lg bg-blue-600 px-4 py-2 text-white outline-none focus:ring-2 focus:ring-blue-400 focus:ring-offset-2">
  Focus me
</button>
```
## Divide Utilities
Adds borders **between** children — no manual `border-t` on each item.
```
<ul className="divide-y divide-gray-200">
  <li className="py-3">Item 1</li>
  <li className="py-3">Item 2</li>
  <li className="py-3">Item 3</li>
</ul>

<div className="flex divide-x divide-gray-200">
  <div className="px-4">Left</div>
  <div className="px-4">Right</div>
</div>
```
Variants: `divide-x-*` `divide-y-*` `divide-{color}` `divide-{style}`
## Border Opacity
Slash syntax on the color, or `border-opacity-*` (legacy).
```
<div className="border-2 border-blue-500/50">50% opacity border</div>
<div className="border border-gray-900/10">Hairline</div>
<div className="ring-2 ring-black/5">Faint ring</div>
```
