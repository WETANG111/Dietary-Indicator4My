# Dietary-Indicator4My Dataset

## Project Overview

This project analyzes dietary quality in a Randomized Controlled Trial (RCT) comparing two dietary intervention approaches: eating healthy foods ("What" group) versus eating at healthy times ("When" group). The study uses various dietary quality indices (e.g., DII, HEI) to assess changes from baseline to follow-up in both groups, followed by multivariable linear regression analysis adjusting for covariates.

## Related Literature Review
Latex Link: https://www.overleaf.com/3874187128qwbjvbpvpvrt#16c908
One Drive Link: https://liveswinburneeduau-my.sharepoint.com/:f:/g/personal/kjiang_swin_edu_au/Ejf8kHgZVOxCjv94SWzFlW8BPf4aT6NsPHJD4_snYCV4ow?e=WF3IxJ

## Study Design

- **Study Type**: Randomized Controlled Trial (RCT)
- **Time Points**: Baseline and Follow-up
- **Study Groups**: 
  - **What Group**: Focus on eating healthy foods (dietary quality intervention)
  - **When Group**: Focus on eating at healthy times (meal timing intervention)
- **Outcomes**: Multiple dietary quality indices
- **Statistical Methods**: Multivariable Linear Regression

## Dietary Quality Indices

