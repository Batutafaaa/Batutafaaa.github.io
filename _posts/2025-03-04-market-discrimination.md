---
layout: post
title: "The Double Squeeze: Market Access and Price Discrimination in Indian Agriculture"
date: 2026-03-19
categories: [Economics, Agriculture, Data Science, Sociology]
---

<div class="text-center mt-4 mb-4">
  <a href="https://github.com/ashwinnsr/agricultural-market-discrimination" 
     class="btn btn-primary" 
     target="_blank" 
     rel="noopener noreferrer">
    <i class="fab fa-github"></i> View Code on GitHub
  </a>
</div>

## Abstract

In the discourse on agrarian inequality in India, attention is typically directed toward land ownership, credit access, and debt cycles. However, the final stage of the agricultural cycle—the market exchange itself—remains deeply stratified by social identity. This article uses the National Sample Survey (NSS) 77th Round (2019–20) *Situation Assessment of Agricultural Households*, a longitudinal panel survey covering two seasonal visits, to analyse **74,565 crop sales records** spanning 36 states, 52,634 households, and 142 crop types. Using high-dimensional fixed-effects models, cluster-bootstrap quantile regressions, and a weighted Blinder-Oaxaca decomposition, we examine how caste and tribal identity shape the terms of agricultural trade. Our core findings are threefold. First, Scheduled Caste (SC) farmers face a consistent **~1.1% to 1.3% price penalty** relative to General Caste farmers within the same district, crop, and season, and a raw gap of −Rs. 4.3/kg (t = −2.34, p = 0.019) confirmed under cluster-robust weighted inference. The Blinder-Oaxaca decomposition reveals that **30.6% of the SC price gap is unexplained** by observable endowments in the baseline model (with high sensitivity to specification due to the small −1.0% raw gap). Second, SC farmers are significantly excluded from formal market channels: only **13.4% of SC crop sales** flow through formal outlets versus **18.3% for General Caste**; conditional on accessing a given channel, SC farmers face a further **2.0% within-channel penalty** in local trader sales (p = 0.065) and a **3.8% penalty within government procurement** (p = 0.071). Third, Scheduled Tribe (ST) farmers present a structurally distinct case: their aggregate price advantage in raw data is a geographic composition artefact. After absorbing district-level heterogeneity (M3), the ST coefficient is near-zero and statistically insignificant; only state-level controls—which are insufficient to remove the crop-district confound—produce a spurious penalty estimate. ST disadvantage operates through geographic capture and infrastructure absence, not through within-market price discrimination.

---

## 1. Research Problem

When a farmer brings their harvest to market, economic theory tells us the price should be determined by supply, demand, crop quality, and volume. But rural Indian agricultural markets do not operate in a social vacuum. They are embedded within hierarchies of caste, land, and credit that have persisted for centuries.

The central puzzle this research addresses is the gap between *market participation* and *market equality*. Marginalized farmers—Dalits (Scheduled Castes) and Adivasis (Scheduled Tribes)—are not absent from the market. They sell crops, engage with traders, and in some regions interact with government procurement systems. Yet the prices they receive, the channels through which they sell, and their subjective experience of the sale transaction differ systematically from those of General Caste farmers.

This project asks a precise empirical question: even after controlling for the crop being sold, the district it is sold in, the quantity carried to market, household wealth, and land size—does social group identity still predict the price a farmer receives? If so, by how much, and through which mechanisms does this penalty operate?

---

## 2. Research Questions

1. **H1 (Price Discrimination):** Do SC and ST farmers receive lower prices than comparably situated General Caste farmers, after controlling for crop type, district, season, quantity, landholding, and wealth? Does any penalty vary by land size class or position in the price distribution?
2. **H2 (Agency Access):** Are marginalized farmers systematically excluded from specific market channels—particularly formal channels like APMC Mandis and government procurement—and if so, does this exclusion help explain the price gap?

---

## 3. Data and Methodology

### 3.1 Data

This analysis uses the **NSS 77th Round (2019–20)**, a nationally representative longitudinal panel survey of agricultural households conducted across two seasonal sub-rounds (Visit 1: Kharif/July–December 2018; Visit 2: Rabi/January–June 2019). The survey design revisits the same households in both sub-rounds: 96.5% of Visit 2 households were matched to their Visit 1 counterparts via a stable eight-digit household identifier (FSU serial number + Second Stage Stratum + household number), confirming the longitudinal panel structure.

