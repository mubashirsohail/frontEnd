# React + TypeScript UI Component Library
## 0. One-time setup
```bash
npm install clsx tailwind-merge
```
## 1. `lib/cn.ts`
```ts
import clsx, { type ClassValue } from "clsx";
import { twMerge } from "tailwind-merge";
export function cn(...args: ClassValue[]) {
  return twMerge(clsx(args));
}
```
## 2. `components/ui/Button.tsx`
```tsx
import { forwardRef, type ButtonHTMLAttributes, type ReactNode } from "react";
import { cn } from "@/lib/cn";
type Variant = "primary" | "secondary" | "ghost" | "danger";
type Size = "sm" | "md" | "lg";
export interface ButtonProps extends ButtonHTMLAttributes<HTMLButtonElement> {
  variant?: Variant;
  size?: Size;
  loading?: boolean;
  children?: ReactNode;
}
export const Button = forwardRef<HTMLButtonElement, ButtonProps>(
  (
    {
      variant = "primary",
      size = "md",
      loading = false,
      disabled,
      className,
      children,
      ...rest
    },
    ref
  ) => {
    const variants: Record<Variant, string> = {
      primary:   "bg-indigo-600 text-white hover:bg-indigo-500",
      secondary:
        "border border-gray-300 bg-white text-gray-800 hover:bg-gray-50 dark:border-gray-700 dark:bg-gray-900 dark:text-gray-100 dark:hover:bg-gray-800",
      ghost:     "text-gray-700 hover:bg-gray-100 dark:text-gray-300 dark:hover:bg-gray-800",
      danger:    "bg-red-600 text-white hover:bg-red-500",
    };

    const sizes: Record<Size, string> = {
      sm: "px-3 py-1.5 text-xs",
      md: "px-4 py-2.5 text-sm",
      lg: "px-5 py-3 text-base",
    };

    return (
      <button
        ref={ref}
        disabled={loading || disabled}
        className={cn(
          "inline-flex items-center justify-center gap-2 rounded-lg font-semibold",
          "transition-all focus:outline-none focus-visible:ring-2 focus-visible:ring-indigo-400 focus-visible:ring-offset-2",
          "disabled:opacity-50 disabled:cursor-not-allowed",
          "motion-reduce:transition-none",
          variants[variant],
          sizes[size],
          className
        )}
        {...rest}
      >
        {loading && (
          <span className="h-4 w-4 animate-spin rounded-full border-2 border-white/40 border-t-white motion-reduce:animate-none" />
        )}
        {children}
      </button>
    );
  }
);

Button.displayName = "Button";
```
## 3. `components/ui/Card.tsx`
```tsx
import type { HTMLAttributes, ReactNode } from "react";
import { cn } from "@/lib/cn";
export interface CardProps extends HTMLAttributes<HTMLDivElement> {
  children?: ReactNode;
}
export function Card({ className, children, ...rest }: CardProps) {
  return (
    <div
      className={cn(
        "rounded-2xl border border-gray-200 bg-white shadow-sm",
        "dark:border-gray-800 dark:bg-gray-900 dark:shadow-none",
        className
      )}
      {...rest}
    >
      {children}
    </div>
  );
}

function Header({ className, children, ...rest }: CardProps) {
  return (
    <div
      className={cn(
        "border-b border-gray-200 p-5 font-semibold dark:border-gray-800",
        className
      )}
      {...rest}
    >
      {children}
    </div>
  );
}

function Body({ className, children, ...rest }: CardProps) {
  return (
    <div className={cn("p-5", className)} {...rest}>
      {children}
    </div>
  );
}

function Footer({ className, children, ...rest }: CardProps) {
  return (
    <div
      className={cn(
        "border-t border-gray-200 p-5 dark:border-gray-800",
        className
      )}
      {...rest}
    >
      {children}
    </div>
  );
}

Card.Header = Header;
Card.Body = Body;
Card.Footer = Footer;
```
## 4. `components/ui/Badge.tsx`
```tsx
import type { HTMLAttributes, ReactNode } from "react";
import { cn } from "@/lib/cn";

type Tone = "neutral" | "brand" | "success" | "warning" | "error";

export interface BadgeProps extends HTMLAttributes<HTMLSpanElement> {
  tone?: Tone;
  dot?: boolean;
  children?: ReactNode;
}

export function Badge({
  tone = "neutral",
  dot = false,
  className,
  children,
  ...rest
}: BadgeProps) {
  const tones: Record<Tone, string> = {
    neutral: "bg-gray-100 text-gray-700 dark:bg-gray-800 dark:text-gray-300",
    brand:   "bg-indigo-50 text-indigo-700 dark:bg-indigo-500/10 dark:text-indigo-400",
    success: "bg-emerald-50 text-emerald-700 dark:bg-emerald-500/10 dark:text-emerald-400",
    warning: "bg-amber-50 text-amber-700 dark:bg-amber-500/10 dark:text-amber-400",
    error:   "bg-red-50 text-red-700 dark:bg-red-500/10 dark:text-red-400",
  };

  return (
    <span
      className={cn(
        "inline-flex items-center gap-1.5 rounded-full px-2.5 py-1 text-xs font-semibold",
        tones[tone],
        className
      )}
      {...rest}
    >
      {dot && <span className="h-1.5 w-1.5 rounded-full bg-current" />}
      {children}
    </span>
  );
}
```
## 5. `components/ui/Alert.tsx`
```tsx
import type { HTMLAttributes, ReactNode } from "react";
import { cn } from "@/lib/cn";

type Tone = "info" | "success" | "warning" | "error";

export interface AlertProps extends HTMLAttributes<HTMLDivElement> {
  tone?: Tone;
  title?: string;
  children?: ReactNode;
}

export function Alert({
  tone = "info",
  title,
  className,
  children,
  ...rest
}: AlertProps) {
  const tones: Record<Tone, string> = {
    info:    "border-blue-200 bg-blue-50 text-blue-800 dark:border-blue-500/30 dark:bg-blue-500/10 dark:text-blue-300",
    success: "border-emerald-200 bg-emerald-50 text-emerald-800 dark:border-emerald-500/30 dark:bg-emerald-500/10 dark:text-emerald-300",
    warning: "border-amber-200 bg-amber-50 text-amber-800 dark:border-amber-500/30 dark:bg-amber-500/10 dark:text-amber-300",
    error:   "border-red-200 bg-red-50 text-red-800 dark:border-red-500/30 dark:bg-red-500/10 dark:text-red-300",
  };

  return (
    <div
      role="alert"
      className={cn("rounded-lg border p-4", tones[tone], className)}
      {...rest}
    >
      {title && <p className="text-sm font-semibold">{title}</p>}
      <p className="mt-0.5 text-sm opacity-90">{children}</p>
    </div>
  );
}
```
## 6. `components/ui/Input.tsx`
```tsx
import {
  forwardRef,
  type InputHTMLAttributes,
  type ReactNode,
} from "react";
import { cn } from "@/lib/cn";

/* ---------- Field wrapper ---------- */
export interface FieldProps {
  id: string;
  label: string;
  hint?: string;
  error?: string;
  className?: string;
  children: ReactNode;
}

export function Field({
  id,
  label,
  hint,
  error,
  className,
  children,
}: FieldProps) {
  return (
    <div className={className}>
      <label
        htmlFor={id}
        className="block text-sm font-medium text-gray-700 dark:text-gray-300"
      >
        {label}
      </label>
      <div className="mt-1.5">{children}</div>
      <p
        className={cn(
          "mt-1.5 text-xs",
          error
            ? "text-red-600 dark:text-red-400"
            : "text-gray-400 dark:text-gray-500"
        )}
      >
        {error || hint || "\u00A0"}
      </p>
    </div>
  );
}

/* ---------- Input ---------- */
export interface InputProps
  extends InputHTMLAttributes<HTMLInputElement> {
  invalid?: boolean;
}

export const Input = forwardRef<HTMLInputElement, InputProps>(
  ({ invalid, className, ...rest }, ref) => (
    <input
      ref={ref}
      aria-invalid={invalid}
      className={cn(
        "w-full rounded-lg border bg-white px-3.5 py-2.5 text-sm text-gray-900",
        "placeholder:text-gray-400",
        "focus:outline-none focus:ring-2",
        "dark:bg-gray-900 dark:text-gray-100 dark:placeholder:text-gray-500",
        invalid
          ? "border-red-500 focus:border-red-500 focus:ring-red-200 dark:border-red-500 dark:focus:ring-red-500/30"
          : "border-gray-300 focus:border-indigo-500 focus:ring-indigo-200 dark:border-gray-700 dark:focus:border-indigo-400 dark:focus:ring-indigo-500/30",
        className
      )}
      {...rest}
    />
  )
);

Input.displayName = "Input";
```
## 7. `components/ui/Spinner.tsx`
```tsx
import type { HTMLAttributes } from "react";
import { cn } from "@/lib/cn";

type Size = "sm" | "md" | "lg";

export interface SpinnerProps extends HTMLAttributes<HTMLSpanElement> {
  size?: Size;
}

export function Spinner({ size = "md", className, ...rest }: SpinnerProps) {
  const sizes: Record<Size, string> = {
    sm: "h-4 w-4",
    md: "h-6 w-6",
    lg: "h-10 w-10",
  };

  return (
    <span
      className={cn(
        "inline-block animate-spin rounded-full border-2 border-gray-300 border-t-indigo-600",
        "motion-reduce:animate-none",
        sizes[size],
        className
      )}
      {...rest}
    />
  );
}

export interface SkeletonProps extends HTMLAttributes<HTMLDivElement> {}

export function Skeleton({ className, ...rest }: SkeletonProps) {
  return (
    <div
      className={cn(
        "animate-pulse rounded-md bg-gray-200 dark:bg-gray-800",
        "motion-reduce:animate-none",
        className
      )}
      {...rest}
    />
  );
}
```
## 8. `components/ui/Modal.tsx`
```tsx
"use client";
import { useEffect, type ReactNode } from "react";
import { cn } from "@/lib/cn";

export interface ModalProps {
  open: boolean;
  onClose: () => void;
  title: string;
  className?: string;
  children: ReactNode;
}

export function Modal({
  open,
  onClose,
  title,
  className,
  children,
}: ModalProps) {
  useEffect(() => {
    if (!open) return;

    function onKey(e: KeyboardEvent) {
      if (e.key === "Escape") onClose();
    }

    document.addEventListener("keydown", onKey);
    document.body.style.overflow = "hidden";

    return () => {
      document.removeEventListener("keydown", onKey);
      document.body.style.overflow = "";
    };
  }, [open, onClose]);

  if (!open) return null;

  return (
    <div className="fixed inset-0 z-50 flex items-center justify-center p-4">
      <div
        onClick={onClose}
        className="absolute inset-0 bg-black/50 backdrop-blur-sm"
      />
      <div
        role="dialog"
        aria-modal="true"
        className={cn(
          "relative z-10 w-full max-w-md rounded-2xl bg-white p-6 shadow-2xl",
          "dark:bg-gray-900",
          className
        )}
      >
        <div className="flex items-start justify-between gap-4">
          <h2 className="text-lg font-bold tracking-tight">{title}</h2>
          <button
            onClick={onClose}
            aria-label="Close"
            className="rounded-full p-1 text-gray-400 transition-colors hover:bg-gray-100 hover:text-gray-700 focus:outline-none focus-visible:ring-2 focus-visible:ring-indigo-400 dark:hover:bg-gray-800 dark:hover:text-gray-200"
          >
            ✕
          </button>
        </div>
        <div className="mt-4">{children}</div>
      </div>
    </div>
  );
}
```
## 9. `components/ui/Navbar.tsx`
```tsx
"use client";
import { useState, type ReactNode } from "react";
import { cn } from "@/lib/cn";

export interface NavLink {
  href: string;
  label: string;
}

export interface NavbarProps {
  brand?: ReactNode;
  links?: NavLink[];
  right?: ReactNode;
  className?: string;
}

export function Navbar({
  brand = "Brand",
  links = [],
  right,
  className,
}: NavbarProps) {
  const [open, setOpen] = useState(false);

  return (
    <header
      className={cn(
        "sticky top-0 z-40 border-b border-gray-200 bg-white/80 backdrop-blur",
        "dark:border-gray-800 dark:bg-gray-950/80",
        className
      )}
    >
      <nav className="mx-auto flex max-w-6xl items-center justify-between px-6 py-4">
        <span className="font-bold tracking-tight">{brand}</span>

        <div className="hidden gap-6 text-sm md:flex">
          {links.map((l) => (
            <a
              key={l.href}
              href={l.href}
              className="text-gray-600 transition-colors hover:text-gray-900 dark:text-gray-400 dark:hover:text-gray-100"
            >
              {l.label}
            </a>
          ))}
        </div>

        <div className="flex items-center gap-3">
          {right}
          <button
            aria-label="Toggle menu"
            onClick={() => setOpen((v) => !v)}
            className="rounded-lg p-2 hover:bg-gray-100 focus:outline-none focus-visible:ring-2 focus-visible:ring-indigo-400 md:hidden dark:hover:bg-gray-800"
          >
            {open ? "✕" : "☰"}
          </button>
        </div>
      </nav>

      {open && (
        <div className="flex flex-col gap-3 border-t border-gray-200 px-6 py-4 text-sm md:hidden dark:border-gray-800">
          {links.map((l) => (
            <a
              key={l.href}
              href={l.href}
              className="text-gray-700 dark:text-gray-300"
            >
              {l.label}
            </a>
          ))}
        </div>
      )}
    </header>
  );
}
```
## 10. `components/ui/Sidebar.tsx`
```tsx
import type { ReactNode } from "react";
import { cn } from "@/lib/cn";

export interface SidebarItem {
  href: string;
  label: string;
  icon?: ReactNode;
}

export interface SidebarProps {
  items?: SidebarItem[];
  active?: string;
  brand?: ReactNode;
  className?: string;
}

export function Sidebar({
  items = [],
  active = "",
  brand = "Brand",
  className,
}: SidebarProps) {
  return (
    <aside
      className={cn(
        "fixed left-0 top-0 hidden h-screen w-64 flex-col md:flex",
        "border-r border-gray-200 bg-white",
        "dark:border-gray-800 dark:bg-gray-900",
        className
      )}
    >
      <div className="flex h-16 items-center border-b border-gray-200 px-6 font-bold dark:border-gray-800">
        {brand}
      </div>

      <nav className="flex-1 space-y-1 p-3">
        {items.map((it) => {
          const isActive = active === it.href;
          return (
            <a
              key={it.href}
              href={it.href}
              className={cn(
                "flex items-center gap-3 rounded-lg px-3 py-2 text-sm font-medium transition",
                isActive
                  ? "bg-indigo-50 text-indigo-700 dark:bg-indigo-500/10 dark:text-indigo-400"
                  : "text-gray-600 hover:bg-gray-100 dark:text-gray-400 dark:hover:bg-gray-800"
              )}
            >
              {it.icon && <span className="text-base">{it.icon}</span>}
              {it.label}
            </a>
          );
        })}
      </nav>
    </aside>
  );
}
```
## 11. `components/ui/index.ts` — Barrel export
```ts
export * from "./Button";
export * from "./Card";
export * from "./Badge";
export * from "./Alert";
export * from "./Input";
export * from "./Spinner";
export * from "./Modal";
export * from "./Navbar";
export * from "./Sidebar";
```

