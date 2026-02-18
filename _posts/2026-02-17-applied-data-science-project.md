---
layout: post
author: Vimalraj T
title: "Applied Data Science Project Documentation"
categories: ITD214
---
## Project Background
E-commerce platforms serve customers with diverse purchasing behaviors. Treating all customers uniformly limits the effectiveness of promotions, pricing, and retention programs.  
This project applies **customer segmentation** to identify distinct purchasing profiles using Amazon e-commerce sales transactions and translate them into actionable business strategies.

**Business Goal:**  
Our team's overall business goal is "To analyze customer purchasing behavior, satisfaction distribution, and transactional drivers in Amazon e-commerce data to produce actionable insights that support targeted marketing strategies and revenue optimization."

**My Individual Objective (Objective 1 – Customer Segmentation):**  
To segment customers based on purchasing behavior using transactional and monetary features derived from sales data, and identify distinct customer profiles to support targeted marketing, pricing, and customer engagement strategies.
.

## Work Accomplished
I successfully:
- Conducted exploratory data analysis (EDA) and data quality checks on the sales dataset (~100K orders)
- Cleaned and validated numeric and categorical fields (quantity, price, discount, shipping, total amount)
- Engineered customer-level behavioral features (spend, frequency, discount sensitivity, shipping impact)
- Built and evaluated **four clustering models**:
  - K-Means
  - MiniBatchKMeans
  - Gaussian Mixture Model (GMM)
  - DBSCAN
- Selected the best clustering approach using multiple evaluation metrics:
  - Silhouette Score (higher is better)
  - Davies–Bouldin Index (lower is better)
  - Calinski–Harabasz Score (higher is better)
- Performed a **Cluster Stability Check** using:
  - Metric stability across seeds
  - Adjusted Rand Index (ARI)
- Interpreted clusters into business-friendly segments and provided recommendations
- Implemented **ipywidgets** for interactive exploration (cluster explorer, spend bucket filter, discount filter)

This work supports the team’s overall project direction by providing a data-driven segmentation framework for decision support.

---

### Data Preparation


### Dataset Overview
- Dataset: Amazon Sales transactional data  
- Granularity: Order-level → aggregated to customer-level  
- Key fields used:
  - Quantity, UnitPrice, Discount, ShippingCost, TotalAmount
  - Category, PaymentMethod, City/State/Country (for optional profiling)

### Preprocessing Steps
1. **Data Cleaning & Validation**
   - Converted numeric fields (quantity, unit price, discount, shipping cost, total amount)
   - Removed invalid rows (negative values, invalid discount bounds, missing critical values)
   - Outlier scan to identify extreme total amount values

2. **Customer-Level Aggregation**
   - total_orders, total_spend, avg_order_value, avg_quantity
   - discount_rate, shipping_ratio

3. **Feature Engineering Enhancements**
   - `log_total_spend` to reduce skew
   - `spend_bucket` (quartile-based) for interpretability
   - `discount_user_flag` to capture promotion sensitivity

4. **Feature Scaling**
   - StandardScaler applied to clustering input features

#### Visuals (Add Images)
- Distribution of Total Amount (order value)
- Discount distribution
- Spend bucket distribution
- Feature correlation heatmap

**Example Images (replace filenames with your own):**
![TotalAmount Distribution](/assets/img/itd214/totalamount_hist.png)
![Spend Buckets](/assets/img/itd214/spend_bucket_bar.png)
![Correlation Heatmap](/assets/img/itd214/corr_heatmap.png)

### Modelling


### Clustering Features Used
The segmentation used the following scaled customer-level features:
- log_total_spend
- avg_order_value
- order_frequency
- avg_quantity
- discount_rate
- shipping_ratio

### Selecting K (Number of Clusters)
K was selected using:
- **Elbow Method** (inertia vs k)
- **Silhouette Score** (cluster separation quality)

![Elbow Method](/assets/img/itd214/elbow_plot.png)
![Silhouette vs K](/assets/img/itd214/silhouette_plot.png)

### Models Compared (4-model evaluation)
To align with assessment expectations, four clustering algorithms were evaluated on a representative sample:
- **K-Means**
- **MiniBatchKMeans**
- **Gaussian Mixture Model (GMM)**
- **DBSCAN** (density-based, also identifies noise/outliers)

**Evaluation Metrics Used**
- Silhouette Score (↑ better)
- Davies–Bouldin Index (↓ better)
- Calinski–Harabasz Score (↑ better)

