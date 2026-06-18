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


## Sleep Quality to Output Tests

### Dataset search

Requested dataset:

```text
sleep_deprivation_dataset_detailed.csv
```

Recursive search result: **not found in this repository**.

Because the dataset is missing, the requested output-side models could not be fitted without inventing data or connections. The analysis notebook still records the full test plan and produces a results table with conservative **do not use** decisions.

Analysis files created:

- `sleep_quality_output_model_analysis.ipynb`
- `sleep_quality_output_model_results.csv`

### Requested paths and decisions

| path | predictors | target | output | rows used | baseline model | linear regression | stronger comparison | decision |
|---|---|---|---|---:|---|---|---|---|
| A | `Sleep_Quality_Score`, `Sleep_Hours`, `Daytime_Sleepiness` | `N_Back_Accuracy` | memory | 0 | not run | not run | not run | do not use |
| B | `Sleep_Quality_Score`, `Sleep_Hours`, `Daytime_Sleepiness` | `PVT_Reaction_Time` | reaction time | 0 | not run | not run | not run | do not use |
| C | `Sleep_Quality_Score`, `Sleep_Hours`, `Daytime_Sleepiness` | `Stroop_Task_Reaction_Time` | processing speed / attention | 0 | not run | not run | not run | do not use |
| D | `Sleep_Quality_Score`, `Sleep_Hours`, `Daytime_Sleepiness` | `Stress_Level` | stress | 0 | not run | not run | not run | do not use |
| E | `Sleep_Quality_Score`, `Sleep_Hours`, `Daytime_Sleepiness` | `Emotion_Regulation_Score` | mood / emotion regulation | 0 | not run | not run | not run | do not use |

### Decision

Current decision: **do not use sleep quality as output-side evidence from this dataset yet**.

Reason:

- The requested CSV was not present when searched recursively.
- No rows were available for model training or testing.
- No baseline, linear regression, or stronger comparison model can honestly beat a baseline without data.
- Therefore no memory, reaction-time, processing-speed, stress, or emotion-regulation support is claimed from this dataset.

When `sleep_deprivation_dataset_detailed.csv` is added, rerun:

```text
python -m jupyter nbconvert --to notebook --execute sleep_quality_output_model_analysis.ipynb --inplace
```

The notebook will test the baseline mean model, linear regression, and random forest comparison for each requested output, then save RMSE graphs to `Graphs/` when models can be fitted.

## StudentLife Bridge Tests

### Download and file status

Attempted dataset source:

- Zenodo record: `https://zenodo.org/records/3529253`
- Requested file: `dataset_rds.zip`
- Requested local folder: `data/studentlife_rds/`

Download attempts made in this environment:

```text
python urllib download → failed with proxy/tunnel 403 Forbidden
curl download → failed with CONNECT tunnel 403 Forbidden
```

The requested StudentLife RDS archive could not be downloaded from this environment. I also checked the local `data/studentlife_rds/` folder after the failed download attempt. No target RDS files were available there.

Files found locally:

| requested file | found in `data/studentlife_rds/` | used in model |
|---|---:|---:|
| `phonelock.Rds` | no | no |
| `Sleep.Rds` | no | no |
| `Mood.Rds` | no | no |
| `PerceivedStressScale.Rds` | no | no |
| `Stress.Rds` | no | no |
| `PAM.Rds` | no | no |

Large-data handling:

- Added `data/studentlife_rds/dataset_rds.zip` to `.gitignore`.
- Added extracted StudentLife `.Rds` / `.rds` files under `data/studentlife_rds/` to `.gitignore`.
- No large dataset ZIP or RDS file is committed.

### Manual data preparation instructions

Because the environment could not download the Zenodo file, prepare the dataset manually with these steps:

1. Open `https://zenodo.org/records/3529253` in a browser.
2. Download `dataset_rds.zip`.
3. Place it at:

```text
data/studentlife_rds/dataset_rds.zip
```

