# Specification Document

## Metadata
- **Generated**: 2026-05-26T21:07:59-04:00
- **Repository**: mserpone/qa-pipeline-sandbox
- **Branch**: main

## Requirement Details
### Requirement ID: 40309985
**Title**: MJ-41 Compound Interest Calculator - Real-Time Calculation with Visual Results

**Description**: As a personal finance user, I want to input investment details and instantly see compound interest projections with a visual breakdown, so that I can make informed decisions about my savings and investments without needing financial expertise.

**Project**: 127305 (qTest Project)

**Additional Details**:
- **Status**: New
- **Priority**: Must have
- **Type**: Functional
- **PID**: RQ-68
- **Created**: 2026-05-26T21:07:59-04:00
- **Last Modified**: 2026-05-26T21:07:59-04:00

### Acceptance Criteria
- All required inputs are validated inline with red underline and helper text on error
- Final Balance, Total Principal, and Total Interest Earned are displayed after valid input
- Growth chart renders as a stacked area chart with principal and interest color-coded
- Chart tooltips show exact values on hover
- Breakdown table is hidden by default and toggleable
- Results update within 300ms of any input change
- Calculator produces correct output: P=1000, r=5%, annually, 10yr = ~$1,628.89
- Calculator produces correct output with contributions: P=1000, r=7%, monthly, 30yr, PMT=$100 = ~$121,997
- Invalid inputs (negative rate, zero principal) show inline errors and block calculation
- Layout is responsive: two-column on desktop, single-column on mobile

### Functional Requirements
**Inputs**:
- Starting Amount ($) - Required, must be greater than 0
- Interest Rate (%) - Required, must be greater than 0 and less than or equal to 100
- Compounded - Required dropdown: Daily, Weekly, Monthly, Quarterly, Semi-Annually, Annually
- Duration - Required, must be greater than 0
- Time Unit - Toggle between Years and Months
- Monthly Contribution ($) - Optional, must be greater than or equal to 0

**Outputs**:
- Final Balance - Total value at end of the investment period
- Total Principal - Sum of initial deposit plus all contributions
- Total Interest Earned - Final balance minus total principal
- Growth Chart - Stacked area chart showing principal vs. interest over time
- Breakdown Table - Expandable year-by-year period breakdown (hidden by default)

**Calculation Formula**: A = P(1 + r/n)^(nt) + PMT x [((1 + r/n)^(nt) - 1) / (r/n)]

**Real-time Behavior**: Results and chart must update instantly as any input changes (no submit button). Input changes must be debounced by 300ms to avoid excessive recalculation.

### Technical Implementation
- **Framework**: React (functional components + hooks)
- **Charting**: Recharts (preferred)
- **Styling**: Tailwind CSS or CSS Modules
- **Backend**: No backend required — all calculations are client-side
- **Performance**: Use useMemo for calculation logic to avoid unnecessary re-renders

## Suitability Assessment
**Score**: A (96/100)
**Assessment Date**: 2026-05-26
**Confidence Score**: 0.95

### Score Breakdown
- **Completeness**: 24/25 - Excellent coverage of functional requirements, inputs/outputs, edge cases, dependencies, and technical specifications
- **Clarity**: 24/25 - Very clear and unambiguous language with well-defined technical terms
- **Testability**: 24/25 - Highly testable with measurable acceptance criteria and specific test cases
- **Detail**: 24/25 - Exceptional level of detail including workflows, specifications, and performance requirements

### Assessment Summary
This is an exceptionally well-written requirement that scores 96/100 (Grade A). It provides comprehensive functional specifications, clear acceptance criteria with measurable outcomes, specific test cases with expected results, and detailed technical guidance. The requirement is immediately suitable for test case generation with minimal refinement needed.

### Recommendations
- Add browser support matrix (Chrome, Firefox, Safari, Edge versions)
- Specify WCAG accessibility compliance level
- Define maximum input limits to prevent calculation overflow
- Add currency formatting rules (comma separators, decimal places)
- Specify chart axis labels and number formatting
- Consider loading indicators for complex calculations

## Generated Test Cases

### Test Case: Input Validation - Required Field Errors
- **ID**: TC-1963 (104328149)
- **Objective**: Verify that all required input fields show inline validation errors when empty or invalid
- **Preconditions**: Calculator page is loaded and accessible
- **Steps**:
  1. Leave Starting Amount field empty and click elsewhere
  2. Enter '0' in Starting Amount field
  3. Enter negative value (-100) in Starting Amount
  4. Enter '0' in Interest Rate field
  5. Enter negative value (-5) in Interest Rate
  6. Enter value greater than 100 (e.g., 150) in Interest Rate
