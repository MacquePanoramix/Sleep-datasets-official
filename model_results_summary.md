# Sleep installation model results summary

This file summarizes tested paths for the sleep-installation analysis.

## Strict decision rule

A path is **usable** only when the interpretable held-out model beats the baseline mean model on test RMSE by a meaningful amount and has non-negative / useful test R². Tiny RMSE gains, negative test R², or gains that only appear in a random forest are marked **weak / exploratory only** or **do not use**.

The four output variables kept in scope are:

- stress
- mood / emotion
- memory
- reaction time / attention / processing speed

## Public dataset search notes

Found public leads for cognitive-output data:

| dataset lead | downloadable status | relevant variables | decision for this repo |
|---|---|---|---|
| [Sleep Deprivation & Cognitive Performance, Kaggle](https://www.kaggle.com/datasets/sacramentotechnology/sleep-deprivation-and-cognitive-performance) | public Kaggle page found; local CSV already exists at `data/external_sleep_datasets/sleep_deprivation_dataset_detailed.csv` | Sleep_Quality_Score, N_Back_Accuracy, PVT_Reaction_Time, Stroop_Task_Reaction_Time, Emotion_Regulation_Score, Stress_Level | tested locally; not usable for final installation under strict rule |
| [OpenNeuro ds000201 / Sleepy Brain Project](https://openneuro.org/datasets/ds000201/versions/1.0.3) | public OpenNeuro BIDS dataset | PVT task and sleep deprivation condition; emotional-processing study | good public lead, but not a direct simple PSQI/sleep-quality → cognitive CSV bridge in current repo |
| [Resting-state EEG sleep deprivation dataset, Scientific Data 2024](https://www.nature.com/articles/s41597-024-03268-2) | public dataset described with PVT, PANAS, PSQI | PSQI, PVT, PANAS, sleep deprivation / normal sleep | promising future download target; not tested in this commit |
| [ENIGMA-Sleep cognitive-performance paper / SHIP-TREND + Liege](https://juser.fz-juelich.de/record/1019187) | public paper record, collaboration data may require access | PSG sleep efficiency, PSQI, cognitive scores | not treated as downloadable installation evidence here |

## All tested paths

| section | path | output | predictors | rows | baseline RMSE | linear RMSE | linear test R² | linear RMSE gain | decision | formula | limitations |
|---|---|---|---|---:|---:|---:|---:|---:|---|---|---|
| Sleep efficiency input | coffee + exercise → sleep efficiency | sleep efficiency | Caffeine consumption, Exercise frequency | 407 | 0.1374 | 0.1305 | 0.0981 | 5.02% | **usable** | `Sleep efficiency = 0.743037 +0.000169 * Caffeine consumption +0.023761 * Exercise frequency` | Kaggle Sleep Efficiency CSV; observational, no final stress/mood/cognition output by itself. |
| Sleep efficiency input | alcohol + exercise → sleep efficiency | sleep efficiency | Alcohol consumption, Exercise frequency | 407 | 0.1374 | 0.1203 | 0.2339 | 12.45% | **usable** | `Sleep efficiency = 0.785374 -0.031671 * Alcohol consumption +0.023741 * Exercise frequency` | Kaggle Sleep Efficiency CSV; observational, no final stress/mood/cognition output by itself. |
| Sleep efficiency input | coffee + alcohol + exercise → sleep efficiency | sleep efficiency | Caffeine consumption, Alcohol consumption, Exercise frequency | 407 | 0.1374 | 0.1202 | 0.2343 | 12.52% | **usable** | `Sleep efficiency = 0.785183 +0.000007 * Caffeine consumption -0.031660 * Alcohol consumption +0.023751 * Exercise frequency` | Kaggle Sleep Efficiency CSV; observational, no final stress/mood/cognition output by itself. |
| Sleep efficiency input | coffee only → sleep efficiency | sleep efficiency | Caffeine consumption | 407 | 0.1374 | 0.1370 | 0.0051 | 0.29% | **weak / exploratory only** | `Sleep efficiency = 0.788734 +0.000074 * Caffeine consumption` | Kaggle Sleep Efficiency CSV; observational, no final stress/mood/cognition output by itself. |
| Sleep efficiency input | alcohol only → sleep efficiency | sleep efficiency | Alcohol consumption | 407 | 0.1374 | 0.1273 | 0.1415 | 7.35% | **usable** | `Sleep efficiency = 0.828634 -0.031527 * Alcohol consumption` | Kaggle Sleep Efficiency CSV; observational, no final stress/mood/cognition output by itself. |
| Sleep efficiency input | exercise only → sleep efficiency | sleep efficiency | Exercise frequency | 407 | 0.1374 | 0.1314 | 0.0848 | 4.37% | **weak / exploratory only** | `Sleep efficiency = 0.747507 +0.023495 * Exercise frequency` | Kaggle Sleep Efficiency CSV; observational, no final stress/mood/cognition output by itself. |
| Stress | Quality of Sleep → Stress Level | stress | Quality of Sleep | 374 | 1.8179 | 0.7230 | 0.8375 | 60.23% | **usable** | `Stress Level = 15.136409 -1.336180 * Quality of Sleep` | Sleep Health & Lifestyle; strong for Quality of Sleep → Stress Level, physical activity bridges are weak unless used only as exploratory inputs. |
| Stress | Sleep Duration + Quality of Sleep → Stress Level | stress | Sleep Duration, Quality of Sleep | 374 | 1.8179 | 0.7181 | 0.8397 | 60.50% | **usable** | `Stress Level = 15.273574 -0.048811 * Sleep Duration -1.307376 * Quality of Sleep` | Sleep Health & Lifestyle; strong for Quality of Sleep → Stress Level, physical activity bridges are weak unless used only as exploratory inputs. |
| Stress | Physical Activity Level → Quality of Sleep | stress | Physical Activity Level | 374 | 1.2591 | 1.2202 | 0.0450 | 3.09% | **weak / exploratory only** | `Quality of Sleep = 6.790611 +0.009350 * Physical Activity Level` | Sleep Health & Lifestyle; strong for Quality of Sleep → Stress Level, physical activity bridges are weak unless used only as exploratory inputs. |
| Stress | Physical Activity Level + Daily Steps → Quality of Sleep | stress | Physical Activity Level, Daily Steps | 374 | 1.2591 | 1.2546 | -0.0095 | 0.36% | **weak / exploratory only** | `Quality of Sleep = 7.834944 +0.027298 * Physical Activity Level -0.000309 * Daily Steps` | Sleep Health & Lifestyle; strong for Quality of Sleep → Stress Level, physical activity bridges are weak unless used only as exploratory inputs. |
| Stress | Physical Activity Level → Stress Level | stress | Physical Activity Level | 374 | 1.8179 | 1.8228 | -0.0327 | -0.27% | **do not use** | `Stress Level = 5.236226 +0.001237 * Physical Activity Level` | Sleep Health & Lifestyle; strong for Quality of Sleep → Stress Level, physical activity bridges are weak unless used only as exploratory inputs. |
| Stress | Quality of Sleep + Physical Activity Level → Stress Level | stress | Quality of Sleep, Physical Activity Level | 374 | 1.8179 | 0.7275 | 0.8355 | 59.98% | **usable** | `Stress Level = 14.591243 -1.377640 * Quality of Sleep +0.014119 * Physical Activity Level` | Sleep Health & Lifestyle; strong for Quality of Sleep → Stress Level, physical activity bridges are weak unless used only as exploratory inputs. |
| Stress / anxiety | Sleep efficiency → Daily stress | stress/anxiety | Efficiency | 21 | 15.4814 | 15.4559 | -1.1904 | 0.16% | **weak / exploratory only** | `Daily_stress = 39.844312 -0.037778 * Efficiency` | MMASH n=21; strict rule downgrades negative R² or tiny gains. |
| Stress / anxiety | Sleep efficiency + TST → Daily stress | stress/anxiety | Efficiency, Total Sleep Time (TST) | 21 | 15.4814 | 14.1808 | -0.8439 | 8.40% | **weak / exploratory only** | `Daily_stress = 40.194577 -0.191276 * Efficiency +0.036644 * Total Sleep Time (TST)` | MMASH n=21; strict rule downgrades negative R² or tiny gains. |
| Stress / anxiety | Sleep fragmentation → Daily stress | stress/anxiety | Sleep Fragmentation Index | 21 | 15.4814 | 14.9429 | -1.0474 | 3.48% | **weak / exploratory only** | `Daily_stress = 17.223861 +0.750613 * Sleep Fragmentation Index` | MMASH n=21; strict rule downgrades negative R² or tiny gains. |
| Stress / anxiety | Activity inputs → Sleep efficiency | stress/anxiety | screen_events, caffeine_events, alcohol_events, exercise_events | 21 | 5.1821 | 9.6661 | -3.3724 | -86.53% | **do not use** | `Efficiency = 73.006801 -1.311610 * screen_events -0.000000 * caffeine_events -2.029804 * alcohol_events +3.189994 * exercise_events` | MMASH n=21; strict rule downgrades negative R² or tiny gains. |
| Stress / anxiety | Sleep efficiency → STAI1 | stress/anxiety | Efficiency | 21 | 8.6688 | 7.6527 | -0.0205 | 11.72% | **weak / exploratory only** | `STAI1 = -13.826817 +0.579836 * Efficiency` | MMASH n=21; strict rule downgrades negative R² or tiny gains. |
| Stress / anxiety | Sleep efficiency → STAI2 | stress/anxiety | Efficiency | 21 | 6.5450 | 6.4305 | -7.1048 | 1.75% | **weak / exploratory only** | `STAI2 = -4.716825 +0.506953 * Efficiency` | MMASH n=21; strict rule downgrades negative R² or tiny gains. |
| StudentLife | A: screen use → sleep quality | sleep quality | see formula CSV | 49 | 0.5148 | 0.5113 | -0.2685 | 0.68% | **weak / exploratory only** | `see studentlife_installation_formula_summary.csv` | Small daily joins; screen-use side is weak/tiny and negative R². |
| StudentLife | B: sleep quality → mood | mood | see formula CSV | 38 | 21.2411 | 21.0353 | 0.0169 | 0.97% | **weak / exploratory only** | `see studentlife_installation_formula_summary.csv` | Small daily joins; screen-use side is weak/tiny and negative R². |
| StudentLife | C: sleep quality → stress | stress | see formula CSV | 46 | 6.2771 | 5.1326 | 0.3306 | 18.23% | **usable** | `see studentlife_installation_formula_summary.csv` | Small daily joins; screen-use side is weak/tiny and negative R². |
| Mood/emotion | Sleep efficiency → panas_pos_10 | mood/emotion | Efficiency | 21 | 2.6888 | 2.6004 | 0.0311 | 3.29% | **weak / exploratory only** | `panas_pos_10 = 31.7288 -0.0493 * Efficiency` | MMASH PANAS n≈20–21; no strong interpretable path. |
| Mood/emotion | Sleep efficiency → panas_pos_14 | mood/emotion | Efficiency | 20 | 6.6662 | 9.1335 | -3.8360 | -37.01% | **do not use** | `panas_pos_14 = 57.8669 -0.3734 * Efficiency` | MMASH PANAS n≈20–21; no strong interpretable path. |
| Mood/emotion | Sleep efficiency → panas_pos_18 | mood/emotion | Efficiency | 21 | 7.6801 | 7.7666 | -0.0915 | -1.13% | **do not use** | `panas_pos_18 = 3.6316 +0.2708 * Efficiency` | MMASH PANAS n≈20–21; no strong interpretable path. |
| Mood/emotion | Sleep efficiency → panas_pos_22 | mood/emotion | Efficiency | 21 | 3.8499 | 3.8427 | -0.1377 | 0.19% | **weak / exploratory only** | `panas_pos_22 = 22.7608 -0.0273 * Efficiency` | MMASH PANAS n≈20–21; no strong interpretable path. |
| Mood/emotion | Sleep efficiency → panas_pos_9+1 | mood/emotion | Efficiency | 21 | 5.5037 | 5.4970 | -0.1902 | 0.12% | **weak / exploratory only** | `panas_pos_9+1 = 25.1926 -0.0066 * Efficiency` | MMASH PANAS n≈20–21; no strong interpretable path. |
| Mood/emotion | Sleep efficiency → panas_neg_10 | mood/emotion | Efficiency | 21 | 4.5631 | 4.6072 | -0.0217 | -0.97% | **do not use** | `panas_neg_10 = 1.9660 +0.1461 * Efficiency` | MMASH PANAS n≈20–21; no strong interpretable path. |
| Mood/emotion | Sleep efficiency → panas_neg_14 | mood/emotion | Efficiency | 20 | 4.0764 | 5.0893 | -0.6188 | -24.85% | **do not use** | `panas_neg_14 = -8.3453 +0.2620 * Efficiency` | MMASH PANAS n≈20–21; no strong interpretable path. |
| Mood/emotion | Sleep efficiency → panas_neg_18 | mood/emotion | Efficiency | 21 | 6.3696 | 6.4613 | -1.1220 | -1.44% | **do not use** | `panas_neg_18 = 23.9194 -0.1197 * Efficiency` | MMASH PANAS n≈20–21; no strong interpretable path. |
| Mood/emotion | Sleep efficiency → panas_neg_22 | mood/emotion | Efficiency | 21 | 5.5641 | 5.7346 | -0.3122 | -3.06% | **do not use** | `panas_neg_22 = 8.5329 +0.0694 * Efficiency` | MMASH PANAS n≈20–21; no strong interpretable path. |
| Mood/emotion | Sleep efficiency → panas_neg_9+1 | mood/emotion | Efficiency | 21 | 2.0504 | 2.0299 | 0.0005 | 1.00% | **weak / exploratory only** | `panas_neg_9+1 = 21.1077 -0.0961 * Efficiency` | MMASH PANAS n≈20–21; no strong interpretable path. |
| Cognitive outputs | Sleep Quality Score → N_Back_Accuracy | memory | Sleep_Quality_Score | 60 | 14.8735 | 14.9378 | -0.0433 | -0.43% | **do not use** | `N_Back_Accuracy = 75.5274 +0.0360 * Sleep_Quality_Score` | Small n=60; public Kaggle dataset metadata indicates sleep deprivation/cognitive task fields, but provenance/real-vs-synthetic must be verified before installation use. |
| Cognitive outputs | Sleep Quality Score → PVT_Reaction_Time | reaction time/attention | Sleep_Quality_Score | 60 | 97.2644 | 97.4454 | -0.1696 | -0.19% | **do not use** | `PVT_Reaction_Time = 342.9037 +0.0770 * Sleep_Quality_Score` | Small n=60; public Kaggle dataset metadata indicates sleep deprivation/cognitive task fields, but provenance/real-vs-synthetic must be verified before installation use. |
| Cognitive outputs | Sleep Quality Score → Stroop_Task_Reaction_Time | processing speed/attention | Sleep_Quality_Score | 60 | 1.0006 | 1.0346 | -0.4862 | -3.40% | **do not use** | `Stroop_Task_Reaction_Time = 3.1908 +0.0263 * Sleep_Quality_Score` | Small n=60; public Kaggle dataset metadata indicates sleep deprivation/cognitive task fields, but provenance/real-vs-synthetic must be verified before installation use. |
| Cognitive outputs | Sleep Quality Score → Stress_Level | stress | Sleep_Quality_Score | 60 | 11.9954 | 11.7044 | -0.3025 | 2.43% | **weak / exploratory only** | `Stress_Level = 13.4241 +0.3182 * Sleep_Quality_Score` | Small n=60; public Kaggle dataset metadata indicates sleep deprivation/cognitive task fields, but provenance/real-vs-synthetic must be verified before installation use. |
| Cognitive outputs | Sleep Quality Score → Emotion_Regulation_Score | mood/emotion | Sleep_Quality_Score | 60 | 19.0627 | 20.0453 | -0.4552 | -5.15% | **do not use** | `Emotion_Regulation_Score = 34.6792 +0.7749 * Sleep_Quality_Score` | Small n=60; public Kaggle dataset metadata indicates sleep deprivation/cognitive task fields, but provenance/real-vs-synthetic must be verified before installation use. |

## Final installation decision

### Usable in the final installation

- **Alcohol + exercise → sleep efficiency** is the best current input bridge. It is stronger than coffee + exercise in the local Sleep Efficiency comparison, but it only predicts sleep efficiency and must not be presented as a direct stress, mood, memory, or reaction-time bridge by itself.
- **Sleep Health & Lifestyle: Quality of Sleep → Stress Level** is the strongest final output path. The linear model improves test RMSE substantially and has strong positive test R².
- **Sleep Duration + Quality of Sleep → Stress Level** is also usable, but the simpler Quality of Sleep-only formula is easier to explain in the installation.
- **StudentLife sleep quality + sleep hours → stress** can be used only as cautious supporting evidence because the sample is small.

### Exploratory only / not for final installation

- **MMASH sleep efficiency → Daily_stress** is too weak under the strict rule because held-out test R² is negative and RMSE improvement is tiny or unstable.
- **MMASH sleep efficiency → PANAS mood/emotion variables** is not strong enough for the final installation. Some random-forest comparisons improve RMSE, but interpretable linear formulas are weak, tiny, or negative-R².
- **StudentLife screen use → sleep quality → stress** is complete as a chain, but the screen-use side has negative test R² and only a tiny RMSE gain, so it is exploratory only.
- **StudentLife sleep quality → mood** is weak. The RMSE gain is tiny despite slightly positive linear test R².
- **Sleep quality → memory / N-back, PVT reaction time, Stroop reaction time / processing speed, and emotion regulation** from the local cognitive CSV is not usable for final installation under the strict rule.

## Main limitation

These models are observational. They can support an interactive installation formula, but they do not prove that changing one behavior causes a stress, mood, memory, or reaction-time change.

## Sleep duration → cognitive bridge check (added)

A new notebook, `sleep_duration_cognitive_bridge_analysis.ipynb`, tested whether **sleep duration** predicts memory or processing-speed outcomes more reliably than the current local cognitive CSV. The strict rule for this check is: **usable only if linear test R² > 0 and linear RMSE improvement is at least 5%**.

| dataset | path | output | predictors | rows | baseline RMSE | linear RMSE | linear test R² | linear RMSE gain | random forest RMSE | random forest test R² | decision | formula | limitations |
|---|---|---|---|---:|---:|---:|---:|---:|---:|---:|---|---|---|
| Local cognitive CSV | Sleep_Hours → N_Back_Accuracy | memory | Sleep_Hours | 60 | 14.8735 | 14.7278 | -0.0142 | 0.98% | 14.9776 | -0.0489 | **do not use** | `not usable; tested formula: N_Back_Accuracy = 77.992707 -0.379989 * Sleep_Hours` | Fails strict rule: RMSE gain is below 5% and linear test R² is negative. |
| Local cognitive CSV | Sleep_Hours → PVT_Reaction_Time | reaction time / attention | Sleep_Hours | 60 | 97.2644 | 101.0353 | -0.2574 | -3.88% | 104.2169 | -0.3378 | **do not use** | `not usable; tested formula: PVT_Reaction_Time = 417.881207 -12.996757 * Sleep_Hours` | Worse than the baseline mean model. |
| Local cognitive CSV | Sleep_Hours → Stroop_Task_Reaction_Time | processing speed / attention | Sleep_Hours | 60 | 1.0006 | 1.0192 | -0.4422 | -1.85% | 1.1488 | -0.8323 | **do not use** | `not usable; tested formula: Stroop_Task_Reaction_Time = 3.070420 +0.058317 * Sleep_Hours` | Worse than the baseline mean model. |
| NHANES sleep + cognitive files | sleep duration → memory / word recall | memory / word recall | sleep duration | 0 | n/a | n/a | n/a | n/a | n/a | n/a | **do not use** | `not available; NHANES XPT download blocked or absent` | Automatic download attempts for `SLQ_G`, `CFQ_G`, `SLQ_H`, and `CFQ_H` failed with `403 Forbidden`; no local NHANES XPT files were present to test. |
| NHANES sleep + cognitive files | sleep duration → processing speed / DSST | processing speed / DSST | sleep duration | 0 | n/a | n/a | n/a | n/a | n/a | n/a | **do not use** | `not available; NHANES XPT download blocked or absent` | Automatic download attempts for `SLQ_G`, `CFQ_G`, `SLQ_H`, and `CFQ_H` failed with `403 Forbidden`; no local NHANES XPT files were present to test. |
| NHANES sleep + cognitive files | sleep duration → verbal/category fluency | verbal/category fluency | sleep duration | 0 | n/a | n/a | n/a | n/a | n/a | n/a | **do not use** | `not available; NHANES XPT download blocked or absent` | Automatic download attempts for `SLQ_G`, `CFQ_G`, `SLQ_H`, and `CFQ_H` failed with `403 Forbidden`; no local NHANES XPT files were present to test. |

### Decision from the sleep-duration cognitive bridge check

- **Sleep duration should not be used as an installation predictor for memory, PVT reaction time, or Stroop / processing-speed outcomes from the local cognitive CSV.**
- **NHANES cannot yet be claimed as support for memory, DSST / processing speed, or category fluency in this repo**, because the target files were not available after the automatic download check.
- Therefore, this update does **not** support any claim that sleep duration predicts memory or processing speed under the strict rule.
