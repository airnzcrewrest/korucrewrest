# Air NZ Cabin Crew Rest Calculator (`crewrest.html`)

Calculator location: [https://airnzcrewrest.github.io/korucrewrest/crewrest.html](https://airnzcrewrest.github.io/korucrewrest/crewrest.html)

## Overview

A self‑contained HTML / JavaScript tool for planning **contract and safety compliant** cabin‑crew rest and meal schedules on Air New Zealand flights. It covers both **NON‑ULR** (short/medium) and **ULR** (AKL ⇄ JFK) services, embedding contractual rules plus CASG / Fatigue Working Group guidance.

## User Instructions

**Air NZ Cabin Crew Rest Calculator – Instructions**

This tool calculates rest patterns for long‑haul Air New Zealand flights. It ensures rest and meal timing complies with contractual rules and CASG/Fatigue Working Group (FWG) guidelines for ULR sectors.

It requires a Wi‑Fi connection to load, but once the page is open it will continue working offline. You can preload it before the flight and leave it in a browser tab.

### How to Use

1. **Select the Flight Route** – Choose from the dropdown: **NON‑ULR**, **AKL–JFK**, or **JFK–AKL**. The route determines how rest is structured.
2. **Enter First Rest Start Time** – Use destination local time.
3. **Enter Top of Descent (ToD) Time** – Destination local time.
4. **Enter Meal Service Duration** – Type `H:MM` (e.g. `1:45`) *or* just digits (`145` auto‑formats to `1:45`).
5. **Click *Calculate*** – The tool validates the inputs and produces rest & meal timings based on CASG‑compliant patterns.

### Time Rounding – How It Works

Both *First Rest* and *ToD* are rounded to the nearest 5‑minute mark **when you press *Calculate***. They do **not** adjust as you type.

`21:47 → 21:45`   `04:03 → 04:05`

### Output Explained

* **Rest Pattern**

  * **NON‑ULR** – Two equal rest blocks; any spare minutes that can't be divided equally (≤ 9) are added to the meal duration.
  * **ULR** – Four‑block preferred pattern:

    * AKL–JFK → Long/Long then Short/Short
    * JFK–AKL → Short/Short then Long/Long
    * The exact durations depend on the available window.
* **Meal Service Timing** – Adjusted automatically so the meal service time ends at Top of Descent.
* **Sense Check** – Shows remaining flight time at the start of *First Rest*. It should match Airshow within ±10 min. If it doesn’t, re‑check your inputs and confirm ToD time with the flight deck.

### Warnings and Flags

* **Meal Duration** – < 1 h 45 m or > 2 h 30 m triggers a prompt to confirm.
* **Long Rest on NON‑ULR** – Rest > 4 h is flagged for review.
* **ULR Rest Shortfalls** – If total rest time is less than usual the calculator offers **Fallback 1** and/or **Fallback 2** patterns (per CASG). When both appear, the ISM selects one and notes it in the sector report.
* **Route‑Based Notices** – AKL–JFK & JFK–AKL routes display reminders that patterns follow CASG guidance and should only be changed if operationally necessary.

---

## File Structure

| File              | Purpose                                                                                        |
| ----------------- | ---------------------------------------------------------------------------------------------- |
| **crewrest.html** | Single‑page app — contains the markup, CSS, and JavaScript logic. No external assets required. |

---

## Key Features

| Category                                                                                      | Details                                                                                                                                 |
| --------------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------- |
| **Route selection**                                                                           | `NON‑ULR`, `AKL‑JFK`, or `JFK‑AKL`.                                                                                                     |
| **Inputs**                                                                                    | • **First Rest Start** – destination local time                                                                                         |
| • **Top of Descent** – destination local time                                                 |                                                                                                                                         |
| • **Meal‑service length** – accepts `H:MM` **or just digits** (`145` auto‑formats to `1:45`). |                                                                                                                                         |
| **Smart meal field**                                                                          | Live **auto‑colon insertion** while typing; deletes behave naturally.                                                                   |
| **5‑minute rounding**                                                                         | \*\*Inputs are rounded once you press \*\****Calculate*** (not on every blur/change).                                                   |
| **Automatic outputs**                                                                         | • Non‑ULR: two equal rest blocks.                                                                                                       |
| • ULR: calculated long/short pattern or one of two fallback patterns.                         |                                                                                                                                         |
| • Meal‑service window and sense‑check advisory.                                               |                                                                                                                                         |
| **Sense‑check**                                                                               | Shows “When First Rest starts, *X h Y m* remaining in flight (±10 min)” — internally adds a 30 min buffer to align with Airshow timing. |
| **Warnings & FYI banners**                                                                    | • Meal service < 1 h 45 or > 2 h 30.                                                                                                    |
| • Non‑ULR rest block > 4 h (prompts a double‑check).                                          |                                                                                                                                         |
| • ULR fallback banners (“≤ 4 h per crew” or “one long rest each” guidance).                   |                                                                                                                                         |
| **Built‑in rules**                                                                            | Internal 10 min change‑over breaks, min / max rest lengths, extra‑time redistribution, and CASG fallback triggers.                      |
| **Mobile friendly**                                                                           | Responsive layout; iPad safe‑area padding; large single‑tap *Calculate* button.                                                         |

---

## Usage (Quick‑Start)

1. **Open** `crewrest.html` in any modern browser (desktop, iPad, phone).
2. **Select Route.**
3. **Enter Times.**

   * *First Rest Start*
   * *Top of Descent*
   * *Meal Service Length* (`145` → auto‑formats to `1:45`).
4. **Click *****Calculate*****.**

   * Inputs are first **rounded to the nearest 5 min**.
   * Results plus any warnings appear instantly.

---

## Output Scenarios

### NON‑ULR

* Two equal rest blocks separated by a 10 min break.
* If either block would exceed **240 min**, an FYI banner advises a manual review to check times input are correct.

### ULR (AKL‑JFK or JFK‑AKL)

1. **Calculated Pattern**

   * `Long/Long/Short/Short` (AKL → JFK)
   * `Short/Short/Long/Long` (JFK → AKL)
2. **Fallback 1** – one long rest per crew group (≥ 90 min each).
3. **Fallback 2** – four‑block pattern with 90 min shorts and computed longs.

   * Banners indicate:

     * “Total rest per crew ≤ 4 h – ISM discretion, sector report.”
     * or “One long rest each – CASG recommends sector report.”

*All patterns finish 10 min before the meal; any leftover minutes (≤ 5 min) are added to the meal duration.*

---

## Decision Tree Logic (Complete)

> **Inputs required:**
> • First Rest Start • Top of Descent • Meal‑service length
> *(All inputs auto‑rounded to the nearest 5 min when you press ****Calculate****.)*

```
STEP 1. Is this flight Ultra‑Long‑Range (ULR)? (AKL⇄JFK pairs)
────────────────────────────────────────────────────────────────
 NO  → follow NON‑ULR branch below
 YES → follow ULR branch below
```

### ► NON‑ULR Branch (Short / Medium range)

```
N‑1. Usable rest window = First Rest Start → 10 min before meal.

N‑2. Is window ≥ 1 h 10 m (70 min)?
     NO  → Warn: “Not enough time for two rest blocks.”
     YES →

N‑3. RestLen = floor((window − 10) ÷ 2) rounded down to 5 min.

N‑4. Does either rest exceed 4 h (240 min)?
     YES → FYI: “Unusually long rest – please re‑check times.”

N‑5. ≤ 5 min leftover after 2×RestLen + 10‑min break?
     YES → Extra minutes added to meal duration.
     NO  → Tool re‑sizes rests to fit exactly.

OUTPUT → Rest A, 10‑min break, Rest B. Meal starts 10 min later.
```

### ► ULR Branch (AKL‑JFK or JFK‑AKL)

```
U‑1. Direction?
     • AKL → JFK  → label order Long/Long then Short/Short
     • JFK → AKL  → label order Short/Short then Long/Long

U‑2. Usable rest window = First Rest Start → 10 min before meal.

U‑3. Is window ≥ 8 h 30 m (510 min)?
     YES → CALCULATED PATTERN (preferred)
           • 2 Long (≤ 4 h) + 2 Short (≥ 2 h) + 10‑min breaks. ✓
     NO  →

U‑4. Is window ≥ 6 h 30 m (390 min)?
     YES → CHOICE WINDOW – ISM may pick either:
           ┌ Fallback 1 ── One long rest each (≥ 90 min) ┐
           │                                               │
           └ Fallback 2 ── 2 long + 2 short (90 min shorts)┘
           Banner: “Total rest per crew ≤ 4 h – ISM discretion.”
     NO  →

U‑5. Is window ≥ 3 h 10 m (190 min)?
     YES → Fallback 1 only (one long rest each, ≥ 90 min)
           Banner: “One long rest each – CASG recommends report.”
     NO  →

U‑6. RESULT → No legal pattern fits – red warning; re‑check inputs.
```

#### Quick Reference

| Window size              | Tool displays                         |
| ------------------------ | ------------------------------------- |
| ≥ 8 h 30 m               | Calculated LLSS / SSLL                |
| 6 h 30 m – 8 h 29 m 59 s | **Both** Fall‑back 1 & 2 (ISM choice) |
| 3 h 10 m – 6 h 29 m 59 s | Fallback 1 only                       |
| < 3 h 10 m               | No legal pattern                      |

---

## Universal Warnings & Reminders

* **Meal‑service length**

  * < 1 h 45 m → “Less than 1 h 45 m – deliberate?”
  * >  2 h 30 m → “More than 2 h 30 m – deliberate?”
* **Sense‑check**

  * When First Rest begins, Airshow should show remaining flight time within ±10 min of the advisory on screen.
* On NON‑ULR, a rest block > 4 h triggers a “double‑check” banner.
* All patterns finish **10 min before** the meal; any leftover ≤ 5 min is added to the meal duration automatically.

---

## Change Log (2025‑07‑26)

* New auto‑colon meal‑length entry.
* Rounding deferred to *Calculate* click.
* Additional warnings: Non‑ULR rest > 4 h; ULR fallback banners.
* **NEW** user‑friendly yes/no decision tree replacing previous flowchart.
