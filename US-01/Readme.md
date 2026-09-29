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
