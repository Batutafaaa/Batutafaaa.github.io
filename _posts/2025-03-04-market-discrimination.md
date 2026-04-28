---
layout: post
title: "Market Access and Price Discrimination in Indian Agriculture"
date: 2026-03-19

comments: true
math: true  

description: "An analysis of NSS 77th Round data to examine how different class-caste groups participate in agricultural trade."

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
In the discourse on agrarian inequality in India, attention is typically directed toward land ownership, credit access, and debt cycles. However, the final stage of the agricultural cycle—the market exchange itself—remains deeply stratified by social identity. This article uses the National Sample Survey (NSS) 77th Round (2019–20) *Situation Assessment of Agricultural Households* to analyse over **74,565 crop sales records** spanning 36 states, 52,634 households, and two agricultural seasons. Using high-dimensional fixed-effects models, quantile regressions, and a manual Blinder-Oaxaca decomposition, we examine how different class-caste groups access agricultural trade. Our core findings are threefold: (1) Scheduled Caste (SC) farmers face a consistent **1.9% price penalty** relative to General Caste farmers within the same district, crop, and season—a gap driven primarily by differential access to formal markets rather than overt price-setting discrimination; (2) SC farmers are **2.6 percentage points less likely** to access APMC Mandis, with their formal market participation rate (14.7%) lagging nearly 6 points behind General Caste farmers (20.6%); (3) Scheduled Tribe (ST) farmers face a **4.6% price penalty** at the state level, driven in part by the near-complete absence of formal market infrastructure in their geographies. Our Blinder-Oaxaca decomposition reveals that 89% of the SC price gap is attributable to observable endowment differences (smaller landholdings, lower-value crop mix), while an 11% residual remains unexplained—consistent with market-level discrimination. These findings suggest that policy interventions focused solely on building more physical market infrastructure will remain insufficient without addressing the deeper socio-economic barriers that exclude marginalized groups from capturing the full value of their harvest.

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
This analysis uses the **NSS 77th Round (2019–20)**, a nationally representative household survey of agricultural households conducted across two seasonal visits (Kharif and Rabi). After merging crop sales records (Block 6) with household characteristics, land data, loan records, MSP awareness data, and input expenditure, and after applying symmetric 1%–99% outlier trimming on unit prices, the final analytic dataset contains **74,565 crop-sale observations** from **52,634 unique households** across **36 states and 142 crops**.

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

*   **Fixed-Effects OLS:** The core identification strategy uses crop fixed effects and district fixed effects to ensure that SC farmers are compared only to General Caste farmers selling the *same crop* in the *same district* in the *same season*. We additionally control for log MPCE and log total land area. Standard errors are clustered at the primary sampling unit (FSU) level.

*   **Quantile Regressions (Frisch-Newton method):** Quantile regressions at the 10th, 50th, and 90th percentiles reveal whether the price penalty concentrates among distress sales, typical transactions, or premium sales.

*   **Linear Probability Models (LPM):** We model the probability of selling to each channel (APMC Mandi, Government procurement, Local Trader, Cooperative, FPO) as a function of caste category, with crop and district fixed effects.

*   **Blinder-Oaxaca Decomposition:** Using separate price regressions for each caste-pair via `feols`, we compute counterfactual predicted values to decompose the raw price gap into an "explained" component (observable endowment differences) and an "unexplained" residual.

*   **Mechanisms Testing:** We examine three mechanisms: MSP awareness differentials, informal lender debt, and input cost differentials.

*(All analysis was conducted in R using the `fixest`, `quantreg`, and `sf` packages.)*

---

## 4. Results

### 4.1 Raw Price Gaps: The Starting Point

The first and most visible finding is a systematic divergence in average prices received across social groups. SC farmers receive an average of **Rs. 26.5/kg**, compared to **Rs. 30.8/kg** for General Caste farmers—a raw gap of Rs. 4.3/kg (roughly 14%).

