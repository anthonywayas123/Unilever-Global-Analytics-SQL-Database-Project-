
# Unilever Global Analytics - End-to-End SQL Project

**Tools:** MySQL, SQL (CTEs, Window Functions, JOINs, Aggregations)
**Dataset:** Simulated Unilever global database with 11 tables, 176 products, 65 brands, 40 countries, and 5,000+ transactions through (2021-2024).

---

## Project Overview

This project simulates a real world data analyst engagement for Unilever, one of the world's largest consumer goods companies with operations across 190 countries and over 400 brands. As the assigned analyst, I was tasked by the CEO to investigate critical business performance gaps across revenue, geography, product, and customer intelligence using SQL as the primary analysis tool.

The database had lots of complexity including NULL values, mixed currencies, anonymous transactions, and multi-table relationships requiring careful JOIN logic. Every query was written from scratch to answer specific, high stakes business questions across four executive pages.

---

## The Database Architecture

The database follows a **star schema** design with 11 tables:

**Dimension Tables**
- `segments` - 5 business segments
- `categories` - 20 product categories with margin targets
- `brands` - 65 brands across Mass Market, Premium and Prestige tiers
- `products` - 176 products including 105 skin care products
- `countries` - 40 countries with GDP, currency and market maturity
- `customers` - 500 customers with loyalty tiers, gender and acquisition channel
- `retail_channels` - 11 sales channels

**Fact Tables**
- `sales_transactions` - 5,000+ transaction records (2021-2024)
- `customer_reviews` - 1,500 product reviews with NPS scores
- `inventory` - 1,800 monthly inventory snapshots
- `marketing_spend` - 233 campaign records

**Analytical Views (built from scratch)**
- `vw_sales_enriched` - fully joined master view across all dimensions
- `vw_segment_revenue` - segment level revenue aggregation
- `vw_country_performance` - country-level KPIs
- `vw_brand_performance` - brand scoreboard with ratings

---

## Dashboard Architecture - 4 Pages

| Page | Title | Audience |
|------|-------|----------|
| 1 | Executive Revenue Overview | CEO, CFO, Board |
| 2 | Geographic & Market Performance | Regional Directors |
| 3 | Product & Brand Intelligence | Brand Managers, Category Directors |
| 4 | Customer Intelligence | CMO, CRM Team |

---

## Business Questions, Key Findings & Recommendations

---

### PAGE 1 - EXECUTIVE REVENUE OVERVIEW

---

**BQ-1 - Revenue by Segment & Year Trend**

I analyzed each business segment performance and GR genration from the previous years down to the last year and accordintg to data, discovered Beauty & Personal Care (BPC) dominates with $332,916 in gross revenue thereby accounting for approximately 78% of the company's total revenue. Foods & Refreshment at ($27,096), Home Care also ($31,579), Ice Cream ($22,065) and Nutrition ($10,930) trail significantly. The year trend reveals BPC peaked in 2021 at $87,741 and has shown slight decline through 2024 at $83,777, this a warning signal for the company's largest revenue driver.

*SQL Technique Used: Multi-CTE with window function `SUM() OVER (PARTITION BY year)` for revenue share percentage*

**Recommendation :** Investigate the gradual BPC revenue decline from 2021 to 2024. Commission a root cause analysis and try to find out properly if this slight drop is actually driven by increased competition, pricing pressure or distribution gaps? Protect BPC at all cost as any significant drop directly impacts total company performance.

---

**BQ-2 - Year-over-Year Revenue Growth**

Emerging markets showed consistent YoY growth throughout 2021-2024. Egypt recorded 90.36% YoY growth in 2024, the highest of any market. Bangladesh grew 45.35% in 2023. Overall company growth is being driven entirely by Emerging market expansion while Developed markets show flat or declining contribution.

*SQL Technique Used: CTE with `LAG()` window function partitioned by country, ordered by year*

**Recommendation :** Set aggressive growth targets specifically for Egypt, Philippines and Kenya based on their demonstrated momentum. These markets are proving the Emerging market strategy is working, also double down with dedicated marketing and distribution investment in 2025 or coming years.

---

**BQ-3 - Gross Margin vs Target**

The Premium tier brand (Simple) achieved the highest actual gross margin at 43.69% thereby exceeding its category target. Prestige brands Dermalogica (39.59%) and Paula's Choice (38.20%) are performing below their margin targets despite premium pricing. Mass Market brands average 39-41% and largely meet targets. Noxzema (31.09%) is the weakest margin performer.

*SQL Technique Used: `AVG(gross_margin_pct)` vs `MAX(margin_target)` with CASE classification per category*

