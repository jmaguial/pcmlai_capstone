# PCMLAI CAPSTONE - ADHD/ADD Detection

## Background and Objectives

### Background

Machine learning (ML) and artificial intelligence (AI) are transforming the healthcare landscape, offering powerful tools to assist in the diagnosis and management of complex neurological conditions like Attention Deficit Hyperactivity Disorder (ADHD) and Attention Deficit Disorder (ADD). These disorders, which affect millions of individuals worldwide, are traditionally diagnosed through behavioral assessments and questionnaires.

### Potential use of ML/AI techniques in ADHD/ADD detection

* **Enhanced Diagnostic Accuracy:** ML algorithms can analyze large datasets, identifying subtle patterns and biomarkers that might be missed in traditional diagnostic methods, leading to more accurate and timely detection
* **Early Detection:** AI models can potentially detect early signs of ADHD and ADD in children by analyzing behavioral data and cognitive tests, allowing for earlier interventions that could improve long-term outcomes.
* **Personalized Treatment Plans:** Machine learning can assess an individual's unique data, such as genetic factors and environmental influences, to tailor personalized treatment plans that address their specific needs.
* **Non-Invasive Brain Analysis:** AI-driven tools, such as those analyzing brain imaging data (e.g., fMRI or EEG), can help in identifying brain patterns associated with ADHD and ADD without the need for invasive procedures.

### Scope and Objective

The **scope** of this study is to conduct a comprehensive analysis of multi-dimensional data points that could potentially aid in the detection and diagnosis of ADHD and ADD in children and adolescents aged 0 to 17 years. These data points may include behavioral patterns, cognitive assessments, genetic factors, and environmental influences, which will be systematically evaluated for their relevance in identifying early signs of these neurodevelopmental disorders. 

The overarching **objective** is to design and implement a predictive model that can flag at-risk profiles, providing healthcare professionals with an advanced tool to support the early diagnosis and treatment of ADHD and ADD. This model aims to improve the accuracy and timeliness of identifying these conditions, ultimately contributing to more personalized and effective intervention strategies.

## Data Source

For this study, the **National Survey of Children’s Health (NSCH) survey has been used, ranging data from 2016 to 2022** (1).

The National Survey of Children's Health (NSCH) is a comprehensive, nationally representative survey that provides rich data on the physical and emotional health of children in the United States, as well as the social and environmental factors that impact their well-being. Conducted by the U.S. Census Bureau on behalf of the Health Resources and Services Administration’s Maternal and Child Health Bureau (MCHB), the NSCH focuses on gathering information on various aspects of children's health, including access to healthcare, developmental milestones, family dynamics, and the community and school environments.

### Key Content Areas

* **Physical and Mental Health:** physical and chronic health conditions, including mental health status (e.g.: asthma, ADHD, autism, developmental delays, etc.)
* **Healthcare Access and Utilization:** Focus on the quality and consistency of healthcare, unmet needs for medical and mental health services, and access to specialists.
* **Development and Well-being:** behavioral, emotional, and social development. Assesses developmental delays, emotional challenges, and social skills. 
* **Family and Household Environment:** impact of family structure, parental involvement, and economic conditions. Covers topics such as family stress, parental health, and the relationship between household income and child outcomes.
* **Community and Neighborhood Environment:** assesses how environmental factors, such as exposure to violence or poor housing conditions, affect children’s health.
* **Health Promotion and Prevention:** Focuses on preventive health measures, such as immunizations, nutrition, physical activity, and dental care.

### Structure

NSHC survey includes a screener and topical surveys. The scope of this analysis is limited to topical survey data, since most of screener information is also included in the **topical survey data** (2).

Data includes surveys to households with **children between 0 and 17 years old, from 2016 to 2022**.

Initial set of data contains **551 columns/categories**, distributed in areas such race/ethnicity, geography, family, financial, coexisting conditions, pregnancy, habits, social, education, behavior, insurance coverage, surgery, skills (motor and intellectual), psychological, Exercise/workout/activity, etc. (3)

Over the years, a series of additions, adjustments and imputations have been made to minimize bias and provide corrections, which might result in inaccuracies when considering multi-year analysis. This includes race and ethnicity, height, weight and BMI, breastfeeding corrections, and children with Special Health Care Needs (CSHCN)(4)(5)

## Exploratory Data Analysis (EDA)

Data from topical surveys from 2016 to 2022 has been combined into a single data frame.
* Target column: K2Q31A ‐ ADD/ADHD (T1 T2 T3)

    _Has a doctor or other health care provider EVER told you that this child has Attention Deficit Disorder or Attention‐Deficit/Hyperactivity Disorder, that is, ADD or ADHD? 1 = Yes 2 = No_

Initial set of data after combination: 
* 551 columns (550 categories + 1 Target)
* 279546 rows

Target is NaN in 1881 records (0.6%)

