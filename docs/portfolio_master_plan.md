# Portfolio Master Plan: "A Portfolio You Can Inspect"

A non-generic, hand-crafted portfolio that shows modern frontend craft (Three.js, WebGL, kinetic type, Lenis smooth scroll) **and** real backend engineering (caching, auth, databases, services), where the site itself is the proof.

---

## 0. TL;DR

- **Do not hide backend skills, and do not keep the site simple.** Make the backend *visible and interactive*, and back it with case studies.
- **Core hook:** a polished surface with an **X-Ray / Engineer Mode** toggle that reveals the real system underneath (cache HIT/MISS, latency, DB queries, auth lifecycle).
- **Stack:** Next.js (App Router, TypeScript) + standalone Express API + Postgres + Redis + React Three Fiber + GSAP + **Lenis**.
- **Look:** refined neo-brutalism / blueprint hybrid, one accent colour, mono + display type, one custom shader, one small 3D object.
- **AI workflow:** write your own `DESIGN.md` and rules file first; optionally layer a design skill on top; review all generated backend and animation code yourself.
- **Timeline:** 20-day focused sprint **or** 5-6 weeks part-time (both laid out below).

---

## 1. Goals and Principles

1. **Show, don't list.** Every backend skill claimed on the site must be demonstrable on the site.
2. **Everything real.** Only show true measurements. A reviewer can open the network tab.
3. **Curate.** 2-3 deep case studies beat 8 shallow cards.
4. **Opinionated design.** At least three layout/interaction decisions that no template would make.
5. **Recruiter-safe.** Within one screen: name, role, what you do, where to click for projects and resume. Clever stuff goes one scroll down or behind the toggle, never blocking the basics.
6. **Defensible.** You must be able to explain every line of backend code in an interview.

---

## 2. Stack Decision

| Layer | Choice | Why |
|---|---|---|
| Frontend | Next.js (App Router) + TypeScript | SSR/ISR, SEO, easy deploy, large ecosystem |
| 3D | React Three Fiber + drei | Declarative Three.js, built-in perf helpers (LOD, Environment, Preload) |
| 3D (optional shortcut) | Spline | Fast interactive scenes exported to React; use for prototyping or a secondary element |
| 3D assets | Blender (low-poly server rack / DB cylinder), exported as compressed GLB | Custom models, small file size |
| Smooth scroll | **Lenis** | Required; synced with GSAP ticker |
| Animation | GSAP + ScrollTrigger (choreography), Motion/Framer Motion (component transitions) | Right tool per job |
| Shaders | Custom GLSL via R3F | Strongest "hand-made" signal |
| Node graph | React Flow (2D) or R3F (3D) | Architecture map |
| API | Express (TypeScript) as a **separate deployed service** | A real service boundary makes "microservices" honest |
| Validation | Zod | Input safety on public endpoints |
| Database | PostgreSQL (Neon or Supabase) | Relational data: users, guestbook, visits, projects |
| Cache | Redis (Upstash) | Read-through cache, counters, rate limiting |
| Auth | GitHub OAuth + magic link; sessions/JWT you implement (or Auth.js if you prefer not to hand-roll) | Demonstrates auth without a "skills" bullet |
| Realtime | Server-Sent Events (simple) or WebSocket | Live telemetry panel |
| Monorepo | pnpm workspaces (+ Turborepo optional) | Small signal of maturity; skip if it slows you down |
| Hosting | Vercel (web) + Render/Railway/Fly (API) | Two deploys = real architecture |
| Prototyping | Figma (+ Excalidraw for architecture diagrams) | Wireframes first |

**Is there anything better?** For this goal, no. SvelteKit has nicer animation ergonomics, but the R3F ecosystem is React-only. Keep Next.js for the frontend and Express for the API.

**Keep "microservices" honest.** Two or three small services with a clear reason each (API, auth, worker/telemetry) is believable. Ten services for a portfolio looks like padding and invites hard questions.

---

## 3. Concept and Signature Features

