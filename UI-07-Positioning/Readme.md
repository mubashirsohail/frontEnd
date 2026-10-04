# Positioning
## relative
Establishes a **positioning context** for absolute children. The element itself still occupies normal flow.
```
<div className="relative">
  <span className="absolute top-0 right-0">Badge</span>
</div>
```
## absolute
Removes the element from flow and positions it against the **nearest relative/absolute/fixed ancestor**.
```
<div className="relative h-40 bg-gray-100">
  <div className="absolute top-4 left-4 bg-blue-500 text-white p-2">Top-left</div>
</div>
```
## fixed
Pins the element to the **viewport** — it doesn't scroll with the page.
```
<button className="fixed bottom-6 right-6 bg-blue-600 text-white p-4 rounded-full shadow-lg">
  +
</button>
```
## sticky
Behaves like `relative` until it hits a scroll threshold, then sticks like `fixed` **within its parent**.
```
<header className="sticky top-0 z-50 bg-white/90 backdrop-blur border-b">
  Sticky nav
</header>
```
## inset-*
Shorthand for `top/right/bottom/left` all at once.
```
<div className="absolute inset-0 bg-black/40">Full overlay</div>
<div className="absolute inset-x-0 bottom-0">Full width, pinned bottom</div>
<div className="absolute inset-y-0 left-0">Full height, pinned left</div>
```
Options: `inset-0` `inset-1` `inset-2` `inset-4` `inset-auto` `inset-x-*` `inset-y-*`
## top-*
```
<div className="absolute top-0">Top edge</div>
<div className="absolute top-4">16px from top</div>
<div className="absolute -top-2">Above parent (negative)</div>
```
## right-*
```
<div className="absolute right-0">Right edge</div>
<div className="absolute right-4">16px from right</div>
```
## bottom-*
```
<div className="absolute bottom-0">Bottom edge</div>
<div className="absolute bottom-4">16px from bottom</div>
```
## left-*
```
<div className="absolute left-0">Left edge</div>
<div className="absolute left-4">16px from left</div>
```
## z-*
Controls stacking order. Higher number = on top.
```
<div className="z-0">Base</div>
<div className="z-10">Dropdown</div>
<div className="z-40">Overlay</div>
<div className="z-50">Modal / sticky nav</div>
```
Options: `z-0` `z-10` `z-20` `z-30` `z-40` `z-50` `z-auto`, plus negatives `-z-10`.
## Layered Components
Stack layers using `absolute` + `z-*` inside a `relative` parent.
```
<div className="relative h-64 rounded-xl overflow-hidden">
  <img src="/bg.jpg" className="absolute inset-0 w-full h-full object-cover z-0" />
  <div className="absolute inset-0 bg-black/50 z-10" />
  <div className="absolute inset-0 flex items-center justify-center z-20 text-white">
    <h1 className="text-3xl font-bold">Layered Hero</h1>
  </div>
</div>
```
## Absolute Badges
Pin a badge to a corner of a `relative` parent.
```
<div className="relative inline-block">
  <span className="text-3xl">🔔</span>
  <span className="absolute -top-1 -right-1 flex h-5 w-5 items-center justify-center rounded-full bg-red-500 text-[10px] font-bold text-white ring-2 ring-white">
    3
  </span>
</div>
```
## Floating Buttons
Fixed circular button (FAB) anchored to the viewport.
```
<button className="fixed bottom-6 right-6 z-50 flex h-14 w-14 items-center justify-center rounded-full bg-indigo-600 text-white text-2xl shadow-xl hover:bg-indigo-500 transition">
  +
</button>
```
## Sticky Navigation
Nav that sticks to the top after scrolling past it.
```
<header className="sticky top-0 z-50 bg-white/80 backdrop-blur border-b border-gray-200">
  <nav className="max-w-6xl mx-auto flex items-center justify-between px-6 py-4">
    <span className="font-bold">Brand</span>
    <div className="hidden md:flex gap-6 text-sm">
      <span>Home</span><span>About</span><span>Contact</span>
    </div>
  </nav>
</header>
```
## Modal Positioning
Center a modal with `fixed` + `inset-0` + flex centering.
```
<div className="fixed inset-0 z-50 flex items-center justify-center">
  {/* Backdrop */}
  <div className="absolute inset-0 bg-black/50 backdrop-blur-sm" />
  {/* Modal box */}
  <div className="relative z-10 w-full max-w-md rounded-2xl bg-white p-6 shadow-2xl">
    <h2 className="text-lg font-bold">Confirm action</h2>
    <p className="mt-2 text-sm text-gray-600">Are you sure you want to continue?</p>
    <div className="mt-6 flex justify-end gap-3">
      <button className="rounded-lg border px-4 py-2 text-sm">Cancel</button>
      <button className="rounded-lg bg-blue-600 px-4 py-2 text-sm text-white">Confirm</button>
    </div>
  </div>
</div>
```
Key trick: parent is `fixed inset-0 flex items-center justify-center`, child is `relative z-10`.
