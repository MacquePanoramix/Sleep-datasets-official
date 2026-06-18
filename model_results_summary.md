


## StudentLife Bridge Tests — Actual Local Results

StudentLife was tested locally using available RDS files.

Decision rule:

- **Use with caution** only if the best tested model has lower test RMSE than the baseline mean model.
- **Do not use** if the model does not beat baseline.
- `sleep_score` was **not** used as evidence.

### Results table

| path                          | output        | target                |   rows |   baseline_r2 |   baseline_mae |   baseline_rmse |   linear_train_r2 |   linear_test_r2 |   linear_mae |   linear_rmse | comparison_model   |   comparison_train_r2 |   comparison_test_r2 |   comparison_mae |   comparison_rmse | best_model        | beats_baseline   | decision         | graph_path                                                                                                                       |
|:------------------------------|:--------------|:----------------------|-------:|--------------:|---------------:|----------------:|------------------:|-----------------:|-------------:|--------------:|:-------------------|----------------------:|---------------------:|-----------------:|------------------:|:------------------|:-----------------|:-----------------|:---------------------------------------------------------------------------------------------------------------------------------|
| A: screen use → sleep quality | sleep quality | sleep_quality_problem |     49 |       -0.2858 |         0.4098 |          0.5148 |            0.0039 |          -0.2685 |       0.4053 |        0.5113 | random forest      |                0.2876 |              -0.2543 |           0.3856 |            0.5085 | random forest     | Yes              | use with caution | D:\Documentos\Sleep datasets\sleep_installation_data_analysis\Graphs\studentlife_screen_to_sleep_quality_actual_vs_predicted.png |
| B: sleep quality → mood       | mood          | mood_score            |     38 |       -0.0024 |        18.4183 |         21.2411 |            0.0489 |           0.0169 |      17.8291 |       21.0353 | random forest      |                0.3408 |              -0.1166 |          19.922  |           22.4185 | linear regression | Yes              | use with caution | D:\Documentos\Sleep datasets\sleep_installation_data_analysis\Graphs\studentlife_sleep_to_mood_actual_vs_predicted.png           |
| C: sleep quality → stress     | stress        | stress_score          |     46 |       -0.0013 |         4.875  |          6.2771 |            0.0935 |           0.3306 |       4.2257 |        5.1326 | random forest      |                0.347  |               0.1355 |           4.9421 |            5.8327 | linear regression | Yes              | use with caution | D:\Documentos\Sleep datasets\sleep_installation_data_analysis\Graphs\studentlife_sleep_to_stress_actual_vs_predicted.png         |

### Bridge decision

The following StudentLife paths beat baseline and may be used with caution:

- A: screen use → sleep quality → sleep quality using best model: random forest
- B: sleep quality → mood → mood using best model: linear regression
- C: sleep quality → stress → stress using best model: linear regression

### Limitations

- StudentLife has a small participant sample.
- Daily joins may lose rows when entries do not occur on the same date.
- Phone lock data is a proxy for screen/phone use, not perfect measured screen time.
- These models are observational and do not prove causality.


## StudentLife Installation Formulas

These formulas were extracted from the linear regression models.

They are provided for installation implementation.

`sleep_score` was not used as evidence.

A formula is marked usable only if the linear regression model beats the baseline mean model on test RMSE.

### A: screen use → sleep quality

- Target: `sleep_quality_problem`
- Predictors: `phone_use_proxy`
- Rows used: `49`
- Baseline RMSE: `0.5148`
- Linear RMSE: `0.5113`
- Linear test R²: `-0.2685`
- Decision: **use with caution**

Standardized formula:

```text
sleep_quality_problem = 0.9224 +0.0238 * z(phone_use_proxy)
```

Raw-value formula:

```text
sleep_quality_problem = 0.8633 +0.0003 * phone_use_proxy
```

### B: sleep quality → mood

