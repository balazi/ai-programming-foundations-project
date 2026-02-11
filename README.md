# ai-programming-foundations-project
# Airbnb NYC Data Cleaning and EDA

## Project Description
This project cleans and explores the AB_NYC_2019 Airbnb dataset for New York City. It focuses on preparing the data for analysis, running exploratory data analysis (EDA), and creating clear visualizations of key patterns. The goal is to produce a small, reproducible workflow that demonstrates practical data-wrangling and insight generation.

## What I Built
- A Jupyter notebook that loads, cleans, and analyzes the AB_NYC_2019 dataset.
- Reusable cleaning functions that handle missing values, outliers, and basic formatting.
- EDA functions and visualizations that summarize prices, neighbourhood patterns, and relationships between variables.

## Dataset
- **Name:** AB_NYC_2019 (New York City Airbnb listings)
- **Source:** https://www.kaggle.com/datasets/dgomonov/new-york-city-airbnb-open-data

## How to Run the Project
1. Open a terminal and change into the project folder:
   ```bash
   cd "c:/Users/bmuruges/OneDrive - DPDHL/Documents/Study/AgentQA/crewai"
   ```
2. Install the required Python packages using the provided requirements file:
   ```bash
   pip install -r requirements.txt
   ```
3. Open `data_workflow.ipynb` (or `analysis.ipynb`) in VS Code or Jupyter:
   - In VS Code: open this folder, then open the notebook file and use **Run All**.
   - In classic Jupyter: run `jupyter notebook` and open the notebook from the browser.
4. Execute the cells from top to bottom to reproduce the cleaning, EDA, and visualizations.

## Requirements File
The dependencies for this project are listed in `requirements.txt`. To regenerate this file from your current environment, run:
```bash
pip freeze > requirements.txt
```

To set up a fresh environment, install everything with:
```bash
pip install -r requirements.txt
```

