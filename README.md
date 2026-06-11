# 🎬 Movie Genre Data Analysis — Python & Pandas

An exploratory data analysis project using Python and Pandas to investigate IMDB movie data across 10,800+ films (1960–2015), uncovering which genres dominate in quantity, revenue, profit, popularity, and audience ratings.

---

## 📌 Project Overview

This project dives deep into movie genre performance using a real-world IMDB dataset. Through data cleaning, feature engineering, genre splitting, correlation analysis, and visualization, the project answers four core research questions and tests four business hypotheses about what drives a movie's financial and critical success.

---

## 📁 Repository Structure

```
├── imdb_movies.csv                      # Raw IMDB movie dataset
└── 2__Movie_Genre_Data_Analysis.ipynb   # Full analysis notebook
```

---

## 🗄️ Dataset

**Source:** IMDB Movies Dataset — 10,866 rows × 21 columns (1960–2015)

| Field | Description |
|---|---|
| `id` | Unique movie ID |
| `imdb_id` | IMDB identifier |
| `original_title` | Movie title |
| `genres` | Pipe-separated genre list (e.g. `Action\|Adventure\|Thriller`) |
| `release_date` / `release_year` | Release date and year |
| `budget` | Production budget (USD, up to $425M) |
| `revenue` | Box office revenue (USD, up to $2.78B) |
| `popularity` | IMDB popularity score |
| `vote_average` | Audience vote average (1.5 – 9.2) |
| `vote_count` | Number of votes |
| `runtime` | Movie runtime (minutes) |
| `cast` | Main cast members |
| `director` | Director name |
| `production_companies` | Production companies |
| `budget_adj` / `revenue_adj` | Inflation-adjusted budget and revenue |

---

## 🧹 Part 1 — Data Cleaning & Preparation

### Steps performed:

**1. Loading & Inspecting the Data**
- Loaded 10,866 rows using `pd.read_csv()`
- Explored structure with `movies.info()` and `movies.head()`
- Configured display options for full visibility: `pd.set_option('display.max.rows', 11000)`

**2. Removing Duplicates & Nulls**
- Dropped duplicate rows with `drop_duplicates(inplace=True)`
- Dropped rows with missing `genres` values using `dropna(subset=['genres'])`

**3. Feature Engineering — Profit Column**
- Created a new `profit` column: `movies['revenue'] - movies['budget']`
- Enables direct profitability comparison across genres and films

**4. Column Selection**
- Narrowed to 10 analysis-relevant columns: `popularity`, `budget`, `revenue`, `original_title`, `runtime`, `genres`, `release_year`, `vote_count`, `vote_average`, `profit`

**5. Genre Splitting (Multi-Label Expansion)**
- Movies have multiple genres separated by `|` — exploded each into its own row using `str.split('|').apply(Series).stack()`
- Reset the multi-index using `droplevel` and `join` to preserve all original columns
- This enabled genre-level aggregation across the full dataset

---

## 🔍 Part 2 — Research Questions

### Q1: Which genres are the most common?
- Counted unique movies per genre using `groupby('genres_split').original_title.nunique()`
- Visualized with a **pie chart** (% share) and a **horizontal bar chart**
- Revealed which genres dominate movie production by volume

### Q2: Which genres have the highest average budget and revenue?
- Calculated `genres_avg = movies_genre.groupby('genres_split').mean()`
- Sorted by revenue and plotted a **dual bar chart** (budget vs. revenue by genre)
- Identified which genres attract the largest financial investment and returns

### Q2.5: Which genres are most profitable?
- Sorted `genres_avg` by `profit` and plotted a **horizontal bar chart**
- Separated raw revenue performance from actual profitability

### Q3: Which genres have the highest average popularity?
- Sorted by `popularity` score and visualized with a **horizontal bar chart**
- Revealed audience engagement patterns independent of financial metrics

### Q4: Which genres have the most movies rated ≥ 8.0?
- Filtered movies with `vote_average >= 8` (two cuts: all movies and those with `vote_count >= 50` for statistical reliability)
- Counted qualifying movies per genre and plotted a **horizontal bar chart**
- Identified critically acclaimed genres vs. mass-market genres

---

## 🔬 Part 3 — Hypothesis Testing

All correlations computed using **Spearman's method** (robust to outliers, suited for non-normal distributions), filtered to movies with `vote_count >= 50` for statistical reliability.

| Hypothesis | Method | Result |
|---|---|---|
| H1: High vote average → high revenue & profit | Spearman correlation + regression plot | Tested |
| H2: High popularity → high revenue & profit | Spearman correlation + regression plot | Tested |
| H3: High budget → high profit | Spearman correlation + regression plot | Tested |
| H4: High budget → high popularity | Spearman correlation + regression plot | Tested |

Each hypothesis visualized with `sns.regplot()` showing the regression line in red.

### Bonus: Profit Per Genre Per Year
- Built a pivot table: `genres_split` × `release_year` with mean profit values
- Visualized as a **heatmap** using `sns.heatmap(cmap='YlGnBu')` to reveal genre profitability trends over 55 years

---

## 🛠️ Tools & Technologies

- **Language:** Python 3
- **Libraries:** Pandas, NumPy, Matplotlib, Seaborn
- **Environment:** Jupyter Notebook
- **Techniques:** Multi-label genre expansion, `groupby` aggregation, pivot tables, Spearman correlation, regression plots, heatmaps, feature engineering, data filtering

---

## 💡 Key Findings

- Dataset covers **10,800+ movies** spanning **55 years** (1960–2015) across multiple genres
- Budgets range from **$0 to $425M** and revenues from **$0 to $2.78B** — massive variance across genres
- Genre splitting revealed multi-genre films dominate; most movies belong to 2–4 genres simultaneously
- Audience vote averages range from **1.5 to 9.2**, showing significant quality spread across genres
- Spearman correlations tested whether financial success, popularity, and critical acclaim are actually linked — or independent of each other
- The profit-per-genre heatmap reveals how genre profitability has shifted dramatically over decades

---

## 🚀 Getting Started

1. Clone this repository
2. Install dependencies:
   ```bash
   pip install pandas matplotlib seaborn jupyter
   ```
3. Launch Jupyter Notebook:
   ```bash
   jupyter notebook 2__Movie_Genre_Data_Analysis.ipynb
   ```
4. Ensure `imdb_movies.csv` is in the same directory as the notebook

> Tested with **Python 3.8+** and **Pandas 1.3+**

---

## 📄 License

This project is open source and available under the [MIT License](LICENSE).
