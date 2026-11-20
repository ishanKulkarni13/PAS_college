# HA-4 study docs

Predicting final exam marks: EDA, correlation, simple linear regression, evaluation metrics.

Read in this order:

| # | File | Use it for |
|---|------|-----------|
| 1 | [01-question-explained.md](01-question-explained.md) | What the question actually asks, phrase by phrase |
| 2 | [02-theory.md](02-theory.md) | Theory + formulas + intuition (correlation, least squares, metrics, residuals) |
| 3 | [03-code-walkthrough.md](03-code-walkthrough.md) | What each part of `ha-4.ipynb` does and why |
| 4 | [04-results-and-interpretation.md](04-results-and-interpretation.md) | Every number in the notebook and how to say it in words |
| 5 | [05-viva-prep.md](05-viva-prep.md) | Likely viva questions with short answers, plus a formula sheet |

**Honest note about the slides:** the PPTs in `notes/ppts/` do not cover regression. What they do cover and what we used: the correlation strength table (Unit I slide 62), histograms / box plots (Unit II), and hypothesis testing ideas (Unit III) for the significance of correlation. Regression theory in these docs is standard textbook material, so study `02-theory.md` properly.

Data: `PAS-HA_4-Probability_Statistics_Datasets.csv`, 100 students, columns `Student_ID, Attendance_%, Study_Hours, Assignment_Marks, Internal_Marks, Final_Exam_Marks`. (It is the same data file as HA-1.)
