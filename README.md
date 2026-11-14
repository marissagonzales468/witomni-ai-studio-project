# WitOmni Customer Intelligence: Repeat-Purchase Prediction

An industry-sponsored Break Through Tech AI Studio project that uses customer transaction history and supervised machine learning to predict which customers are most likely to make a future purchase.

## Project Goal

The goal is to help WitOmni prioritize customer-retention and marketing efforts by ranking customers according to their likelihood of returning.

## Dataset

This project uses the publicly available [UCI Online Retail dataset](https://archive.ics.uci.edu/dataset/352/online+retail). The dataset contains 541,909 transaction records from a UK-based online retailer covering December 2010 through December 2011.

After removing duplicates, missing customer IDs, canceled transactions, invalid quantities or prices, and non-product records, 391,153 valid transactions remained. These transactions represented 3,701 customers in the final modeling dataset.

## Dataset Access

The dataset is publicly available through the
[UCI Machine Learning Repository](https://archive.ics.uci.edu/dataset/352/online+retail).

The original dataset is not duplicated in this repository. Download it from UCI and place it in the `data/` folder before running the notebook.

## Methodology

### Data preparation

- Removed 5,268 duplicate records.
- Removed transactions without a customer ID.
- Removed canceled invoices and invalid purchases.
- Removed non-product records such as postage, fees, and adjustments.
- Used a temporal cutoff of October 7, 2011 to separate historical customer behavior from future outcomes.

### Target definition

A customer was labeled a repeat purchaser if they made at least one purchase after the temporal cutoff. The final labels included 1,796 repeat purchasers and 1,905 non-repeat purchasers, for an overall repeat-purchase rate of 48.53%.

### Feature engineering

Customer-level features were calculated using only transactions before the cutoff date:

- Recency
- Purchase frequency
- Total amount spent
- Average order value
- Customer tenure
- Average days between orders
- Number of unique products purchased
- Number of orders in the previous 90 days
- Whether the customer was located in the United Kingdom

The temporal split was used to reduce data leakage and better represent a real forecasting task.

## Models

The project compared:

- Logistic Regression
- K-Nearest Neighbors
- Decision Tree
- Random Forest
- XGBoost

Models were evaluated using ROC-AUC, PR-AUC, precision, recall, F1 score, cross-validation, and a held-out test set. A simple RFM ranking rule and a dummy classifier were also used as baselines.

## Results and Key Findings

Adding the new customer-level features improved both Logistic Regression and XGBoost:

| Feature set | Model | Test ROC-AUC |
|---|---|---:|
| Original four features | Logistic Regression | 0.736 |
| Original four features | XGBoost | 0.744 |
| Original plus new features | Logistic Regression | 0.751 |
| Original plus new features | XGBoost | 0.754 |

Additional findings:

- The top decile of customers ranked by XGBoost had an approximately 94.6% repeat-purchase rate.
- At the default 0.50 threshold, XGBoost achieved 0.744 ROC-AUC, 0.704 precision, 0.542 recall, and 0.612 F1.
- Lowering the decision threshold to approximately 0.36 increased recall to 0.844 and F1 to 0.698, while precision decreased to 0.595.
- Logistic Regression, Random Forest, and XGBoost performed similarly; XGBoost was not significantly better than most competing models in the paired bootstrap comparison.
- Purchase frequency was the strongest predictor in the logistic model, followed by total spending and recency.
- The results suggest that better customer-level features contributed more than simply choosing a more complex model.

## My Contributions

My contributions included:

- Cleaning and preparing the transaction data.
- Engineering customer-level features from historical transactions.
- Creating the temporal train-test setup and repeat-purchaser label.
- Comparing machine learning models and evaluating their performance.
- Interpreting model results, thresholds, customer-ranking lift, and limitations.
- Contributing to the project notebook and documentation.

## Repository Contents

- `README.md` — project overview, methodology, results, contributions, and reproducibility information.
- `witOmni_1B_Notebook_Nov3.ipynb` — notebook containing data preparation, feature engineering, model training, and evaluation.
- `visualizations/` — project result visualizations, if included.

The original code, requirements, and shared project materials are maintained in the official Break Through Tech repository.

## Setup and Installation

Clone the official project repository:

```bash
git clone https://github.com/Break-Through-Tech/witOmni-1B-ai-powered-customer-intelligence-for-smarter-marketing.git
cd witOmni-1B-ai-powered-customer-intelligence-for-smarter-marketing

Create and activate a virtual environment:

python -m venv .venv

# macOS/Linux
source .venv/bin/activate

# Windows
.venv\Scripts\activate
```

Install the required dependencies:

```bash
pip install -r requirements.txt
```

Launch Jupyter Notebook:

```bash
jupyter notebook
```

Open a notebook in the `notebooks/` folder and run the data-loading, cleaning, feature-engineering, model-training, and evaluation sections in order.

### 3. Add a direct notebook section

Add this after the setup section:


## Notebook

- [Open the Google Colab notebook](https://colab.research.google.com/drive/1XAYf_vb9C_Qgti77I6mzn8LFBNpTX_JE?authuser=1#scrollTo=IxbE4Kr46PVI)
- [View the notebooks in the repository](https://github.com/Break-Through-Tech/witOmni-1B-ai-powered-customer-intelligence-for-smarter-marketing/tree/main)

## Visualizations
<img width="1290" height="496" alt="image" src="https://github.com/user-attachments/assets/258b0648-0e36-4909-b3e2-d34f67b74bbb" />
<img width="916" height="310" alt="image" src="https://github.com/user-attachments/assets/0909d0bb-0a79-44ee-b303-d1c58de14f71" />

The notebooks include visualizations and analyses of:

- Transaction and customer behavior
- Feature distributions
- Repeat-purchaser rates
- ROC and precision-recall curves
- Model comparison
- Customer-ranking lift
- Permutation importance and feature effects

## Limitations

The model predicts which customers are likely to return; it does not prove that a marketing campaign causes customers to return. The dataset represents one historical time period, and the model may not generalize to future customer behavior or other retailers. The middle-ranked customers were difficult to distinguish because their behavior was close to the overall base rate.

## API Documentation

This project is notebook-based and does not expose a public API. The analysis can be reproduced using the provided notebook, dataset instructions, installation steps, and evaluation workflow.

## Next Steps

- Test the model on a newer time period.
- Add richer customer, product, and marketing features.
- Evaluate whether targeted outreach changes customer behavior through an experiment.
- Improve model interpretability for marketing stakeholders.
- Compare performance across additional customer segments.

## Acknowledgements

This project was completed through Break Through Tech's AI Studio program in collaboration with WitOmni. The project team contributed to the shared repository, notebook development, analysis, and evaluation.

## Team Members

This project was completed collaboratively through Break Through Tech’s AI Studio program. Team members and their GitHub handles are listed in the official project repository.

The original project repository and shared materials are maintained by Break Through Tech and the WitOmni project team.