After merging crop sales records (Block 6, Levels 06 and 07) with household characteristics (Block 4), land data (Block 5), loan records (Block 13), MSP awareness data (Block 14), and input expenditure (Block 7), and after applying symmetric 1%–99% outlier trimming on unit prices, the final analytic dataset contains **74,565 crop-sale observations** from **52,634 unique households** across **36 states and 142 crops**.

<div class="table-responsive w-100">
  <table class="table table-bordered table-striped" style="width: 100%;">
    <thead>
      <tr>
        <th>Category</th>
        <th>Count</th>
      </tr>
    </thead>
    <tbody>
      <tr>
        <td>Total crop-sale observations</td>
        <td>74,565</td>
      </tr>
      <tr>
        <td>Unique households</td>
        <td>52,634</td>
      </tr>
      <tr>
        <td>States covered</td>
        <td>36</td>
      </tr>
      <tr>
        <td>Distinct crops</td>
        <td>142</td>
      </tr>
      <tr>
        <td>General Caste observations</td>
        <td>20,722</td>
      </tr>
      <tr>
        <td>OBC observations</td>
        <td>30,335</td>
      </tr>
      <tr>
        <td>SC observations</td>
        <td>8,343</td>
      </tr>
      <tr>
        <td>ST observations</td>
        <td>15,165</td>
      </tr>
    </tbody>
  </table>
</div>

### 3.2 Methodology

*   **Fixed-Effects OLS:** The core identification strategy uses crop fixed effects and district fixed effects to ensure that SC farmers are compared only to General Caste farmers selling the *same crop* in the *same district* in the *same season*. We additionally control for log MPCE, log total land area, and visit wave. Standard errors are clustered at the primary sampling unit (FSU) level throughout.

*   **Cluster-Bootstrap Quantile Regressions (Frisch-Newton method):** Quantile regressions at the 10th, 50th, and 90th percentiles reveal whether the price penalty concentrates among distress sales, typical transactions, or premium sales. Standard errors are computed via FSU-level cluster bootstrap (B = 100 replications) to account for within-cluster correlation. Crop group fixed effects (18 groups) are used in place of individual crop dummies (142 crops) to ensure numerical stability of the quantile estimator.

*   **Linear Probability Models (LPM):** We model the probability of selling to each channel (APMC Mandi, Government procurement, Local Trader, Cooperative, FPO) as a function of caste category, with crop and district fixed effects and FSU-clustered standard errors.

*   **Weighted Blinder-Oaxaca Decomposition:** Using separate price regressions for each caste-pair, we compute population-weighted counterfactual predicted values to decompose the raw price gap into an "explained" component (observable endowment differences) and an "unexplained" residual. All means and counterfactuals are computed using NSS survey weights (`MLT/100`). Observations with zero survey weight are excluded prior to prediction to avoid length mismatches.

*   **Mechanisms Testing:** We examine three mechanisms: MSP awareness differentials, informal lender debt, and input cost differentials. For input costs, we use an Inverse Hyperbolic Sine (IHS) transformation to include observations with zero paid-out input expenditure, which are disproportionately concentrated among ST households (10.6% zero/NA, versus <1% for other groups).

*   **Baron-Kenny Mediation:** We test the extent to which differential formal channel access mediates the price gap, using a formal Baron-Kenny path decomposition with FSU-clustered errors.

*(All analysis was conducted in R using the `fixest`, `quantreg`, and `sf` packages.)*

---

## 4. Results

### 4.1 Raw Price Gaps: The Starting Point

The first and most visible finding is a systematic divergence in average prices received across social groups. SC farmers receive an average of **Rs. 26.5/kg**, compared to **Rs. 30.8/kg** for General Caste farmers—a raw gap of −Rs. 4.3/kg (roughly 14%).

<figure class="text-center">
  <img src="/assets/img/agriculture-discrimination/p1_price_by_caste.png"
       alt="Bar chart showing average crop price received by social group, with SC receiving the lowest at Rs.26.5/kg and General caste the highest at Rs.30.8/kg" 
       class="img-fluid" 
       style="max-width: 85%; border-radius: 8px; box-shadow: 0 4px 8px rgba(0,0,0,0.1);">
  <figcaption class="mt-2 text-muted">
     <strong>Figure 1:</strong> Average crop price received by social group (weighted means with 95% confidence intervals). N = 74,565 crop-sale records, NSS 77th Round.
  </figcaption>
