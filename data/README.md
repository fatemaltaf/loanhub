# Data dictionary

All data is synthetic and was generated for this hackathon. It contains no real applicants, customers or loans. Applicants appear only as anonymised IDs.

| File | Rows | One row per |
|---|---:|---|
| [`loan_applications.csv`](loan_applications.csv) | 402 | Loan application, October 2026 |

The file is a UTF-8, comma-separated CSV with a header row.

## loan_applications.csv

| Column | Type | Example | Notes |
|---|---|---|---|
| `application_id` | Text | `APP-2026-000279` | Identifies the application. See [Things to know](#things-to-know). |
| `submitted_at` | ISO 8601 date and time with offset | `2026-10-23T18:18:46+03:00` | Kuwait time (UTC+03:00, no daylight saving time). Its date part is the application date. The file is sorted by this column. |
| `applicant_id` | Text | `APL-85709` | The applicant, as an anonymised ID. |
| `date_of_birth` | Date | `1969-01-12` | Used by `AGE`. See [Things to know](#things-to-know). |
| `employment_status` | Text | `GOVERNMENT` | `GOVERNMENT`, `PRIVATE`, `SELF_EMPLOYED` or `RETIRED`. |
| `monthly_income_kwd` | Decimal, 3 places | `1556.088` | Monthly salary, pension or business income in Kuwaiti dinars. Used by `MIN_INCOME` and `DEBT_TO_INCOME`. |
| `existing_monthly_debt_kwd` | Decimal, 3 places | `256.461` | The total of the applicant's existing monthly instalments. `0.000` if there are none. |
| `loan_amount_kwd` | Decimal, 3 places | `8000.000` | The amount requested: P in the instalment formula. |
| `term_months` | Integer | `48` | The number of monthly instalments: n in the instalment formula. Mostly 12 to 60, with a few 72 or 84. |
| `annual_rate_pct` | Decimal, 2 places | `6.25` | The annual interest rate in percent. `0.00` for staff and promotional loans. |
| `loan_purpose` | Text | `CAR` | `PERSONAL`, `CAR`, `EDUCATION`, `HOME_IMPROVEMENT`, `MEDICAL` or `TRAVEL` |
| `channel` | Text | `MOBILE` | `BRANCH`, `ONLINE` or `MOBILE`: where the application was submitted. Branches open Sunday to Thursday. |

The [eligibility rules](../README.md#-what-youll-build) explain how to compute the instalment, the debt-to-income ratio and the ages from these columns.

## Things to know

Real application data is messy, so this file is too. It deliberately contains a small number of:

- **Duplicate records:** the same `application_id` appears more than once, because an application was resubmitted.
- **Missing values:** some rows have no `monthly_income_kwd`, and one row has no `date_of_birth`.
- **Impossible values:** a few values can't be right, such as a term of 0 months, a negative existing debt or a date of birth after the submission date.
- **Inconsistent date formats:** a few dates of birth are in DD/MM/YYYY instead of YYYY-MM-DD.
- **Thousands separators:** a few amounts include a comma, for example `"1,250.000"`. The CSV quotes these values.

The file also has a few loans at 0%, for staff or promotions. They're valid, but the usual instalment formula divides by zero for them. The [eligibility rules](../README.md#-what-youll-build) say how to handle them.

These issues can change which applications are approved, and which are refused as invalid. Decide how your app handles each one, record the decision in your plan and test it.

KWD has 3 decimal places (1 KWD = 1,000 fils). If your stack has a decimal type for money, use it.
