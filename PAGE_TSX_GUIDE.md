# `page.tsx` — Complete Guide

## What is it?
The **actual page content** for each route. Every route in Next.js needs a `page.tsx`.

---

## Simplest page

```tsx
export default function Home() {
  return (
    <div>
      <h1>Welcome to LifeMine</h1>
    </div>
  )
}
```

---

## How routes work

Folder name = URL path. No react-router, no config file.

```
app/
├── page.tsx              → /
├── dashboard/
│   └── page.tsx          → /dashboard
├── profile/
│   └── page.tsx          → /profile
└── settings/
    └── page.tsx          → /settings
```

---

## How it connects to layout.tsx

```
Browser goes to /dashboard

1. layout.tsx loads        ← always
2. dashboard/page.tsx      ← goes into {children}
```

```tsx
// layout.tsx
<body>
  <Providers>
    {children}  ← page.tsx renders here
  </Providers>
</body>
```

---

## Server Component by default

```tsx
// direct data fetching — no useEffect needed
export default async function Dashboard() {
  const data = await fetch('...')
  return <div>{data}</div>
}
```

Need hooks or Redux → add `'use client'`:

```tsx
'use client'

import { useState } from 'react'

export default function Dashboard() {
  const [count, setCount] = useState(0)
  return <div>{count}</div>
}
```

---

## Vite vs Next.js routing

| Vite (React Router) | Next.js |
|---|---|
| `<Route path="/dashboard">` | `app/dashboard/page.tsx` |
| `react-router-dom` package | Built-in, no package needed |
| One config file for all routes | Each folder is a route |

---

## Golden Rules

1. Folder name = URL route
2. Every route needs a `page.tsx` inside it
3. `page.tsx` renders inside `{children}` of `layout.tsx`
4. Default = Server Component
5. Need hooks or Redux → add `'use client'` at top
