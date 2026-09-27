# UQ SPH research project (2014-2022)

Documentation and files from my doctoral research at the School of Public Health, University of Queensland, carried out between October 2014 and November 2022 and submitted in 2022 as the thesis "Modelling the health impact of obesity in Australia".

The thesis had three objectives: refining the conventional cut-off methods used to identify implausible energy intake reporters, projecting the future distribution of body mass index in Australian adults, and using those projections to model the future health impact of obesity. The energy balance work was published as a paper and forms one chapter of the thesis. The BMI projections and the health impact model are the two components held in this repository.

Funding was provided by an NHMRC Centre of Research Excellence in Healthy Liveable Communities scholarship (APP1061404).

## At a glance

- **Degree:** Doctor of Philosophy, Public Health and Epidemiology, The University of Queensland, Faculty of Medicine, School of Public Health
- **Period:** October 2014 to November 2022
- **Principal supervisor:** Lennert Veerman, with associate supervisors Jan Barendregt, Mark Jones and Leopold Aminde
- **Study one:** energy balance and body mass among Australian adults, published in *Nutrition & Dietetics* in 2019
- **Study two:** future BMI distribution of Australian adults, fitted with GAMLSS models in R
- **Study three:** health impact of trends in obesity, a Markov multi-state life table simulation implemented in Excel with VBA and run with Monte Carlo sampling
- **Data:** secondary analysis only, using Australian Bureau of Statistics confidentialised unit record files and Global Burden of Disease Study 2019 estimates
- **In this repository:** the multi-state life table model workbook and the GAMLSS results workbook

## Study one: energy balance and body mass

The first study addressed the energy balance side of the problem: how much energy people report eating, how much they are estimated to require, and how far the two diverge. Misreporting of energy intake is the main reason the two diverge, and it is a common source of measurement error in dietary surveys, producing biased estimates and reduced statistical power. The study refined the conventional Goldberg cut-off method and then examined the extent and the characteristics of energy under-reporting in Australia.

The analysis used the 1995 National Nutrition Survey and the 2011-12 National Nutrition and Physical Activity Survey, with analysis samples of 1,196 and 5,332 adults and response rates of 61% and 77%. Respondents under 18 years and pregnant women were excluded. Basal metabolic rate was estimated with the Schofield equations, and the conventional critical value was replaced with calibrated values of 1.23 for men and 1.35 for women, selected to improve sensitivity while retaining reasonable specificity. Physical activity was not recorded in the 1995 survey, so the variable cut-off variant was not available for that year and the constant cut-off was used instead. Identified under-reporters then formed the outcome in Poisson regression with robust standard errors, so that estimates could be read as relative risks rather than odds ratios. Analyses were run in Stata.

Three methods were compared: the Goldberg constant cut-off, the Goldberg variable cut-offs based on physical activity level, and the ratio of energy intake to estimated energy requirement. With the revised cut-offs, estimated prevalence in 2011-12 was 40.5% by the constant cut-off, 48.1% by the variable cut-off and 47.1% by the estimated energy requirement method, against 32.0% by the constant cut-off in 1995.

The revised critical values were not chosen arbitrarily. They were calibrated to reach sensitivity above 0.8 and specificity of about 0.9, using the sensitivity and specificity trade-off reported for two-day 24-hour dietary recall, and centred on an assumed true prevalence of about 38% for men and 33% for women. The cut-offs depend on assumed coefficients of variation, taken as 23% for reported energy intake, 8.5% for estimated basal metabolic rate and 15% for true physical activity level in the Goldberg calculation, and 23%, 11% and 8.2% in the estimated energy requirement calculation. Anyone reproducing the thresholds needs these values, because the cut-offs cannot be recomputed without them.

The prevalence of under-reporting among men increased from 24% in 1995 to 41% in 2012, while the prevalence among women was unchanged at about 40%. Under-reporting was more common in people with a higher BMI and in areas of greater socioeconomic disadvantage, and less common in older, more educated and higher-income respondents. Using the conventional cut-off values would have underestimated prevalence by 50% to 100%.

The study also estimated how much energy intake was being misreported. The mean ratio of reported energy intake to estimated energy requirement was 0.80 for men and 0.82 for women. Once low energy reporters were excluded, the mean rose to 1.01 and 1.00, meaning plausible reporters on average report close to their full requirement, and the implied underestimation of energy intake was 19.4%. This is the link between the first objective and the rest of the thesis: once implausible reporters are removed, the remaining sample behaves as an energetically balanced population, which is what makes the later projections interpretable.

## Study two: future BMI distribution of Australian adults