![Model Comparison Metrics](/assets/img/itd214/model_compare_bar.png)

**Best Performer: K-Means**  
K-Means achieved the most balanced performance across all metrics and produced stable, interpretable segments suitable for business use.

---


### Evaluation


### PCA Visualization (2D Projection)
PCA was used solely to visualize clustering structure (not for training).
![PCA Cluster Projection](/assets/img/itd214/pca_clusters.png)

### Cluster Stability Check
The K-Means solution was validated for stability across multiple random seeds:
- Internal metrics showed negligible variance (Silhouette/DB/CH)
- ARI results were near perfect, indicating consistent cluster assignments across runs

**Result Summary**
- Mean ARI ≈ 0.996  
- Min ARI ≈ 0.990  
- Max ARI = 1.000  

This confirms the segmentation is robust and not sensitive to centroid initialization.

---

## Cluster Interpretation (Business Profiles)

The final solution produced **four distinct customer segments**:

1. **High-Value Loyal Customers**  
   - High average order value, strong spending, low discount dependence  
   - Strategy: Loyalty programs, VIP perks, personalized recommendations

2. **Discount-Sensitive Shoppers**  
   - Low order value and frequency, highest discount usage  
   - Strategy: Targeted promotions, coupons, free shipping thresholds

3. **Frequent Buyers / Habitual Customers**  
   - Higher order frequency, steady spend over repeated purchases  
   - Strategy: Bundles, cross-sell, subscriptions, replenishment reminders

4. **Mid-Tier Regular Customers**  
   - Moderate spend and frequency, growth potential  
   - Strategy: Convert to higher value via engagement and threshold offers

![Cluster Profiles](/assets/img/itd214/cluster_profile_bar.png)
![Spend Bucket by Cluster](/assets/img/itd214/spend_bucket_by_cluster.png)

---

## Interactive Exploration (ipywidgets)

An interactive widget interface was developed to:
- Select a cluster and view summary statistics
- Filter customers by spend bucket and discount usage
- Support explainability and stakeholder exploration

![ipywidgets Demo](/assets/img/itd214/widgets_demo.png)


## Recommendations and Analysis (Business Insights)

Based on the final K-Means segmentation results, four distinct customer behavioral segments were identified. These segments provide actionable guidance for targeted marketing, pricing, and customer engagement strategies.

### Segment 1: Premium Loyal Customers
**Behavioral Signals**
- Highest average order value and high average quantity per order  
- Low discount reliance and minimal shipping sensitivity  

**Business Interpretation**
This segment represents the most valuable customers who contribute high revenue per transaction and are less price-sensitive.

**Recommended Actions**
- Implement VIP/loyalty tiers (exclusive perks, early product access)
- Prioritize personalized recommendations over heavy discounting
- Offer premium service options (fast delivery, priority support)

---

### Segment 2: Promotion-Driven Customers
**Behavioral Signals**
- Lowest average order value and lowest purchase frequency  
- Highest discount usage and highest shipping cost ratio  

**Business Interpretation**
Customers in this segment are highly price-sensitive and respond most to promotions. Shipping cost can be a major purchase friction.

**Recommended Actions**
- Targeted discounts (coupons, flash sales) instead of blanket promotions
- Free-shipping thresholds to reduce shipping friction
- Bundle promotions (buy-more-save-more) to increase basket size

---

### Segment 3: Frequent Value Buyers
**Behavioral Signals**
- Highest order frequency  
- Strong total spend accumulated through repeated purchases  
- Moderate discount usage and low shipping sensitivity  

**Business Interpretation**
A reliable repeat-purchase segment that contributes steady revenue through frequent orders and has upsell potential.

**Recommended Actions**
- Cross-sell and upsell recommendations based on purchase history
- Subscription/replenishment programs for repeat purchase items
- Loyalty point accelerators to encourage higher basket size

---

### Segment 4: Core Regular Customers
**Behavioral Signals**
- Moderate spend and moderate purchase frequency  
- Balanced discount usage and modest shipping sensitivity  

**Business Interpretation**
A stable “middle” segment with strong potential to be upgraded into higher value segments through engagement strategies.

**Recommended Actions**
- Personalized engagement campaigns (product discovery, tailored offers)
- Tier-based incentives to drive higher order value and repeat purchasing
- Improve customer experience (delivery transparency, service responsiveness)

