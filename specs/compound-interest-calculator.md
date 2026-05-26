# Compound Interest Calculator - Full Product Specification

## 1. Overview

A web-based compound interest calculator that allows users to project investment growth over time. Designed for personal finance use cases: savings accounts, retirement planning, and investment comparisons. Should be intuitive, visually engaging, and immediately useful without requiring financial expertise.

---

## 2. Goals & Success Criteria

- Users can calculate compound interest in under 30 seconds with no instructions needed.
- Results are presented clearly with both a summary and a visual breakdown.
- The calculator handles edge cases gracefully (zero values, very large numbers, invalid inputs).
- The app is fully functional on desktop and mobile browsers.

---

## 3. Functional Requirements

### 3.1 Inputs

| Field | Label | Type | Validation |
|---|---|---|---|
| Principal | Starting Amount ($) | Number | Required, > 0 |
| Annual Interest Rate | Interest Rate (%) | Number | Required, > 0, <= 100 |
| Compounding Frequency | Compounded | Dropdown | Required |
| Time Period | Duration | Number | Required, > 0 |
| Time Unit | - | Toggle | Years / Months |
| Additional Contributions | Monthly Contribution ($) | Number | Optional, >= 0 |

Compounding Frequency Options: Daily (365x/year), Weekly (52x/year), Monthly (12x/year), Quarterly (4x/year), Semi-Annually (2x/year), Annually (1x/year)

### 3.2 Outputs

- **Final Balance** - total value at end of period
- **Total Principal** - sum of initial deposit + all contributions
- **Total Interest Earned** - final balance minus total principal
- **Growth Chart** - year-by-year or month-by-month area chart showing balance over time
- **Breakdown Table** - optional expandable table (period, contributions, interest earned, balance)

### 3.3 Calculations

Standard compound interest formula:

A = P(1 + r/n)^(nt) + PMT x [((1 + r/n)^(nt) - 1) / (r/n)]

Where: A = Final balance, P = Principal, r = Annual rate (decimal), n = Compounding frequency per year, t = Time in years, PMT = Regular contribution per period.

### 3.4 Real-Time Updates

- Results and chart update instantly as the user adjusts any input (no submit button required).
- Debounce input changes by 300ms to avoid excessive recalculation on rapid typing.

---

## 4. UI/UX Guidelines

### 4.1 Design Aesthetic

- Tone: Clean, modern, trustworthy - financial tool with approachable personality.
- Theme: Light background, strong typographic hierarchy, single bold accent color (deep teal or indigo).
- Layout: Two-column on desktop (inputs left, results + chart right); single-column on mobile.
- Typography: Distinctive display font for numbers/results; clean sans-serif for labels and body.
- Avoid: Purple gradients, generic shadows, cookie-cutter fintech aesthetics.

### 4.2 Input Panel

- Currency prefix ($) for monetary fields; percent suffix (%) for rate field.
- Optional slider companions for Principal, Rate, and Duration inputs.
- Inline validation: red underline + helper text on invalid input; no modal errors.

### 4.3 Results Panel

- Final Balance displayed prominently in large typography.
- Total Interest Earned and Total Principal as secondary stats.
- Color coding: principal in neutral, interest in accent color.
- Count-up animation on Final Balance when values update.

### 4.4 Chart

- Stacked area chart: one area for principal, one for interest, color-coded.
- Hoverable tooltips with exact values at each data point.
- X-axis: time (years or months); Y-axis: dollar amount formatted with K/M suffixes.
- Responsive: resizes fluidly with the viewport.

### 4.5 Breakdown Table

- Hidden by default; toggled via "Show Year-by-Year Breakdown".
- Columns: Year/Period | Starting Balance | Contributions | Interest Earned | Ending Balance.
- Alternating row shading for readability; optional CSV export.

### 4.6 Accessibility

- All inputs have associated label elements.
- Chart includes accessible data table alternative for screen readers.
- Keyboard navigable; tab order follows visual layout.
- Minimum 4.5:1 contrast ratio for all text.
- ARIA live region on results panel for screen reader announcements.

### 4.7 Responsive Behavior

| Breakpoint | Layout |
|---|---|
| >= 1024px | Two-column: inputs left, results + chart right |
| 768px-1023px | Two-column, compressed |
| < 768px | Single column: inputs -> results -> chart -> table |

---

## 5. Technical Requirements

### 5.1 Stack

- Framework: React (functional components + hooks)
- Charting: Recharts (preferred) or Chart.js
- Styling: Tailwind CSS or CSS Modules
- No backend required - all calculations are client-side
- No external API dependencies

### 5.2 State Management

- Local component state (useState) is sufficient.
- All calculator inputs stored in a single state object for easy serialization.

### 5.3 URL State (Stretch Goal)

- Encode calculator inputs into URL query parameters for shareable links.
- Example: ?p=10000&r=7&n=12&t=20&pmt=200

### 5.4 Performance

- Initial load under 2 seconds on a standard broadband connection.
- Chart renders within 100ms of input change.
- Use useMemo for calculation logic to avoid unnecessary re-renders.

### 5.5 Browser Support

- Chrome, Firefox, Safari, Edge (latest 2 versions each)
- iOS Safari 15+, Android Chrome 110+

---

## 6. QA Considerations

### 6.1 Test Cases

| Scenario | Input | Expected Output |
|---|---|---|
| Basic calculation | P=1000, r=5%, n=annually, t=10yr | ~$1,628.89 |
| With contributions | P=1000, r=7%, n=monthly, t=30yr, PMT=$100 | ~$121,997 |
| Zero contribution | PMT=0 | Same as basic formula |
| High frequency compounding | n=daily vs n=annually | Daily yields slightly more |
| Edge: very short duration | t=1 month | Correct fractional year calculation |
| Edge: large numbers | P=$1,000,000 | No overflow, formats as $1M+ |
| Invalid input | Rate = -5% | Inline error, no calculation |
| Invalid input | Principal = 0 | Inline error or zero result |

### 6.2 Cross-Browser Testing

- Validate chart rendering across all supported browsers.
- Verify slider behavior on touch devices.
- Confirm CSV export (if implemented) works in all browsers.

---

## 7. Out of Scope (v1)

- User accounts or saved calculations
- Tax calculations
- Inflation adjustment
- Multiple scenarios / comparison mode (consider for v2)
- Backend API

---

## 8. Open Questions

- Should contributions be made at the beginning or end of each period? (Default: end of period)
- Should we support withdrawal scenarios (negative contributions)?
- Is CSV export in scope for v1?

---

## 9. Deliverables

- [ ] React component: CompoundInterestCalculator
- [ ] Unit tests for calculation logic
- [ ] Integration/E2E tests covering key user flows
- [ ] Responsive layout verified on mobile and desktop
- [ ] Accessibility audit passed