Customer Shopping Behavior — Data Analysis Project

Tools: Python · pandas · PostgreSQL · Power BI · VS Code
Dataset: 3,900 customers · 18 features · End-to-end analysis pipeline


Table of Contents

Project Overview
Dataset Description
Step 1 — Data Loading & Inspection
Step 2 — Exploratory Data Analysis
Step 3 — Data Cleaning & Transformation
Step 4 — Feature Engineering
Step 5 — Database Integration
Step 6 — SQL Analysis
Step 7 — Power BI Dashboard
Key Findings
Business Recommendations
Conclusions


1. Project Overview
This project analyses the shopping behavior of 3,900 customers to uncover patterns in revenue, product preferences, demographics, and purchasing habits. The goal is to translate raw transaction data into clear, actionable insights that can guide marketing, product, and retention strategies.
The workflow follows a complete data analysis pipeline — from loading and cleaning raw data in Python, through structured querying in PostgreSQL, to an interactive visual dashboard in Power BI.

2. Dataset Description
The original dataset customer_shopping_behavior.csv contains 3,900 rows and 18 columns covering customer demographics, transaction details, and behavioral attributes.
ColumnDescriptionCustomer IDUnique identifier for each customerAgeCustomer age in yearsGenderMale or FemaleItem PurchasedName of the product boughtCategoryProduct group — Clothing, Accessories, Footwear, OuterwearPurchase Amount (USD)Transaction valueLocationUS state of the customerSizeProduct size (S / M / L / XL)ColorProduct colourSeasonSeason of purchase — Spring, Summer, Fall, WinterReview RatingCustomer rating out of 5 stars (had 37 missing values)Subscription StatusWhether the customer has an active subscription (Yes / No)Shipping TypeDelivery method chosenDiscount AppliedWhether a discount was used (Yes / No)Promo Code UsedWhether a promo code was applied (Yes / No)Previous PurchasesNumber of past transactionsPayment MethodPayPal, Credit Card, Cash, Debit Card, Venmo, Bank TransferFrequency of PurchasesHow often the customer shops (Weekly, Monthly, Annually, etc.)

3. Step 1 — Data Loading & Inspection
The dataset was loaded into a Jupyter notebook inside VS Code using pandas. The first step was to understand the full structure of the data before making any changes.
What was checked:

Overall shape — confirmed 3,900 rows and 18 columns
Data types of each column to identify any type mismatches
Summary statistics (min, max, mean, distribution) for all numeric columns
Null value counts across every column
Unique value counts for categorical columns to spot any inconsistencies or typos

What was found:

37 missing values, all concentrated in the Review Rating column (~0.9% of rows)
No duplicate Customer IDs across the dataset
Column names had inconsistent casing and spaces, requiring standardisation before database loading
Purchase Amount (USD) ranged from $20 to $100 with a mean of $59.76
Previous Purchases ranged from 1 to 50, indicating a mix of new and experienced buyers


4. Step 2 — Exploratory Data Analysis
With the data loaded and understood, a thorough exploration of distributions, patterns, and relationships was carried out across all key variables.
Distributions explored:

Age was spread fairly evenly across the full adult range
Purchase amount was roughly uniform between $20 and $100 — no major outliers
Gender split — Male 68%, Female 32% of total customers
Category breakdown — Clothing 44.5%, Accessories 31.8%, Footwear 15.4%, Outerwear 8.3%
Season distribution — almost perfectly even across Spring, Fall, Winter, and Summer
Subscription status — only 27% of customers held an active subscription
Payment methods — all six methods used roughly equally, between 612 and 677 customers each
Purchase frequency — spread across seven categories from Weekly to Annually

Relationships examined:

Average purchase amount by subscription status — virtually identical between subscribed and non-subscribed
Average spend by shipping type — Express customers spend slightly more than Standard
Review ratings by product — tight cluster between 3.5 and 3.9 across all items
Previous purchases by subscription — subscribed customers have marginally more prior orders
Revenue contribution broken down by both gender and age group


