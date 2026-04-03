<div align="center">
  <img width="1000px" src="Visualization%20PNGs/Logo.png" />
</div>

<h1 align="center">AeroPulse Airlines Passenger Satisfaction Analysis</h1>

## Project Background

**AeroPulse** is a global airline operating a diverse fleet serving both business and leisure travellers. Since its founding in 2015, the company has expanded rapidly, but like many in the industry, it faces increasing competition and evolving passenger expectations. To better understand its customers and improve service quality, AeroPulse conducted a satisfaction survey of its passengers.

The dataset contains **129,880 responses** with 24 variables, including service ratings (1‑5, plus “Not Applicable”), passenger demographics, travel details, and overall satisfaction (Satisfied / Neutral / Dissatisfied). The company’s customer base spans a wide range of travel classes, loyalty statuses, and age groups.

Reporting to the VP of Operations, an in-depth analysis was conducted to evaluate AeroPulse’s performance across key service areas. This comprehensive review provides actionable insights that will help the company streamline operations, enhance passenger satisfaction, and retain valuable customers. The analysis focuses on three main questions:

- Which services are rated highest and lowest by passengers?
- What are the characteristics of satisfied vs. dissatisfied passengers?
- Which services have the strongest impact on overall satisfaction – and where should the airline invest?

---

<h1 align="center">Executive Summary</h1>

<div align="center">
  <img src="Visualization%20PNGs/Dashboard.png" alt="Dashboard" width="100%">
</div>

### Key Findings at a Glance

| Area | Summary |
|------|---------|
| **Best & worst rated services** | **Top 3:** Cleanliness (3.72/5), Baggage handling (3.71/5), Online support (3.54/5)<br> **Bottom 3:** Food & drink (2.99/5), Seat comfort (2.95/5), Gate location (3.00/5)<br> **Lowest rated overall:** **Seat comfort (2.95/5)** |
| **Who is satisfied vs. dissatisfied?** (strongest predictors) | **More satisfied:** Business class (71% satisfied), Loyal customers (62%)<br>**More dissatisfied:** Economy class (39% satisfied), Disloyal customers (24%)<br>These differences are both statistically significant and practically meaningful (Cramér’s V = 0.44 and 0.29 respectively). Gender and age also show differences, but their influence is secondary. |
| **Top 3 drivers of satisfaction** (Average Marginal Effect) | 1. Inflight entertainment – +11.7 pp per 1‑point increase<br>2. Seat comfort – +5.8 pp<br>3. Ease of online booking – +5.2 pp |
---

<h1 align="center">Data Preparation & Methodology</h1>

The raw dataset contained 129,880 survey responses. Service ratings include a value of `0` meaning “Not Applicable” – i.e., the passenger did not use that service. Including these zeros would artificially lower average ratings and bias logistic regression coefficients (treating non‑usage as a poor rating). Therefore, we retained only passengers who used **all services** (i.e., no zero ratings). This removed only **8% of records**, leaving a clean sample of 119,612 passengers for service‑related analysis. For passenger characteristic analysis (age, gender, class, etc.), we used the full dataset because satisfaction grouping does not require service usage.

To prepare the target variable for modelling, and since this variable only has two outcomes in the first place, we transformed the original Overall_Satisfaction into a binary indicator: `1` for “Satisfaction” and `0` for “Neutral or Dissatisfaction”.

All statistical tests were performed with appropriate assumptions checked:
- **Categorical variables**: Chi‑square test of independence + Cramér’s V (effect size)
- **Age**: Independent t‑test (Welch’s correction) + Cohen’s d (effect size)
- **Service impact**: Multiple logistic regression + Average Marginal Effects (AME), plus diagnostics (VIF, univariate regressions, reduced models) to investigate unexpected coefficients.

<div align="center">
  <img src="Visualization%20PNGs/Cleaning%20Steps%20Flowchart.png" alt="Data cleaning flowchart" width="100%">
</div>

---

<h1 align="center">Insights Deep‑Dive</h1>

