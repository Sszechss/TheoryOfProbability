# TheoryOfProbability
# Does studying more help you pass math?

A course project for **Theory of Probability and Mathematical Statistics** at Narxoz University (2026).

I took the real final grades of 395 secondary school students and asked a simple question: do students who study more, or who pay for extra classes, pass math more often? The whole analysis uses only the first tools of probability theory: a sample space, events, Kolmogorov's axioms and classical probability (Larsen & Marx, Sections 2.2–2.3).

The result is a small website. It is a single HTML file with no build step.

## The short answer

Yes, a little. In this data, both groups pass about 10 percentage points more often, but most students pass either way, and the data can't show cause and effect.

| Group | Passed / students | Share passed |
|---|---:|---:|
| All students | 265 / 395 | 0.671 (67%) |
| Study more than 5 h a week | 69 / 92 | 0.750 (75%) |
| Study 5 h or less | 196 / 303 | 0.647 (65%) |
| Take paid extra classes | 130 / 181 | 0.718 (72%) |
| No paid classes | 135 / 214 | 0.631 (63%) |

## What's on the site

The home page has an interactive grid with one dot per student and a card for each part of the project. Every part opens as its own page.

| Part | What it covers |
|---|---|
| 1. Real-world problem | Why the question matters |
| 2. Research question | The question, turned into three events: Y (passed), A (studies more than 5 h a week), T (paid classes) |
| 3. Data collection | Where the data comes from, the four columns used and how clean they are |
| 4. Statistical methods | Sample space, a Venn diagram of A and Y, De Morgan's law, a check of all three axioms, Theorems 2.3.1, 2.3.3 and 2.3.6, and a comparison of pass rates between groups |
| 5. Interpretation | What the numbers show and what they can't |
| 6. Conclusion | The answer and references |
| 7. Extensions | Ideas for continuing with Sections 2.4 and 2.5 |

The pandas code behind every number is in the appendix at the end of part 7.

## Viewing it

Download `index.html` and open it in any browser. Nothing needs to be installed.

To publish it with GitHub Pages:

1. Push the repository to GitHub.
2. Open **Settings → Pages**.
3. Under **Build and deployment**, pick **Deploy from a branch**, then select `main` and `/ (root)`.
4. After a minute the site will be live at `https://USERNAME.github.io/REPOSITORY/`.

Each part has its own link, for example `.../#/methods` or `.../#/conclusion`, so you can send someone straight to one section.

## Reproducing the numbers

1. Download the dataset from the [UCI Machine Learning Repository](https://archive.ics.uci.edu/dataset/320/student+performance) and unzip `student-mat.csv` into the project folder.
2. Install pandas:

   ```bash
   pip install pandas
   ```

3. Run this script:

   ```python
   import pandas as pd

   df = pd.read_csv("student-mat.csv", sep=";")   # the UCI file uses ";"

   Y = df.G3 >= 10            # passed
   A = df.studytime >= 3      # studies more than 5 hours a week
   T = df.paid == "yes"       # paid extra classes
   N = lambda e: int(e.sum())

   print(len(df))                                     # 395
   print(N(Y), N(A), N(T))                            # 265 92 181
   print(N(A & Y), N(A & ~Y), N(~A & Y), N(~A & ~Y))  # 69 23 196 107
   print(N(A | Y), N(~(A | Y)))                       # 288 107 (De Morgan)
   print(N(T & Y), N(A & T))                          # 130 50
   print(df.groupby("studytime").G3.agg(["size", lambda g: (g >= 10).sum()]))

   for name, e in [("A", A), ("not A", ~A), ("T", T), ("not T", ~T)]:
       print(name, round(N(e & Y) / N(e), 3))          # share passed
   ```

The comments show the output you should get. Every number on the site comes from these counts.

## How it's built

- Plain HTML, CSS and JavaScript in one file. There is no framework and no build step. The only outside request is Google Fonts, and the page falls back to system fonts without it.
- Navigation between parts is handled in JavaScript instead of plain anchor links. This keeps it working inside sandboxed previews and iframes, where `#` links can break. The browser's Back button and direct links to a part still work.
- Light and dark themes follow the system setting.
- Accessibility was checked against WCAG 2.1 AA. Text contrast is at least 5:1 in both themes, and every button and link works from the keyboard. Focus moves to the heading of each new page, results are announced to screen readers, and animations switch off when the system asks for reduced motion.
- The layout works down to 320 px wide without horizontal scrolling. On phones, wide tables turn into cards.

## Data

The data is the Student Performance dataset by P. Cortez, collected in 2005–2006 at two public secondary schools in Portugal. This project uses only `student-mat.csv` (the math course) and four of its 33 columns: `G3`, `studytime`, `paid` and `failures`.

The dataset is published under the [Creative Commons Attribution 4.0 International (CC BY 4.0)](https://creativecommons.org/licenses/by/4.0/) license.

> Cortez, P. (2008). *Student Performance* [Dataset]. UCI Machine Learning Repository. https://doi.org/10.24432/C5TG7T

## References

1. Larsen, R. J., & Marx, M. L. (2018). *An Introduction to Mathematical Statistics and Its Applications* (6th ed.). Pearson. Sections 2.2–2.3.
2. Cortez, P., & Silva, A. (2008). Using data mining to predict secondary school student performance. In *Proceedings of the 5th Future Business Technology Conference (FUBUTEC 2008)*, 5–12.
3. Cortez, P. (2008). *Student Performance* [Dataset]. UCI Machine Learning Repository. https://doi.org/10.24432/C5TG7T

## Author

Kenzhebaiuly Yerzhan, Narxoz University, 2026.