### Decisions
* Rename target column as ‘y’
* Define ‘hhid’ as dataframe index
* Drop records with target column = NaN
* Family Poverty Ratio was averaged across FPL_I1 to FPL_I6 to new column ‘fpl’
* Drop categories which are not common across evaluated timespan (2016-2022)
* Drop imputation flags
* Drop columns asking for currently existing conditions after confirming existing conditions (*_curr)
* Drop columns related to the current diagnosis of ADHD/ADD, as they might introduce confirmation bias if considered as categories instead of target 
* Converted non-numerical categories to numerical (only 2, 'stratum', 'formtype’)
* Replace NaN by median values
* Normalize all columns

### Resulting Dataset
Final dataset: 277665 rows by 331 columns
* Target ‘y’
* 330 numerical normalized categories
Data exploration shows very low levels of correlation for the most part, with the existence of isolated clusters of higher positive and negative correlation values

![Correlation chart](https://github.com/jmaguial/pcmlai_capstone/blob/main/resources/img/EDA_correlation.png)

### Histograms for subset of features for ADHD/ADD and non-ADHD/ADD diagnosed children
Compared histograms for some categories between records with target ‘y’=1 (ADHD/ADD diagnosed) and records with target ‘y’=2 (ADHD/ADD never diagnosed).
For this exploration, the following 2 set of 8 categories where selected. 
#### Situations the children has experienced
* ACE1 ‐ Hard to Cover Basics Like Food or Housing
* ACE3 ‐ Child Experienced ‐ Parent or Guardian Divorced
* ACE4 ‐ Child Experienced ‐ Parent or Guardian Died
* ACE6 ‐ Child Experienced ‐ Adults Slap, Hit, Kick, Punch Others
* ACE7 ‐ Child Experienced ‐ Victim of Violence
* ACE8 ‐ Child Experienced ‐ Lived with Mentally Ill
* ACE9 ‐ Child Experienced ‐ Lived with Person with Alcohol/Drug Problem
* ACE10 ‐ Child Experienced ‐ Treated Unfairly Because of Race
  
![Histograms for ACE features](https://github.com/jmaguial/pcmlai_capstone/blob/main/resources/img/EDA_ACE_hist.png)

#### Children behavioral traits
* K7Q70_R ‐ Argues Too Much
* K7Q82_R ‐ Cares About Doing Well in School
* K7Q83_R ‐ Does All Required Homework
* K7Q84_R ‐ Works to Finish Tasks Started
* K7Q85_R ‐ Stays Calm and In Control When Challenged 
* K6Q70_R ‐ Affectionate
* K6Q71_R ‐ Show Interest and Curiosity
* K6Q72_R ‐ Smiles Laughs

![Histograms for K6 and K7 features](https://github.com/jmaguial/pcmlai_capstone/blob/main/resources/img/EDA_K6K7_hist.png)

## Modeling

The objective of the model is to analyze if a diagnose of ADHD/ADD could be advanced because of the children socio-economic context, behavioral patterns and skills.

This is a classification problem with high unbalance.

![Distribution of ADHD/ADD Diagnosis](https://github.com/jmaguial/pcmlai_capstone/blob/main/resources/img/Model_Target_hist_distribution.png)

### Model specifications

* Defined ADHD/ADD diagnose as target
* Generated train/test sets, split at 30%
* Generated a grid using:
  * Random Forest
    * Estimators: 50, 100; Max depth: 5, 10, 15
  * Logistic Regression
    * C: 0.01, 0.1, 1; solver: liblinear
  * KNN
    * neighbors: 3, 5, 7, 11; weights: uniform, distance
  * Decision Tree
    * Max depth: 5, 10, 15; min sample split: 2, 5
  * Gradient Boosting
    * Estimators: 50, 100; learning rate: 0.01, 0.1; Max depth: 3, 5, 7

## Model Results and Findinds

With only about 1% difference in the different scoring dimensions, and considering the dramatic difference in terms of performance, the best overall model is **Decision Tree with {'max_depth': 5, 'min_samples_split': 2}**, achieving the following results in about **41s of execution**:
* Accuracy:		0.912257
* Precision:		0.896317
* Recall: 		0.912257
* F1 Score:		0.898723
  
Second best model, especially considering the gap in execution times, with very similar scoring results, is **Logistic Regression**.

Most **relevant features across models are: hcability, k7q83_r, K7q04r_r, and k7q84_r**

Socio-economic, educational context together with behavioral profiling could be used to automatically flag candidates of suffering some degree of ADHD/ADD. Early intervention in the share of psychological therapy or access to medication could improve both children and their family experience

The confusion matrix highlights a relatively high number of false positives. These results need to be handled with care. 
* In one hand, there seems to be a trend in over-diagnosing patients of all ages with some levels of the ADHD/ADD spectrum, particularly in private practice (6). 
* On the other hand, due to the ambiguous nature of these conditions and recent further understanding of the same, they go unnoticed and underdiagnosed (7), resulting in patients ignoring some conditions and not getting treated for them, which could translate in challenging experiences in many different aspects of life which could be improved through appropriate medication or therapy.

## Special considerations

* Branching. Given the nature of the survey, some questions were only applicable based on age range of the selected child or based on inputs from previous questions. The study does not implement any special treatment for this situation.
* All questions asking or related to medical condition diagnosis have been removed, as they could be highly correlated to the target feature (directly related or resulting effect of ADHD/ADD), to avoid confirmation bias.
* When running the model in the entire data set (combination of .dta files from 2016 to 2022) I reached timeouts in my executions when including SVM, after running the grid for 12+ hours. As a result, I decided to exclude that model from the grid.
* Additionally, SVM was run in a 30% sampled dataset, not throwing any better results than Gradient Bossting, Decision Tree or LR models, at a significantly higher computational effort.

## References:

https://www.census.gov/programs-surveys/nsch.html
https://www.census.gov/programs-surveys/nsch/data/datasets.html
https://www2.census.gov/programs-surveys/nsch/technical-documentation/codebook/2022-NSCH-Topical-Variable-List.pdf
https://www2.census.gov/programs-surveys/nsch/technical-documentation/NSCH_Enhancements_Technical_Document.pdf
https://www2.census.gov/programs-surveys/nsch/technical-documentation/NSCH_Weighting_Revisions.pdf
https://www.bbc.com/news/health-65534449
https://www.ncbi.nlm.nih.gov/pmc/articles/PMC4195639/#:~:text=Attention%2Ddeficit%2Fhyperactivity%20disorder%20(ADHD)%20is%20an%20underdiagnosed,and%20debilitating%20condition%20in%20adults.






