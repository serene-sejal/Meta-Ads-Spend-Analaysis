# Meta-Ads-Spend-Analaysis
A data analytics project examining 10,000+ Meta advertising transactions to identify patterns in market concentration, advertiser dominance, and ROI efficiency. Using SQL and Tableau, the project transforms raw ad data into actionable business insights that mirror financial portfolio analysis and support strategic market decisions.

### Process Overview

- ### Data Preparation:
  Imported multi-table Meta Ads data, cleaned duplicates, standardized naming conventions, and handled NULL values through SQL CTEs and conditional logic.

- ### Data Modelling:
  Computed key market structure metrics such as Herfindahl-Hirschman Index (HHI), CR10, and Top-15 Spend Share to assess competitiveness.

- ### Statistical Analysis:
  Derived KPIs, including Spend Volatility Index and Spend–Volume Correlation, to evaluate advertiser consistency and efficiency.

- ### Visualization:
  Designed an interactive Tableau dashboard illustrating advertiser share distribution, volatility fluctuations, and spend concentration levels.

- ### Interpretation:
  Correlated market concentration metrics with performance outcomes to uncover trends in competitive behavior and market sustainability.

  ### SQL Snippet: Market Concentration Computation
To evaluate advertiser dominance, the Ad Saturation Index computes each advertiser's share of total ad volume and spend. The query safely casts data and aggregates proportional representation to identify overexposed players in the Meta Ads market

```sql
-- 5) Ad Saturation Index (schema-qualified + safe casting)
select v.advertisers_name,
sum(v.spend_usd) as total_spend_usd, 
sum(coalesce(cast(replace(r.num_ads,',','') as unsigned),0)) as total_ads,
round(
	100.0 * sum(coalesce(cast(replace(r.num_ads,',','') as unsigned),0))
    /
nullif(
(select sum(coalesce(cast(replace(num_ads,',','') as unsigned),0)) 
from meta_ads_spend.raw_advertisers),0),2) as ad_saturation_pct
from meta_ads_spend.v_advertisers_clean as v
left join meta_ads_spend.raw_advertisers as r 
using (page_id)
group by v.advertisers_name
having total_ads > 0
order by ad_saturation_pct desc
limit 15;
```

The following SQL snippet highlights the data modelling logic used to compute market concentration through the Herfindahl–Hirschman Index (HHI), a key measure of competitiveness in advertising markets.

<img width="854" height="430" alt="Screenshot 2025-10-25 at 8 43 42 PM" src="https://github.com/user-attachments/assets/3f5bd393-100c-4f9f-b341-08d4ddc7e138" />

## Tools and Techniques

- ### Languages & Software:
  SQL (data engineering), Tableau (data visualization), Excel (validation and KPI testing)
- ### Key Methods:
  Market concentration analysis, data cleaning and transformation, correlation analysis, dashboard-driven insight reporting

<img width="1466" height="849" alt="Screenshot 2025-10-25 at 7 49 34 PM" src="https://github.com/user-attachments/assets/a4562851-372d-449e-89de-3ccacf029098" />

## Key Outcomes

- Quantified competition intensity using HHI (0.0074) and CR10 (19.1%), identifying a low-concentration, competitive ad market.

- Highlighted a Top-15 advertiser share of 59.8%, revealing semi-oligopolistic dynamics within the Meta Ads ecosystem.

- Developed a Tableau dashboard enabling clear visibility into volatility, efficiency, and concentration for business stakeholders.

- Provided a framework adaptable for marketing efficiency audits, pricing strategy decisions, and portfolio risk diversification.

## Business Question

How concentrated is Meta’s digital advertising ecosystem, and how does advertiser dominance influence spending efficiency, market volatility, and overall competitiveness?

## Business Impact

The analysis addresses the above question by examining the degree of concentration and its impact on efficiency within Meta’s ad market. It enables marketing and finance teams to pinpoint where budget concentration limits market agility and where diversification can reduce volatility risk. By linking market share data to spending behavior, the findings empower data-driven decisions on ad allocation, competitive benchmarking, and strategic investment planning.

## Summary
This project examines how market structure and advertiser behaviour influence competitiveness within Meta’s advertising ecosystem. Through rigorous SQL modelling, KPI design, and Tableau visualization, it quantifies market balance and efficiency to support decisions that improve revenue sustainability and portfolio diversification. The results illustrate how structured analytics can convert raw ad data into measurable business intelligence, positioning data as a strategic asset for operational and financial advantage.