</figure>

However, this raw gap is importantly a *composition* effect as much as a discrimination effect. SC households grow more cereals and pulses (lower-value crops) and hold smaller landholdings on average (2.21 acres vs. 3.75 acres for General Caste). A cluster-robust weighted test confirms the SC gap (−Rs. 4.29/kg, t = −2.34, p = 0.019) is statistically significant. The ST group shows a small and statistically insignificant raw gap (−Rs. 1.18/kg, t = −0.56, p = 0.574)—consistent with ST farmers being concentrated in districts where they grow higher-value crops such as vegetables, condiments, and tubers relative to their local General Caste comparators.

---

### 4.2 The Agency Squeeze: Who Sells Where?

Before turning to prices, we examine *where* farmers sell. The structure of market channel access differs substantially across social groups.

<figure class="text-center">
  <img src="/assets/img/agriculture-discrimination/p2_channel_by_caste.png"
       alt="Stacked bar chart showing market channel distribution by social group, with SC and ST more reliant on local traders and less likely to use APMC Mandis" 
       class="img-fluid" 
       style="max-width: 85%; border-radius: 8px; box-shadow: 0 4px 8px rgba(0,0,0,0.1);">
  <figcaption class="mt-2 text-muted">
     <strong>Figure 2:</strong> Market channel distribution by social group (weighted % of crop sales). Blue shades = formal/regulated channels; Red shades = informal/trader channels.
  </figcaption>
</figure>

SC (80.5%) and ST (83.6%) farmers are substantially more reliant on Local Traders than General (76.3%) or OBC (75.4%) farmers. Access to formal channels is markedly lower for marginalized groups: only **13.4% of SC** and **10.8% of ST** sales flow through formal channels, versus **18.3% for General Caste**.

The Linear Probability Models (controlling for crop type and district) confirm this is not merely a composition artifact. **SC farmers are 2.57 percentage points less likely to sell at an APMC Mandi** (p < 0.01). Conditional on selling through a given channel (restricting to single-channel transactions), SC farmers face a further **2.0% within-channel penalty inside Local Trader sales** (p = 0.065) and a **3.8% within-channel penalty inside Government procurement** (p = 0.071)—indicating that caste-based disadvantage operates *inside* market channels, not only through channel selection.

A Baron-Kenny mediation analysis finds that differential formal channel access explains only **9.8% of the SC total price penalty** and **18.2% of the ST total price penalty**. The majority of the price gap is a direct within-channel effect, not a channel-selection effect.

---

#### 4.2.1 Spatial Polarization: State-Level Infrastructure Voids

To understand why market channel access differs so sharply across social groups, we map the spatial distribution of crop disposal channels across India using official state shapefiles. The maps reveal three distinct institutional regimes:

1. **Government Procurement Monopoly (Figure 3a):** Public procurement (FCI and state civil supplies corporations) is highly concentrated in surplus states like Punjab (42.5%), Haryana (38.1%), and Chhattisgarh (31.2%). Across eastern and southern India, government procurement accounts for under 3% of sales, leaving farmers in these regions without a state floor price.

<figure class="text-center">
  <img src="/assets/img/agriculture-discrimination/map_govt_share.png"
       alt="State-wise choropleth map of India showing Government Procurement share of crop sales, with highest procurement concentrated in Punjab, Haryana, and Chhattisgarh" 
       class="img-fluid" 
       style="max-width: 85%; border-radius: 8px; box-shadow: 0 4px 8px rgba(0,0,0,0.1);">
  <figcaption class="mt-2 text-muted">
     <strong>Figure 3a:</strong> Government Procurement Share by State (% of crop sales). State procurement (FCI / state agencies) is heavily concentrated in Punjab (42.5%), Haryana (38.1%), and Chhattisgarh (31.2%), while remaining below 3% across eastern and southern states.
  </figcaption>
</figure>

