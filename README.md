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
  - In later parts of the analysis, we will take the stages into account and compare early stages with  benign  or control samples

- The column 'plasma_CA19_9' indicates blood plasma levels of a protein, and was only collected for some of the patients.
  - _Drop 'plasma_CA19_9'_ since we are only interested in urine biomarkers

- The column 'REG1A' was only collected in 306 of the 590 patients
   - _Drop 'REG1A'_ for now. This may be revisited later for more domain specific analysis

- The columns 'sample_id', 'patient_cohort', and 'sample_origin' do not give enough information without more context
  - _Drop these columns_

- The column 'sample_origin' is also not meaningful under the assumption that the same sample collection procedure was used in all centers
  - _Drop sample_origin_

- The column 'sex' is categorical, with two values M and F.
  - _Convert to dummy variables_ and drop one. This is done to avoid _multicolinearity_ between two features, where the value of one feature is highly correlated to another (in this case, 'Male' perfectly predicts 'Female')


The function `clean_data` performs the cleaning operations and has an optional  argument _cohort_ to take into account the different cohorts that will be tested in this analysis.

A pairplot showed a positive correlation between age and having cancer, but this did not play a specific role in the analysis going forward. 

Frequency distributions show most of the data for biomarker levels are right-skewed.

## Classification Models:
When measuring the performance of a machine learning model, the accuracy score is a quick and easy tool to compare different models. Accuracy is the number of correct predictions divided by the total number of predictions. However, in the case of medical diagnoses, the accuracy is not always important. A more important question to ask is:

**Of all the disease samples, how many can the model correctly identify as positive?**
This question is answered by the metric _recall_. Recall is defined as the number of true positives divided  by the sum of true positives and false negatives. In other words, a high recall score minimises the number of false negatives. 

**Recall = TP / (TP + FN)**

This is important in medical diagnoses because the cost of flasely classifying a disease sample as no-disease (false negative) is very high. 

Another metric, the _F1 score_ is the harmonic mean of _recall_ and _precision_, which gives a  measure of overall performance.

First, a train-test split was performed to create an 'unseen' dataset for final validation of the selected model. Next, 4 machine learning algorithms were used to classify the samples. A function `get_results` was made to take the dataset, labels, and pipelines for the algorithms as arguments.  `RepeatedStratifiedKFold` was used to perform stratified 10-fold cross validation with 5 repetitions. The mean accuracy, recall and F1 scores were calculated for all 4 models. The results are summarized in the table below: 

|Model|Recall|F1 Score|Accuracy|
|:--|:--:|:--:|:--:|
|Logistic Regression|61.1%|66.7%|80%|
|Random Forest|63.9%|69.9%|81.4%|
|Support Vector Machines|61.7%|68.4%|81.7%|
|k-Nearest Neighbors|62.6%|66.1%|78.9%|

The exact scores will differ with each run of the code, but random forest had the highest recall most often, and was thus chosen as the classification model. 

## Optional Feature Selection:
A correlation heatmap showed some features weakly correlated to cancer. The heatmap below shows these correlations:

![heatmap](https://github.com/s-mushnoori/pancreatic-cancer-2/blob/main/Figures/corr.PNG)

Creatinine levels were very weakly correlated with _has_cancer_. Sex (M) also had a relatively weak correlation.

Ultimately however, dropping these features did not improve the results to statistical significance. In fact, in most runs, the results worsened when these features were dropped (again, statistically insignificant). It was decided to leave these two features in for the final evaluation. 

## Hyperparameter Tuning:
Having selected the final algorithm and set of features, we can perform hyperparameter tuning to optimize the model. A function `tune_hyperparameters` was created to perform 5-fold cross validation with different combinations of hyperparameters using `RandomizedSearchCV`.

## Results:
Below are the results for the final evaluation on the 'unseen' dataset we created earlier through the train-test split. 

Confusion Matrix (Cancer vs. No Cancer):

![fig1](https://github.com/s-mushnoori/pancreatic-cancer-2/blob/main/Figures/Fig%201.png)

The final scores were:

|Metric|Final Score|
|:--|:--:|
|Recall|83.3%|
|F1 Score|75.2%|
|Accuracy|81.4%|

This test set had 177 total samples. Of the 60 cancer samples, the model correctly identified 50 samples (i.e. a recall score of 83.3%). This is a great result!

## Other Considerations:

#### No Cancer vs. Stage I/II Cancer samples

Note that the original goal of this project is to determine the utility of urine anlaysis in the _early_ detection of cancer. This means that we should also investigate the results when we remove all samples with stage III and IV cancer from the dataset. Now the dataset contains samples with no disease or benign disease as 'no cancer' and stage I and II PDAC samples as 'cancer'. 

The results are shown below:

Confusion Matrix (Stage I/II Cancer vs. No Cancer):

![fig2](https://github.com/s-mushnoori/pancreatic-cancer-2/blob/main/Figures/Fig%202.png)

|Metric|Final Score|
|:--|:--:|
|Recall|45.2%|
|F1 Score|58.3%|
|Accuracy|86.5%|

These are terrible results! This can indicate two points:
- The late stage cancer samples are significantly different from early stage cancers
- Control samples and benign samples may be similar enough to reduce the predictive power of the model

#### Control vs. Benign samples

To test this, we can only compare Control (no disease) and Benign samples and ignore the cancer samples for the time being. 

Confusion Matrix (Control vs. Benign):

![fig3](https://github.com/s-mushnoori/pancreatic-cancer-2/blob/main/Figures/Fig%203.png)

|Metric|Final Score|
|:--|:--:|
|Recall|73%|
|F1 Score|75.4%|
|Accuracy|74.6%|

This is quite a good recall score. This is great news, since we now know that the model can reasonably distinguish between control and benign disease. Since some benign hepatobiliary disease are risk factors for PDAC, this could be a step forward in early detection of PDAC.

#### Benign vs. Stage I/II Cancer samples

This leads us to the final question we will look at: Can we distinguish between bengin and early stage cancers?

Confusion Matrix (Benign vs. Stage I/II Cancer):

![fig4](https://github.com/s-mushnoori/pancreatic-cancer-2/blob/main/Figures/Fig%204.png)

|Metric|Final Score|
|:--|:--:|
|Recall|48.4%|
|F1 Score|54.5%|
|Accuracy|73.1%|

Again we see that the model cannot distinguish between bengin samples and early stage cancer samples. 

## Conclusions:

This project demonstrates that there exist minimally invasive diagnostic processes that can aid in the early detection of PDAC. Such tools can be developed, not with the purpose of definitively diagnosing cancer, but rather, to identify patients for further tests and evaluation. Since completely healthy control samples are distinctly different from benign or malignant hepatobiliary disease, and considering that some benign diseases can be precursors to PDAC, the use of models like this one should be used as the primary line of defense against deadly cancers, in the form of routine or annual checkups in high-risk populations. 
