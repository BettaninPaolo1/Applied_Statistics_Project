# Beer Sales Analysis: Impact of Promotions and Product Behavior

This project analyzes weekly sales data of the 20 most sold beer products in Italian hypermarkets from February 2019 to February 2024. The objective is to understand the influence of different types of promotions on sales volume and to characterize the sales behavior of different products and brands.

## Dataset

The dataset originally included over 727,000 observations and 74 covariates, covering 4,589 beer types sold in hypermarkets and supermarkets. The analysis was restricted to the top 20 beers by sales volume in hypermarkets only (supermarkets were excluded due to incomplete data past 2021).

### Preprocessing
- Missing values were handled using a mix of interpolation and removal strategies.
- Outliers, especially for Peroni and Peroni Nastro Azzurro, were kept to preserve data integrity.
- Promotions were binarized: each promotion type was treated as a 0/1 indicator for its presence.
- Sales were log-transformed to stabilize variance and meet model assumptions.

## Research Questions

1. Do different types of promotions significantly impact sales volume?
2. Do different products and brands exhibit different sales behavior under promotions?

## Methodology

### 1. ANOVA and MANOVA
- MANOVA tests assessed whether both sales volume and discount percentage vary significantly by **product** and by **brand**.
- ANOVA was used to test if sales volume varies significantly across different **promotion types** (14 in total, including combinations).
- Findings showed significant differences across groups, justifying further modeling.

### 2. Clustering
- Performed **K-means clustering (k = 2)** to categorize products as **Leader** or **Follower** based on sales volume and value.
- Clustering was applied to:
  - Annual aggregated data
  - Full time-series data
  - Total aggregate over the full period
- The classification (Moretti, Heineken, and Ichnusa as leaders) was used as a covariate in regression models.

### 3. Regression Analysis
- A log-linear regression model was constructed using:
  - Discount types: Flyer, Display, Price Reduction
  - Pricing: Discounted and Non-discounted prices (log-transformed)
  - Seasonal indicators: Summer, Holiday
  - Cluster indicator: Leader/Follower
  - Outlier flag: Low volume weeks
- Final model achieved an adjusted R² of approximately 65%.
- Residual analysis revealed non-normality and heteroskedasticity.

### 4. Linear Mixed Models (LMM)
- Two LMMs were tested: one with random effects by **product**, one by **brand**.
- The **LMM by product** was selected as best based on:
  - ANOVA comparison
  - AIC scores
  - Pinball loss on test set
- Predictions were made on the test period (last 20%), focusing on the best-selling product (Moretti 66cl).

## Key Findings

- Promotions significantly increase sales, with **Price Reduction** being the most effective type.
- Leader products consistently outperform Followers in terms of volume.
- Seasonal effects (e.g., summer) positively influence beer sales.
- Discounted prices are negatively correlated with sales (as expected), while non-discounted prices show a positive effect likely due to brand strength and product positioning.
- Random effects reveal product-specific sensitivity to promotions, especially for brands like Corona and Dreher.

## Scripts

- `Preprocessing.R`: Data cleaning and filtering
- `ANOVA.R`: ANOVA and MANOVA analysis
- `Cluster.R`: Clustering of products into Leaders and Followers
- `Regression.R`: Linear regression and LMM modeling
- `Analisi_esplorativa.R`: Exploratory plots and data summaries
