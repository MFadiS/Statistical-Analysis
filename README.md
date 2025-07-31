# Statistical Analysis: Data Analysis and Regression Modeling

## Table of Contents
- [Introduction](#introduction)
- [Project Overview](#project-overview)
- [Question 1: Bike Hires Analysis](#question-1-bike-hires-analysis)
    - [Objective](#objective)
    - [Data Understanding & Preparation](#data-understanding--preparation)
    - [Correlation Analysis](#correlation-analysis)
    - [Regression Analysis](#regression-analysis)
    - [Key Findings (Bike Hires)](#key-findings-bike-hires)
- [Question 2: Book Sales Analysis](#question-2-book-sales-analysis)
    - [Objective](#objective-1)
    - [Data Understanding & Preparation](#data-understanding--preparation-1)
    - [Correlation Analysis](#correlation-analysis-1)
    - [Regression Analysis](#regression-analysis-1)
    - [Key Findings (Book Sales)](#key-findings-book-sales)
- [Conclusion](#conclusion)
- [How to Interpret the Findings](#how-to-interpret-the-findings)

## Introduction
This repository presents the findings and methodologies employed in a comprehensive Business Statistics End of Term project. The project delves into various statistical analyses, primarily focusing on regression modeling to understand the impact of different factors on key business metrics. It showcases data cleaning, exploratory data analysis, correlation projects, and the application of multiple linear regression models, including the examination of interaction effects and model comparisons using ANOVA.

## Project Overview
The project is structured around two main questions, each addressing a distinct business problem with its own dataset and analytical goals:
1.  **Understanding Bike Hires:** Analyzing factors influencing the number of bikes hired in London, including the effects of COVID-19 related restrictions and temporal trends.
2.  **Predicting Book Sales:** Investigating how average reviews, total reviews, sale price, and genre impact daily book sales.

This project highlights the application of statistical techniques to derive actionable insights from real-world (or simulated real-world) data.

## Question 1: Bike Hires Analysis

### Objective
To understand the effect of various factors, including work-from-home policies, "Rule of 6 indoors," "Eat Out to Help Out" scheme, and temporal variables (month, year, day), on the number of bikes hired.

### Data Understanding & Preparation
* **Dataset:** `London_COVID_bikes.csv` containing daily bike hire data from 2010-2023, along with binary indicators for various COVID-19 related restrictions and temporal information.
* **Initial Checks:** Verified data structure (`str()`) and summary statistics (`summary()`).
* **Outlier Handling:**
    * Identified and removed two instances where `Hires` was 0, attributed to a scheme shutdown on specific dates in September 2022.
    * Confirmed that high `Hires` values (e.g., above 60,000) were not outliers and were retained for analysis.
* **Data Transformation:** Converted categorical variables (`day`, `month`, `year`, and all binary policy indicators) to factors for proper regression analysis.
* **Visualization for Understanding:** Used histograms (`ggplot`) to check the distribution of `Hires` (approximating normal distribution) and bar plots (`grid.arrange`) to visualize the frequency distribution of binary policy variables across years, highlighting data imbalance.
* **Baseline Adjustment:** Set the baseline category for the `year` factor to "2016" for regression models, as 2010 exhibited an atypical pattern.

### Correlation Analysis
* **Method:** Pearson Correlation was used, given the visual approximation of a normal distribution for the `Hires` variable.
* **Key Findings:**
    * Weak positive correlation (0.13) between `Hires` and `wfh` (work from home).
    * Moderate positive correlation (0.23) between `Hires` and `rule_of_6_indoors`.
    * Weak positive correlation (0.08) between `Hires` and `eat_out_to_help_out`.
    * All correlations were statistically significant (p-values close to 0).
    * Checked for multicollinearity between predictors (`wfh`, `rule_of_6_indoors`, `eat_out_to_help_out`) using Variance Inflation Factor (VIF) scores, confirming low VIFs (less than 5), justifying their use together in multiple regression.

### Regression Analysis
Multiple linear regression models were built to assess the impact of various predictors on `Hires`.
* **Simple Regression Models:**
    * `Hires ~ wfh`: Significant positive effect, predicting an average increase of 1820 bikes hired when WFH is encouraged.
    * `Hires ~ rule_of_6_indoors`: Significant positive effect, predicting an average increase of 9298 bikes hired when the rule is imposed.
    * `Hires ~ eat_out_to_help_out`: Significant positive effect, predicting an average increase of 9857 bikes hired when the scheme is offered.
* **Multiple Regression Models:**
    * `Hires ~ wfh + rule_of_6_indoors + eat_out_to_help_out`: All three predictors showed a significant positive impact on bike hires. VIF scores confirmed no multicollinearity.
    * `Hires ~ year`: Years 2010, 2011, 2012, 2013, 2021, 2022, and 2023 had a significant impact on `Hires`.
    * `Hires ~ month + year`: All months (except December) and several years had a significant impact.
    * `Hires ~ day + month + year`: All days of the week significantly impacted `Hires` (Monday, Saturday, Sunday having a negative impact, others positive), along with most months and significant years.
* **ANOVA Tests for Model Comparison:**
    * Compared `Hires ~ year` vs. `Hires ~ month + year`: Using both month and year significantly improved model fit ($F(11, 4785) = 324.66, p<.001$), with a substantial increase in R-squared (0.16 to 0.52).
    * Compared `Hires ~ month + year` vs. `Hires ~ day + month + year`: Adding `day` also significantly improved model fit ($F(6, 4779) = 88.283, p<.001$).

### Key Findings (Bike Hires)
The analysis revealed that various factors, including work-from-home policies, the "Rule of 6 Indoors," and the "Eat Out to Help Out" scheme, had a statistically significant positive impact on bike hires. Temporal factors like month, year, and day of the week also played a crucial role, with distinct seasonal patterns (peak in summer) and weekly trends (higher demand on weekdays). The comprehensive multiple regression model incorporating day, month, year, and policy indicators provided the most robust fit, explaining a significant portion of the variance in bike hires.

---

## Question 2: Book Sales Analysis

### Objective
To understand the effect of average reviews, total number of reviews, sale price, and genre on daily book sales.

### Data Understanding & Preparation
* **Dataset:** Publisher sales data.
* **Initial Checks:** Checked data structure and summary.
* **Outlier Handling:** Identified and removed one observation with negative daily sales, as it was not technically correct.
* **Data Transformation:** Converted categorical variables (`sold by`, `publisher.type`, `genre`) to factors.
* **Visualization for Understanding:** Used histograms (`ggplot`) to visualize the distributions of `daily.sales`, `avg.review`, `total.reviews`, and `sale.price`, noting non-normal distributions for some.

### Correlation Analysis
* **Method:** Spearman correlation was used due to the non-normal distribution of some variables.
* **Key Findings:**
    * Weak correlation between `avg_review` and `daily_sales` (0.01).
    * Strong positive correlation (0.68) between `daily_sales` and `total_reviews`.
    * Moderate negative correlation (-0.52) between `daily_sales` and `sale_price`.
    * Moderate negative correlation (-0.53) between `total_reviews` and `sale_price`.

### Regression Analysis
Linear regression models were constructed to examine the relationships between sales and various predictors.
* **Simple Regression Models:**
    * `daily_sales ~ avg_review`: No significant effect.
    * `daily_sales ~ total_reviews`: Significant positive effect ($b=0.53, p<.001$), indicating that more total reviews are associated with higher daily sales.
    * `daily_sales ~ sale_price`: Significant negative effect ($b=-3.98, p<.001$), indicating higher sale prices are associated with lower daily sales.
* **Multiple Regression Models:**
    * `daily_sales ~ avg_review + total_reviews`: Both `avg_review` (negative effect) and `total_reviews` (positive effect) were significant.
    * `daily_sales ~ avg_review * total_reviews` (Interaction Model):
        * Significant interaction effect ($b=0.096, p<.001$).
        * Interpretation: When total reviews exceed 160, books with higher average reviews tend to have higher daily sales. Conversely, when total reviews are below 150, books with lower average reviews tend to achieve higher daily sales on average.
    * `daily_sales ~ sale_price + genre`: Both `sale_price` (negative effect) and `genre` (non-fiction negative, YA fiction positive, relative to adult fiction baseline) had significant effects.
    * `daily_sales ~ sale_price * genre` (Interaction Model):
        * Significant interaction effect between `sale_price` and `genreYA_fiction` (negative effect).
        * No significant interaction effect between `sale_price` and `genrenon_fiction`.
        * Interpretation: The impact of sale price on daily sales varies across genres. YA fiction sales are more negatively affected by price increases compared to adult fiction. Non-fiction sales are relatively insensitive to price changes.
* **ANOVA Tests for Model Comparison:**
    * Compared `daily_sales ~ total_reviews` vs. `daily_sales ~ avg_review + total_reviews`: Adding `avg_review` significantly improved model fit ($F(1, 5997) = 69.92, p<.001$).
    * Compared `daily_sales ~ avg_review + total_reviews` vs. `daily_sales ~ avg_review * total_reviews`: The interaction term significantly improved model fit ($F(1, 5996) = 149.19, p<.001$).
    * Compared `daily_sales ~ sale_price` vs. `daily_sales ~ sale_price + genre`: Adding `genre` significantly improved model fit ($F(2, 5995) = 1168.9, p<.001$).
    * Compared `daily_sales ~ sale_price + genre` vs. `daily_sales ~ sale_price * genre`: The interaction term significantly improved model fit ($F(2, 5993) = 51.24, p<.001$).

### Key Findings (Book Sales)
Book sales are strongly influenced by the total number of reviews, with higher reviews generally leading to more sales. While average reviews alone had a less direct impact, their interaction with total reviews revealed nuanced effects: high-review books perform better with many reviews, while low-review books might perform better with fewer reviews. Sale price generally has a negative impact on sales, but this effect varies significantly by genre. YA fiction is highly price-sensitive, adult fiction moderately so, and non-fiction sales are relatively stable regardless of price changes.

## Conclusion
This business statistics project successfully applied various analytical techniques, including data cleaning, correlation analysis, and multiple linear regression with interaction terms, to two distinct datasets. The project effectively identified key drivers for bike hires (COVID-19 policies, temporal trends) and book sales (reviews, pricing, genre), demonstrating a strong understanding of predictive algorithms and their application in deriving actionable business insights. The use of ANOVA tests confirmed the statistical significance of adding predictors and interaction terms, leading to more robust and explanatory models.

## How to Interpret the Findings
The conclusions are based on the observed trends within the provided datasets and their corresponding statistical analyses. To fully understand the context, detailed R code, and specific data points leading to these conclusions, please refer to the complete project document.
