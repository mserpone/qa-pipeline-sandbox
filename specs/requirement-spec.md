# Specification Document: Compound Interest Calculator

## Document Metadata

| Property | Value |
|----------|-------|
| **Generated** | 2026-07-17 |
| **Repository** | mserpone/qa-pipeline-sandbox |
| **Branch** | main |
| **Document Version** | 2.0 |

---

## Requirement Overview

### Identification

| Property | Value |
|----------|-------|
| **Requirement ID** | 41262552 |
| **Project ID** | 127305 |
| **PID** | RQ-75 |
| **Name** | MJ-43 Compound Interest Calculator - Real-Time Calculation with Visual Results |
| **Status** | New |
| **Priority** | Must have |
| **Type** | Functional |
| **Parent ID** | 41090557 |

### Key Dates

| Event | Date |
|-------|------|
| **Created** | 2026-07-17T10:50:38-04:00 |
| **Last Modified** | 2026-07-17T10:50:48-04:00 |

### References

- **qTest URL**: [View in qTest](https://sademo.qtestnet.com/p/127305/portal/project#tab=requirements&object=5&id=41262552)
- **GitHub Specification**: [compound-interest-calculator.md](https://github.com/mserpone/qa-pipeline-sandbox/blob/main/specs/compound-interest-calculator.md)

---

## Requirement Description

### User Story

As a personal finance user, I want to input investment details and instantly see compound interest projections with a visual breakdown, so that I can make informed decisions about my savings and investments without needing financial expertise.

### Functional Requirements

#### Input Fields

| Field | Type | Validation | Required |
|-------|------|------------|----------|
| **Starting Amount ($)** | Number | Must be > 0 | Yes |
| **Interest Rate (%)** | Number | Must be > 0 and ≤ 100 | Yes |
| **Compounded** | Dropdown | Daily, Weekly, Monthly, Quarterly, Semi-Annually, Annually | Yes |
| **Duration** | Number | Must be > 0 | Yes |
| **Time Unit** | Toggle | Years or Months | Yes |
| **Monthly Contribution ($)** | Number | Must be ≥ 0 | No |

#### Output Components

1. **Final Balance** - Total value at end of investment period
2. **Total Principal** - Sum of initial deposit plus all contributions
3. **Total Interest Earned** - Final balance minus total principal
4. **Growth Chart** - Stacked area chart showing principal vs. interest over time
5. **Breakdown Table** - Expandable year-by-year period breakdown (hidden by default)

#### Calculation Formula

```
A = P(1 + r/n)^(nt) + PMT × [((1 + r/n)^(nt) - 1) / (r/n)]
```

Where:
- **A** = Final amount
- **P** = Principal (starting amount)
- **r** = Annual interest rate (decimal)
- **n** = Number of times interest is compounded per year
- **t** = Time in years
- **PMT** = Monthly contribution amount

#### Real-Time Behavior

- Results and chart must update instantly as any input changes (no submit button)
- Input changes must be debounced by 300ms to avoid excessive recalculation

### Acceptance Criteria

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

### Technical Specifications

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

### Overall Grade: **A (98/100)**

**Assessment**: SUITABLE  
**Confidence Score**: 0.95

This requirement is **HIGHLY SUITABLE** for test case generation. It demonstrates exceptional quality with comprehensive functional specifications, explicit validation test cases with exact expected results, detailed technical implementation guidance, and clear acceptance criteria.

### Scoring Breakdown

| Criterion | Score | Maximum | Notes |
|-----------|-------|---------|-------|
| **Completeness** | 24 | 25 | Exceptionally complete with user story, functional requirements, acceptance criteria, technical notes, and specific test cases. Edge cases and error scenarios are well-documented. Only minor gap: maximum input limits not explicitly defined. |
| **Clarity** | 25 | 25 | Unambiguous language throughout. Technical terms are clearly defined. Mathematical formula is explicit. Behavior specifications (debouncing, validation, real-time updates) are precise. Scope is well-bounded with no room for misinterpretation. |
| **Testability** | 25 | 25 | Highly testable with 10 specific acceptance criteria. Includes concrete test cases with expected outputs (e.g., P=1000, r=5%, 10yr = $1,628.89). Performance metrics are measurable (300ms debounce). Visual elements have clear pass/fail conditions. |
| **Detail** | 24 | 25 | Excellent detail across all dimensions. Input/output specifications are comprehensive. User workflows are clear. Error handling is well-defined. UI/UX behavior is specific (tooltips, responsive breakpoints, expandable tables). Minor gap: decimal precision/rounding rules not specified. |
| **TOTAL** | **98** | **100** | |

### Identified Test Areas

The following 12 test areas have been identified for comprehensive coverage:

1. **Input field validation** (required fields, data types, range constraints)
2. **Calculation accuracy** across multiple scenarios
3. **Real-time update behavior** and debouncing
4. **Growth chart rendering** and data visualization
5. **Chart tooltips** and hover interactions
6. **Breakdown table toggle** functionality
7. **Responsive layout** (desktop vs mobile)
8. **Error state display** (inline validation with red underline)
9. **Edge cases** (zero values, negative inputs, boundary conditions)
10. **Performance** (calculation speed under 300ms)
11. **Currency formatting** and display
12. **Compounding frequency variations** (daily, monthly, annually, etc.)

### Missing Elements

While the requirement is excellent, the following elements could enhance completeness:

1. Accessibility requirements (WCAG compliance, keyboard navigation, screen reader support)
2. Browser compatibility matrix (Chrome, Firefox, Safari, Edge versions)
3. Maximum input limits (upper bounds for principal, rate, duration)
4. Decimal precision and rounding rules for currency display
5. Internationalization considerations (currency symbols, number formats)
6. Fallback behavior if JavaScript is disabled or chart library fails to load

### Recommendations

1. **Add accessibility acceptance criteria**: Keyboard navigation, ARIA labels, focus management, and screen reader testing
2. **Specify supported browsers and versions** for cross-browser testing
3. **Define maximum input constraints** (e.g., principal ≤ $1,000,000,000, duration ≤ 100 years)
4. **Clarify decimal precision rules** (e.g., round to 2 decimal places for currency display)
5. **Consider adding loading states** if calculation becomes complex with large durations
6. **Document internationalization strategy** if the app will support multiple regions

### Summary

This is an exceptionally well-crafted requirement that is immediately ready for test case generation. It provides comprehensive functional specifications, clear acceptance criteria with concrete test cases, explicit validation rules, and detailed UI/UX behavior. The inclusion of a mathematical formula, specific expected outputs, and performance requirements makes it highly testable. Test engineers can confidently create a full test suite covering functional, UI, performance, and negative test scenarios.

---

## Generated Test Cases

### Coverage Summary

| Metric | Value |
|--------|-------|
| **Total Test Cases** | 24 |
| **Acceptance Criteria Covered** | 10 out of 10 |
| **Coverage Percentage** | 100% |

### Test Case Distribution by Priority

| Priority | Count | Test Cases |
|----------|-------|------------|
| **High** | 11 | TC-01 through TC-05, TC-07, TC-09, TC-12, TC-13, TC-19 |
| **Medium** | 10 | TC-04, TC-06, TC-08, TC-10, TC-11, TC-14, TC-15, TC-17, TC-18, TC-21 |
| **Low** | 3 | TC-16, TC-22, TC-23 |

### Test Case Distribution by Type

| Type | Count | Test Cases |
|------|-------|------------|
| **Functional** | 22 | TC-01 through TC-11, TC-13 through TC-24 |
| **Performance** | 1 | TC-12 |
| **Edge Cases** | 3 | TC-16, TC-22, TC-23 |

---

## Test Cases

### TC-01: Validate Required Input Field - Starting Amount

**Priority**: High  
**Type**: Functional  
**Acceptance Criteria**: AC1, AC9

**Description**: Verify inline validation for Starting Amount field when empty or invalid

**Preconditions**: Calculator page is loaded and all fields are in default state

**Test Steps**:

| Step | Action | Expected Result |
|------|--------|-----------------|
| 1 | Leave Starting Amount field empty and click on another field | Red underline appears under Starting Amount field with helper text 'Starting Amount is required' |
| 2 | Enter '0' in Starting Amount field | Red underline appears with helper text 'Starting Amount must be greater than 0' |
| 3 | Enter '-500' in Starting Amount field | Red underline appears with helper text 'Starting Amount must be greater than 0' |
| 4 | Enter '1000' in Starting Amount field | Validation error clears, field appears normal, calculation proceeds |

---

### TC-02: Validate Required Input Field - Interest Rate

**Priority**: High  
**Type**: Functional  
**Acceptance Criteria**: AC1, AC9

**Description**: Verify inline validation for Interest Rate field with boundary conditions

**Preconditions**: Calculator page is loaded

**Test Steps**:

| Step | Action | Expected Result |
|------|--------|-----------------|
| 1 | Leave Interest Rate field empty and click on another field | Red underline appears with helper text 'Interest Rate is required' |
| 2 | Enter '-5' in Interest Rate field | Red underline appears with helper text 'Interest Rate must be greater than 0' |
| 3 | Enter '0' in Interest Rate field | Red underline appears with helper text 'Interest Rate must be greater than 0' |
| 4 | Enter '101' in Interest Rate field | Red underline appears with helper text 'Interest Rate must be less than or equal to 100' |
| 5 | Enter '100' in Interest Rate field | Validation passes, calculation proceeds with 100% interest rate |

---

### TC-03: Validate Required Input Field - Duration

**Priority**: High  
**Type**: Functional  
**Acceptance Criteria**: AC1, AC9

**Description**: Verify inline validation for Duration field

**Preconditions**: Calculator page is loaded

**Test Steps**:

| Step | Action | Expected Result |
|------|--------|-----------------|
| 1 | Leave Duration field empty and click on another field | Red underline appears with helper text 'Duration is required' |
| 2 | Enter '0' in Duration field | Red underline appears with helper text 'Duration must be greater than 0' |
| 3 | Enter '-10' in Duration field | Red underline appears with helper text 'Duration must be greater than 0' |
| 4 | Enter '10' in Duration field | Validation passes, calculation proceeds |

---

### TC-04: Validate Optional Input Field - Monthly Contribution

**Priority**: Medium  
**Type**: Functional  
**Acceptance Criteria**: AC1

**Description**: Verify validation for optional Monthly Contribution field

**Preconditions**: Calculator page is loaded with all required fields populated

**Test Steps**:

| Step | Action | Expected Result |
|------|--------|-----------------|
| 1 | Leave Monthly Contribution field empty | No validation error, calculation proceeds treating contribution as 0 |
| 2 | Enter '-100' in Monthly Contribution field | Red underline appears with helper text 'Monthly Contribution must be greater than or equal to 0' |
| 3 | Enter '0' in Monthly Contribution field | Validation passes, calculation proceeds with no contribution |
| 4 | Enter '100' in Monthly Contribution field | Validation passes, calculation includes monthly contributions |

---

### TC-05: Verify Calculation Accuracy - Base Scenario

**Priority**: High  
**Type**: Functional  
**Acceptance Criteria**: AC2, AC7

**Description**: Validate correct calculation for basic compound interest without contributions

**Preconditions**: Calculator page is loaded

**Test Steps**:

| Step | Action | Expected Result |
|------|--------|-----------------|
| 1 | Enter Starting Amount: '1000' | Value is accepted |
| 2 | Enter Interest Rate: '5' | Value is accepted |
| 3 | Select Compounded: 'Annually' | Value is selected |
| 4 | Enter Duration: '10' with Time Unit: 'Years' | Value is accepted |
| 5 | Verify displayed results | Final Balance: ~$1,628.89, Total Principal: $1,000.00, Total Interest Earned: ~$628.89 |

---

### TC-06: Verify Calculation Accuracy - With Monthly Contributions

**Priority**: High  
**Type**: Functional  
**Acceptance Criteria**: AC2, AC8

**Description**: Validate correct calculation with recurring monthly contributions

**Preconditions**: Calculator page is loaded

**Test Steps**:

| Step | Action | Expected Result |
|------|--------|-----------------|
| 1 | Enter Starting Amount: '1000' | Value is accepted |
| 2 | Enter Interest Rate: '7' | Value is accepted |
| 3 | Select Compounded: 'Monthly' | Value is selected |
| 4 | Enter Duration: '30' with Time Unit: 'Years' | Value is accepted |
| 5 | Enter Monthly Contribution: '100' | Value is accepted |
| 6 | Verify displayed results | Final Balance: ~$121,997.00, Total Principal: $37,000.00 (1000 + 100×12×30), Total Interest Earned: ~$84,997.00 |

---

### TC-07: Verify All Compounding Frequencies

**Priority**: High  
**Type**: Functional  
**Acceptance Criteria**: AC2

**Description**: Test calculation with each compounding frequency option

**Preconditions**: Calculator page is loaded

**Test Steps**:

| Step | Action | Expected Result |
|------|--------|-----------------|
| 1 | Enter Starting Amount: '5000', Interest Rate: '6', Duration: '5 Years' | Values are accepted |
| 2 | Select Compounded: 'Daily' and verify calculation | Final Balance displays correctly with daily compounding (n=365) |
| 3 | Select Compounded: 'Weekly' and verify calculation | Final Balance displays correctly with weekly compounding (n=52) |
| 4 | Select Compounded: 'Monthly' and verify calculation | Final Balance displays correctly with monthly compounding (n=12) |
| 5 | Select Compounded: 'Quarterly' and verify calculation | Final Balance displays correctly with quarterly compounding (n=4) |
| 6 | Select Compounded: 'Semi-Annually' and verify calculation | Final Balance displays correctly with semi-annual compounding (n=2) |
| 7 | Select Compounded: 'Annually' and verify calculation | Final Balance displays correctly with annual compounding (n=1) |

---

### TC-08: Verify Time Unit Toggle - Years vs Months

**Priority**: Medium  
**Type**: Functional  
**Acceptance Criteria**: AC2, AC6

**Description**: Validate calculation adjusts correctly when toggling between Years and Months

**Preconditions**: Calculator page is loaded

**Test Steps**:

| Step | Action | Expected Result |
|------|--------|-----------------|
| 1 | Enter Starting Amount: '2000', Interest Rate: '4', Duration: '3', Time Unit: 'Years' | Calculation displays for 3 years duration |
| 2 | Toggle Time Unit to 'Months' (keeping Duration as '3') | Results update instantly to show calculation for 3 months instead of 3 years; Final Balance is significantly lower |
| 3 | Change Duration to '36' with Time Unit: 'Months' | Results match the original 3-year calculation |

---

### TC-09: Verify Growth Chart Rendering

**Priority**: High  
**Type**: Functional  
**Acceptance Criteria**: AC3

**Description**: Validate that the stacked area chart renders correctly with proper data visualization

**Preconditions**: Calculator page is loaded with valid inputs entered

**Test Steps**:

| Step | Action | Expected Result |
|------|--------|-----------------|
| 1 | Enter Starting Amount: '1000', Interest Rate: '5', Compounded: 'Annually', Duration: '10 Years' | Input values are accepted |
| 2 | Observe the Growth Chart section | A stacked area chart is displayed showing growth over 10 years |
| 3 | Verify chart has two distinct areas with different colors | Principal amount is shown in one color (bottom layer), interest earned in another color (top layer) |
| 4 | Verify X-axis shows time progression | X-axis displays years from 0 to 10 |
| 5 | Verify Y-axis shows monetary values | Y-axis displays dollar amounts from $0 to approximately $1,629 |

---

### TC-10: Verify Chart Tooltips on Hover

**Priority**: Medium  
**Type**: Functional  
**Acceptance Criteria**: AC4

**Description**: Validate that hovering over the chart displays tooltips with exact values

**Preconditions**: Calculator is loaded with valid inputs and chart is rendered

**Test Steps**:

| Step | Action | Expected Result |
|------|--------|-----------------|
| 1 | Hover mouse over the chart at Year 1 position | Tooltip appears showing Year 1, Principal value, and Interest value with exact dollar amounts |
| 2 | Move mouse to Year 5 position on chart | Tooltip updates and displays Year 5 values accurately |
| 3 | Move mouse to final year position on chart | Tooltip displays final year values matching the Final Balance summary |
| 4 | Move mouse away from chart | Tooltip disappears |

---

### TC-11: Verify Breakdown Table Toggle Functionality

**Priority**: Medium  
**Type**: Functional  
**Acceptance Criteria**: AC5

**Description**: Validate that breakdown table is hidden by default and can be expanded/collapsed

**Preconditions**: Calculator is loaded with valid inputs and results are displayed

**Test Steps**:

| Step | Action | Expected Result |
|------|--------|-----------------|
| 1 | Observe the breakdown table section on page load | Breakdown table is hidden/collapsed by default, only a toggle button/link is visible |
| 2 | Click the toggle button to expand breakdown table | Table expands and displays year-by-year (or period-by-period) breakdown showing: Period, Starting Balance, Contributions, Interest Earned, Ending Balance |
| 3 | Verify table contains correct number of rows | If duration is 10 years, table shows 10 rows with accurate calculations for each period |
| 4 | Click the toggle button again | Table collapses and becomes hidden again |

---

### TC-12: Verify Real-Time Update with Debouncing

**Priority**: High  
**Type**: Performance  
**Acceptance Criteria**: AC6

**Description**: Validate that results update in real-time within 300ms after input changes

**Preconditions**: Calculator is loaded with valid inputs and results are displayed

**Test Steps**:

| Step | Action | Expected Result |
|------|--------|-----------------|
| 1 | Note the current Final Balance value | Current value is recorded |
| 2 | Change Starting Amount value (e.g., from 1000 to 2000) | Within 300ms, Final Balance, Total Principal, Total Interest, and Growth Chart all update to reflect new calculation |
| 3 | Rapidly type multiple characters in Interest Rate field | Calculation waits until typing pauses for 300ms before updating (debouncing works correctly) |
| 4 | Change Compounded dropdown value | Results update immediately within 300ms |
| 5 | Use browser developer tools to measure update time | Update time from input change to result display is under 300ms |

---

### TC-13: Verify Calculation Blocked with Invalid Inputs

**Priority**: High  
**Type**: Functional  
**Acceptance Criteria**: AC9

**Description**: Ensure calculation does not proceed when invalid inputs are present

**Preconditions**: Calculator page is loaded

**Test Steps**:

| Step | Action | Expected Result |
|------|--------|-----------------|
| 1 | Enter Starting Amount: '-500' (invalid) | Validation error is displayed |
| 2 | Enter other valid inputs: Interest Rate: '5', Compounded: 'Annually', Duration: '10 Years' | Other fields accept input but calculation does not execute |
| 3 | Verify results section | Final Balance, Total Principal, and Total Interest Earned are not displayed OR show placeholder/empty state |
| 4 | Verify chart section | Growth chart is not rendered or shows empty/placeholder state |
| 5 | Correct Starting Amount to '1000' | Validation clears and calculation proceeds, displaying all results |

---

### TC-14: Verify Responsive Layout - Desktop View

**Priority**: Medium  
**Type**: Functional  
**Acceptance Criteria**: AC10

**Description**: Validate that the calculator displays in two-column layout on desktop screens

**Preconditions**: Calculator page is loaded on a desktop browser

**Test Steps**:

| Step | Action | Expected Result |
|------|--------|-----------------|
| 1 | Open calculator on desktop browser with viewport width ≥ 1024px | Page loads successfully |
| 2 | Observe the layout structure | Layout displays in two columns: Input fields on left, Results/Chart on right |
| 3 | Verify all elements are properly aligned and readable | No overflow, text is readable, charts are properly sized |

---

### TC-15: Verify Responsive Layout - Mobile View

**Priority**: Medium  
**Type**: Functional  
**Acceptance Criteria**: AC10

**Description**: Validate that the calculator displays in single-column layout on mobile screens

**Preconditions**: Calculator page is accessed on mobile device or browser with mobile viewport

**Test Steps**:

| Step | Action | Expected Result |
|------|--------|-----------------|
| 1 | Open calculator on mobile device or set browser viewport to 375px width | Page loads successfully |
| 2 | Observe the layout structure | Layout displays in single column: Input fields stacked above Results/Chart section |
| 3 | Scroll through the page | All elements are accessible, no horizontal scrolling required, touch targets are appropriately sized |
| 4 | Verify chart responsiveness | Growth chart scales to fit mobile screen width without distortion |

---

### TC-16: Verify Non-Numeric Input Handling

**Priority**: Medium  
**Type**: Functional  
**Acceptance Criteria**: AC1, AC9

**Description**: Test how calculator handles non-numeric input in numeric fields

**Preconditions**: Calculator page is loaded

**Test Steps**:

| Step | Action | Expected Result |
|------|--------|-----------------|
| 1 | Enter 'abc' in Starting Amount field | Either: a) Characters are blocked from entry, or b) Validation error displays 'Starting Amount must be a number' |
| 2 | Enter special characters '!@#$' in Interest Rate field | Either: a) Characters are blocked from entry, or b) Validation error displays 'Interest Rate must be a number' |
| 3 | Enter 'ten' in Duration field | Either: a) Characters are blocked from entry, or b) Validation error displays 'Duration must be a number' |

