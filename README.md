# Project: Predicting Household Electricity Consumption

### STA 521 Fall 2026

**Submission deadline:** Thursday, December 10, noon.

### Academic integrity

By submitting an assignment, you pledge to uphold the Duke Community Standard:

- I will not lie, cheat, or steal in my academic endeavors;
- I will conduct myself honorably in all my endeavors; and
- I will act if the Standard is compromised.

**Include your AI statement as appropriate, including a full transcript of any use**.

## Project introduction

Two households can live in similarly sized homes, but use very different amounts of electricity. For instance, one might heat its home with natural gas, while the other uses electric resistance heating; one might occupy an apartment shaded by neighboring buildings, while the other cools a detached house through a long, hot summer (welcome to NC). Appliances, household size, insulation, and daily routines introduce further differences.

Understanding these differences matters for anticipating electricity demand and identifying where additional information about a home's energy use would be valuable. For this project, you will investigate how accurately household electricity consumption can be predicted from information about the home, its occupants, its equipment, and the weather.

Our data come from the U.S. Energy Information Administration's **Residential Energy Consumption Survey (RECS)**. RECS combines a household survey with information collected from energy suppliers to describe energy use in occupied primary residences across the United States. The household survey provides information about housing characteristics and energy-related behavior, while energy supplier records provide the basis for estimates of energy consumption and expenditures.

Your task is to predict **annual household electricity consumption in 2020**. You will have labeled observations from some households surveyed in 2020, and you will also have access to a smaller dataset from the 2015 survey, which you may use as historical training data.

The historical data introduce an additional modeling wrinkle: **Does information from five years earlier improve predictions for households in 2020?** More observations may help a model learn relationships, but household characteristics, equipment, behavior, and survey methods can change. You will need to decide whether—and how—the historical observations are useful.

## Prediction task

The response variable is `KWH`: annual total site electricity consumption, measured in kilowatt-hours.

For each household in the testing dataset, submit one prediction of its 2020 electricity consumption (more details below). Predictions must be finite, nonnegative numeric values expressed in the original units of kWh.

The observational unit is a surveyed household occupying a primary residence. The 2015 and 2020 surveys are separate cross-sectional samples and do **not** follow the same households over time.

## Data and supporting files

The following files are provided:

| File | Number of households | Contents |
| --- | ---: | --- |
| `recs_2020_train.csv` | 12,000 | Predictors and observed `KWH` |
| `recs_2015_train.csv` | 5,686 | Historical predictors and observed `KWH` |
| `recs_2020_test.csv` | 6,496 | Predictors only; `KWH` withheld |
| `data_dictionary.csv` | — | Variable descriptions and harmonization details |
| `README.txt` | — | That's this file! Instructions and notes |

The 2020 training and testing datasets were created using a random split of the public-use records. All available 2015 households are included in the historical training dataset.

Each data file contains a `case_id` identifying the household for submission purposes. **Do not use `case_id` as a predictor.**

The predictors cover several aspects of household energy use:

- **Housing characteristics:** housing type, floor area, construction period, rooms, windows, insulation, and draftiness.
- **Household characteristics:** household size, income band, and ownership or rental status.
- **Equipment and behavior:** heating and water-heating fuels, heating equipment, air conditioning, appliances, and selected usage frequencies.
- **Geography and weather:** region, division, urban classification, and heating and cooling degree days.
- **Survey information:** survey year and a state field available for 2020 households.

Read the data dictionary before beginning your analysis. Variable names alone are not sufficient to establish how a predictor should be interpreted.

## Important features of the prepared data

### Harmonization across survey years

The supplied datasets have been prepared to make comparisons and pooled modeling feasible. Categorical codes have been converted to readable labels, and selected variables have been harmonized across years. Some differences do still exist though between years:

- Household size is capped at 7, bedrooms at 6, and total rooms at 15. Values at those limits represent that number or more.
- Income is expressed in common nominal-dollar bands which have **not** been adjusted for inflation.
- Floor-area measurement and estimation methods changed between the two survey cycles. The same reported area may therefore have different measurement properties across years.
- The state field is available in 2020 and takes the value `not_collected` in 2015.
- Homes built during 2016-2020 appear only in the 2020 data.