### 1. Service Ratings – Where the Airline Excels and Where It Fails

<div align="center">
  <img src="Visualization%20PNGs/Top%205%20and%20Bottom%205%20Service%20Ratings.png" alt="Top 5 and bottom 5 service ratings" width="100%">
</div>

- **Highest rated:** Cleanliness (3.72), Baggage handling (3.71), Online support (3.54), Leg room service (3.52), Ease of online booking (3.51). All are above the overall service average of 3.35, but none exceed 4, indicating room for improvement.
- **Lowest rated:** Seat comfort (2.95), Food & drink (2.99), Gate location (3.00), Departure/Arrival time convenient (3.13), Inflight wifi service (3.26).
- **Critical gap:** Seat comfort (2.95/5) is the only service below 3. This is an operational emergency. Passengers actively dislike their seating experience.  

*Confidence intervals (95%) are extremely narrow due to large sample size, so all rankings are statistically robust.*

---

### 2. Passenger Characteristics – Who Is Satisfied and Who Is Not?

We compared satisfied passengers (Overall_Satisfaction = “Satisfaction”) with not‑satisfied (Neutral or Dissatisfaction) across five characteristics. All differences were statistically significant (p < 0.001), but practical significance varies. The table below groups them by impact level.

<div align="center">
  <img src="Visualization%20PNGs/At-risk%20Segments%20Breakdown.png" alt="At-risk segments breakdown" width="100%">
</div>

<div style="display: flex; gap: 2rem; flex-wrap: wrap;">
  <div style="flex: 0.6; min-width: 250px; border-right: 3px solid #ddd; padding-right: 2rem;">
    <h4>Tier 1: High‑Impact Factors (The Priorities)</h4>
    <ul>
      <li>
        <strong>Class:</strong> Cramér’s V = 0.44 (considerable).<br>
        – Business class: 71% satisfied<br>
        – Economy: 39% satisfied<br>
        – Eco Plus: 43% satisfied<br>
        <em>Insight:</em> Economy class is the primary dissatisfied segment. Investing in better seats or perks for this group would yield the largest improvement in satisfaction.
      </li>
      <li>
        <strong>Customer Type:</strong> Cramér’s V = 0.29 (moderate).<br>
        – Loyal customers: 62% satisfied<br>
        – Disloyal customers: 24% satisfied<br>
        <em>Insight:</em> Disloyal customers are disproportionately dissatisfied. This suggests a failure in the first‑time user experience or a lack of incentives to stay. Focus on onboarding and loyalty enrolment.
      </li>
    </ul>
  </div>
  <div style="flex: 1; min-width: 250px;">
    <h4>Tier 2: Secondary Factors (The Nuance)</h4>
    <ul>
      <li>
        <strong>Gender:</strong> Cramér’s V = 0.21 (small‑to‑moderate).<br>
        – Female: 65% satisfied<br>
        – Male: 44% satisfied<br>
        <em>Insight:</em> While gender shows a meaningful gap, its effect is smaller than class or loyalty. Tailored marketing may help, but it is not a primary predictor of dissatisfaction.
      </li>
      <li>
        <strong>Age:</strong> Cohen’s d = 0.24 (small‑to‑medium).<br>
        – Satisfied: mean age 41.05<br>
        – Dissatisfied: mean age 37.47<br>
        <em>Insight:</em> Younger passengers are somewhat more dissatisfied. However, the difference is modest (3.6 years). Consider entertainment or digital offerings that appeal to younger demographics, but do not prioritise over class and loyalty.
      </li>
      <li>
        <strong>Travel Type:</strong> Cramér’s V = 0.11 (negligible).<br>
        – Business: 58% satisfied<br>
        – Personal: 47% satisfied<br>
        <em>Insight:</em> The difference is small and likely not actionable on its own.
      </li>
    </ul>
  </div>
</div>  

---

### 3. What Drives Overall Satisfaction? (Logistic Regression & Marginal Effects)