---

### TC-17: Verify Decimal Input Handling

**Priority**: Medium  
**Type**: Functional  
**Acceptance Criteria**: AC2

**Description**: Test calculator behavior with decimal/fractional inputs

**Preconditions**: Calculator page is loaded

**Test Steps**:

| Step | Action | Expected Result |
|------|--------|-----------------|
| 1 | Enter Starting Amount: '1500.50' | Decimal value is accepted and calculation proceeds |
| 2 | Enter Interest Rate: '5.75' | Decimal percentage is accepted and calculation is accurate |
| 3 | Enter Monthly Contribution: '99.99' | Decimal contribution is accepted and included in calculation |
| 4 | Verify Final Balance calculation | Results are calculated correctly using decimal inputs, displayed with appropriate precision |

---

### TC-18: Verify Large Value Input Handling

**Priority**: Medium  
**Type**: Functional  
**Acceptance Criteria**: AC2

**Description**: Test calculator with very large input values

**Preconditions**: Calculator page is loaded

**Test Steps**:

| Step | Action | Expected Result |
|------|--------|-----------------|
| 1 | Enter Starting Amount: '10000000' (10 million) | Value is accepted |
| 2 | Enter Interest Rate: '8', Compounded: 'Annually', Duration: '50 Years' | Values are accepted |
| 3 | Verify calculation completes successfully | Results display correctly with proper currency formatting (commas, decimal points). No overflow errors or UI breaking. |
| 4 | Verify chart renders with large values | Y-axis scales appropriately, chart remains readable |

