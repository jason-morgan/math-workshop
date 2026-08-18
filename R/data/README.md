# Dataset Manifest & Documentation

This directory contains the datasets used in the R programming modules (`01-R-Intro.Rmd`, `02-R-Basics.Rmd`, and `03-R-Analysis.Rmd`) for the Ohio State University Political Science Graduate Math Workshop.

---

## Quick Reference Summary

| File | Format | Dimensions | Topic / Context | Used In |
| :--- | :--- | :--- | :--- | :--- |
| **`Latin America legislator data.tab`** | Tab-separated (`.tab`) | 1,569 rows $\times$ 89 cols | Latin American legislative survey & demographic data | `02-R-Basics.Rmd` |
| **`values.csv`** | Space-separated (`.csv`) | 1,122 rows $\times$ 21 cols | Colombian municipality nighttime light luminosity (1993–2013) | `02-R-Basics.Rmd` |
| **`LeedsJohnsonJOPrep.dta.zip`** | Stata dataset (`.dta` in ZIP) | 1,000,000+ directed dyad-years | Military alliances and conflict initiation (Leeds & Johnson 2011) | `03-R-Analysis.Rmd` |

---

## Detailed Dataset Descriptions

### 1. Latin American Legislator Dataset
* **Filename:** `Latin America legislator data.tab`
* **File Format:** Tab-separated text file (TSV / `.tab`)
* **Observations ($N$):** 1,569 legislators
* **Variables ($K$):** 89 variables
* **Substantive Description:** 
  A comprehensive survey and demographic dataset of Latin American parliamentarians (e.g., from the Latin American Elites Project / PELA or parliamentary roll-call records). Variables capture legislators' individual attributes, socioeconomic backgrounds, ideological positioning, and policy attitudes across multiple sectors.
* **Key Variables:**
  * `pais`: Numeric country identifier (e.g., Argentina, Colombia, Chile).
  * `age`: Age in years.
  * `gender_1`, `gender_2`: Binary indicators for gender.
  * `marital_*`: Marital status indicators.
  * `education_*`: Educational attainment levels.
  * `denomination_*`, `churchattend_*`: Religious denomination and frequency of church attendance.
  * `occupation_*`, `occ1`, `occ2`, `parent_*`: Occupational background and parental occupation.
  * `urbanization`: Urbanization level of the constituency / region.
  * `party_ideology`: Left-Right ideological self-placement scale.
  * `inf_scale`, `health_scale`, `safety_scale`, `ed_scale`, `unemp_scale`, `housing_scale`, `pensions_scale`: Standardized policy scale metrics for public investments (infrastructure, health, public security, education, employment, housing, pensions).
* **Tutorial Usage:** 
  * Used in **`02-R-Basics.Rmd`** (Sections: *Data Frames* and *Functions*).
  * Demonstrates tabular data loading with `read.table()`, data frame indexing (`df[row, col]`), extracting columns via the `$` operator (`df$urbanization`), and computing summary statistics.
* **Loading in R:**
  ```r
  df <- read.table("data/Latin America legislator data.tab", 
                   sep = "\t", 
                   header = TRUE)
  ```

---

### 2. Colombian Municipality Nighttime Lights Dataset
* **Filename:** `values.csv`
* **File Format:** Space-separated CSV text file
* **Observations ($N$):** 1,122 Colombian municipalities
* **Variables ($K$):** 21 annual measurement columns (`X1993` through `X2013`)
* **Substantive Description:** 
  Satellite-measured nighttime light luminosity data for 1,122 municipalities in Colombia recorded annually over a 21-year period (1993–2013). In empirical political science and economics, satellite night lights are widely used as a high-resolution proxy for local economic activity, urbanization, infrastructure development, and state capacity.
* **Structure:**
  Each row represents a specific Colombian municipality. The 21 columns (`X1993`, `X1994`, ..., `X2013`) record the average luminosity value for each year.