Now consumers can do:

```ts
import { Button, Card, Badge, Alert, Input, Field, Spinner, Skeleton, Modal, Navbar, Sidebar } from "@/components/ui";
```
## 12. `app/page.tsx` — Showcase
```tsx
"use client";
import { useEffect, useState } from "react";
import {
  Alert,
  Badge,
  Button,
  Card,
  Field,
  Input,
  Modal,
  Navbar,
  Sidebar,
  Skeleton,
  Spinner,
} from "@/components/ui";

export default function Showcase() {
  const [dark, setDark] = useState(false);
  const [ready, setReady] = useState(false);
  const [modalOpen, setModalOpen] = useState(false);

  useEffect(() => {
    setDark(document.documentElement.classList.contains("dark"));
    setReady(true);
  }, []);

  function toggleTheme() {
    const next = !dark;
    setDark(next);
    document.documentElement.classList.toggle("dark", next);
    localStorage.setItem("theme", next ? "dark" : "light");
  }
  return (
    <div className="min-h-screen bg-gray-50 text-gray-900 dark:bg-gray-950 dark:text-gray-100">
      <Navbar
        brand={
          <span>
            UI<span className="text-indigo-600 dark:text-indigo-400">Kit</span>
          </span>
        }
        links={[
          { href: "#buttons", label: "Buttons" },
          { href: "#cards", label: "Cards" },
          { href: "#feedback", label: "Feedback" },
          { href: "#forms", label: "Forms" },
        ]}
        right={
          <button
            onClick={toggleTheme}
            aria-label="Toggle theme"
            className="relative flex h-9 w-16 items-center rounded-full border border-gray-300 bg-gray-100 px-1 transition-colors focus:outline-none focus-visible:ring-2 focus-visible:ring-indigo-400 dark:border-gray-700 dark:bg-gray-800"
          >
            <span
              className={`flex h-7 w-7 items-center justify-center rounded-full bg-white text-xs shadow transition-transform duration-300 dark:bg-gray-950 ${
                ready && dark ? "translate-x-7" : "translate-x-0"
              }`}
            >
              {ready && dark ? "🌙" : "☀️"}
            </span>
          </button>
        }
      />

      <main className="mx-auto max-w-6xl space-y-12 px-6 py-12">
        <section className="text-center">
          <Badge tone="brand" dot>
            Reusable Components
          </Badge>
          <h1 className="mt-4 text-3xl font-extrabold tracking-tight md:text-5xl">
            React + TS UI Kit
          </h1>
          <p className="mx-auto mt-4 max-w-xl text-sm text-gray-600 md:text-base dark:text-gray-400">
            Fully typed, accessible, dark-mode-ready components.
          </p>
        </section>

        <Section id="buttons" title="Button">
          <div className="flex flex-wrap gap-3">
            <Button>Primary</Button>
            <Button variant="secondary">Secondary</Button>
            <Button variant="ghost">Ghost</Button>
            <Button variant="danger">Delete</Button>
            <Button loading>Loading</Button>
            <Button disabled>Disabled</Button>
          </div>
        </Section>

        <Section id="cards" title="Card">
          <div className="grid grid-cols-1 gap-6 md:grid-cols-2">
            <Card>
              <Card.Header>Project Settings</Card.Header>
              <Card.Body>
                <p className="text-sm text-gray-600 dark:text-gray-400">
                  Manage your project details and preferences.
                </p>
              </Card.Body>
              <Card.Footer>
                <Button size="sm">Save changes</Button>
              </Card.Footer>
            </Card>

            <Card className="overflow-hidden p-0">
              <div className="h-40 bg-gradient-to-br from-indigo-500 to-purple-600" />
              <Card.Body>
                <div className="flex items-center justify-between">
                  <h3 className="font-semibold">Aero Headphones</h3>
                  <Badge tone="brand">New</Badge>
                </div>
                <p className="mt-1 text-sm text-gray-500 dark:text-gray-400">
                  Wireless · 40h battery
                </p>
              </Card.Body>
            </Card>
          </div>
        </Section>

        <Section id="feedback" title="Feedback">
          <div className="grid grid-cols-1 gap-6 md:grid-cols-2">
            <div className="space-y-3">
              <Alert tone="info" title="Update available">
                A new version is ready to install.
              </Alert>
              <Alert tone="success" title="Saved">
                Your changes have been saved.
              </Alert>
              <Alert tone="warning" title="Careful">
                This action can't be undone.
              </Alert>
              <Alert tone="error" title="Error">
                Something went wrong. Try again.
              </Alert>
            </div>

            <Card>
              <Card.Header>Loading States</Card.Header>
              <Card.Body className="space-y-4">
                <div className="flex items-center gap-4">
                  <Spinner size="sm" />
                  <Spinner size="md" />
                  <Spinner size="lg" />
                  <Button loading size="sm">
                    Saving
                  </Button>
                </div>
                <div className="space-y-2">
                  <Skeleton className="h-4 w-3/4" />
                  <Skeleton className="h-4 w-1/2" />
                  <Skeleton className="h-4 w-2/3" />
                </div>
              </Card.Body>
            </Card>
          </div>

          <div className="mt-6">
            <Button onClick={() => setModalOpen(true)}>Open Modal</Button>
          </div>
        </Section>

        <Section id="forms" title="Form Controls">
          <Card>
            <Card.Body className="grid grid-cols-1 gap-6 md:grid-cols-2">
              <Field id="name" label="Full name" hint="At least 2 characters.">
                <Input id="name" placeholder="Ali Raza" />
              </Field>

              <Field id="email" label="Email" error="Invalid email format.">
                <Input
                  id="email"
                  placeholder="you@example.com"
                  invalid
                  defaultValue="not-an-email"
                />
              </Field>

              <Field id="pass" label="Password" hint="Minimum 8 characters.">
                <Input id="pass" type="password" placeholder="••••••••" />
              </Field>

              <Field id="role" label="Role">
                <select
                  id="role"
                  className="w-full rounded-lg border border-gray-300 bg-white px-3.5 py-2.5 text-sm focus:border-indigo-500 focus:ring-2 focus:ring-indigo-200 focus:outline-none dark:border-gray-700 dark:bg-gray-900 dark:text-gray-100 dark:focus:border-indigo-400 dark:focus:ring-indigo-500/30"
                >
                  <option>Choose a role…</option>
                  <option>Developer</option>
                  <option>Designer</option>
                </select>
              </Field>
            </Card.Body>
          </Card>
        </Section>
      </main>

      <Modal
        open={modalOpen}
        onClose={() => setModalOpen(false)}
        title="Confirm action"
      >
        <p className="text-sm text-gray-600 dark:text-gray-400">
          Are you sure you want to continue? This cannot be undone.
        </p>
        <div className="mt-6 flex justify-end gap-3">
          <Button variant="secondary" onClick={() => setModalOpen(false)}>
            Cancel
          </Button>
          <Button variant="danger" onClick={() => setModalOpen(false)}>
            Confirm
          </Button>
        </div>
      </Modal>

      <footer className="border-t border-gray-200 py-8 text-center text-xs uppercase tracking-widest text-gray-400 dark:border-gray-800 dark:text-gray-500">
        UIKit · React + TS + Tailwind v4
      </footer>
    </div>
  );
}
/* ---------- Local helper ---------- */
interface SectionProps {
  id: string;
  title: string;
  children: React.ReactNode;
}
function Section({ id, title, children }: SectionProps) {
  return (
    <section id={id} className="scroll-mt-24">
      <h2 className="mb-5 text-xl font-bold tracking-tight md:text-2xl">
        {title}
      </h2>
      {children}
    </section>
  );
}
```
## TypeScript Specifics — What You Get

