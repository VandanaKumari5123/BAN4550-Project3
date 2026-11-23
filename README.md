# Do good earnings news lift the stock more for carmakers than for banks?

**Author:** Vandana Kumari · BAN 4550, Fall 2026 · Industry: SIC 37 Transportation Equipment · Comparison industry: SIC 60 Depository Institutions

## The question
My client is Ford. Ford wants to know if its stock moves up or down after an earnings announcement in the same way as bank stocks do. SIC is a number code that tells which industry a company is in.

## Main result
I looked at earnings announcements from 2021 to 2024. For each one I measured how big the earnings surprise was (SUE) and how much the stock moved compared with the market in the three days around the news (CAR). I put the announcements in three equal groups, Low, Middle and High surprise. In both industries, the High group had a higher CAR than the Low group. The difference is 4.33 percentage points for carmakers (985 announcements) and 0.91 for banks (3,968 announcements). So the reaction is about four times bigger for carmakers.

![Figure 1](figures/figure1.png)

## Is the difference real?
- **t-test, High group against Low group:** carmakers: difference 4.33 points, t = 5.52, p below 0.001. Banks: difference 0.91 points, t = 4.60, p below 0.001. In both industries the difference is significant, because p is below our class cutoff of 0.10.
- **Regression (CAR = a + b × SUE):** carmakers: b = 0.084, p = 0.009, R² = 0.7%. Banks: b = 0.049, p = 0.004, R² = 0.2%. When SUE goes up by 0.01, CAR goes up by about 0.08 points for carmakers. R² is very small. This is normal, because many other things move a stock in three days.
- **Did b change in the other models?** I also ran the regression with year effects and with a loss variable, with errors grouped by company. For carmakers b stayed between 0.084 and 0.105 and was significant every time. For banks b was 0.049 and not significant in the basic grouped model, 0.063 with year effects and 0.078 with the loss variable. So the bank result is weaker.

![Figure 2](figures/figure2.png)

## Why the industries differ
My idea was loss quarters. Carmakers lose money in many quarters (21.5% of quarters, and 42% in the Low group), while banks almost never do (2.5%). So I thought losses might cause the big gap. I tested it by taking out all loss quarters. The carmaker gap did not get smaller. It got bigger, from 4.33 to 4.9 points. For banks it went from 0.91 to 0.81. So loss quarters are not the reason. I did not test other reasons. One idea for later is timing: about 90% of bank announcements are in January, April, July and October, but carmaker announcements are spread over the year.

![Figure 3](figures/figure3.png)

## Advice for the client
- **What Ford should know:** Ford's stock reacts strongly to earnings news. The worst-news group fell about 2% against the market and the best-news group rose about 2%. So Ford should be careful about how it shares its results.
- **Be careful because:** I only used four years of data and one comparison industry. R² is under 1%. The results show that two things move together, not that one causes the other. For banks the result is weaker in the basic grouped model.
- **Next questions:**
  - *Predictive:* can SUE plus other information predict the stock reaction before the announcement? Extra data needed: analyst forecasts (IBES), trading volume and company size.
  - *Prescriptive:* should Ford change when or how it shares earnings, for example the time of day or adding guidance, to make bad-news reactions smaller? Extra data needed: the time of each announcement and the text of press releases and earnings calls.

## Data
- Sources: Compustat quarterly (`comp.fundq`), CRSP daily stock file and market index (`crsp.dsf`, `crsp.dsi`), and the CRSP/Compustat link file, all through WRDS.
- **The data are not in this repository** because of the WRDS license. Someone with WRDS access can run the notebook with the link file in the same folder to get the same results.
- Period and rules: announcements from January 2021 to December 2024. The announcement date must be known, SUE must be possible to calculate, the stock price from last quarter must be at least $5, SUE must be between −1 and 1, and the three daily returns around the news must exist. I used the same rules for both industries.

## How the tables fit together
![Merge map](figures/merge_map.png)

First I join the Compustat quarters with the link file using `gvkey` to get the CRSP stock id (`permno`). I keep a link only if it was valid on the quarter-end date. Then I add the EPS and price from earlier quarters using `gvkey` and the quarter number. Day 0 is the first trading day on or after the announcement date, and I get the three daily returns using `permno` and the trading day. One row in the final table is one announcement.

## How I checked the results
I counted the rows after every merge and rule. This showed that the last rule removed 26 bank announcements. I looked into it: five banks stop trading in the CRSP data before 2024, probably after a merger or delisting, so they have no stock returns. I also checked duplicates, dates, the biggest CAR values, and one JPMorgan announcement by hand. I also re-ran my Project 2 notebook. It gives the same carmaker numbers as this project (985 announcements, group averages −0.0212, −0.0001 and 0.0221). They are a little different from my Project 2 report, by at most 0.0005. I think WRDS updated its data, but I did not prove it.

## Changes since Project 2
| What was wrong | How I fixed it | How the results changed |
| --- | --- | --- |
| Nothing. My Project 2 feedback did not name any code or merge problem. | I added banks (SIC 60) to the same tables, counted rows by industry, and added the t-test, the regressions and Figures 1 to 3. | The carmaker results are the same as in my Project 2 re-run. |

## Where the code came from
I started from the class notebooks and my own Project 2 notebook. I used Claude (an AI tool) to write and test the code and to write the first version of the text and slides. I then ran all the code on WRDS, checked the results, and reworded the text myself.

| AI tool | Link (or PDF in docs/) | What I used it for |
| --- | --- | --- |
| Claude | https://claude.ai/share/68ce6e0d-9f32-45e6-9ce6-c128e911ab7c  | Writing and testing the code, explaining each step, and first drafts of the slides and this README |

## Files
```
notebooks/   Kumari_Vandana_Project3.ipynb (runs from top to bottom on WRDS JupyterHub)
figures/     figure1.png, figure2.png, figure3.png, merge_map.png
docs/        Kumari_Vandana_Project3.pdf (slides), Kumari_Vandana_Project3_notebook.pdf
README.md
```