---

### TC-19: Verify Chart Updates When Inputs Change

**Priority**: High  
**Type**: Functional  
**Acceptance Criteria**: AC3, AC6

**Description**: Validate that the growth chart updates dynamically with input changes

**Preconditions**: Calculator is loaded with valid inputs and chart is displayed

**Test Steps**:

| Step | Action | Expected Result |
|------|--------|-----------------|
| 1 | Enter Starting Amount: '1000', Interest Rate: '5', Duration: '5 Years' and observe chart | Chart displays 5-year growth projection |
| 2 | Change Duration to '20 Years' | Chart updates within 300ms to display 20-year projection with more data points on X-axis |
| 3 | Change Interest Rate from '5' to '10' | Chart updates showing steeper interest growth curve (larger top area) |
| 4 | Add Monthly Contribution: '200' | Chart updates showing increased principal layer (bottom area grows linearly) and adjusted interest layer |

---

### TC-20: Verify Breakdown Table Content Accuracy

**Priority**: Medium  
**Type**: Functional  
**Acceptance Criteria**: AC5

**Description**: Validate that the breakdown table shows accurate period-by-period calculations

**Preconditions**: Calculator is loaded with valid inputs

**Test Steps**:

| Step | Action | Expected Result |
|------|--------|-----------------|
| 1 | Enter Starting Amount: '1000', Interest Rate: '5', Compounded: 'Annually', Duration: '3 Years', expand breakdown table | Table displays 3 rows for 3 years |
| 2 | Verify Year 1 row values | Starting Balance: $1,000.00, Interest: ~$50.00, Ending Balance: ~$1,050.00 |
| 3 | Verify Year 2 row values | Starting Balance: ~$1,050.00, Interest: ~$52.50, Ending Balance: ~$1,102.50 |
| 4 | Verify Year 3 row values | Starting Balance: ~$1,102.50, Interest: ~$55.13, Ending Balance: ~$1,157.63 |
| 5 | Verify final row matches Final Balance summary | Year 3 Ending Balance matches the Final Balance displayed in summary section |

