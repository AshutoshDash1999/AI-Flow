# AI Flow — AGENTS.md

## Structure

- **Root** — Vite + React 18 + TypeScript + Tailwind CSS 3 + Framer Motion (frontend)
- **`server/`** — Standalone Express.js backend for contact form (nodemailer)

## Commands

| Command                    | Action                              |
| -------------------------- | ----------------------------------- |
| `npm run dev`              | Vite dev server                     |
| `npm run build`            | Vite production build               |
| `npm run lint`             | ESLint on `.` (only frontend check) |
| `cd server && npm start`   | Start server (port 5000)            |
| `cd server && npm run dev` | Server with nodemon                 |

No test framework or typecheck script configured.

## Key architecture

- **Entry**: `src/main.tsx` → `src/App.tsx` with React Router routes for 8 pages
- **AI Tools page** (`/ai-tools`): protected by `ProtectedRoute` → requires Supabase auth
- **Auth**: Supabase email/password + Google OAuth via `src/contexts/AuthContext.tsx`
- **AI**: Direct calls to Gemini 2.0 Flash API (`VITE_APP_GEMINI_API_KEY`) from `AITools.tsx` and `Chatbot.tsx`
- **Chatbot**: Floating widget (`Chatbot.tsx`) on all pages, also uses Gemini API directly
- **Contact form**: POST to server (`http://localhost:5000/api/contact`) which sends email via nodemailer
- **Dark mode**: Class-based (`darkMode: 'class'` in tailwind.config.js), toggled by navbar sun/moon button

## Environment

- **Frontend** (`VITE_*`): `.env` in root — loaded by Vite at build time; uses `import.meta.env.VITE_*`
- **Server** (`EMAIL_USER`, `EMAIL_PASS`, `PORT`): `.env` in `server/` — loaded by `dotenv` at runtime
- Both have `.env.example` files with setup instructions

## Deployment

- **Frontend + server** both deployable to Vercel separately
- Root `vercel.json` rewrites all paths to `index.html` (SPA fallback)
- Server has its own `vercel.json` using `@vercel/node` builder

## Conventions

- Component files: `src/components/*.tsx`, pages: `src/pages/*.tsx` (PascalCase)
- Gradient styling pattern: `bg-gradient-to-r from-blue-600 to-purple-600`
- Tailwind classes for backgrounds: `bg-white dark:bg-gray-900` (explicit both modes)
- Animation: framer-motion `motion.div` with `initial/animate/exit` throughout
- Branches: `feature/<name>`, `fix/<name>`, `docs/<name>` (from Contributing.md)

## Installed Skills

| Skill                      | Use in this project                                                                                       |
| -------------------------- | --------------------------------------------------------------------------------------------------------- |
| `tailwind-design-system`   | Build consistent Tailwind design tokens, component variants, and dark-mode theming for the React frontend |
| `wcag-audit-patterns`      | Audit `src/components/` and `src/pages/` for WCAG 2.1 violations (contrast, ARIA, keyboard nav)           |
| `accessibility-compliance` | Enforce a11y rules across components; pairs with `wcag-audit-patterns` for remediation                    |
| `core-web-vitals`          | Diagnose and fix LCP/CLS/INP in the Vite+React SPA; optimize bundle, images, and framer-motion animations |
| `conventional-commit`      | Generate `feat/fix/docs/chore` commit messages following the project's branch naming conventions          |
| `deploy-to-vercel`         | Deploy frontend (root) and Express server (`server/`) to Vercel using project `vercel.json` configs       |
| `eslint-prettier-config`   | Set up or update ESLint + Prettier for the TypeScript frontend                                            |
| `nodemailer`               | Configure and debug the nodemailer contact-form mailer in `server/` (EMAIL_USER / EMAIL_PASS env vars)    |
| `nodejs-express-server`    | Extend or refactor the Express contact-form server in `server/`                                           |
