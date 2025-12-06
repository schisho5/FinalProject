# Final Project README 

# "Trends and Relationships Between CO₂ Emissions and Temperature: A Global and Country Level Analysis"

## Overview

This project analyzes the relationship between global CO₂ emissions and average temperature changes across several countries using Python, pandas, and statsmodels. Multiple regression and logistic regression models were used to evaluate whether CO₂ emissions predict temperature increases and whether temperature predicts high CO₂ levels in the following year.

All figures, tables, and metrics were generated directly through the code contained in the Quarto (`.qmd`) and linked  file.

---

## Repository Contents

* **FinalProject.html** – The main html document containing code, analysis, figures, and analysis.
* **data/** – The CO₂ and temperature CSV files.
* **README.md** – Project description and instructions.

---

## Data Sources

* **CO₂ Emissions Data:** CSV file of annual CO₂ emissions for each country.
* **Temperature Data:** CSV file containing annual average temperatures.
* Each dataset contains corresponding `Year` and `Country` fields, enabling merging and modeling.

---

## Methods

### 1. **Data Cleaning and Merging**

* Datasets were read using `pandas.read_csv()`.
* Temperature and CO₂ data were merged on `Country` and `Year`.
* New features were created:

  * **HighCO2 (binary)** – whether CO₂ emissions exceeded the country's median.
  * **TempIncrease (binary)** – whether temperature increased the following year.

### 2. **Regression Models**

Two logistic regression models were run:

#### **Model 1: Predicting Next Year's High CO₂ From Temperature**

```
merged = merged.sort_values(["Country","Year"])
merged['HighCO2_next'] = merged.groupby('Country')['HighCO2'].shift(-1)

df2 = merged.dropna(subset=['HighCO2_next','AvgTemp'])

y = df2['HighCO2_next']
X = sm.add_constant(df2['AvgTemp'])

fit2 = sm.Logit(y, X).fit(disp=False)
fit2.summary2()
```

#### **Model 2: Predicting Next Year's Temperature Increase From CO₂**

```
merged['AvgTemp_next'] = merged.groupby('Country')['AvgTemp'].shift(-1)
merged['TempIncrease'] = (merged['AvgTemp_next'] > merged['AvgTemp']).astype(int)

df3 = merged.dropna(subset=['TempIncrease','CO2'])

y = df3['TempIncrease']
X = sm.add_constant(df3['CO2'])

fit3 = sm.Logit(y, X).fit(disp=False)
fit3.summary2()
```

---

## Outputs

### **Figures**

* CO₂ emissions trends over time
* Average temperature trends
* Scatterplots of CO₂ vs. temperature
* Logistic regression probability curves

All plots automatically appear when the `.qmd` is rendered as HTML.

### **Tables**

* Summary statistics for CO₂ and temperature by country
* Logistic regression coefficient tables
---

## Summary of Key Findings

Globally, temperatures have risen over time, while CO₂ emissions in the dataset show a decreasing trend.

Within countries, year-to-year fluctuations in CO₂ and temperature do not show meaningful predictive relationships.

Years with above median CO₂ are actually less likely to have above median temperatures, but this effect is small and reflects noise, not climate trends.

Neither CO₂ predicts next year’s temperature, nor temperature predicts next year’s CO₂.

Overall, climate change signals appear in long term global patterns, not in short term annual variations within countries.

---

## Author

**Siona Chisholm**

Final project for academic coursework.
