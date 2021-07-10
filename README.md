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
|plasma_CA19_9|	Blood plasma levels of CA19-9 U/ml (often elevated in PDAC). Only assessed in 350 patients|
|creatinine|	Creatinine mg/ml, urinary biomarker for kidney function|
|LYVE1| LYVE1 ng/ml, urinary levels of a protein that may have a role in PDAC metastasis (spread to other organs)|
|REG1B|	REG1B ng/ml, urinary levels of a protein associated with pancreas regenration|
|TFF1|	TFF1 ng/ml, urinary levels of a protein assiciated with regeneration and repair of the urinary tract|
|REG1A|	REG1A ng/ml, urinary levels of another protein associated with pancreas regeneration. Only assessed in 306 patients to compare roles of REG1A vs REG1B.|


## Data Cleaning and EDA: 


## Classification Models:

Why recall is the metric we want to track. 

(Table of recall scores for different models)

## Optional Feature Selection:


## Hyperparameter Tuning:


## Results:


## Other Considerations:


## Conclusions:
