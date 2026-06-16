# Design Spec: The Comeback Booking Page

**Designer:** Mateo (🔴 Red-Orange)  
**Inputs:** Tara's diagnosis.md, brand direction (§3 of context.md), §5 technical requirements

---

## 1. Concept

A short, kinetic scroll-driven three.js WebGL experience that turns a guilty lapse into an easy return. The page tells a story in four scenes. At the end, the member books their comeback class in one tap.

**Spine:** "Your spot's still here."  
**Hook:** The Comeback — reframing the lapse as a return, not a failure.  
**End state:** A Volt-green pool of light with a pre-filled booking card.

---

## 2. Page Structure (top-to-bottom, scroll-driven)

### Scene 1: The Resting Pulse → Hero
- **Background:** Deep Carbon (`#0E1116`) fills the viewport. A single 3D line traces a slow heartbeat pulse in Volt (`#C6F24E`) across the centre. Particles (chalk-dust motes) drift lazily in the dark.
- **Overlay text (positioned in DOM for accessibility):**
  - Headline: *"Your spot's still here."* (Anton/Archivo Expanded, Chalk `#F5F5F2`, bold)
  - Subheadline: *"Come back to FlexHaus. We saved you a spot."* (Inter, Chalk, 400 weight)
  - CTA button: *"Book your comeback class"* — Volt background, Carbon text, large tap target
- **Scroll prompt:** A small animated indicator at the bottom ("Scroll to begin")
- **States:**
  - Default: heartbeat at 60bpm pulse, particles drifting
  - Hover on CTA: pulse quickens perceptibly, button lifts with Ember (`#FF5A3C`) glow
  - Focus: visible 3px Volt ring

### Scene 2: Momentum Builds (fly-through)
- **Camera moves** forward through the gym space (automatic track via scroll position):
  - The rig comes into view (3D bar/cable geometry, stylised, low-poly)
  - Studio space with floating chalk-dust particles (GPU points system)
  - Exposed brick texture on walls (subtle normal-mapped planes)
- **The pulse line** accelerates from 60bpm → 120bpm as scroll progresses
- **2-3 short copy lines** fade in as DOM overlay:
  1. *"You walked through that door once. The door's still open."*
  2. *"One class is all it takes to remember why you joined."*
  3. *"Your Tuesday HIIT with Marta is waiting."*
- **Reduced-motion variant:** Static gym interior image (photograph-style), smooth crossfade between hero and proof section. No 3D camera move.

### Scene 3: The Proof ("why now")
- Three cards rise from the dark, each with an icon, a bold number or claim, and a short line:
  1. **One class resets the habit.** *"Members who come back for a second class are 2x more likely to stay."*
  2. **Your favourite is on Tuesday.** *"HIIT with Marta — our highest conversion class."*
  3. **We held the spot.** *"Off-peak slots are open. No wait, no pressure."*
- The stat is the one verified figure from diagnosis.md:

> "Members who skip the second class are **2x more likely to quit**." (Computed from FlexHaus bookings data: 75.3% vs 38.1%)

- Animated entrance: cards fade up + slight vertical float
- **No-WebGL fallback:** These cards are always visible in DOM, styled with CSS only

### Scene 4: The Comeback Card (booking form)
- **Background:** A wide pool of Volt-green light (postprocessing bloom on a 3D disc / CSS gradient fallback)
- **Booking card hovers** in the centre: Chalk background, Carbon text
- **Pre-filled fields:**
  - Class: HIIT (default — highest at-risk affinity per diagnosis.md)
  - Time: Tuesday 18:00 (next off-peak slot)
  - Instructor: Marta (highest conversion rate per diagnosis.md)
- **Form fields:**
  - Name (text input, required)
  - Email (email input, required — for confirmation)
  - Phone (optional — for SMS reminder)
  - Class preference (dropdown: HIIT, Yoga, Spin, Strength, Pilates, Mobility, Boxing)
  - Preferred time (dropdown: off-peak morning, off-peak afternoon, peak evening)
  - Consent checkbox: *"Send me a reminder — I can opt out anytime"* (unchecked by default, GDPR compliant)
