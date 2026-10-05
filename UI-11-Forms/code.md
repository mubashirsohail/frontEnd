```
// app/page.jsx
"use client";
import { useState } from "react";

export default function UI11() {
  const [form, setForm] = useState({
    name: "",
    email: "",
    password: "",
    confirm: "",
    role: "",
    terms: false,
  });

  const [touched, setTouched] = useState({});
  const [submitted, setSubmitted] = useState(false);
  const [loading, setLoading] = useState(false);

  /* ---------- validation rules ---------- */
  const rules = {
    name: form.name.trim().length >= 2,
    email: /^[^\s@]+@[^\s@]+\.[^\s@]+$/.test(form.email),
    password: form.password.length >= 8,
    confirm: form.confirm.length > 0 && form.confirm === form.password,
    role: form.role !== "",
    terms: form.terms === true,
  };

  const allValid = Object.values(rules).every(Boolean);

  /* ---------- handlers ---------- */
  const update = (key, value) => setForm((f) => ({ ...f, [key]: value }));
  const blur = (key) => setTouched((t) => ({ ...t, [key]: true }));

  const showError = (key) => (touched[key] || submitted) && !rules[key];
  const showSuccess = (key) => (touched[key] || submitted) && rules[key];

  function handleSubmit(e) {
    e.preventDefault();
    setSubmitted(true);
    if (!allValid) return;
    setLoading(true);
    setTimeout(() => setLoading(false), 1500);
  }

  /* ---------- password strength ---------- */
  const strength = getStrength(form.password);

  return (
    <div className="min-h-screen bg-gray-50 px-6 py-16 dark:bg-gray-950">

      <div className="mx-auto w-full max-w-xl">

        {/* ===== Header ===== */}
        <div className="text-center mb-8">
          <span className="text-2xl font-extrabold tracking-tight text-gray-900 dark:text-gray-100">
            Neur<span className="text-indigo-600 dark:text-indigo-400">o</span>n
          </span>
          <h1 className="mt-4 text-2xl md:text-3xl font-bold tracking-tight text-gray-900 dark:text-gray-100">
            Create your account
          </h1>
          <p className="mt-2 text-sm text-gray-500 dark:text-gray-400">
            Get started in less than a minute.
          </p>
        </div>

        {/* ===== Card ===== */}
        <form
          onSubmit={handleSubmit}
          noValidate
          className="rounded-2xl border border-gray-200 bg-white p-8 shadow-sm dark:border-gray-800 dark:bg-gray-900 dark:shadow-none"
        >

          {/* -------- Full name -------- */}
          <Field
            id="name"
            label="Full name"
            hint="At least 2 characters."
            error={showError("name") && "Name must be at least 2 characters."}
            success={showSuccess("name") && "Looks good!"}
          >
            <input
              id="name"
              type="text"
              value={form.name}
              onChange={(e) => update("name", e.target.value)}
              onBlur={() => blur("name")}
              placeholder="Ali Raza"
              autoComplete="name"
              aria-invalid={showError("name")}
              className={inputCls(showError("name"), showSuccess("name"))}
            />
          </Field>

          {/* -------- Email -------- */}
          <Field
            id="email"
            label="Email address"
            error={showError("email") && "Please enter a valid email address."}
            success={showSuccess("email") && "Email looks good!"}
          >
            <input
              id="email"
              type="email"
              value={form.email}
              onChange={(e) => update("email", e.target.value)}
              onBlur={() => blur("email")}
              placeholder="you@example.com"
              autoComplete="email"
              aria-invalid={showError("email")}
              className={inputCls(showError("email"), showSuccess("email"))}
            />
          </Field>

          {/* -------- Password + Confirm (side by side) -------- */}
          <div className="grid grid-cols-1 sm:grid-cols-2 gap-4">
            <Field
              id="password"
              label="Password"
              error={showError("password") && "Minimum 8 characters."}
              success={showSuccess("password") && "Strong enough."}
            >
              <input
                id="password"
                type="password"
                value={form.password}
                onChange={(e) => update("password", e.target.value)}
                onBlur={() => blur("password")}
                placeholder="••••••••"
                autoComplete="new-password"
                aria-invalid={showError("password")}
                className={inputCls(showError("password"), showSuccess("password"))}
              />
            </Field>

            <Field
              id="confirm"
              label="Confirm password"
              error={showError("confirm") && "Passwords do not match."}
              success={showSuccess("confirm") && "Passwords match."}
            >
              <input
                id="confirm"
                type="password"
                value={form.confirm}
                onChange={(e) => update("confirm", e.target.value)}
                onBlur={() => blur("confirm")}
                placeholder="••••••••"
                autoComplete="new-password"
                aria-invalid={showError("confirm")}
                className={inputCls(showError("confirm"), showSuccess("confirm"))}
              />
            </Field>
          </div>

          {/* -------- Password strength bar -------- */}
          {form.password.length > 0 && (
            <div className="mt-3">
              <div className="flex gap-1.5">
                {[1, 2, 3, 4].map((n) => (
                  <span
                    key={n}
                    className={`h-1 flex-1 rounded-full transition-colors ${
                      n <= strength.score ? strength.bar : "bg-gray-200 dark:bg-gray-800"
                    }`}
                  />
                ))}
              </div>
              <p className={`mt-1.5 text-xs ${strength.text}`}>
                {strength.label}
              </p>
            </div>
          )}

          {/* -------- Role (select) -------- */}
          <Field
            id="role"
            label="Role"
            error={showError("role") && "Please choose a role."}
            success={showSuccess("role") && "Great choice."}
          >
            <div className="relative mt-1.5">
              <select
                id="role"
                value={form.role}
                onChange={(e) => update("role", e.target.value)}
                onBlur={() => blur("role")}
                aria-invalid={showError("role")}
                className={`${inputCls(showError("role"), showSuccess("role"))} appearance-none pr-10 mt-0`}
              >
                <option value="">Choose a role…</option>
                <option value="developer">Developer</option>
                <option value="designer">Designer</option>
                <option value="manager">Product Manager</option>
                <option value="founder">Founder</option>
              </select>
              <span className="pointer-events-none absolute right-3 top-1/2 -translate-y-1/2 text-gray-400 dark:text-gray-500">
                ▾
              </span>
            </div>
          </Field>

          {/* -------- Terms checkbox -------- */}
          <div className="mt-5">
            <label className="inline-flex items-start gap-3 cursor-pointer select-none">
              <input
                type="checkbox"
                checked={form.terms}
                onChange={(e) => update("terms", e.target.checked)}
                onBlur={() => blur("terms")}
                className="peer sr-only"
              />
              <span className="mt-0.5 flex h-4 w-4 shrink-0 items-center justify-center rounded border border-gray-300 bg-white text-[10px] text-white transition peer-checked:border-indigo-600 peer-checked:bg-indigo-600 peer-focus-visible:ring-2 peer-focus-visible:ring-indigo-400 dark:border-gray-600 dark:bg-gray-900">
                ✓
              </span>
              <span className="text-sm text-gray-600 dark:text-gray-400">
                I agree to the{" "}
                <a href="#" className="font-medium text-indigo-600 dark:text-indigo-400 hover:underline">
                  Terms of Service
                </a>{" "}
                and{" "}
                <a href="#" className="font-medium text-indigo-600 dark:text-indigo-400 hover:underline">
                  Privacy Policy
                </a>
                .
              </span>
            </label>

            {showError("terms") && (
              <p className="mt-1.5 text-xs text-red-600 dark:text-red-400">
                You must accept the terms to continue.
              </p>
            )}
          </div>

          {/* -------- Submit -------- */}
          <button
            type="submit"
            disabled={!allValid || loading}
            className="
              mt-6 w-full rounded-xl bg-indigo-600 px-5 py-3
              text-sm font-semibold text-white shadow-sm
              transition-all
              hover:bg-indigo-500 hover:shadow-lg hover:-translate-y-0.5
              active:translate-y-0 active:shadow-sm
              focus:outline-none focus-visible:ring-2 focus-visible:ring-indigo-400 focus-visible:ring-offset-2
              disabled:opacity-50 disabled:cursor-not-allowed
              disabled:hover:bg-indigo-600 disabled:hover:shadow-sm disabled:hover:translate-y-0
              dark:focus-visible:ring-offset-gray-900
            "
          >
            {loading ? (
              <span className="inline-flex items-center justify-center gap-2">
                <span className="h-4 w-4 animate-spin rounded-full border-2 border-white/40 border-t-white" />
                Creating account…
              </span>
            ) : (
              "Create account"
            )}
          </button>

        </form>

        {/* ===== Footer ===== */}
        <p className="mt-6 text-center text-sm text-gray-500 dark:text-gray-400">
          Already have an account?{" "}
          <a
            href="#"
            className="font-medium text-indigo-600 dark:text-indigo-400 hover:underline"
          >
            Sign in
          </a>
        </p>

      </div>
    </div>
  );
}

/* ================= Field wrapper ================= */
function Field({ id, label, hint, error, success, children }) {
  return (
    <div className="mt-5 first:mt-0">
      <label
        htmlFor={id}
        className="block text-sm font-medium text-gray-700 dark:text-gray-300"
      >
        {label}
      </label>

      {children}

      {/* Helper / error / success message — reserves space so layout doesn't jump */}
      <p
        className={`mt-1.5 text-xs transition-colors ${
          error
            ? "text-red-600 dark:text-red-400"
            : success
            ? "text-emerald-600 dark:text-emerald-400"
            : "text-gray-400 dark:text-gray-500"
        }`}
      >
        {error || success || hint || "\u00A0"}
      </p>
    </div>
  );
}

/* ================= Input class builder ================= */
function inputCls(hasError, hasSuccess) {
  const base =
    "mt-1.5 w-full rounded-lg border bg-white px-3.5 py-2.5 text-sm text-gray-900 placeholder:text-gray-400 transition focus:outline-none focus:ring-2 dark:bg-gray-900 dark:text-gray-100 dark:placeholder:text-gray-500";

  if (hasError) {
    return `${base} border-red-500 focus:border-red-500 focus:ring-red-200 dark:border-red-500 dark:focus:ring-red-500/30`;
  }
  if (hasSuccess) {
    return `${base} border-emerald-500 focus:border-emerald-500 focus:ring-emerald-200 dark:border-emerald-500 dark:focus:ring-emerald-500/30`;
  }
  return `${base} border-gray-300 focus:border-indigo-500 focus:ring-indigo-200 dark:border-gray-700 dark:focus:border-indigo-400 dark:focus:ring-indigo-500/30`;
}

/* ================= Password strength ================= */
function getStrength(pw) {
  let score = 0;
  if (pw.length >= 8) score++;
  if (/[A-Z]/.test(pw)) score++;
  if (/[0-9]/.test(pw)) score++;
  if (/[^A-Za-z0-9]/.test(pw)) score++;

  const map = {
    0: { label: "Too short", bar: "bg-red-500", text: "text-red-600 dark:text-red-400" },
    1: { label: "Weak", bar: "bg-red-500", text: "text-red-600 dark:text-red-400" },
    2: { label: "Fair", bar: "bg-amber-500", text: "text-amber-600 dark:text-amber-400" },
    3: { label: "Good", bar: "bg-indigo-500", text: "text-indigo-600 dark:text-indigo-400" },
    4: { label: "Strong", bar: "bg-emerald-500", text: "text-emerald-600 dark:text-emerald-400" },
  };
  return { score, ...map[score] };
}
```
