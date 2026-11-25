# US Paycheck & Tax Data 2026–2027

Free, machine-readable **US federal payroll tax numbers for 2026 and 2027**: tax brackets, standard deductions, Social Security wage base, Medicare, Child Tax Credit, 401(k)/IRA limits and the new overtime & tips deductions. Use it in your own payroll scripts, spreadsheets or apps.

📄 Data file: [`us-tax-data-2026-2027.json`](us-tax-data-2026-2027.json)

## Key numbers at a glance

| Item | 2026 | 2027 |
|---|---|---|
| Standard deduction (single) | $16,100 | $16,600 |
| Standard deduction (married joint) | $32,200 | $33,200 |
| 22% bracket starts (single) | $50,400 | $52,025 |
| Social Security wage base | $184,500 | $192,300 |
| Child Tax Credit | $2,200 | $2,300 |
| 401(k) limit | $24,500 | $25,500 |
| IRA limit | $7,500 | $7,500 |

Bracket arrays are `[threshold, rate]` pairs on **taxable income** (after the standard deduction).

## Example (JavaScript)

```js
const data = require('./us-tax-data-2026-2027.json');

function federalTax(gross, year = '2026') {
  const y = data[year];
  const taxable = Math.max(0, gross - y.standard_deduction.single);
  const b = y.brackets_single;
  let tax = 0;
  for (let i = 0; i < b.length; i++) {
    const [lo, rate] = b[i];
    const hi = i + 1 < b.length ? b[i + 1][0] : Infinity;
    if (taxable > lo) tax += (Math.min(taxable, hi) - lo) * rate;
  }
  return Math.round(tax);
}

console.log(federalTax(60000, '2026')); // 5020
console.log(federalTax(60000, '2027')); // 4952
```

## Free online calculators

Don't want to code it? These free tools use the same numbers:

- [Paycheck Calculator (all 50 states)](https://paycalclab.com/paycheck-calculator/) – take-home pay after federal, state and FICA taxes
- [Hourly to Salary Calculator](https://paycalclab.com/hourly-to-salary-calculator/)
- [Overtime Pay Calculator](https://paycalclab.com/overtime-calculator/) – including the No Tax on Overtime deduction
- [Take-Home Pay by State](https://paycalclab.com/take-home-pay-by-state/) – all 50 states compared
- [Self-Employment Tax Calculator](https://paycalclab.com/self-employment-tax-calculator/)
- [Bonus Tax Calculator](https://paycalclab.com/bonus-tax-calculator/)
- [401(k) Calculator](https://paycalclab.com/401k-calculator/)

## Guides

- [2027 Tax Brackets & Standard Deduction](https://paycalclab.com/blog/2027-tax-brackets-standard-deduction/)
- [No Tax on Overtime: How Much You Actually Save](https://paycalclab.com/blog/no-tax-on-overtime-how-much-you-save/)
- [No Tax on Tips Explained](https://paycalclab.com/blog/no-tax-on-tips-explained/)
- [Minimum Wage by State 2027](https://paycalclab.com/blog/minimum-wage-by-state-2027/)
- [States With No Income Tax](https://paycalclab.com/blog/states-with-no-income-tax/)

## Sources

IRS Revenue Procedure 2025-32 (2026) and 2026-35 (2027); IRS Notice 2025-67 and 2026-66 (retirement limits); Social Security Administration COLA fact sheets. Numbers are provided for educational use — verify with IRS.gov before filing.

## License

MIT. Maintained by [PayCalcLab](https://paycalclab.com/). Issues and corrections welcome.