We built a multiple logistic regression model with all 14 service ratings as predictors and overall satisfaction (binary) as the outcome. To make results interpretable, we calculated **Average Marginal Effects (AME)**; the average change in the probability of being satisfied for a one‑unit increase in a service’s rating, holding all other services constant.

<div align="center">
  <img src="Visualization%20PNGs/Services%20Sorted%20by%20Average%20Marginal%20Effect.png" alt="Services sorted by Average Marginal Effect" width="100%">
</div>

#### Top 3 positive drivers (largest AME)

1. **Inflight entertainment** – +11.7 percentage points
2. **Seat comfort** – +5.8 pp
3. **Ease of online booking** – +5.2 pp

#### Notable negative coefficients in the full model

- **Departure/Arrival time convenient** – AME = -5.9 pp (higher rating → lower satisfaction)
- **Inflight wifi service** – AME = -1.7 pp

These negative signs are counterintuitive and prompted a deeper investigation.

#### Investigating the Negative Signs

**For Inflight wifi:**  
- **Univariate regression:** coefficient = +0.385 (positive, as expected).  
- **Correlation matrix:** wifi correlated with Online support (0.55), Ease of Online booking (0.59), and Online boarding (0.62).  
- **VIF:** all < 4, indicating no severe multicollinearity but moderate correlations.  
- **Reduced models:**  
  - Removing correlated services one at a time kept wifi’s coefficient negative.  
  - Removing *all three* correlated services simultaneously (Online support, Ease of Online booking, Online boarding) flipped the coefficient to **positive (+0.212, OR=1.236)**.  
- **Conclusion:** This is a suppression effect. The full model over‑controls for shared variance, masking wifi’s true positive impact. Inflight wifi does positively affect satisfaction, but its unique contribution is smaller than the top three drivers.
- **Robustness of top drivers:** Across all sensitivity tests; univariate regressions, reduced models, and the full model, the top services with the largest positive impact on satisfaction remained consistent: Inflight entertainment, Seat comfort, and Ease of online booking. The sign flip for wifi, while interesting, does not affect the ranking of these top drivers. This indicates that the full model's core conclusions are reliable despite the suppression effect.

**For Departure/Arrival time convenient:**  
- **Univariate regression:** coefficient = –0.038 (still negative, though small).  
- **Further investigation:** We broke down average satisfaction by rating for this service across passenger segments (see appendix). The negative relationship is not uniform:  
  - For Business travellers, satisfaction increases with rating (as expected).  
  - For Personal travellers and Economy class, satisfaction *decreases* as rating increases.  
- **Conclusion:** The variable may be a proxy for other factors (e.g., passengers traveling on holidays rate convenience high but have high expectations, leading to lower overall satisfaction). Additional data is needed to interpret this service correctly.

---

### 4. Which Services Should the Airline Prioritise?

Combining the **rating gaps** (lowest rated services) with the **impact on satisfaction** (highest AME), we identify clear priorities:

<div align="center">
  <table>
    <thead>
      <tr><th>Service</th><th>Current rating (1‑5)</th><th>AME (pp)</th><th>Priority</th></tr>
    </thead>
    <tbody>
      <tr><td><strong>Seat comfort</strong></td><td>2.95 (lowest)</td><td>+5.8</td><td><strong>Urgent</strong> (worst rated + high impact)</td></tr>
      <tr><td><strong>Inflight entertainment</strong></td><td>3.47</td><td>+11.7</td><td>High (largest impact, moderate rating)</td></tr>
      <tr><td><strong>Ease of online booking</strong></td><td>3.51</td><td>+5.2</td><td>Medium‑High</td></tr>
    </tbody>
  </table>
</div>
<div align="center">
  <img src="Visualization%20PNGs/Impact%20vs.%20Current%20Avg.%20Rating%20Matrix.png" alt="Impact vs. Current Rating Matrix" width="100%">
</div>

---

<h1 align="center">Recommendations</h1>

