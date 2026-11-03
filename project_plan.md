# STAT 486 Project Plan

**Due:** Tuesday, October 27, 2026, at 11:59 PM Mountain Time  
**Team:** Corbin Christiansen, Jun Kim, Eli Barnes  
**Repository:** https://github.com/vkdlxj008/final_prj_for_stat486  
**Date updated:** Nov 2, 2026

## Project question and purpose

- **What specific question do you want to investigate?**  
  Does knowing what happened to a passenger's travel companions improve Titanic survival prediction beyond the passenger's own characteristics?
- **Why does this question interest your team, and what might someone learn from answering it?**  
  Most models treat passengers as independent, but families and ticket groups may share outcomes. The answer shows how much that dependence adds to prediction.
- **What claim or expectation could your evidence support, challenge, or leave uncertain?**  
  We expect a small improvement. The evidence could support this, show no gain, or leave the difference too small to distinguish from noise.
- **What does one observation represent in the data you expect to use?**  
  One passenger.

## Chosen route

- [ ] **A. Develop something new** in an ML method or system
- [x] **B. Kaggle competition** with two meaningfully different pipelines
- [ ] **C. Build a dataset** and use it in a reproducible ML pipeline

**Why this route:** The competition provides fixed data and an independent score, so the team can focus on comparing two pipelines.

## Data source and access

- **What data will you use?**  
  Kaggle's [Titanic competition data](https://www.kaggle.com/competitions/titanic).
- **What does each row or record represent, and which fields are relevant to the question?**  
  Each row is a passenger. Relevant fields are `Survived`, `Pclass`, `Name`, `Sex`, `Age`, `SibSp`, `Parch`, `Ticket`, `Fare`, and `Embarked`.
- **How will the team obtain the data?**  
  Each member will accept the competition rules and download the data from Kaggle. The 891 training and 418 test rows can be processed on a laptop.
- **What license, competition rules, privacy, ethical, or other use limits apply?**  
  We will check the competition rules and will not commit raw data unless they allow it. The data contain historical passenger names, which we will use only to extract titles and surnames.
- **What data quality issues might affect the investigation?**  
  `Age` and `Cabin` have many missing values. Surnames and tickets are imperfect indicators of who traveled together, and the small sample makes small differences hard to interpret.

## Route-specific plan

### Route B: Kaggle competition: Titanic

- **Which competition are you considering, and has the instructor approved it?**  
  Titanic: Machine Learning from Disaster. It has not been approved yet, and we are requesting approval.
- **Do its rules allow the team's planned data and code use? Are data access and compute feasible by Dec. 10?**  
  We will confirm the rules, and we do not plan to use external data. Access and compute are feasible by Dec. 10.
- **What will pipeline 1 do, from competition data through preparation, local validation, training, and inference?**  
  It will use passenger-level features, handle missing values and encoding inside each fold, evaluate with cross-validation, train gradient-boosted trees, and predict the test set for submission.
- **What substantive design decision will differ in pipeline 2? Why investigate that change?**  
  Pipeline 2 will add ticket and surname group features, including the survival rate of known companions. This tests whether modeling relationships between passengers improves on treating them as independent.
- **What local validation approach and metric will you use? How will you guard against leakage?**  
  We will use repeated stratified 5-fold cross-validation with accuracy, the competition metric, and log loss as a secondary metric. Group survival features will use training-fold labels only and exclude the passenger's own label.
- **How will you record each pipeline's run command, configuration, commit, valid Kaggle submission, local result, and Kaggle result?**  
  We will record them in `results/runs.csv` and keep each configuration in `configs/`.

## Initial reference and proposed contribution

- **What is the simplest credible baseline, reference method, or comparison for your question?**  
  Kaggle's sex-only `gender_submission.csv`, followed by Pipeline 1.
- **What will your team design, build, collect, or investigate beyond that reference?**  
  We will build leakage-safe group features and compare the two pipelines under the same folds.
- **Which choices, code, data, papers, notebooks, packages, or pretrained models will come from others, and how will you credit them?**  
  The data come from Kaggle, and the code will use scikit-learn and related packages. We will cite any public notebook we use in the README and code comments. We do not plan to use pretrained models.

## Evaluation and evidence

- **What evaluation design fits the question and how the data were collected?**  
  Repeated stratified 5-fold cross-validation with the same folds for both pipelines. We will also hold out whole ticket groups as a stability check.
- **What metric or diagnostic will you use, and why does it match the question?**  
  Accuracy matches the Kaggle score, and log loss shows whether predicted probabilities improve.
- **How will you compare your work with a baseline or other reference? What would count as evidence against your initial expectation?**  
  We will compare fold-by-fold differences between the pipelines and against the baseline. Little or no improvement in accuracy and log loss would count against our expectation.
- **What informative error, failure, uncertainty, tradeoff, or limitation will you inspect?**  
  We will inspect passengers whose predictions change, errors by group size, and false surname matches. With only 418 test passengers, small Kaggle score differences may not be meaningful.
- **What figures, tables, examples, or other outputs could make the results understandable?**  
  A results table, a plot of fold-by-fold differences, and a few examples of changed predictions.

## Risks and backup scope

- **What could prevent the plan from working?**  
  The instructor may not approve Titanic, group features may leak labels, or the improvement may be too small to detect.
- **What smaller version would still let you answer a useful question if a risk occurs?**  
  Compare Pipeline 1 with a version that adds ticket-based group features only.
- **What evidence or feedback would lead you to pivot, and how would you document the reason?**  
  Instructor feedback, a rules conflict, or an unfixable leakage problem would lead us to pivot, possibly to Spaceship Titanic. We would record the reason in a dated README entry.

## Reproducibility and team contributions

- **Where will code, configuration, data instructions, and results live in the repository?**  
  Code in `src/`, configurations in `configs/`, data instructions in `data/README.md`, and results in `results/`.
- **What is the first small runnable result you aim to produce, and what will a reader need to reproduce it?**  
  A cross-validation result and valid submission from Pipeline 1. A reader will need the repository, `uv`, and a Kaggle account with the competition data.
- **How will team members contribute substantively and review one another's work?**  
  Tentatively, Eli and Corbin will handle the project-plan midpoint check and dataset and API exploration, and Jun will handle pipeline design and experiment testing. Members will review one another's pull requests.
- **What support, approval, or early instructor feedback would help you proceed?**  
  We need approval for Titanic and feedback on whether adding group features counts as a meaningfully different pipeline.

## Sources

- [Kaggle Titanic competition](https://www.kaggle.com/competitions/titanic): data and competition rules; license details to be confirmed.
- [scikit-learn documentation](https://scikit-learn.org/stable/): modeling and validation tools.