<figure class="text-center">
  <img src="\assets\img\agriculture-discrimination\p1_price_by_caste.png"
       alt="Bar chart showing average crop price received by social group, with SC receiving the lowest at Rs.25.1/kg and General caste the highest at Rs.30.2/kg" 
       class="img-fluid" 
       style="max-width: 85%; border-radius: 8px; box-shadow: 0 4px 8px rgba(0,0,0,0.1);">
  <figcaption class="mt-2 text-muted">
     <strong>Figure 1:</strong> Average crop price received by social group (weighted means with 95% confidence intervals). N = 74,565 crop-sale records, NSS 77th Round.
  </figcaption>
</figure>

However, this raw gap is importantly a *composition* effect as much as a discrimination effect. SC households grow more cereals and pulses (lower-value crops) and hold smaller landholdings on average (4.78 acres vs. 7.6 acres for General Caste). A t-test confirms the SC gap (−Rs. 4.30/kg, t = −5.46, p < 0.001) is highly significant. Interestingly, the ST group shows a smaller raw gap (−Rs. 1.19/kg, t = −1.92, p = 0.054)—explained by their concentration in certain higher-value niches despite overall structural disadvantage.

---

### 4.2 The Agency Squeeze: Who Sells Where?

Before turning to prices, we examine *where* farmers sell. The structure of market channel access differs substantially across social groups.

<figure class="text-center">
  <img src="\assets\img\agriculture-discrimination\p2_channel_by_caste.png"
       alt="Stacked bar chart showing market channel distribution by social group, with SC and ST more reliant on local traders and less likely to use APMC Mandis" 
       class="img-fluid" 
       style="max-width: 85%; border-radius: 8px; box-shadow: 0 4px 8px rgba(0,0,0,0.1);">
  <figcaption class="mt-2 text-muted">
     <strong>Figure 2:</strong> Market channel distribution by social group (weighted % of crop sales). Blue shades = formal/regulated channels; Red shades = informal/trader channels.
  </figcaption>
</figure>

SC (79.5%) and ST (81.6%) farmers are substantially more reliant on Local Traders than General (74.3%) or OBC (74.0%) farmers. Access to formal channels is markedly lower for marginalized groups: only **14.7% of SC** and **12.6% of ST** sales flow through formal channels, versus **20.6% for General Caste**.

The Linear Probability Models (controlling for crop type and district) confirm this is not merely a composition artifact. **SC farmers are 2.57 percentage points less likely to sell at an APMC Mandi** (p < 0.01)—one of the only statistically significant and robust channel results in the data. Conditional on selling through a given channel, SC farmers face a further **2.8% penalty within Local Trader sales** (p < 0.05) and a **4.9% penalty within Government procurement sales**—indicating that caste-based disadvantage operates *inside* market channels, not only through channel selection.

> **Geographic Context:** Private Trader dominance is highest in Eastern and North-Eastern India—precisely where ST agricultural households concentrate—reflecting both the absence of regulated markets and the structural isolation of tribal farming communities.

<figure class="text-center">
  <img src="\assets\img\agriculture-discrimination\map_trader_share.png"
       alt="State-wise choropleth map of India showing the share of crop sales to private informal traders, with the darkest shades (90%+) concentrated in Jharkhand, West Bengal, and the North-East" 
       class="img-fluid" 
       style="max-width: 85%; border-radius: 8px; box-shadow: 0 4px 8px rgba(0,0,0,0.1);">
  <figcaption class="mt-2 text-muted">
     <strong>Figure 3:</strong> Private Trader Share by State (% of crop sales). Jharkhand (96.2%), Mizoram (90%), and West Bengal (90.2%) show the highest trader dependence—all states with large ST populations and minimal formal market infrastructure.
  </figcaption>
</figure>

---

### 4.3 The Price Squeeze: Regression Results (H1)

After controlling for crop type, district, quantity, wealth, and land size, a systematic price penalty for marginalized groups persists across model specifications.

