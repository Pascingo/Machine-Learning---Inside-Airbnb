# Machine-Learning---Inside-Airbnb
## Airbnb Pricing Decision-Support System: Canton of Vaud

## 📌 Project Overview
This project proposes a data-driven pricing decision-support system for Airbnb hosts in the Canton of Vaud, Switzerland. The goal is to provide market-aligned price recommendations based on specific listing characteristics. 

By analyzing inputs such as location, room type, guest capacity, and host status, the tool helps both new and existing hosts balance revenue and occupancy. A key feature of our approach is **interpretability**, ensuring that hosts understand exactly *why* a certain price is recommended.

## 🎯 Business Case & Research Questions
Our objective is to increase booking rates, reduce price dispersion, and improve host satisfaction by answering two main research questions:
1. **Willingness to Pay (WTP):** Which factors (host-centered vs. location-centered) most strongly influence guests' willingness to pay?
2. **Predictive Pricing:** To what extent can these factors accurately predict whether the price of a rental property is justified?

### Hypotheses
*   **H1:** When host-centered indicators (e.g., reviews, Superhost status) improve, tenants' willingness to pay increases.
*   **H2:** When location-centered indicators (e.g., capacity, property type) improve, tenants’ willingness to pay increases.

## 🗄️ Data Preparation & Feature Engineering
The dataset uses Airbnb listings from the Canton of Vaud (`listings-2.csv`). The data was cleaned and limited to 20 key variables to meet project requirements.
*   **Transformations:** Price data was log-transformed (`log_price`) to account for right-skewness. Extreme outliers (e.g., a multi-million CHF private room) were removed.
*   **New Features:** Added metrics such as `price_per_guest`, a binary `high_demand` indicator (occupancy > 200 days), and a `price_high` classification (above median price).

## 🧠 Modeling Approach
We applied a diverse set of Machine Learning models to capture both linear and non-linear relationships:
*   **Linear Model (LM):** To establish baseline relationships between reviews, host status, capacity, and log price.
*   **GLM (Poisson / Quasi-Poisson):** To analyze count data (occupancy days) and identify drivers of demand while correcting for overdispersion.
*   **GLM (Binomial):** To classify whether listings are priced above or below the market median.
*   **Generalized Additive Model (GAM):** To capture non-linear spatial pricing effects (longitude/latitude splines) and generate a "traffic light" pricing verdict (Underpriced, Fair Price, Overpriced).
*   **Support Vector Machine (SVM):** To classify the host's pricing strategy into the GAM's traffic light categories using a radial basis function (RBF) kernel.
*   **Artificial Neural Networks (ANN):** To demonstrate omitted variable bias and confirm the true impact of structural vs. host-centric features.

## 💡 Key Findings
*   **The Superhost Strategy (Volume over Margin):** Contrary to H1, Superhosts do *not* charge a premium. Instead, they price competitively (often below the median) to achieve significantly higher occupancy (up to 2.6x more bookings). Superhost status is an occupancy driver, not a price driver.
*   **Location & Capacity Rule:** H2 is strongly confirmed. The primary drivers of willingness to pay are structural (room type, bathrooms, guest capacity) and non-linear spatial geography (proximity to Lake Geneva or urban centers).

## 👥 Authors
*   Pascal Ingold
*   Timon Reichmuth
*   Davide Gualdani