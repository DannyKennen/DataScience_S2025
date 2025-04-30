Massachusetts Highway Stops
================
Danny Kennen
2025-04-23

- [Grading Rubric](#grading-rubric)
  - [Individual](#individual)
  - [Submission](#submission)
- [Setup](#setup)
  - [**q1** Go to the Stanford Open Policing Project page and download
    the Massachusetts State Police records in `Rds` format. Move the
    data to your `data` folder and match the `filename` to load the
    data.](#q1-go-to-the-stanford-open-policing-project-page-and-download-the-massachusetts-state-police-records-in-rds-format-move-the-data-to-your-data-folder-and-match-the-filename-to-load-the-data)
- [EDA](#eda)
  - [**q2** Do your “first checks” on the dataset. What are the basic
    facts about this
    dataset?](#q2-do-your-first-checks-on-the-dataset-what-are-the-basic-facts-about-this-dataset)
  - [**q3** Check the set of factor levels for `subject_race` and
    `raw_Race`. What do you note about overlap / difference between the
    two
    sets?](#q3-check-the-set-of-factor-levels-for-subject_race-and-raw_race-what-do-you-note-about-overlap--difference-between-the-two-sets)
  - [**q4** Check whether `subject_race` and `raw_Race` match for a
    large fraction of cases. Which of the two hypotheses above is most
    likely, based on your
    results?](#q4-check-whether-subject_race-and-raw_race-match-for-a-large-fraction-of-cases-which-of-the-two-hypotheses-above-is-most-likely-based-on-your-results)
  - [**q5** Compare the *arrest rate*—the fraction of total cases in
    which the subject was arrested—across different factors. Create as
    many visuals (or tables) as you need, but make sure to check the
    trends across all of the `subject` variables. Answer the questions
    under *observations*
    below.](#q5-compare-the-arrest-ratethe-fraction-of-total-cases-in-which-the-subject-was-arrestedacross-different-factors-create-as-many-visuals-or-tables-as-you-need-but-make-sure-to-check-the-trends-across-all-of-the-subject-variables-answer-the-questions-under-observations-below)
  - [**q6** Run the following code and interpret the regression
    coefficients. Answer the the questions under *observations*
    below.](#q6-run-the-following-code-and-interpret-the-regression-coefficients-answer-the-the-questions-under-observations-below)
  - [**q7** Re-fit the logistic regression from q6 setting `"white"` as
    the reference level for `subject_race`. Interpret the the model
    terms and answer the questions
    below.](#q7-re-fit-the-logistic-regression-from-q6-setting-white-as-the-reference-level-for-subject_race-interpret-the-the-model-terms-and-answer-the-questions-below)
  - [**q8** Re-fit the model using a factor indicating the presence of
    contraband in the subject’s vehicle. Answer the questions under
    *observations*
    below.](#q8-re-fit-the-model-using-a-factor-indicating-the-presence-of-contraband-in-the-subjects-vehicle-answer-the-questions-under-observations-below)
  - [**q9** Go deeper: Pose at least one more question about the data
    and fit at least one more model in support of answering that
    question.](#q9-go-deeper-pose-at-least-one-more-question-about-the-data-and-fit-at-least-one-more-model-in-support-of-answering-that-question)
  - [Further Reading](#further-reading)

*Purpose*: In this last challenge we’ll focus on using logistic
regression to study a large, complicated dataset. Interpreting the
results of a model can be challenging—both in terms of the statistics
and the real-world reasoning—so we’ll get some practice in this
challenge.

<!-- include-rubric -->

# Grading Rubric

<!-- -------------------------------------------------- -->

Unlike exercises, **challenges will be graded**. The following rubrics
define how you will be graded, both on an individual and team basis.

## Individual

<!-- ------------------------- -->

| Category | Needs Improvement | Satisfactory |
|----|----|----|
| Effort | Some task **q**’s left unattempted | All task **q**’s attempted |
| Observed | Did not document observations, or observations incorrect | Documented correct observations based on analysis |
| Supported | Some observations not clearly supported by analysis | All observations clearly supported by analysis (table, graph, etc.) |
| Assessed | Observations include claims not supported by the data, or reflect a level of certainty not warranted by the data | Observations are appropriately qualified by the quality & relevance of the data and (in)conclusiveness of the support |
| Specified | Uses the phrase “more data are necessary” without clarification | Any statement that “more data are necessary” specifies which *specific* data are needed to answer what *specific* question |
| Code Styled | Violations of the [style guide](https://style.tidyverse.org/) hinder readability | Code sufficiently close to the [style guide](https://style.tidyverse.org/) |

## Submission

<!-- ------------------------- -->

Make sure to commit both the challenge report (`report.md` file) and
supporting files (`report_files/` folder) when you are done! Then submit
a link to Canvas. **Your Challenge submission is not complete without
all files uploaded to GitHub.**

*Background*: We’ll study data from the [Stanford Open Policing
Project](https://openpolicing.stanford.edu/data/), specifically their
dataset on Massachusetts State Patrol police stops.

``` r
library(tidyverse)
```

    ## ── Attaching core tidyverse packages ──────────────────────── tidyverse 2.0.0 ──
    ## ✔ dplyr     1.1.4     ✔ readr     2.1.5
    ## ✔ forcats   1.0.0     ✔ stringr   1.5.1
    ## ✔ ggplot2   3.5.1     ✔ tibble    3.2.1
    ## ✔ lubridate 1.9.4     ✔ tidyr     1.3.1
    ## ✔ purrr     1.0.2     
    ## ── Conflicts ────────────────────────────────────────── tidyverse_conflicts() ──
    ## ✖ dplyr::filter() masks stats::filter()
    ## ✖ dplyr::lag()    masks stats::lag()
    ## ℹ Use the conflicted package (<http://conflicted.r-lib.org/>) to force all conflicts to become errors

``` r
library(broom)
```

# Setup

<!-- -------------------------------------------------- -->

### **q1** Go to the [Stanford Open Policing Project](https://openpolicing.stanford.edu/data/) page and download the Massachusetts State Police records in `Rds` format. Move the data to your `data` folder and match the `filename` to load the data.

*Note*: An `Rds` file is an R-specific file format. The function
`readRDS` will read these files.

``` r
## TODO: Download the data, move to your data folder, and load it
filename <- "./data/yg821jf8611_ma_statewide_2020_04_01.rds"
df_data <- readRDS(filename)
```

# EDA

<!-- -------------------------------------------------- -->

### **q2** Do your “first checks” on the dataset. What are the basic facts about this dataset?

``` r
glimpse(df_data)
```

    ## Rows: 3,416,238
    ## Columns: 24
    ## $ raw_row_number             <chr> "1", "2", "3", "4", "5", "6", "7", "8", "9"…
    ## $ date                       <date> 2007-06-06, 2007-06-07, 2007-06-07, 2007-0…
    ## $ location                   <chr> "MIDDLEBOROUGH", "SEEKONK", "MEDFORD", "MED…
    ## $ county_name                <chr> "Plymouth County", "Bristol County", "Middl…
    ## $ subject_age                <int> 33, 36, 56, 37, 22, 34, 54, 31, 21, 56, 56,…
    ## $ subject_race               <fct> white, white, white, white, hispanic, white…
    ## $ subject_sex                <fct> male, male, female, male, female, male, mal…
    ## $ type                       <fct> vehicular, vehicular, vehicular, vehicular,…
    ## $ arrest_made                <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE, F…
    ## $ citation_issued            <lgl> TRUE, FALSE, FALSE, FALSE, TRUE, TRUE, TRUE…
    ## $ warning_issued             <lgl> FALSE, TRUE, TRUE, TRUE, FALSE, FALSE, FALS…
    ## $ outcome                    <fct> citation, warning, warning, warning, citati…
    ## $ contraband_found           <lgl> NA, FALSE, NA, NA, NA, NA, NA, NA, NA, NA, …
    ## $ contraband_drugs           <lgl> NA, FALSE, NA, NA, NA, NA, NA, NA, NA, NA, …
    ## $ contraband_weapons         <lgl> NA, FALSE, NA, NA, NA, NA, NA, NA, NA, NA, …
    ## $ contraband_alcohol         <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE, F…
    ## $ contraband_other           <lgl> NA, FALSE, NA, NA, NA, NA, NA, NA, NA, NA, …
    ## $ frisk_performed            <lgl> NA, FALSE, NA, NA, NA, NA, NA, NA, NA, NA, …
    ## $ search_conducted           <lgl> FALSE, TRUE, FALSE, FALSE, FALSE, FALSE, FA…
    ## $ search_basis               <fct> NA, other, NA, NA, NA, NA, NA, NA, NA, NA, …
    ## $ reason_for_stop            <chr> "Speed", NA, NA, NA, NA, "Speed", NA, NA, N…
    ## $ vehicle_type               <chr> "Passenger", "Commercial", "Passenger", "Co…
    ## $ vehicle_registration_state <fct> MA, MA, MA, MA, MA, MA, MA, MA, MA, MA, MA,…
    ## $ raw_Race                   <chr> "White", "White", "White", "White", "Hispan…

``` r
df_data
```

    ## # A tibble: 3,416,238 × 24
    ##    raw_row_number date       location      county_name  subject_age subject_race
    ##    <chr>          <date>     <chr>         <chr>              <int> <fct>       
    ##  1 1              2007-06-06 MIDDLEBOROUGH Plymouth Co…          33 white       
    ##  2 2              2007-06-07 SEEKONK       Bristol Cou…          36 white       
    ##  3 3              2007-06-07 MEDFORD       Middlesex C…          56 white       
    ##  4 4              2007-06-07 MEDFORD       Middlesex C…          37 white       
    ##  5 5              2007-06-07 EVERETT       Middlesex C…          22 hispanic    
    ##  6 6              2007-06-07 MEDFORD       Middlesex C…          34 white       
    ##  7 7              2007-06-07 SOMERVILLE    Middlesex C…          54 hispanic    
    ##  8 8              2007-06-07 HOPKINTON     Middlesex C…          31 hispanic    
    ##  9 9              2007-06-07 SOMERVILLE    Middlesex C…          21 white       
    ## 10 10             2007-06-06 BARNSTABLE    Barnstable …          56 white       
    ## # ℹ 3,416,228 more rows
    ## # ℹ 18 more variables: subject_sex <fct>, type <fct>, arrest_made <lgl>,
    ## #   citation_issued <lgl>, warning_issued <lgl>, outcome <fct>,
    ## #   contraband_found <lgl>, contraband_drugs <lgl>, contraband_weapons <lgl>,
    ## #   contraband_alcohol <lgl>, contraband_other <lgl>, frisk_performed <lgl>,
    ## #   search_conducted <lgl>, search_basis <fct>, reason_for_stop <chr>,
    ## #   vehicle_type <chr>, vehicle_registration_state <fct>, raw_Race <chr>

**Observations**:

- What are the basic facts about this dataset?
  - There are 24 catagories in this dataset, the first column is just
    numbers 1 through 3,416,238 to account for every individual in the
    data set, the rest of the catagories relate to information about the
    individual case, such as race, age, location, and if an arrest was
    made
- …

Note that we have both a `subject_race` and `race_Raw` column. There are
a few possibilities as to what `race_Raw` represents:

- `race_Raw` could be the race of the police officer in the stop
- `race_Raw` could be an unprocessed version of `subject_race`

Let’s try to distinguish between these two possibilities.

### **q3** Check the set of factor levels for `subject_race` and `raw_Race`. What do you note about overlap / difference between the two sets?

``` r
## TODO: Determine the factor levels for subject_race and raw_Race

df_data %>% 
  pull(subject_race) %>% 
  unique
```

    ## [1] white                  hispanic               black                 
    ## [4] asian/pacific islander other                  <NA>                  
    ## [7] unknown               
    ## Levels: asian/pacific islander black hispanic white other unknown

``` r
df_data %>% 
  pull(raw_Race) %>% 
  unique
```

    ## [1] "White"                                        
    ## [2] "Hispanic"                                     
    ## [3] "Black"                                        
    ## [4] "Asian or Pacific Islander"                    
    ## [5] "Middle Eastern or East Indian (South Asian)"  
    ## [6] "American Indian or Alaskan Native"            
    ## [7] NA                                             
    ## [8] "None - for no operator present citations only"
    ## [9] "A"

**Observations**:

- What are the unique values for `subject_race`?
  - white, black, hispanic, asian/pacific islander, other, NA, and
    unknown
- What are the unique values for `raw_Race`?
  - White, Hispanic, Black, Middle Eastern or East Indian (South Asian),
    American Indian or Alaskan Native, None - for no operator present
    citations only, NA, and A
- What is the overlap between the two sets?
  - They both account for White, Hispanic, Black, and Asian/Pacific
    Islanders in their categories
- What is the difference between the two sets?
  - Subject race has two very vague categories (other, and unknown)
    where as raw_Race has categories for South Asians and Middle Eastern
    people as well as Indigenous people. It doesn’t have either other or
    unknown categories, but it does have an option for “None” for
    specific situations, and “A”. I am unsure what “A” corresponds to,
    perhaps its raw_Race’s own version of other/unknown?

### **q4** Check whether `subject_race` and `raw_Race` match for a large fraction of cases. Which of the two hypotheses above is most likely, based on your results?

*Note*: Just to be clear, I’m *not* asking you to do a *statistical*
hypothesis test.

``` r
## TODO: Devise your own way to test the hypothesis posed above.

# Convert both to lowercase character vectors
subj <- tolower(as.character(df_data$subject_race))
raw <- tolower(as.character(df_data$raw_Race))

# Check how many are exactly equal
sum(subj == raw, na.rm = TRUE) / sum(!is.na(subj) & !is.na(raw))
```

    ## [1] 0.9429308

**\`Observations**

Between the two hypotheses:

- `race_Raw` could be the race of the police officer in the stop
- `race_Raw` could be an unprocessed version of `subject_race`

which is most plausible, based on your results?

- race_Raw is probably an unprocessed version of subject_race, their
  overlap is over 94%. Which is especially impressive considering that
  there are a total of 6 categories split between the two sets that are
  only in one of the sets. This means that 94% of the cases fall into
  the 4 overlapping categories and are the same between the two sets of
  data. This high of an overlap suggests that they are reporting the
  same information and subject_race is a processes version of race_Raw

- <div>

  ## Visuals

  </div>

<!-- ------------------------- -->

### **q5** Compare the *arrest rate*—the fraction of total cases in which the subject was arrested—across different factors. Create as many visuals (or tables) as you need, but make sure to check the trends across all of the `subject` variables. Answer the questions under *observations* below.

(Note: Create as many chunks and visuals as you need)

``` r
df_data %>%
  filter(!is.na(arrest_made)) %>%
  group_by(subject_age) %>%
  summarize(arrest_rate = mean(arrest_made)) %>%
  ggplot(aes(x = subject_age, y = arrest_rate)) +
  geom_col() +
  labs(title = "Arrest Rate by Race", y = "Arrest Rate")
```

    ## Warning: Removed 1 row containing missing values or values outside the scale range
    ## (`geom_col()`).

![](c12-policing-assignment_files/figure-gfm/q5%20age%20visual-1.png)<!-- -->

``` r
df_data %>%
  filter(!is.na(arrest_made)) %>%
  group_by(subject_sex) %>%
  summarize(arrest_rate = mean(arrest_made)) %>%
  ggplot(aes(x = subject_sex, y = arrest_rate)) +
  geom_col() +
  labs(title = "Arrest Rate by Sex", y = "Arrest Rate")
```

![](c12-policing-assignment_files/figure-gfm/sex%20visual-1.png)<!-- -->

``` r
df_data %>%
  filter(!is.na(arrest_made)) %>%
  group_by(subject_race) %>%
  summarize(arrest_rate = mean(arrest_made)) %>%
  ggplot(aes(x = subject_race, y = arrest_rate)) +
  geom_col() +
  labs(title = "Arrest Rate by Race", y = "Arrest Rate")
```

![](c12-policing-assignment_files/figure-gfm/race%20visual-1.png)<!-- -->

**Observations**:

- How does `arrest_rate` tend to vary with `subject_age`?
  - There is a huge spike around 10 years old, which seems unrealistic
    however there just may not be many 10 year olds in traffic stops and
    all 10 year olds who are are detained because they are a minor so
    that skews the data. There is another peak around age 25
  - A lot of the lines were removed, likely because they didn’t have a
    reported age and only showed up as “NA”  
    Additionally, there are just some gaps in the plot entirely, this
    suggests that there are just some ages that aren’t represented in
    this data set which is very interesting. They are somewhat evenly
    spaced out as well? Interesting

- How does `arrest_rate` tend to vary with `subject_sex`?
  - Males are more likely to be in this data set in the first place, and
    so it makes sense that they have a higher arrest rate. Females are
    around50% less likely to be arrested then males.

- How does `arrest_rate` tend to vary with `subject_race`?
  - Hispanic has the highest arrest rates, followed by black, other, and
    then white. Interesting considering that white has the largest count
    in this dataset

- <div>

  # Modeling

  </div>

<!-- -------------------------------------------------- -->

We’re going to use a model to study the relationship between `subject`
factors and arrest rate, but first we need to understand a bit more
about *dummy variables*

### **q6** Run the following code and interpret the regression coefficients. Answer the the questions under *observations* below.

``` r
## NOTE: No need to edit; inspect the estimated model terms.
fit_q6 <-
  glm(
    formula = arrest_made ~ subject_age + subject_race + subject_sex,
    data = df_data %>%
      filter(
        !is.na(arrest_made),
        subject_race %in% c("white", "black", "hispanic")
      ),
    family = "binomial"
  )

fit_q6 %>% tidy()
```

    ## # A tibble: 5 × 5
    ##   term                 estimate std.error statistic   p.value
    ##   <chr>                   <dbl>     <dbl>     <dbl>     <dbl>
    ## 1 (Intercept)           -2.67    0.0132      -202.  0        
    ## 2 subject_age           -0.0142  0.000280     -50.5 0        
    ## 3 subject_racehispanic   0.513   0.0119        43.3 0        
    ## 4 subject_racewhite     -0.380   0.0103       -37.0 3.12e-299
    ## 5 subject_sexfemale     -0.755   0.00910      -83.0 0

**Observations**:

- Which `subject_race` levels are included in fitting the model?
  - White, Black, Hispanic
- Which `subject_race` levels have terms in the model?
  - Hispanic, White, Age, and Female

You should find that each factor in the model has a level *missing* in
its set of terms. This is because R represents factors against a
*reference level*: The model treats one factor level as “default”, and
each factor model term represents a change from that “default” behavior.
For instance, the model above treats `subject_sex==male` as the
reference level, so the `subject_sexfemale` term represents the *change
in probability* of arrest due to a person being female (rather than
male).

The this reference level approach to coding factors is necessary for
[technical
reasons](https://www.andrew.cmu.edu/user/achoulde/94842/lectures/lecture10/lecture10-94842.html#why-is-one-of-the-levels-missing-in-the-regression),
but it complicates interpreting the model results. For instance; if we
want to compare two levels, neither of which are the reference level, we
have to consider the difference in their model coefficients. But if we
want to compare all levels against one “baseline” level, then we can
relevel the data to facilitate this comparison.

By default `glm` uses the first factor level present as the reference
level. Therefore we can use
`mutate(factor = fct_relevel(factor, "desired_level"))` to set our
`"desired_level"` as the reference factor.

### **q7** Re-fit the logistic regression from q6 setting `"white"` as the reference level for `subject_race`. Interpret the the model terms and answer the questions below.

``` r
## TODO: Re-fit the logistic regression, but set "white" as the reference
## level for subject_race

fit_q7 <-
  glm(
    formula = arrest_made ~ subject_age + subject_race + subject_sex,
    data = df_data %>%
      filter(
        !is.na(arrest_made),
        subject_race %in% c("white", "black", "hispanic")
        ) %>%
           mutate(subject_race = relevel(factor(subject_race), ref = "white")
                  ),
    family = "binomial"
      )

fit_q7 %>% tidy()
```

    ## # A tibble: 5 × 5
    ##   term                 estimate std.error statistic   p.value
    ##   <chr>                   <dbl>     <dbl>     <dbl>     <dbl>
    ## 1 (Intercept)           -3.05    0.0109      -279.  0        
    ## 2 subject_age           -0.0142  0.000280     -50.5 0        
    ## 3 subject_raceblack      0.380   0.0103        37.0 3.12e-299
    ## 4 subject_racehispanic   0.893   0.00859      104.  0        
    ## 5 subject_sexfemale     -0.755   0.00910      -83.0 0

**Observations**:

- Which `subject_race` level has the highest probability of being
  arrested, according to this model? Which has the lowest probability?
  - Hispanic has the highest probability, and Black has the lowest
    probability, but we cannot see the White probability.
  - As a side note, the subject_raceblack category has a P value of
    3.123e-299 where all the other ones have a p value of 0.
- What could explain this difference in probabilities of arrest across
  race? List **multiple** possibilities.
  - The demographics of Massachusetts might lean to having more Hispanic
    people then black people in areas that are more heavily paroled my
    cops
  - Hispanic people are more likely to be targeted by ICE and are more
    likely to be profiled and pulled over on the basis of immigration
    status
  - If a Hispanic person is an immigrant and they were doing something
    illegal they are much more likely to be detained as opposed to
    getting a warning or a ticket
- Look at the set of variables in the dataset; do any of the columns
  relate to a potential explanation you listed?
  - If there is a location bias, such as more Hispanic people living in
    one location, district could help us find that out
  - Data about outcome could help us figure out if there is a bias
    against Hispanics for arrests specifically or if all outcomes are
    hightened
  - search_conducted could indicate bias in search rates

One way we can explain differential arrest rates is to include some
measure indicating the presence of an arrestable offense. We’ll do this
in a particular way in the next task.

### **q8** Re-fit the model using a factor indicating the presence of contraband in the subject’s vehicle. Answer the questions under *observations* below.

``` r
## TODO: Repeat the modeling above, but control for whether contraband was found
## during the police stop
fit_q8 <-
  glm(
    formula = arrest_made ~ subject_age + subject_race + subject_sex + contraband_found,
    data = df_data %>%
      filter(
        !is.na(arrest_made),
        subject_race %in% c("white", "black", "hispanic")
        ) %>%
           mutate(contraband_found = relevel(factor(contraband_found), ref = "TRUE")
           ) %>%
      mutate(subject_race = relevel(factor(subject_race), ref = "white")
           ),
    family = "binomial"
      )

fit_q8%>% tidy()
```

    ## # A tibble: 6 × 5
    ##   term                  estimate std.error statistic   p.value
    ##   <chr>                    <dbl>     <dbl>     <dbl>     <dbl>
    ## 1 (Intercept)            -1.11    0.0303      -36.8  4.44e-296
    ## 2 subject_age             0.0225  0.000866     26.0  2.19e-149
    ## 3 subject_raceblack      -0.0511  0.0270       -1.90 5.80e-  2
    ## 4 subject_racehispanic    0.221   0.0237        9.31 1.32e- 20
    ## 5 subject_sexfemale      -0.306   0.0257      -11.9  1.06e- 32
    ## 6 contraband_foundFALSE  -0.609   0.0192      -31.7  4.29e-221

**Observations**:

- How does controlling for found contraband affect the `subject_race`
  terms in the model?
  - The estimate values decrease pretty significantly with
    subject_raceblack being -0.05? I’ll be honest I don’t understand the
    negative values but I think it has to do with the fact that we are
    referencing the white probability so all of our numbers are based on
    that, the negative suggests that it’s really not likely
- What does the *finding of contraband* tell us about the stop? What
  does it *not* tell us about the stop?
  - This suggests that finding contraband is not heavily tied to making
    arrests, although subject_racehispanic is still at 22% of arrests
    include contraband.

### **q9** Go deeper: Pose at least one more question about the data and fit at least one more model in support of answering that question.

``` r
  fit_q9 <-
  glm(
    formula = warning_issued ~ subject_age + subject_race + subject_sex,
    data = df_data %>%
      filter(
        !is.na(warning_issued),
        subject_race %in% c("white", "black", "hispanic")
        ) %>%
           mutate(subject_race = relevel(factor(subject_race), ref = "white")
                  ),
    family = "binomial"
      )

fit_q9 %>% tidy()
```

    ## # A tibble: 5 × 5
    ##   term                 estimate std.error statistic   p.value
    ##   <chr>                   <dbl>     <dbl>     <dbl>     <dbl>
    ## 1 (Intercept)           -1.13   0.00375      -300.  0        
    ## 2 subject_age            0.0124 0.0000880     141.  0        
    ## 3 subject_raceblack     -0.112  0.00392       -28.7 1.31e-180
    ## 4 subject_racehispanic  -0.277  0.00414       -66.8 0        
    ## 5 subject_sexfemale      0.236  0.00258        91.4 0

**Observations**:

- Document your question and findings
- Question: What is the race that gets the most warnings and how does it
  differ from the model about arrests?
- Hispanics are most likely to be given a warning then black people,
  this is the same as the model for arrests by race, however the
  percentages of warnings given is much lower then then the arrests.
  This suggests that while black and Hispanic people are more likely to
  to get arrested then white people and white people are more likely to
  get let off with a warning.

## Further Reading

<!-- -------------------------------------------------- -->

- Stanford Open Policing Project
  [findings](https://openpolicing.stanford.edu/findings/).