### 3.1 X-Ray / Engineer Mode (signature feature)
- A physical-feeling 3D-style switch in the UI.
- Flipped on, the page strips back to a wireframe/terminal view. Each section shows which API call fed it, response time, cache status (HIT/MISS), and the DB query behind it.
- **How:** the Express API returns headers such as `X-Cache`, `X-Response-Time`, `X-Request-Id`; the frontend reads them into a global store and renders overlays.

### 3.2 Live Architecture Map
- Interactive node graph of the **actual** portfolio system: Next.js, API, Redis, Postgres, Auth, Worker.
- Real requests animate as pulses along edges.
- Clicking a node explains your decision (why Redis here, TTL strategy, invalidation approach, JWT vs session choice).

### 3.3 Real Cache Demo
- Visitor counter / "API requests served" / "currently viewing".
- First read from Postgres, repeats from Redis, with an honest latency comparison ("Served from Redis in 12ms" vs "from DB in 150ms") and a "bust cache" button.
- **Rate limiting** (Redis token bucket): spam a button, see a 429 state with a cooldown UI.

### 3.4 Auth-Gated Guestbook ("Terminal" contact)
- Command-line style UI (`> sign-in --github`, `> leave-message`).
- Real auth: GitHub OAuth and/or magic link, protected routes, session/JWT lifecycle.
- In X-Ray mode, show token lifecycle (issue, expiry, refresh) without exposing secrets.
- Moderation, sanitization and a kill switch, since a public write endpoint will be abused.

### 3.5 Case Studies With Architecture
- 2-3 projects only. Each: problem, architecture diagram, schema snippet, one hard tradeoff, what you'd change.
- This is where microservices, DB design and caching from your *other* projects get their full credit.

### 3.6 3D Bento Projects Grid
- Bento layout where tiles behave like system panels: one shows live latency, one a 3D object, one a project.
- Project tiles can flip or tilt in 3D on hover to reveal an architecture diagram or code snippet on the back.

---

## 4. Visual Design Direction

**Base style: refined neo-brutalism / blueprint hybrid.** Exposed grid lines, mono labels, hard shadows, one sharp accent colour. It matches the "inspectable system" concept and is rarely done well, which makes it memorable.

- **Bento with a twist:** tiles as system panels (see 3.6).
- **Kinetic typography:** headline reacts to scroll velocity (Lenis exposes it): skew, stretch or letter spacing shifts; optionally text wrapping around a 3D object.
- **Hero:** one custom GLSL shader (flow field, noise, or pointer distortion / data-stream effect) plus **one** simple 3D object (e.g. stylized server rack with blinking lights, or an abstract shape reacting to scroll). Lighting and materials do the work; keep geometry simple.
- **Typography:** one display font + one mono font. Candidates: Space Grotesk, Clash Display, Geist / Geist Mono, JetBrains Mono, Inter. Strict type scale and spacing rules. Typography is most of the design.
- **Colour:** neutral base + exactly one accent, as CSS variables with dark and light variants.
- **Motion tokens:** consistent easing and duration everywhere. Inconsistent motion is the clearest "generated" tell.
- **Micro-interactions:** magnetic buttons, custom cursor states, smooth page transitions, a console easter egg (`console.log`) and one quirky interaction reflecting your personality.

**Avoid (the "AI-generated" tells):** purple-to-blue gradients, glassmorphism everywhere, identical rounded cards, emoji icons, centered-everything layouts, "Hi, I'm X 👋" hero, stock-sounding copy. Write your own copy.

---

## 5. AI Tooling and Workflow (Antigravity IDE)

### 5.1 Design guidance
I can't verify the exact contents or current versions of "taste skill" or "impeccable", so read their READMEs first. In general these are instruction files that stop the agent from producing default-looking UI. Treat them as an optional layer. **Your own written spec matters more.**

1. **`docs/DESIGN.md`**: palette, fonts, type scale, spacing, grid, motion tokens, do/don't list, and 3-5 reference sites with *what specifically* you like about each.
2. **`AGENTS.md` / rules file**: TypeScript strict, folder structure, "all colours from tokens", "no new dependencies without asking", "clean up ScrollTriggers and Lenis on unmount".
3. Optionally load **one** design skill at a time (try impeccable, then taste skill) and compare output on the same task. Your DESIGN.md wins any conflict.

