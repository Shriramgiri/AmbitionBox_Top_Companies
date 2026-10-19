# Top 1000 Companies Analysis

## Project Overview

This project analyzes the top 1,000 companies listed on **AmbitionBox** using web scraping, data cleaning, exploratory data analysis (EDA), and visualization.

The objective is to convert publicly available company listing information into a structured dataset and identify descriptive patterns across company ratings, review activity, salary-record counts, job listings, sectors, locations, and highly rated attributes.

**Workflow:** Web Scraping → Data Cleaning → Data Validation → EDA → Visualization → Insights

> **Important:** This is a descriptive exploratory analysis based on one scraped snapshot. It does not establish causal relationships or provide a definitive measure of company quality, employee satisfaction, compensation, or hiring success.

## Why This Project?

Coming from an **Economics background** and currently pursuing a **PGDM in Marketing**, this project was undertaken to bridge business understanding with practical data-analysis skills.

The goal was not simply to learn Python or create charts, but to work with a real-world, imperfect dataset and experience the complete analytics journey—from collecting raw information to interpreting business-relevant patterns.

## Objectives

1. How are company ratings distributed?
2. Which companies attract the most review activity?
3. How many jobs are listed per company?
4. Which sectors are most represented?
5. Which cities are most represented?
6. Do ratings relate to review, salary-record, or job counts?
7. What patterns emerge when multiple variables are analyzed together?

## Data Source

