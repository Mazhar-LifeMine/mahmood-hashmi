# `globals.css` — Complete Guide

## What is it?
The **one global stylesheet** in a Next.js App Router project. Loads on every page. Lives at `app/globals.css` and is imported once in `app/layout.tsx`.

---

## Default content

```css
@tailwind base;
@tailwind components;
@tailwind utilities;
```

These 3 lines ARE the entire Tailwind system:

| Directive | What it injects |
|---|---|
| `@tailwind base` | CSS reset — removes browser margins, normalizes headings etc. |
| `@tailwind components` | Component classes from plugins |
| `@tailwind utilities` | All `p-4`, `text-sm`, `flex` classes you use |

**Order matters — never rearrange them.**

---

## What you PUT here

### 1. CSS Variables (design tokens)
```css
:root {
  --primary: #1B4DFF;
  --font-main: 'Inter', sans-serif;
}
```
Use in components: `color: var(--primary)`

### 2. Body / html baseline
```css
body {
  font-family: var(--font-main);
  color: #1C222B;
  background: #fff;
}
```

### 3. Universal reset
```css
* {
  box-sizing: border-box;
}
```

### 4. Scrollbar styles
```css
::-webkit-scrollbar {
  width: 6px;
}
::-webkit-scrollbar-thumb {
  background: #ccc;
  border-radius: 4px;
}
```

### 5. @font-face declarations (if self-hosting fonts)
```css
@font-face {
  font-family: 'Inter';
  src: url('/fonts/Inter.woff2') format('woff2');
}
```

---

## What you do NOT put here

- Component styles → use CSS Modules (`Button.module.css`) or Tailwind classes
- Page-specific styles → keep them in that page's file
- Anything that can be a Tailwind utility class → use the class directly

---

## How it connects to layout.tsx

```tsx
// app/layout.tsx
import './globals.css'  // ← imported here, applies everywhere

export default function RootLayout({ children }) {
  return (
    <html lang="en">
      <body>{children}</body>
    </html>
  )
}
```

**One import. Every page gets it automatically.**

---

## Full example globals.css

```css
@tailwind base;
@tailwind components;
@tailwind utilities;

:root {
  --primary: #1B4DFF;
  --primary-dark: #1640d6;
  --font-main: 'Inter', sans-serif;
}

* {
  box-sizing: border-box;
}

body {
  font-family: var(--font-main);
  color: #1C222B;
  background: #ffffff;
}
```

---

## Golden Rules

1. Only **one** `globals.css` — imported once in `layout.tsx`
2. Always keep the 3 `@tailwind` directives at the top
3. CSS variables go inside `:root {}`
4. Body font, reset, scrollbar = goes here
5. Component/page-specific styles = do NOT go here
