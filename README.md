# Triage-Sim
Simulation and analysis codebooks for estimating lives saved from distinct crisis standards of care protocols.

### Dependencies
The analysis programs require Python v3.11, pandas, matplotlib, numpy, scipy, seaborn and statsmodels packages. Conversion of ICD-10 to Elixhauser and Charlson scores uses the comorbidipy package (https://github.com/vvcb/comorbidipy). De-identified data is available upon reasonable request.

## Simulation Strategy
Code simulates the implementation of each protocol under different levels of scarcity using a Monte Carlo model. Extending the approach of Bhavani et al.,7 for integers 1 ≤ n ≤ 19 our model simulates an n/20 (i.e. 5%, 10%...95% capacity) ventilator shortage by: (1) randomly sampling 10 pairs of patients (A and B) from our dataset, (2) if n>10, assigning ventilators to both patients in n/2 pairs (i.e. A=1, B=1), and if n≤10 assigning no ventilator to both patients in n/2 pairs (i.e. A=0, B=0), (3) ranking the individuals in each remaining pair for priority based on the relevant protocol (i.e. A>B), 4) assigning a ventilator to the highest priority patient in each pair (e.g. A=1, B=0), and 4) repeating this process until all patients in our dataset have been assigned to receive or not receive a ventilator. Estimated survival in each simulation is calculated by observing the actual (scarcity-free) survival to discharge of patients in the reference dataset. 

**Model Assumptions**
 - Patients who were not allocated a ventilator in our simulation did not survive. We repeated the simulation 1,000 times for the 50% capacity results and 250 times for all other capacity levels.
 - No re-allocation. Once a ventilator is allocated it is not removed until patient expires or discharges (N.B. many existing systems allow for removal from poorly perofmring patients and re-allocation to patients with higher likelihood of survival)

## Analysis Strategy
For each simulation, our analysis calculates (i) survival rate, (ii) age-adjusted survival rate, and (iii) aggregate comorbidity-adjusted life expectancy (in years). Survival rates for sub-populations are age-adjusted and confidence intervals for age-adjusted rates are calculated using the modified Gamma method of Tiwari et al. Raw life expectancy is calculated for each subject from the 2019 National Vital Statistics System (NVSS) life tables for their age, sex and race. The impact of comorbidities on life expectancy was estimated by applying the adjustments previously calculated by Cho et. al for Medicare recipients without a history of cancer. Subjects were placed into one of three comorbidity bands (none, low/medium, high) and the corresponding age adjustment in Cho et. al was applied, before re-calculating the subject’s life expectancy using the NVSS tables. The comorbidity adjustment for a 65yr old in Cho et. al’s estimates was applied to all subjects in our cohort <65yrs.