---

### Strategic Impact Summary
Overall, the segmentation enables:
- **More efficient marketing allocation** (right offer to the right segment)
- **Reduced unnecessary discounting** (protect margin by targeting promotions)
- **Improved retention strategies** (focus on premium customers)
- **Revenue growth opportunities** through cross-sell, bundle, and loyalty programs

---

## Limitations and Future Work

### Limitations
1. **No time-based customer behavior modeling**
   - The segmentation is based on aggregated historical behavior and does not include recency-based features (e.g., last purchase date) or seasonality patterns.

2. **No demographic enrichment**
   - The dataset does not include demographic information (age, income, household profile), limiting deeper persona-style segmentation.

3. **Moderate cluster overlap**
   - Silhouette scores indicate clusters are moderately separated, which is common in consumer transactional datasets where behaviors overlap naturally.

4. **No external KPI validation**
   - Segments are evaluated using internal metrics and stability checks, but are not validated against campaign response rates, retention outcomes, or profitability KPIs.

5. **Static segmentation**
   - Customers may shift between segments over time; the current model reflects a snapshot and may require periodic re-training.

---

### Future Work
1. **Add time-based features (RFM segmentation)**
   - Incorporate Recency, Frequency, and Monetary features to detect churn risk and customer lifecycle changes.

2. **Dynamic segmentation**
   - Recompute segments monthly/quarterly and monitor drift to capture changes in customer purchasing patterns.

3. **Link segmentation with business outcomes**
   - Validate clusters using downstream KPIs such as retention rate, campaign uplift, and repeat purchase probability.

4. **Predictive modeling layer**
   - Add supervised models for churn prediction or customer lifetime value (CLV) estimation to complement the clustering results.

5. **Advanced segmentation methods**
   - Explore autoencoder-based clustering or spectral clustering for potentially improved separation in high-dimensional behavior space.

---


---

## AI Ethics
Discuss the potential data science ethics issues (privacy, fairness, accuracy, accountability, transparency) in your project. 

Pricacy and Data Protection:
- Customer data is anonymized and analyzed at ID-level.
- No personally identifiable information (PII) is used.
- Segmentation is behavior-based, not identity-based.
- No sensitive attributes (race, gender, religion) are included.

Fairness and Bias:
The model segments customers based on purchasing behavior.
Historical purchasing data may reflect:
- Income inequality
- Access disparities
- Promotional targeting bias

Risk:
Low-spend customers could be unfairly deprioritized in marketing strategies.

Mitigation:
Segmentation should support service personalization, not discrimination.


Transparency and Explainability.
- K-Means clustering is interpretable.
- Feature contributions are transparent (log_total_spend, frequency, etc.).
- Business labels are derived from measurable behavioral patterns.

Accuracy and Model Realiability.
Although clustering is unsupervised, reliability was validated through:
- Multi-model comparison (K-Means, MiniBatch, GMM, DBSCAN)
- Multiple evaluation metrics (Silhouette, DB, CH)
- Cluster stability validation (ARI ≈ 0.996)

This ensures:
- Reproducibility
- Robustness
- Stability across random initializations

Important clarification:
Clustering does not produce "accuracy" like classification models. Instead, reliability is assessed through stability and internal validation metrics.


Responsible Business Use
Segmentation should:
- Enhance customer experience
- Improve marketing efficiency
- Avoid penalizing customers solely based on spending capacity
- Be monitored continuously for drift

## Conclusion

This project demonstrated an end-to-end customer segmentation workflow using Amazon sales transactions, covering data preparation, feature engineering, clustering model comparison, stability validation, and business interpretation.

Key achievements:
- Constructed customer-level behavioral features capturing spend, frequency, discount sensitivity, and shipping impact
- Evaluated multiple clustering approaches (K-Means, MiniBatchKMeans, GMM, DBSCAN) using standard internal validation metrics
- Selected **K-Means** as the final model due to the best balance of performance, interpretability, and stability
- Confirmed robustness through a stability check with **high ARI consistency (~0.996)**
- Produced actionable segment-driven recommendations to support targeted marketing strategies and revenue optimization

Overall, the segmentation provides a practical framework for businesses to tailor engagement strategies, allocate promotions more effectively, and improve customer retention outcomes.

## Source Codes and Datasets
Upload your model files and dataset into a GitHub repo and add the link here. 