2. **APMC Mandi Density (Figure 3b):** Regulated wholesale mandis handle 25%–50%+ of crop sales in northwestern and central India (Punjab, Haryana, MP, Rajasthan). In contrast, mandis handle 0% of sales in Bihar (due to the 2006 APMC Act repeal) and Kerala, forcing farmers entirely into private channels.

<figure class="text-center">
  <img src="/assets/img/agriculture-discrimination/map_mandi_share.png"
       alt="State-wise choropleth map of India showing APMC Mandi share of crop sales, showing strong Mandi presence in Punjab, Haryana, MP, and Rajasthan" 
       class="img-fluid" 
       style="max-width: 85%; border-radius: 8px; box-shadow: 0 4px 8px rgba(0,0,0,0.1);">
  <figcaption class="mt-2 text-muted">
     <strong>Figure 3b:</strong> APMC Mandi Share by State (% of crop sales). Regulated wholesale mandis account for 25%–50%+ of sales in northwestern and central states, but virtually 0% in Bihar (following 2006 APMC repeal) and Kerala.
  </figcaption>
</figure>

3. **Private Trader Dominance (Figure 3c):** Informal private traders absorb 80% to 96% of all crop sales across Eastern India (Jharkhand: 96.2%, West Bengal: 90.2%, Mizoram: 90.0%). Crucially, these trader-dominated, infrastructure-poor states overlap almost exactly with the primary geographic concentration of Scheduled Tribe (ST) agricultural households.

<figure class="text-center">
  <img src="/assets/img/agriculture-discrimination/map_trader_share.png"
       alt="State-wise choropleth map of India showing the share of crop sales to private informal traders, with the darkest shades (90%+) concentrated in Jharkhand, West Bengal, and the North-East" 
       class="img-fluid" 
       style="max-width: 85%; border-radius: 8px; box-shadow: 0 4px 8px rgba(0,0,0,0.1);">
  <figcaption class="mt-2 text-muted">
     <strong>Figure 3c:</strong> Private Trader Share by State (% of crop sales). Jharkhand (96.2%), Mizoram (90.0%), and West Bengal (90.2%) show near-total trader dependence—regions characterized by large ST populations and an absence of formal market infrastructure.
  </figcaption>
</figure>

---

### 4.3 The Price Squeeze: Regression Results (H1)

After controlling for crop type, district, quantity, wealth, and land size, a systematic price penalty for marginalized groups persists for SC farmers across model specifications. The ST coefficient requires careful interpretation and is discussed separately.

<div class="table-responsive w-100">
  <table class="table table-bordered table-striped" style="width: 100%;">
    <thead>
      <tr>
        <th>Model</th>
        <th>SC Penalty</th>
        <th>ST Coefficient</th>
        <th>OBC</th>
      </tr>
    </thead>
    <tbody>
      <tr>
        <td>M1: Crop FE only</td>
        <td>−1.4%</td>
        <td>+1.5%</td>
        <td>+2.0%*</td>
      </tr>
      <tr>
        <td>M2: + Wealth & Land</td>
        <td>−1.0%</td>
        <td>−0.6%</td>
        <td>+1.9%*</td>
      </tr>
      <tr>
        <td>M3: + District FE <em>(preferred)</em></td>
        <td><strong>−1.1%</strong></td>
        <td><strong>−0.6%</strong> (n.s.)</td>
        <td><strong>+2.0%**</strong></td>
      </tr>
      <tr>
        <td>M4: + State FE <em>(over-controlled)</em></td>
        <td>−0.8%</td>
        <td><strong>−4.0%**</strong> ⚠</td>
        <td>+0.0%</td>
      </tr>
      <tr>
        <td>M5: District FE, no qty</td>
        <td>−0.3%</td>
        <td>+2.3%</td>
        <td>+2.5%**</td>
      </tr>
      <tr>
        <td>M6: Categorical Land FE <em>(replaces log land)</em></td>
        <td><strong>−1.3%</strong></td>
        <td><strong>+0.1%</strong> (n.s.)</td>
        <td><strong>+2.2%**</strong></td>
      </tr>
      <tr>
        <td>M7: Caste × Land Class Interaction</td>
        <td colspan="3"><em>Exploratory diagnostic (see text below)</em></td>
      </tr>
    </tbody>
  </table>
</div>

