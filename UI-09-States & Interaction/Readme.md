# States & Interaction
## hover:
Applies styles when the pointer is over the element.
```
<button className="bg-blue-600 hover:bg-blue-700">Hover me</button>
<div className="hover:shadow-lg hover:-translate-y-1 transition">Card</div>
```
## focus:
Applies when the element receives focus (click or keyboard).
```
<input className="border focus:border-blue-500 focus:ring-2 focus:ring-blue-200 outline-none" />
<button className="focus:ring-2 focus:ring-indigo-400">Focus</button>
```
## active:
Applies while the element is being pressed (mouse down / tap).
```
<button className="bg-blue-600 active:bg-blue-800 active:scale-95 transition">
  Press me
</button>
```
## visited:
Applies to already-visited links.
```
<a href="#" className="text-blue-600 visited:text-purple-600">Link</a>
```
## disabled:
Applies when the element has the `disabled` attribute.
```
<button disabled className="bg-gray-300 text-gray-500 disabled:opacity-50 disabled:cursor-not-allowed">
  Disabled
</button>
<input disabled className="disabled:bg-gray-100 disabled:text-gray-400" />
```
## checked:
Applies to checked checkboxes / radio buttons.
```
<input type="checkbox" className="h-5 w-5 accent-blue-600 checked:bg-blue-600" />
```
Also use **`peer-checked:`** on a sibling to style around it (see peer below).
## group-hover:
Style a **child** when the **parent** (with `group`) is hovered.
```
<div className="group rounded-xl border p-6 hover:border-indigo-400 transition">
  <h3 className="group-hover:text-indigo-600 transition">Card title</h3>
  <p className="text-sm text-gray-500">Body text</p>
  <span className="opacity-0 group-hover:opacity-100 transition">→</span>
</div>
```
Add `group` to the parent. Then any child can use `group-hover:*`, `group-focus:*`, etc.
## group-focus:
Style children when the parent has focus inside.
```
<div className="group border rounded-lg p-4 focus-within:ring-2">
  <label className="group-focus:text-indigo-600">Email</label>
  <input className="outline-none w-full" />
</div>
```
## peer
Marks a sibling as the "control" element, so **later siblings** can react to its state.
```
<input type="checkbox" id="notify" className="peer sr-only" />
<label htmlFor="notify" className="cursor-pointer rounded-lg border px-4 py-2 peer-checked:bg-indigo-600 peer-checked:text-white">
  Notify me
</label>
```
Rule: `peer` goes on the control, `peer-*` goes on a **sibling that comes after it**.
## peer-*
Variants: `peer-hover:` `peer-focus:` `peer-checked:` `peer-disabled:` `peer-invalid:` `peer-placeholder-shown:`.
```
<input type="email" className="peer border p-2 rounded" placeholder="Email" />
<p className="hidden peer-invalid:block text-red-600 text-sm">
  Invalid email
</p>
```
```
<input type="checkbox" className="peer sr-only" />
<div className="h-6 w-11 rounded-full bg-gray-300 peer-checked:bg-indigo-600 transition" />
```
## Focus-visible states
Only shows the ring for **keyboard** focus — not mouse clicks. Best for buttons.
```
<button className="focus:outline-none focus-visible:ring-2 focus-visible:ring-indigo-500 focus-visible:ring-offset-2">
  Keyboard only ring
</button>
```
## Button Interaction States
Combine hover, active, disabled, and focus-visible for a complete feel.
```
<button className="
  rounded-lg bg-indigo-600 px-5 py-2.5 text-sm font-semibold text-white
  transition-all
  hover:bg-indigo-500 hover:shadow-lg
  active:scale-95 active:shadow-sm
  focus:outline-none focus-visible:ring-2 focus-visible:ring-indigo-400 focus-visible:ring-offset-2
  disabled:opacity-50 disabled:cursor-not-allowed disabled:hover:bg-indigo-600
">
  Save changes
</button>
```
## Form Interaction States
Combine focus, invalid, disabled, and placeholder for robust forms.
```
<input
  type="email"
  placeholder="you@example.com"
  className="
    w-full rounded-lg border border-gray-300 px-3 py-2 text-sm
    placeholder:text-gray-400
    focus:border-indigo-500 focus:ring-2 focus:ring-indigo-200 focus:outline-none
    invalid:border-red-500 invalid:focus:ring-red-200
    disabled:bg-gray-100 disabled:text-gray-400 disabled:cursor-not-allowed
  "
/>
```
## Card Hover Effects
Combine `group`, `transition`, and transforms for a polished card.
```
<div className="group overflow-hidden rounded-2xl border border-gray-200 bg-white shadow-sm transition-all hover:shadow-xl hover:-translate-y-1 hover:border-indigo-300">
  <img
    src="/thumb.jpg"
    className="aspect-video w-full object-cover transition-transform duration-500 group-hover:scale-105"
  />
  <div className="p-5">
    <h3 className="font-semibold transition-colors group-hover:text-indigo-600">
      Card title
    </h3>
    <p className="mt-1 text-sm text-gray-500 line-clamp-2">
      Short description.
    </p>
    <span className="mt-4 inline-block text-sm text-indigo-600 opacity-0 transition-opacity group-hover:opacity-100">
      Read more →
    </span>
  </div>
</div>
```