<div class="table-responsive w-100">
  <table class="table table-bordered table-striped" style="width: 100%;">
    <thead>
      <tr>
        <th>Model</th>
        <th>SC Penalty</th>
        <th>ST Penalty</th>
        <th>OBC</th>
      </tr>
    </thead>
    <tbody>
      <tr>
        <td>M1: Crop FE only</td>
        <td>−2.4%</td>
        <td>−2.0%</td>
        <td>+1.2%</td>
      </tr>
      <tr>
        <td>M2: + Wealth & Land</td>
        <td>−1.7%</td>
        <td>−3.8%</td>
        <td>+1.5%</td>
      </tr>
      <tr>
        <td>M3: + District FE</td>
        <td>−1.9%</td>
        <td>−3.4%</td>
        <td>+1.2%</td>
      </tr>
      <tr>
        <td>M4: + State FE</td>
        <td>−0.7%</td>
        <td><strong>−4.6%**</strong></td>
        <td>+0.3%</td>
      </tr>
      <tr>
        <td>M5: District FE, no qty</td>
        <td>−1.3%</td>
        <td>−1.3%</td>
        <td>+1.6%</td>
      </tr>
    </tbody>
  </table>
</div>

*Standard errors clustered at FSU level. \*p<0.05, \*\*p<0.01. Coefficients converted from log-points.*

The **SC penalty of −1.9%** (M3) is consistent across specifications, and remains economically meaningful though the within-district precision is limited. The **ST penalty of −4.6%** with state controls (M4) is statistically significant (p < 0.01), reflecting within-state disadvantage concentrated in lower-infrastructure regions.

**Quantile Regression Results:** The price penalty does not concentrate at the extremes. At the 10th percentile (distress sales), SC farmers show a small *positive* coefficient (+0.8%)—market distress is broadly shared. At the median, SC farmers show −0.6%, and at the 90th percentile −0.8%. For ST farmers, the most pronounced penalty is at the median (−1.2%). This pattern is inconsistent with a simple "glass ceiling" story, and more consistent with a generalised exclusion from formal market channels that pays a price premium.

---

### 4.4 Land Size Heterogeneity

The price penalty is not uniform across land size classes.

<figure class="text-center">
  <img src="\assets\img\agriculture-discrimination\p3_land_size_penalty.png"
       alt="Point plot with 95% confidence intervals showing price penalty by land size category for OBC, SC, and ST farmers. Red dots indicate statistical significance." 
       class="img-fluid" 
       style="max-width: 90%; border-radius: 8px; box-shadow: 0 4px 8px rgba(0,0,0,0.1);">
  <figcaption class="mt-2 text-muted">
     <strong>Figure 4:</strong> Price penalty by land size category. Red = statistically significant (95% CI excludes zero). Error bars show 95% confidence intervals. All estimates control for crop type, district, and season.
  </figcaption>
</figure>

The most striking finding is that **OBC farmers with marginal landholdings receive 32.2% higher prices** than comparable General Caste farmers—a premium concentrated in the smallest land categories, possibly reflecting OBC integration into cooperative and local trader networks in certain states.

For **ST farmers, the largest penalty (−4.2%, significant)** is concentrated among *large* landholding households—a counterintuitive result that likely reflects the geographic distribution of large-landholding ST households in states with very low formal market penetration (Jharkhand, Chhattisgarh), where even size cannot substitute for absent infrastructure.

SC farmers show a consistent negative gradient: −5.0% for marginal landholders (not significant due to small cell sizes), and −2.0% for large farmers—consistent with the hypothesis that the smallest SC farmers face the most acute pre-market dependencies.

---

### 4.5 Sale Satisfaction: The Subjective Market Experience

The NSS survey asks farmers whether they found each sale satisfactory, and if not, the primary reason for dissatisfaction. This is a powerful complement to the price data.

<figure class="text-center">
  <img src="\assets\img\agriculture-discrimination\p4a_satisfaction_major.png"
       alt="Bar chart showing satisfaction rates and below-market price dissatisfaction by social group." 
       class="img-fluid" 
       style="max-width: 85%; border-radius: 8px; box-shadow: 0 4px 8px rgba(0,0,0,0.1);">
  <figcaption class="mt-2 text-muted">
     <strong>Figure 5:</strong> Major sale outcomes by social group. Left panel: % reporting a satisfactory sale. Right panel: % citing below-market price as the primary dissatisfaction reason.
  </figcaption>
