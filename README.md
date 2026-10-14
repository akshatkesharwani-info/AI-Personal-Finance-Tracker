# AI Personal Finance Tracker — Track Income and Expenses, Get Spending Insights

A Colab notebook that reads your income and expenses from a CSV or Excel file, shows where the money goes, compares this month with last month, checks a simple budget, and uses an AI to write the insights in plain words. All numbers and the reasons behind them are worked out in code first, so the AI only has to explain them.

## Problem

Most people do not know where their money goes each month, and a spreadsheet of rows does not tell you what changed or why.

## Approach

1. Load a CSV or Excel file with the columns `Date`, `Category`, `Description`, `Amount`, `Type` (Income or Expense). If you skip the upload, the notebook builds 3 months of sample data
2. Clean the data (dates, numbers, months) with pandas
3. Monthly summary: income, expenses, savings and savings percentage
4. Charts: income/expense/savings by month, expenses by category for the latest month (pie), and expenses by category for each month (stacked bar)
5. Compare the latest month with the one before, and check a simple budget per category
6. Work out the facts in code: the biggest increase, over-budget categories, how income, expenses and savings changed, and which of them is the bigger cause
7. Send the summary table and the facts to an LLM on Groq (`openai/gpt-oss-120b`) with rules: use only the given facts, do not claim a trend that is not in them
8. Check the AI text: list any number in it that is not in the data

## Tech Stack

Python, pandas, matplotlib, Groq API (OpenAI-compatible client), Google Colab

## Results

One real run on a made-up file of 70 transactions for Aug to Oct 2026.

| Month | Income | Expense | Savings | Savings % |
|---|---|---|---|---|
| 2026-08 | 42,000 | 28,100 | 13,900 | 33.1 |
| 2026-09 | 48,000 | 29,800 | 18,200 | 37.9 |
| 2026-10 | 42,000 | 31,000 | 11,000 | 26.2 |

- **Biggest increase:** Food, up 25% from September to October (7,200 to 9,000)
- **Budget check (October):** only Food was over, by 1,000 rupees (9,000 against 8,000). The other five categories were within budget
- **Why savings fell by 7,200:** income fell by 6,000 and expenses rose by 1,200, so income was the bigger cause. The AI's insight said exactly this
- **Number check:** no number in the AI text was missing from the data

**A mistake I found and fixed.** In my first run the AI wrote that savings fell "even though income stayed the same". Income had actually dropped from 48,000 to 42,000. Every number in that sentence was correct, so a number check alone would have missed it (I tested this). The fix was to work out the reason in code, hand it to the AI as a fact, and tell the AI not to claim trends that are not in the facts.

## How to Run

1. Open `06_Personal_Finance_Tracker.ipynb` in Google Colab
2. Get a free API key from [console.groq.com](https://console.groq.com)
3. Click Runtime → Run all, paste the key when asked
4. Upload your CSV or Excel file (or cancel to use the sample data)
5. Read the charts and the insights. Change the budget limits in Step 8 to your own

## Limitations

- Tested on one made-up file (3 months, 70 rows), not on real bank data
- The number check only looks at numbers. A wrong sentence made of correct numbers can still get through, so read the insights
- In the final run I skipped the "add a transaction" step (Step 10) and used an uploaded file, so that step and the built-in sample data were not checked in that run
- Budget limits are examples, and the currency is rupees
- In the stacked bar chart the legend covers part of the last bar
- Needs a clean file with the 5 columns. Bank statement PDFs are not supported
- It runs as a notebook only and is not deployed

## Next Steps

- Move the chart legend outside the plot
- Read bank statement PDFs
- Recurring bills and savings goals
- Streamlit dashboard

## Author

**Akshat Kesharwani** | [GitHub](https://github.com/akshatkesharwani-info) | [Portfolio](https://akshatkesharwani-info.github.io/)