- Target: `mood_score`
- Predictors: `sleep_quality_problem, sleep_hours`
- Rows used: `38`
- Baseline RMSE: `21.2411`
- Linear RMSE: `21.0353`
- Linear test R²: `0.0169`
- Decision: **use with caution**

Standardized formula:

```text
mood_score = 55.3811 +1.2606 * z(sleep_quality_problem) +2.6240 * z(sleep_hours)
```

Raw-value formula:

```text
mood_score = 27.6911 +3.7101 * sleep_quality_problem +3.2848 * sleep_hours
```

### C: sleep quality → stress

- Target: `stress_score`
- Predictors: `sleep_quality_problem, sleep_hours`
- Rows used: `46`
- Baseline RMSE: `6.2771`
- Linear RMSE: `5.1326`
- Linear test R²: `0.3306`
- Decision: **use with caution**

Standardized formula:

```text
stress_score = 18.2353 +0.4816 * z(sleep_quality_problem) -1.6668 * z(sleep_hours)
```

Raw-value formula:

```text
stress_score = 31.1402 +1.1541 * sleep_quality_problem -1.9451 * sleep_hours
```

### Important interpretation notes

- `sleep_quality_problem` means worse sleep quality when the value is higher.
- `mood_score` means better mood when the value is higher.
- `stress_score` means more stress when the value is higher.
- These formulas are observational and do not prove causality.
- Use them with caution because StudentLife has a small sample and the usable paths only modestly beat baseline.



## StudentLife Bridge Tests — Actual Local Results

StudentLife was tested locally using available RDS files.

Decision rule:

- **Use with caution** only if the best tested model has lower test RMSE than the baseline mean model.
- **Do not use** if the model does not beat baseline.
- `sleep_score` was **not** used as evidence.

### Results table

| path                          | output        | target                |   rows |   baseline_r2 |   baseline_mae |   baseline_rmse |   linear_train_r2 |   linear_test_r2 |   linear_mae |   linear_rmse | comparison_model   |   comparison_train_r2 |   comparison_test_r2 |   comparison_mae |   comparison_rmse | best_model        | beats_baseline   | decision         | graph_path                                                                                                                       |
|:------------------------------|:--------------|:----------------------|-------:|--------------:|---------------:|----------------:|------------------:|-----------------:|-------------:|--------------:|:-------------------|----------------------:|---------------------:|-----------------:|------------------:|:------------------|:-----------------|:-----------------|:---------------------------------------------------------------------------------------------------------------------------------|
| A: screen use → sleep quality | sleep quality | sleep_quality_problem |     49 |       -0.2858 |         0.4098 |          0.5148 |            0.0039 |          -0.2685 |       0.4053 |        0.5113 | random forest      |                0.2876 |              -0.2543 |           0.3856 |            0.5085 | random forest     | Yes              | use with caution | D:\Documentos\Sleep datasets\sleep_installation_data_analysis\Graphs\studentlife_screen_to_sleep_quality_actual_vs_predicted.png |
| B: sleep quality → mood       | mood          | mood_score            |     38 |       -0.0024 |        18.4183 |         21.2411 |            0.0489 |           0.0169 |      17.8291 |       21.0353 | random forest      |                0.3408 |              -0.1166 |          19.922  |           22.4185 | linear regression | Yes              | use with caution | D:\Documentos\Sleep datasets\sleep_installation_data_analysis\Graphs\studentlife_sleep_to_mood_actual_vs_predicted.png           |
| C: sleep quality → stress     | stress        | stress_score          |     46 |       -0.0013 |         4.875  |          6.2771 |            0.0935 |           0.3306 |       4.2257 |        5.1326 | random forest      |                0.347  |               0.1355 |           4.9421 |            5.8327 | linear regression | Yes              | use with caution | D:\Documentos\Sleep datasets\sleep_installation_data_analysis\Graphs\studentlife_sleep_to_stress_actual_vs_predicted.png         |

### Bridge decision

The following StudentLife paths beat baseline and may be used with caution:

