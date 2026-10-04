# Typography
## Font Family
Sets the typeface. Tailwind defaults: `font-sans`, `font-serif`, `font-mono`.
```
<p className="font-sans">Sans-serif text</p>
<p className="font-serif">Serif text</p>
<p className="font-mono">Monospace text</p>
```
## Font Size
Controls text size. Scale from `text-xs` → `text-9xl`.
```
<p className="text-sm">Small</p>
<p className="text-base">Base</p>
<p className="text-2xl">Large</p>
```
Sizes: `text-xs` `text-sm` `text-base` `text-lg` `text-xl` `text-2xl` `text-4xl` `text-6xl` `text-9xl`
## Font Weight
Controls thickness. `font-thin` (100) → `font-black` (900).
```
<p className="font-light">Light</p>
<p className="font-semibold">Semibold</p>
<p className="font-bold">Bold</p>
```
Weights: `font-thin` `font-light` `font-normal` `font-medium` `font-semibold` `font-bold` `font-extrabold` `font-black`
## Line Height
Controls vertical spacing between lines. `leading-*`.
```
<p className="leading-tight">Tight lines</p>
<p className="leading-relaxed">Relaxed lines</p>
<p className="leading-loose">Loose lines</p>
```
Options: `leading-none` `leading-tight` `leading-snug` `leading-normal` `leading-relaxed` `leading-loose` `leading-3` … `leading-10`
## Letter Spacing
Controls space between characters. `tracking-*`.
```
<p className="tracking-tight">Tight</p>
<p className="tracking-normal">Normal</p>
<p className="tracking-widest">Widest</p>
```
Options: `tracking-tighter` `tracking-tight` `tracking-normal` `tracking-wide` `tracking-wider` `tracking-widest`
## Text Alignment
Aligns text horizontally. `text-left` `text-center` `text-right` `text-justify` `text-start` `text-end`.
```
<p className="text-left">Left</p>
<p className="text-center">Center</p>
<p className="text-right">Right</p>
```
## Text Decoration
Adds lines to text. `underline` `overline` `line-through` `no-underline`.
```
<p className="underline">Underlined</p>
<p className="line-through">Strikethrough</p>
<p className="underline decoration-red-500 decoration-2">Colored underline</p>
```
## Text Transformation
Changes letter case. `uppercase` `lowercase` `capitalize` `normal-case`.
```
<p className="uppercase">shouty text</p>
<p className="capitalize">each word capitalized</p>
```
## Text Truncation
Cuts off overflowing text with ellipsis. Needs `truncate` + fixed width.
```
<p className="truncate w-40">
  This is a very long sentence that gets cut off
</p>
```
`truncate` = `overflow-hidden` + `text-ellipsis` + `whitespace-nowrap`
## Line Clamp
Limits text to N lines with ellipsis. `line-clamp-*`.
```
<p className="line-clamp-2">
  Long paragraph text that will be cut after two lines with an ellipsis...
</p>
```
Options: `line-clamp-1` … `line-clamp-6`, `line-clamp-none`
## Gradient Text
Clips a gradient background to the text shape.
```
<h1 className="text-4xl font-bold bg-gradient-to-r from-blue-600 to-purple-600 bg-clip-text text-transparent">
  Gradient Heading
</h1>
```
Needs 3 classes: `bg-gradient-*` + `bg-clip-text` + `text-transparent`
## Responsive Typography
Scales font size per breakpoint.
```
<h1 className="text-2xl sm:text-3xl md:text-4xl lg:text-6xl font-bold">
  Responsive Heading
</h1>
```
## Custom Google Fonts
Load via `next/font/google` in Next.js, apply with a className.
```
// app/layout.jsx
import { Inter, Playfair_Display } from "next/font/google";
const inter = Inter({ subsets: ["latin"], variable: "--font-inter" });
const playfair = Playfair_Display({ subsets: ["latin"], variable: "--font-playfair" });
export default function RootLayout({ children }) {
  return (
    <html lang="en" className={`${inter.variable} ${playfair.variable}`}>
      <body>{children}</body>
    </html>
  );
}
```
```
// tailwind.config.js
fontFamily: {
  sans: ["var(--font-inter)"],
  serif: ["var(--font-playfair)"],
}
```
```
<h1 className="font-serif text-4xl">Playfair Heading</h1>
<p className="font-sans">Inter body text</p>
```
## Heading Hierarchy
Consistent sizes and weights from `h1` → `h6`.
```
<h1 className="text-4xl md:text-5xl font-bold">H1 — Page title</h1>
<h2 className="text-3xl md:text-4xl font-semibold">H2 — Section</h2>
<h3 className="text-2xl font-semibold">H3 — Subsection</h3>
<h4 className="text-xl font-medium">H4 — Card title</h4>
<h5 className="text-lg font-medium">H5 — Label</h5>
<h6 className="text-base font-medium text-gray-500">H6 — Caption</h6>
```
