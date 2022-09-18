# Week7: Data Cleansing_2

- Missing Data Mechanisms
- Missing Data Pattern
- Methods for Handling Missing Values

## Missing Data Mechanisms

:wave: Describe relationships between measured variables and the probability of missing data

:point_up: Deciding upon the method for analysing missing values requires understanding about both the reasons for the missing values and the nature of the data for the missing observations.

:seedling: Three different missingness mechanisms:

- Missing at random
- Missing completely at random
- Missing not at random

### Missing at Random (MAR)

**MAR**: the probability of missing data on a variable is related to some other measured variable (or variables) in the analysis model but not to the values of the variable itself.

<div align=center><img src="./Images/w7_MAR.png" alt="w7_MAR" width = "600"/></div>

Practical issue: no way to confirm that the probability of missing data on Y is solely a function of other measured variables.

Examples

1. A psychologist is studying quality of life in a group of cancer patients and finds that elderly patients and patients with less education have a higher propensity to refuse the quality of life questionnaire.
   - The missingness in the quality of life is related to the age and education

2. An educational researcher is studying reading achievement and finds that Hispanic students have a higher rate of missing data than Caucasian students
   - The missingness in reading achievement is related to the ethic groups of students.

### Missing Completely at Random (MCAR)

**MCAR**: the probability of missing data on a variable is unrelated to other measured variables and is unrelated to the values of the variable itself.

<div align=center><img src="./Images/w7_MCAR.png" alt="w7_MCAR" width = "600"/></div>

MCAR is a more restrictive condition than MAR.
Both MAR and MCAR could be ignorable.

#### Test MCAR

Separate the missing and the complete cases on a particular variable and examine group mean differences on other variables in the data set.

- Univariate T-test Comparisons: It separates the missing and the complete cases on a particular variable and uses a T-test to examine group mean differences on other variables in the data set.
  - A non-significant t test: the data are MCAR
  - A significant T statistic (or alternatively, a large mean difference): the data are MAR or MNAR.
- Little’s MCAR Test: A multivariate extension of the t-test approach that simultaneously evaluates mean differences on every variable in the data set
  - A global test of MCAR that applies to the entire data set