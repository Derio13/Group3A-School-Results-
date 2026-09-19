
Eben Derio, [9/18/2026 11:23 PM]
rm scope
git status

Eben Derio, [9/19/2026 12:43 AM]
# DATA QUALITY ASSESSMENT REPORT
Group 3a — School Results Analysis  
Week 2: SQL Data Profiling  

Project Question: Which subjects show the weakest results, and how does attendance relate to score across terms?

---

## 1. Executive Summary
The Week 2 profiling exercise assessed the reliability of the raw school dataset before cleaning, modelling, and dashboard development. The review confirmed several data-quality issues that could materially affect analysis if left unresolved.

| Key Issue | Finding |
| :--- | :--- |
| Missing scores | 1,507 rows |
| Invalid scores | 55 rows outside 0–100 |
| Duplicate results | 120 extra rows |
| Orphan student references | 76 rows |
| Missing student regions | 84 rows |
| Mixed date formats | 4 formats in both date fields |
| Category inconsistencies | term, sex, and region |

> Presentation Takeaway: The raw data is usable, but not yet trustworthy enough for final reporting. Week 3 cleaning must resolve the identified issues before the Power BI model is built.

---

## 2. Source Tables and Row Counts

| Table | Rows |
| :--- | :--- |
| results | 30,120 |
| students | 1,200 |
| subjects | 10 |
| teachers | 50 |

---

## 3. Results Table — Key Findings

### 3.1 Missing and Invalid Scores

| Metric | Result |
| :--- | :--- |
| Missing score values | 1,507 |
| Minimum valid/observed score | 0 |
| Maximum observed score | 139.9 |
| Invalid score rows | 55 |
| Invalid attendance rows | 0 |

* Why this matters: Scores drive the core performance analysis. Missing or impossible scores would distort subject averages and weaken the reliability of the final dashboard.

### 3.2 Duplicate Records
Duplicate profiling identified 120 duplicate groups, equal to 120 extra result rows. These records should be deduplicated before calculating final counts and averages.

### 3.3 Orphan Keys

| Relationship | Orphan Rows |
| :--- | :--- |
| results $\rightarrow$ students | 76 |
| results $\rightarrow$ subjects | 0 |
| results $\rightarrow$ teachers | 0 |

* Model Impact: The 76 unmatched student references would break the student relationship in a star schema unless they are investigated and handled during cleaning.

### 3.4 Inconsistent Term Values
The term field contains 12 stored variants representing only three real terms. Examples include term 1, Term 1, TERM 1, and similar variants for Terms 2 and 3.

---

## 4. Date Quality

### 4.1 exam_date

| Format | Rows |
| :--- | :--- |
| YYYY-MM-DD | 24,696 |
| DD-Mon-YYYY | 1,826 |
| YYYY/MM/DD | 1,811 |
| DD/MM/YYYY or MM/DD/YYYY | 1,787 |

### 4.2 enrolled_date

| Format | Rows |
| :--- | :--- |
| YYYY-MM-DD | 1,004 |
| DD/MM/YYYY or MM/DD/YYYY | 68 |
| YYYY/MM/DD | 66 |
| DD-Mon-YYYY | 62 |

* Cleaning Requirement: Both date fields are stored as text and must be parsed into one consistent date type. Slash-formatted dates require a clearly documented day/month rule.

---

## 5. Students Table — Key Findings

### 5.1 Category Standardisation
The sex field contains 8 stored variants representing two real categories (e.g., female, Female, FEMALE, male, Male, MALE`). `region values also appear with inconsistent capitalization and spacing (e.g., ashanti`/`Ashanti`/`ASHANTI and `bono`/`Bono`/`BONO`).

### 5.2 Missing Values

| Field | Missing Values |
| :--- | :--- |
| student_name | 0 |
| sex | 0 |
| year_group | 0 |
| region | 84 |
| enrolled_date | 0 |

* No duplicate student groups were identified using student_name, sex, year_group, region, and enrolled_date.

---

## 6. Preliminary Analytical Findings
*Note: These findings are based on raw data and are therefore provisional. They should be repeated after Week 3 cleaning.*

### 6.1 Weakest Subjects by Average Valid Score

| Rank | Subject | Average Score |
| :---: | :--- | :---: |
| 1 | Integrated Science | 57.19 |
| 2 | Career Technology | 57.37 |
| 3 | French | 57.72 |
| 4 | Religious & Moral Education | 57.79 |
| 5 | Ghanaian Language | 57.79 |

* Interpretation: Integrated Science currently has the lowest average valid score.

Eben Derio, [9/19/2026 12:43 AM]
However, subject averages are close together, so the final presentation should avoid overstating the differences.

### 6.2 Attendance and Score Across Terms

| Term | Avg. Score | Avg. Attendance | Correlation |
| :--- | :---: | :---: | :---: |
| Term 1 | 57.81 | 86.37% | -0.009 |
| Term 2 | 57.99 | 86.37% | 0.002 |
| Term 3 | 57.81 | 86.26% | 0.000 |

* Interpretation: The raw data shows virtually no linear relationship between attendance percentage and exam score across the three terms. This is a relationship finding only; it does not establish causation.

---

## 7. Week 3 Cleaning Priorities
1. Convert exam_date and enrolled_date to true date fields.
2. Standardize term, sex, and region labels.
3. Handle 1,507 missing scores using a documented rule.
4. Flag or remove 55 invalid score values outside 0–100.
5. Remove 120 extra duplicate result rows.
6. Investigate 76 orphan student references.
7. Handle 84 missing region values appropriately.
8. Re-run all validation checks after cleaning.
9. Write cleaned tables to the group3a schema.
10. Re-run the core analysis using cleaned data.

---

## 8. Conclusion
The profiling stage confirmed that the school dataset contains meaningful quality issues that could distort the final analysis if left untreated. The next phase will focus on producing cleaned, validated tables that can support a reliable star schema and a clear dashboard for the head teacher. All current performance findings should remain labelled as preliminary until they are reproduced using the cleaned data.
