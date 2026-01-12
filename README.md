# FinanKu Credit Card Payment Prediction

## PROBLEM STATEMENT
Kekhawatiran adanya keterlambatan pembayaran kartu kredit pada FinanKu yang akan merugikan bisnis. Sehingga orang-orang yang memiliki potensi untuk mengalami keterlambatan bayar bisa diprediksi lebih cepat untuk menentukan strategi yang sesuai dalam menghadapi kondisi di masa mendatang.

## OBJECTIVE
Membuat sebuah model yang dapat memprediksi setidaknya 60% dari pelanggan yang akan mengalami telat bayar kartu kredit [Accuracy & Recall di atas 60%].

## VARIABLES AVAILABLE
From the provided dataset, the following variables are available:
1.  **Customer ID**: Unique ID Customer
2.  **Branch**: Lokasi Cabang Nasabah Terdaftar
3.  **City**: Lokasi Kota Nasabah Terdaftar
4.  **Age**: Umur Nasabah Pada Periode Observasi
5.  **Avg. Annual Income/Month**: Rata-rata penghasilan nasabah dalam satu tahun
6.  **Balance (Q1-Q4)**: Saldo mengendap yang dimiliki nasabah di akhir kuartal
7.  **Num of Products (Q1-Q4)**: Jumlah kepemilikan produk nasabah di akhir kuartal
8.  **HasCrCard (Q1-Q4)**: Status kepemilikan produk kartu kredit nasabah di akhir kuartal
9.  **Active Member (Q1-Q4)**: Status keaktifan nasabah
10. **Unpaid Tagging**: Status nasabah gagal bayar (Target Variable)

## EXPERIMENT DETAILS
Two main experiments were conducted with different review periods and variable adjustments:

1.  **Review Period 1**: Nasabah direview selama satu tahun terakhir.
2.  **Review Period 2**: Nasabah direview selama 6 bulan terakhir.

**Variable Adjustments:**
*   **Balance**: Calculated as the average balance over the horizon and the change in balance between the end and beginning of the review period.
*   **Number of Products**: Examined the average, maximum, and minimum number of products owned by customers during the review period.
*   **Active Member**: Converted into active months within the review period.

## DATA UNDERSTANDING
Initial exploration revealed insights into:
*   **Customer Distribution by City**: Surabaya had the most customers, followed by Bandung and Jakarta.
*   **Unpaid Customer Distribution by City**: Surabaya also had the highest number of unpaid customers.
*   **Customer Distribution by Age**: Visualizations showed the age distribution of both all customers and unpaid customers.
*   **Average Balance**: Unpaid customers had a slightly higher average annual and quarterly balance compared to paid customers.
*   **Average Product Ownership**: Paid customers had a slightly higher average number of products owned.

## DATA PREPARATION
1.  **Duplicate and Missing Data Check**: No duplicate rows or missing values were found.
2.  **Feature Engineering**: New relevant variables were created based on the problem statement and experimental design:
    *   `Mean Balance`: Average balance over the observation period.
    *   `Delta Balance`: Change in balance (End Quarter - Start Quarter).
    *   `Active Months`: Total active months during the observation period.
    *   `Diff PH`: Difference in product holding (End Quarter - Start Quarter).
    *   `Vintage_CR`: Length of credit card ownership.
3.  **Variable Removal**: Original quarterly `Balance`, `NumOfProducts`, `HasCrCard`, and `ActiveMember` columns were dropped as they were replaced by the engineered features.
4.  **Categorical Encoding**: `Branch Code` was converted to string type and `Branch Code` and `City` were one-hot encoded.
5.  **Numerical Data Standardization**: Numerical predictor variables were standardized using `StandardScaler`.
6.  **Correlation Check**: Highly correlated variables (correlation > 0.7) were identified and removed to prevent multicollinearity.
7.  **Train-Test Split**: Data was split into training and testing sets (70% train, 30% test) using stratification for the target variable `Unpaid Tagging`.

## MODELING
Three supervised machine learning algorithms were used for modeling:
1.  **Logistic Regression**
2.  **Gradient Boosting (XGBoost)**
3.  **Random Forest**

Hyperparameter tuning was performed using `GridSearchCV` with `recall` as the primary scoring metric.

## EVALUATION & CONCLUSION
Model performance was evaluated based on Accuracy and Recall scores on both the test set and a separate validation set.

**Logistic Regression:**
*   **Experiment 1 (1-Year Review):** Test Recall: 0.436, Validation Recall: 0.264
*   **Experiment 2 (6-Month Review):** Test Recall: 0.409, Validation Recall: 0.248

**Gradient Boosting (XGBoost):**
*   **Experiment 1 (1-Year Review):** Test Recall: 0.606, Validation Recall: 0.449
*   **Experiment 2 (6-Month Review):** Test Recall: 0.507, Validation Recall: 0.389

**Random Forest:**
*   **Experiment 1 (1-Year Review):** Test Recall: 0.343, Validation Recall: 0.357
*   **Experiment 2 (6-Month Review):** Test Recall: 0.333, Validation Recall: 0.328

**Key Findings:**
*   While most models achieved accuracy above 60%, the recall scores were generally below 40% (except for Gradient Boosting Experiment 1 on the test set).
*   This indicates that a significant number of potentially defaulting customers are still being misclassified as non-defaulters.
*   The objective of predicting at least 60% of defaulting customers (based on recall) was not consistently achieved across all models and experiments, especially on the validation data.

**Future Development Suggestions:**
1.  **Increase Sample Size**: Acquire more customer data, assuming the current dataset is not the total population.
2.  **Oversampling**: Implement oversampling techniques for the minority class (unpaid customers) to address class imbalance and reduce model bias.
3.  **Expand Time Horizon**: Explore longer observation periods for customer data.
4.  **Variable Exploration**: Introduce new variables or remove low-importance features to enhance predictive power.
5.  **Hyperparameter Tuning**: Broaden the search space and granularity for hyperparameter optimization.
6.  **Alternative Models**: Experiment with other supervised machine learning algorithms.