The second study projected how the distribution of BMI in Australian adults would change, allowing for the increasingly right-skewed shape of that distribution.

Height and weight came from the Risk Factor Prevalence Surveys of 1980, 1983 and 1989, the 1995 National Nutrition Survey, the 2007-08 and 2014-15 National Health Surveys, the 2011-12 National Nutrition and Physical Activity Survey and the 2017-18 National Health Survey. Analyses were run separately for men and women.

Two approaches were used. The first fitted separate linear regression models for the mean and the variance of BMI, which is the two-step approach used early in the project, and combined them into a log-normal distribution. The second used a non-linear GAMLSS model, which estimates all distribution parameters simultaneously, with the Box-Cox power exponential distribution, so that the median, variance, skewness and kurtosis of BMI are all modelled in one fit. Taking the logarithm of the period variable allows BMI to keep increasing at a decelerating rate rather than indefinitely in a straight line. Analyses were performed in R with the `gamlss` package.

The linear age-adjusted trend in BMI was 3.6% per decade for women and 3.0% for men. Adult overweight prevalence rose from about 41% in 1980 to 67% in 2018 and is projected to reach 70% by 2025, while adult obesity prevalence rose from 8% to 30% over the same period and is projected to exceed 43% by 2050 once the deceleration is taken into account. The four-parameter distribution also reduced bias in the upper tail, so prevalence at BMI of 35 and above could be reported. Projections were extended to 2105 to supply input data for the simulation model, whose youngest simulated cohort survives into the second half of that century. The trend models were also checked for extrapolation behaviour well beyond the horizon needed for the simulation.

## The GAMLSS results

`GAMLSS/gamlss_results.xlsx` contains the outputs of the BMI distribution models from the second study. It has eight sheets covering the four fitted models, each with a results sheet and a charts sheet.

- `logno` and `logno_charts` for the linear log-normal model
- `bcpe` and `bcpe_charts` for the linear Box-Cox power exponential model
- `logno_nonlinear` and `logno_nonlinear_charts` for the non-linear log-period log-normal model
- `bcpe_nonlinear` and `bcpe_nonlinear_charts` for the non-linear log-period Box-Cox power exponential model

The paired sheets allow the fitted distributions to be compared interactively across the linear and non-linear trends and across the two distributional assumptions.

## Study three: health impact of trends in obesity

The third study used a modified proportional multi-state life table model to estimate the disease burden associated with current and projected body mass in Australia. The model is a Markov model: age-sex-specific cohorts move between multiple health states, and the life table aggregates the results of a separate life table for each modelled disease.

The model was derived from the Assessing Cost Effectiveness in Prevention project, with substantial modifications: restructuring to allow different scenarios, updated input data, user-defined BMI distributions, improved computational efficiency, and a new implementation of the risk function. Twenty-seven obesity-related diseases are modelled explicitly, comprising 12 cancers, 6 cardiovascular conditions, asthma, gallbladder and biliary diseases, Alzheimer's disease and other dementias, type 2 diabetes mellitus, chronic kidney disease, cataract, osteoarthritis, low back pain and gout.

Two populations are simulated. In the target scenario the population follows the projected BMI distribution, and in the reference scenario the population either holds a stable BMI distribution at 2020 levels or has a distribution without excess weight, representing the theoretical minimum risk exposure distribution. All scenarios start from the 2020 Australian population of 12.7 million males and 13.0 million females and run until everyone has died or reached 100 years of age. Years lived by each cohort are weighted by the disability associated with prevalent disease, and disease-specific disability weights are derived by dividing years lived with disability by prevalence and then adjusted for other co-occurring conditions.

Over the lifetime of the current population, an estimated 68 million health-adjusted life years will be lost due to obesity, of which 28 million can be attributed to the upward trend in BMI rather than to current body mass.

### Health-adjusted life years

Health-adjusted life years are deliberately not the same quantity as the disability-adjusted life years reported in the Global Burden of Disease studies. A DALY is the sum of years of life lost and years lived with disability. A health-adjusted life year in this work is life years lived multiplied by the complement of the prevalent years lived with disability rate, which is a morbidity rate analogue of the all-cause mortality rate. The two are not interchangeable, so the 68 million figure should not be compared directly with GBD DALY totals.

### Uncertainty and what it covers

Uncertainty intervals come from 2,000 Monte Carlo samples drawn with Ersatz. The intervals reflect uncertainty in the theoretical minimum risk exposure level and in the disease relative risks. They do not include uncertainty in the underlying epidemiological input data, so prevalence, incidence and case fatality are treated as fixed. The intervals are therefore narrower than a full probabilistic sensitivity analysis would give.