---

### TC-21: Verify Breakdown Table with Monthly Contributions

**Priority**: Medium  
**Type**: Functional  
**Acceptance Criteria**: AC5

**Description**: Validate breakdown table accuracy when monthly contributions are included

**Preconditions**: Calculator is loaded

**Test Steps**:

| Step | Action | Expected Result |
|------|--------|-----------------|
| 1 | Enter Starting Amount: '1000', Interest Rate: '6', Compounded: 'Monthly', Duration: '2 Years', Monthly Contribution: '50' | Values are accepted |
| 2 | Expand breakdown table | Table displays period breakdown (either 24 monthly rows or 2 annual summary rows depending on implementation) |
| 3 | Verify each period row includes contribution column | Each period shows: Starting Balance, Contribution amount, Interest Earned, Ending Balance |
| 4 | Verify total contributions sum correctly | Sum of all contributions equals Total Principal minus Starting Amount (2 years × 12 months × $50 = $1,200 in contributions) |

---

### TC-22: Verify Edge Case - Zero Interest Rate

**Priority**: Low  
**Type**: Functional  
**Acceptance Criteria**: AC2

**Description**: Test calculation behavior with 0.01% interest rate (minimum valid value)

**Preconditions**: Calculator page is loaded

**Test Steps**:

| Step | Action | Expected Result |
|------|--------|-----------------|
| 1 | Enter Starting Amount: '5000', Interest Rate: '0.01', Compounded: 'Annually', Duration: '10 Years' | Values are accepted (0.01 is greater than 0) |
| 2 | Verify calculation results | Final Balance is slightly higher than principal, Total Interest Earned is minimal but non-zero |
| 3 | Verify chart displays | Chart shows very flat interest growth line, principal layer dominates visualization |

