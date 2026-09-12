

## 1. Dataset Source

**Source:** Kaggle

**link:** https://www.kaggle.com/datasets/sibamsamanta07/movies-dataset-45k-films-with-budget-and-revenue?select=movies_metadata.csv

**Dataset Name:** Movies Dataset 45K Films with Budget and Revenue

The dataset contains metadata for **45,466 movies**.

### Collection Information

The exact data collection date is not specified in the dataset documentation.
 The Kaggle page indicates that the dataset was updated recently.

### License

Apache 2.0
________________________________________________________________________________
________________________________________________________________________________

# R1 - Dataset Overview

## 1. Dataset Structure

### Shape

```python
print(df.shape)
```

*Observation:*
The dataset contains 45,466 rows and  24 columns.

### Column Names

```python
print(df.columns.tolist())
```

The dataset contains information related to movie metadata such as title,
budget, revenue, genre, language, ratings, runtime and release details.

### Data Types

```python
print(df.info())
```

**Observation:**

- 20 columns are of object type.
- 4 columns are numeric (`revenue`, `runtime`, `vote_average`, `vote_count`).
- Noticable Things : Here "budget" and 'id' are object type ,so careful about
that. At first make budget as float or int and id should be  int type,after that
perform further things.

---

### ID Uniqueness Check

```python
print(df['id'].duplicated().sum())
```

**Output:** 30

**Observation:**

- The `id` column should uniquely identify each movie.
- However, 30 duplicate IDs were found, indicating that uniqueness is not fully
  maintained in the dataset.
- Duplicate IDs should be investigated before removing any record
- duplicates should only be removed when they are occured by preprocessing 
  techniques errors and data collections.
- like if watching movie duration is same that is not occuring problem but same 
   Id means same record,just create biasness.

________________________________________________________________________________
________________________________________________________________________________

# R2 — Data Quality Audit
- *Nmber of null values are :*


  belongs_to_collection    --> 40972 approx 90.11%

  homepage                 --> 37684 approx 82.88%

  tagline                  --> 25054 approx 55.10%

  overview                   --> 954 approx 2.09%

  poster_path                --> 386 approx 0.84%

  runtime                    --> 263 approx 0.57%

  status                      --> 87 approx 0.19%

  release_date                --> 87 approx 0.19%

  imdb_id                     --> 17 approx 0.038%

  original_language           --> 11 approx 0.024%

  vote_average                 --> 6 approx 0.013%

  vote_count                   --> 6 approx 0.013%

  title                        --> 6 approx 0.013%

  video                        --> 6 approx 0.013%

  spoken_languages             --> 6 approx 0.013%

  revenue                      --> 6 approx 0.013%

  popularity                   --> 5 approx 0.0109%

  production_countries         --> 3 approx 0.0065%

  production_companies         --> 3 approx 0.0065%

  genres                       --> 0

  id                           --> 0

  adult                        --> 0

  budget                       --> 0

  original_title               --> 0

--------------------------

- *Impossible / Invalid Values*

  adult(it contains some additional unwanted text except True and False)

  runtime (contains 0 values)
------------------------------
- *Sentinel Values Pretending to Be Data*

  budget (0 values)

  revenue (0 values)

  runtime (0 values)
------------------------

- *Wrong Data Types*

  budget(the given datatype is object but it should be numerical.)

  id(the datatype is object but it should be int)
----------------------------------

- *Constant or Near-Constant Columns*

  adult(it contains True and False but majority contains False)

  status(it contains majority Release Attribute than other attribute)

________________________________________________________________________________
________________________________________________________________________________

# R3 - Target Analysis

##*Target Variable Selection*

- here we selected Revenue as the target variable because the revenue earned by 
a movie depends on many other factors present in the dataset, such as budget, 
popularity, genre, runtime, release date, production companies, vote count, and 
vote average. These factors can influence how successful a movie becomes 
financially. Since revenue is a numerical value, this is a regression problem.


##*Skewness Analysis*

- The revenue distribution is highly right-skewed. The mean revenue is much 
larger than the median revenue.

  Mean Revenue = 20,038,650

  Median Revenue = 0