### Model inputs

| Input | Source |
| --- | --- |
| All-cause mortality | ABS Deaths 2019 |
| Population statistics | Australian Demographic Statistics, 2020 |
| Disease prevalence, incidence and case fatality | GBD 2019, via DisMod II |
| Disability weights | GBD 2019 |
| Relative risk | GBD 2019 |

Mortality data from 2019 rather than 2020 were used because deaths are registered with a delay, so the most recent year is incomplete at the time of extraction.

Remission rates are set to vary by disease, which differs from earlier implementations of similar models where all remission rates were set to zero. Rates are set to zero for pancreatic cancer, ischemic heart disease, ischemic stroke, hypertensive heart disease, chronic kidney disease due to diabetes and chronic kidney disease due to glomerulonephritis, because the remission estimates behave aberrantly for those conditions. Where GBD 2019 provides remission rates directly through the visualization tool they can be set exactly, without further modelling. Mortality from type 2 diabetes is excluded from the main life table to avoid double counting the cardiovascular conditions already represented, and osteoarthritis and low back pain carry a case fatality of zero because they are not fatal conditions.

## The multi-state life table model

`MSLT/obesity_MSLT_vba.xlsm` is the model described above. It has 46 sheets.

- `Intro` records the settings needed to run the model, including the base year, discounting rate, age range for BMI integration, theoretical minimum risk exposure level and the selected BMI models.
- `Notes` is a dated change log recording each revision to the model.
- `Output` holds the summary results.
- `BMI`, `BMI Models`, `GAMLSS`, `BCPE` and `logno` implement the BMI distributions that can be selected as the base or target model.
- `PIFs` holds the potential impact fraction calculations.
- `Input-Disease`, `Input-Remission`, `Input-Cost_Offsets`, `Input-LifeTable` and `Input-RR` hold the model inputs.
- `LifeTables` aggregates the disease life tables into the main life table.
- One disease sheet per modelled condition, for 31 sheets in total. Chronic kidney disease is separated into four aetiologies and osteoarthritis into hip and knee, which accounts for the difference between 31 sheets and the 27 modelled conditions.

Eleven options are available for the base and target BMI distribution, including GAMLSS models with linear and log-period terms under both log-normal and Box-Cox power exponential distributions, a theoretical minimum risk exposure distribution, and a base case with the trend switched off. The VBA project includes a custom `gen_RR` function that generates a quasi-exponential relative risk function which becomes linear above a user-specified BMI cut-off, so that the risk function cannot grow faster than the population distribution decays. The model can be run to 2105.

The model requires Microsoft Excel with two add-ins: EpiGearXL for the potential impact fraction integrals, and Ersatz for the Monte Carlo sampling. The `Intro` sheet gives the settings and the sequence of steps to run the model.

### Where the model came from

The multi-state life table structure follows Barendregt JJ, Van Oortmarssen GJ, Van Hout BA, Van Den Bosch JM, Bonneux L. Coping with multiple morbidity in a life table. *Mathematical Population Studies*. 1998;7(1):29-49, and the Assessing Cost Effectiveness in Prevention project described in Vos T, Carter R, Barendregt J, Mihalopoulos C, Veerman L, Magnus A, et al. Assessing cost-effectiveness in prevention: ACE-prevention September 2010 final report. University of Queensland; 2010.

### Other software used

The remission rates required by the model were estimated with `disbayes`, the Bayesian multi-state disease modelling package by Jackson, which derives internally consistent transition rates in the same modelling family as DisMod II and DisMod-MR. It is a third-party package used for this work and is not part of this repository.

## What this repository does not contain

This repository is a partial release. The workbooks are the versions used for the submitted thesis. The following are not included.

- The analysis code for the energy balance study, which was written in Stata
- The R code for the BMI trend and GAMLSS modelling in the second study
- Survey microdata. The Australian Bureau of Statistics confidentialised unit record files cannot be redistributed
- Global Burden of Disease and other third-party input datasets
- The thesis document and the full set of supporting appendices

## Related publication

Tam KW, Veerman JL. Prevalence and characteristics of energy intake under-reporting among Australian adults in 1995 and 2011 to 2012. *Nutrition & Dietetics*. 2019;76(5):546-559.

https://doi.org/10.1111/1747-0080.12565

The paper was sourced from the energy under-reporting chapter of the thesis, with Veerman listed as a co-author for his supervisory role.

## Thesis record

Tam KW. *Modelling the health impact of obesity in Australia*. PhD thesis, The University of Queensland; 2022. https://espace.library.uq.edu.au/view/UQ:ef99295 (the full text is not yet available).