---

### TC-23: Verify Edge Case - Very Short Duration

**Priority**: Low  
**Type**: Functional  
**Acceptance Criteria**: AC2

**Description**: Test calculation with minimum duration of 1 month

**Preconditions**: Calculator page is loaded

**Test Steps**:

| Step | Action | Expected Result |
|------|--------|-----------------|
| 1 | Enter Starting Amount: '1000', Interest Rate: '12', Compounded: 'Monthly', Duration: '1', Time Unit: 'Months' | Values are accepted |
| 2 | Verify calculation results | Final Balance shows ~$1,010 (approximately 1% monthly interest), calculation is accurate for 1-month period |
| 3 | Verify chart renders | Chart displays even with minimal time span, showing start and end points |

---

### TC-24: Verify Currency Formatting

**Priority**: Medium  
**Type**: Functional  
**Acceptance Criteria**: AC2

**Description**: Validate that all monetary values are formatted correctly with currency symbols and decimal places

**Preconditions**: Calculator is loaded with valid inputs and results are displayed

**Test Steps**:

| Step | Action | Expected Result |
|------|--------|-----------------|
| 1 | Enter Starting Amount: '12345.67' and complete other inputs | Results display properly formatted |
| 2 | Verify Final Balance formatting | Displays with dollar sign ($), comma thousands separator, and 2 decimal places (e.g., $15,432.89) |
| 3 | Verify Total Principal formatting | Displays with dollar sign ($), comma thousands separator, and 2 decimal places |
| 4 | Verify Total Interest Earned formatting | Displays with dollar sign ($), comma thousands separator, and 2 decimal places |
| 5 | Verify breakdown table monetary values | All monetary columns use consistent currency formatting |