**Recommendation :** Conduct an urgent cost-of-goods (cogs) review for Prestige brands. Their high formulation and packaging costs are eroding the margin advantage their premium prices should deliver. Either reduce COGS or increase prices to restore the margin premium that justifies Prestige positioning.

---

**BQ-4 - Revenue & Profitability by Channel**

Even though Pharmacy leads in net revenue generation, that doesn't necessarily mean its our most profitable channel, Supermarket on the other hand ranks 4th in revenue genration but has the highest profit margin at 39.86%. The gap between Pharmacy's revenue dominance and Supermarket's margin efficiency reveals an optimization opportunity. Online channels and (Direct to Consumer) show healthy margins with growing transaction volumes.

*SQL Technique Used: Simple aggregation with `ROUND(SUM(gross_profit_usd) / SUM(net_revenue_usd) * 100, 2)`*

**Recommendation :** Develope a Supermarket volume growth strategy. It is the most margin efficient channel even though it currently ranks 4th in revenue, if Supermarket volume can be grown while maintaining its 39.86% margin, it would potentially become the single most valuable channel in the portfolio.

---

**BQ-5 - Returns & Cancellations Financial Impact**

Total financial impact from returns and cancellations across all segments is $33,667. Beauty & Personal Care accounts for $27,632 of that loss and 82% of total. Nutrition has the highest cancellation rate at 4.27% despite being the smallest segment. Beauty & Personal Care's return rate is 5.24% while Ice Cream has the lowest return rate at 2.55%.

*SQL Technique Used: `SUM(CASE WHEN order_status = 'Returned' THEN 1 ELSE 0 END)` with `SUM(SUM()) OVER()` for grand total*

**Recommendation :** Launch a targeted returns reduction program for Beauty & Personal Care, even a 1% reduction in return rate translates to thousands in recovered revenue. Investigate Nutrition's high cancellation rate specifically; at 4.27% it suggests either product-market fit issues or fulfilment problems in that category.

---

### PAGE 2 - GEOGRAPHIC & MARKET PERFORMANCE

---

**BQ-6 - Top Revenue Markets & Fastest Growing Emerging Markets**

Ghana leads all 40 markets in net revenue at $22,086, followed by Tanzania ($20,866), Kenya ($20,540) and Nigeria ($20,247). The United States ranks 8th at $16,985 below seven Emerging and Developing markets. Egypt is the fastest growing market at 90.36% YoY in 2024. Philippines grew 47.06% and Bangladesh 45.35%.

*SQL Technique Used: i used a two-query approach - simple aggregation for Part 1 and CTE with `LAG()` for YoY growth in Part 2*

**Recommendation :** Prioritize Egypt, Philippines and Bangladesh for 2025 and beyond investment. Their growth trajectories are exceptional and represent the clearest market expansion opportunities in the portfolio, also establish local distribution partnerships and targeted digital campaigns in these three markets before competitors capture the growth.

---

**BQ-7 - Africa Deep Dive**

Across Nigeria, Ghana, Kenya, Ethiopia and Tanzania, Skin Care dominates as the top revenue category. Kenya's top category is Hair Care and it's the only African market where Hair Care outperforms Skin Care. Dishwashing products appear as top performers in Nigeria and Ethiopia, suggesting Home Care has stronger penetration in those markets than expected.

*SQL Technique Used: `IN()` filter with multi column GROUP BY, ordered by country and revenue*

**Recommendation :** Develop Africa specific product bundles combining Skin Care and Home Care products. The data shows African consumers are purchasing across both categories, also a bundled promotional strategy could increase basket size significantly across all five markets.

---

**BQ-8 - Market Maturity Revenue Mix**

Emerging markets hold 47-49% of total revenue and have grown consistently every single year from 2021 to 2024. Developing markets are volatile, peaked in 2022 then declined. Developed markets are losing revenue share annually despite generating high absolute revenue.

*SQL Technique Used: `SUM(SUM()) OVER (PARTITION BY year)` for percentage share with year-over-year comparison*

**Recommendation :** Formally shift the investment allocation model. Emerging markets are the only segment showing consistent, unbroken YoY share growth. Reallocate a portion of Developed market marketing budget toward Emerging market expansion - the return on investment trajectory clearly favors this rebalancing.

---

**BQ-9 - Product-Market Fit & Untapped Opportunities**

Dermalogica dominates as the top revenue product in almost every country but this is mostly price driven and not volume driven. REN Clean Skincare generates under $34 in Ethiopia, Morocco and Bangladesh. Onnit Alpha Brain Capsules underperforms even in its home market United States . Paula's Choice OMEGA+ Complex Serum generates under $40 in Malaysia and Mexico.

