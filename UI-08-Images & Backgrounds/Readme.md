# Images & Backgrounds
## Background Images
Use arbitrary values or a custom `bgImage` in config.
```
<div className="bg-[url('/hero.jpg')]">BG image</div>
```
```js
// tailwind.config.js
theme: {
  extend: {
    backgroundImage: {
      hero: "url('/hero.jpg')",
      pattern: "url('/dots.svg')",
    },
  },
}
```
```
<div className="bg-hero">Custom bg</div>
```
## Background Positioning
`bg-{position}` — controls where the image sits inside its box.
```
<div className="bg-center">Center</div>
<div className="bg-top">Top</div>
<div className="bg-bottom">Bottom</div>
<div className="bg-left">Left</div>
<div className="bg-right">Right</div>
<div className="bg-left-top">Top-left corner</div>
<div className="bg-right-bottom">Bottom-right corner</div>
```
## Background Sizing
Controls how the image scales.
```
<div className="bg-auto">Natural size</div>
<div className="bg-cover">Fill box (may crop)</div>
<div className="bg-contain">Fit inside (may letterbox)</div>
<div className="bg-[length:200px_100px]">Custom size</div>
```
## bg-cover
Image fills the entire box, cropping overflow. **Most common for heroes.**
```
<div className="h-64 bg-cover bg-center bg-[url('/hero.jpg')]" />
```
## bg-contain
Image scales to fit fully inside the box — may leave empty space.
```
<div className="h-64 bg-contain bg-center bg-no-repeat bg-[url('/logo.png')]" />
```
Tip: pair `bg-contain` with `bg-no-repeat` so it doesn't tile.
## Background Gradients
`bg-gradient-to-{dir}` + `from-*` `via-*` `to-*`.
```
<div className="bg-gradient-to-r from-blue-500 to-purple-600" />
<div className="bg-gradient-to-br from-pink-500 via-red-500 to-yellow-500" />
```
Directions: `to-t` `to-tr` `to-r` `to-br` `to-b` `to-bl` `to-l` `to-tl`
## Background Overlays
Layer a `bg-black/40` on top of a background image for readable text.
```
<div className="relative h-64 bg-cover bg-center bg-[url('/hero.jpg')]">
  <div className="absolute inset-0 bg-black/50" />
  <div className="relative z-10 flex h-full items-center justify-center text-white">
    <h1 className="text-3xl font-bold">Readable over image</h1>
  </div>
</div>
```
## Object Positioning
For `<img>` elements — controls which part of the image stays visible.
```
<img className="h-64 w-full object-cover object-top" src="/portrait.jpg" />
<img className="h-64 w-full object-cover object-center" src="/portrait.jpg" />
<img className="h-64 w-full object-cover object-bottom" src="/portrait.jpg" />
```
Options: `object-{top|bottom|left|right|center|left-top|right-bottom|...}`
## Object Fit
Controls how `<img>` fills its box. Same idea as `bg-cover`/`bg-contain` but for `<img>`.
```
<img className="h-48 w-full object-cover" src="/a.jpg" />   // fill + crop
<img className="h-48 w-full object-contain" src="/a.jpg" /> // fit + letterbox
<img className="h-48 w-full object-fill" src="/a.jpg" />    // stretch
<img className="h-48 w-full object-none" src="/a.jpg" />    // natural
<img className="h-48 w-full object-scale-down" src="/a.jpg" />
```
## Image Aspect Ratios
`aspect-{ratio}` locks an element's width-to-height ratio.
```
<img className="aspect-square w-full object-cover" src="/a.jpg" />
<img className="aspect-video w-full object-cover" src="/a.jpg" />
<img className="aspect-[4/3] w-full object-cover" src="/a.jpg" />
```
Presets: `aspect-auto` `aspect-square` `aspect-video` · Arbitrary: `aspect-[16/9]`
## Image Cards
Combine radius, shadow, overflow-hidden, and object-cover.
```
<div className="overflow-hidden rounded-2xl border border-gray-200 bg-white shadow-sm">
  <img className="aspect-video w-full object-cover" src="/post.jpg" />
  <div className="p-4">
    <h3 className="font-semibold">Card title</h3>
    <p className="mt-1 text-sm text-gray-500 line-clamp-2">
      Short description of the card content.
    </p>
  </div>
</div>
```
## Hero Image Overlays
Full-bleed image with a gradient overlay for legibility.
```
<section className="relative h-[500px] bg-cover bg-center bg-[url('/hero.jpg')]">
  {/* Dark gradient overlay */}
  <div className="absolute inset-0 bg-gradient-to-t from-black/80 via-black/40 to-transparent" />

  <div className="relative z-10 flex h-full flex-col items-center justify-center px-6 text-center text-white">
    <h1 className="text-3xl md:text-5xl font-extrabold tracking-tight">
      Build something great
    </h1>
    <p className="mt-4 max-w-xl text-sm md:text-base opacity-90">
      Ship faster with a modern stack.
    </p>
    <button className="mt-6 rounded-lg bg-white px-6 py-3 text-sm font-semibold text-gray-900 hover:bg-gray-100 transition">
      Get Started
    </button>
  </div>
</section>
```
