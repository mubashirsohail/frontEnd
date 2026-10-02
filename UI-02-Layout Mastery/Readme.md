# Layout Mastery
## CSS Box Model in Tailwind
### 1. Content
w-* / h-*
```
<div className="w-32 h-20">Content</div>
```
### 2. Padding (inside)
p-*, px-*, py-*, pt/pr/pb/pl-*
```
<div className="p-4">All sides</div>
<div className="px-6 py-2">X / Y</div>
```
### 3. Border
border-*, border-{color}, rounded-*
```
<div className="border-2 border-red-500 rounded-lg">Box</div>
```
### 4. Margin (outside)
m-*, mx-*, my-*, mt/mr/mb/ml-*
```
<div className="m-4">Pushed away</div>
<div className="mx-auto w-40">Centered</div>
```
### Box Sizing
Tailwind default: border-box → width includes padding + border
```
<div className="w-64 p-4 border-4">Total = 256px</div>
```
### Visual
```
┌──────── Margin (m-*) ────────┐
│ ┌──── Border (border-*) ───┐ │
│ │ ┌── Padding (p-*) ────┐  │ │
│ │ │   Content (w/h)     │  │ │
│ │ └─────────────────────┘  │ │
│ └──────────────────────────┘ │
└──────────────────────────────┘
```
## Use block
Makes an element a block-level box — takes full width, starts on a new line.
```
<div className="block w-full bg-blue-500">Full width element</div>
```
## Use inline
Makes an element inline-level — sits in the text flow, only takes content width, no width/height or vertical margins.
```
<span className="inline bg-yellow-200">Sits inline with text</span>
```
## Use inline-block
Flows inline like text, but accepts width, height, padding, and margin on all sides.
```
<span className="inline-block w-24 h-10 bg-green-300 align-middle">Box in flow</span>
```
## Use flex
Turns an element into a flex container — its children lay out along a row or column with easy alignment and spacing.
```
<div className="flex items-center justify-between gap-4">
  <span>Left</span>
  <span>Right</span>
</div>
```
## Use flex-row
Sets the main axis to horizontal — flex children line up left → right.
```
<div className="flex flex-row gap-4">
  <div>A</div><div>B</div><div>C</div>
</div>
```
## Use flex-col
Sets the main axis to vertical — flex children stack top → bottom.
```
<div className="flex flex-col gap-4">
  <div>A</div><div>B</div><div>C</div>
</div>
```
## Use flex-wrap
Allows flex children to wrap onto the next line when they run out of space, instead of shrinking.
```
<div className="flex flex-wrap gap-4">
  <div>Item 1</div><div>Item 2</div><div>Item 3</div>
</div>
```
## Use justify-* (main axis)
Aligns flex children along the main axis (horizontal in flex-row, vertical in flex-col).
Options: justify-start · justify-center · justify-end · justify-between · justify-around · justify-evenly
```
<div className="flex justify-between">
  <span>Left</span><span>Right</span>
</div>
```
## Use items-* (cross axis)
Aligns flex children along the cross axis (vertical in flex-row, horizontal in flex-col).
Options: items-start · items-center · items-end · items-stretch · items-baseline
```
<div className="flex items-center h-24">
  <span>Vertically centered</span>
</div>
```
## Use content-* (multi-line alignment)
Aligns wrapped flex lines along the cross axis — only works with flex-wrap and multiple lines.
Options: content-start · content-center · content-end · content-between · content-around · content-evenly
```
<div className="flex flex-wrap content-center h-48 gap-2">
  <div>A</div><div>B</div><div>C</div><div>D</div>
</div>
```
## Use gap-* 
Adds consistent spacing between flex/grid children — no margin hacks needed.
Options: gap-*, gap-x-* (columns), gap-y-* (rows)
```
<div className="flex gap-4">
  <div>A</div><div>B</div><div>C</div>
</div>
```
## Use space-x-*
Adds horizontal margin between children — first child gets no left margin (used before gap existed).
```
<div className="flex space-x-4">
  <div>A</div><div>B</div><div>C</div>
</div>
```
## Use space-y-*
Adds vertical margin between children — first child gets no top margin.
```
<div className="flex flex-col space-y-4">
  <div>A</div><div>B</div><div>C</div>
</div>
```
## Use grid
Turns an element into a grid container — children lay out in rows and columns.
```
<div className="grid grid-cols-3 gap-4">
  <div>A</div><div>B</div><div>C</div>
</div>
```
## Grid Col
Defines the number of columns in a grid container.
```
<div className="grid grid-cols-3 gap-4">...</div>
```
## Grid Row
Defines the number of rows in a grid container.
```
<div className="grid grid-rows-2 grid-flow-col gap-4">...</div>
```
## Col-Span-*
Makes a grid item span across multiple columns.
```
<div className="grid grid-cols-3 gap-4">
  <div className="col-span-2 bg-blue-300">Wide</div>
  <div className="bg-green-300">Normal</div>
</div>
```
## Use responsive grids
Change grid columns/rows at different screen sizes using breakpoint prefixes.
Breakpoints: sm: ≥640px · md: ≥768px · lg: ≥1024px · xl: ≥1280px · 2xl: ≥1536px
```
<div className="grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-3 xl:grid-cols-4 gap-4">
  <div>A</div><div>B</div><div>C</div><div>D</div>
</div>
```
Mobile-first rule: Start with base, layer up with sm:, md:, lg: — never down.
## Use place-items-*
Shorthand for align-items + justify-items — aligns all grid/flex children on both axes at once.
Options: place-items-start · place-items-center · place-items-end · place-items-stretch · place-items-baseline
```
<div className="grid place-items-center h-48">
  <div>Perfectly centered</div>
</div>
```
place-items-* → align all items (both axes)
place-self-* → align one item
place-content-* → align the whole grid inside container