4. Run the notebook:

```text
python -m jupyter nbconvert --to notebook --execute studentlife_bridge_model_analysis.ipynb --inplace
```

The notebook will extract the archive when needed, search recursively for the requested RDS files, read them with `pyreadr`, and test the bridge paths only from variables present in the files.

### A. Screen time / phone use → sleep quality

Status: **not tested**.

Reason:

- `phonelock.Rds` was not available.
- `Sleep.Rds` was not available.
- Therefore no phone-use variables could be joined to sleep quality variables.

Decision: **not usable yet**.

No support is claimed for screen time / phone use → sleep quality until the StudentLife files are present and a model beats the baseline mean model.

### B. Sleep quality → mood

Status: **not tested**.

Reason:

- `Sleep.Rds` was not available.
- `Mood.Rds` was not available.
- Therefore no sleep quality variables could be joined to mood variables.

Decision: **not usable yet**.

No support is claimed for sleep quality → mood until the StudentLife files are present and a model beats the baseline mean model.

### C. Sleep quality → stress

Status: **not tested**.

Reason:

- `Sleep.Rds` was not available.
- `PerceivedStressScale.Rds` was not available.
- `Stress.Rds` was not available.
- Therefore no sleep quality variables could be joined to stress variables.

Decision: **not usable yet**.

No support is claimed for sleep quality → stress until the StudentLife files are present and a model beats the baseline mean model.

### Is sleep quality now better supported as a replacement bridge?

Current decision: **not yet**.

Sleep quality remains a better conceptual bridge candidate than sleep efficiency alone, but the StudentLife bridge evidence is still missing because the RDS files could not be downloaded in this environment and were not already present locally.

The replacement bridge should only be accepted after the StudentLife notebook can test the actual variables and show that at least one model beats the baseline for the relevant links.

### Limitations

- The Zenodo download was blocked by this environment with a proxy/tunnel `403 Forbidden` error.
- No StudentLife RDS files were available locally after the failed download.
- The StudentLife paths were not silently skipped; they are recorded here as not tested.
- No `sleep_score` variable was used as evidence.
- No causal claim is made.
- No bridge connection is claimed unless it can be tested from real dataset variables and beat the baseline mean model.

## Cognitive Bridge Dataset Search

### Search rules

This search used the fixed evidence rules for the remaining cognitive outputs:

- `sleep_score` is **not** used as evidence.
- Sleep duration is treated as **secondary only**.
- A bridge is only considered directly supportable when one dataset contains both:
  - a sleep-quality bridge variable, and
  - the cognitive output variable.
- No dataset link is treated as a download link unless a public source directly provides or names it.

Analysis file created:

- `cognitive_sleep_quality_bridge_dataset_search.ipynb`

### Candidate dataset table