</figure>

The satisfaction data tells a clear story for SC farmers: only **56.2% report satisfactory sales**, compared to 68.2% for General Caste—a 12-point gap. SC farmers are **39.4% likely to cite receiving a below-market price** as their grievance, compared to 27.2% for General Caste. Regression-adjusted estimates confirm these gaps: SC farmers are **9.97 percentage points less likely to report satisfaction** (p < 0.001) and **10.56 percentage points more likely to report below-market pricing** (p < 0.001). These are the most precisely estimated effects in the entire analysis.

The ST result is a deliberate irony: **ST farmers report the highest satisfaction rate (72.7%)** of any group, and the lowest below-market price complaints (24.6%). This is entirely consistent with the Oaxaca finding—ST farmers grow high-value crops in their specific geographies and receive above-average aggregate prices within those contexts. Their structural disadvantage lies in geographic isolation and channel exclusion, not in dissatisfaction within their local market transactions.

<figure class="text-center">
  <img src="\assets\img\agriculture-discrimination\p4b_satisfaction_minor.png"
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

The Blinder-Oaxaca decomposition decomposes the raw price gap between General Caste and other groups into: (a) the "explained" share due to observable differences in endowments (land, wealth, crop mix), and (b) the "unexplained" residual that cannot be attributed to these observables.

<div class="table-responsive w-100">
  <table class="table table-bordered table-striped" style="width: 100%;">
    <thead>
      <tr>
        <th>Comparison</th>
        <th>Raw Gap</th>
        <th>Explained (Endowments)</th>
        <th>Unexplained (Residual)</th>
      </tr>
    </thead>
    <tbody>
      <tr>
        <td>General vs SC</td>
        <td>+6.9% (General higher)</td>
        <td>89.0% of gap</td>
        <td><strong>11.0% of gap</strong></td>
      </tr>
      <tr>
        <td>General vs ST</td>
        <td>−6.8% (ST higher)</td>
        <td>179.3%</td>
        <td>−79.3%</td>
      </tr>
      <tr>
        <td>General vs OBC</td>
        <td>−2.2% (OBC higher)</td>
        <td>23.1%</td>
        <td>76.9%</td>
      </tr>
    </tbody>
  </table>
</div>

For **SC farmers**, 89% of the raw gap is accounted for by observable differences—SC households grow more cereals and pulses and own less land, both of which mechanically predict lower prices. The remaining **11% (~0.7 log points)** is unexplained, and is consistent with direct market discrimination in price-setting. In absolute terms, this represents roughly Rs. 0.55/kg of the raw gap that cannot be attributed to crop or land differences.

For **ST farmers**, the raw gap is *negative*—ST farmers aggregate higher prices because of their crop mix. The large "unexplained" share reflects the structural complexity of comparing groups with very different geographic and crop-type footprints and should not be misread as ST farmers facing no disadvantage.

---

### 4.7 Mechanisms: The Three-Legged Stool of the Double Squeeze

**1. MSP Awareness Gap:** SC farmers are **8.3 percentage points less likely** to be aware of Minimum Support Prices; ST farmers are even further behind at **−15.5 percentage points** (both p < 0.001, controlling for land, wealth, and district). Farmers who do not know the floor price cannot negotiate toward it, and are structurally more vulnerable to below-market offers.

**2. Informal Debt and Tied Sales:** SC farmers are **7.2 percentage points more likely** to hold debt from moneylenders or traders (p < 0.01) than General Caste farmers. This is the classic foothold of tied credit—where loan access is conditioned on channelling the crop back to the lender at a suppressed price. The direct price impact of informal lender debt in our model is small and insignificant (−0.19%), suggesting that either the mechanism operates through channel selection (not captured in the price equation) or the survey price variable does not fully capture tied-sale discount.