- **Expected Results**: Each invalid input shows red underline and appropriate helper text
- **Priority**: High
- **Type**: Functional

### Test Case: Basic Calculation Accuracy - Standard Compound Interest
- **ID**: TC-1964 (104328150)
- **Objective**: Verify calculator produces correct output for the specified test case without contributions
- **Preconditions**: Calculator page is loaded with no validation errors
- **Steps**:
  1. Enter '1000' in Starting Amount field
  2. Enter '5' in Interest Rate field
  3. Select 'Annually' from Compounded dropdown
  4. Enter '10' in Duration field and ensure Time Unit is 'Years'
  5. Verify calculated results are displayed
- **Expected Results**: Final Balance shows ~$1,628.89, Total Principal shows $1,000, Total Interest Earned shows ~$628.89
- **Priority**: High
- **Type**: Functional

### Test Case: Advanced Calculation Accuracy - With Monthly Contributions
- **ID**: TC-1965 (104328151)
- **Objective**: Verify calculator produces correct output for the specified test case with monthly contributions
- **Preconditions**: Calculator page is loaded with no validation errors
- **Steps**:
  1. Enter '1000' in Starting Amount field
  2. Enter '7' in Interest Rate field
  3. Select 'Monthly' from Compounded dropdown
  4. Enter '30' in Duration field and ensure Time Unit is 'Years'
  5. Enter '100' in Monthly Contribution field
  6. Verify calculated results are displayed
- **Expected Results**: Final Balance shows ~$121,997
- **Priority**: High
- **Type**: Functional

### Test Case: Real-Time Updates and Performance
- **ID**: TC-1966 (104328152)
- **Objective**: Verify that results update within 300ms of input changes
- **Preconditions**: Calculator page is loaded with valid initial values entered
- **Steps**:
  1. Enter valid values: Starting Amount=5000, Rate=8%, Compounded=Annually, Duration=5 Years
  2. Change Starting Amount to 10000 and measure response time
  3. Change Interest Rate to 6% and measure response time
  4. Change Duration to 10 and measure response time
  5. Change Monthly Contribution to 200 and measure response time
- **Expected Results**: Results update within 300ms of each change
- **Priority**: High
- **Type**: Performance

### Test Case: Growth Chart Rendering and Functionality
- **ID**: TC-1967 (104328153)
- **Objective**: Verify that the growth chart renders correctly as a stacked area chart with proper color coding
- **Preconditions**: Calculator page is loaded with valid input values that generate results
- **Steps**:
  1. Enter valid values: Starting Amount=2000, Rate=6%, Compounded=Monthly, Duration=5 Years
  2. Verify chart displays as stacked area chart
  3. Hover over different points on the principal area
  4. Hover over different points on the interest area
  5. Verify color coding is consistent
- **Expected Results**: Chart shows two distinct colored areas with tooltips showing exact values on hover
- **Priority**: High
- **Type**: Functional

### Test Case: Breakdown Table Toggle Functionality
- **ID**: TC-1968 (104328154)
- **Objective**: Verify that breakdown table is hidden by default and can be toggled on/off
- **Preconditions**: Calculator page is loaded with valid input values generating results
- **Steps**:
  1. Enter valid calculation parameters and verify results are displayed
  2. Look for breakdown table in the results area
  3. Locate and click the toggle button/link for breakdown table
  4. Click the toggle button/link again
  5. Verify table contains year-by-year details when visible
- **Expected Results**: Table is hidden by default, toggleable, and shows period-by-period breakdown when visible
- **Priority**: Medium
- **Type**: Functional

### Test Case: Responsive Design - Desktop and Mobile Layout
- **ID**: TC-1969 (104328155)
- **Objective**: Verify layout adapts correctly between desktop two-column and mobile single-column layouts
- **Preconditions**: Calculator page is accessible on both desktop and mobile viewports
- **Steps**:
  1. Open calculator on desktop browser (viewport width > 768px)
  2. Resize browser window to mobile width (< 768px) or open on mobile device
  3. Enter valid values on mobile view and verify functionality
  4. Resize back to desktop width
- **Expected Results**: Layout displays in two-column format on desktop and single-column on mobile
- **Priority**: Medium
- **Type**: Functional