---

## Traceability Matrix

### Test Case Linking Summary

| Metric | Value |
|--------|-------|
| **Requirement ID** | 41262552 |
| **Requirement Name** | MJ-43 Compound Interest Calculator - Real-Time Calculation with Visual Results |
| **Total Test Cases Generated** | 24 |
| **Successfully Linked** | 24 |
| **Failed to Link** | 0 |
| **Link Type** | `is_covered_by` (bidirectional) |
| **Traceability Status** | ✅ Complete |

### Successfully Linked Test Case IDs

```
105391512, 105391513, 105391514, 105391515, 105391516, 105391517, 
105391518, 105391520, 105391519, 105391522, 105391521, 105391523, 
105391524, 105391525, 105391526, 105391527, 105391528, 105391529, 
105391530, 105391531, 105391532, 105391533, 105391534, 105391535
```

### Test Coverage by Acceptance Criteria

| AC | Description | Test Cases | Count |
|----|-------------|------------|-------|
| **AC1** | Inline validation with red underline and helper text | TC-01, TC-02, TC-03, TC-04, TC-16 | 5 |
| **AC2** | Display Final Balance, Total Principal, Total Interest | TC-05, TC-06, TC-07, TC-08, TC-17, TC-18, TC-22, TC-23, TC-24 | 9 |
| **AC3** | Stacked area chart with color-coded principal/interest | TC-09, TC-19 | 2 |
| **AC4** | Chart tooltips show exact values on hover | TC-10 | 1 |
| **AC5** | Breakdown table hidden by default and toggleable | TC-11, TC-20, TC-21 | 3 |
| **AC6** | Results update within 300ms of input changes | TC-08, TC-12, TC-19 | 3 |
| **AC7** | Validation: P=1000, r=5%, 10yr = $1,628.89 | TC-05 | 1 |
| **AC8** | Validation: P=1000, r=7%, 30yr, PMT=$100 = $121,997 | TC-06 | 1 |
| **AC9** | Invalid inputs show errors and block calculation | TC-01, TC-02, TC-03, TC-13, TC-16 | 5 |
| **AC10** | Responsive: two-column desktop, single-column mobile | TC-14, TC-15 | 2 |

