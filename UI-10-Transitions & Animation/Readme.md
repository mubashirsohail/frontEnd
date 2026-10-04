# Transitions & Animation
## transition
Enables smooth changes on **all** animatable properties. Add to any element whose state changes.
```
<button className="bg-blue-600 transition hover:bg-blue-700">Hover me</button>
```
## transition-colors
Only animates color-related properties (bg, text, border, fill, stroke). Cheapest option for links and buttons.
```
<a className="text-gray-600 transition-colors hover:text-indigo-600">Link</a>
```
## transition-transform
Only animates transforms (scale, rotate, translate, skew). Best for hover lifts and icon motion.
```
<div className="transition-transform hover:scale-105">Card</div>
```
## transition-opacity
Only animates opacity. Great for fade-in overlays and reveal-on-hover.
```
<span className="opacity-0 transition-opacity group-hover:opacity-100">
  Read more →
</span>
```
## Duration
How long the transition takes. `duration-*` in milliseconds (`duration-75` … `duration-1000`).
```
<div className="transition duration-300 hover:bg-blue-500">300ms</div>
<div className="transition duration-700 hover:bg-blue-500">700ms</div>
```
Rule: 150–300ms feels snappy; >500ms feels slow.
## Delay
Wait before the transition starts. `delay-*`.
```
<div className="transition delay-150 hover:bg-blue-500">Starts 150ms later</div>
```
## Easing
Controls the speed curve. `ease-*`.
```
<div className="transition ease-in-out">Smooth both ends</div>
<div className="transition ease-out">Fast start, soft end</div>
<div className="transition ease-in">Soft start, fast end</div>
<div className="transition ease-linear">Constant speed</div>
```
Options: `ease-linear` `ease-in` `ease-out` `ease-in-out`
## Transform
Enables transform utilities. Tailwind v3+ transforms are auto-enabled — no `transform` class needed, but you can add it to be explicit.
```
<div className="transform hover:scale-110 hover:rotate-3 hover:translate-x-2">…</div>
```
## Scale
Resizes an element. `scale-*`, plus `scale-x-*` / `scale-y-*`.
```
<div className="scale-100 hover:scale-105 transition">Subtle grow</div>
<div className="scale-50">Half size</div>
<div className="scale-x-110">Stretch horizontally</div>
```
## Rotate
Rotates an element. `rotate-*` (degrees, positive or negative).
```
<div className="rotate-45">45°</div>
<div className="rotate-90">90°</div>
<div className="group-hover:rotate-180 transition">Chevron flips</div>
<div className="-rotate-12">Tilted left</div>
```
## Translate
Moves an element. `translate-x-*` / `translate-y-*`.
```
<div className="translate-y-0 hover:-translate-y-1 transition">Lifts up</div>
<div className="translate-x-4">Shifted right</div>
<div className="group-hover:translate-x-1 transition">Arrow slides</div>
```
## Skew
Tilts an element along an axis. `skew-x-*` / `skew-y-*`.
```
<div className="skew-y-3">Slight tilt</div>
<div className="-skew-x-6">Lean left</div>
```
## Built-in animations
Ready-made keyframe animations. Just add the class.
**animate-spin** — continuous rotation (spinners).
```
<div className="h-6 w-6 animate-spin rounded-full border-2 border-gray-300 border-t-indigo-600" />
```
**animate-pulse** — soft fade in/out (skeletons, live indicators).
```
<div className="h-4 w-32 animate-pulse rounded bg-gray-200" />
<span className="h-2 w-2 rounded-full bg-red-500 animate-pulse" />
```
**animate-bounce** — bounces up and down (scroll hints, playful icons).
```
<div className="animate-bounce text-2xl">↓</div>
```
## Custom animations
Define keyframes + animation utilities in `tailwind.config.js`.
```js
// tailwind.config.js
theme: {
  extend: {
    keyframes: {
      "fade-up": {
        "0%":   { opacity: "0", transform: "translateY(12px)" },
        "100%": { opacity: "1", transform: "translateY(0)" },
      },
      wiggle: {
        "0%, 100%": { transform: "rotate(-3deg)" },
        "50%":      { transform: "rotate(3deg)" },
      },
      shimmer: {
        "0%":   { backgroundPosition: "-200% 0" },
        "100%": { backgroundPosition: "200% 0" },
      },
    },
    animation: {
      "fade-up": "fade-up 0.5s ease-out both",
      wiggle: "wiggle 0.4s ease-in-out infinite",
      shimmer: "shimmer 2s linear infinite",
    },
  },
}
```
```
<div className="animate-fade-up">Enters smoothly</div>
<div className="animate-wiggle">Wiggling</div>
<div className="animate-shimmer bg-[linear-gradient(90deg,#eee,white,#eee)] bg-[size:200%_100%] bg-clip-text text-transparent">
  Shimmer text
</div>
```