| dataset name | access link or data location | sleep bridge variables | output variables | can directly test the bridge? | limitations | recommended next action |
|---|---|---|---|---|---|---|
| Exploring the Alignment of Perceived and Measured Sleep Quality with Working Memory using Consumer Wearables | Paper/arXiv: `https://arxiv.org/abs/2507.19491`; DOI: `https://doi.org/10.1145/3745900.3746104`. The arXiv abstract says all experiment data are publicly available, but this search did not find a direct repository/download URL on the public landing pages checked. | Daily subjective sleep self-assessment; REM sleep / prior-night REM sleep; bedtimes; nocturnal heart rate. Sleep duration is secondary only. | N-back scores / working-memory task performance. | **Yes in principle** for sleep quality → memory, because the study directly contains subjective sleep assessment / REM metrics and N-back performance. **Not directly testable in this repo yet** because no raw file was downloaded. | Direct data URL was not located from the checked public pages. Small longitudinal sample reported in abstract: 29 participants over 4–8 weeks. Consumer wearable measurements are not clinical polysomnography. Need raw variable audit before modeling. | First-priority memory dataset. Contact authors or inspect ACM supplemental materials / arXiv source for the data package. If obtained, save only small derived metadata/results in the repo; do not commit large raw data. |
| Cognitive Performance Measurements and the Impact of Sleep Quality Using Wearable and Mobile Sensors | Open-access article: `https://link.springer.com/article/10.1007/s00779-025-01847-7`; arXiv: `https://arxiv.org/abs/2501.15583`. The data availability statement says data will be made openly available on request. | Sleep latency; sleep restfulness; sleep phases; night-time heart rate/HRV; total sleep duration and time in bed as secondary. Exclude sleep score. | PVT reaction-test results; smartphone typing speed / milliseconds between keystrokes as a proxy or complementary cognitive-performance metric. | **Yes in principle** for sleep quality → reaction time / processing speed, because the paper reports a joined Reaction+Sleep dataset with previous-night sleep metrics and PVT results. **Not directly testable in this repo yet** because the data are available on request, not as an immediate public download found here. | Requires requesting access. PVT is alertness/reaction time, not memory. Typing speed is a proxy and should be secondary to PVT. Must exclude `sleep_score` even if present in raw Oura exports. | First-priority reaction-time dataset. Request the data from the authors. After access, model PVT from non-score sleep-quality variables first; use typing speed only as a secondary processing-speed proxy. |
| MAUS: A Dataset for Mental Workload Assessment on N-back Task Using Wearable Sensor | IEEE DataPort landing/download referenced by baseline repo: `http://ieee-dataport.org/4216`; baseline code: `https://github.com/rickwu11/MAUS_dataset_baseline_system`; arXiv: `https://arxiv.org/abs/2111.02561`. | PSQI questionnaire / subjective sleep quality grouping. | N-back task responses / wrong amount / task performance. | **Likely yes** for PSQI + N-back if the IEEE DataPort data include participant-level PSQI and N-back response/performance files. **Not directly testable in this repo** because the DataPort dataset was not downloaded. | Small sample reported in public abstract: 22 subjects. DataPort access may require login/terms. Primarily a mental-workload dataset, not a sleep study. PSQI is baseline/global sleep quality, not daily prior-night sleep quality. | Backup memory dataset only. Download through IEEE DataPort if accessible, then audit whether PSQI scores can be joined to N-back accuracy/error variables by participant. |

### Current decision for the fixed outputs

#### Memory

Recommended candidate:

```text
Exploring the Alignment of Perceived and Measured Sleep Quality with Working Memory using Consumer Wearables
```

Reason:

- It directly targets subjective sleep self-assessment, measured sleep variables such as REM sleep, and N-back working-memory performance.
- It is more relevant than MAUS for the installation because it is a sleep-quality and working-memory study, not mainly a workload-sensing study.

Current decision:

```text
Do not use yet for installation formulas until the public raw data package is obtained and audited.
```

Backup:

```text
MAUS, only if PSQI can be joined to N-back accuracy/error variables in the downloaded files.
```

#### Reaction time / processing speed

Recommended candidate:

```text
Cognitive Performance Measurements and the Impact of Sleep Quality Using Wearable and Mobile Sensors
```

Reason:

- It directly contains prior-night wearable sleep metrics and PVT reaction-test results in a joined Reaction+Sleep dataset according to the article.
- It also contains smartphone typing speed metrics that may support a secondary processing-speed proxy, but PVT should remain the primary output.

Current decision:

```text
Do not use yet for installation formulas until the requested dataset is obtained and audited.
```

### Overall next action

1. Request or locate the raw public data package for the working-memory wearable dataset.
2. Request the Oura + PVT + typing dataset from the authors.
3. Use MAUS only as a backup for `PSQI → N-back` if the first memory dataset cannot be obtained.
4. After any dataset is obtained, create a modeling notebook that reports:
   - rows used;
   - baseline performance;
   - model performance;
   - coefficients or feature importance;
   - final decision: use / do not use;
   - installation formula only if the model is usable.

No large datasets were committed.