### 5.2 Model usage
- **Claude (primary):** architecture, shader logic, tricky R3F + GSAP + Lenis sync, auth and cache design review.
- **Gemini (fallback when limits hit):** repetitive components, styling variants, boilerplate.
- Work in **small tasks**: one component or endpoint per prompt, with a reference screenshot and DESIGN.md attached.

### 5.3 Review rules
- Always review animation code for `useFrame` leaks, un-killed ScrollTriggers, and Lenis not destroyed on unmount.
- Write or line-by-line review the **backend logic** (auth, cache invalidation, rate limiter, input validation). It's your claimed expertise.

---

## 6. Repo Structure

```
portfolio/
  apps/
    web/              # Next.js
    api/              # Express (TS)
    worker/           # optional: metrics aggregation / cron
  packages/
    shared/           # shared types, tokens, zod schemas
  docs/
    DESIGN.md
    ARCHITECTURE.md   # diagrams + decisions (also feeds the site's node graph)
  AGENTS.md
  docker-compose.yml  # postgres + redis for local dev
  README.md
```

---

## 7. Data Model (starting point)

- `users` (id, github_id, handle, avatar_url, created_at)
- `guestbook_entries` (id, user_id, message, status, created_at)
- `visits` (id, path, ts, country_bucket optional), with counters kept in Redis and flushed periodically
- `projects` (id, slug, title, summary, stack, case_study_md, order)

**Cache keys:** `projects:list`, `projects:{slug}`, `visitors:count`, `rl:{ip}:{route}`.
**Strategy:** read-through caching with TTL, explicit invalidation on writes, and counters via `INCR` flushed to Postgres by the worker.

---

## 8. Phased Plan

Two pacing options. Pick one; the phase contents are the same.

| Phase | 20-day sprint | Part-time (5-6 weeks) |
|---|---|---|
| 0. Narrative, references, wireframes | Days 1-3 | Week 1 |
| 1. Skeleton + Lenis/GSAP integration | Days 4-5 | Week 1-2 |
| 2. Backend that matters | Days 6-10 | Week 2-3 |
| 3. Hero and 3D | Days 8-12 (overlaps) | Week 3-4 |
| 4. X-Ray mode, architecture map, bento | Days 11-15 | Week 4-5 |
| 5. Case studies + content | Days 14-16 | Week 5 |
| 6. Micro-interactions, performance, a11y | Days 17-19 | Week 5-6 |
| 7. Deploy + launch checks | Day 20 | Week 6 |

### Phase 0: Narrative and Wireframes
**Do not open the editor yet.**
- Collect 5 references; write `DESIGN.md`.
- Define your hook (e.g. "I build scalable systems and beautiful interfaces").
- Write real copy for every section now.
- Wireframe in Figma: **Hero (3D/interactive)**, **About**, **The Guts / Systems (X-Ray)**, **Projects (bento)**, **Contact / Guestbook (terminal)**.
- Define tokens: colours, type scale, spacing, easing, durations.

### Phase 1: Skeleton
- Set up monorepo, Next.js, Express, docker-compose (Postgres + Redis).
- Build layout, nav, grid, typography with **no animation yet**.
- Integrate **Lenis + GSAP ScrollTrigger** correctly:
  - Drive Lenis from `gsap.ticker` (`gsap.ticker.add(t => lenis.raf(t * 1000))`)
  - `gsap.ticker.lagSmoothing(0)`
  - `lenis.on('scroll', ScrollTrigger.update)`
  - Destroy Lenis and kill triggers on unmount
- Set up linting, formatting, strict TS, basic CI.

### Phase 2: Backend That Matters
- Schema and migrations; seed `projects`.
- Endpoints with Redis read-through caching, explicit invalidation, and X-Ray response headers.
- Auth: GitHub OAuth (+ magic link if time), session/JWT, protected routes, CSRF/cookie settings.
- Rate limiting (Redis token bucket), Zod validation, structured logging.
- Tests for auth flow, cache behaviour (HIT/MISS/invalidate) and rate limiter.
- Live telemetry endpoint (SSE) feeding latency, hit ratio and request counts.

