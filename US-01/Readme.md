# Tailwind Fundamentals and Configration
## Here we will discuss fundamentals of Tailwind css in detail from installation to advance working
# Install Tailwind CSS in a project
1. Go to [Tailwind Documentation](https://tailwindcss.com/docs/installation/tailwind-cli)
2. Copy ```npm install tailwindcss @tailwindcss/cli```
3. Paste it into VS Code (Any other code editor) Terminal and hit Enter
4. (If you are working with Nextjs, congrates, your Tailwind is already insatlled)
### Apply Text Colors
```
<h1 className="text-blue-600">
   Hello Tailwind CSS
</h1>
```
### Apply background colors
```
<div className="bg-green-300">
```
### Apply border colors
```
<div className="border border-blue-500">Border color</div>
```
### Set font sizes
```
<h1 className="text-2xl">Heading</h1>
```
### Set font weights
```
<p className="font-bold">Text</p>
```
### Set line heights
```
<p className="leading-none">Text</p>
```
 (or `leading-tight`, `leading-snug`, `leading-relaxed`, `leading-loose`)
### Set letter spacing
```
<p className="tracking-wide">Text</p>
```
(or tracking-tighter, tracking-tight, tracking-normal, tracking-wider, tracking-widest)
### Set element width
```
<div className="w-full">Content</div>
```
(or `w-auto`, `w-1/2`, `w-64`, `w-screen`)
### Set element height
```
<div className="h-full">Content</div>
```
(or h-auto, h-64, h-screen, h-fit)
### Set minimum/maximum width
```
<div className="min-w-0 max-w-md">Content</div>
```
(or min-w-full, max-w-full, max-w-lg, max-w-screen-xl)
### Set minimum/maximum Height
```
<div className="min-h-0 max-h-screen">Content</div>
```
or min-h-full, max-h-full, max-h-96)
### Apply Padding
```
<div className="p-4 ">Content</div>
```
or (pt-4 pb-2 pl-6 pr-8 px-4 py-4)
### Apply Margin
```
<div className="m-4">Content</div>
```
or (mt-4 mb-2 ml-6 mr-8 mx-3 my-3)
### Understand spacing scale
p-0 for 0px, p-1 for 4px, p-2 for 8px, p-4 for 16px, p-8 for 32px, p-16 for 64px
### Use arbitrary values such as w-[420px]
```
<div className="w-[420px]">Content</div>
```
(or h-[300px], text-[17px], p-[18px], m-[22px], bg-[#bada55])
### Combine multiple utilities correctly
<div className="flex items-center justify-between p-4 bg-white rounded-lg shadow-md">Content</div> (or text-sm font-bold text-gray-700 hover:text-blue-500, w-full max-w-sm mx-auto mt-4, grid grid-cols-3 gap-4 p-6)