*Standard errors clustered at FSU level. \*p<0.05, \*\*p<0.01. Coefficients converted from log-points.*

The **SC penalty of −1.1% to −1.3%** (M3 and M6) is consistent across specifications. Model M6 replaces continuous `log_total_land` with categorical land-class fixed effects (`land_class_simple`), confirming that the estimated SC penalty is not an artefact of a log-linear land assumption.

**Caste × Class Interaction (M7):** Model M7 introduces interaction terms between caste categories and land-size classes (`caste_cat * land_class_simple`). This model is treated as *exploratory* for two reasons. First, the `SC × Large` cell has very few observations (SC farmers hold large landholdings at low rates nationally; $n=7$ in the regression sample), making any coefficient in that cell potentially driven by a single influential household rather than a systematic pattern. Second, the original specification included a "Landless" reference category with only $n=2$ observations, which caused a near-rank-deficient design matrix and degenerate standard errors in the hundreds to thousands. After collapsing "Landless" into "Marginal" (the smallest well-populated class), the model is better-identified, but the `SC × Large` cell remains flagged for thin coverage and its coefficient (approximately −50%) should not be interpreted as a robust finding. The M7 specification is retained in the pipeline as a diagnostic to show *where* caste-class interactions may be strongest, but specific interaction cell coefficients are not reported as primary findings.

The **M4 ST coefficient of −4.0%** (p < 0.01) is a geographic composition artefact and should not be interpreted as evidence of price discrimination against ST farmers. A geographic diagnostic shows that ST farmers are concentrated in 420 distinct districts—with 50% of all ST observations in top 42 districts—where they grow high-value crops and receive *higher* raw prices than General Caste farmers in both ST-concentrated and non-ST-concentrated geographic splits (concentrated districts: ST = 3.20, General = 2.94 in log price; non-concentrated: ST = 3.08, General = 2.95). State fixed effects (M4) are too coarse to absorb the crop-district confound that explains this pattern; district fixed effects (M3) and categorical land FE (M6) correctly reduce the ST coefficient to near-zero (+0.1% in M6).

**Quantile Regression Results (cluster-bootstrap SEs):** The SC price penalty does not concentrate at the extremes of the price distribution. At the 10th percentile (distress sales), SC farmers show a small positive coefficient (+1.1%, p = 0.41); at the median, −0.2% (p = 0.72); at the 90th percentile, −0.4% (p = 0.72). For ST farmers, the only statistically significant quantile effect is at the **median (−3.1%, p = 0.0013)**, with directional effects at Q90 (−2.1%, p = 0.085). The absence of a Q10 penalty for any marginalised group is inconsistent with a distress-sale mechanism and more consistent with a generalised within-market penalty at typical transaction prices.

---

### 4.4 Land Size Heterogeneity

The price penalty is not uniform across land size classes, as confirmed by the M7 joint interaction test.

<figure class="text-center">
  <img src="/assets/img/agriculture-discrimination/p3_land_size_penalty.png"
       alt="Point plot with 95% confidence intervals showing price penalty by land size category for OBC, SC, and ST farmers. Red dots indicate statistical significance." 
       class="img-fluid" 
       style="max-width: 90%; border-radius: 8px; box-shadow: 0 4px 8px rgba(0,0,0,0.1);">
  <figcaption class="mt-2 text-muted">
     <strong>Figure 4:</strong> Price penalty by land size category. Red = statistically significant (95% CI excludes zero). Error bars show 95% confidence intervals. All estimates control for crop type, district, and season.
  </figcaption>
</figure>

The most striking finding is that **OBC farmers with marginal landholdings receive substantially higher prices** than comparable General Caste farmers—a premium concentrated in the smallest land categories, possibly reflecting OBC integration into cooperative and local trader networks in certain states.

For **ST farmers, the largest penalty** is concentrated among *large* landholding households—a counterintuitive result that likely reflects the geographic distribution of large-landholding ST households in states with very low formal market penetration (Jharkhand, Chhattisgarh), where even size cannot substitute for absent infrastructure.

SC farmers show a consistent negative gradient across land classes—consistent with the hypothesis that the smallest SC farmers face the most acute pre-market dependencies.

---

### 4.5 Sale Satisfaction: The Subjective Market Experience

