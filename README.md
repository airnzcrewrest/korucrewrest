# Air NZ Cabin Crew Rest Calculator

### Overview
This simple HTML/JavaScript application calculates crew rest periods specifically designed for the AKL-JFK and AKL-ORD flights. It employs a Short-Short-Long-Long rest pattern to manage crew fatigue effectively, and abides by all rules set by the Crew Fatigue Working Group.

### Usage

1. **Open the HTML file in your browser**.
2. Enter the following details:
   - First rest start time (local destination time)
   - Top of Descent time (local destination time)
   - Desired time allowance for the next meal service (format: h:mm)
3. Click **Calculate**.

The calculator will display clearly defined rest periods, meal service timings, and a sense-check value.

### Rules Implemented

- **Short rests:** Minimum of 2 hours each.
- **Long rests:** Any remaining available time after short rests, equally distributed.
- **Spare minutes:** Allocated according to the following logic:
  - Exactly 10 spare minutes: add 5 minutes to each short rest.
  - Less than 10 spare minutes: add these minutes to the final meal service.
  - More than 10 spare minutes: first add 5 minutes to each short rest, then add any remaining minutes to the final meal service.

### Technical Information
- **Languages:** HTML, CSS, JavaScript
- **Dependencies:** None (pure JavaScript)

### Browser Compatibility
Works in all modern browsers supporting JavaScript and HTML5 input types.

### Notes
- Specifically designed for AKL-JFK and AKL-ORD sectors only.
- Not suitable or authorized for other sectors or rest patterns.
