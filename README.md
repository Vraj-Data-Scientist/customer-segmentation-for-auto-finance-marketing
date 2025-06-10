
# Customer Segmentation for Auto Finance Marketing

## Project Overview
This project performs customer segmentation for auto finance marketing using a dataset of bank transactions. By applying RFM (Recency, Frequency, Monetary) analysis and K-Means clustering, the project identifies distinct customer segments to enable targeted marketing strategies, risk assessment, and product customization for an auto finance company.

## Objective
The goal is to segment customers based on their financial behavior and demographics to optimize marketing campaigns, improve loan approval decisions, and enhance customer retention and revenue for auto financing products.

## Dataset
The dataset (`bank_transactions.csv`) contains over 1 million banking transaction records with the following columns:

| Column Name             | Description                                          | Data Type       |
|-------------------------|------------------------------------------------------|-----------------|
| TransactionID           | Unique identifier for each transaction                | String          |
| CustomerID              | Unique identifier for each customer                   | String          |
| CustomerDOB             | Customer's date of birth                             | Datetime        |
| CustGender              | Customer's gender (M, F, T)                           | String          |
| CustLocation            | Customer's location                                  | String          |
| CustAccountBalance      | Customer's account balance (INR)                     | Float           |
| TransactionDate         | Date of the transaction                              | Datetime        |
| TransactionTime         | Time of the transaction                              | Integer         |
| TransactionAmount (INR) | Transaction amount in INR                            | Float           |
| CustomerAge             | Derived customer age (calculated from DOB)           | Integer         |

## Methodology
1. **Data Preprocessing**:
   - Handled missing values using imputation (mode for `CustGender` and `CustLocation`, median for `CustAccountBalance`, and gender-based median for `CustomerDOB`).
   - Corrected invalid ages (dropped ages < 0 or > 100) and ensured `CustAccountBalance` ≥ `TransactionAmount`.
   - Converted date columns to datetime format and adjusted future dates by subtracting 100 years.
   - Final dataset shape after cleaning: (893,946, 10).

2. **Feature Engineering**:
   - Created an RFM table by aggregating data by `CustomerID`:
     - **Recency**: Days since last transaction (relative to 2016-10-01).
     - **Frequency**: Number of transactions.
     - **Monetary**: Mean `CustAccountBalance` and `TransactionAmount`.
   - Added `CustomerAge` and `CustGender` as features.
   - Capped outliers using the IQR method for numerical features.
   - Encoded `CustGender` (M=1, F=0) and scaled features using `StandardScaler`.

3. **Exploratory Data Analysis (EDA)**:
   - Analyzed distributions of `CustomerAge`, `CustAccountBalance`, `TransactionAmount`, `Recency`, and `Frequency`.
   - Visualized gender distribution and top 10 customer locations.
   - Reduced dataset size via stratified sampling (20% of data) by `CustGender` for clustering efficiency.

4. **Clustering**:
   - Applied K-Means clustering with the elbow method to determine the optimal number of clusters (k=5).
   - Clustered customers based on `Frequency`, `CustGender`, `CustAccountBalance`, `TransactionAmount`, `CustomerAge`, and `Recency`.

5. **Visualization**:
   - Used PCA to reduce dimensionality and visualize clusters in a 2D scatter plot.

## Results
The K-Means clustering identified five distinct customer segments, each with unique characteristics and actionable marketing strategies:

| Cluster | Mean Account Balance (INR) | Mean Transaction Amount (INR) | Mean Age (Years) | Mean Recency (Days) | Most Frequent Gender | Marketing Strategy |
|---------|----------------------------|-------------------------------|------------------|---------------------|---------------------|--------------------|
| 0       | 21,732.25                  | 443.75                        | 28.89            | 31.21               | Male                | Target with starter loans or compact vehicle leases for young, budget-conscious males. |
| 1       | 114,359.80                 | 1,615.72                      | 37.50            | 53.54               | Male                | Offer premium loans for luxury vehicles (e.g., Cadillac) for affluent, middle-aged customers. |
| 2       | 30,066.88                  | 614.91                        | 29.43            | 171.12              | Male                | Re-engage with promotional financing or loyalty programs for mid-tier vehicles. |
| 3       | 35,275.15                  | 681.28                        | 28.51            | 41.10               | Female              | Target with entry-level loans or compact vehicle promotions for young, financially stable women. |
| 4       | 52,455.20                  | 834.52                        | 30.56            | 28.72               | Male                | Promote mid-tier financing or crossover/SUV loans for active, mid-career customers. |

### Visualization Insights
- The PCA scatter plot shows clear separation of clusters, with:
  - **Cluster 1** occupying high PCA1/PCA2 regions due to high financial metrics.
  - **Cluster 2** in a low PCA2 region, reflecting less recent transactions.
  - **Clusters 0, 3, 4** centrally located, indicating moderate metrics.

## Business Applications
This segmentation provides actionable insights for auto finance marketing:
1. **Targeted Marketing**: Tailor campaigns to specific segments (e.g., luxury vehicle loans for Cluster 1, compact car leases for Cluster 0).
2. **Risk Assessment**: Identify low-risk (Cluster 1) vs. high-risk (Cluster 0) customers for better loan approval decisions.
3. **Product Customization**: Design financing products suited to each segment’s financial capacity (e.g., low-interest loans for Cluster 3).
4. **Customer Retention**: Re-engage inactive customers (Cluster 2) with loyalty programs or promotional offers.
5. **Revenue Optimization**: Focus marketing on high-balance, high-transaction customers (Cluster 1) to maximize profitability.
6. **Market Expansion**: Target underserved segments (e.g., young females in Cluster 3) for new financing opportunities.
7. **Operational Efficiency**: Allocate marketing resources to high-potential segments (e.g., Clusters 1 and 4).

## Prerequisites
- Python 3.8+
- Libraries: `pandas`, `numpy`, `matplotlib`, `seaborn`, `scikit-learn`, `kneed`, `scipy`
- Install dependencies:
  ```bash
  pip install pandas numpy matplotlib seaborn scikit-learn kneed scipy
  ```

## How to Run
1. Clone the repository:
   ```bash
   git clone https://github.com/Vraj-Data-Scientist/customer-segmentation-for-auto-finance-marketing
   ```
2. Place the `bank_transactions.csv` dataset in the project directory.
3. Run the Jupyter notebook (`customer-segmentation-for-auto-finance-marketing.ipynb`) to execute the code.
4. Review the generated plots and cluster summaries for insights.

## Limitations
- **Data Quality**: Assumes consistent `CustGender` and `CustLocation` per customer; inconsistencies may require additional validation.
- **Outlier Handling**: Capping outliers may oversimplify extreme cases, potentially affecting segment accuracy.
- **Scalability**: Clustering on a large dataset requires stratified sampling, which may miss rare patterns.
- **Feature Selection**: Limited to RFM-based features; additional variables (e.g., credit score) could enhance segmentation.

## Future Improvements
- Incorporate additional features (e.g., credit history, loan repayment behavior).
- Explore hierarchical clustering or DBSCAN for alternative segmentation approaches.
- Implement real-time segmentation for dynamic marketing campaigns.
- Integrate with a database (e.g., SQL) for scalable processing.