The NSS survey asks farmers whether they found each sale satisfactory, and if not, the primary reason for dissatisfaction. This is a powerful complement to the price data.

<figure class="text-center">
  <img src="/assets/img/agriculture-discrimination/p4a_satisfaction_major.png"
       alt="Bar chart showing satisfaction rates and below-market price dissatisfaction by social group." 
       class="img-fluid" 
       style="max-width: 85%; border-radius: 8px; box-shadow: 0 4px 8px rgba(0,0,0,0.1);">
  <figcaption class="mt-2 text-muted">
     <strong>Figure 5:</strong> Major sale outcomes by social group. Left panel: % reporting a satisfactory sale. Right panel: % citing below-market price as the primary dissatisfaction reason.
  </figcaption>
</figure>

The satisfaction data tells a clear story for SC farmers: only **56.2% report satisfactory sales**, compared to **64.7% for General Caste**—an 8.5-point gap. SC farmers are **39.2% likely to cite receiving a below-market price** as their primary grievance, compared to **29.5% for General Caste**. Regression-adjusted estimates confirm these gaps are statistically significant. These are the most precisely estimated effects in the entire analysis.

The ST result is a deliberate irony: **ST farmers report the highest satisfaction rate (69.3%)** of any group, and the lowest below-market price complaints (26.9%). This is entirely consistent with the geographic concentration finding—ST farmers grow high-value crops in their specific geographies and receive above-average aggregate prices within those local contexts. Their structural disadvantage lies in geographic isolation and channel exclusion, not in dissatisfaction within their local market transactions.

<figure class="text-center">
  <img src="/assets/img/agriculture-discrimination/p4b_satisfaction_minor.png"
       alt="Bar chart showing low-frequency dissatisfaction events by social group: delayed payment, faulty weighing, and loan deductions." 
       class="img-fluid" 
       style="max-width: 85%; border-radius: 8px; box-shadow: 0 4px 8px rgba(0,0,0,0.1);">
  <figcaption class="mt-2 text-muted">
     <strong>Figure 6:</strong> Minor dissatisfaction events (all below 2.5% of sales). Scale differs from Figure 5. No statistically significant caste differentials were found for these low-frequency grievances.
  </figcaption>
</figure>

For minor dissatisfaction events—faulty weighing, delayed payments, loan deductions—no statistically significant caste differentials emerge. These forms of low-level market manipulation appear broadly dispersed rather than targeted.

---

### 4.6 Blinder-Oaxaca Decomposition: Separating Endowments from Discrimination

The Blinder-Oaxaca decomposition decomposes the raw price gap between General Caste and other groups into: (a) the "explained" share due to observable differences in endowments (land, wealth, crop mix), and (b) the "unexplained" residual that cannot be attributed to these observables. All counterfactual expectations and group means are computed using NSS survey weights (`MLT/100`).

To test the sensitivity of the "explained" endowment component—which might launder historical land dispossession into an "observable" control—we run the decomposition both with and without the land control (`log_total_land`).

<div class="table-responsive w-100">
  <table class="table table-bordered table-striped" style="width: 100%;">
    <thead>
      <tr>
        <th>Specification / Comparison</th>
        <th>Raw Gap</th>
        <th>Explained (Endowments)</th>
        <th>Unexplained (Residual)</th>
      </tr>
    </thead>
    <tbody>
      <tr>
        <td colspan="4"><strong>Baseline (With Land Control)</strong></td>
      </tr>
      <tr>
        <td>General vs SC</td>
        <td>−1.0%</td>
        <td>69.4% of gap</td>
        <td><strong>30.6% of gap</strong></td>
      </tr>
      <tr>
        <td>General vs ST</td>
        <td>−14.8%</td>
        <td>118.5%</td>
        <td>−18.5%</td>
      </tr>
      <tr>
        <td>General vs OBC</td>
        <td>−6.7%</td>
        <td>59.7%</td>
        <td><strong>40.3%</strong></td>
      </tr>
      <tr>
        <td colspan="4"><strong>Sensitivity Check (Without Land Control)</strong></td>
      </tr>
      <tr>
        <td>General vs SC (No Land)</td>
        <td>−1.0%</td>
        <td>469.5% of gap</td>
        <td><strong>−369.5% of gap</strong> ⚠</td>
      </tr>
      <tr>
        <td>General vs ST (No Land)</td>
        <td>−14.8%</td>
        <td>96.4%</td>
        <td><strong>3.6%</strong></td>
      </tr>
      <tr>
        <td>General vs OBC (No Land)</td>
        <td>−6.7%</td>
        <td>59.9%</td>
        <td><strong>40.1%</strong></td>
      </tr>
    </tbody>
  </table>