### Coverage Visualization

```
Acceptance Criteria Coverage: 10/10 (100%)

AC1: ████████████████████ 5 tests
AC2: ████████████████████ 9 tests
AC3: ████████████████████ 2 tests
AC4: ████████████████████ 1 test
AC5: ████████████████████ 3 tests
AC6: ████████████████████ 3 tests
AC7: ████████████████████ 1 test
AC8: ████████████████████ 1 test
AC9: ████████████████████ 5 tests
AC10: ████████████████████ 2 tests
```

---

## Document History

| Date | Version | Author | Description |
|------|---------|--------|-------------|
| 2026-07-17 | 2.0 | QA Pipeline Automation | Updated specification document with RQ-75 requirement details, comprehensive suitability assessment (Grade A, 98/100), 24 test cases with full traceability, and complete coverage mapping |
| 2026-05-26 | 1.0 | QA Pipeline Automation | Initial specification document created |

---

## Appendix A: Test Execution Guidance

### Pre-Execution Checklist

- [ ] Verify application is deployed and accessible
- [ ] Confirm test environment meets technical specifications (React, Recharts, Tailwind CSS)
- [ ] Ensure browser compatibility (recommend testing on Chrome, Firefox, Safari, Edge)
- [ ] Prepare test data and validation tools (calculator for expected values)
- [ ] Review acceptance criteria and expected outcomes
- [ ] Verify developer tools are available for performance testing (TC-12)