5. Step 3 — Data Cleaning & Transformation
After the exploration phase, a series of cleaning steps were applied to prepare the dataset for SQL analysis and visualisation.
Column renaming
All 18 column names were converted to lowercase with underscores replacing spaces, and the (USD) suffix was removed from the purchase amount column. This standardisation was necessary for the data to be queried cleanly in PostgreSQL.
Handling missing values
The 37 nulls in Review Rating were filled with the column median value. Dropping these rows was considered but rejected, as removing less than 1% of the data was unnecessary and would have slightly skewed any product rating analysis.
Dropping a redundant column
Promo Code Used was removed from the dataset entirely. It was found to be almost perfectly correlated with Discount Applied, meaning it carried no independent information. Keeping it would have added noise without adding any analytical value.
Saving the cleaned file
The fully cleaned dataframe was exported as Customer_churn.csv — 3,900 rows, 19 columns (including the two engineered features), and zero null values remaining.

6. Step 4 — Feature Engineering
Two new columns were created from existing data to enable richer segmentation that the original dataset did not directly support.
Age Group
Customers were grouped into four age bands based on their age value. This allowed revenue, spend, and behavior to be compared meaningfully across life stages rather than working with individual age values.
Age RangeLabelUnder 26Young Adult26 to 45Middle-aged46 to 60AdultOver 60Senior
Purchase Frequency Days
The Frequency of Purchases column stored text labels such as "Weekly" or "Annually". These were mapped to their numeric equivalent in days — for example, Weekly became 7 and Monthly became 30. This transformed a categorical label into a usable numeric variable for comparison and visualisation.

7. Step 5 — Database Integration
The cleaned dataframe was loaded into a local PostgreSQL database using SQLAlchemy. This step was taken to move the analysis into a structured SQL environment, which more closely reflects how data analysis is performed in real business and engineering settings.
A table named customer was created inside a database called shopping_db, containing all 3,900 rows of the cleaned dataset. All ten analysis queries were then written in SQL and run directly against this table, with results displayed in the notebook for review.

8. Step 6 — SQL Analysis
Ten business questions were defined and answered using SQL queries run against the PostgreSQL database.
QueryBusiness QuestionKey ResultQ1What is total revenue split by gender?Male $157,890 (67.7%) · Female $75,191 (32.3%)Q2Which customers used discounts AND spent above average?839 customers qualify as high-value discount usersQ3Which products have the highest average review rating?Gloves 3.86 · Sandals 3.84 · Boots 3.82 · Hat 3.80 · Skirt 3.79Q4Does shipping type affect average spend?Express $60.48 · Standard $58.46Q5How do subscribed vs non-subscribed customers compare?Subscribed: 1,053 customers · Non-subscribed: 2,847Q6Which products have the highest discount application rate?Coat 47.8% · Handbag 46.2% · Boots 44.9%Q7How are customers split across loyalty segments?New 54% · Returning 31% · Loyal 15%Q8What are the top 3 products in each category by orders?Clothing: Blouse, Shirt, Dress · Footwear: Shoes, Sandals, BootsQ9How many repeat buyers (5+ purchases) are subscribed?Subscribed: 1,148 · Non-subscribed: 764Q10Which age group generates the most revenue?Middle-aged $87,576 · Adult $67,711 · Senior $43,164 · Young Adult $34,630

9. Step 7 — Power BI Dashboard
The cleaned dataset was imported into Power BI Desktop to build an interactive report allowing dynamic filtering and visual exploration of the data.
Visuals built:
VisualDescriptionKPI CardsTotal customers, total revenue, average purchase amount, subscription rateRevenue by GenderDonut chart showing Male vs Female revenue contributionSales by CategoryBar chart comparing order volume across all four product categoriesSales by SeasonColumn chart across Spring, Fall, Winter, SummerAvg Spend by Shipping TypeHorizontal bar comparing all six shipping methodsTop-Rated ProductsTable ranked by average review ratingCustomer SegmentationBreakdown of New, Returning, and Loyal customer tiersRevenue by Age GroupColumn chart across all four age bandsSubscription ImpactDonut charts showing subscription rate and repeat purchase behaviourPayment Method DistributionBar chart across all six payment methods
The dashboard supports filtering by season, category, gender, and subscription status, enabling cross-dimensional exploration of all key metrics.

10. Key Findings