* **Tutorial Usage:**
  * Used in **`02-R-Basics.Rmd`** (Section: *Looping and Iteration*).
  * Demonstrates the need for automation: calculating annual municipality means manually vs. using `for` loops, `apply()` family functions, and vectorized row operations (`rowMeans()`).
* **Loading in R:**
  ```r
  values <- read.csv("data/values.csv", 
                     sep = "", 
                     stringsAsFactors = FALSE)
  ```

---

### 3. Alliance and Conflict Replication Dataset (Leeds & Johnson 2011)
* **Filename:** `LeedsJohnsonJOPrep.dta.zip` (contains `LeedsJohnsonJOPrep.dta`)
* **File Format:** Stata `.dta` file (uncompressed size ~248 MB; compressed ~22.5 MB)
* **Source & Scholarly Context:** 
  Replication dataset from Jesse C. Johnson and Brett Ashley Leeds (2011), *"Defending the Target: Alliances, MIDs, and Interstate Conflict"*, studying the deterrence and empowerment effects of military alliances on Militarized Interstate Disputes (MIDs) between 1816 and 2000.
* **Observations ($N$):** Over 1,000,000 directed dyad-years (all pairs of interacting states observed annually).
* **Key Variables:**
  * `challenger`: Correlates of War (COW) numeric country code for the potential initiating/challenging state.
  * `target`: Correlates of War (COW) numeric country code for the target state.
  * `year`: Calendar year (1816–2000).
  * `dispute`: Binary indicator ($1 = \text{Militarized Interstate Dispute initiated}, 0 = \text{No dispute}$).
  * `ptargdef`: Target defensive alliance commitment ($1 = \text{Target has a defensive alliance}, 0 = \text{No}$).
  * `pchaloff`: Challenger offensive alliance commitment ($1 = \text{Challenger has an offensive alliance}, 0 = \text{No}$).
  * `capprop`: Capability proportion (Challenger's probability of winning based on military capabilities).
  * `ln_distance`: Natural logarithm of the distance in kilometers between capitals.
  * `s_un_glo`: Global alliance portfolio similarity index ($S$-score / UN voting affinity).
  * `jdem`: Joint democracy indicator ($1 = \text{Both states are democracies}, 0 = \text{Otherwise}$).
* **Tutorial Usage:**
  * Used in **`03-R-Analysis.Rmd`** (Comprehensive Exploratory Data Analysis tutorial).
  * Demonstrates large dataset ingestion via `rio` or `haven`, logical subsetting (filtering to post-WWII: `year > 1945`), data cleaning, univariate visualizations (histograms and density plots using `ggplot2`), multi-panel layouts (`Rmisc::multiplot`), annual time series trend aggregation (`for` loops), $2 \times 2$ contingency tables, difference-in-means $t$-tests, correlation tests, and multivariate logistic regression (`glm(..., family="binomial")` and regression table formatting with `texreg`).
* **Loading in R:**
  ```r
  # If unzipped:
  dat <- rio::import("data/LeedsJohnsonJOPrep.dta")
  # Or using haven:
  # dat <- haven::read_dta("data/LeedsJohnsonJOPrep.dta")
  ```

---

## Working with Large Datasets: Best Practices

1. **Relative File Paths:**
   Always use relative paths from your working directory or R project root (e.g., `"data/values.csv"` or `here::here("R", "data", "values.csv")`) rather than machine-specific absolute paths (such as `"~/code/..."` or `"C:/Users/..."`).
2. **Memory Management (`LeedsJohnsonJOPrep.dta`):**
   Because `LeedsJohnsonJOPrep.dta` contains over one million rows:
   * Subset the dataset immediately after importing if you only need a specific time period (e.g., `postWW2 <- dat[dat$year > 1945, ]`).
   * Delete large raw objects when finished with them: `rm(dat)` followed by `gc()` (garbage collection) to free RAM.
