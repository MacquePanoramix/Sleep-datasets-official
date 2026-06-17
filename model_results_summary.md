# Model Results Summary

## Bridge Variable Evaluation

### Files inspected

I recursively inspected the repository for the requested datasets, StudentLife RDS files, and existing notebooks/summaries.

Found:

- `data/external_sleep_datasets/Sleep_Efficiency.csv`
- Existing notebooks:
  - `attention_speed_output_analysis.ipynb`
  - `caffeine_input_analysis.ipynb`
  - `exercise_input_analysis.ipynb`
  - `mood_stress_output_analysis.ipynb`
  - `sleep_bridge_variables_analysis.ipynb`
  - `sleep_connection_models_analysis.ipynb`
  - `studentlife_screen_minmax.ipynb`

Not found in this repository:

- `sleep_deprivation_dataset_detailed.csv`
- `phonelock.Rds`
- `Sleep.Rds`
- `Mood.Rds`
- `PerceivedStressScale.Rds`
- `Stress.Rds`

MMASH optional validation check:

- Internet search found the PhysioNet MMASH dataset page.
- Direct download/listing from `https://physionet.org/files/mmash/1.0.0/` failed in this environment with a proxy `403 Forbidden` error.
- Therefore MMASH could not be downloaded or tested here.
- Based on the PhysioNet description only, MMASH contains physical activity, sleep quality, stress, and emotions, but it does not directly solve the fixed installation path unless the needed caffeine and screen-use variables are present and joinable inside the downloaded files.

### Bridge-support table

| connection | dataset | variables used | direct support yes/no | model/test needed | conclusion |
|---|---|---:|---|---|---|
| A. caffeine + exercise → sleep efficiency | `Sleep_Efficiency.csv` | `Caffeine consumption`, `Exercise frequency`, `Sleep efficiency` | Yes | Tested with baseline mean and linear regression | Direct dataset support exists, but predictive strength is weak. Exercise has a clearer positive relationship than caffeine. Linear regression slightly improves over the baseline. |
| B. screen time / phone use → sleep quality | StudentLife `phonelock.Rds` + `Sleep.Rds` | Intended: phone lock/screen-use measures + sleep `rate` | No in current repo | Need RDS files, date/user alignment, and model of phone use → sleep quality rating | Cannot claim support from current files because the needed RDS files are missing. Existing notebooks reference StudentLife paths, but the raw files are not present. |
| C. caffeine + exercise/activity + screen usage → sleep quality/sleep efficiency | MMASH | Intended: caffeine, activity, screen usage, sleep quality/efficiency | No in current repo; optional external check blocked | Need successful MMASH download and variable audit | Not tested. PhysioNet summary suggests activity, sleep quality, stress, and emotions are available, but direct caffeine + screen-use support cannot be claimed without the data files. |
| D. sleep quality → mood | StudentLife `Sleep.Rds` + `Mood.Rds` | Intended: sleep `rate` + mood variables | No in current repo | Need RDS files, user/date merge, baseline and linear model | Cannot claim support from current files because the required StudentLife RDS files are missing. |
| E. sleep quality → stress | StudentLife `Sleep.Rds` + `PerceivedStressScale.Rds` or `Stress.Rds` | Intended: sleep `rate` + stress scale/event variables | No in current repo | Need RDS files, user/date merge, baseline and linear model | Cannot claim support from current files because the required StudentLife RDS files are missing. |
| F. sleep quality score → memory | `sleep_deprivation_dataset_detailed.csv` | `Sleep_Quality_Score` → `N_Back_Accuracy` | No | Need missing CSV, baseline and linear model | Cannot claim support because `sleep_deprivation_dataset_detailed.csv` is missing from the repository. |
| G. sleep quality score / daytime sleepiness → reaction or processing speed | `sleep_deprivation_dataset_detailed.csv` | `Sleep_Quality_Score`, `Daytime_Sleepiness` → `PVT_Reaction_Time`, `Stroop_Task_Reaction_Time` | No | Need missing CSV, baseline and linear model | Cannot claim support because `sleep_deprivation_dataset_detailed.csv` is missing from the repository. |

### Test A: caffeine + exercise → sleep efficiency

Dataset used: `data/external_sleep_datasets/Sleep_Efficiency.csv`

Rows available after dropping missing values in the required variables: **421**

Variables:

- Predictors:
  - `Caffeine consumption`
  - `Exercise frequency`
- Outcome:
  - `Sleep efficiency`

Model comparison on a 25% test split with `random_state=42`:

| model | test rows | R² | MAE | RMSE | decision |
|---|---:|---:|---:|---:|---|
| Baseline mean model | 106 | -0.0088 | 0.1224 | 0.1425 | Reference only |
| Linear regression | 106 | 0.0504 | 0.1131 | 0.1382 | Use only as weak supporting bridge evidence |