### Phase 3: Hero and 3D
- Start small. One shader background, then one 3D object in R3F (or Spline for a first prototype).
- Pointer interaction; scroll-linked camera using GSAP ScrollTrigger.
- Premium lighting and materials over complex geometry.
- Decide the **mobile / no-WebGL fallback now** (slick 2D version), not at the end.

### Phase 4: X-Ray Mode, Architecture Map, Bento
- Global store for the toggle; overlay components per section.
- Architecture graph with request pulses driven by real API calls.
- Bento tiles as live panels; flip/tilt effect on project tiles.
- Terminal-style guestbook UI wired to real auth.

### Phase 5: Case Studies and Content
- 2-3 projects, each with architecture diagram (Excalidraw/Figma SVG), schema snippet, tradeoff, retrospective.
- Resume page/PDF, links, SEO metadata, Open Graph image.

### Phase 6: Micro-interactions, Performance, Accessibility
- Magnetic buttons, cursor details, kinetic type, page transitions, console easter egg.
- **Performance:**
  - Compress textures (WebP / KTX2) and models (Draco/meshopt)
  - Lazy-load R3F with `next/dynamic` and `Suspense`
  - Cap pixel ratio (`dpr={[1, 2]}`); pause rendering when offscreen
  - Use drei helpers: LOD / `Detailed`, baked environment maps, `PerformanceMonitor`
- **Accessibility:** semantic HTML, keyboard navigation, focus states, contrast, `prefers-reduced-motion` (disable Lenis easing and heavy animation), readable content without WebGL.
- **Typography check:** strict spacing and grid alignment.
- Lighthouse plus real mid-range phone testing.

### Phase 7: Deploy
- Vercel (web), Render/Railway/Fly (API), managed Postgres and Redis, env secrets.
- Use a shared parent domain (e.g. `api.yourname.dev`) so cross-site cookies behave (SameSite/Secure).
- Keep-alive or non-sleeping host so the cache demo isn't ruined by cold starts.
- Basic uptime monitoring; error logging.

---

## 9. Pitfalls

- **Cold starts** on free tiers make latency demos look bad.
- **Cross-domain auth cookies** between web and API: plan the domain setup early.
- **Public endpoint abuse:** rate limit everything, sanitize input, keep a kill switch.
- **Fake numbers:** show only real measurements.
- **Over-scoping:** if behind, cut the 3D object before X-Ray mode. Backend visibility is your differentiator.
- **Lenis + R3F conflicts:** keep one animation clock (the GSAP ticker) and avoid duplicate rAF loops.
- **Performance cliffs:** large textures, uncapped DPR, rendering offscreen canvases.
- **Template drift:** if the agent keeps producing generic layouts, tighten DESIGN.md rather than re-prompting repeatedly.

---

## 10. Launch Checklist

- [ ] First screen: name, role, projects link, resume link are clear within seconds
- [ ] Fast on a mid-range phone; 3D degrades gracefully; no-WebGL fallback works
- [ ] Content readable and navigable by keyboard; reduced-motion respected
- [ ] Every claim on the site is true and demonstrable
- [ ] Case studies explain *decisions and tradeoffs*, not just tech names
- [ ] Auth, cache and rate limiter have tests; secrets are not exposed
- [ ] You can explain every backend component without opening the code
- [ ] Public repo with README and ARCHITECTURE.md; OG image and metadata set
- [ ] Lighthouse performance and accessibility scores checked

---

## 11. Final Tips

- **Curate, don't dump.** Two or three projects, written as case studies (architecture, schema, caching strategy).
- **Typography is most of the design.** A site with no 3D but masterful type beats messy type with great 3D.
- **Personality.** Keep one hidden easter egg or quirky interaction that reflects your actual interests.
- **Start with the boring parts.** Tokens, grid and copy before shaders. The flashy bits only land on a solid base.
- **Ship, then iterate.** Launch a clean version with X-Ray mode working, then add extras.
