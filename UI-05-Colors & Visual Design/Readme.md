# Colors & Visual Design
## Tailwind Color System
Every color has **11 shades** — `50` (lightest) → `950` (darkest). Use `{property}-{color}-{shade}`.
```
<div className="bg-blue-500 text-white border-blue-700">Shade 500</div>
```
Palettes: `slate` `gray` `zinc` `neutral` `stone` `red` `orange` `amber` `yellow` `lime` `green` `emerald` `teal` `cyan` `sky` `blue` `indigo` `violet` `purple` `fuchsia` `pink` `rose`
## Background Colors
`bg-{color}-{shade}` — also `bg-transparent`, `bg-white`, `bg-black`, `bg-current`.
```
<div className="bg-blue-500">Blue bg</div>
<div className="bg-gray-100">Light gray</div>
<div className="bg-transparent">Transparent</div>
```
## Text Colors
`text-{color}-{shade}` — contrast against your background.
```
<p className="text-gray-900">Primary text</p>
<p className="text-gray-500">Muted text</p>
<p className="text-red-600">Error text</p>
```
## Border Colors
`border-{color}-{shade}` — must pair with a `border-*` width.
```
<div className="border-2 border-blue-500">Blue border</div>
<div className="border border-gray-200">Hairline border</div>
```
## Ring Colors
`ring-{color}-{shade}` — outline-like ring, great for focus states.
```
<input className="ring-2 ring-blue-500 ring-offset-2" />
<button className="focus:ring-2 focus:ring-blue-500 focus:outline-none">Focus me</button>
```
## Opacity
Two ways: **slash syntax** on the color, or the standalone `opacity-*` utility.
```
<div className="bg-blue-500/50">50% blue bg</div>
<div className="text-black/70">70% black text</div>
<div className="opacity-50">Whole element at 50%</div>
```
Scale: `0` `5` `10` `20` `25` `30` `40` `50` `60` `70` `75` `80` `90` `95` `100`
## Color Combinations
Pair a strong background with a readable foreground and a mid-tone border.
```
<div className="bg-blue-50 text-blue-800 border border-blue-200 rounded-lg p-4">
  Info card
</div>
```
Rule: `bg-{color}-50` + `text-{color}-800` + `border-{color}-200` = clean alert palette.
## Neutral Color Palettes
Greys for text, backgrounds, and dividers.
```
<main className="bg-white text-gray-900">
  <p className="text-gray-500">Muted copy</p>
  <hr className="border-gray-200" />
</main>
```
Neutrals: `slate` (cool) · `gray` (balanced) · `zinc` (neutral) · `neutral` (true) · `stone` (warm)
## Brand Color Palettes
Pick one main color and stick to a few shades for consistency.
```
<button className="bg-indigo-600 text-white hover:bg-indigo-700">
  Brand Button
</button>
<a className="text-indigo-600 hover:text-indigo-800">Brand link</a>
```
## Dark Color Palettes
Deep shades (`900` / `950`) for dark-mode surfaces.
```
<div className="bg-gray-900 text-gray-100">
  <div className="bg-gray-800 border border-gray-700 rounded-lg p-4">
    Dark card
  </div>
</div>
```
Rule: bg `900` → card `800` → border `700` → text `100`.
## Gradient Backgrounds
`bg-gradient-to-{dir}` + `from-*` `via-*` `to-*`.
```
<div className="bg-gradient-to-r from-blue-500 to-purple-600 p-8 text-white">
  Gradient bg
</div>
<div className="bg-gradient-to-br from-pink-500 via-red-500 to-yellow-500">
  Multi-stop
</div>
```
Directions: `to-t` `to-tr` `to-r` `to-br` `to-b` `to-bl` `to-l` `to-tl`
## Gradient Text
Clip a gradient to the text shape.
```
<h1 className="text-4xl font-bold bg-gradient-to-r from-blue-600 to-purple-600 bg-clip-text text-transparent">
  Gradient Heading
</h1>
```
Needs: `bg-gradient-*` + `bg-clip-text` + `text-transparent`
## Multi-color Gradients
Use `via-*` (and multiple stops) for richer blends.
```
<div className="bg-gradient-to-r from-indigo-500 via-purple-500 to-pink-500 text-white p-6 rounded-xl">
  Three-stop gradient
</div>
```
## Hover Color Changes
Prefix any color utility with `hover:` (and `focus:`, `active:`).
```
<button className="bg-blue-600 hover:bg-blue-700 text-white px-4 py-2 rounded-lg transition">
  Hover me
</button>
<a className="text-gray-600 hover:text-blue-600 transition-colors">Link</a>
```
Always add `transition` so the color change feels smooth.