### Recommended Execution Order

#### Phase 1: Critical Path (High Priority)
Execute these tests first to validate core functionality:

1. **TC-05** - Validation Test: Base Scenario (AC7)
2. **TC-06** - Validation Test: With Monthly Contributions (AC8)
3. **TC-01, TC-02, TC-03** - Required field validation
4. **TC-13** - Invalid input blocking
5. **TC-09** - Chart rendering
6. **TC-12** - Performance testing

#### Phase 2: Comprehensive Coverage (Medium Priority)
Execute these tests for complete feature validation:

1. **TC-04** - Optional field validation
2. **TC-07** - All compounding frequencies
3. **TC-08** - Time unit toggle
4. **TC-10** - Chart tooltips
5. **TC-11** - Breakdown table toggle
6. **TC-14, TC-15** - Responsive layout
7. **TC-16, TC-17, TC-18** - Input handling variations
8. **TC-19** - Chart updates
9. **TC-20, TC-21** - Breakdown table accuracy
10. **TC-24** - Currency formatting

#### Phase 3: Edge Cases (Low Priority)
Execute these tests for comprehensive coverage:

1. **TC-22** - Minimum interest rate
2. **TC-23** - Minimum duration

### Success Criteria

- **100% Pass Rate Required** for AC7 and AC8 validation tests (TC-05, TC-06)
- **95% Pass Rate Minimum** for all High priority test cases
- **Zero Critical Defects** in input validation and calculation accuracy
- **Performance Standard**: Real-time calculations must complete within 300ms (TC-12)
- **Responsive Design**: Both desktop and mobile layouts must render correctly (TC-14, TC-15)

### Defect Severity Guidelines

| Severity | Description | Examples |
|----------|-------------|----------|
| **Critical** | Feature is completely broken or produces incorrect financial calculations | Calculation formula wrong, app crashes, AC7/AC8 validation fails |
| **High** | Core functionality is impaired but workarounds exist | Validation not working, chart not rendering, performance > 500ms |
| **Medium** | Feature works but has usability issues | Tooltips missing, table doesn't collapse, minor formatting issues |
| **Low** | Cosmetic issues that don't affect functionality | Color scheme variations, minor text alignment |

### Browser Compatibility Testing

While not explicitly specified in requirements, recommend testing on:

- **Chrome** (latest 2 versions)
- **Firefox** (latest 2 versions)
- **Safari** (latest 2 versions)
- **Edge** (latest 2 versions)

### Performance Testing Notes

For TC-12 (Real-Time Update with Debouncing):

1. Open browser Developer Tools (F12)
2. Navigate to Performance tab
3. Record user interactions (input changes)
4. Measure time from input event to DOM update
5. Verify debouncing: rapid typing should trigger only one calculation after 300ms pause

---

## Appendix B: Calculation Verification Reference

### Test Data Reference Table

For manual verification of calculations:

| Scenario | Principal | Rate | Compound | Duration | Contribution | Expected Final Balance |
|----------|-----------|------|----------|----------|--------------|----------------------|
| **AC7 Validation** | $1,000 | 5% | Annually | 10 years | $0 | ~$1,628.89 |
| **AC8 Validation** | $1,000 | 7% | Monthly | 30 years | $100/month | ~$121,997 |
| **TC-07 Daily** | $5,000 | 6% | Daily | 5 years | $0 | ~$6,749.29 |
| **TC-07 Weekly** | $5,000 | 6% | Weekly | 5 years | $0 | ~$6,746.77 |
| **TC-07 Monthly** | $5,000 | 6% | Monthly | 5 years | $0 | ~$6,744.25 |
| **TC-07 Quarterly** | $5,000 | 6% | Quarterly | 5 years | $0 | ~$6,734.28 |
| **TC-07 Semi-Annually** | $5,000 | 6% | Semi-Annually | 5 years | $0 | ~$6,719.58 |
| **TC-07 Annually** | $5,000 | 6% | Annually | 5 years | $0 | ~$6,691.13 |

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

*This document serves as the single source of truth for requirement RQ-75 (ID: 41262552) and its associated test cases. All stakeholders should refer to this document for requirement details, test case specifications, and traceability information.*