*SQL Technique Used: `ROW_NUMBER() OVER (PARTITION BY country ORDER BY SUM() DESC)` for top product per country; LEFT JOIN with revenue threshold filter for gap analysis*

**Recommendation :** Unilever has global reach but not global penetration. Launch targeted activation campaigns for REN in Morocco and Bangladesh, and a full diagnostic review of Onnit's US performance - a brand acquired in 2021 underperforming in its home market is a serious investment concern requiring immediate attention.

---

**BQ-10 - Average Transaction Value by Market**

Average transaction values range narrowly from $79 to $107 across all 40 markets with no consistent pattern by market maturity. New Zealand leads at $107, Egypt second at $103, beating France, Canada and the United States. Emerging and Developing markets perform comparably to Developed ones.

*SQL Technique Used: `ROUND(AVG(gross_revenue_usd), 0)` grouped by country and market maturity*

**Recommendation :** Abandon the assumption that Emerging market customers are inherently price-sensitive. Egypt at $103 average transaction value proves urban Emerging market consumers will pay premium prices. Developee premium product positioning specifically for urban consumer segments in Egypt, Bangladesh and Vietnam.

---

### PAGE 3 - PRODUCT & BRAND INTELLIGENCE

---

**BQ-11 - Top Revenue Brands & Tier Margin Profile**

Dove leads at $51,936 gross revenue, 12.23% of total brand revenue. Dermalogica ($41,674) and Paula's Choice ($32,241) rank 2nd and 3rd despite being Prestige brands with lower transaction volumes. Premium tier (Simple at 43.69%) has the highest margin. Prestige brands average 37-39% margin - lower than Mass Market brands at 39-41%.

*SQL Technique Used: `SUM(SUM()) OVER()` for revenue share percentage alongside `AVG(gross_margin_pct)` per brand*

**Recommendation :** Dove's revenue concentration at 12.23% of total brand revenue is a portfolio risk. If Dove loses market share to competitors, the impact on total company revenue is immediate and severe. therefore a brand diversification strategy is needed to reduce dependency on any single brand below 8% of total revenue within 3 years.

---

**BQ-12 - Skin Care Category Deep Dive**

Across 105 skin care products, Serums and Body Lotions generate the highest revenue within the category. Prestige product types Anti-Aging Creams, Vitamin C Serums and Retinol Treatments all generate strong revenue per unit but lower total volume. Bar Soaps and Body Wash generate consistent high volume revenue driven by repeat purchase behavior.

*SQL Technique Used: Category filter `WHERE category_name = 'Skin Care'` with product type aggregation*

**Recommendation :** Invest in expanding the Serum sub category specifically. It has the highest revenue per unit across skin care and growing global consumer demand. Develop 3-5 new serum SKUs under existing trusted brands like Dove and Vaseline to capture mass market serum demand at accessible price points.

---

**BQ-13 - Prestige vs Mass Market Strategy**

Prestige brands generate top 3 revenue rankings but their gross margins (37-39%) trail Mass Market brands (39-41%) and significantly trail Premium tier Simple (43.69%). The premium price charged by Prestige products is being consumed by high formulation, packaging and marketing costs.

*Answered through BQ-11 data - no separate query required*

**Recommendation :** Reposition Prestige brand pricing upward by 8-12% to restore margin parity with Mass Market. The Prestige brand customer is demonstrably willing to pay, Dermalogica and Paula's Choice generating strong revenue proves demand exists. The issue is cost structure, not revenue potential.

---

**BQ-14 - Customer Rating vs Sales Performance**

32 products generating significant revenue have average ratings below 4 stars. Liquid I.V. Hydration Multiplier is the most critical, highest revenue among underperformers at $30,037 with only a 3-star rating. Vaseline Original Pure Petroleum Jelly, a heritage product rates only 2 stars. Dove Sensitive 48h Antiperspirant rates 3 stars, a brand reputation risk.

*SQL Technique Used: Multi table JOIN `sales_transactions + products + customer_reviews` on `product_id` with `HAVING AVG(rating) < 4`*

**Recommendation :** Immediately initiate product improvement reviews for the top 10 high-revenue low-rating products. Liquid I.V. at $30,037 revenue with 3-star satisfaction is a churn risk, customers are buying out of habit or necessity but are not satisfied. One viral negative review campaign could collapse that revenue overnight.

---

**BQ-15 - Sustainability Certification Premium**

