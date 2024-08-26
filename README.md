# pcmlai_capstone - Activity 20.1

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
Final dataset: 277665 rows by 423 columns
* Target ‘y’
* 422 numerical normalized categories
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


