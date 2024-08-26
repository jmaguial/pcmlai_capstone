# pcmlai_capstone

## Exploratory Data Analysis (EDA)

Data from topical surveys from 2016 to 2022 has been combined into a single data frame.
* Target column: K2Q31A ‐ ADD/ADHD (T1 T2 T3)
* *Has a doctor or other health care provider EVER told you that this child has Attention Deficit Disorder or Attention‐Deficit/Hyperactivity Disorder, that is, ADD or ADHD? 1 = Yes 2 = No

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