- This happens because most movies earn very little revenue or even zero 
revenue, while a few blockbuster movies earn hundreds of millions or even 
billions of dollars. These very large values pull the mean upward, creating a 
large difference between the mean and median.

- so here we can notice that the difference between mean and median is very much.
So , mean >> median .
Thats why the distribution is strongly right skewed bell shaped.

- Because the distribution is highly skewed, the median is a more representative
 measure of central tendency than the mean. For modelling or visualization, a 
 log transformation could also be applied.


##*Class Balance*

- Class balance is not applicable in this project because the target variable 
(revenue) is numerical and not categorical.

- Class balance is only used in classification problems where the target 
contains different classes.

- For example, if we choose the adult column as the target variable, the class 
distribution would be:

  False = 45454 / 45466 × 100 = 99.97%

  True  = 9 / 45466 × 100 = 0.02%

  

This would create a highly imbalanced classification problem because almost all 
records belong to the False class. Such a model may become biased towards the 
majority class.

However, since Revenue is the target variable in this project, class balance 
analysis is not required.

##*Outlier Analysis*

- The revenue column contains many extreme values. Most movies have revenue 
close to 0, while some movies have revenue in billions of dollars.

- These values appear as outliers in the distribution. However, they are not 
errors or incorrect entries. They represent real movies that performed 
exceptionally well at the box office.

- In the movie industry, it is normal for a small number of blockbuster movies 
to earn significantly more revenue than most other movies. Therefore, these 
outliers contain important information and should not be removed without a 
strong reason.

- Keeping these values will help the model learn the difference between 
low-performing movies and highly successful movies.
________________________________________________________________________________
________________________________________________________________________________

#**R4 — Relationships :**

## *Correlation Matrix and Heatmap*

A correlation matrix was generated for the numerical features: Revenue, 
Runtime, Vote Average, and Vote Count. The heatmap helps visualize the 
strength of linear relationships between these variables.

The strongest positive correlation was observed between Revenue and Vote Count
 (0.80), indicating that movies receiving more audience votes generally tend to
generate higher revenue.

In contrast, Runtime (0.13) and Vote Average (0.085) showed weak correlations 
with Revenue, suggesting that these features have only a limited linear 
relationship with the target variable.

---

## *Revenue vs Runtime*

A scatter plot was created between Runtime and Revenue.

Most movies have runtimes between 80 and 200 minutes. Although many high-revenue
movies fall within this range, the points are highly scattered and do not form a
clear linear pattern. Therefore, Runtime alone is not a strong predictor of Revenue.

---

## *Revenue vs Vote Count*

The scatter plot between Vote Count and Revenue shows a noticeable positive trend.

Movies with higher vote counts generally generate higher revenue. This observation
 is consistent with the correlation value of 0.80, making Vote Count one of the 
 most influential features in the dataset.

---

## *Revenue vs Vote Average*

The relationship between Vote Average and Revenue appears weak.

Several movies with moderate ratings generate very high revenue, while some 
highly rated movies generate relatively low revenue. This indicates that 
audience ratings alone cannot fully explain a movie's financial success.

---

## *Revenue vs Adult*

A box plot was used to compare Revenue across the Adult category.

The dataset is highly imbalanced, with almost all movies belonging to the 
non-adult category. Therefore, meaningful comparison between categories is 
limited.

---

## *Example of Misleading Correlation*

Runtime provides an example where correlation can be misleading.

The correlation between Runtime and Revenue is only 0.13, which suggests a very 
weak linear relationship. However, the scatter plot shows that most blockbuster 
movies are concentrated within a runtime range of approximately 100–200 minutes.

This suggests that some relationship may exist, but it is not linear. Since 
Pearson correlation measures only linear relationships, it may underestimate the
importance of Runtime.

________________________________________________________________________________
________________________________________________________________________________

# R5 - Findings

## Finding 1: Multiple Columns Contain Large Numbers of Missing Values

### What is true

  Several columns contain different number of missing data:

- belongs_to_collection: 40,972 missing values (90.11%)
- homepage: 37,684 missing values (82.88%)
- tagline: 25,054 missing values (55.10%)
- overview: 954 missing values
- runtime: 263 missing values

### Why it matters

  If in the data ,the number of missing value is very high then our model cannot 
  predict the correct output or we can say the chances of the right prediction 
  of a model will be decreased,if we take those records which has missing or 
  null value.

