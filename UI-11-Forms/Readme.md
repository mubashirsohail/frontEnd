# Forms
## Input styling
Base style for text, email, password, number inputs.
```
<input
  type="text"
  placeholder="Enter your name"
  className="w-full rounded-lg border border-gray-300 bg-white px-3.5 py-2.5 text-sm text-gray-900 placeholder:text-gray-400 focus:border-indigo-500 focus:ring-2 focus:ring-indigo-200 focus:outline-none disabled:bg-gray-100 disabled:cursor-not-allowed dark:border-gray-700 dark:bg-gray-900 dark:text-gray-100 dark:placeholder:text-gray-500"
/>
```
Essentials: width, padding, border, radius, placeholder color, focus ring, disabled state.
## Select styling
Native `<select>` — style the box; options keep OS styling.
```
<select className="w-full appearance-none rounded-lg border border-gray-300 bg-white px-3.5 py-2.5 pr-10 text-sm text-gray-900 focus:border-indigo-500 focus:ring-2 focus:ring-indigo-200 focus:outline-none dark:border-gray-700 dark:bg-gray-900 dark:text-gray-100">
  <option>Choose a plan</option>
  <option>Starter</option>
  <option>Pro</option>
</select>
```
`appearance-none` removes the native arrow — add your own caret with a background image or an absolute icon.
## Checkbox styling
Two approaches: `accent-*` (simple) or a custom `peer` checkbox (full control).
**Simple:**
```
<input type="checkbox" className="h-4 w-4 rounded border-gray-300 text-indigo-600 accent-indigo-600 focus:ring-2 focus:ring-indigo-200" />
```
**Custom (recommended):**
```
<label className="inline-flex items-center gap-2 cursor-pointer text-sm text-gray-700 dark:text-gray-300">
  <input type="checkbox" className="peer sr-only" />
  <span className="flex h-4 w-4 items-center justify-center rounded border border-gray-300 bg-white text-[10px] text-white transition peer-checked:border-indigo-600 peer-checked:bg-indigo-600 peer-focus-visible:ring-2 peer-focus-visible:ring-indigo-400 dark:border-gray-600 dark:bg-gray-900">
    ✓
  </span>
  Remember me
</label>
```
## Radio styling
Same idea as checkbox, but `rounded-full` and `type="radio"`.
```
<label className="inline-flex items-center gap-2 cursor-pointer text-sm text-gray-700 dark:text-gray-300">
  <input type="radio" name="plan" className="peer sr-only" />
  <span className="flex h-4 w-4 items-center justify-center rounded-full border border-gray-300 bg-white transition peer-checked:border-indigo-600 peer-focus-visible:ring-2 peer-focus-visible:ring-indigo-400 dark:border-gray-600 dark:bg-gray-900">
    <span className="h-2 w-2 rounded-full bg-indigo-600 opacity-0 transition peer-checked:opacity-100" />
  </span>
  Monthly
</label>
```
Note: `name="plan"` groups radios so only one can be selected.
## Textarea styling
Like input but with `resize` control and `rows`.
```
<textarea
  rows={4}
  placeholder="Write your message…"
  className="w-full resize-y rounded-lg border border-gray-300 bg-white px-3.5 py-2.5 text-sm text-gray-900 placeholder:text-gray-400 focus:border-indigo-500 focus:ring-2 focus:ring-indigo-200 focus:outline-none dark:border-gray-700 dark:bg-gray-900 dark:text-gray-100 dark:placeholder:text-gray-500"
/>
```
Resize options: `resize-none` `resize-y` `resize-x` `resize`.

---

## Placeholder styling
`placeholder:text-*` controls the placeholder color.
```
<input className="placeholder:text-gray-400 dark:placeholder:text-gray-500" />
<input className="placeholder:italic placeholder:text-gray-300" />
```
## Focus styling
Ring + border color change on focus.
```
<input className="border border-gray-300 focus:border-indigo-500 focus:ring-2 focus:ring-indigo-200 focus:outline-none" />
```
- `focus:outline-none` removes the browser outline.
- `focus:border-*` + `focus:ring-*` replaces it with a branded indicator.
- Dark: `dark:focus:border-indigo-400 dark:focus:ring-indigo-500/30`.
## Error states
Border, ring, and helper text turn red.
```
<input
  type="email"
  className="w-full rounded-lg border border-red-500 bg-white px-3.5 py-2.5 text-sm text-gray-900 placeholder:text-gray-400 focus:border-red-500 focus:ring-2 focus:ring-red-200 focus:outline-none dark:border-red-500 dark:bg-gray-900 dark:text-gray-100"
/>
<p className="mt-1.5 text-xs text-red-600 dark:text-red-400">
  Please enter a valid email address.
</p>
```
Extras: `aria-invalid="true"` and a red icon next to the helper text.
## Success states
Same pattern, emerald palette.
```
<input
  className="w-full rounded-lg border border-emerald-500 bg-white px-3.5 py-2.5 text-sm focus:border-emerald-500 focus:ring-2 focus:ring-emerald-200 focus:outline-none dark:border-emerald-500 dark:bg-gray-900"
/>
<p className="mt-1.5 text-xs text-emerald-600 dark:text-emerald-400">
  Username is available ✓
</p>
```
## Disabled states
Mute the surface + block cursor.
```
<input
  disabled
  className="w-full rounded-lg border border-gray-300 bg-gray-100 px-3.5 py-2.5 text-sm text-gray-400 cursor-not-allowed disabled:opacity-60 dark:border-gray-700 dark:bg-gray-800 dark:text-gray-500"
/>
```
Key: `disabled:bg-gray-100`, `disabled:text-gray-400`, `disabled:cursor-not-allowed`.
## Form layouts
Wrap each field in a `<div>` with a label + input + helper text.
```
<div className="space-y-5">
  <div>
    <label className="block text-sm font-medium text-gray-700 dark:text-gray-300">
      Full name
    </label>
    <input className="mt-1.5 w-full rounded-lg border ..." />
  </div>
</div>
```
**Side-by-side fields:**
```
<div className="grid grid-cols-1 sm:grid-cols-2 gap-4">
  <div>First name</div>
  <div>Last name</div>
</div>
```
**Inline label + input:**
```
<label className="flex items-center gap-3">
  <span className="w-24 text-sm font-medium">Email</span>
  <input className="flex-1 rounded-lg border ..." />
</label>
```
## Responsive forms
- Fields stack by default, go side-by-side at `sm:` or `md:`.
- Buttons go full-width on mobile, auto-width on larger screens.
- Use `grid-cols-1 sm:grid-cols-2` for paired fields.
```
<div className="grid grid-cols-1 md:grid-cols-2 gap-4">
  <input className="w-full rounded-lg border ..." />
  <input className="w-full rounded-lg border ..." />
</div>
<div className="mt-6 flex flex-col sm:flex-row gap-3">
  <button className="w-full sm:w-auto ...">Save</button>
  <button className="w-full sm:w-auto ...">Cancel</button>
</div>
```
