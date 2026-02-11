# ai-programming-foundations-project
#
## Requirements File
The dependencies for this project are listed in `requirements.txt`. To regenerate this file from your current environment, run:
```bash
pip freeze > requirements.txt
```

To set up a fresh environment, install everything with:
```bash
pip install -r requirements.txt
```

## 1. Project Overview

This project focuses on cleaning and exploring the **AB_NYC_2019** Airbnb dataset for New York City. The work is organized as a small, reproducible data workflow implemented in Jupyter notebooks. It demonstrates practical data-wrangling, exploratory data analysis (EDA), and visualization, and also reflects on how this workflow could evolve into a production ML pipeline or an agentic, automated system.

## 2. Dataset

- **Name:** AB_NYC_2019 (New York City Airbnb listings)
- **Source:** https://www.kaggle.com/datasets/dgomonov/new-york-city-airbnb-open-data
- **Format:** CSV file containing listing-level information such as price, location, room type, minimum nights, and reviews.

The main notebook assumes the raw CSV is available locally at:

```text
C:\Users\bmuruges\Downloads\archive\AB_NYC_2019.csv
```

You can adjust this path in the notebook if your file is stored elsewhere.

## 3. Environment and Setup

1. **Navigate to the project folder**

   ```bash
   cd "c:/Users/Documents/Study/AgentQA/crewai"
   ```

2. **(Optional but recommended) Create a virtual environment**

   ```bash
   python -m venv .venv
   .venv\Scripts\activate
   ```

3. **Install dependencies** (listed in `requirements.txt`):

   ```bash
   pip install -r requirements.txt
   ```

4. **Launch Jupyter or VS Code Notebooks**
   - In **VS Code**: Open this folder, open `data-workflow.ipynb` (or `analysis.ipynb`), select a Python interpreter, then run cells top to bottom.
   - In **Jupyter Notebook**:
     ```bash
     jupyter notebook
     ```
     Then open `data-workflow.ipynb` from the browser interface.

## 4. Data Workflow Overview

The core data workflow is implemented in `data-workflow.ipynb` and follows these stages:

### 4.1 Data Loading

- Reads the raw CSV file into a pandas `DataFrame`:
  - `df = pd.read_csv(r"C:\\Users\\Downloads\\archive\\AB_NYC_2019.csv")`
- Prints `df.head()` to confirm that the data loaded correctly and to inspect column names and basic structure.

### 4.2 Data Cleaning and Preparation

The notebook defines several small, reusable functions to clean the data. These are composed into a simple pipeline to produce `df_clean`.

- **`drop_missing_essential(df)`**  
  Drops rows with missing values in key analysis columns:
  - `price`
  - `room_type`
  - `neighbourhood_group`

- **`remove_price_outliers(df, min_price=10, max_price=1000)`**  
  Removes listings with **implausibly low or high prices** to reduce the impact of outliers:
  - Filters to rows where `min_price <= price <= max_price`.
  - Helps stabilize summary statistics and visualizations.

- **`remove_extreme_minimum_nights(df, max_nights=365)`**  
  Filters out listings with `minimum_nights` greater than a chosen maximum (default 365):
  - Interprets extremely long minimum stays as outliers or long-term rental patterns that are less relevant for typical Airbnb usage.

- **`fill_missing_reviews_per_month(df)`**  
  Handles missing values in `reviews_per_month` by filling them with `0`:
  - Assumes that a missing value means no reviews have been left yet.
  - Keeps the column numeric and ready for later analysis.

These functions are applied in sequence:

```python
df_clean = drop_missing_essential(df)
df_clean = remove_price_outliers(df_clean)
df_clean = remove_extreme_minimum_nights(df_clean)
df_clean = fill_missing_reviews_per_month(df_clean)
```

The notebook prints the number of rows before and after cleaning so the impact of each step is visible.

### 4.3 Exploratory Data Analysis (EDA)

EDA is encapsulated in helper functions to keep the notebook organized and reusable.

- **`eda_summary(df)`**  
  Provides a high-level overview:
  - Prints the shape of the dataset (`df.shape`).
  - Shows column data types.
  - Prints summary statistics for numeric columns via `df.describe()`.

- **`eda_price_by_neighbourhood(df)`**  
  Focuses on how **location and room type** affect price:
  - Groups by `neighbourhood_group` and `room_type`.
  - Computes the mean `price` for each combination.
  - Sorts and prints the top rows to show which neighbourhood/room-type pairs are most expensive.

These functions are run on `df_clean` to produce a clear, textual summary of the cleaned dataset.

### 4.4 Visualizations

Using Matplotlib and Seaborn, the notebook creates three key visualizations:

1. **Distribution of Listing Prices**
   - Histogram with optional KDE on `df_clean["price"]`.
   - Highlights the skewed nature of Airbnb prices, with many affordable listings and a smaller number of high-end properties.

2. **Average Price by Neighbourhood Group**
   - Bar chart of mean `price` grouped by `neighbourhood_group`.
   - Makes spatial price differences across New York City visually obvious.

3. **Price vs Minimum Nights**
   - Scatter plot showing the relationship between `minimum_nights` and `price`.
   - Demonstrates that length of stay alone does not fully explain price variation; other factors such as location and room type also matter.