Male customers dominate revenue — generating 67.7% of total sales ($157,890) despite representing 68% of customers, meaning average spend is roughly equal across genders
Very low subscription rate — only 27% of customers (1,053) hold an active subscription, leaving 2,847 customers unengaged with any retention mechanism
839 high-value discount users spend above the dataset average while actively using discounts — a segment that is already price-motivated and high-spending
Express shipping customers spend more — $60.48 on average vs $58.46 for standard, a consistent pattern pointing to a willingness to pay for speed and convenience
Sales are evenly spread across all four seasons — there is no peak shopping period, meaning revenue growth cannot rely on seasonal demand and must be campaign-driven
Clothing accounts for 44.5% of all orders — it is the dominant category by a significant margin and the most important for revenue strategy
Middle-aged customers (26–45) generate the most revenue at $87,576, nearly double that of Young Adults ($34,630) and the clear priority segment
Gloves are the highest-rated product at 3.86 stars, closely followed by Sandals and Boots — all from Accessories and Footwear categories
54% of customers have made only 1–5 purchases — the majority of the customer base is still in the earliest stage of their brand relationship
Subscribed customers have marginally more previous purchases (26.1 vs 25.1) — suggesting subscription correlates with slightly higher loyalty, but the effect is small


11. Business Recommendations
R1 — Introduce a milestone discount programme
Since 839 high-value customers already spend above average while using discounts, the data supports formalising this into a structured loyalty scheme. Offering 10–15% off triggered at purchase milestones — such as the 5th, 10th, and 20th purchase — would reward existing high-spenders and give new customers a clear reason to return.
R2 — Drive subscription sign-ups at checkout
With 73% of customers unsubscribed, the largest retention opportunity is converting casual buyers into subscribers. A 30-day free trial offered at checkout — with clear messaging on benefits such as exclusive discounts, free shipping, and early sale access — would lower the barrier to signing up and begin building a loyalty habit.
R3 — Win back new customers early
54% of customers have fewer than 5 purchases, meaning most of the base has not yet developed a pattern of returning. A targeted re-engagement email sequence at 30, 60, and 90 days after the first purchase — with personalised product recommendations based on their first order — would help convert one-time buyers into returning customers before they are lost.
R4 — Prioritise the middle-aged (26–45) segment
This age group generates 37.7% of total revenue, significantly more than any other. Premium product lines, higher price-point items, and quality-focused messaging would resonate best with this group and protect the highest-value revenue stream from competitive pressure.
R5 — Use express shipping as an upsell lever
Express shipping customers consistently spend more per transaction. Introducing a free express shipping threshold — for example, free express on orders above $75 — would encourage customers to increase their basket size to qualify, lifting average order value across the board.
R6 — Feature top-rated products in all marketing content
Gloves, Sandals, and Boots carry the highest review ratings in the dataset. Placing these products prominently in homepage banners, email campaigns, and social content builds trust through social proof and channels new traffic toward items with the strongest satisfaction track record.
R7 — Invest in the Clothing category
Clothing drives 44.5% of all orders and contains the three highest revenue-generating products — Blouse, Shirt, and Dress. Expanding the range, improving stock depth, and running Clothing-specific seasonal campaigns would amplify the contribution of the most important product group across the business.

12. Conclusions
This project delivered a complete end-to-end analysis of customer shopping behavior — from a raw, uncleaned CSV through to a structured PostgreSQL database, ten answered business questions, and an interactive Power BI dashboard.
The analysis reveals a customer base that is largely male-dominated in revenue contribution, overwhelmingly un-subscribed, and predominantly made up of first-time or early-stage buyers. Importantly, the data shows no seasonal demand pattern — meaning revenue growth must be driven by deliberate campaigns and retention investment rather than organic shopping peaks.
The most significant opportunity identified is subscription conversion. With 73% of customers holding no subscription, and subscribed customers showing modestly higher purchase history, even a modest uplift in subscription rate from 27% to 35% would meaningfully increase repeat purchase behaviour and customer lifetime value across the entire base.
The combination of a structured discount programme, a subscription onboarding campaign, and an early win-back sequence addresses the three core gaps the data reveals — low loyalty, weak subscription engagement, and high first-purchase drop-off. Together these strategies target different stages of the customer journey and form a coherent, evidence-based roadmap for improving both retention and revenue.
The methodology used here — Python for cleaning, PostgreSQL for querying, and Power BI for visualisation — is a production-grade workflow that scales directly to larger datasets and real business environments, making this project a strong foundation for further analysis or machine learning work such as purchase prediction or customer lifetime value modelling.
