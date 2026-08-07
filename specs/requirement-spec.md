# Specification Document: Compound Interest Calculator

## Document Metadata

| Property | Value |
|----------|-------|
| **Generated** | 2026-05-26 |
| **Repository** | mserpone/qa-pipeline-sandbox |
| **Branch** | main |
| **Document Version** | 3.0 |
| **Last Updated** | 2026-05-26T21:07:59-04:00 |

---

## Requirement Overview

### Identification

| Property | Value |
|----------|-------|
| **Requirement ID** | 40309985 |
| **Project ID** | 127305 |
| **PID** | RQ-68 |
| **Name** | MJ-41 Compound Interest Calculator - Real-Time Calculation with Visual Results |
| **Status** | New |
| **Priority** | Must have |
| **Type** | Functional |
| **External Link** | [MJ-41 in Jira](https://tricentisqtestus.atlassian.net/browse/MJ-41) |

### Key Dates

| Event | Date |
|-------|------|
| **Created** | 2026-05-26T21:07:59-04:00 |
| **Last Modified** | 2026-05-26T21:07:59-04:00 |

### References

- **qTest URL**: [View in qTest](https://sademo.qtestnet.com/p/127305/portal/project#tab=requirements&object=5&id=40309985)
- **GitHub Specification**: [compound-interest-calculator.md](https://github.com/mserpone/qa-pipeline-sandbox/blob/main/specs/compound-interest-calculator.md)
- **Jira Issue**: [MJ-41](https://tricentisqtestus.atlassian.net/browse/MJ-41)

---

## Requirement Description

### User Story

As a personal finance user, I want to input investment details and instantly see compound interest projections with a visual breakdown, so that I can make informed decisions about my savings and investments without needing financial expertise.

---

## Functional Requirements

### Input Fields

| Field | Type | Validation | Required |
|-------|------|------------|----------|
| **Starting Amount ($)** | Number | Must be > 0 | Yes |
| **Interest Rate (%)** | Number | Must be > 0 and ≤ 100 | Yes |
| **Compounded** | Dropdown | Daily, Weekly, Monthly, Quarterly, Semi-Annually, Annually | Yes |
| **Duration** | Number | Must be > 0 | Yes |
| **Time Unit** | Toggle | Years or Months | Yes |
| **Monthly Contribution ($)** | Number | Must be ≥ 0 | No |

### Output Components

1. **Final Balance** - Total value at end of the investment period
2. **Total Principal** - Sum of initial deposit plus all contributions
3. **Total Interest Earned** - Final balance minus total principal
4. **Growth Chart** - Stacked area chart showing principal vs. interest over time
5. **Breakdown Table** - Expandable year-by-year period breakdown (hidden by default)

### Calculation Formula

```
A = P(1 + r/n)^(nt) + PMT × [((1 + r/n)^(nt) - 1) / (r/n)]
```

Where:
- **A** = Final balance
- **P** = Principal (starting amount)
- **r** = Annual rate (decimal)
- **n** = Compounding frequency per year
- **t** = Time in years
- **PMT** = Regular contribution per compounding period

### Real-Time Behavior

- Results and chart must update instantly as any input changes (no submit button)
- Input changes must be debounced by 300ms to avoid excessive recalculation

---

## Acceptance Criteria

- ✅ **AC1**: All required inputs are validated inline with red underline and helper text on error
- ✅ **AC2**: Final Balance, Total Principal, and Total Interest Earned are displayed after valid input
- ✅ **AC3**: Growth chart renders as a stacked area chart with principal and interest color-coded
- ✅ **AC4**: Chart tooltips show exact values on hover
- ✅ **AC5**: Breakdown table is hidden by default and toggleable
- ✅ **AC6**: Results update within 300ms of any input change
- ✅ **AC7**: Calculator produces correct output: P=1000, r=5%, annually, 10yr = ~$1,628.89
- ✅ **AC8**: Calculator produces correct output with contributions: P=1000, r=7%, monthly, 30yr, PMT=$100 = ~$121,997
- ✅ **AC9**: Invalid inputs (negative rate, zero principal) show inline errors and block calculation
- ✅ **AC10**: Layout is responsive: two-column on desktop, single-column on mobile

---

## Technical Specifications

| Component | Technology/Requirement |
|-----------|----------------------|
| **Framework** | React (functional components + hooks) |
| **Charting Library** | Recharts (preferred) |
| **Styling** | Tailwind CSS or CSS Modules |
| **Architecture** | No backend required — all calculations are client-side |
| **Performance** | Use useMemo for calculation logic to avoid unnecessary re-renders |
| **Update Latency** | < 300ms with debouncing for result updates |

---

## Suitability Assessment

### Overall Grade: **A (96/100)**

**Assessment**: SUITABLE  
**Confidence Score**: 0.96

This requirement is **HIGHLY SUITABLE** for manual test case generation. It scores 96/100 with exceptional testability due to concrete test cases with expected outputs, comprehensive acceptance criteria, and clear pass/fail conditions. The requirement provides excellent detail on inputs, outputs, behaviors, and edge cases.

### Scoring Breakdown

| Criterion | Score | Maximum | Notes |
|-----------|-------|---------|-------|
| **Completeness** | 23 | 25 | Excellent coverage of functional requirements, inputs, outputs, edge cases, and dependencies. Minor gap: browser compatibility and accessibility requirements not explicitly stated. |
| **Clarity** | 24 | 25 | Very clear and unambiguous language throughout. User story format is well-structured. Technical terms (debouncing, compounding) are explained. Formula is provided with variable definitions. Minimal ambiguity. |
| **Testability** | 25 | 25 | Outstanding testability. Includes specific test cases with exact expected outputs (e.g., P=1000, r=5%, 10yr = $1,628.89). Measurable acceptance criteria with concrete timing requirements (300ms debounce). Clear pass/fail conditions for validation and behavior. |
| **Detail** | 24 | 25 | Comprehensive detail across all aspects. User workflows are clear, input/output specs are complete, error handling is defined, UI behavior is detailed (tooltips, responsive layout, expandable table). Technical implementation notes are helpful. |
| **TOTAL** | **96** | **100** | |

### Identified Test Areas

The following 10 test areas have been identified for comprehensive coverage:

1. **Input validation** (required fields, numeric ranges, zero/negative values)
2. **Calculation accuracy** (with and without monthly contributions)
3. **Real-time update behavior** and 300ms debouncing
4. **Growth chart rendering** (stacked area, color coding, tooltips)
5. **Breakdown table toggle** functionality
6. **Responsive layout** (desktop two-column vs mobile single-column)
7. **Error messaging** (inline validation with red underline and helper text)
8. **Dropdown selection** (compounding frequency options)
9. **Time unit toggle** (Years/Months)
10. **Edge cases** (maximum values, decimal inputs, boundary conditions)

### Missing Elements

While the requirement is excellent, the following elements could enhance completeness:

1. Browser and device compatibility requirements
2. Accessibility standards (WCAG compliance, screen reader support, keyboard navigation)
3. Upper bound constraints for inputs (max principal amount, max years/months)
4. Decimal precision requirements for displayed results
5. Performance requirements beyond the 300ms debounce
6. Internationalization/localization needs (currency formats, number formats)
7. Data persistence or session handling (if applicable)
8. Loading or processing states (if calculations could take time)

### Recommendations

1. **Add explicit browser/device compatibility matrix** (e.g., Chrome 90+, Safari 14+, mobile browsers)
2. **Specify accessibility requirements** (ARIA labels, keyboard navigation, screen reader compatibility)
3. **Define upper bounds for inputs** to prevent overflow (e.g., max principal $10M, max duration 100 years)
4. **Clarify decimal precision** for displayed results (e.g., currency to 2 decimal places)
5. **Consider adding test cases** for extreme values and rapid input changes
6. **Document expected behavior** when compounding frequency doesn't align with contribution frequency
7. **Add visual regression testing criteria** for chart rendering across browsers

### Summary

This requirement is HIGHLY SUITABLE for manual test case generation. It scores 96/100 with exceptional testability due to concrete test cases with expected outputs, comprehensive acceptance criteria, and clear pass/fail conditions. The requirement provides more than sufficient information to create a thorough test suite. This is a model requirement document for test case generation.

---

## Generated Test Cases

### Coverage Summary

| Metric | Value |
|--------|-------|
| **Total Test Cases** | 25 |
| **Acceptance Criteria Covered** | 10 out of 10 |
| **Coverage Percentage** | 100% |

### Test Case Distribution by Priority

| Priority | Count | Test Cases |
|----------|-------|------------|
| **High** | 10 | TC-01, TC-02, TC-03, TC-05, TC-06, TC-07, TC-09, TC-11, TC-12, TC-19 |
| **Medium** | 11 | TC-04, TC-08, TC-10, TC-13, TC-14, TC-15, TC-17, TC-20, TC-21, TC-22, TC-24 |
| **Low** | 4 | TC-16, TC-18, TC-23, TC-25 |

### Test Case Distribution by Type

| Type | Count | Test Cases |
|------|-------|------------|
| **Functional** | 22 | TC-01 to TC-06, TC-08 to TC-11, TC-13 to TC-21, TC-23, TC-24 |
| **Performance** | 2 | TC-07, TC-22 |
| **Integration** | 1 | TC-20, TC-24 |
| **Edge Case** | 3 | TC-15, TC-16, TC-17, TC-18 |
| **UI/Responsive** | 2 | TC-13, TC-14 |
| **Accessibility** | 1 | TC-25 |

---

## Test Cases

### TC-01: Validate Required Field - Starting Amount Inline Validation

**Priority**: High  
**Type**: Functional  
**Acceptance Criteria**: AC1, AC9

**Description**: Verify that the Starting Amount field displays inline validation error when empty or invalid

**Preconditions**: Calculator page is loaded and empty

**Test Steps**:

| Step | Action | Expected Result |
|------|--------|-----------------|
| 1 | Leave Starting Amount field empty and click or tab to another field | Starting Amount field shows red underline and helper text 'This field is required' |
| 2 | Enter '0' in Starting Amount field | Field shows red underline and helper text 'Must be greater than 0' |
| 3 | Enter '-100' in Starting Amount field | Field shows red underline and helper text 'Must be greater than 0' |
| 4 | Verify calculation results section | No results are displayed; calculation is blocked |

---

### TC-02: Validate Required Field - Interest Rate Inline Validation

**Priority**: High  
**Type**: Functional  
**Acceptance Criteria**: AC1, AC9

**Description**: Verify that the Interest Rate field displays inline validation error for invalid values

**Preconditions**: Calculator page is loaded

**Test Steps**:

| Step | Action | Expected Result |
|------|--------|-----------------|
| 1 | Leave Interest Rate field empty and tab to another field | Interest Rate field shows red underline and helper text 'This field is required' |
| 2 | Enter '0' in Interest Rate field | Field shows red underline and helper text 'Must be greater than 0' |
| 3 | Enter '-5' in Interest Rate field | Field shows red underline and helper text 'Must be greater than 0' |
| 4 | Enter '150' in Interest Rate field | Field shows red underline and helper text 'Must be less than or equal to 100' |

---

### TC-03: Validate Required Field - Duration Inline Validation

**Priority**: High  
**Type**: Functional  
**Acceptance Criteria**: AC1, AC9

**Description**: Verify that the Duration field displays inline validation error for invalid values

**Preconditions**: Calculator page is loaded

**Test Steps**:

| Step | Action | Expected Result |
|------|--------|-----------------|
| 1 | Leave Duration field empty and tab to another field | Duration field shows red underline and helper text 'This field is required' |
| 2 | Enter '0' in Duration field | Field shows red underline and helper text 'Must be greater than 0' |
| 3 | Enter '-10' in Duration field | Field shows red underline and helper text 'Must be greater than 0' |

---

### TC-04: Validate Optional Field - Monthly Contribution Validation

**Priority**: Medium  
**Type**: Functional  
**Acceptance Criteria**: AC1

**Description**: Verify that Monthly Contribution field accepts zero or positive values only

**Preconditions**: Calculator page is loaded with valid required inputs entered

**Test Steps**:

| Step | Action | Expected Result |
|------|--------|-----------------|
| 1 | Leave Monthly Contribution field empty | No error is shown; field is optional |
| 2 | Enter '0' in Monthly Contribution field | No error is shown; zero is valid |
| 3 | Enter '-50' in Monthly Contribution field | Field shows red underline and helper text 'Must be greater than or equal to 0' |
| 4 | Enter '100' in Monthly Contribution field | No error is shown; positive value is accepted and results update |

---

### TC-05: Calculate Simple Compound Interest Without Contributions

**Priority**: High  
**Type**: Functional  
**Acceptance Criteria**: AC2, AC7

**Description**: Verify accurate calculation for P=1000, r=5%, annually, 10 years, no contributions

**Preconditions**: Calculator page is loaded

**Test Steps**:

| Step | Action | Expected Result |
|------|--------|-----------------|
| 1 | Enter '1000' in Starting Amount field | Value is accepted |
| 2 | Enter '5' in Interest Rate field | Value is accepted |
| 3 | Select 'Annually' from Compounded dropdown | Annually is selected |
| 4 | Enter '10' in Duration field and ensure 'Years' is selected | Duration and time unit are set |
| 5 | Leave Monthly Contribution empty or set to 0 | No contribution is added |
| 6 | Verify Final Balance displayed | Final Balance shows approximately $1,628.89 |
| 7 | Verify Total Principal displayed | Total Principal shows $1,000.00 |
| 8 | Verify Total Interest Earned displayed | Total Interest Earned shows approximately $628.89 |

---

### TC-06: Calculate Compound Interest With Monthly Contributions

**Priority**: High  
**Type**: Functional  
**Acceptance Criteria**: AC2, AC8

**Description**: Verify accurate calculation for P=1000, r=7%, monthly compounding, 30 years, PMT=$100

**Preconditions**: Calculator page is loaded

**Test Steps**:

| Step | Action | Expected Result |
|------|--------|-----------------|
| 1 | Enter '1000' in Starting Amount field | Value is accepted |
| 2 | Enter '7' in Interest Rate field | Value is accepted |
| 3 | Select 'Monthly' from Compounded dropdown | Monthly is selected |
| 4 | Enter '30' in Duration field and ensure 'Years' is selected | Duration is set to 30 years |
| 5 | Enter '100' in Monthly Contribution field | Monthly contribution of $100 is set |
| 6 | Verify Final Balance displayed | Final Balance shows approximately $121,997 |
| 7 | Calculate expected Total Principal: 1000 + (100 × 12 × 30) = $37,000 | Total Principal shows approximately $37,000 |
| 8 | Verify Total Interest Earned displayed | Total Interest Earned shows approximately $84,997 (Final Balance minus Total Principal) |

---

### TC-07: Verify Real-Time Calculation Updates

**Priority**: High  
**Type**: Performance  
**Acceptance Criteria**: AC6

**Description**: Verify that results update within 300ms after input changes with debouncing

**Preconditions**: Calculator page is loaded with valid inputs: P=1000, r=5%, annually, 10 years

**Test Steps**:

| Step | Action | Expected Result |
|------|--------|-----------------|
| 1 | Note the current Final Balance value (~$1,628.89) | Initial calculation is displayed |
| 2 | Change Starting Amount from 1000 to 2000 and start a timer | Results update within 300ms; Final Balance approximately doubles to ~$3,257.79 |
| 3 | Rapidly type multiple digits in Interest Rate field (e.g., change 5 to 10) | Calculation is debounced; final update occurs within 300ms after last keystroke |
| 4 | Change Compounded dropdown from 'Annually' to 'Monthly' | Results update within 300ms; Final Balance changes to reflect monthly compounding |
| 5 | Toggle Time Unit from 'Years' to 'Months' and back to 'Years' | Each toggle triggers recalculation within 300ms |

---

### TC-08: Verify Growth Chart Rendering and Visualization

**Priority**: Medium  
**Type**: Functional  
**Acceptance Criteria**: AC3

**Description**: Verify that growth chart renders as stacked area chart with proper color coding

**Preconditions**: Calculator page is loaded with valid inputs entered (P=1000, r=5%, annually, 10 years)

**Test Steps**:

| Step | Action | Expected Result |
|------|--------|-----------------|
| 1 | Verify that a chart is visible on the page | Growth chart is displayed below or beside the input fields |
| 2 | Verify chart type | Chart is a stacked area chart (not line or bar chart) |
| 3 | Verify principal area is visible and color-coded | Principal area is shown in one distinct color (e.g., blue) and labeled 'Principal' |
| 4 | Verify interest area is visible and color-coded differently | Interest area is shown in a different color (e.g., green) and labeled 'Interest' |
| 5 | Verify chart shows progression over time | X-axis shows time periods (years); Y-axis shows dollar amounts; stacked areas show growth |
| 6 | Verify legend is present | Legend identifies Principal and Interest areas with corresponding colors |

---

### TC-09: Verify Chart Tooltips Show Exact Values

**Priority**: High  
**Type**: Functional  
**Acceptance Criteria**: AC4

**Description**: Verify that hovering over the chart displays tooltips with precise values

**Preconditions**: Calculator page is loaded with valid inputs; growth chart is rendered

**Test Steps**:

| Step | Action | Expected Result |
|------|--------|-----------------|
| 1 | Hover mouse over the chart at the starting point (year 0) | Tooltip appears showing exact Principal value ($1,000) and Interest value ($0) |
| 2 | Hover over the chart at midpoint (year 5) | Tooltip appears showing exact Principal ($1,000) and Interest earned values at year 5 |
| 3 | Hover over the chart at endpoint (year 10) | Tooltip appears showing exact Final Balance breakdown: Principal ($1,000) and Interest (~$628.89) |
| 4 | Move mouse away from chart | Tooltip disappears |
| 5 | Hover over different data points on the chart | Tooltip follows mouse and displays accurate values for each time period |

---

### TC-10: Verify Breakdown Table Hidden by Default and Toggleable

**Priority**: Medium  
**Type**: Functional  
**Acceptance Criteria**: AC5

**Description**: Verify that breakdown table is initially hidden and can be shown/hidden via toggle

**Preconditions**: Calculator page is loaded with valid inputs; results are displayed

**Test Steps**:

| Step | Action | Expected Result |
|------|--------|-----------------|
| 1 | Verify initial state of breakdown table section | Breakdown table is not visible; only a toggle button/link is visible (e.g., 'Show Breakdown' or expand icon) |
| 2 | Click the toggle button/link to expand breakdown table | Breakdown table becomes visible, showing year-by-year or period-by-period details |
| 3 | Verify table content | Table shows columns for Period, Principal, Interest Earned, Total Balance with data for each period |
| 4 | Click the toggle button/link again to collapse breakdown table | Breakdown table is hidden again; toggle button shows 'Show Breakdown' or collapsed state |
| 5 | Expand table again and change an input value | Table remains expanded and updates with new calculation data |

---

### TC-11: Verify Compounding Frequency Dropdown Options

**Priority**: High  
**Type**: Functional  
**Acceptance Criteria**: AC1

**Description**: Verify that all six compounding frequency options are available and selectable

**Preconditions**: Calculator page is loaded

**Test Steps**:

| Step | Action | Expected Result |
|------|--------|-----------------|
| 1 | Click on the Compounded dropdown | Dropdown expands showing all options |
| 2 | Verify 'Daily' option is present | 'Daily' is listed as an option |
| 3 | Verify 'Weekly' option is present | 'Weekly' is listed as an option |
| 4 | Verify 'Monthly' option is present | 'Monthly' is listed as an option |
| 5 | Verify 'Quarterly' option is present | 'Quarterly' is listed as an option |
| 6 | Verify 'Semi-Annually' option is present | 'Semi-Annually' is listed as an option |
| 7 | Verify 'Annually' option is present | 'Annually' is listed as an option |
| 8 | Select 'Quarterly' and verify calculation updates | 'Quarterly' is selected; results recalculate using quarterly compounding (n=4) |

---

### TC-12: Verify Time Unit Toggle Between Years and Months

**Priority**: High  
**Type**: Functional  
**Acceptance Criteria**: AC6

**Description**: Verify that time unit can be toggled between Years and Months and affects calculations

**Preconditions**: Calculator page is loaded with valid inputs

**Test Steps**:

| Step | Action | Expected Result |
|------|--------|-----------------|
| 1 | Enter P=1000, r=6%, monthly compounding, duration=24 with 'Months' selected | Time unit toggle shows 'Months' is active; calculation is for 24 months (2 years) |
| 2 | Note the Final Balance value | Final Balance calculated for 2 years is displayed |
| 3 | Toggle Time Unit to 'Years' (duration value remains 24) | Time unit toggle shows 'Years' is active; calculation recalculates for 24 years |
| 4 | Compare new Final Balance | Final Balance is significantly higher (24 years vs 2 years); results update within 300ms |
| 5 | Toggle back to 'Months' | Calculation reverts to 24 months; results update within 300ms |

---

### TC-13: Verify Responsive Layout - Desktop Two-Column

**Priority**: Medium  
**Type**: UI/Responsive  
**Acceptance Criteria**: AC10

**Description**: Verify that on desktop viewport, the layout displays in two-column format

**Preconditions**: Calculator page is loaded in a browser

**Test Steps**:

| Step | Action | Expected Result |
|------|--------|-----------------|
| 1 | Set browser window to desktop size (≥1024px width) | Browser is at desktop resolution |
| 2 | Verify layout structure | Page displays in two-column layout: inputs on left, results/chart on right (or similar side-by-side arrangement) |
| 3 | Verify inputs column | All input fields are visible and properly aligned in one column |
| 4 | Verify results column | Results, chart, and breakdown table toggle are visible in second column |

---

### TC-14: Verify Responsive Layout - Mobile Single-Column

**Priority**: Medium  
**Type**: UI/Responsive  
**Acceptance Criteria**: AC10

**Description**: Verify that on mobile viewport, the layout displays in single-column format

**Preconditions**: Calculator page is loaded in a browser or mobile device

**Test Steps**:

| Step | Action | Expected Result |
|------|--------|-----------------|
| 1 | Set browser window to mobile size (≤768px width) or use mobile device | Browser is at mobile resolution |
| 2 | Verify layout structure | Page displays in single-column layout: inputs stacked vertically, followed by results/chart below |
| 3 | Scroll through the page | All elements are accessible via vertical scrolling; no horizontal scroll is required |
| 4 | Verify chart responsiveness | Chart resizes to fit mobile width while maintaining readability |
| 5 | Verify input fields are usable on touch screens | All input fields and dropdowns are easily tappable and functional on mobile |

---

### TC-15: Edge Case - Decimal Input Values

**Priority**: Medium  
**Type**: Edge Case  
**Acceptance Criteria**: AC2

**Description**: Verify that decimal values are accepted and calculated correctly

**Preconditions**: Calculator page is loaded

**Test Steps**:

| Step | Action | Expected Result |
|------|--------|-----------------|
| 1 | Enter '1000.50' in Starting Amount field | Decimal value is accepted |
| 2 | Enter '5.25' in Interest Rate field | Decimal percentage is accepted |
| 3 | Enter '10.5' in Duration field with 'Years' selected | Decimal duration is accepted |
| 4 | Enter '50.75' in Monthly Contribution field | Decimal contribution is accepted |
| 5 | Verify calculation completes | Final Balance, Total Principal, and Total Interest are calculated and displayed with proper decimal precision |

---

### TC-16: Edge Case - Very Large Principal Amount

**Priority**: Low  
**Type**: Edge Case  
**Acceptance Criteria**: AC2

**Description**: Verify calculator handles large principal amounts without errors

**Preconditions**: Calculator page is loaded

**Test Steps**:

| Step | Action | Expected Result |
|------|--------|-----------------|
| 1 | Enter '10000000' (10 million) in Starting Amount field | Large value is accepted |
| 2 | Enter '5' in Interest Rate, select 'Annually', and enter '20' years | All inputs are accepted |
| 3 | Verify calculation completes without errors | Final Balance is calculated correctly (approximately $26.5 million); no JavaScript errors or display issues |
| 4 | Verify large numbers are formatted properly | Results display with proper number formatting (commas or localized formatting) |

---

### TC-17: Edge Case - Very High Interest Rate

**Priority**: Medium  
**Type**: Edge Case  
**Acceptance Criteria**: AC1

**Description**: Verify calculator handles maximum allowed interest rate (100%)

**Preconditions**: Calculator page is loaded

**Test Steps**:

| Step | Action | Expected Result |
|------|--------|-----------------|
| 1 | Enter '1000' in Starting Amount | Value is accepted |
| 2 | Enter '100' in Interest Rate field | Maximum rate (100%) is accepted without validation error |
| 3 | Select 'Annually' and enter '10' years | Inputs are accepted |
| 4 | Verify calculation completes | Extremely high Final Balance is calculated correctly using 100% annual rate |
| 5 | Change interest rate to '100.01' | Validation error appears: 'Must be less than or equal to 100' |

---

### TC-18: Edge Case - Minimum Valid Duration

**Priority**: Low  
**Type**: Edge Case  
**Acceptance Criteria**: AC2

**Description**: Verify calculator accepts very small but valid duration values

**Preconditions**: Calculator page is loaded

**Test Steps**:

| Step | Action | Expected Result |
|------|--------|-----------------|
| 1 | Enter valid Starting Amount (1000) and Interest Rate (5%) | Inputs are accepted |
| 2 | Select 'Monthly' compounding | Monthly is selected |
| 3 | Enter '1' in Duration field with 'Months' selected | Duration of 1 month is accepted (minimum valid value > 0) |
| 4 | Verify calculation completes | Final Balance is calculated for 1 month period and displayed correctly |
| 5 | Enter '0.5' in Duration field | Decimal duration (0.5 months) is either accepted and calculated, or validation error is shown based on business rules |

---

### TC-19: Different Compounding Frequencies Comparison

**Priority**: High  
**Type**: Functional  
**Acceptance Criteria**: AC2

**Description**: Verify that different compounding frequencies produce different results for same inputs

**Preconditions**: Calculator page is loaded

**Test Steps**:

| Step | Action | Expected Result |
|------|--------|-----------------|
| 1 | Enter P=1000, r=12%, duration=5 years with 'Annually' compounding | Results are calculated and displayed |
| 2 | Note the Final Balance for Annual compounding | Final Balance recorded (approx. $1,762) |
| 3 | Change compounding to 'Monthly' (keep all other inputs same) | Results recalculate within 300ms |
| 4 | Compare Final Balance for Monthly compounding | Final Balance is higher with monthly compounding (approx. $1,817) due to more frequent compounding |
| 5 | Change compounding to 'Daily' | Final Balance increases further (approx. $1,822) reflecting daily compounding effect |

---

### TC-20: Chart Updates When Inputs Change

**Priority**: Medium  
**Type**: Integration  
**Acceptance Criteria**: AC3, AC6

**Description**: Verify that the growth chart dynamically updates when any input value changes

**Preconditions**: Calculator page is loaded with valid inputs; chart is displayed

**Test Steps**:

| Step | Action | Expected Result |
|------|--------|-----------------|
| 1 | Enter P=1000, r=5%, annually, 10 years and observe initial chart | Chart displays growth curve for 10-year period |
| 2 | Change Duration from 10 to 20 years | Chart updates within 300ms to show 20-year period with extended growth curve |
| 3 | Change Interest Rate from 5% to 10% | Chart updates to show steeper growth curve reflecting higher interest rate |
| 4 | Add Monthly Contribution of $100 | Chart updates to show increased principal area growing over time with contributions |
| 5 | Verify chart axes and scale adjust appropriately | Y-axis scale adjusts to accommodate higher values; chart remains readable |

---

### TC-21: Breakdown Table Content Accuracy

**Priority**: Medium  
**Type**: Functional  
**Acceptance Criteria**: AC5, AC2

**Description**: Verify that breakdown table displays accurate period-by-period calculations

**Preconditions**: Calculator page is loaded with P=1000, r=10%, annually, 5 years

**Test Steps**:

| Step | Action | Expected Result |
|------|--------|-----------------|
| 1 | Expand the breakdown table | Table becomes visible showing year-by-year breakdown |
| 2 | Verify Year 0 (initial) row | Shows Period 0, Principal $1,000, Interest $0, Total $1,000 |
| 3 | Verify Year 1 row calculation | Shows Principal $1,000, Interest ~$100, Total ~$1,100 |
| 4 | Verify Year 5 (final) row matches summary results | Final row Total Balance matches Final Balance shown in summary (approx. $1,610.51) |
| 5 | Verify sum of Interest column | Sum of all interest entries matches Total Interest Earned in summary |
| 6 | Change an input value and verify table updates | All rows recalculate with new values; table remains expanded |

---

### TC-22: Multiple Sequential Input Changes (Debouncing Test)

**Priority**: Medium  
**Type**: Performance  
**Acceptance Criteria**: AC6

**Description**: Verify debouncing prevents excessive recalculations during rapid input

**Preconditions**: Calculator page is loaded with valid inputs

**Test Steps**:

| Step | Action | Expected Result |
|------|--------|-----------------|
| 1 | Rapidly type multiple characters in Starting Amount field (e.g., type '12345' quickly) | Calculation does not trigger for each keystroke; debouncing is applied |
| 2 | Stop typing and wait 300ms | After 300ms of no input, calculation triggers once with final value '12345' |
| 3 | Open browser developer console and monitor network/computation activity | Only one calculation occurs after typing stops, not multiple calculations during typing |
| 4 | Rapidly change compounding frequency dropdown multiple times | Only final selection triggers calculation after 300ms debounce |

---

### TC-23: Clear All Inputs and Verify Reset State

**Priority**: Low  
**Type**: Functional  
**Acceptance Criteria**: AC9

**Description**: Verify behavior when all inputs are cleared or reset

**Preconditions**: Calculator page is loaded with valid inputs and results displayed

**Test Steps**:

| Step | Action | Expected Result |
|------|--------|-----------------|
| 1 | Clear the Starting Amount field | Validation error appears; calculation is blocked |
| 2 | Verify results section | Results are hidden or show previous values but with clear indication that inputs are invalid |
| 3 | Clear Interest Rate field | Additional validation error appears on Interest Rate field |
| 4 | Clear Duration field | All three required fields show validation errors |
| 5 | Verify chart and table state | Chart either remains with previous data or displays placeholder; no errors occur |
| 6 | Re-enter valid values | Validation errors clear; calculation resumes; results update |

---

### TC-24: Browser Back/Forward Navigation

**Priority**: Medium  
**Type**: Integration  
**Acceptance Criteria**: AC6

**Description**: Verify calculator state and results persist or reset appropriately during navigation

**Preconditions**: Calculator page is loaded with valid inputs and results displayed

**Test Steps**:

| Step | Action | Expected Result |
|------|--------|-----------------|
| 1 | Enter valid inputs and note the Final Balance value | Results are calculated and displayed |
| 2 | Navigate to another page or URL | User leaves the calculator page |
| 3 | Click browser Back button to return to calculator | Calculator page loads; inputs and results may persist (if session/state is saved) or reset to empty (acceptable default behavior) |
| 4 | If inputs persisted, verify results match previous calculation | Results are consistent with input values |
| 5 | If inputs reset, verify no errors occur and page loads cleanly | Page loads in initial empty state with no validation errors until user enters data |

---

### TC-25: Chart Accessibility - Color Contrast and Labels

**Priority**: Low  
**Type**: Accessibility  
**Acceptance Criteria**: AC3

**Description**: Verify that chart colors have sufficient contrast and are properly labeled

**Preconditions**: Calculator page is loaded with valid inputs; chart is displayed

**Test Steps**:

| Step | Action | Expected Result |
|------|--------|-----------------|
| 1 | Verify principal and interest areas use distinct, contrasting colors | Colors are easily distinguishable (e.g., blue vs green, not light gray vs white) |
| 2 | Verify chart has a legend identifying each area | Legend clearly labels 'Principal' and 'Interest' with corresponding colors |
| 3 | Verify axis labels are present | X-axis shows time periods (Years/Months); Y-axis shows dollar amounts with $ symbol or label |
| 4 | Test with browser zoom at 200% | Chart remains readable and labels are not cut off or overlapping |

---

## Traceability Matrix

### Test Case Linking Summary

| Metric | Value |
|--------|-------|
| **Requirement ID** | 40309985 |
| **Requirement Name** | MJ-41 Compound Interest Calculator - Real-Time Calculation with Visual Results |
| **Total Test Cases Generated** | 25 |
| **Traceability Status** | ⚠️ Pending qTest IDs |

**Note**: Test case IDs from qTest are required to establish bidirectional traceability links. Once test cases are created in qTest, their IDs should be added to this section for complete requirement-to-test-case traceability.

### Test Coverage by Acceptance Criteria

| AC | Description | Test Cases | Count |
|----|-------------|------------|-------|
| **AC1** | Inline validation with red underline and helper text | TC-01, TC-02, TC-03, TC-04, TC-11, TC-17 | 6 |
| **AC2** | Display Final Balance, Total Principal, Total Interest | TC-05, TC-06, TC-15, TC-16, TC-18, TC-19, TC-21 | 7 |
| **AC3** | Stacked area chart with color-coded principal/interest | TC-08, TC-20, TC-25 | 3 |
| **AC4** | Chart tooltips show exact values on hover | TC-09 | 1 |
| **AC5** | Breakdown table hidden by default and toggleable | TC-10, TC-21 | 2 |
| **AC6** | Results update within 300ms of input changes | TC-07, TC-12, TC-20, TC-22, TC-24 | 5 |
| **AC7** | Validation: P=1000, r=5%, 10yr = $1,628.89 | TC-05 | 1 |
| **AC8** | Validation: P=1000, r=7%, 30yr, PMT=$100 = $121,997 | TC-06 | 1 |
| **AC9** | Invalid inputs show errors and block calculation | TC-01, TC-02, TC-03, TC-04, TC-23 | 5 |
| **AC10** | Responsive: two-column desktop, single-column mobile | TC-13, TC-14 | 2 |

### Coverage Visualization

```
Acceptance Criteria Coverage: 10/10 (100%)

AC1:  ██████████████████████ 6 tests
AC2:  ██████████████████████ 7 tests
AC3:  ██████████████████████ 3 tests
AC4:  ██████████████████████ 1 test
AC5:  ██████████████████████ 2 tests
AC6:  ██████████████████████ 5 tests
AC7:  ██████████████████████ 1 test
AC8:  ██████████████████████ 1 test
AC9:  ██████████████████████ 5 tests
AC10: ██████████████████████ 2 tests
```

---

## Document History

| Date | Version | Author | Description |
|------|---------|--------|-------------|
| 2026-05-26 | 3.0 | QA Pipeline Automation | Updated specification document with RQ-68 requirement details, comprehensive suitability assessment (Grade A, 96/100), 25 test cases with detailed steps, and complete coverage mapping |
| 2026-07-17 | 2.0 | QA Pipeline Automation | Updated with RQ-75 requirement details |
| 2026-05-26 | 1.0 | QA Pipeline Automation | Initial specification document created |

---

## Appendix A: Test Execution Guidance

### Pre-Execution Checklist

- [ ] Verify application is deployed and accessible
- [ ] Confirm test environment meets technical specifications (React, Recharts, Tailwind CSS)
- [ ] Ensure browser compatibility (recommend testing on Chrome, Firefox, Safari, Edge)
- [ ] Prepare test data and validation tools (calculator for expected values)
- [ ] Review acceptance criteria and expected outcomes
- [ ] Verify developer tools are available for performance testing (TC-07, TC-22)

### Recommended Execution Order

#### Phase 1: Critical Path (High Priority)
Execute these tests first to validate core functionality:

1. **TC-05** - Validation Test: Base Scenario (AC7)
2. **TC-06** - Validation Test: With Monthly Contributions (AC8)
3. **TC-01, TC-02, TC-03** - Required field validation
4. **TC-07** - Performance testing: Real-time updates
5. **TC-08** - Chart rendering
6. **TC-09** - Chart tooltips
7. **TC-11** - Compounding frequency dropdown
8. **TC-12** - Time unit toggle
9. **TC-19** - Compounding frequencies comparison

#### Phase 2: Comprehensive Coverage (Medium Priority)
Execute these tests for complete feature validation:

1. **TC-04** - Optional field validation
2. **TC-10** - Breakdown table toggle
3. **TC-13, TC-14** - Responsive layout
4. **TC-15, TC-17** - Edge cases with decimal and high values
5. **TC-20** - Chart updates
6. **TC-21** - Breakdown table accuracy
7. **TC-22** - Debouncing test
8. **TC-24** - Browser navigation

#### Phase 3: Edge Cases & Accessibility (Low Priority)
Execute these tests for comprehensive coverage:

1. **TC-16** - Very large principal amounts
2. **TC-18** - Minimum duration
3. **TC-23** - Reset state
4. **TC-25** - Chart accessibility

### Success Criteria

- **100% Pass Rate Required** for AC7 and AC8 validation tests (TC-05, TC-06)
- **95% Pass Rate Minimum** for all High priority test cases
- **Zero Critical Defects** in input validation and calculation accuracy
- **Performance Standard**: Real-time calculations must complete within 300ms (TC-07)
- **Responsive Design**: Both desktop and mobile layouts must render correctly (TC-13, TC-14)

### Defect Severity Guidelines

| Severity | Description | Examples |
|----------|-------------|----------|
| **Critical** | Feature is completely broken or produces incorrect financial calculations | Calculation formula wrong, app crashes, AC7/AC8 validation fails |
| **High** | Core functionality is impaired but workarounds exist | Validation not working, chart not rendering, performance > 500ms |
| **Medium** | Feature works but has usability issues | Tooltips missing, table doesn't collapse, minor formatting issues |
| **Low** | Cosmetic issues that don't affect functionality | Color scheme variations, minor text alignment |

---

## Appendix B: Calculation Verification Reference

### Test Data Reference Table

For manual verification of calculations:

| Scenario | Principal | Rate | Compound | Duration | Contribution | Expected Final Balance |
|----------|-----------|------|----------|----------|--------------|----------------------|
| **AC7 Validation** | $1,000 | 5% | Annually | 10 years | $0 | ~$1,628.89 |
| **AC8 Validation** | $1,000 | 7% | Monthly | 30 years | $100/month | ~$121,997 |
| **TC-19 Annual** | $1,000 | 12% | Annually | 5 years | $0 | ~$1,762 |
| **TC-19 Monthly** | $1,000 | 12% | Monthly | 5 years | $0 | ~$1,817 |
| **TC-19 Daily** | $1,000 | 12% | Daily | 5 years | $0 | ~$1,822 |

### Formula Validation

The compound interest formula used is:

```
A = P(1 + r/n)^(nt) + PMT × [((1 + r/n)^(nt) - 1) / (r/n)]
```

**Example calculation for AC7:**
- P = 1000
- r = 0.05 (5% as decimal)
- n = 1 (annually)
- t = 10 years
- PMT = 0

```
A = 1000(1 + 0.05/1)^(1×10) + 0
A = 1000(1.05)^10
A = 1000 × 1.62889...
A ≈ $1,628.89
```

**Example calculation for AC8:**
- P = 1000
- r = 0.07
- n = 12 (monthly)
- t = 30 years
- PMT = 100

```
A = 1000(1 + 0.07/12)^(12×30) + 100 × [((1 + 0.07/12)^(12×30) - 1) / (0.07/12)]
A ≈ $121,997
```

---

**End of Specification Document**

*This document serves as the single source of truth for requirement RQ-68 (ID: 40309985) and its associated test cases. All stakeholders should refer to this document for requirement details, test case specifications, and traceability information.*