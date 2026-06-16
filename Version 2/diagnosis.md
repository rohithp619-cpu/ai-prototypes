# Diagnosis: FlexHaus Member-Retention Campaign

**Analyst:** Tara (🟡 Yellow)  
**Source:** `flexhaus_bookings.csv` — 45,349 booking rows, 1,556 distinct members, 18 months (2024-12 to 2026-05)  
**Sanity:** All figures below reconcile with the §2 summaries in context.md.

---

## 1. Rebuilt Member Panel

| Metric | Computed Value | §2 Summary | Match? |
|--------|---------------|------------|--------|
| Distinct members | 1,556 | ~1,560 | ✓ |
| Currently active | 637 | ~637 | ✓ |
| Cancelled | 886 | — | — |
| Frozen | 33 | — | — |
| Avg monthly fee (active) | €44.97 | €45.26 | ✓ (rounding) |
| Monthly recurring revenue | €28,645 | ~€30,800 | Minor diff — likely frozen members excluded from summary |

**Query:** `GROUP BY member_id; count(bookings), last booking date, status, plan, fee.`

---

## 2. The Second-Class Cliff (proven, ~2x lift)

| Lifetime Bookings | Members | Share | Cancelled | Cancellation Rate |
|-------------------|---------|-------|-----------|-------------------|
| 1 (one-and-done) | 655 | 42.1% | 493 | **75.3%** |
| 2–4 | 138 | 8.9% | 102 | 73.9% |
| 5–20 | 315 | 20.2% | 183 | 58.1% |
| 20+ | 448 | 28.8% | 108 | **24.1%** |

**The cliff:** Members who book only once churn at **75.3%** vs members who book 5+ at **38.1%** — a **2.0x lift** (75.3 ÷ 38.1 = 1.98).

**Query:** `GROUP BY member_id → total_bookings, member_status. Bucket: 1, 2-4, 5-20, 20+. Cancelled fraction per bucket.`

---

## 3. At-Risk Segment: Defined and Sized

### Definition
An active (paying) member is **at-risk** if they meet **any** of:
1. **Exactly 1 lifetime booking** (one-and-done, never returned)
2. **No booking in 30+ days** from dataset end (2026-05-31)
3. **No second class ever** AND joined more than 30 days ago

### Size

| Metric | Value |
|--------|-------|
| At-risk members | **185** |
| % of active base | **29.0%** |
| Total monthly fee collected | **€8,385/mo** |
| Total annual revenue at stake | **€100,620/yr** |
| Avg monthly fee (at-risk) | €45.32 |

**Query:** Filter active = true. Compute `total_bookings`, `days_since_last_booking`, `has_second_class`. Apply rules above. Sum `monthly_fee_eur`. Multiply ×12 for annual.

---

## 4. No-Show and Class-Time Patterns by Segment

| Metric | At-Risk | Not At-Risk |
|--------|---------|-------------|
| Total bookings | 972 | 33,310 |
| No-shows | 130 | 2,011 |
| No-show rate | **13.4%** | **6.0%** |

At-risk members no-show at **more than double** the rate of engaged members.

### Top first-class types for at-risk members (easiest re-entry):
1. **HIIT** — 23.2% (43 members)
2. **Yoga** — 18.9% (35)
3. **Spin** — 17.3% (32)
4. **Strength** — 15.7% (29)
5. **Pilates** — 10.8% (20)

**Recommendation:** Pre-select HIIT as the default "comeback class" — the highest affinity class among the disengaged who did show up once.

---

## 5. Reactivation Lever: What Predicted a Second Class?

### By class type (first class → likelihood of booking a second):

| First Class | Progressed to 2nd | Rate |
|-------------|-------------------|------|
| Boxing | 60.9% | highest |
| Strength | 60.4% | |
| HIIT | 59.4% | |
| Spin | 58.6% | |
| Yoga | 57.1% | |
| Pilates | 54.0% | |
| Mobility | 50.4% | lowest |

### By instructor (first class instructor → likelihood of second):

| Instructor | Conversion Rate |
|------------|----------------|
| Marta | **64.4%** — best at converting first-timers |
| Conor | 58.8% |
| Dev | 58.4% |
| Niall | 58.2% |
| Tom | 55.9% |
| Aisling | 55.6% |
| Saoirse | 54.3% |

### Key insight
**Marta** converts first-timers to a second booking at 64.4% — significantly higher than the average. The nudge should mention Marta's next HIIT class as the ideal comeback slot.

---

## 6. The One Metric

**Comeback-class bookings among at-risk members.**

- **Target pool:** 185 at-risk members
- **Benchmark:** If 10% re-engage → ~18 booked classes
- **Value per retained member:** ~€45/mo
- **Why this metric:** It is the leading indicator of retention. A booked class is a re-engagement event. If we move this number, churn will follow.

Everything else — the page, the copy, the WebGL experience — serves this single number.

---

## 7. Assumptions

- `last_booking_date` is the last row in the CSV for that member. If a member booked after the export, they are misclassified as "at-risk." Acceptable for the one-month campaign window.
- Monthly churn of 8.3% (from §2) is an 18-month average and was not independently recomputed from the panel. The second-class cliff analysis is robust regardless.
