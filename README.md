# Pricing Decision-Support for Airbnb Hosts in Vaud

## Project Overview

This repository contains a data-driven pricing decision-support system tailored for Airbnb hosts in the Canton of Vaud, Switzerland.
By applying advanced Machine Learning models, the system models guests' Willingness to Pay (WTP) and delivers actionable price recommendations based on individual listing characteristics. A core focus of this architecture is interpretability, ensuring that hosts do not just receive a black-box price suggestion but understand why a specific price is recommended to balance revenue and occupancy optimally.

## Repository Structure (.zip Content)

The submitted .zip file contains all the necessary components required to run and review the project:
- Main_Work_ML.rmd: The central R-Markdown document. It contains the complete, fully documented R code—covering everything from data cleaning and feature engineering to the training and evaluation of all machine learning models.
- Main_Work_ML.html: The pre-rendered ("knitted") HTML version of the project. This document serves as an interactive report for business clients and stakeholders, allowing them to review the findings and visualizations without needing any programming knowledge.
- Data_Vaud_all/: The data directory containing the primary source files:
- listings-2.csv: The official, unmodified raw dataset from Inside Airbnb for the Canton of Vaud.

## Reproducibility Guide

Follow these steps to locally reproduce the entire analysis, models, and the final HTML report from the provided .zip file:

### Prerequisites
- Software Requirements: Ensure that R (version 4.2.0 or newer) and a suitable IDE (preferably RStudio) are installed on your system.
- Extract the ZIP Archive: Unzip the contents of the .zip file into a local directory on your machine.

### Step-by-Step Execution

Open the Project: Launch RStudio and open the Main_Work_ML.rmd file. (Alternatively, double-clicking the file from your file explorer will automatically open it within your environment).
Execute and Render (Knit): In the top toolbar of RStudio, click the "Knit" button (or use the shortcut Ctrl+Shift+K / Cmd+Shift+K).
The environment will automatically handle the preset relative paths and ensure any necessary package checks included in the script are executed.
R will then run the entire pipeline sequentially: loading the raw data from Data_Vaud_all/listings-2.csv, processing the features, training the models, and rendering the final visualizations. Once complete, a freshly generated Main_Work_ML.html file will be saved to your directory.

## Research Core & Hypotheses

Our business case bridges quantitative predictive modeling with strategic market positioning by addressing two overarching research questions:
Willingness to Pay (WTP): Which features (structural/location-centric vs. reputational/host-centered) drive a guest's willingness to pay a premium?
Predictive Pricing: To what extent can an algorithmic system determine whether an active listing's price is objectively justified by its features?

##  Hypotheses

H1 (Host-Centric): Higher host reputation and service indicators (e.g., review scores, host_is_superhost status) allow hosts to charge a pricing premium.
H2 (Location-Centric): Structural features (room_type, bathrooms, accommodates) and precise non-linear spatial geography are the dominant drivers of WTP.

## Data Engineering & Feature Engineering

The project is built upon the official Inside Airbnb dataset for the Canton of Vaud (listings-2.csv).

## Data Cleaning Pipeline

Target Isolation & Truncation: Extreme outliers (e.g., highly priced private rooms skewing the distribution) were removed to ensure statistical robustness.
Logarithmic Transformation: Due to the severe right-skewness of price data, the target variable was transformed into log_price.
Dimensionality Reduction: The initial feature space was carefully filtered down to 20 crucial predictor variables to comply with algorithmic efficiency constraints.

## Key Engineered Features

price_per_guest: Calculated feature to normalize pricing layout.
high_demand: A binary indicator flag derived from high annual occupancy thresholds (>200 days).
price_high: A binary classification variable capturing whether a property lies above the regional median price.

## Modeling Architecture

We implemented a diverse model zoo to capture linear baselines, non-linear geographies, and deep complex structural interactions:
                      [ Raw Data: listings-2.csv ]
                                   │
                     [ Cleaned & Filtered Feature Space ]
                                   │
         ┌─────────────────────────┼────────────────────────┐
         ▼                         ▼                        ▼
  [ Regression ]           [ Classification ]      [ Spatial Optimization ]
  ├─ Baseline LM           ├─ Binomial GLM          └─ GAM (Lat/Long Splines)
  └─ Poisson /             └─ SVM (RBF Kernel)              │
     Quasi-Poisson GLM                                      ▼
                                                    [ Traffic Light Verdict ]
                                                    (Under-, Fair, Overpriced)
### 1. Parametric Baselines & Count Models

Linear Model (LM): Serves as our baseline benchmark to assess global trends between basic features and log_price.
Poisson & Quasi-Poisson GLM: Utilized to explicitly model count outcomes (such as occupancy/demand days), incorporating a dispersion parameter adjustment to address massive overdispersion in user behaviors.
Binomial GLM: Employed to compute structural log-odds for the binary target variable price_high.

### 2. Semi-Parametric & Non-Linear Machine Learning

Generalized Additive Models (GAM): Integrates penalized thin-plate regression splines for geographical coordinates (latitude, longitude). This captures high-resolution spatial price patterns (e.g., proximity premium along Lake Geneva and urban Lausanne).
Support Vector Machines (SVM): Utilizes a Radial Basis Function (RBF) kernel to categorize listings into a "Traffic Light" verdict system:
- Underpriced: High opportunity to increase margins without losing occupancy.
- Fair Price: Optimal trade-off between volume and margin.
- Overpriced: Risk of booking loss due to sub-optimal market-alignment.
Artificial Neural Networks (ANN): Implemented via multilayer perceptrons to specifically demonstrate omitted variable bias (OVB). It contrasts performance when host-centric variables are intentionally excluded versus included.

## Key Empirical Findings

1. The Superhost Paradox (Volume over Margin) — H1 Refuted
Contrary to H1, the data reveals that Superhosts do not charge a price premium. Instead, they deliberately use a volume-driven strategic alignment: pricing competitively (often slightly below the local median) to secure up to 2.6× higher occupancy rates compared to regular hosts. Superhost status acts as an occupancy stabilizer rather than a premium pricing lever.
2. Structural Domination — H2 Strongly Confirmed
Willingness to Pay is heavily dominated by structural capacities (room_type, bathrooms, accommodates) and non-linear geographic factors. Proximity to economic centers and the Lake Geneva basin creates a steep non-linear price curve that cannot be compensated for by purely behavioral or service-oriented host traits.

## Authors

Pascal Ingold
Timon Reichmuth
Davide Gualdani

Developed as part of a Master's Seminar in Predictive Modeling and Machine Learning, 2026.