FSC certified products command the highest average unit price at $16, the only certification delivering a clear price premium. Vegan Certified ($12) and Cruelty Free ($11) certifications show no meaningful price premium over uncertified products ($13). Rainforest Alliance certified products have the second highest transaction volume at 812 units.

*SQL Technique: `CASE WHEN sustainability_cert IS NULL THEN 'No Certification' ELSE sustainability_cert END` with comparative aggregation*

**Recommendation :** Prioritize FSC certification expansion as the only certification with proven commercial return. Evaluate whether Vegan and Cruelty Free certification investment is delivering sufficient brand equity to justify the cost, or whether resources should be redirected toward FSC and Rainforest Alliance certifications that show measurable impact.

---

**BQ-16 - Stockout Revenue Loss**

Estimated revenue lost from stockouts across 59 affected products totals significant value. Dermalogica Skin Smoothing Cream SPF 15 lost an estimated $33,814 from only 19 stockout days, because its $54.99 price means every day out of stock is catastrophically expensive. Dove Men+Care Clean Comfort Body Wash had the most stockout days at 53, losing an estimated $10,437.

*SQL Technique Used: `SUM(COALESCE(stockout_days, 0))` with revenue loss formula `(stockout_days/30) * AVG(monthly_units) * base_price`*

**Recommendation :** Implement a tiered inventory alert system that weights stockout urgency by unit price, not just volume. Prestige products like Dermalogica losing $33K from 19 days proves that high-price products need tighter safety stock levels than Mass Market products. Every day of Dermalogica stockout costs 9x more than a day of Lifebuoy stockout.

---

### PAGE 4 - CUSTOMER INTELLIGENCE

---

**BQ-17 - Customer Segmentation by Type & Loyalty**

Individual Consumer Bronze is the single largest revenue group at $74,967 with 849 transactions. Retail Chain Gold has the highest average order value at $101.36. Platinum customers across all types represent a very small transaction volume, Individual Consumer Platinum has only 116 transactions. Average order values range from $83-$101 with no dramatic tier based premium.

*SQL Technique Used: `COALESCE()` for NULL handling with dual GROUP BY across customer type and loyalty tier*

**Recommendation :** The loyalty program is not effectively driving premium spending behavior - Platinum customers do not consistently outspend Gold or even Silver customers per transaction. Redesign the loyalty program to include spend-based incentives that reward higher basket sizes, not just frequency. The goal should be increasing average order value per tier, not just transaction count.

---

**BQ-18 - Loyalty Tier Revenue Concentration (Pareto Analysis)**

Bronze tier generates 37.65% of total revenue, the largest share of any tier. Silver follows at 25.74%, Gold at 13.24% and Platinum at only 8.42%. In this case, This is actually a reverse Pareto as the lowest loyalty tier drives the most revenue and driven purely by volume. This means the loyalty program is not creating the expected concentration at premium tiers.

*SQL Technique Used: `SUM(SUM()) OVER()` window function for revenue share across loyalty tiers*

**Recommendation :** The absence of a Pareto distribution in loyalty tiers is a strategic failure of the loyalty program. Introduce exclusive Platinum tier benefits, early product access, dedicated customer service, premium bundle pricing that incentivize Bronze and Silver customers to upgrade tiers. The goal is moving revenue concentration upward toward Gold and Platinum.

---

**BQ-19 - Acquisition Channel Lifetime Value**

Retail Store acquisition produces the highest customer lifetime value at $8,985,751 with 607 orders. Online Ad follows at $7,405,415. Distributor and Trade Show perform comparably at $7.3M and $6.7M respectively. Word of Mouth produces the lowest LTV at $4,497,648 - organic referrals attract lower value customers.

*SQL Technique Used: `customers LEFT JOIN sales_transactions` with `SUM(lifetime_value_usd)` grouped by acquisition channel*

**Recommendation :** Protect and grow Retail Store presence as the highest LTV acquisition channel. In-store customer acquisition costs may be higher than digital but the lifetime value return justifies the investment. Avoid over rotating toward digital only acquisition strategies, the data shows in store acquired customers are worth nearly double Word of Mouth acquired customers.

---

**BQ-20 - Payment Method Trends**

Across 2021-2024, no single payment method dominates consistently, methods rotate between Installment, Credit Card, Debit Card, Mobile Money and PayPal in the top 3 each year. Mobile Money held 14.63% share in 2021 and 14.48% in 2022, showing flat adoption globally despite its critical importance in African markets.

*SQL Technique Used: `COUNT() / SUM(COUNT()) OVER(PARTITION BY year)` for payment method share trend by year*

