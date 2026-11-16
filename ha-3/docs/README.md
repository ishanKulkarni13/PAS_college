# HA-3 study docs

Patient satisfaction: sampling, confidence intervals, hypothesis tests, one-way ANOVA.

Read in this order:

| # | File | Use it for |
|---|------|-----------|
| 1 | [01-question-explained.md](01-question-explained.md) | What the question actually asks, phrase by phrase |
| 2 | [02-theory.md](02-theory.md) | Theory + formulas + intuition behind every step |
| 3 | [03-code-walkthrough.md](03-code-walkthrough.md) | What each part of `ha-3.ipynb` does and why |
| 4 | [04-results-and-interpretation.md](04-results-and-interpretation.md) | Every number in the notebook and how to say it in words |
| 5 | [05-viva-prep.md](05-viva-prep.md) | Likely viva questions with short answers, plus a formula sheet |

Where I mention "Unit III slide 22" it means the PDF in `notes/ppts/` (slide number = page number in the PDF).

**Honest note about the slides:** Sir's slides cover sampling (Unit I), the CLT, confidence intervals, hypothesis testing, p-values, Type I/II error, Z-test and t-tests (Unit III). One-way ANOVA appears only in the syllabus slide (Unit III slide 2), so the ANOVA and Tukey theory in these docs comes from standard textbook material. Be ready to explain it from `02-theory.md`.

Data: `PAS-HA_3-Probability_Statistics_Datasets.csv`, 120 patients, columns `Patient_ID, Hospital (A/B/C), Age_Group (18-30, 31-45, 46-60, 60+), Satisfaction_Score`.