</div>

For **SC farmers**, under the baseline specification with land, 69.4% of the raw gap is accounted for by observable differences in endowments—SC households grow more cereals and pulses and own less land. The remaining **30.6% is unexplained** by observable characteristics in the baseline specification.

However, a critical methodological insight emerges from the sensitivity check: because the SC raw price gap in the weighted sample is very small in magnitude (−0.96% or −0.0097 log points), omitting the land control causes the decomposition to swing dramatically (explained share becomes 469.5% and unexplained −369.5%). When the denominator (the total gap) is near zero relative to sampling noise in component counterfactuals, component percentages become hyper-sensitive and mechanically inflated. This is a known structural property of Oaxaca decompositions on small overall gaps. The SC decomposition must therefore be interpreted as showing that observable endowments explain a substantial portion of the small raw gap, rather than as a precise point estimate of the discrimination share.

For **ST farmers**, the raw gap is −14.8% (ST lower in the weighted decomposition), and the explained component is 118.5% with land and 96.4% without land—confirming that ST farmers' observable endowments (crop-district combinations) account for virtually the entire raw gap. The "negative unexplained" component (−18.5%) in the baseline model reflects the residual within-context price advantage ST farmers retain after accounting for their concentration in high-value crop-district pairs.

For **OBC farmers**, the unexplained residual remains highly stable between 40.3% (with land) and 40.1% (without land).

---

### 4.7 Mechanisms: The Three-Legged Stool of the Double Squeeze

**1. MSP Awareness Gap:** SC farmers are significantly less likely to be aware of Minimum Support Prices than General Caste farmers, controlling for land, wealth, and district (23.2% vs 40.3% raw awareness rates; ST even further behind at 23.2%). Farmers who do not know the floor price cannot negotiate toward it, and are structurally more vulnerable to below-market offers.

**2. Informal Debt and Tied Sales:** SC farmers are more likely to hold debt from moneylenders or traders than General Caste farmers (31.9% vs 19.5% carrying informal lender debt). This is the classic foothold of tied credit—where loan access is conditioned on channelling the crop back to the lender at a suppressed price. The direct price impact of informal lender debt in the preferred model is −1.25% but does not reach conventional significance thresholds (p = 0.168), suggesting that the mechanism may operate primarily through channel selection rather than a directly observable price discount.

**3. Input Cost Differentials:** Using an Inverse Hyperbolic Sine transformation—which retains the 10.6% of ST households with zero paid-out input expenditure (compared to <1% for all other groups)—ST farmers pay **88.7% less** in paid-out input expenses (p < 0.001), and SC farmers **6.4% less** (p = 0.122, not significant). The ST zero-expenditure rate is 25× higher than for OBC or SC farmers, reflecting a combination of subsistence input sourcing and resource constraints rather than differential access to formal input markets (no significant caste differences in institutional input sourcing are found). The large ST input expenditure gap reflects poverty and subsistence-mode production, not a formal market access barrier per se. The SC input gap is smaller in magnitude and less precisely estimated.

---

## 5. Discussion and Limitations

### What This Analysis Tells Us

The results reveal a picture of *structural channel exclusion and within-market disadvantage* operating simultaneously and at different intensities for different social groups.

**For SC farmers**, the corrected Blinder-Oaxaca decomposition shows that observable endowments (crop mix and land size) account for 69.4% of the raw gap in the baseline specification, leaving a 30.6% unexplained residual, though this ratio is sensitive to specification given the small overall raw gap (−1.0%). The within-channel penalty findings provide clearer evidence of within-market disadvantage: SC farmers face a 2.0% penalty in Local Trader sales (p = 0.065) and a 3.8% penalty in Government procurement (p = 0.071) *even within the same channel* as General Caste farmers.