### What I would do

- If the dataset contains missing value in high volume then first of all our 
  model do not take the null values. We have to either remove them or fill them
  by mean,median,mode or somthing else as the requirement.

- If a corresponding column contains a huge amount of missing data or null 
  values like above 50% of data then according to the team leader,the column should 
  be deleted from the dataset if it does not effect very much .

---

## Finding 2: Duplicate Records and Duplicate Movie IDs Exist

### What is true

- The dataset contains 17 duplicate rows.
- The 'id' column contains 30 duplicate values.

### Why it matters

  Some movies appear more than once in the dataset. This can increase the movie 
  count incorrectly and affect the results ,actually it causes biasness. Each 
  movie should have a unique ID, so duplicate IDs should be removed."

### What I would do

Investigate duplicate records and duplicate IDs, then remove only those 
confirmed to be true duplicates.

---

## Finding 3: The Adult Column Contains Invalid Values

### What is true

The 'adult' column is expected to contain only 'True' and 'False' values. 
However, several records contain unexpected text values, it looks like movie 
description .
Unique value present for this column is 5 but although it has only two values 
thats why i came to know that something messy inside it.

### Why it matters

This indicates possible data corruption or parsing errors and may lead to 
incorrect categorical analysis.

### What I would do

Identify the affected records and either correct or remove them before further
analysis.

---
## Finding 4: The Budget and id Column has Object datatype 

### What is True

The datatype of budget and id has object type

### Why it matters
If we do not change the type of column then model might throw error while using 
these values for computing other result.

### What I would do
budget and id column should be numerical type, before giving the dataset to the 
model we have to modify the datatype of the columns such that we can conclude 
mean,median,mode or another mathematical operation further.
---

## Finding 5: Revenue Contains an Extremely Large Number of Zero Values

### What is true

- 38,052 movies have revenue equals to 0.
- This represents approximately 83.7% of the dataset.

### Why it matters

Revenue results may be wrong because some movies have revenue as 0. 
These zeros might represent missing data instead of actual earnings

### What I would do

Investigate whether zero indicates missing revenue information and handle such 
records appropriately before modeling.
Also we can use a log transformation to reduce the large numbers.

---

## Finding 6: Runtime Contains Unrealistic Zero Values(impossible values)

### What is true

- 1,558 movies have runtime is equal to 0.

### Why it matters

A movie cannot realistically have a runtime of zero minutes. These values likely
indicate missing or invalid data.

### What I would do

Runtime values equal to 0 were treated as missing values because a movie cannot 
have a runtime of 0 minutes. These missing values can be imputed using the median 
(or mode) of the runtime column.As it numerical column thats why may be median is
more suitable for this.

---

## Finding 7: Vote Count is Highly Right-Skewed or presence of outliers.

### What is true

- Mean vote_count = 109.89
- Median vote_count = 10
- Maximum vote_count = 14,075

### Why it matters

Some movies get many more votes than others,this type of data are called outliers.
These movies make the average vote count higher, so the average is not a good 
representation of most movies.


### What I would do

We can use here a log transformation to reduce the extrme effect of high rated
movies and that make the distribution more balanced.

Instead of this,we also can use IQR method Which is not known to me .
---

## Finding 8: Adult and Status Columns Are Highly Imbalanced

### What is true

- In the adult column, almost all movies are marked as False, 
while only a few movies are marked as True.
- In the status column,the majority of movies have the status Released, 
while other status categories contain very few records.

### Why it matters

The distribution of values is highly imbalanced. This can bias the analysis 
toward the dominant categories (False and Released) and make the minority 
categories less visible. As a result, conclusions drawn from these columns may 
not accurately represent all groups.

### What I would do

- Calculate and report the percentage distribution of each category.
- Analyze minority categories separately when possible.
- If possible ,avoid these type of categories or features in your model if it 
does not effect more or contribute much to the output.
-------

###Finding 9: Sentinel values in revenue
Revenue contains 38,052 zero values (83.7% of records). 
Since it is unlikely that such a large proportion of movies earned exactly zero 
revenue, these values may represent missing or unreported revenue rather than 
genuine zeros.

### What I would do

- log Transformation can be used.