- **Immediate operational fix:** **Seat comfort** (2.95/5) is the lowest rated and the second most impactful driver. Invest in new seat cushions, legroom reconfiguration, or premium economy upgrades. Run A/B tests on a subset of aircraft.
- **High‑impact, moderate‑rating services:** **Inflight entertainment** (3.47/5) has the highest AME (+11.7 pp). Update content libraries, improve screen quality, and promote availability. This yields the biggest satisfaction lift per unit improvement.
- **Digital experience:** **Ease of online booking** (3.51/5) drives satisfaction and is correlated with wifi and online support. Streamline the booking funnel and ensure the website/app is fast and intuitive.

- **Target at‑risk segments (prioritised by impact):**  
  - **Economy class** (39% satisfied) – offer affordable seat upgrade promotions or dedicated cabin enhancements. This is the most critical demographic.  
  - **Disloyal customers** (24% satisfied) – launch loyalty enrolment campaigns with immediate perks (e.g., priority boarding, extra legroom).  
  - **Younger passengers** – tailor entertainment and digital offerings, but this is secondary to the above.  
  - **Males** (44% satisfied) – investigate if certain services (e.g., seat comfort, wifi) are rated differently; consider targeted communication.

- **Investigate “Departure/Arrival time convenient” further:** The negative coefficient is inconsistent across segments. Collect additional data: actual delay minutes, ticket price, and flight date (to identify holiday travel). This will help determine whether the variable is a proxy for other factors or if the survey scale is misinterpreted.

- **Maintain strengths with a hygiene perspective:** Cleanliness and baggage handling are rated highest, yet they have very low AME (0.2 pp and 0.4 pp respectively). According to Herzberg’s two‑factor theory, these are **hygiene factors**, they do not directly drive satisfaction when good, but their absence would cause significant dissatisfaction. Do not cut investment here; maintain current levels to avoid a sharp drop in passenger perception.

- **Improve the survey itself:** The high correlation among online‑related services (wifi, online support, online booking, online boarding) makes it hard to isolate their individual effects. Consider redesigning the survey to reduce multicollinearity, e.g., by rephrasing questions or using conjoint analysis.

---

<h1 align="center">Technical Appendix</h1>

- **Software:** Microsoft Excel (Office 365).  
  - **Data cleaning and summary statistics:** Built‑in functions (`XLOOKUP`, `STDEV.S`, `TOCOL`, etc.).  
  - **t‑Test:** Built-in function (`SKEW`, `SQRT`), Data Analysis Toolpak.  
  - **Logistic regression:** XLMiner (for coefficient estimates, odds ratios, marginal effects).  
  - **Chi-Square, VIF, AME:** Manual calculations using built-in functions.  
- **Effect size benchmarks:** Cramér’s V: 0.1 = small, 0.3 = moderate, 0.5 = large. Cohen’s d: 0.2 = small, 0.5 = medium, 0.8 = large.
- **Logistic regression diagnostics:** Pseudo R‑square (calculated as McFadden’s, but not included in the final output because it is less interpretable (check the full Excel workbook "AeroPulse Airlines Satisfaction Analysis"); the focus is on marginal effects and odds ratios). VIF all < 4. Negative coefficients for wifi and departure time were investigated with univariate and reduced models as described.
- **Data cleaning:** Rows with any service rating = 0 (Not Applicable) were removed for service‑related analyses only. Final sample size for regression: 119,612. Full dataset used for passenger characteristics: 129,880.
- **Reproducibility:** The Excel workbook with all formulas, pivot tables, and XLMiner outputs, is included in this repository. Any user can follow the tabs and see the exact calculations.

---

<hr>

<p align="center">📊 <strong>Author:</strong> Taha B. Ahmed | 🔗 <a href="https://www.linkedin.com/in/tahabahaa/">LinkedIn</a> | 📧 <a href="mailto:tahabahaa@outlook.com">tahabahaa@outlook.com</a> | 📁 <a href="AeroPulse%20Airlines%20Satisfaction%20Analysis.xlsx">Full Excel Analysis</a></p>