| Feature | Where |
|---|---|
| **Union types for variants** | `type Variant = "primary" \| "secondary" \| "ghost" \| "danger"` |
| **Extends native props** | `interface ButtonProps extends ButtonHTMLAttributes<HTMLButtonElement>` |
| **Generic HTML attrs pass-through** | `...rest` still typed — `onClick`, `type`, `aria-*` all work |
| **Ref forwarding** | `forwardRef<HTMLButtonElement, ButtonProps>` for DOM access |
| **`Record<Union, string>` class maps** | Compile-time guarantee every variant exists |
| **Optional props with defaults** | `variant?: Variant` + `variant = "primary"` |
| **Barrel export** | `components/ui/index.ts` re-exports everything |
| **`displayName`** | For better React DevTools output |
| **Children typing** | `ReactNode` for anything renderable |
## File Tree
```
your-project/
├── app/
│   ├── globals.css
│   ├── layout.tsx
│   └── page.tsx
├── lib/
│   └── cn.ts
└── components/
    └── ui/
        ├── index.ts
        ├── Button.tsx
        ├── Card.tsx
        ├── Badge.tsx
        ├── Alert.tsx
        ├── Input.tsx       (Field + Input)
        ├── Spinner.tsx     (Spinner + Skeleton)
        ├── Modal.tsx
        ├── Navbar.tsx
        └── Sidebar.tsx
```
## `tsconfig.json` — ensure the `@/*` path
```json
{
  "compilerOptions": {
    "baseUrl": ".",
    "paths": {
      "@/*": ["./*"]
    }
  }
}
```
## Test Checklist
- `npm run build` — no TypeScript errors
- Autocomplete in editor shows `variant`, `size`, `loading` on `<Button>`
- Wrong variant (`<Button variant="foo" />`) → red squiggle
- `<Button onClick={(e) => ...}>` — `e` typed as `React.MouseEvent<HTMLButtonElement>`
- `ref` on `<Button>` and `<Input>` — typed correctly
- Modal Escape / backdrop close works
- Theme toggle flips every component
- Barrel import: `import { Button } from "@/components/ui"` works