### 4.5 Key Takeaways from the Workflow

- The cleaned dataset is **more reliable** and suitable for downstream analysis or modeling.
- Prices are **highly skewed**, and outlier removal improves interpretability.
- Neighbourhood and room type are **strong drivers** of price.
- The workflow is implemented in **small, testable functions**, which is a good starting point for automation and integration into larger systems.

## 5. Analysis Summary

The notebook also includes a written summary of what was learned from the analysis:

- Explored the structure of the AB_NYC_2019 dataset and basic distributions.
- Built a simple cleaning and analysis pipeline that can be reused or extended.
- Identified clear patterns in price distribution and neighbourhood-level differences.
- Noted limitations such as heuristic cleaning rules and the snapshot nature of the dataset (no time dimension).

## 6. Future Integration Reflections

README includes complete answers on ML workflow changes, neural network preparation, and agentic automation potential.

### 6.1 ML Workflow Changes

To extend this notebook into a more complete **machine learning workflow**, several enhancements would be needed:

- **Data Ingestion and Versioning**
  - Move from one-off CSV loading to a repeatable ingestion step (e.g., from cloud storage, database, or API).
  - Track dataset versions so experiments and models can be reproduced exactly.

- **Train/Validation/Test Splits**
  - Split `df_clean` into training, validation, and test sets (e.g., 70/15/15), or use time-based splits once temporal data is included.
  - Avoid data leakage, especially for host-level or listing-level features.

- **Feature Engineering**
  - Encode categorical variables such as `neighbourhood_group` and `room_type` (e.g., one-hot, target encoding).
  - Create new features like:
    - `price_per_minimum_night`
    - log-transformed `price` to handle skew
    - aggregated review metrics or host behavior indicators
  - Normalize or standardize numeric variables where appropriate.

- **Pipeline Formalization**
  - Wrap cleaning and feature engineering steps into a single, reproducible pipeline object (e.g., scikit-learn `Pipeline`).
  - Ensure that training and inference share exactly the same preprocessing logic.

- **Model Training and Evaluation**
  - Start with simpler baselines (linear models, tree-based models) to establish reference performance.
  - Use clear metrics such as MAE, RMSE, or MAPE and track them over time.

- **Monitoring and Retraining**
  - Once deployed, monitor prediction performance and data drift.
  - Define conditions under which retraining is triggered (e.g., drop in accuracy, distribution shift in key features).

### 6.2 Neural Network Preparation

If the project moves towards **neural networks** for price prediction or related tasks, additional preparation steps are required:

- **Input Representation**
  - Encode categorical features as one-hot vectors or learned embeddings.
  - Standardize or normalize continuous variables (`price`, `minimum_nights`, `reviews_per_month`, etc.) for stable training.

- **Handling Skew and Scale**
  - Consider predicting log-price instead of raw price to reduce skew.
  - Optionally model extreme luxury listings as a separate regime if they behave very differently from typical listings.

- **Model Architecture**
  - Start with a simple feed-forward neural network:
    - Input layer matching the engineered feature vector.
    - 1–3 hidden layers with non-linear activations (e.g., ReLU).
    - Output layer predicting price or log-price.
  - Later extend to multi-task models (e.g., predicting both price and probability of booking).

- **Training Infrastructure**
  - Use mini-batch training with appropriate batch sizes and learning rates.
  - Apply regularization (dropout, weight decay) and early stopping based on validation performance.
  - Log experiments (hyperparameters, metrics, model versions) for reproducibility.

- **Serving Considerations**
  - Package the model together with its preprocessing pipeline to avoid training-serving skew.
  - Provide an API endpoint that takes raw listing data and returns predicted price or other relevant outputs.

### 6.3 Agentic Automation Potential

The current, modular notebook is a good starting point for **agentic automation**, where software agents orchestrate data, models, and monitoring with minimal human intervention.

Potential agents include:

- **Data Ingestion Agent**
  - Periodically fetches updated Airbnb data.
  - Validates schema and basic quality metrics (row counts, missingness, value ranges).
  - Triggers downstream cleaning and analysis steps when new data is available.

- **Data Cleaning and QA Agent**
  - Runs the cleaning pipeline on incoming data.
  - Monitors for anomalies or schema changes and suggests updated thresholds or rules.

- **Exploratory Analysis Agent**
  - Regenerates summary statistics and visualizations on a schedule.
  - Produces human-readable reports highlighting recent changes (e.g., rising prices in specific neighbourhoods).

- **Model Training and Evaluation Agent**
  - Detects when model performance has degraded or when data drift is significant.
  - Launches training jobs with different models or hyperparameters.
  - Compares results and recommends or automatically selects the best model.

- **Deployment and Monitoring Agent**
  - Automates deployment of new model versions behind feature flags or A/B tests.
  - Continuously tracks performance, latency, and fairness metrics in production.
  - Rolls back models or raises alerts when problems are detected.

By starting from clear, modular notebook code (data loading → cleaning → EDA → visualization), this project is well-positioned to grow into a **robust, end-to-end ML system** that leverages agentic automation while keeping humans in the loop for oversight and key decisions.


