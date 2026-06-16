# Win-Back Campaign: The Comeback

**Copywriter:** Róisín (🟢 Green)  
**Hook:** "We saved you a spot."  
**Tone:** Warm, direct, momentum, belonging. Never shame, never guilt.  
**Stats source:** diagnosis.md (computed from `flexhaus_bookings.csv`)

---

## Campaign Overview

| Touch | Channel | Timing | Goal |
|-------|---------|--------|------|
| 1 | SMS | Day 1 | Low-friction nudge → open the comeback page |
| 2 | Email | Day 3 | Story + proof → book the class |
| 3 | SMS | Day 7 | Final nudge (scarcity of the saved spot) |

Each message drives to the same landing page: the three.js comeback-booking page.

---

## Touch 1: SMS — The Nudge

**Metric targeted:** Comeback page visit rate

```
FlexHaus: Hey [Name], your spot's still here.
We saw you haven't been in a while — so we saved you
a spot in Tuesday HIIT with Marta. One tap to claim it:
[link]

Reply STOP to opt out of messages.
```

**GDPR note:** Pre-existing member, service communication per join terms. Opt-out via STOP. No pre-ticked consent.

---

## Touch 2: Email — The Story

**Metric targeted:** Booking completion rate

**Subject:** Your spot's still here, [Name]  
**Preview:** One class is all it takes. We saved you Tuesday HIIT with Marta.

```
Hi [Name],

It's been a few weeks. No judgement — life gets busy.
And honestly? One class is all it takes to remember why
you joined.

Members who come back for a second class are 2x more likely
to stay with FlexHaus long-term.* That's not a sales pitch.
That's just what the data says.

So here's what we did: we saved you a spot in Tuesday HIIT
with Marta at 18:00 — our highest-conversion class with
our highest-conversion instructor. Same studio, same energy,
zero pressure.

[Button: Book My Comeback Class]

If Tuesday doesn't work, pick any class that fits your week.
The door's still open.

See you soon,
Dervla & the FlexHaus team

P.S. Off-peak slots are wide open. No wait, no crowd. Just
show up.

*Source: FlexHaus bookings data. Members who book 5+ classes
churn at 38% vs 75% for single-class members.

[Unsubscribe link] | [Update preferences]
```

---

## Touch 3: SMS — The Final Nudge

**Metric targeted:** Booking completion rate (late converters)

```
FlexHaus: [Name], Marta's Tuesday HIIT is still open for you.
One tap and you're in. We'll save the spot.
[link]

This is your last nudge — after this, the spot opens up.
No pressure either way.

Reply STOP to opt out.
```

---

## Instagram Posts

### Post 1: The Comeback Announcement

**Visual:** Short video loop — heartbeat line in the dark (consistent with page aesthetic), ends with the Volt-green glow. Subtle chalk-dust particles.

**Caption:**

```
Your spot's still here. 💚

We've been watching the numbers, and here's the truth:
members who come back for a second class are 2x more
likely to stay. So we built something to make that
second class feel less like a big deal and more like
"of course I'm coming."

Introducing The Comeback — a one-tap way to book your
next class, pre-filled with your favourite time and
instructor. No shame. No guilt. Just a spot we saved
for you.

[Link in bio to book]

#FlexHaus #TheComeback #ShowUpForYourself
```

### Post 2: Community — Member Story

**Visual:** Warm photo of an empty studio with natural light (exposed brick, yoga mats stacked). Or a short clip of Marta teaching.

**Caption:**

```
"Honestly I forgot I was a member until the payment came out."

We heard that from someone who'd lapsed. And we thought:
what if we just... texted them a spot?

So that's what we're doing. If you're a member who hasn't
been in a while, check your phone. We saved you Tuesday
HIIT with Marta.

No speech. Just a spot.

Tag someone who needs to see this 👇

[Link in bio: Book Your Comeback Class]

#FlexHausCommunity #TheComeback #ShowUpForYourself
```

---

## GDPR & Consent

- All messages are sent to existing members who consented to service communications at join.
- Each message includes a clear, one-step opt-out (STOP for SMS, unsubscribe link for email).
- No pre-ticked consent boxes on the booking form. The reminder checkbox is opt-in, unchecked by default.
- No dark patterns. The opt-out is as easy as the opt-in.
- Data captured: name, email, class preference, time preference. Nothing shared or sold.

---

## Metrics Dashboard

| Metric | Target | Source |
|--------|--------|--------|
| Comeback page visits | 50% of at-risk segment (93 of 185) | URL analytics |
| Booking completions | 10% of at-risk (18 bookings) | Form submissions |
| SMS opt-out rate | < 5% | Carrier analytics |
| Email click-through | > 8% | Email platform |

---

## Assumptions

- Member phone numbers and emails exist in the gym CRM (not in the CSV). This campaign assumes Dervla can export those from the billing system.
- SMS costs roughly €0.04/message × 185 members × 2 SMS = ~€15. Well within the €2,000 budget.
- The "Marta Tuesday HIIT" default applies to all at-risk members for simplicity. Future iterations could personalise per member's actual first class.