This project uses the [`dietaryindex`](https://github.com/jamesjiadazhan/dietaryindex) R package to calculate the following dietary quality indices:

- **HEI-2020** (Healthy Eating Index-2020)
- **AHEI** (Alternative Healthy Eating Index)
- **DASH** (Dietary Approaches to Stop Hypertension)
- **aMED** (Alternate Mediterranean Diet Score)
- **DII** (Dietary Inflammation Index)
- **PHDI** (Planetary Health Diet Index)

### Citation

If you use this code, please cite:

> Zhan JJ, Hodge RA, Dunlop A, et al. Dietaryindex: A User-Friendly and Versatile R Package for Standardizing Dietary Pattern Analysis in Epidemiological and Clinical Studies. *Am J Clin Nutr*. 2024. doi: 10.1016/j.ajcnut.2024.08.021

## Project Structure

```
.
├── README.md                    # Project documentation
├── data/                        # Data directory
│   ├── raw/                     # Raw dietary data
│   ├── processed/               # Processed data
│   └── results/                 # Analysis results
├── scripts/                     # Analysis scripts
│   ├── 01_data_preparation.R    # Data preparation
│   ├── 02_calculate_indices.R   # Calculate dietary quality indices
│   ├── 03_descriptive_stats.R   # Descriptive statistics
│   └── 04_regression_analysis.R # Multivariable regression analysis
├── output/                      # Output directory
│   ├── tables/                  # Statistical tables
│   └── figures/                 # Figures and plots
└── docs/                        # Documentation
```

## Environment Setup

### Required R Packages

```r
# Install dietaryindex package
install.packages("devtools")
devtools::install_github("jamesjiadazhan/dietaryindex")

# Other required packages
install.packages(c(
  "tidyverse",    # Data manipulation
  "haven",        # Read SPSS/Stata data
  "readxl",       # Read Excel data
  "tableone",     # Create baseline characteristics table
  "gtsummary",    # Create statistical tables
  "ggplot2",      # Data visualization
  "car",          # ANOVA and regression diagnostics
  "lme4",         # Mixed-effects models (if needed)
  "emmeans"       # Estimated marginal means
))
```

### Load Packages

```r
library(dietaryindex)
library(tidyverse)
library(haven)
library(readxl)
library(tableone)
library(gtsummary)
library(car)
```

## Usage

### 1. Data Preparation

Ensure your dietary data includes:
- Subject ID
- Group (What/When)
  - **What Group**: Participants who received healthy food intervention
  - **When Group**: Participants who received meal timing intervention
- Time point (Baseline/Follow-up)
- Food intake or nutrient intake data
- Covariates (age, sex, BMI, etc.)

### 2. Calculate Dietary Indices

```r
# Example: Calculate HEI-2020
source("scripts/02_calculate_indices.R")

# Adjust calculation method based on your data format
# See dietaryindex package documentation for details
```

### 3. Descriptive Statistics

```r
# Calculate mean ± SD of dietary quality indices by group and time point
source("scripts/03_descriptive_stats.R")

# Generate baseline characteristics table (Table 1)
# Compare changes from baseline to follow-up
```

### 4. Multivariable Linear Regression

```r
# Regression analysis adjusting for covariates
source("scripts/04_regression_analysis.R")

# Model example:
# DII ~ Group * Time + Age + Sex + BMI + Education + Income
```

## Statistical Analysis Workflow

### Baseline Comparison

Compare dietary quality indices and covariates between What and When groups at baseline:
- t-test or Wilcoxon rank-sum test (continuous variables)
- Chi-square test (categorical variables)
- Ensure randomization was successful (no significant baseline differences)

### Within-group Changes

Assess changes in dietary quality from baseline to follow-up within each group:
- **What Group**: Expected to show improvement in dietary quality indices (HEI, AHEI, etc.)
- **When Group**: May show different patterns in dietary indices depending on meal timing effects
- Paired t-test or Wilcoxon signed-rank test
- Calculate effect sizes (Cohen's d)

### Between-group Comparison

Compare differences in dietary quality changes between What and When groups:
- Compare change scores (Follow-up - Baseline) between groups
- Independent t-test or Wilcoxon rank-sum test for change scores
- Repeated Measures ANOVA with Group × Time interaction
- Mixed-Effects Models

### Multivariable Regression

Assess intervention effects after adjusting for covariates:

```r
# Linear regression model comparing What vs When groups
# Using change score as outcome
model <- lm(DII_change ~ Group + Age + Sex + BMI + 
            Education + Income + Baseline_DII, 
            data = diet_data)

# View results
summary(model)

# Generate regression table
tbl_regression(model) %>%
  add_global_p() %>%
  bold_p() %>%
  bold_labels()

# Alternative: Using interaction term
model_interaction <- lm(DII ~ Group * Time + Age + Sex + BMI + 
                        Education + Income, 
                        data = diet_data_long)
```

## Key Variables

### Outcome Variables
- Absolute values of dietary quality indices at baseline and follow-up
- Change in dietary quality indices (Follow-up - Baseline)
- Expected patterns:
  - **What Group**: Should show improvement in overall diet quality (higher HEI, AHEI; lower DII)
  - **When Group**: May show changes depending on how meal timing affects food choices

### Main Independent Variable
- Group (What vs. When)
  - Reference group: What (healthy food) or When (meal timing) - specify in analysis

### Covariates
- Age
- Sex
- Body Mass Index (BMI)
- Education level
- Income level
- Baseline dietary quality index
- Other relevant variables

## Output

Analysis results will be saved in the `output/` directory:

### Tables
- **Table 1**: Baseline characteristics by group (What vs. When)
- **Table 2**: Descriptive statistics of dietary quality indices at baseline and follow-up
- **Table 3**: Within-group changes from baseline to follow-up
- **Table 4**: Between-group comparison of change scores (What vs. When)
- **Table 5**: Multivariable linear regression results

### Figures
- Bar plots or box plots comparing dietary quality indices between groups
- Trend plots showing changes from baseline to follow-up for each group
- Forest plots comparing effect sizes between What and When groups
- Regression diagnostic plots

## Research Questions

This analysis aims to answer:

1. **Within-group effects**: 
   - Does the "What" intervention improve dietary quality indices?
   - Does the "When" intervention affect dietary quality indices?

2. **Between-group comparison**:
   - Are there differential effects on dietary quality between focusing on "what to eat" vs "when to eat"?
   - Which approach leads to greater improvements in specific dietary indices (HEI, DII, etc.)?

3. **Covariate-adjusted effects**:
   - Do the intervention effects persist after adjusting for baseline characteristics and other covariates?

## Data Privacy

⚠️ **Important Note**:
- Do not upload raw data containing personal information
- All data files should be added to `.gitignore`
- Only share de-identified summary results

## References

1. Zhan JJ, Hodge RA, Dunlop A, et al. Dietaryindex: A User-Friendly and Versatile R Package for Standardizing Dietary Pattern Analysis in Epidemiological and Clinical Studies. *Am J Clin Nutr*. 2024. doi: 10.1016/j.ajcnut.2024.08.021

2. Shivappa N, Steck SE, Hurley TG, et al. Designing and developing a literature-derived, population-based dietary inflammatory index. *Public Health Nutr*. 2014;17(8):1689-1696.

3. Krebs-Smith SM, Pannucci TE, Subar AF, et al. Update of the Healthy Eating Index: HEI-2015. *J Acad Nutr Diet*. 2018;118(9):1591-1602.

4. Chiuve SE, Fung TT, Rimm EB, et al. Alternative dietary indices both strongly predict risk of chronic disease. *J Nutr*. 2012;142(6):1009-1018.

## Contributing

Contributions are welcome! Please feel free to submit issues or pull requests.

## License

This project is licensed under the MIT License. See the [LICENSE](LICENSE) file for details.

## Contact

For questions, please contact: [Your email]

---

**Last Updated**: 2025-11-01
