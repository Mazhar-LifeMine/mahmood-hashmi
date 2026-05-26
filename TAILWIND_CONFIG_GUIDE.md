# `tailwind.config.ts` — Complete Guide

## What is it?
The brain of Tailwind CSS. Controls everything about how Tailwind works in your project.

---

## Structure
```ts
import type { Config } from 'tailwindcss';

const config: Config = {
  content: [],
  theme: {
    extend: {}
  },
  plugins: [],
};
export default config;
```

---

## 1. `content`
**Purpose:** Tells Tailwind which files to scan for class names. In production, only generates CSS for classes it finds here.

```ts
content: [
  './app/**/*.{js,ts,jsx,tsx,mdx}',
  './components/**/*.{js,ts,jsx,tsx,mdx}',
]
```

**Rules:**
- `**` = any depth, any subfolder — but only inside the specified folder
- Only add folders that have `className` in them
- If `components/` is inside `app/` → one path is enough
- If `components/` is outside `app/` → must add separately

**Best practice:**
```ts
// covers everything inside app/ at any depth
'./app/**/*.{js,ts,jsx,tsx,mdx}'
```

**Bad practice:**
```ts
// forgetting a folder → styles disappear in production
// but work in development → sneaky bug
```

---

## 2. `theme`

Tailwind comes with a default design system:
- Colors (`red-500`, `blue-200`)
- Spacing (`p-4`, `m-8`)
- Font sizes (`text-sm`, `text-xl`)
- Breakpoints (`sm`, `md`, `lg`, `xl`)

### `theme` vs `theme.extend`

| | `theme` | `theme.extend` |
|---|---|---|
| What it does | Replaces defaults | Adds on top of defaults |
| Default colors | Gone ✗ | Still there ✓ |
| Use case | Full custom design system | Add custom tokens |
| Recommended | Rarely | Almost always |

**Best practice — always use extend:**
```ts
theme: {
  extend: {
    colors: {
      brand: {
        500: '#1B4DFF',
        600: '#1640d6',
      }
    },
    fontFamily: {
      sans: ['Inter', 'sans-serif'],
    },
    spacing: {
      128: '32rem',
    },
    borderRadius: {
      '4xl': '2rem',
    },
    screens: {
      '3xl': '1920px',
    },
    boxShadow: {
      brand: '0 2px 8px 0 #1B4DFF22',
    }
  }
}
```

**Bad practice:**
```ts
// Replaces entire theme — loses ALL default colors, spacing etc.
theme: {
  colors: {
    primary: '#1B4DFF'
  }
}
```

---

## 3. `plugins`
**Purpose:** Add extra utilities Tailwind doesn't have by default.

```ts
plugins: []  // empty by default
```

**Popular plugins:**
```ts
plugins: [
  require('@tailwindcss/forms'),       // auto styles form inputs
  require('@tailwindcss/typography'),  // prose class for rich text
  require('@tailwindcss/aspect-ratio') // aspect ratio utilities
]
```

**Custom plugin:**
```ts
plugins: [
  function({ addUtilities }) {
    addUtilities({
      '.center': {
        display: 'flex',
        alignItems: 'center',
        justifyContent: 'center',
      }
    })
  }
]
```

---

## Golden Rules
1. Always use `theme.extend` not `theme`
2. Every folder with `className` must be in `content`
3. Adding a new folder to project → update `content`
4. `plugins` → keep empty unless you specifically need extra utilities
