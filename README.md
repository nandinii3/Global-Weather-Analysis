# Weather Trend Forecasting — Global Weather Repository
This was a technical assessment where I analyzed a global daily weather dataset and built a temperature forecasting model from scratch. The dataset has around 87,000 rows covering cities worldwide, with 41 features ranging from temperature and humidity to air quality and moon phase.

---

## What I did

I kept the scope realistic and focused on doing a few things well rather than rushing through everything. Here's how I approached it:

**Cleaning the data first.** The raw dataset had empty strings and whitespace values scattered across columns. I replaced those with NaN, dropped any fully empty rows or columns, and saved a clean version before doing anything else. From there I filled remaining nulls with column medians for numeric features and the mode for categoricals.

**Outlier treatment using IQR.** For temperature, precipitation, wind speed, humidity and pressure I calculated the interquartile range and clipped values at 1.5×IQR. I chose clipping over dropping  this way I kept the sample size while removing distortion from extreme readings.

**EDA to understand the data.** Before jumping into models I spent time visualizing things: global temperature over time, monthly precipitation patterns, how different features correlate with each other, and how temperature varies across countries. A few things stood out  humidity and temperature have a clear negative relationship, and `feels_like_celsius` tracks `temperature_celsius` almost perfectly, which makes sense.

**Building the forecasting model.** I aggregated temperature to a daily global average and used that as the time series. My first attempt with Linear Regression and basic features (month, day of year, time index) gave an MAE of around 8°C which wasn't great. Random Forest on the same features improved it to about 1.7°C.

The real improvement came when I added lag features the previous day's temperature, the 7-day lag, 14-day lag, and 7/30-day rolling averages. With those added the Random Forest came down to **MAE: 0.93°C and RMSE: 1.00°C**, which I was pretty happy with. Predicting within 1°C on a global average is solid.

**A note on R².** It came out negative even with the improved model. This isn't a bug  it's a known issue with R² on time series where the test variance is low. When the signal is already stable, beating a simple mean prediction on R² is genuinely hard. MAE and RMSE are more meaningful metrics for this kind of problem and both look good.

---

## Results summary

| Model | MAE | RMSE | R² |
|---|---|---|---|
| Linear Regression | 8.16°C | 8.28°C | -109.5 |
| Random Forest (basic) | 1.66°C | 1.82°C | -4.35 |
| Random Forest + lag features | **0.93°C** | **1.00°C** | -1.07 |

---

## Project structure

```
├── GlobalWeatherRepository1.csv   # cleaned dataset
├── GlobalWeatherRepository1.ipynb      # main notebook
├── requirements.txt
└── README.md
```

---

## How to run it

Clone the repo and install dependencies:

```bash
git clone https://github.com/nandinii3/Global-Weather_Analysis
cd Global-Weather_Analysis
pip install -r requirements.txt
```

Or just open the notebook directly in Google Colab, it's self-contained and runs top to bottom without any setup beyond uploading the CSV.

---

## Requirements

```
pandas
numpy
matplotlib
seaborn
scikit-learn
```

---

## Dataset

Global Weather Repository from Kaggle daily weather readings for cities worldwide.
https://www.kaggle.com/datasets/nelgiriyewithana/global-weather-repository

---

## PM Accelerator

This project was completed as part of a technical assessment for PM Accelerator. PM Accelerator is a product management training program that helps aspiring and early-career PMs build real skills through hands-on projects, mentorship, and a strong professional network. Their mission is to accelerate careers by bridging the gap between learning and doing.

---

*Built with Python · Scikit-learn · Pandas · Matplotlib · Seaborn*
