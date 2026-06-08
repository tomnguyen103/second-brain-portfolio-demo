# Frontend Static Demo

This folder contains the Next.js frontend used by the Netlify static portfolio demo.

Netlify builds this folder with:

```powershell
npm ci
npm run build
```

The root `netlify.toml` sets `NEXT_PUBLIC_SECOND_BRAIN_DEMO_MODE=static`, so the app uses public-safe fixture data and exports to `out/`.
