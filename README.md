# Market Sentiment & Macro Geopolitical Signal Prediction
Built an end-to-end quantitative pipeline combining FinBERT-based news sentiment and GDELT macro geopolitical indicators to study short-horizon equity market direction using time-series machine learning.

OBJECTIVE: This project builds an end-to-end quantitative research pipeline to examine whether financial news sentiment and macro–geopolitical signals contain predictive information about short-term equity market movements.

It combines large-scale news sentiment analysis (FinBERT, VADER, TextBlob) with geopolitical event data (GDELT) and applies time-series aware machine learning models to predict market direction under different volatility and regime conditions.

----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

PROJECT HIGHLIGHTS:

Built a multi-stage pipeline covering:

1. News ingestion (Yahoo Finance RSS, S&P 500 constituents)
2. Transformer-based sentiment modeling (FinBERT)
3. Market return alignment (IVV / S&P 500)
4. Macro–geopolitical signal modeling (GDELT)


5. Implemented three sentiment models:

    a. VADER
    b. TextBlob
    c. FinBERT (ProsusAI)

6. Developed lagged and regime-filtered signals to reduce noise
7. Applied time-series cross-validation to avoid look-ahead bias
8. Trained XGBoost classifier on macro–geopolitical indicators
9. Evaluated models using directional accuracy, not just raw returns
10. Identified high-confidence market signal days during conflict-driven regimes

----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

PROBLEM STATEMENT:

Financial markets react not only to fundamentals but also to:

1. News sentiment
2. Investor attention
3. Geopolitical conflict
4. Government activity
5. Macro uncertainty

However, news and macro data are noisy, and naive models often fail due to look-ahead bias and regime changes.

----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

GOAL OF THIS PROJECT: To test whether lagged sentiment and macro geopolitical indicators can help predict next-day market direction, especially during high-volatility and stress periods.

----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

DATA SOURCES: 

1. FINANCIAL NEWS SENTIMENT:

a. Yahoo Finance RSS feeds
b. S&P 500 constituent companies
c. Daily headline aggregation

2. MARKET DATA:

a. IVV (S&P 500 ETF)
b. S&P 500 Index (^GSPC)
c. Daily returns & rolling volatility

3. MACRO GEOPOLITICAL DATA (GDELT):

a. Event counts
b. Goldstein conflict scale
c. Protest & conflict activity
d. Government-related events
e. Media attention & tone

----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

DATA EXTRACTION USING GOOGLE BIGQUERY:
Macro–geopolitical data used in this project was sourced from the GDELT database, accessed via Google BigQuery. BigQuery was used to efficiently query, aggregate, and export large-scale historical event data that would be impractical to process locally due to size and complexity.

The following datasets were extracted using SQL queries executed directly in BigQuery:

1. EVENT LEVEL DATA:

1.1 Daily total event counts
1.2 Average Goldstein conflict scores
1.3 Protest-related events
1.4 Conflict-related events
1.5 Government-related events
   
2. MEDIA ATTENTION AND TONE DATA:
   
2.1 Total article mentions per day
2.2 Average media tone
2.3 Aggregate attention intensity indicators

3. QUERY OPERATIONS PERFORMED:
3.1 Date-based aggregation at daily frequency
3.2 Filtering by relevant event types and actors
3.3 Computation of summary statistics (counts and averages)
3.4 Alignment of multiple GDELT tables (Events, Mentions, GKG)
   
Query results were exported from BigQuery as CSV files and subsequently ingested into the Python pipeline for preprocessing, feature engineering, and modeling. All data processing steps ensured temporal integrity, with macro features lagged appropriately to prevent data leakage and look-ahead bias.

----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

FEATURE ENGINEERING:

A. SENTIMENT FEATURES:

1. Daily mean FinBERT sentiment
2. Lagged sentiment (t-1, t-2)
3. Sentiment sign (positive / negative)
4. Volatility-filtered sentiment regimes


B. MACRO FEATURES (LAGGED):

1. avg_goldstein to goldstein_lag1
2. conflict_events to conflict_lag1
3. protest_events to protest_lag1
4. govt_events to govt_lag1
5. total_mentions to attention_lag1
6. avg_tone to tone_lag1

All features are shifted appropriately to prevent data leakage and look-ahead bias.

----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

MODELS USED: 

1. SENTIMENT-BASED DIRECTIONAL MODEL

a. FinBERT sentiment aligned with IVV returns
b. Construction of lagged sentiment signals
c. Volatility-filtered regime analysis

Evaluation Metric: Directional Accuracy (Up vs Down)

2. MACRO GEOPOLITICAL XGBOOST CLASSIFIER:

a. Model used: XGBoost Classifier
b. Validation approach: TimeSeriesSplit (walk-forward)
c. Target variable: Next-day S&P 500 market direction

Key Properties:

a. Captures non-linear interactions between macro signals
b. Robust to noisy geopolitical and media driven indicators
c. Feature importance analysis included for interpretability

----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

EVALUATION AND RESULTS:

A. SENTIMENT-ONLY RESULTS:

1. Directional accuracy improves under the following conditions:
  a. Periods of elevated market volatility
  b. High absolute magnitude of sentiment scores

2. Lagged FinBERT sentiment shows statistically meaningful alignment with market returns during stress and high-uncertainty regimes.

B. MACRO–GEOPOLITICAL MODEL RESULTS (XGBOOST):

METRICS AND OUTCOME:

1. DIRECTIONAL ACCURACY: Approximately 60 to 65 percent using time-series cross-validation
2. VALIDATION METHOD: Walk-forward evaluation
3. MOST INFLUENTIAL PREDICTORS: Conflict intensity, media attention, and aggregate tone

The achieved accuracy is modest but economically meaningful, reflecting the inherent difficulty of predicting daily equity market direction using real-world noisy macro and geopolitical data.


<img width="1105" height="624" alt="image" src="https://github.com/user-attachments/assets/a107ce12-b018-4650-8fc6-ac53aabc2a81" />

GRAPH: This graph summarizes the project by combining S&P 500 price dynamics with volatility-based stress regimes, momentum deterioration, and predicted dip signals. Highlighted points indicate periods where elevated volatility, deteriorating momentum, and macro stress align with short-term market drawdowns.


(⚠️ DISCLAIMER: This project is for academic and research purposes only and does not constitute financial advice.)