Standardized linear coefficients:

| predictor | coefficient |
|---|---:|
| `Caffeine consumption` | 0.0091 |
| `Exercise frequency` | 0.0386 |

Correlations with `Sleep efficiency`:

| variable | correlation |
|---|---:|
| `Caffeine consumption` | 0.0626 |
| `Exercise frequency` | 0.2709 |

Interpretation:

- The dataset directly supports testing this connection.
- The model only explains about **5%** of test-set variance.
- Exercise frequency is more useful than caffeine for this dataset.
- Caffeine is very weak and should not be presented as strong evidence.
- The direction of the caffeine coefficient is positive in this dataset, so it should not be used to invent a simple “more caffeine causes worse sleep efficiency” rule from this file.

Installation formula if used cautiously:

```text
predicted_sleep_efficiency = 0.7932
                           + 0.0091 × z(caffeine_consumption)
                           + 0.0386 × z(exercise_frequency)
```

Decision for this model: **use only as weak input-side support, not as a strong predictor.**

### StudentLife paths B, D, and E

The requested StudentLife files are not present in the repository. Because of that:

- screen time / phone use → sleep quality cannot be tested here;
- sleep quality → mood cannot be tested here;
- sleep quality → stress cannot be tested here.

Existing notebooks contain StudentLife-related analysis plans and path references, but the raw RDS files needed for evidence are absent. Therefore, the StudentLife links should be treated as **not currently evidenced by this repository**.

### Sleep deprivation paths F and G

`sleep_deprivation_dataset_detailed.csv` is missing from the repository.

Therefore:

- `Sleep_Quality_Score → N_Back_Accuracy` cannot be tested;
- `Sleep_Quality_Score + Daytime_Sleepiness → PVT_Reaction_Time` cannot be tested;
- `Sleep_Quality_Score + Daytime_Sleepiness → Stroop_Task_Reaction_Time` cannot be tested.

No memory or reaction-time claims should be made from this missing dataset.

### Decision: can sleep quality replace sleep duration as the bridge?

**Current decision: not yet as a fully valid replacement bridge.**

Reason:

- The only directly testable file in the current repository supports **caffeine + exercise → sleep efficiency**, but the effect is weak.
- The key output-side evidence requested for mood, stress, memory, and reaction/processing speed is not testable because the needed StudentLife RDS files and `sleep_deprivation_dataset_detailed.csv` are missing.
- Since different datasets can only be combined through shared bridge variables, the repository currently has an input-side bridge candidate but not enough output-side evidence to complete the full installation path.

Sleep quality is still a **better conceptual candidate** than sleep duration because it can be operationalized as sleep efficiency, sleep quality rating, sleep quality score, or daytime sleepiness. However, the available files do not yet provide enough direct tested evidence for the complete fixed path:

```text
coffee / caffeine + exercise + screen time
→ sleep quality bridge
→ memory + reaction time / processing speed + mood + stress
```

### Decision: can sleep efficiency alone replace sleep duration?

**No, sleep efficiency alone should not replace sleep duration by itself.**

Reason:

- Sleep efficiency is available and testable in `Sleep_Efficiency.csv`.
- It gives direct input-side support for caffeine/exercise, but the model is weak.
- Sleep efficiency alone does not connect to the required output variables in the available repository files.
- It also does not cover subjective sleep quality or daytime sleepiness, which may be more relevant for mood, stress, memory, and reaction-time outcomes.

Recommended bridge approach:

```text
Use a broader sleep-quality bridge, not sleep efficiency alone.
```

Preferred bridge variables, if the missing datasets are added:

1. sleep efficiency, for input-side physiological sleep quality;
2. sleep quality rating/score, for subjective sleep quality;
3. daytime sleepiness, especially for reaction time and processing speed.

### Final conclusion

The main hypothesis is **promising but not proven with the current repository files**.

Sleep quality should be kept as the preferred replacement candidate for the weak sleep-duration bridge, but it should be defined broadly as:

```text
sleep efficiency + sleep quality rating/score + daytime sleepiness where relevant
```

Sleep efficiency alone is **not enough**.

The strongest evidence currently available is only:

```text
caffeine + exercise → sleep efficiency
```

from `Sleep_Efficiency.csv`, and even that support is weak.

The following evidence is still missing before the bridge can be accepted as valid for the full installation:

- screen time / phone use → sleep quality;
- sleep quality → mood;
- sleep quality → stress;
- sleep quality score → memory;
- sleep quality score / daytime sleepiness → reaction time or processing speed.

Final decision:

```text
Do not replace sleep duration with sleep efficiency alone.
Use sleep quality as the replacement bridge only after adding and testing the missing StudentLife and sleep-deprivation datasets.
```
