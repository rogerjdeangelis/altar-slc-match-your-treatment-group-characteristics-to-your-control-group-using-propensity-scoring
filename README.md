# altar-slc-match-your-treatment-group-characteristics-to-your-control-group-using-propensity-scoring
Altar slc match your treatment group characteristics to your control group using propensity scoring
    %let pgm=altar-slc-match-your-treatment-group-characteristics-to-your-control-group-using-propensity-scoring;

    %stop_submissions;

    Altar slc match your treatment group characteristics to your control group using propensity scoring

    Too long to post here

    github
    https://github.com/rogerjdeangelis/altar-slc-match-your-treatment-group-characteristics-to-your-control-group-using-propensity-scoring

    hi res graphic
    https://github.com/rogerjdeangelis/altar-slc-match-your-treatment-group-characteristics-to-your-control-group-using-propensity-scoring/blob/main/match_compare.png

    This solution uses the 'persistent work library' defined in the autoexec file, libname workx wpd "d:/wpswrkx";

    Related Repos
    -----------------------------------------------------------------------------------------------------------------------------------
    https://github.com/rogerjdeangelis/utl-propensity-score-matching-sas-psmatch-r-psm-package-matchit
    https://github.com/rogerjdeangelis/utl-select-equal-size-samples-from-NY-voters-where-age-and-gender-are-equal-propensity-scoring
    https://github.com/rogerjdeangelis/utl_propensity_for_students_to_be_in_the_same_classes

    PROBLEM CREATE THIS PLOT

      Match patient characteristics Control group population with your treatment group

         * all      Orginal data
         * matched  Matched subset

         Standardized Mean Difference is the mean difference
         by categories, single, race, education, degree and age


                          Standardized Mean Difference (SMD)
             -1.5      -1.0      -0.5       0.0       0.5       1.0
             --+---------+---------+---------+---------+---------+-----------------+
             | Sample Sizes: w matched sets  | NOTE THE REDUCED MEAN DIFFERENCES|
             |                               |              VAR     SMD   MARKER   |
    Covariate|           Control Treated     |              age    0.063* MATCHED  |Covariates
        sngl + All           185     185  all|*-* matched   age   -0.129  ALL      + single
             | Matched Set    78      78     |              deg    0.055  MATCHED  |
             | Unmatched     107     107     |              deg   -0.228  ALL      |
             |                               |              edu    0.207  MATCHED  |
             |                               |              edu    0.648  ALL      |
        race +  * all-----------------------*|matched       race  -0.046  MATCHED  + race
             |       RACE IS NOW BALANCED    |              race  -1.475  ALL      |
             |                               |              sngl   0.157  MATCHED  |
             |                               |              sngl   0.067  ALL      |
             |                               |                                     |
        edu  +                           matched *--------* all  * NICE REDUCTIONS + educaton
             |Standardized Mean Difference   |                                     |
             | SMD = (MEAN1 - MEAN2)         |                                     |
             |      ------------------       |                                     |
             |    +-------------------  all  |                                     |
        deg  +   /       2         2    *------* matched                           + deg
             |  / (StdDev1 + stdDev0)        |                                     |
             |\/  --------------------       |                                     |
             |            2                  |                                     |
             |                               |                                     |
        age  +                        all *---* matched                            + age
             |                               |                                     |
             |                               |                                     |
             |                               |                                     |
             --+---------+---------+---------+---------+---------+-----------------+
             -1.5      -1.0      -0.5       0.0       0.5       1.0
                        Standardized Mean Difference (SMD)


    Propensity scoring can be thought of like randomization, Randomization
    does natural balancing of covariates.

    For example, if the underlying population ("universe" of patients) has a female-to-male ratio of 55/45,
     and your treatment arm matches this ratio (55/45), but your control arm is imbalanced (75/25), this
    introduces a potential confounder. In this case, sex is a confounding factor because differences in
    outcomes between groups could be due to the unequal sex distribution rather than the effect of the
    treatment itself.

    To address this, you should rebalance the control arm so that its sex
    ratio matches that of the treatment arm (and ideally, the underlying population). This can be done
    through matching, stratification, or statistical adjustment. This process helps ensure that any
    observed differences in outcomes are more likely attributable to the treatment rather than to
    differences in sex distribution[6].

    Sex is a well-known confounder in clinical research
    because it can influence disease risk, treatment response, and outcomes. If not properly
    balanced or adjusted for, it can bias the results and reduce the validity of your conclusions.

    Mathmatical explanation

    The objective function in propensity scoring is to maximize the likelihood
    of correctly predicting treatment assignment based on observed covariates.
    The resulting propensity scores are then used to balance the distribution of these covariates
    between treated and untreated groups, thereby reducing confounding and mimicking ran
    domization.

    In practice, for binary treatments, this propensity score
    is often estimated using logistic regression:


    logitPr(Z=1|X)=b  + b X  + b X  ...   b X
                    0    1 1    2 2        n n

    and the estimated propensity score is the expected value

                                        b0*b2 + b1*x1 + b1 + b2*x2
                                       e
     expected propensity score psm = -------------------------------
                                           b0*b2 + b1*x1 + b2*x2
                                     b1 + e                      + 1
     maximize this?

             /         \
             |  psm    |
       logit | -----   |
             | 1 - psm |
             \         /
    FYI

      CONTENTS

        1 better if you have more controls than treatments?
        2 how caliper is computed

      1 if you have more contols you are likly to lose
        fewer treatmest when matching?

      2 One of the key factors in propensity scoring is caliper.

        Here is how it is calculated.
        0.2 is an empirical finding, seems to work best?

        Run logistic model
        m_ps <- glm(trt ~ age + race + deg + sngl +  edu, data = lalonde, family = binomial)

        Vector of propensity scores
        df$ps <- predict(m_ps, type = "response")

        Vector of log of odds ratio
        df$logit_ps <- log(df$ps / (1 - df$ps))

        Caliper_with  = .2 * standard deviation of the log of the propensity(probability) odds ratioo
        caliper_width <- 0.2 * sd(df$logit_ps)

    /*                    _                             _           _ _ _
      ___ _ __ ___  _ __ | |_ _   _ __      _____  _ __| | ____  __| (_) |__  _ __ __ _ _ __ _   _
     / _ \ `_ ` _ \| `_ \| __| | | |\ \ /\ / / _ \| `__| |/ /\ \/ /| | | `_ \| `__/ _` | `__| | | |
    |  __/ | | | | | |_) | |_| |_| | \ V  V / (_) | |  |   <  >  < | | | |_) | | | (_| | |  | |_| |
     \___|_| |_| |_| .__/ \__|\__, |  \_/\_/ \___/|_|  |_|\_\/_/\_\|_|_|_.__/|_|  \__,_|_|   \__, |
                   |_|        |___/                                                          |___/
    */


    proc datasets lib==workx kill;
    run;quit;


    /*                   _
    (_)_ __  _ __  _   _| |_
    | | `_ \| `_ \| | | | __|
    | | | | | |_) | |_| | |_
    |_|_| |_| .__/ \__,_|\__|
            |_|
    */

    options validvarname=v7;
    libname sd1 sas7bdat "d:/sd1";
    data sd1.have ;
      input trt age race deg edu sngl @@;
    cards4;
    1 37 1 0 11 0  1 28 1 0 08 1  1 24 1 0 10 1  1 20 1 0 10 1  0 29 2 0 06 0  0 19 3 1 13 1  0 29 3 1 12 0  0 55 1 0 04 1
    1 22 2 0 09 1  1 31 1 0 11 0  1 26 1 0 10 1  1 28 1 0 10 1  0 25 3 1 13 0  0 18 1 0 11 1  0 40 3 1 16 0  0 18 1 0 11 1
    1 30 1 1 12 1  1 18 1 0 11 1  1 25 1 0 11 1  1 24 1 1 12 1  0 39 3 0 10 0  0 21 3 0 10 1  0 19 3 0 10 1  0 16 2 0 09 0
    1 27 1 0 11 1  1 25 1 1 12 1  1 18 1 0 11 1  1 19 1 0 08 1  0 33 3 1 12 0  0 24 3 1 12 0  0 16 1 0 06 1  0 53 3 1 12 1
    1 33 1 0 08 1  1 30 1 0 11 0  1 19 1 0 11 1  1 23 1 1 12 1  0 31 3 1 12 0  0 18 3 0 11 1  0 22 3 0 08 0  0 17 1 0 08 1
    1 22 1 0 09 1  1 17 1 0 10 1  1 43 1 0 09 1  1 42 1 0 09 0  0 36 1 1 12 0  0 18 1 0 11 1  0 49 3 1 14 0  0 27 1 1 14 1
    1 23 1 1 12 1  1 37 1 0 09 1  1 27 1 1 13 1  1 25 1 1 13 1  0 35 3 0 07 0  0 26 1 1 12 1  0 47 3 0 09 0  0 37 1 0 08 1
    1 32 1 0 11 1  1 41 1 0 04 0  1 17 1 0 09 1  1 18 1 0 09 1  0 26 2 0 08 1  0 18 1 1 12 1  0 34 2 0 11 0  0 17 1 0 10 1
    1 22 1 1 16 1  1 42 1 1 14 0  1 30 1 0 11 1  1 21 1 1 12 1  0 53 3 1 12 1  0 32 3 0 04 0  0 30 3 0 10 0  0 48 3 1 12 0
    1 33 3 1 12 0  1 22 3 0 11 1  1 26 1 0 10 0  1 27 1 0 10 1  0 28 3 1 12 1  0 18 1 0 11 1  0 22 1 0 11 0  0 55 3 0 07 1
    1 19 1 0 09 1  1 17 1 0 08 1  1 20 1 0 09 1  1 21 1 0 08 1  0 26 2 1 12 1  0 40 3 0 10 0  0 55 3 0 07 0  0 21 3 1 15 1
    1 21 1 1 13 1  1 29 1 0 08 1  1 17 2 0 09 1  1 22 1 0 09 1  0 38 2 0 08 0  0 31 3 0 06 1  0 34 3 1 12 1  0 16 1 0 10 1
    1 18 1 0 08 1  1 35 1 0 10 1  1 20 1 1 12 1  1 31 1 0 04 1  0 23 3 0 08 0  0 46 3 0 07 0  0 22 3 0 11 1  0 17 3 0 10 1
    1 27 1 0 10 0  1 27 1 0 11 1  1 18 1 0 11 1  1 24 1 0 10 0  0 25 3 1 15 0  0 20 2 0 09 0  0 19 3 1 12 1  0 27 3 1 12 0
    1 17 1 0 07 1  1 29 1 0 04 1  1 27 1 1 12 0  1 29 1 0 10 1  0 37 2 0 11 1  0 16 3 0 10 1  0 24 3 0 10 0  0 51 1 0 04 1
    1 19 1 0 10 1  1 28 1 0 09 1  1 21 3 1 12 1  1 29 1 1 12 1  0 48 3 0 07 0  0 18 3 1 12 1  0 36 3 1 12 0  0 39 1 0 02 0
    1 27 1 1 13 1  1 27 1 0 11 1  1 27 1 1 12 1  1 19 3 0 10 1  0 16 3 0 09 1  0 40 2 1 12 1  0 35 3 0 11 0  0 25 3 1 14 1
    1 23 1 0 10 1  1 23 3 0 07 1  1 20 1 1 12 1  1 19 2 0 11 0  0 29 3 0 10 0  0 16 3 0 09 1  0 29 3 0 10 1  0 24 2 0 01 0
    1 40 1 1 12 1  1 45 1 0 05 0  1 19 1 0 10 1  1 31 1 0 09 1  0 22 3 0 11 0  0 16 3 0 10 1  0 24 3 0 09 0  0 21 3 1 18 1
    1 26 1 1 12 1  1 29 1 1 13 1  1 23 1 1 12 1  1 22 1 0 10 0  0 24 1 1 12 0  0 16 3 0 08 1  0 18 3 0 10 1  0 21 3 1 18 1
    1 23 1 0 11 1  1 27 1 0 09 1  1 29 1 1 14 1  1 21 1 0 09 1  0 47 3 0 11 1  0 20 3 0 11 0  0 21 2 1 13 1
    1 41 3 1 14 1  1 46 1 1 13 1  1 18 1 0 10 1  1 17 1 0 10 1  0 34 3 0 11 0  0 32 2 0 06 0  0 29 3 1 13 0
    1 38 3 0 09 1  1 18 1 0 06 1  1 19 1 0 09 1  1 26 1 1 12 0  0 19 3 1 12 0  0 32 1 1 16 1  0 21 3 1 15 1
    1 24 1 0 11 1  1 25 1 1 12 1  1 27 3 1 13 0  1 20 2 0 09 1  0 21 3 1 15 0  0 19 3 1 12 1  0 20 3 1 14 1
    1 18 1 0 10 1  1 28 1 1 15 1  1 18 3 0 11 1  1 19 1 0 10 1  0 28 3 1 13 1  0 17 1 0 05 1  0 19 1 0 06 1
    1 29 1 0 11 0  1 25 3 0 11 1  1 27 1 0 09 0  1 26 1 0 10 1  0 24 3 1 15 1  0 18 1 0 11 1  0 19 1 1 12 1
    1 25 1 0 11 1  1 22 1 1 12 1  1 22 1 1 12 1  1 28 1 0 11 1  0 28 2 0 08 0  0 20 3 1 12 0  0 20 3 1 13 1
    1 27 2 0 10 1  1 21 1 0 09 1  1 23 1 0 10 0  1 22 2 1 12 0  0 23 3 1 12 0  0 19 1 1 13 1  0 26 3 0 05 0
    1 17 1 0 10 1  1 40 1 0 11 1  1 23 2 1 12 1  1 33 1 0 11 1  0 42 3 0 07 0  0 24 3 0 08 0  0 26 2 0 09 1
    1 24 1 0 11 1  1 22 1 0 11 1  1 20 1 0 11 1  1 22 3 1 12 1  0 25 3 1 12 0  0 27 3 1 12 0  0 17 1 0 09 1
    1 17 1 0 10 1  1 25 1 1 12 1  1 17 1 0 09 1  1 29 2 0 10 1  0 27 3 1 12 0  0 19 3 0 09 1  0 28 3 0 08 0
    1 48 1 0 04 1  1 18 1 1 12 1  1 28 1 0 11 1  1 33 1 1 12 0  0 24 3 0 07 0  0 52 1 0 08 0  0 16 3 0 09 0
    1 25 1 0 11 0  1 38 3 1 12 1  1 26 1 0 11 0  1 25 1 1 14 0  0 23 3 1 12 0  0 18 2 1 12 1  0 50 1 0 05 0
    1 20 1 1 12 1  1 27 1 1 13 1  1 20 1 0 11 1  1 35 1 0 09 0  0 50 3 1 12 0  0 45 3 1 12 1  0 19 1 0 09 1
    1 25 1 1 12 1  1 27 1 0 08 1  1 24 1 0 11 0  1 35 1 0 08 0  0 19 3 1 12 1  0 36 3 0 08 0  0 18 3 0 11 1
    1 42 1 1 14 1  1 38 1 0 11 1  1 31 1 0 09 1  1 33 1 0 11 0  0 51 3 1 12 0  0 19 3 0 03 0  0 16 2 0 07 1
    1 25 1 0 05 1  1 23 2 0 08 1  1 23 3 0 08 0  0 37 1 0 09 0  0 20 3 0 10 0  0 32 3 1 13 0  0 21 3 0 11 0
    1 23 1 1 12 0  1 26 1 0 11 1  1 18 1 0 10 1  0 20 1 1 12 1  0 19 1 0 09 1  0 17 1 0 08 1  0 16 3 0 08 1
    1 46 1 0 08 0  1 21 3 1 12 1  1 29 1 1 12 1  0 34 3 0 08 0  0 20 3 0 07 1  0 21 3 0 08 0  0 16 2 0 09 1
    1 24 1 0 10 1  1 25 1 0 08 1  1 26 3 0 11 1  0 22 1 1 14 0  0 23 3 1 12 0  0 18 3 0 10 1  0 18 1 0 10 1
    1 21 1 1 12 1  1 31 1 0 11 0  1 24 1 0 09 1  0 42 2 0 00 0  0 37 3 1 14 0  0 24 1 1 12 0  0 16 1 0 09 1
    1 19 3 0 09 1  1 17 1 0 10 1  1 25 1 1 12 1  0 25 2 0 09 1  0 24 3 0 10 0  0 42 3 0 08 1  0 20 1 0 09 1
    1 17 1 0 08 1  1 25 1 0 11 1  1 24 1 0 10 1  0 40 3 1 13 0  0 21 3 1 12 1  0 19 2 0 08 1  0 18 1 0 11 1
    1 18 2 0 08 0  1 21 1 1 12 1  1 46 1 0 08 1  0 36 3 1 12 0  0 24 3 1 12 1  0 17 1 0 09 1  0 46 1 0 11 0
    1 20 1 0 11 1  1 44 1 0 11 1  1 31 3 1 12 1  0 39 2 0 06 0  0 20 3 1 13 1  0 23 1 1 13 1  0 17 1 0 08 1
    1 25 1 0 11 0  1 25 3 1 12 1  1 19 1 0 11 1  0 29 3 1 12 0  0 17 1 0 07 1  0 46 3 0 07 1  0 16 3 0 09 1
    1 17 1 0 08 1  1 18 1 0 09 1  1 19 1 0 08 1  0 22 2 1 13 1  0 18 1 0 10 1  0 18 3 0 10 0  0 33 2 1 12 0
    1 17 1 0 09 1  1 42 1 1 12 1  1 27 1 0 11 1  0 40 2 0 03 0  0 30 3 1 12 0  0 19 1 0 10 1  0 29 3 0 11 0
    1 25 1 0 05 1  1 25 1 0 10 1  1 26 1 0 11 0  0 25 3 1 12 0  0 30 3 0 07 0  0 26 3 0 07 0  0 19 3 1 12 1
    1 23 1 1 12 1  1 31 2 0 09 1  0 25 1 1 12 0  0 20 3 1 12 0  0 17 3 0 10 1  0 16 3 0 10 1  0 20 1 1 12 1
    ;;;;
    run;quit;

    proc print data=sd1.have width=min;
    run;quit;

    /**************************************************************************************************************************/
    /* Obs    trt    age    race    deg    edu    sngl                                                                        */
    /*                                                                                                                        */
    /*   1     1      37      1      0      11      0                                                                         */
    /*   2     1      28      1      0       8      1                                                                         */
    /*   3     1      24      1      0      10      1                                                                         */
    /*   4     1      20      1      0      10      1                                                                         */
    /*   5     0      29      2      0       6      0                                                                         */
    /*                                                                                                                        */
    /* 366     0      25      1      1      12      0                                                                         */
    /* 367     0      20      3      1      12      0                                                                         */
    /* 368     0      17      3      0      10      1                                                                         */
    /* 369     0      16      3      0      10      1                                                                         */
    /* 370     0      20      1      1      12      1                                                                         */
    /**************************************************************************************************************************/

    /*
    | | ___   __ _
    | |/ _ \ / _` |
    | | (_) | (_| |
    |_|\___/ \__, |
             |___/
    */

    1                                          Altair SLC     13:12 Thursday, November 13, 2025

    NOTE: Copyright 2002-2025 World Programming, an Altair Company
    NOTE: Altair SLC 2026 (05.26.01.00.000758)
          Licensed to Roger DeAngelis
    NOTE: This session is executing on the X64_WIN11PRO platform and is running in 64 bit mode

    NOTE: AUTOEXEC processing beginning; file is C:\wpsoto\autoexec.sas
    NOTE: AUTOEXEC source line
    1       +  ?;;;;
               ^
    ERROR: Expected a statement keyword : found " "

    NOTE: 1 record was written to file PRINT

    NOTE: The data step took :
          real time : 0.009
          cpu time  : 0.031


    NOTE: AUTOEXEC processing completed

    1         &_init_;
    2         options validvarname=v7 ls=255;
    3         data have ;
    4           input trt age race deg edu sngl @@;
    5         cards4;

    NOTE: A new line was read when INPUT statement read past the end of a line
    NOTE: Data set "WORK.have" has 370 observation(s) and 6 variable(s)
    NOTE: The data step took :
          real time : 0.000
          cpu time  : 0.000


    6         1 37 1 0 11 0  1 28 1 0 08 1  1 24 1 0 10 1  1 20 1 0 10 1  0 29 2 0 06 0  0 19 3 1 13 1  0 29 3 1 12 0  0 55 1 0 04 1
    7         1 22 2 0 09 1  1 31 1 0 11 0  1 26 1 0 10 1  1 28 1 0 10 1  0 25 3 1 13 0  0 18 1 0 11 1  0 40 3 1 16 0  0 18 1 0 11 1
    8         1 30 1 1 12 1  1 18 1 0 11 1  1 25 1 0 11 1  1 24 1 1 12 1  0 39 3 0 10 0  0 21 3 0 10 1  0 19 3 0 10 1  0 16 2 0 09 0
    9         1 27 1 0 11 1  1 25 1 1 12 1  1 18 1 0 11 1  1 19 1 0 08 1  0 33 3 1 12 0  0 24 3 1 12 0  0 16 1 0 06 1  0 53 3 1 12 1
    10        1 33 1 0 08 1  1 30 1 0 11 0  1 19 1 0 11 1  1 23 1 1 12 1  0 31 3 1 12 0  0 18 3 0 11 1  0 22 3 0 08 0  0 17 1 0 08 1

    .....

    53        1 17 1 0 09 1  1 42 1 1 12 1  1 27 1 0 11 1  0 40 2 0 03 0  0 30 3 1 12 0  0 19 1 0 10 1  0 29 3 0 11 0
    54        1 25 1 0 05 1  1 25 1 0 10 1  1 26 1 0 11 0  0 25 3 1 12 0  0 30 3 0 07 0  0 26 3 0 07 0  0 19 3 1 12 1
    55        1 23 1 1 12 1  1 31 2 0 09 1  0 25 1 1 12 0  0 20 3 1 12 0  0 17 3 0 10 1  0 16 3 0 10 1  0 20 1 1 12 1
    56        ;;;;
    57        run;quit;
    58
    59        proc print data=have width=min;
    60        run;quit;
    NOTE: 370 observations were read from "WORK.have"
    NOTE: Procedure print step took :
          real time : 0.046
          cpu time  : 0.015


    61
    ERROR: Error printed on page 1

    NOTE: Submitted statements took :
          real time : 0.112
          cpu time  : 0.078

    /*
     _ __  _ __ ___   ___ ___  ___ ___
    | `_ \| `__/ _ \ / __/ _ \/ __/ __|
    | |_) | | | (_) | (_|  __/\__ \__ \
    | .__/|_|  \___/ \___\___||___/___/
    |_|
    */


    *_init_;

    /*--- empty ultraedit persistent work library ---*/
    proc datasets lib=workx kill;
    run;quit;

    options set=RHOME "D:\d451";
    libname sd1 "d:/sd1";
    proc r;
    export data=sd1.have r=have;
    submit;
    library(MatchIt)
    library(cobalt)
    library(haven)
    library(tableone)
    library(dplyr)
    # Propensity Score Matching with a 0.2 standard deviation caliper
    set.seed(123)  # For reproducibility

    mtch <- matchit(
      trt ~ age + edu + race + sngl + deg,
      data = have,
      method = "nearest",
      distance = "glm",  # Logistic regression for PS estimation
      caliper = 0.2,     # Caliper width (in SD of logit(PS))
      std.caliper = TRUE # Use SD units for caliper
    )

    # Show matching results
    summary(mtch)
    sink("d:/txt/match_summary.txt")
    summary(mtch)
    sink()

    mtchdf <- match.data(mtch)
    mtchful <- match.data(mtch, drop.unmatched = FALSE)

    # Extract matched data

    png("d:/png/match_compare.png")
    # Visualize balance assessment
    love.plot(mtch,
              threshold = 0.1,   # Balance threshold
              stars = "std")     # Show standardized mean differences
    dev.off();
    endsubmit;
    import data=workx.mtchful r=mtchful;
    run;quit;

    proc print data=workx.mtchful;
    run;quit;

    data _null_;
     infile "d:/txt/match_summary.txt";
     input;
     put _infile_;
    run;quit;

    /**************************************************************************************************************************/
    /* matchit(formula = trt ~ age + edu + race + sngl + deg, data = have,                                                    */
    /*     method = "nearest", distance = "glm", caliper = 0.2, std.caliper = TRUE)                                           */
    /*                                                                                                                        */
    /* Summary of Balance for All Data:                                                                                       */
    /*          Means Treated Means Control Std. Mean Diff. Var. Ratio eCDF Mean  eCDF Max                                    */
    /* distance        0.6939        0.3061          1.8367     0.5923    0.3567    0.5946                                    */
    /* age            25.8162       26.9676         -0.1609     0.4724    0.0707    0.1243                                    */
    /* edu            10.3459       10.1784          0.0833     0.4753    0.0411    0.1135                                    */
    /* race            1.2541        2.3622         -1.7831     0.5197    0.3694    0.5892                                    */
    /* sngl            0.8108        0.5189          0.7453          .    0.2919    0.2919                                    */
    /* deg             0.2919        0.4000         -0.2378          .    0.1081    0.1081                                    */
    /*                                                                                                                        */
    /* Summary of Balance for Matched Data:                                                                                   */
    /*          Means Treated Means Control Std. Mean Diff. Var. Ratio eCDF Mean  eCDF Max Std. Pair Dist.                    */
    /* distance        0.5870        0.5504          0.1731     1.1880    0.0588    0.2308          0.1761                    */
    /* age            27.2051       26.5897          0.0860     0.4195    0.1045    0.3333          1.4406                    */
    /* edu            10.3718        9.9872          0.1913     0.7490    0.0285    0.1282          1.3008                    */
    /* race            1.5769        1.6154         -0.0619     1.0491    0.0214    0.0513          0.1857                    */
    /* sngl            0.7949        0.7051          0.2291          .    0.0897    0.0897          0.7529                    */
    /* deg             0.3333        0.3077          0.0564          .    0.0256    0.0256          1.0152                    */
    /**************************************************************************************************************************/

    proc sort data=workx.mtchful;
     by subclass trt;
    run;quit;

    /**************************************************************************************************************************/
    /* SD1.MTCHFUL                                                                                                            */
    /* Obs    rownames    trt    age    race    deg    edu    sngl    distance    weights    subclass                         */
    /*                                                                                                                        */
    /*   1         5       0      29      2      0       6      0      0.17204       0           . NOT MATCHED                */
    /*   2         6       0      19      3      1      13      1      0.15007       0           .                            */
    /*   3         7       0      29      3      1      12      0      0.06756       0           .                            */
    /*   4        13       0      25      3      1      13      0      0.07527       0           .                            */
    /*   5        24       0      16      2      0       9      0      0.22309       0           .                            */
    /*  ...                                                                                                                   */
    /*                                                                                                                        */
    /* 365       342       0      16      3      0       9      1      0.14168       1          76 MATCHED PAIR               */
    /* 366       337       1      25      3      1      12      1      0.14197       1          76                            */
    /*                                                                                                                        */
    /* 367       215       0      19      1      1      13      1      0.77395       1          77 MATCHED PAIR               */
    /* 368       358       1      25      1      0      10      1      0.83400       1          77                            */
    /*                                                                                                                        */
    /* 369       346       0      22      2      1      13      1      0.45649       1          78 MATCHED PAIR               */
    /* 370       365       1      31      2      0       9      1      0.51671       1          78                            */
    /**************************************************************************************************************************/

    options nolabel;
    proc means data=workx.mtchful stackodsoutput n mean stddev;
    class trt weights;
    types trt trt*weights;
    var
      age
      race
      deg
      sngl
      edu;
    ods output summary=workx.stats;
    run;quit;

    proc print data=workx.stats;
    run;quit;


    /**************************************************************************************************************************/
    /*  Altair SLC                                                                                                            */
    /*                                                                                                                        */
    /* Obs    TRT    NOBS    VARIABLE      N            MEAN             STD    WEIGHTS                                       */
    /*                                                                                                                        */
    /*   1     0      185      AGE       185    26.967567568    10.410541638       .                                          */
    /*   2     0      185      RACE      185    2.3621621622    0.8620474085       .                                          */
    /*   3     0      185      DEG       185             0.4    0.4912273891       .                                          */
    /*   4     0      185      SNGL      185    0.5189189189    0.5009978292       .                                          */
    /*   5     0      185      EDU       185    10.178378378    2.9165136214       .                                          */
    /*   6     1      185      AGE       185    25.816216216    7.1550192748       .                                          */
    /*   7     1      185      RACE      185    1.2540540541    0.6214440558       .                                          */
    /*   8     1      185      DEG       185    0.2918918919    0.4558665771       .                                          */
    /*   9     1      185      SNGL      185    0.8108108108    0.3927216791       .                                          */
    /*  10     1      185      EDU       185    10.345945946    2.0106502564       .                                          */
    /*  11     0      107      AGE       107    27.242990654    9.5422013218       0                                          */
    /*  12     0      107      RACE      107    2.9065420561    0.2924428756       0                                          */
    /*  13     0      107      DEG       107    0.4672897196    0.5012768039       0                                          */
    /*  14     0      107      SNGL      107    0.3831775701    0.4884488311       0                                          */
    /*  15     0      107      EDU       107    10.317757009    3.1190169316       0                                          */
    /*  16     0       78      AGE        78     26.58974359    11.549369913       1                                          */
    /*  17     0       78      RACE       78    1.6153846154    0.8254203059       1                                          */
    /*  18     0       78      DEG        78    0.3076923077    0.4645257967       1                                          */
    /*  19     0       78      SNGL       78    0.7051282051    0.4589364996       1                                          */
    /*  20     0       78      EDU        78    9.9871794872    2.6210614968       1                                          */
    /*  21     1      107      AGE       107    24.803738318    6.7648359005       0                                          */
    /*  22     1      107      RACE      107    1.0186915888    0.1360707648       0                                          */
    /*  23     1      107      DEG       107     0.261682243    0.4416189897       0                                          */
    /*  24     1      107      SNGL      107    0.8224299065    0.3839488004       0                                          */
    /*  25     1      107      EDU       107    10.327102804    1.8107420606       0                                          */
    /*  26     1       78      AGE        78    27.205128205    7.4804662928       1                                          */
    /*  27     1       78      RACE       78    1.5769230769    0.8454497116       1                                          */
    /*  28     1       78      DEG        78    0.3333333333    0.4744557146       1                                          */
    /*  29     1       78      SNGL       78    0.7948717949    0.4064088645       1                                          */
    /*  30     1       78      EDU        78    10.371794872    2.2684108194       1                                          */
    /**************************************************************************************************************************/

    /*---- no sort needed ----*/
    %utl_transpose(
        data=workx.stats
        ,sort=YES
        ,by=variable n
        ,id=trt
        ,out=workx.want
        ,var=weights n mean std);

    proc print data=workx.want;
    run;

    /**************************************************************************************************************************/
    /* WORK.WANT                                                                                                              */
    /*                                                                                                                        */
    /* VARIABLE N0           MEAN0            STD0 WEIGHTS0  N1           MEAN1         STD1 WEIGHTS1                         */
    /*                                                                                                                        */
    /*   AGE    78     26.58974359    11.549369913        1  78    27.205128205 7.4804662928        1                         */
    /*   AGE   107    27.242990654    9.5422013218        0 107    24.803738318 6.7648359005        0                         */
    /*   AGE   185    26.967567568    10.410541638        . 185    25.816216216 7.1550192748        .                         */
    /*                                                                                                                        */
    /*   DEG    78    0.3076923077    0.4645257967        1  78    0.3333333333 0.4744557146        1                         */
    /*   DEG   107    0.4672897196    0.5012768039        0 107     0.261682243 0.4416189897        0                         */
    /*   DEG   185             0.4    0.4912273891        . 185    0.2918918919 0.4558665771        .                         */
    /*                                                                                                                        */
    /*   EDU    78    9.9871794872    2.6210614968        1  78    10.371794872 2.2684108194        1                         */
    /*   EDU   107    10.317757009    3.1190169316        0 107    10.327102804 1.8107420606        0                         */
    /*   EDU   185    10.178378378    2.9165136214        . 185    10.345945946 2.0106502564        .                         */
    /*                                                                                                                        */
    /*   RACE   78    1.6153846154    0.8254203059        1  78    1.5769230769 0.8454497116        1                         */
    /*   RACE  107    2.9065420561    0.2924428756        0 107    1.0186915888 0.1360707648        0                         */
    /*   RACE  185    2.3621621622    0.8620474085        . 185    1.2540540541 0.6214440558        .                         */
    /*                                                                                                                        */
    /*   SNGL   78    0.7051282051    0.4589364996        1  78    0.7948717949 0.4064088645        1                         */
    /*   SNGL  107    0.3831775701    0.4884488311        0 107    0.8224299065 0.3839488004        0                         */
    /*   SNGL  185    0.5189189189    0.5009978292        . 185    0.8108108108 0.3927216791        .                         */
    /**************************************************************************************************************************/

    proc sql;
      create
         table workx.smd as
      select
         variable
        ,n0
        ,(mean1-mean0)/(sqrt((Std0**2+std1**2)/2)) as smd
        ,case n0
           when 185 then "all"
           else "matched"
         end as ltr length 44
      from
         workx.want
      where
         weights0 ne 0
    ;quit;

    proc print data=workx.smd;
    run;quit;

    /**************************************************************************************************************************/
    /*  Altair SLC                                                                                                            */
    /*                                                                                                                        */
    /*  Obs    VARIABLE     N0      SMD         LTR                                                                           */
    /*                                                                                                                        */
    /*  1      AGE        78     0.06325    matched                                                                           */
    /*  2      AGE       185    -0.12890    all                                                                               */
    /*  3      DEG        78     0.05461    matched                                                                           */
    /*  4      DEG       185    -0.22814    all                                                                               */
    /*  5      EDU        78     0.15692    matched                                                                           */
    /*  6      EDU       185     0.06690    all                                                                               */
    /*  7      RACE       78    -0.04603    matched                                                                           */
    /*  8      RACE      185    -1.47465    all                                                                               */
    /*  9      SNGL       78     0.20704    matched                                                                           */
    /* 10      SNGL      185     0.64847    all                                                                               */
    /**************************************************************************************************************************/

    options ls=80 ps=200;
    proc plot data=workx.smd;
     title "Calculate Standardized Mean Difference (SMD)";
     label smd="Standardized Mean Difference";
     label variable="Covariate";
     plot variable*smd='*' $ ltr /box href=0;
    run;quit;
    options ls=255 ps=66;


    /**************************************************************************************************************************/
    /*  WORK.ANNO                                                                                                             */
    /*   variable    nobs       smd      ltr                                                                                  */
    /*                                                                                                                        */
    /*     age         78     0.06325    matched                                                                              */
    /*     age        185    -0.12890    all                                                                                  */
    /*     deg         78     0.05461    matched                                                                              */
    /*     deg        185    -0.22814    all                                                                                  */
    /*     edu         78     0.15692    matched                                                                              */
    /*     edu        185     0.06690    all                                                                                  */
    /*     race        78    -0.04603    matched                                                                              */
    /*     race       185    -1.47465    all                                                                                  */
    /*     sngl        78     0.20704    matched                                                                              */
    /*     sngl       185     0.64847    all                                                                                  */
    /*     sngl       185    -1.75000    Standardized M ean Difference                                                        */
    /*                                                                                                                        */
    /*  Calculate Standardized Mean Difference (SMD)                                                                          */
    /*                                                                                                                        */
    /*           Plot of variable*smd$ltr.  Symbol used is '*'.                                                               */
    /*                                                                                                                        */
    /*               Plot of VARIABLE*SMD. Symbol used is '*'.                                                                */
    /*                                                                                                                        */
    /*          -+----------+----------+---------------+-------+-                                                             */
    /*     SNGL +                      |  * matced *all         +                                                             */
    /*          |                      |                        |                                                             */
    /*C         |                      |                        |                                                             */
    /*o    RACE + * all               *|matched                 +                                                             */
    /*v         |                      |                        |                                                             */
    /*a         |                      |                        |                                                             */
    /*r     EDU +                      |**all                   +                                                             */
    /*i         |                      |                        |                                                             */
    /*a         |                      |                        |                                                             */
    /*t     DEG +                 * all|* matched               +                                                             */
    /*e         |                      |                        |                                                             */
    /*          |                      |                        |                                                             */
    /*      AGE +                   * all matched               +                                                             */
    /*          -+----------+----------+---------------+-------+-                                                             */
    /*         -1.5        -1          0              1.5      2                                                              */
    /*                                                                                                                        */
    /*                             Standardized Mean Difference                                                               */
    /**************************************************************************************************************************/

    /*                    _                              _           _ _ _
      ___ _ __ ___  _ __ | |_ _   _  __      _____  _ __| | ____  __| (_) |__  _ __ __ _ _ __ _   _
     / _ \ `_ ` _ \| `_ \| __| | | | \ \ /\ / / _ \| `__| |/ /\ \/ /| | | `_ \| `__/ _` | `__| | | |
    |  __/ | | | | | |_) | |_| |_| |  \ V  V / (_) | |  |   <  >  < | | | |_) | | | (_| | |  | |_| |
     \___|_| |_| |_| .__/ \__|\__, |   \_/\_/ \___/|_|  |_|\_\/_/\_\|_|_|_.__/|_|  \__,_|_|   \__, |
                   |_|        |___/                                                           |___/
    */


    proc datasets lib==workx kill;
    run;quit;

    /*              _
      ___ _ __   __| |
     / _ \ `_ \ / _` |
    |  __/ | | | (_| |
     \___|_| |_|\__,_|

    */
