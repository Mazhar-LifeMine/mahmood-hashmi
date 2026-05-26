# `layout.tsx` — Complete Guide

## What is it?
The **most important file** in Next.js App Router. Every page goes through it. Lives at `app/layout.tsx`.

---

## Default structure

```tsx
import type { Metadata } from 'next'
import './globals.css'

export const metadata: Metadata = {
  title: 'My App',
  description: 'My App Description',
}

export default function RootLayout({
  children,
}: {
  children: React.ReactNode
}) {
  return (
    <html lang="en">
      <body>
        {children}
      </body>
    </html>
  )
}
```

---

## The most important concept

`{children}` = whatever page you're on right now.

```
layout.tsx  (always loaded, never re-renders)
    └── page.tsx  (changes per route)
```

- `/dashboard` → layout loads → puts `dashboard/page.tsx` in `{children}`
- `/profile` → layout loads → puts `profile/page.tsx` in `{children}`

**Layout never re-renders. Only `{children}` changes.**

---

## Point 1 — `globals.css` import

```tsx
import './globals.css'
```

- Import once here = every page gets global styles automatically
- Never import `globals.css` in individual pages
- If imported in only one page → other pages look broken

---

## Point 2 — `metadata`

```tsx
export const metadata: Metadata = {
  title: 'LifeMine Vendor',
  description: 'Vendor portal for LifeMine services',
  keywords: ['vendor', 'services', 'lifemine'],
  icons: {
    icon: '/favicon.ico',
  },
}
```

- Controls browser tab name and Google search results
- Next.js auto-converts to `<head>` tags — never write `<head>` manually
- `layout.tsx` metadata = default for entire app
- Individual pages can export their own `metadata` to override

```tsx
// app/dashboard/page.tsx — overrides layout title for this page only
export const metadata: Metadata = {
  title: 'Dashboard — LifeMine',
}
```

---

## Point 3 — Providers (Redux + MUI Theme)

### The problem
`layout.tsx` is a **Server Component** by default.
Redux and MUI are **client-side** — they need browser APIs.
You cannot put providers directly in `layout.tsx`.

### The solution — separate `providers.tsx`

**Step 1 — create `app/providers.tsx`:**
```tsx
'use client'

import { Provider } from 'react-redux'
import { store } from '@/store/store'
import { ThemeProvider } from '@mui/material/styles'
import { theme } from '@/theme'

export default function Providers({ children }: { children: React.ReactNode }) {
  return (
    <Provider store={store}>
      <ThemeProvider theme={theme}>
        {children}
      </ThemeProvider>
    </Provider>
  )
}
```

**Step 2 — use in `layout.tsx`:**
```tsx
import Providers from './providers'

export default function RootLayout({ children }) {
  return (
    <html lang="en">
      <body>
        <Providers>
          {children}
        </Providers>
      </body>
    </html>
  )
}
```

---

## Server vs Client Components

| | Server Component | Client Component |
|---|---|---|
| Default in Next.js | Yes | No — needs `'use client'` |
| Can use Redux | No | Yes |
| Can use useState/useEffect | No | Yes |
| Can use browser APIs | No | Yes |

`'use client'` at top of file = "this file runs in the browser."

---

## Final `layout.tsx` — complete

```tsx
import type { Metadata } from 'next'
import './globals.css'
import Providers from './providers'

export const metadata: Metadata = {
  title: 'LifeMine Vendor',
  description: 'Vendor portal for LifeMine services',
  icons: { icon: '/favicon.ico' },
}

export default function RootLayout({
  children,
}: {
  children: React.ReactNode
}) {
  return (
    <html lang="en">
      <body>
        <Providers>
          {children}
        </Providers>
      </body>
    </html>
  )
}
```

---

## Golden Rules

1. `layout.tsx` wraps every page — put only truly global things here
2. Import `globals.css` here and nowhere else
3. `metadata` here = default title/description for entire app
4. Never put client-side code (Redux, MUI) directly in `layout.tsx`
5. Create `providers.tsx` with `'use client'` for all providers
6. `layout.tsx` stays a Server Component — keep it clean