A survey-year indicator may be useful, but including it does not automatically resolve every difference between the datasets.

### Missing and NA values

The prepared files distinguish several situations:

- `not_applicable` identifies a structurally inapplicable categorical response.
- `unknown` identifies an unavailable categorical response.
- `not_collected` identifies information unavailable for that survey year.
- Blank numeric entries should be read as missing values.

For example, a household without a dishwasher may have no applicable dishwasher-use frequency. Consider that information together with the indicator for dishwasher ownership.

Many values in the original RECS files were already edited or imputed by EIA. The prepared files retain those published values. You are not working with entirely unprocessed survey responses.

### Interpretation of the response

`KWH` represents total site electricity consumption, including electricity consumed from on-site solar generation.

The datasets retain households with unusually high consumption. Consider how these observations affect fitting, validation, and the evaluation metrics.

## Tasks

Your final predictions will be evaluated on 2020 households. Your model-selection strategy should reflect that target. You might consider decisions that materially affect your final model, such as:

- Which predictors and representations to use.
- Whether to transform the response or predictors.
- How to handle missing and inapplicable values.
- Whether interactions or nonlinear relationships improve prediction.
- How tuning parameters are selected.
- Whether additional model complexity produces a meaningful improvement.

Note that using the 2015 data is **optional**. A model trained only on 2020 households is an acceptable final submission.

Choose **one final model** and your assessment of its complexity and limitations. Apply the final procedure to the testing dataset and submit exactly one prediction for each testing household.

## Evaluation metrics

The competition will be ranked by the primary metric of root mean squared error (RMSE) on the withheld 2020 outcomes. Also report the secondary outcome of mean absolute error (MAE).

Both metrics are unweighted, with each evaluation household contributes equally. The competition assesses performance across the held-out sample.

## What to submit

### 1. Testing-set predictions

Submit a CSV file named `predictions.csv` containing exactly two columns:

- `case_id`
- `predicted_KWH`

The file must contain 6,496 rows, with each testing `case_id` appearing exactly once. Do not include an additional row-number column.

Check that predictions are finite, nonnegative, and expressed in kWh.

### 2. Brief manuscript

Submit an accompanying write-up of no more than **three pages, including tables and figures**. Do not include code in the manuscript. Organize the manuscript around the following components:

#### Introduction

State the prediction problem and explain why it is worth investigating. Describe the data source, observational unit, response, and main types of predictors. Briefly describe exploratory findings that motivated your modeling decisions.

#### Methodology

Describe any validation strategies and candidate approaches, and explicitly provideyour final model. Explain any relevant preprocessing, feature construction, tuning, and model-selection decisions. State whether you used the historical data and how that decision was made. Provide enough detail for a reader to understand the procedure without consulting the code.

#### Results

Report any validation RMSE and MAE (if you used a validation split to help your approach). Clearly distinguish validation results from training performance. Use units and descriptive labels that make results understandable in the context of electricity consumption.

#### Discussion

Explain what your results suggest about the predictability of household electricity use. Discuss limitations arising from the predictors, measurement and imputation, differences between survey years, or the population represented by the data. Identify additional information or alternative strategies that might improve predictions.

**Your discussion should follow from your analysis rather than offering a generic list of limitations.**

### 3. Reproducible code

Submit the code needed to reproduce your analysis and final predictions.

Your code should:

- Read the supplied data from clearly specified paths.
- Set random seeds where needed.
- Document package dependencies.
- Reproduce the preprocessing and validation procedure.
- Fit the final model.
- Write `predictions.csv` in the required format.

Provide a short explanation of how to run the code. Avoid undocumented manual edits or steps that depend on objects already present in your workspace.

This may be via any reproducible means (e.g., R, python, etc.).

## Background resources

- [2015 RECS public-use data and documentation](https://www.eia.gov/consumption/residential/data/2015/index.php?view=microdata)
- [2020 RECS public-use data and documentation](https://www.eia.gov/consumption/residential/data/2020/index.php?view=microdata)
- [Comparing the 2020 RECS with previous surveys](https://www.eia.gov/consumption/residential/data/2020/pdf/Comparing_2020_with_previous_RECS.pdf)
