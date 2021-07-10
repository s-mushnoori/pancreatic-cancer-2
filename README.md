# pancreatic-cancer-2
Can biomarkers from a urine  sample detect pancreatic cancer?

## Problem Statement:
This project is a spiritual successor to a [previous project](https://github.com/s-mushnoori/diagnosing-pancreatic-cancer). Pancreatic Ductal Adenocarcenoma (PDAC), accounts for more than 90% of cases of pancreatic cancer, and is rarely detected early enough to be effectively treated. The key to reducing deaths related to this type of cancer is early detection. In this quest to enable early detection of pancreatic cancer, this time I utilize a dataset with information about the levels of certain biomarkers in urine samples to determine if pancreatic cancer is distinguishable from control samples.

## Purpose:
Such an analysis could be the first step in developing early detection strategies, where non-invasive tests could identify patients at risk of developing PDAC and recommend further evaluation for high risk samples. This could in theory:
- Reduce the loss of life to the disease through early detection
- Reduce the cost burden on families by detecting cancer in more treatable stages
- Reduce burden on hospitals and physicians

## Data
**Debernardi et al 2020 data.csv:** The dataset contains data from 590 urine samples collected from different centers. Relevent data includes age, sex, and urine levels of 5 important biomarkers in control samples, samples with benign pancreatic conditions as well as PDAC at different stages of the disease. The following are the descriptions of the columns as described in **Debernardi et al 2020 documentation.csv**.

|Column name| Column Description|
|:--|:--|
|sample_id|	Unique string identifying each subject |
|patient_cohort|	Cohort 1,  previously used samples; Cohort 2, newly added samples|
|sample_origin|	Sample Origin between 4 different treatment centers|
|age|	Age in years|
|sex|	Sex, M = Male; F = Female|
|diagnosis|	Diagnosis (1=Control, 2=Benign, 3=PDAC)|
|stage|	Stage for those with cancer; IA, IB, IIA, IIB, III, IV |
|benign_sample_diagnosis|	Name of condition for those with bengign hepatobiliary disease|
|plasma_CA19_9|	Blood plasma levels of CA19-9 (U/ml) (often elevated in PDAC). Only assessed in 350 patients|
|creatinine|	Creatinine level (mg/ml), urinary biomarker for kidney function|
|LYVE1| LYVE1 level (ng/ml), urinary levels of a protein that may have a role in PDAC metastasis (spread to other organs)|
|REG1B|	REG1B level (ng/ml), urinary levels of a protein associated with pancreas regenration|
|TFF1|	TFF1 level (ng/ml), urinary levels of a protein assiciated with regeneration and repair of the urinary tract|
|REG1A|	REG1A level (ng/ml), urinary levels of another protein associated with pancreas regeneration. Only assessed in 306 patients to compare roles of REG1A vs REG1B.|


## Data Cleaning and EDA: 
The  plot below shows the null values in each column. This quick visualisation allows us to get an idea of what the next steps in cleaning would look like. 

![null](https://github.com/s-mushnoori/pancreatic-cancer-2/blob/main/Figures/null.png)

Based on the plot and an understanding of the project goals, the following decisions are made about cleaning the dataset:

- Since the purpose of this project is to use the urinary levels of certain biomarkers to detect presence of PDAC. This means we are not concerned (for now) with the stage of cancer or the type of benign disease.
  - _Create a new output variable called 'has_cancer'_ with values 0 (no) and 1 (yes) using information from the columns 'diagnosis', 'stage', and 'benign_sample_diagnosis'

- The column 'plasma_CA19_9' indicates blood plasma levels of a protein, and was only collected for some of the patients.
  - _Drop 'plasma_CA19_9'_ since we are only interested in urine biomarkers

- The column 'REG1A' was only collected in 306 of the 590 patients
   - _Drop 'REG1A'_ for now. This may be revisited later for more domain specific analysis

- The columns 'sample_id', 'patient_cohort', and 'sample_origin' do not give enough information without more context
  - _Drop these columns_

- The column 'sample_origin' is also not meaningful under the assumption that the same sample collection procedure was used in all centers
  - _Drop sample_origin_

- The column 'sex' is categorical, with two values M and F.
  - _Convert to dummy variable_ for one of the genders

The function `clean_data` performs the cleaning operations and has an optional  argument _cohort_ to take into account the different cohorts that will be tested in this analysis.

A pairplot showed a positive correlation between age and having cancer, but this did not play a specific role in the analysis going forward. 

Frequency distributions show most of the data for biomarker levels are right-skewed.

## Classification Models:
Why recall is the metric we want to track. 

(Table of recall scores for different models)

## Optional Feature Selection:
Correlation heatmaps showed some features weakly correlated to cancer, but dropping these features did not improve the results to statistical significance. In fact, in most runs, the results worsened when these features were dropped, but the changes were statistically insignificant. 

## Hyperparameter Tuning:


## Results:


## Other Considerations:


## Conclusions:
