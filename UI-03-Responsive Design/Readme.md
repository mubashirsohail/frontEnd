# Responsive Design
## Understand mobile-first design
Write base styles for mobile first, then layer up with breakpoint prefixes (`sm:` → `2xl:`). Never design down.
```
<div className="text-sm md:text-base lg:text-lg">Mobile-first text</div>
```
## Use sm:
Applies styles from **≥640px** upward.
```
<div className="p-2 sm:p-4">Padding grows at 640px</div>
```
## Use md:
Applies styles from **≥768px** upward.
```
<div className="grid-cols-1 md:grid-cols-2">2 cols at 768px</div>
```
## Use lg:
Applies styles from **≥1024px** upward.
```
<div className="hidden lg:block">Visible at 1024px+</div>
```
## Use xl:
Applies styles from **≥1280px** upward.
```
<div className="max-w-4xl xl:max-w-6xl">Wider at 1280px</div>
```
## Use 2xl:
Applies styles from **≥1536px** upward.
```
<div className="text-2xl 2xl:text-4xl">Bigger at 1536px</div>
```
## Change typography responsively
Scale font size and weight per breakpoint.
```
<h1 className="text-xl sm:text-2xl md:text-4xl lg:text-5xl font-bold">
  Responsive Heading
</h1>
```
## Change spacing responsively
Adjust padding, margin, and gap per breakpoint.
```
<div className="p-4 md:p-8 lg:p-12 gap-2 md:gap-4">
  Spacing grows with screen
</div>
```
## Change layout responsively
Switch between stacked and side-by-side layouts.
```
<div className="flex flex-col md:flex-row gap-4">
  <div>Sidebar</div>
  <div>Content</div>
</div>
```
## Hide/show elements responsively
Toggle visibility per breakpoint.
```
<div className="block md:hidden">Mobile only</div>
<div className="hidden md:block">Desktop only</div>
```
## Change grid columns responsively
Define column count per breakpoint.
```
<div className="grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-3 xl:grid-cols-4 gap-4">
  <div>A</div><div>B</div><div>C</div><div>D</div>
</div>
```
## Change flex direction responsively
Stack on mobile, row on larger screens.
```
<div className="flex flex-col lg:flex-row gap-6">
  <div>Left</div>
  <div>Right</div>
</div>
```
## Build mobile navigation
Hamburger on mobile, full menu on desktop.
```
<nav className="flex items-center justify-between p-4">
  <span className="font-bold">Logo</span>
  {/* Mobile */}
  <button className="md:hidden">☰</button>
  {/* Desktop */}
  <ul className="hidden md:flex gap-6">
    <li>Home</li>
    <li>About</li>
    <li>Contact</li>
  </ul>
</nav>
```
## Build responsive cards
Stack on mobile, grid on larger screens.
```
<div className="grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-3 gap-4">
  <div className="p-6 bg-white rounded-lg shadow">Card 1</div>
  <div className="p-6 bg-white rounded-lg shadow">Card 2</div>
  <div className="p-6 bg-white rounded-lg shadow">Card 3</div>
</div>
```
## Build responsive tables
Horizontal scroll on mobile, full table on desktop.
```
<div className="overflow-x-auto">
  <table className="min-w-full text-sm md:text-base">
    <thead className="bg-gray-100">
      <tr><th className="p-2">Name</th><th className="p-2">Email</th></tr>
    </thead>
    <tbody>
      <tr><td className="p-2">Ali</td><td className="p-2">ali@mail.com</td></tr>
    </tbody>
  </table>
</div>
```
## Build responsive hero sections
Centered stack on mobile, two-column on desktop.
```
<section className="flex flex-col lg:flex-row items-center gap-8 p-6 lg:p-16">
  <div className="flex-1 text-center lg:text-left">
    <h1 className="text-3xl md:text-5xl font-bold">Build faster</h1>
    <p className="mt-4 text-gray-600">Tailwind makes responsive design simple.</p>
    <button className="mt-6 px-6 py-3 bg-blue-600 text-white rounded-lg">
      Get Started
    </button>
  </div>
  <div className="flex-1">
    <img src="/hero.png" alt="Hero" className="w-full rounded-lg" />
  </div>
</section>
```
## Breakpoint Reference
| Prefix | Min Width |
|--------|-----------|
| (base) | 0px |
| `sm:` | 640px |
| `md:` | 768px |
| `lg:` | 1024px |
| `xl:` | 1280px |
| `2xl:` | 1536px |