### Test Case: Compound Frequency Variations
- **ID**: TC-1970 (104328156)
- **Objective**: Verify calculations are accurate across different compounding frequencies
- **Preconditions**: Calculator page is loaded and accessible
- **Steps**:
  1. Enter: Starting Amount=1000, Rate=5%, Duration=10 Years, select 'Daily' compounding
  2. Change compounding to 'Weekly'
  3. Change compounding to 'Quarterly'
  4. Change compounding to 'Semi-Annually'
  5. Verify higher frequency compounding yields higher results
- **Expected Results**: Final balance decreases as compounding frequency decreases
- **Priority**: Medium
- **Type**: Functional

### Test Case: Time Unit Toggle - Years vs Months
- **ID**: TC-1971 (104328157)
- **Objective**: Verify duration calculations work correctly when toggling between Years and Months
- **Preconditions**: Calculator page is loaded and accessible
- **Steps**:
  1. Enter: Starting Amount=5000, Rate=8%, Compounded=Monthly, Duration=2, Time Unit='Years'
  2. Toggle Time Unit to 'Months' without changing Duration value
  3. Change Duration to 24 while Time Unit remains 'Months'
  4. Toggle back to 'Years' with Duration=24
- **Expected Results**: Calculations adjust correctly for time unit changes
- **Priority**: Medium
- **Type**: Functional

### Test Case: Edge Case - Zero Monthly Contributions
- **ID**: TC-1972 (104328158)
- **Objective**: Verify calculator handles zero monthly contributions correctly
- **Preconditions**: Calculator page is loaded and accessible
- **Steps**:
  1. Enter valid required values and leave Monthly Contribution empty
  2. Enter '0' in Monthly Contribution field
  3. Enter negative value (-50) in Monthly Contribution field
  4. Compare results with and without contributions using same other parameters
- **Expected Results**: Handles zero contributions correctly and validates negative values
- **Priority**: Low
- **Type**: Functional

### Test Case: Input Boundary Values
- **ID**: TC-1973 (104328159)
- **Objective**: Test calculator behavior with extreme but valid input values
- **Preconditions**: Calculator page is loaded and accessible
- **Steps**:
  1. Enter very small starting amount (e.g., $0.01)
  2. Enter very large starting amount (e.g., $1,000,000)
  3. Enter very small interest rate (e.g., 0.01%)
  4. Enter maximum interest rate (100%)
  5. Test maximum duration values and verify no overflow errors occur
- **Expected Results**: Calculator handles extreme values without errors
- **Priority**: Low
- **Type**: Functional

### Test Case: Error Recovery and Calculation Blocking
- **ID**: TC-1974 (104328160)
- **Objective**: Verify that invalid inputs prevent calculation and clear errors allow calculation to resume
- **Preconditions**: Calculator page is loaded
- **Steps**:
  1. Enter invalid Starting Amount (-1000) and valid other fields
  2. Correct Starting Amount to positive value (1000)
  3. Enter invalid Interest Rate (-5%) while other fields remain valid
  4. Correct Interest Rate to valid value (5%)
- **Expected Results**: Invalid inputs block calculation; correcting errors resumes calculation
- **Priority**: Medium
- **Type**: Functional

## Traceability
### Requirement-to-Test-Case Links
| Requirement ID | Test Case ID | Test Case Name |
|----------------|--------------|----------------|
| 40309985 | TC-1963 | Input Validation - Required Field Errors |
| 40309985 | TC-1964 | Basic Calculation Accuracy - Standard Compound Interest |
| 40309985 | TC-1965 | Advanced Calculation Accuracy - With Monthly Contributions |
| 40309985 | TC-1966 | Real-Time Updates and Performance |
| 40309985 | TC-1967 | Growth Chart Rendering and Functionality |
| 40309985 | TC-1968 | Breakdown Table Toggle Functionality |
| 40309985 | TC-1969 | Responsive Design - Desktop and Mobile Layout |
| 40309985 | TC-1970 | Compound Frequency Variations |
| 40309985 | TC-1971 | Time Unit Toggle - Years vs Months |
| 40309985 | TC-1972 | Edge Case - Zero Monthly Contributions |
| 40309985 | TC-1973 | Input Boundary Values |
| 40309985 | TC-1974 | Error Recovery and Calculation Blocking |

### Coverage Summary
- **Total Test Cases**: 12
- **Requirements Covered**: 1
- **Acceptance Criteria Coverage**: 100% (10/10)
- **Test Priority Distribution**: High (5), Medium (4), Low (3)

## Document History
- 2026-05-26T21:07:59-04:00: Document created with complete requirement analysis, suitability assessment, and comprehensive test case generation