- A: screen use → sleep quality → sleep quality using best model: random forest
- B: sleep quality → mood → mood using best model: linear regression
- C: sleep quality → stress → stress using best model: linear regression

### Limitations

- StudentLife has a small participant sample.
- Daily joins may lose rows when entries do not occur on the same date.
- Phone lock data is a proxy for screen/phone use, not perfect measured screen time.
- These models are observational and do not prove causality.


## StudentLife Installation Formulas

These formulas were extracted from the linear regression models.

They are provided for installation implementation.

`sleep_score` was not used as evidence.

A formula is marked usable only if the linear regression model beats the baseline mean model on test RMSE.

### A: screen use → sleep quality

- Target: `sleep_quality_problem`
- Predictors: `phone_use_proxy`
- Rows used: `49`
- Baseline RMSE: `0.5148`
- Linear RMSE: `0.5113`
- Linear test R²: `-0.2685`
- Decision: **use with caution**

Standardized formula:

```text
sleep_quality_problem = 0.9224 +0.0238 * z(phone_use_proxy)
```

Raw-value formula:

```text
sleep_quality_problem = 0.8633 +0.0003 * phone_use_proxy
```

### B: sleep quality → mood

- Target: `mood_score`
- Predictors: `sleep_quality_problem, sleep_hours`
- Rows used: `38`
- Baseline RMSE: `21.2411`
- Linear RMSE: `21.0353`
- Linear test R²: `0.0169`
- Decision: **use with caution**

Standardized formula:

```text
mood_score = 55.3811 +1.2606 * z(sleep_quality_problem) +2.6240 * z(sleep_hours)
```

Raw-value formula:

```text
mood_score = 27.6911 +3.7101 * sleep_quality_problem +3.2848 * sleep_hours
```

### C: sleep quality → stress

- Target: `stress_score`
- Predictors: `sleep_quality_problem, sleep_hours`
- Rows used: `46`
- Baseline RMSE: `6.2771`
- Linear RMSE: `5.1326`
- Linear test R²: `0.3306`
- Decision: **use with caution**

Standardized formula:

```text
stress_score = 18.2353 +0.4816 * z(sleep_quality_problem) -1.6668 * z(sleep_hours)
```

Raw-value formula:

```text
stress_score = 31.1402 +1.1541 * sleep_quality_problem -1.9451 * sleep_hours
```

### Important interpretation notes

- `sleep_quality_problem` means worse sleep quality when the value is higher.
- `mood_score` means better mood when the value is higher.
- `stress_score` means more stress when the value is higher.
- These formulas are observational and do not prove causality.
- Use them with caution because StudentLife has a small sample and the usable paths only modestly beat baseline.



## StudentLife Bridge Tests — Actual Local Results

StudentLife was tested locally using available RDS files.

Decision rule:

- **Use with caution** only if the best tested model has lower test RMSE than the baseline mean model.
- **Do not use** if the model does not beat baseline.
- `sleep_score` was **not** used as evidence.

### Results table