**Recommendation :** Mobile Money's flat global share masks its true strategic importance in Africa specifically. Run the African market filter to determine if Mobile Money is actually growing within its target geography. If African Mobile Money adoption is growing while global share appears flat due to dilution by other regions, invest in checkout optimization specifically for Mobile Money in Nigeria, Ghana, Kenya and Tanzania.

---

**BQ-21 - Net Promoter Score by Segment**

All five business segments have negative NPS scores, this means detractors outnumber promoters across the entire Unilever portfolio. Foods & Refreshment has the best NPS at -33.0. Nutrition is the worst at -46.7. Beauty & Personal Care scores -41.2 despite being the revenue leader. Ice Cream at -43.5 is concerning for a category built on emotional enjoyment.

*SQL Technique Used: `fact_customer_reviews` joined through 4 dimension tables to `dim_segments`; CASE-based SUM for Promoters/Passives/Detractors*

**Recommendation :** Negative NPS across all segments is a company-wide customer satisfaction crisis that must be treated as a boardlevel priority. A company of Unilever's scale with universally negative NPS is vulnerable to brand erosion and competitive switching. Immediately commission qualitative customer research to understand the root causes of dissatisfaction, starting with Nutrition and Ice Cream as the weakest performers.

---

## Key Standout Insights

These are the findings most likely to save or generate significant money:

**1. $33,814 lost from 19 days of one product being out of stock** - Dermalogica SPF 15. This single finding justifies an immediate investment in inventory management software for Prestige product lines.

**2. All 5 segments have negative NPS** - No segment has more promoters than detractors. For a $60 billion company this represents billions in potential revenue at risk from customer churn.

**3. Onnit underperforms in its own home market** - A brand acquired by Unilever in 2021 generating only $37 average revenue in the United States is a red flag on a significant acquisition investment.

**4. Egypt beats France, Canada and the US in average transaction value** - This single data point destroys the assumption that Emerging markets are price-sensitive and should reshape Unilever's global pricing strategy.

**5. Dermalogica's revenue is price-driven not volume-driven** - It ranks top globally not because many people buy it but because each unit costs $60-$79. This is a fragile revenue base that could collapse quickly if a lower-cost competitor enters those markets.

**6. $33,667 in total returns and cancellations** - 82% concentrated in Beauty & Personal Care. A targeted returns reduction program in BPC alone could recover over $27,000 annually.

---

## SQL Techniques Demonstrated in this Analysis 

- Multi-table JOINs (up to 6 tables in a single query)
- Common Table Expressions (CTEs) - single and chained multi-CTE queries
- Window Functions - `LAG()`, `ROW_NUMBER()`, `SUM() OVER()`, `PARTITION BY`
- Conditional aggregation with `CASE WHEN` inside `SUM()`
- `COALESCE()` for NULL handling in real-world messy data
- `HAVING` clause for post-aggregation filtering
- Subquery joins for gap analysis
- Revenue share percentage calculation using double SUM window function
- Year-over-year growth calculation using LAG
- NPS score calculation from raw review data
- Stockout revenue loss estimation formula

---

## Data Quality Challenges Handled

- 15% NULL emails (privacy opt-outs) - handled with COALESCE
- 25% NULL discount values - excluded from discount analysis
- 15% anonymous transactions (NULL customer_id) - handled with COALESCE labeling
- Mixed currency data - consistently used USD columns for cross-country comparison
- Accented characters in product names - resolved at database creation level
- Cartesian join explosion risk in review-to-transaction joins - resolved by joining directly through product dimension

---

## Project Files

| File | Description |
|------|-------------|
| `unilever_database.sql` | Complete database creation script - 11 tables, all data, indexes and views |
| `page1_executive_revenue.sql` | Queries for BQ-1 through BQ-5 |
| `page2_geographic_performance.sql` | Queries for BQ-6 through BQ-10 |
| `page3_product_brand.sql` | Queries for BQ-11 through BQ-16 |
| `page4_customer_intelligence.sql` | Queries for BQ-17 through BQ-23 |

---

## How to Run

1. Open MySQL Workbench
2. Run `DROP DATABASE IF EXISTS unilever_analytics;`
3. Run the full `unilever_database.sql` script
4. Run individual page query files to reproduce all findings
5. Connect Power BI to MySQL and load the four analytical views for dashboard building

---

####  Author
##### Anthony Wayas 
Data Analyst | Power BI | Excel | SQL | Python |

This project is part of my data analytics portfolio demonstrating end-to-end SQL DBMS and Business Intelligence skills.
