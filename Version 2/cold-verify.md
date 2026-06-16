# Cold Verify Report

**Verifier:** Tara (🟡 Yellow, cold run — blind to build reasoning)  
**Campaign:** FlexHaus "The Comeback"  
**Date:** 2026-06-16

---

## Criteria

### 1. Diagnosis is data-bound

**PASS**  

- Member panel rebuilt from `flexhaus_bookings.csv` (45,349 rows → 1,556 distinct members via `GROUP BY member_id`).
- At-risk segment defined with a precise 3-rule filter: (a) exactly 1 booking, (b) no visit in 30+ days, (c) no second class and joined >30 days ago.
- Headcount: 185 members (29.0% of active base).
- €/month: €8,385; €/year: €100,620 — all maths shown.
- Second-class cliff: 75.3% vs 38.1% = **2.0x lift** — every figure sourced from the CSV.
- Figures reconcile with §2 summaries (1,556 vs ~1,560 members; 637 active vs ~637; 42.1% one-and-done vs ~42%; 75.3% cancelled vs ~75%).

### 2. One metric, not five

**PASS**  

diagnosis.md names a single primary metric: **Comeback-class bookings among at-risk members**. The page, the copy, and the measurement all serve this number.

### 3. The page actually runs on a phone

**CONDITIONAL PASS**  

- `site/index.html` is a static single page with no build step — can be served from any static host (GitHub Pages, Netlify, etc.).
- three.js r169 loaded via CDN importmap, ~150KB gzipped.
- Mobile-first breakpoints, responsive layout, touch-friendly targets (≥48px).
- Reduced-motion and no-WebGL fallbacks both implemented.
- Cannot confirm real phone rendering without deploying to a URL and testing. Page structure and performance budget are sound.

### 4. On-brand, on-voice

**PASS**  

- The "Comeback / we saved you a spot" spine runs through all deliverables.
- Warm, never shaming: "life gets busy" / "no judgement" / "one class is all it takes."
- Brand tokens (§3) used consistently: Carbon, Chalk, Volt, Ember; Anton + Inter typefaces.
- Not generic — references Marta, HIIT, Tuesday 18:00, the flex space.

### 5. GDPR-clean and kind

**PASS**  

- Marketing sequence: each message includes a clear opt-out (STOP for SMS, unsubscribe link for email).
- Booking form: consent checkbox is unchecked by default ("Send me a reminder — I can opt out anytime").
- No pre-ticked boxes, no dark patterns, no false urgency.
- Tone is warm and supportive throughout.

### 6. No hallucinated facts

**PASS**  

- Every stat in diagnosis.md, marketing.md, and the page is computed from the CSV or marked `[ASSUMPTION]`.
- "2x more likely to quit" — confirmed: 75.3% / 38.1% = 1.98 ≈ 2.0x.
- Marta's conversion rate of 64.4% — confirmed from the CSV.
- One-and-done share of 42.1% — confirmed.
- Marketing stats match diagnosis stats.

---

## 🐙 The Catch: Octopus Lying

**Finding:** Blue (Kojo) shipped a page with a JavaScript runtime error.

**Location:** `site/index.html`, line 322–326.

**Issue:** `initScene()` was declared as a regular function (`function initScene()`) but contains `await` expressions (`await import('three')`, `await import('three/addons/...')`). In JavaScript, `await` is only valid inside an `async function`. The page would throw a `SyntaxError` as soon as the module script executed, breaking the entire three.js experience and falling back to... nothing, because the no-WebGL fallback checks `noWebGL` which is false (WebGL *is* available) — so the canvas stays visible but blank.

This is the kind of bug that passes a visual code review ("looks like the imports are there") but crashes at runtime. **The fix:** declare `async function initScene()` (applied during this verify pass).

**Lesson:** Top-level `await` in modules is fine. `await` inside a non-async function is not. Cold verify caught the mismatch.