| path                          | output        | target                |   rows |   baseline_r2 |   baseline_mae |   baseline_rmse |   linear_train_r2 |   linear_test_r2 |   linear_mae |   linear_rmse | comparison_model   |   comparison_train_r2 |   comparison_test_r2 |   comparison_mae |   comparison_rmse | best_model        | beats_baseline   | decision         | graph_path                                                                                                                       |
|:------------------------------|:--------------|:----------------------|-------:|--------------:|---------------:|----------------:|------------------:|-----------------:|-------------:|--------------:|:-------------------|----------------------:|---------------------:|-----------------:|------------------:|:------------------|:-----------------|:-----------------|:---------------------------------------------------------------------------------------------------------------------------------|
| A: screen use → sleep quality | sleep quality | sleep_quality_problem |     49 |       -0.2858 |         0.4098 |          0.5148 |            0.0039 |          -0.2685 |       0.4053 |        0.5113 | random forest      |                0.2876 |              -0.2543 |           0.3856 |            0.5085 | random forest     | Yes              | use with caution | D:\Documentos\Sleep datasets\sleep_installation_data_analysis\Graphs\studentlife_screen_to_sleep_quality_actual_vs_predicted.png |
| B: sleep quality → mood       | mood          | mood_score            |     38 |       -0.0024 |        18.4183 |         21.2411 |            0.0489 |           0.0169 |      17.8291 |       21.0353 | random forest      |                0.3408 |              -0.1166 |          19.922  |           22.4185 | linear regression | Yes              | use with caution | D:\Documentos\Sleep datasets\sleep_installation_data_analysis\Graphs\studentlife_sleep_to_mood_actual_vs_predicted.png           |
| C: sleep quality → stress     | stress        | stress_score          |     46 |       -0.0013 |         4.875  |          6.2771 |            0.0935 |           0.3306 |       4.2257 |        5.1326 | random forest      |                0.347  |               0.1355 |           4.9421 |            5.8327 | linear regression | Yes              | use with caution | D:\Documentos\Sleep datasets\sleep_installation_data_analysis\Graphs\studentlife_sleep_to_stress_actual_vs_predicted.png         |

### Bridge decision

The following StudentLife paths beat baseline and may be used with caution:

- A: screen use → sleep quality → sleep quality using best model: random forest
- B: sleep quality → mood → mood using best model: linear regression
- C: sleep quality → stress → stress using best model: linear regression

### Limitations

- StudentLife has a small participant sample.
- Daily joins may lose rows when entries do not occur on the same date.
- Phone lock data is a proxy for screen/phone use, not perfect measured screen time.
- These models are observational and do not prove causality.


## StudentLife Installation Formulas

These formulas were extracted from the linear regression models.

They are provided for installation implementation.

`sleep_score` was not used as evidence.

A formula is marked usable only if the linear regression model beats the baseline mean model on test RMSE.

### A: screen use → sleep quality

- Target: `sleep_quality_problem`
- Predictors: `phone_use_proxy`
- Rows used: `49`
- Baseline RMSE: `0.5148`
- Linear RMSE: `0.5113`
- Linear test R²: `-0.2685`
- Decision: **use with caution**

Standardized formula:

```text
sleep_quality_problem = 0.9224 +0.0238 * z(phone_use_proxy)
```

Raw-value formula:

```text
sleep_quality_problem = 0.8633 +0.0003 * phone_use_proxy
```

### B: sleep quality → mood

- Target: `mood_score`
- Predictors: `sleep_quality_problem, sleep_hours`
- Rows used: `38`
- Baseline RMSE: `21.2411`
- Linear RMSE: `21.0353`
- Linear test R²: `0.0169`
- Decision: **use with caution**

Standardized formula:

```text
mood_score = 55.3811 +1.2606 * z(sleep_quality_problem) +2.6240 * z(sleep_hours)
```

Raw-value formula:

```text
mood_score = 27.6911 +3.7101 * sleep_quality_problem +3.2848 * sleep_hours
```

### C: sleep quality → stress

- Target: `stress_score`
- Predictors: `sleep_quality_problem, sleep_hours`
- Rows used: `46`
- Baseline RMSE: `6.2771`
- Linear RMSE: `5.1326`
- Linear test R²: `0.3306`
- Decision: **use with caution**

Standardized formula:

```text
stress_score = 18.2353 +0.4816 * z(sleep_quality_problem) -1.6668 * z(sleep_hours)
```

Raw-value formula:

```text
stress_score = 31.1402 +1.1541 * sleep_quality_problem -1.9451 * sleep_hours
```

### Important interpretation notes

- `sleep_quality_problem` means worse sleep quality when the value is higher.
- `mood_score` means better mood when the value is higher.
- `stress_score` means more stress when the value is higher.
- These formulas are observational and do not prove causality.
- Use them with caution because StudentLife has a small sample and the usable paths only modestly beat baseline.
