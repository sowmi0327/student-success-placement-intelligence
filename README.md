# Student Success & Placement Intelligence

An interactive Power BI dashboard analyzing student academic performance, skill readiness, placement outcomes, and risk levels — built to help institutions identify at-risk students early and track placement readiness across a student population.

![Status](https://img.shields.io/badge/status-complete-brightgreen)
![Tool](https://img.shields.io/badge/tool-Power%20BI-yellow)
![Language](https://img.shields.io/badge/language-Python-blue)

---

## Problem Statement

Academic institutions often struggle to identify which students are at risk of poor placement outcomes until it's too late to intervene. This project builds a data-driven system that scores every student on academic strength, skill readiness, and placement risk — then surfaces those insights through an interactive dashboard.

## Objectives

- Analyze academic performance (CGPA, previous semester results, academic rating) across the student population
- Score students on skill readiness using communication, projects completed, and extracurricular involvement
- Classify every student into a Placement Readiness tier (LOW / MEDIUM / HIGH)
- Classify every student into a Risk tier (LOW / MEDIUM / HIGH) to flag who needs support
- Present all of this through a clean, interactive, multi-page Power BI dashboard

## Dataset

**Source:** [College Student Placement Factors Dataset](https://www.kaggle.com/datasets/sahilislam007/college-student-placement-factors-dataset) (Kaggle)

10,000 student records with the following raw columns:

| Column | Description |
|---|---|
| College_ID | College identifier (not unique per student) |
| IQ | Aptitude score |
| Prev_Sem_Result | Previous semester GPA |
| CGPA | Cumulative GPA |
| Academic_Performance | Rated academic performance (1–10) |
| Internship_Experience | Yes / No |
| Extra_Curricular_Score | Extracurricular activity score |
| Communication_Skills | Communication rating |
| Projects_Completed | Number of projects completed |
| Placement | Placed / Not Placed |

**Note:** Department, Gender, Attendance, Certifications, and Salary were not present in the source data and were intentionally **not fabricated**. This is documented as a future enhancement rather than filled with synthetic values.

## Tools & Technologies

- **Python** (Pandas) — data cleaning, feature engineering
- **Jupyter Notebook / Google Colab** — development environment
- **Power BI Desktop** — dashboard, DAX measures, visualization
- **Kaggle** — dataset source

## Workflow

```
Kaggle Dataset (CSV)
        │
        ▼
Python — Cleaning & Feature Engineering
  • Load & inspect data
  • Fix out-of-range CGPA values (capped at 10.0)
  • Create unique Student_ID
  • Engineer: Academic_Score, Skill_Score, Placement_Score,
    Placement_Readiness, Risk_Score, Risk_Level, Skill_Gap
  • Export → cleaned_student_data.csv
        │
        ▼
Power BI — Interactive Dashboard
  • Executive Overview
  • Academic Analytics
  • Placement Intelligence
  • Student Risk Analytics
```

## Feature Engineering

All raw scores are normalized to a 0–10 scale before being combined, so no single metric (e.g. IQ) unfairly dominates the result.

| Feature | Formula (weighted) |
|---|---|
| `Academic_Score` | CGPA (40%) + Prev_Sem_Result (30%) + Academic_Performance (30%) |
| `Skill_Score` | Communication (40%) + Projects (40%) + Extracurricular (20%) |
| `Placement_Score` | Academic_Score (40%) + Skill_Score (40%) + IQ (20%) |
| `Placement_Readiness` | Top 25% of Placement_Score → HIGH · middle 50% → MEDIUM · bottom 25% → LOW |
| `Risk_Score` | 10 − Placement_Score |
| `Risk_Level` | Percentile-banded, same logic as Placement_Readiness |
| `Skill_Gap` | Distance from an 8.0/10 "well-prepared" skill benchmark |

Readiness/Risk tiers use **percentile banding** rather than fixed cutoffs — this avoids the common problem where most students cluster into the middle bucket, keeping each tier meaningfully sized (~25% / 50% / 25%).

## Dashboard Pages

1. **Executive Overview** — Total students, average CGPA, placement-ready %, high-risk count, readiness & risk distribution
2. **Academic Analytics** — CGPA distribution, CGPA vs previous semester, academic performance breakdown
3. **Placement Intelligence** — Placement rate, placement by internship / academic performance / projects
4. **Student Risk Analytics** — Risk distribution, risk vs academic/skill score, student-level risk table with conditional formatting

All pages share synced slicers (`Placement_Readiness`, `Risk_Level`) and page-navigation buttons for smooth cross-page filtering.

## Repository Structure

```
student-success-placement-intelligence/
│
├── data/
│   ├── raw/
│   │   └── college_student_placement_dataset.csv
│   └── processed/
│       └── cleaned_student_data.csv
│
├── notebooks/
│   └── data_cleaning_feature_engineering.ipynb
│
├── powerbi/
│   └── student_success.pbix
│
├── docs/
│   ├── project_documentation.md
│   └── screenshots/
│       ├── executive_overview.png
│       ├── academic_analytics.png
│       ├── placement_intelligence.png
│       └── student_risk_analytics.png
│
├── README.md
└── .gitignore
```

## Key Insights

- Roughly a quarter of students fall into the HIGH placement-readiness tier, while a similar share are flagged HIGH risk — giving institutions a clear, actionable slice of the population to focus support on.
- Skill Score and Academic Score are both meaningful, independent drivers of placement risk — a student can be academically strong but still skill-gapped, or vice versa.
- Internship experience shows a visible association with placement outcomes on the Placement Intelligence page.

## Future Enhancements

- Incorporate Department and Gender for deeper cohort-level breakdowns
- Add real Attendance data to strengthen the risk model
- Include Certifications and Salary data if available in future dataset versions
- Explore a lightweight predictive model (e.g. logistic regression) for placement probability

## Author

**Sowmiya R**
Dr.N.G.P. Arts and Science College
Data Analytics Internship Project

---

*Dataset sourced from Kaggle. This project is for educational/internship purposes.*