The SC raw price gap of −Rs. 4.3/kg (cluster-robust weighted t = −2.34, p = 0.019) is precisely estimated and significant. The pathway to this penalty operates primarily through direct within-channel price-setting (explaining ~90% of the gap), with formal channel exclusion as a secondary but reinforcing mechanism (~10% mediated by channel access).

**For ST farmers**, the key finding is a *null* within-district price effect. After absorbing district-level heterogeneity, there is no statistically significant price penalty for ST farmers relative to General Caste—a finding that is obscured by state-level models which are too coarse to control for the geographic sorting of ST farmers into high-value crop-district combinations. The apparent M4 state-level penalty (−4.1%**) is a spurious artefact: ST farmers are concentrated in 42 districts where their crops command above-average prices, and state fixed effects cannot absorb this crop-district confound. Within those districts, ST farmers receive higher raw prices than any other group (log price 3.20 vs. 2.94 for General Caste). Their structural disadvantage is geographic: they live overwhelmingly in regions where 90%+ of sales flow through private traders with no formal market alternative.

**For OBC farmers**, the 42.1% unexplained Oaxaca residual is the most consistent finding across all robustness checks, and the within-APMC penalty (−3.4%*, single-channel-restricted) survives restriction to unambiguously single-channel transactions.

The quantile regression results further clarify the *type* of disadvantage: the absence of any significant SC penalty at Q10 (distress sales) and the presence of effects at Q50 for ST farmers indicates that caste disadvantage operates at *median* market conditions, not as a distress-specific mechanism. This is inconsistent with a pure debt-tied distress sale story and more consistent with persistent bargaining power asymmetries in normal market transactions.

### Limitations

1. **Causal identification is limited.** Fixed effects control for crop-district composition but cannot rule out within-district confounders such as intra-season timing of sales, unobserved crop quality differences, or transaction-level characteristics not captured in Block 6. The estimated coefficients should be interpreted as conditional associations, not causal estimates of the effect of social group on price.

2. **The Oaxaca residual is not pure discrimination.** Unobserved crop quality, bargaining experience, or timing differences could contribute to the 58.1% SC unexplained component. The decomposition establishes the upper bound on the market discrimination component; the true causal discrimination share may be lower.

3. **The ST null finding at M3 should be interpreted carefully.** The district-FE null for ST does not mean ST farmers face no disadvantage. Geographic isolation—documented in the state-level maps and the channel access data—is a real and significant form of structural exclusion that does not generate a within-district price gap precisely because there are no formal market alternatives in those districts to generate a price differential.

4. **SC sample size constrains precision at sub-group level.** With 8,343 SC observations in total, some land-size-class-level and within-channel coefficients carry wide confidence intervals. Joint tests and sign consistency across all model specifications strengthen inference in these cases.

5. **Mechanism testing is cross-sectional.** The NSS 77th Round covers a single agricultural year. Dynamic relationships—whether chronic informal debt leads to persistently lower prices over time, or whether cooperative membership improves prices through a learning effect—cannot be recovered from a single-period panel.

6. **Channel self-reports may misclassify mandis.** The NSS relies on farmer self-reports for market channel identification, which may conflate regulated APMCs with informal periodic markets in some states. Within-channel penalty estimates should be interpreted as lower bounds if misclassification attenuates the measured channel contrast.

7. **Multi-channel disposal is a small but present concern.** 5.1% of households sold the same crop through multiple channels, creating potential price-agency mismatch in recorded unit values. Restricting to single-channel-only households leaves all main results substantively unchanged (maximum coefficient shift of 0.5 percentage points), confirming robustness to this issue.

8. **The caste-class interaction model (M7) is exploratory, not confirmatory.** The original M7 specification used a "Landless" reference level with only $n=2$ observations, producing a near-rank-deficient design matrix and degenerate standard errors in the hundreds to thousands. After collapsing "Landless" into "Marginal," the model is better-identified, but several caste-class interaction cells—particularly `SC × Large`—remain thinly populated. Coefficients from thin interaction cells cannot be reliably separated from the influence of individual outlier households. The M7 model is retained as a diagnostic tool; no specific interaction cell coefficients are reported in the main text as confirmatory findings.

---

***
*All datasets, R scripts, diagnostic outputs, and spatial mappings used in this analysis are available open-source in the project GitHub repository. The full pipeline runs end-to-end in approximately 10 minutes on a standard laptop.*
