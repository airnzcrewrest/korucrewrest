# Air NZ Cabin Crew Rest Calculator

## Overview

This is a self-contained HTML/JavaScript tool for calculating compliant cabin crew rest and meal schedules for Air New Zealand flights. It supports both NON‑ULR (short/medium range) and ULR (Ultra Long Range) routes with built‑in contractual and CASG/Fatigue Working Group guidelines.

## File Structure

* **crewrest.html**: Main HTML file containing markup, styles, and embedded JavaScript logic.

## Features

* Route selection (`NON-ULR`, `AKL-JFK`, `JFK-AKL`).
* Input fields for first rest time, top of descent (TOD) time in destination port's local time, and desired next meal service duration in hours/minutes.
* Automatic calculation of rest blocks, meal service window, and sense-check summary.
* Warnings for non‑standard meal durations and rest durations.
* Rounding to nearest 5 minutes, with any remaining time (up to 5 minutes extra) added to meal service duration.
* Distinct handling of ULR routes with calculated and fallback options to ensure CASG recommendations are met.

## Usage

1. Open **crewrest.html** in a modern browser (desktop or mobile - optimized for iPad).
2. Select the flight route.
3. Enter the first rest start time and top of descent time (local destination times).
4. Enter meal service length (format `H:MM`).
5. Click **Calculate** to view:

   * Two equal rest blocks for NON‑ULR flights.
   * Calculated or fallback rest patterns for ULR flights.
   * Meal service start/end and tops of descent summary.
   * Sense-check indicator for remaining flight time at first rest.

## Verbal Flowchart for ULR Routes

1. **Initialize Inputs**

   * `route`: either `AKL-JFK` or `JFK-AKL`.
   * `firstRest`, `tod`, `serviceMin` (in minutes).

2. **Calculate Windows**

   * `mealEnd = tod`
   * `mealStartBase = mealEnd - serviceMin`
   * `finalRestEnd = mealStartBase - 10` (10 min break before meal)
   * `restWindowAll = duration(firstRest → finalRestEnd)`
   * **Fixed internal changeover**: `internalCO = 3 * 10` min (3 breaks of 10 min)
   * `breakWindow = restWindowAll - internalCO`

3. **Set Initial Rest Durations**

   * `minShort = 120` min (minimum short rest)
   * `maxLong = 240` min (maximum long rest)
   * `shortRest = minShort`
   * `extra = breakWindow - 2 * shortRest`
   * `longRest = floor(min(extra/2, maxLong) / 5) * 5`
   * `rem = extra - 2 * longRest`
   * If `longRest == maxLong` and `rem > 0`:

     * Distribute remaining `rem` evenly into both `shortRest` blocks (rounded down to 5 min)

4. **Check for Calculated Pattern**

   * If both `longRest >= minShort` **and** `shortRest >= minShort`:

     * **Pattern =**

       * `AKL-JFK`: **Long/Long/Short/Short**
       * `JFK-AKL`: **Short/Short/Long/Long**
     * Schedule four rest blocks in sequence, each separated by 10 min:

       1. Rest A (dur = first block)
       2. 10 min break
       3. Rest B (dur = second block)
       4. 10 min break
       5. Rest C (dur = third block)
       6. 10 min break
       7. Rest D (dur = fourth block, ends 10 min before meal)
     * Recalculate `mealStart = mealEnd - (serviceMin + leftoverP)` where `leftoverP` is unused time in breakWindow.
     * **End** – Display calculated pattern.

5. **Else → Fallback Options**

   * **Fallback Option 1**: One long rest per group

     * Compute `fb1len = floor((restWindowAll - 10) / 2 / 5) * 5`
     * If `fb1len >= 90` min:

       * Two equal rests of `fb1len`, 10 min break between, then meal.
   * **Fallback Option 2**: Four-block pattern with minimum rests

     * Required total: `4*90 + 3*10 =  4 short rests + 3 breaks`
     * If `restWindowAll >= required`:

       * Compute `longFB = floor((restWindowAll - (2*90 + 3*10)) / 2 / 5) * 5`
       * Build pattern:

         * `AKL-JFK`: **Long/Long/Short/Short** fallback
         * `JFK-AKL`: **Short/Short/Long/Long** fallback
       * Four rests of durations (`longFB`, `longFB`, 90, 90) or vice versa.
       * Separate by 10 min breaks.  Final block ends 10 min before meal.
   * If neither fallback applies:

     * **No legal rest pattern found** – display warning.