**Source:** [AmbitionBox](https://www.ambitionbox.com/)

The data was collected from AmbitionBox's public **list of companies** pages.

- Pages collected: **1–50**
- Company cards collected: **1,000**
- Company cards per page: **20**
- Final analytical dataset: **995 companies**

The dataset reflects the ordering and information available on AmbitionBox at the time of collection. It should not be interpreted as an independently validated ranking of India's top companies.

## Dataset Fields

| Field | Description |
|---|---|
| `name` | Company name |
| `rating` | Average company rating |
| `review` | Number of reviews |
| `salary` | Number of salary records shown on the company card |
| `job` | Number of job listings shown |
| `type` | Combined sector and location field in the raw data |
| `highly_rated` | Attribute identified as highly rated |
| `critically_rated` | Attribute identified as rated poorly; later dropped during EDA |

### Important Interpretation

The `salary` field **does not represent salary amount**. It represents the **number of salary records** shown on AmbitionBox.

Therefore, relationships involving `salary` should be interpreted as relationships involving **salary-record volume**, not employee compensation.

## Technology Stack

- **Python**
- **Pandas**
- **NumPy**
- **curl_cffi / requests**
- **BeautifulSoup**
- **Regular Expressions (`re`)**
- **Matplotlib**
- **Seaborn**

## Data Collection Workflow

1. Access AmbitionBox company listing pages.
2. Iterate through pages 1 to 50.
3. Locate company cards using the relevant HTML structure.
4. Extract company-level fields.
5. Handle missing HTML elements.
6. Store extracted information in a DataFrame.
7. Export the raw dataset to CSV.

### Fields Attempted During Scraping

- Company name
- Rating
- Review count
- Salary-record count
- Job count
- Sector
- Location
- Highly rated attribute

## Data Quality Challenges

The raw dataset contained several real-world data-quality issues:

### Counts stored as text

Examples included `1.2L`, `79.6k`, `4.1k`, and `--`.

### Messy text

Company names contained tabs and newline characters.

### Combined fields

Sector and location were initially contained within a single field.

### Missing values

Job information was missing for **144 rows**.

### Duplicate data

One exact duplicate row was identified and removed.

### Company-name extraction issue

The name extraction logic retained the first word in some cases, so multi-word company names can be represented incorrectly.

## Data Cleaning & Transformation

The following transformations were performed:

- Removed unnecessary index columns.
- Stripped unwanted whitespace.
- Cleaned company names.
- Extracted ratings using regular expressions.
- Split sector and location.
- Converted `L` values into numeric lakh values.
- Converted `k` values into numeric thousand values.
- Converted numeric fields to floating-point values.
- Removed exact duplicate rows.
- Handled missing values.
- Removed rows missing key analytical fields.

### Example Conversion

| Raw Value | Converted Value |
|---|---:|
| `1.2L` | `120000` |
| `79.6k` | `79600` |
| `10.5L` | `1050000` |
| `--` | `NaN` / treated as 0 for job analysis |

> **Caution:** Missing job values were treated as zero in the final analysis. This is an assumption because `--` may indicate unavailable information rather than zero openings.

## Final Dataset

After cleaning:

- Initial rows: **1,000**
- Final rows: **995**
- Numeric variables: **4** — rating, review count, salary-record count, and job count
- Categorical variables: company name, highly rated attribute, sector, and location
- Exact duplicates after cleaning: **0**
- Missing values after cleaning: **0**

The final dataset was considered ready for exploratory analysis, with the company-name extraction and job-value assumptions retained as limitations.

# Exploratory Data Analysis

The analysis covered four major areas:

### Descriptive Statistics

Summary statistics were calculated for numeric and categorical variables.

### Univariate Analysis

Individual distributions were examined for ratings, reviews, salary-record counts, job counts, sectors, locations, and highly rated attributes.

### Bivariate Analysis

Relationships were examined between rating, salary-record count, sector, and location.

### Multivariate Analysis

The project used correlation heatmaps, pivot heatmaps, scatter plots, and pair plots.

### Outlier Analysis

Boxplots were used to examine review counts, salary-record counts, job counts, and ratings.

# Key Findings

## 1. Ratings Are Tightly Grouped

- Mean rating: **3.78**
- Median rating: **3.8**
- Standard deviation: **0.34**
- **530 companies** fall between ratings of **3.5 and 4.0**

**Interpretation:** Ratings alone provide limited differentiation among companies in this sample.

## 2. Review Activity Is Highly Concentrated

- Median reviews: approximately **2.2k**
- Maximum reviews: approximately **120k**

A relatively small number of large companies account for a disproportionate amount of review activity.

## 3. Review Count and Salary-Record Count Have a Strong Relationship

The correlation between review count and salary-record count is approximately **0.91**.

This is more reasonably interpreted as a reflection of **company scale and visibility** than as a salary-quality relationship.

## 4. Rating Has Very Weak Relationships with Volume Metrics

| Relationship | Correlation |
|---|---:|
| Rating vs. Review | -0.05 |
| Rating vs. Salary-record count | -0.11 |
| Rating vs. Job count | -0.05 |

Larger companies do not automatically receive higher ratings.

## 5. IT Services & Consulting Is the Largest Sector

**IT Services & Consulting — 124 companies**

Other strongly represented sectors include Auto Components (53), NBFC (52), Pharma (51), Financial Services (40), and Banking (38).

## 6. Bengaluru and Mumbai Lead by Company Representation

| Location | Companies |
|---|---:|
| Bengaluru | 211 |
| Mumbai | 197 |
| Pune | 100 |
| Chennai | 85 |
| Hyderabad | 78 |
| Gurugram | 68 |

These figures describe the composition of this AmbitionBox sample, not job quality or salary competitiveness.

## 7. Promotions and Job Security Are Common Highly Rated Attributes

| Attribute | Companies |
|---|---:|
| Promotions | 289 |
| Job Security | 249 |
| Work-Life Balance | 200 |
| Culture | 113 |
| Salary | 86 |
| Skill Development | 51 |
| Satisfaction | 7 |

These are attributes identified as highly rated on the company cards, not numerical scores.

# Business Takeaways

1. **Ratings have limited differentiation:** Most companies are concentrated around the 3.5–4.0 range.
2. **Company activity is highly concentrated:** Large companies dominate review, salary-record, and job counts.
3. **Company scale strongly influences activity metrics:** Review and salary-record counts have a strong positive relationship.
4. **Company size does not equal company quality:** Rating has very weak relationships with review, salary-record, and job counts.

# Applications

The dataset can serve as a starting point for:

- Exploratory company comparison
- Sector analysis
- City/market representation analysis
- Identification of highly reviewed companies
- Initial HR analytics exploration
- Data-analysis and visualization practice

# Limitations

- **Snapshot limitation:** The analysis is based on a single scraped snapshot.
- **Selection bias:** The companies reflect AmbitionBox's listing/order and are not a complete census.
- **Company-name extraction:** Regex-based extraction can truncate multi-word names.
- **Job-value assumption:** Missing job values were treated as zero.
- **Salary interpretation:** Salary is a record count, not salary amount.
- **Causality:** Correlation does not imply causation.
- **Website dependency:** Changes to AmbitionBox's HTML structure could affect the scraper.
- **Collection date:** The collection date was not recorded.
- **Final-row reconciliation:** The dataset moves from 1,000 scraped records to 995 final records, while the available intermediate outputs do not fully trace every removed row.

# Future Improvements

1. Collect data periodically to create a time-series dataset.
2. Improve company-name extraction and entity matching.
3. Preserve a separate status for zero jobs vs. unavailable jobs.
4. Add actual compensation measures where legitimately available.
5. Track changes in ratings and review activity over time.
6. Compare sectors and cities across multiple time periods.
7. Build a more robust data-validation framework.
8. Maintain a detailed audit trail showing why every row is removed.
9. Add additional company-level variables where legitimately available.
10. Build an interactive dashboard for management users.

# Suggested Project Structure

```text
Top-1000-Companies-Analysis/
│
├── README.md
├── data/
│   ├── raw/
│   │   └── ambitionbox_top_companies.csv
│   └── processed/
│       └── cleaned_companies.csv
├── notebooks/
│   ├── 01_web_scraping.ipynb
│   ├── 02_eda.ipynb
│   └── 03_visualization.ipynb
├── visualizations/
└── presentation/
    └── AmbitionBox_Top_Companies_Presentation.pptx
```

# Reproduction

```bash
git clone <repository-url>
cd Top-1000-Companies-Analysis
pip install pandas numpy beautifulsoup4 curl_cffi requests matplotlib seaborn
```

Run the notebooks in sequence:

```text
01_web_scraping.ipynb
        ↓
02_eda.ipynb
        ↓
03_visualization.ipynb
```

> Update the filenames and commands if the repository uses a different structure.

# Skills Demonstrated

- Python
- Web scraping
- HTML parsing
- BeautifulSoup
- curl_cffi / requests
- Regular expressions
- Pandas
- NumPy
- Data cleaning
- Missing-value handling
- Duplicate detection
- Data transformation
- Exploratory Data Analysis
- Statistical interpretation
- Correlation analysis
- Outlier analysis
- Data visualization
- Business storytelling
- Analytical communication

# Conclusion

This project demonstrates an end-to-end approach to working with imperfect real-world data.

**Scrape → Clean → Explore → Visualize → Interpret**

The key learning is not just how to produce analytical results, but how to understand **what the data represents, what conclusions it supports, and where its limitations begin**.

# Author

**Shriram A. Giri**

- B.A. Economics — SRTMU, Nanded (2024)
- P.G.D.M. Marketing — MIT-SDE, Pune (Pursuing)
- Batch: 530

# Disclaimer

This project is intended for educational and exploratory analytical purposes.

The analysis is based on publicly available information from AmbitionBox and represents a specific scraped snapshot. It should not be interpreted as an official ranking of companies, a definitive measure of employee satisfaction, a compensation benchmark, or a causal study.

Before using the scraper or collected data in a production or commercial context, users should review the applicable website terms, permissions, and legal requirements.
