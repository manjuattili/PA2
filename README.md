# PA2

##What Drives the Price of a Car?

##Used Car Price Model

**Business Understanding:** The data task is to analyze a dataset of 426,000 used cars, characterized by features such as year, manufacturer, model, condition, cylinders, fuel, odometer, title_status, transmission, drive, size, type, and paint_color, to identify and quantify the attributes that most significantly influence the target variable price. By applying statistical modeling, feature selection, and other relevant techniques, the objective is to develop a predictive model that accurately estimates car prices based on these features. The resulting insights will provide actionable recommendations to a used car dealership, highlighting consumer-valued attributes to optimize pricing strategies and align with market demand.

**Data Understanding:** To understand the used car dataset and identify quality issues, I would first inspect the data by reviewing a sample of rows and checking column data types to ensure they align with expected data types, such as numeric for price and categorical for manufacturer. Next, I would analyze missing values to determine their extent and patterns, and examine summary statistics using describe() function to detect outliers. For categorical data, I would examine the frequency of unique values to get an idea on the dominant vs rare categories. By exploring these aspects, I can identify any quality issues within and ensure the dataset is reliable for modeling.

**Data Preparation:** After examining the data, checking for missing values and duplicate values, columns with high missingness-size (~72%), condition (40.79%), cylinders (41.62%), drive (30.59%)and low importance VIN ~38% were dropped, an 'unknown' category was created for high missing - paint_color (30.50%), type (21.75%); and and some low missing categorical columns - manufacturer (4.13%), model (1.24%). Other columns - fuel, title status and transmission were imputed with mode for missing values (<2% missingness). Rows were delete where either the year or odometer column has a missing value (~1% missingness). Next, the odometer values above 300000 were removed as these values are unrealistic and mostly errors. Similarly, price values of 0s and those above 100000 were removed as these either belong to a niche subset or are errors/outliers.

**Modeling:** Correlations were observed for numeric values using a heat map. As expected there were positive and negative correlations with year and odometer respectively - a low positive correlation with year (0.35) and an even lower negative correlation with odometer (-0.51) were observed. Several different regression models were examined, coefficients were studied – there were no 0 values and features were selected based on the coefficients. GridsearchCV was utilized to get the best alpha value. Model was fine-tuned based on previous model results. Below is a table with the model details and r2 score values. (Model #9 is the model with lowest error and highest R2 values).

| Model | X Features | R² - Train | R² - Test | MSE |
|-------|------------|------------|-----------|-------------|
| 1. Linear Regression | year, manufacturer, fuel, odometer, title_status, transmission, type, paint_color, state | 0.548 | 0.544 | 94394261.14 |
| 2. Ridge Regression | year, manufacturer, fuel, odometer, title_status, transmission, type, paint_color, state | 0.548 | 0.544 | 94378978.45 |
| 3. Lasso Regression (α=1) | year, manufacturer, fuel, odometer, title_status, transmission, type, paint_color, state | 0.548 | 0.544 | 94378978.45 |
| 4. Linear Regression | year, manufacturer, odometer | 0.406 | 0.401 | 124117305.87 |
| 5. Linear Regression | year, odometer | 0.299 | 0.296 | 146012502.62 |
| 6. Linear Regression | year | 0.121 | 0.116 | 183239489.06 |
| 7. LASSO (Poly degree=2, α=0.1) | year, manufacturer, odometer | 0.427 | 0.421 | 119929661.46 |
| 8. LASSO (Poly degree=2, α=0.1) | year, manufacturer, fuel, odometer, title_status, transmission, type, paint_color, state | 0.560 | 0.555 | 92167311.15 |
| 9. LASSO (Poly degree=3, α=0.1) | year, manufacturer, fuel, odometer, title_status, transmission, type, paint_color, state | 0.564 | 0.560 | 91144455.49 |
| 10. Ridge (GridSearchCV, best α=10) | year, odometer | 0.299 | 0.296 | 146012463.52 |
| 11. LASSO (Poly degree=3, α=10) | year, manufacturer, fuel, odometer, title_status, transmission, type, paint_color, state | 0.556 | 0.551 | 92989662.12 |
| 12. LASSO (Poly degree=3, α=1) | year, manufacturer, fuel, odometer, title_status, transmission, type, paint_color, state | 0.564 | 0.559 | 91294591.38 |

**Evaluation:** Model #9 from the above table is the recommended model. This uses the features - ‘year', 'manufacturer', 'fuel', 'odometer', 'title_status', 'transmission', 'type', 'paint_color', 'state’ to predict the target value ‘price’ for the used car. 12 different models were run and this model has the lowest error, MSE of 91144455.48 and R2 values for train and test were 0.56 and 0.56 respectively which show that there is no significant overfitting.