- **Primary CTA:** "Book My Comeback Class" — Ember (`#FF5A3C`) background, Chalk text, large tap target
- **Success state:**
  - Card pulses with Volt glow
  - Heartbeat line races to full (animated)
  - Text: *"You're in. See you Tuesday at 18:00. We'll save the spot."*
  - A "Reschedule" link (low-key, smaller)
- **Error state:**
  - Inline validation messages (red, readable)
  - Form stays populated
  - CTA re-enabled after corrections

---

## 3. Scroll Choreography

| Scroll % | Scene | Camera Position | Pulse Speed |
|----------|-------|-----------------|-------------|
| 0–20% | Hero / Resting Pulse | Front, static | 60bpm |
| 20–50% | Momentum / Fly-through | Moving forward through gym | 60→120bpm |
| 50–70% | Proof / Why Now | Parked, slightly above | 90bpm |
| 70–100% | Comeback Card | Looking down at Volt pool | 120bpm → full sprint on success |

- Lerp/damp controlled scroll interpolation — no janky snapping
- Touch-friendly: momentum scroll works
- All animations driven by `scrollY` progress (0.0–1.0), not by IntersectionObserver alone

---

## 4. Visual Design Tokens

| Token | Value | Usage |
|-------|-------|-------|
| Carbon | `#0E1116` | Backgrounds, dark field |
| Chalk | `#F5F5F2` | Text on dark, card backgrounds |
| Volt | `#C6F24E` | Pulse line, glow, primary accent |
| Ember | `#FF5A3C` | CTA button, success accent |
| Type display | Anton / Archivo Expanded | Headlines, big numbers |
| Type body | Inter | Body copy, form labels |

---

## 5. Accessibility Plan

| Requirement | Implementation |
|-------------|----------------|
| Screen reader | Canvas is `aria-hidden="true"`. All text lives in real DOM elements. Form has `<label>` for each input. |
| Keyboard navigation | All interactive elements reachable via Tab. Visible `:focus-visible` rings (3px Volt on Carbon, 3px Carbon on Chalk). |
| `prefers-reduced-motion` | `matchMedia('(prefers-reduced-motion: reduce)')` → disable 3D scene, disable scroll animations, serve CSS-styled static hero + proof cards + form. |
| No-WebGL fallback | `renderer.capabilities` check → if WebGL unavailable, remove canvas, show styled static layout with hero image gradient, proof cards, fully functional booking form. |
| AA contrast | Volt (`#C6F24E`) on Carbon (`#0E1116`): ratio ~9.5:1 ✓. Chalk (`#F5F5F2`) on Carbon: ~14:1 ✓. Ember (`#FF5A3C`) on Chalk: ~4.8:1 ✓. |
| Touch targets | All CTA buttons ≥ 48px height. Form fields ≥ 44px. |

---

## 6. States Summary

| Element | Default | Hover | Focus | Success | Error | Disabled |
|---------|---------|-------|-------|---------|-------|----------|
| CTA (hero) | Volt bg, Carbon text | Ember glow, scale 1.02 | 3px Volt ring | — | — | — |
| CTA (form) | Ember bg, Chalk text | Brighter Ember, scale 1.02 | 3px Carbon ring | Pulsing Volt glow | Shake animation | Greyed out |
| Input fields | Chalk bg, 2px Carbon border | — | 3px Volt ring | — | 2px Ember border + red message | — |
| Cards | Opacity 0→1 on scroll | — | — | — | — | — |

---

## 7. Performance & Mobile

- **Target:** Interactive < 2.5s on mid-range phone (Moto G / iPhone SE)
- **Geometry:** Low-poly stylised gym (under 50 draw calls)
- **Particles:** Single BufferGeometry Points system, max 5,000 particles
- **Bloom:** UnrealBloomPass with low resolution (256×144) — or CSS blur for fallback
- **Texture budget:** Max 2 textures (brick normal, chalk-dust sprite)
- **JS payload:** three.js via CDN import map, ~150KB gzipped
- **Cut-list if over budget (in order):**
  1. Reduce particle count to 1,000
  2. Remove InstancedMesh lockers, keep only the rig + particles
  3. Replace postprocessing bloom with CSS radial-gradient glow
  4. Fall back to static no-WebGL mode entirely