**3. Input Cost Differentials:** SC farmers pay **9.6% less** in paid-out input expenses (p < 0.05), and ST farmers **53.5% less** (p < 0.001). This reflects resource constraints—SC and ST households cannot afford purchased inputs and are correspondingly less able to produce high-quality or commercially-oriented crops that command price premiums in the market.

---

## 5. Discussion and Limitations

### What This Analysis Tells Us

The results reveal a picture of *structural channel exclusion* rather than simple overt discrimination at the point of sale.

The raw SC price gap of Rs. 5.1/kg shrinks but does not vanish when crop mix, district, and season are controlled—a 2% regression-adjusted penalty persists. The pathway to this penalty is more important than its size: SC farmers are excluded from the formal APMC circuit (−2.6 ppts access), which consistently pays higher prices. When they do access formal outlets, they face an additional within-channel penalty. Their subjective experience confirms this: they are 10 percentage points more likely to report feeling underpaid.

The Oaxaca decomposition finds that 89% of the SC raw price gap is "explained" by crop composition and land endowments. This should not be interpreted as exonerating the market—these endowment differences are themselves the product of historical land dispossession and credit exclusion rooted in caste hierarchy. The 11% unexplained residual is a conservative lower bound on the market discrimination component.

The ST findings are structurally distinct. ST farmers' aggregate price advantage is a crop-mix artifact. Their real disadvantage is geographic capture: they live overwhelmingly in states where private traders hold 90%+ market share and no formal alternative exists. Their penalty is not competitive; it is structural.

### Limitations

1. **Causal identification is limited.** Fixed effects control for crop-district composition but cannot rule out within-district confounders such as intra-season timing of sales or unobserved crop quality differences.

2. **SC sample size constrains precision.** With 4,412 SC observations, many coefficients are economically meaningful but do not individually clear conventional significance thresholds. Joint tests and the consistent sign across all 6 model specifications strengthen the inference.

3. **The Oaxaca residual is not pure discrimination.** Unobserved crop quality, bargaining skill, or timing differences could contribute to the unexplained component. The exercise establishes bounds, not a causal estimate of discrimination.

4. **Cross-sectional limits mechanism testing.** The NSS 77th Round is a single cross-section. Dynamic relationships between debt and price outcomes over time cannot be recovered.

5. **Channel self-reports may misclassify mandis.** The NSS relies on farmer self-reports for market channel identification, which may conflate regulated APMCs with informal periodic markets in some states.

---

## 6. Conclusion and Policy Implications

Agricultural markets in India are not neutral price discovery mechanisms—they are social institutions that reproduce existing hierarchies unless deliberately structured otherwise.

The primary mechanism of the double squeeze operates through structural channel exclusion and information deprivation. SC farmers are locked out of the formal APMC circuit by a combination of social distance, lower MSP awareness, and informal debt dependencies. ST farmers face geographic capture in regions where no formal alternative exists. Policy interventions focused only on building more Mandis or expanding MSP lists will not reach farmers who do not know these prices exist, or farmers in states where trader monopolies are total.

Effective responses must work across multiple levels simultaneously:
- **Targeted MSP information campaigns** directed at SC and ST communities, to close the 8–15 percentage point awareness gap.
- **Disaggregated monitoring within APMC Mandis**, to detect and penalise within-market caste-based price-setting (our models find a −4.8% SC penalty within APMC sales).
- **Institutional credit for SC and ST households** at rates that break the link between input financing and tied crop sales.
- **Cooperative and FPO development in tribal regions**—notably, SC and ST farmers in cooperatives receive *higher* prices than comparable General Caste farmers (SC: +7.3%\*\*, ST: +10.7%\*\*), making these institutions particularly high-return interventions.

The 11% unexplained component of the SC price gap, and the 4.6% ST penalty within states, represent a credible evidence base for market-level discrimination in Indian agricultural price-setting. Closing these gaps requires moving beyond infrastructure and toward structural reform of the social terms on which marginalized farmers enter the market.

***
*All datasets, R scripts, diagnostic outputs, and spatial mappings used in this analysis are available open-source in the project GitHub repository. The full pipeline runs end-to-end in approximately 1 minute on a standard laptop.*

