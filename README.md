# Music-Streaming-Churn-Analysis
https://public.tableau.com/app/profile/fakayode.emmanuel/viz/MusicStreamingChurn/MusicStreamingChurn

## TABLE OF CONTENT
- [INTRODUCTION](#INTRODUCTION)
- [OBJECTIVE](#objective)
- [PROBLEM STATEMENT](#problem-statement)
- [GOALS](#goals)
- [SKILLS DEMONSTRATED](#skills-demonstrated)
- [DATA DICTIONARY](#data-dictionary)
- [SQL Codes](#sql-codes)
- [Python Approach](#python-approach)
- [DATA MODELLING](#data-MODELLING)
- [DATA TRANSFORMATION](#data-transformation)
- [ANALYSIS](#analysis)
- [DASHBOARD LINK](#dashboard-link)
- [RECOMMENDATIONS](#recommendations)
- [CONCLUSION](#conclusion)


## INTRODUCTION
Subscriber churn is a silent revenue killer, especially in subscription-based businesses like music streaming platforms. Research shows that retaining an existing customer is 5x cheaper than acquiring a new one. Yet, many companies focus their efforts on user acquisition while neglecting the signals of discontent and disengagement among their current subscribers.

## PROBLEM STATEMENT
Churn in music streaming services is not always a straightforward “goodbye.” Users may pause their subscriptions, downgrade to a lesser or free plan, or simply disengage without formally canceling. Traditional churn analysis often misses these nuances, leading to inaccurate metrics and missed intervention opportunities.
 
## OBJECTIVE
The objective of this project is to build a data-driven churn analysis and retention dashboard for a music streaming platform, enabling stakeholders to:

- Proactively identify churned and at-risk subscribers based on actual plan activity.

- Track subscriber behavior and plan engagement over time, segmented by month, plan type, and geography.

- Quantify the financial impact of both complete and downgrade churns to support smarter business decisions.

- Enable dynamic classification and real-time filtering, so teams can respond immediately to shifting user patterns.

This solution empowers music streaming platforms to retain more users, reduce acquisition costs and drive long-term revenue growth through data-backed decision-making.


This repository documents the end-to-end process—from data generation to analysis and dashboarding—and aims to demonstrate how churn analytics can become a powerful tool for customer retention strategy.

## GOALS
The primary goals of this project are to:

1. Detect Different Types of Churn
Accurately identify and classify churn events as:

- Complete Churn – when a user has not renewed their subscription or streamed after certain days.
- Downgrade Churn – when a user switches from a premium to less expensive/free plan.
- No Churn.

2. Track Subscriber Activity Dynamically
Build a system that allows monthly tracking of:
- Active users
= New subscribers
- Churned and at-risk users

3. Quantify Revenue Loss
Calculate lost revenue based on user exit or downgrade behavior, enabling smarter financial decisions and retention strategies.

4. Understand User Behavior Over Time
Analyze engagement timelines, subscription cycles and user retention patterns across months and years.

5. Enable Real-Time Churn Monitoring
Develop a Tableau dashboard that adapts to selected timeframes, helping stakeholders monitor churn trends and act quickly.

6. Highlight Geographic Churn Trends
Show churn distribution across U.S. states to identify regional differences and target retention efforts effectively.

7. Detect Potential Churn & support Proactive Retention Strategies
Provide business teams with actionable insights to reach out to users before they churn.

## SKILLS DEMONSTRATED
- Critical thinking
- Reasearch
- Data Generation & Simulation
- Data Engineering & Cleaning
- Advanced SQL for Churn Analytics
- Problem solving
- Dashboard Design(Figma)

## DATA DICTIONARY
This project leverages three synthetic datasets built from scratch to simulate user behavior on a music streaming platform. The data closely mimics real-world user lifecycle patterns — including joining, subscribing to plans, streaming content, and potentially churning.


**1. Main Users** - User Profile Table
Each row represents a unique user on the platform.


| Field Name | Type    | Description                   | Example           |
| ---------- | ------- | ----------------------------- | ----------------- |
| User\_ID   | STRING  | Unique ID for each user       | USER-00023        |
| Name       | STRING  | User’s full name              | Sarah Barr |
| Age        | INTEGER | Age of user                   | 29                |
| Region     | STRING  | U.S. region                   | Midwest           |
| State      | STRING  | U.S. state                    | Illinois          |
| City       | STRING  | User’s city and state         | Chicago, Illinois |
| Join\_Date | DATE    | Date user joined the platform | 2023-06-01        |

**2. Plan_history** - Subcription Plan History Table
Each row represents one biling cycle for a user
| Field Name        | Type    | Description                                          | Example            |
| ----------------- | ------- | ---------------------------------------------------- | ------------------ |
| User\_ID          | STRING  | Foreign key to main user table                       | USER-00023         |
| Plan              | STRING  | Plan type subscribed to                              | Individual Premium |
| Start\_Date       | DATE    | Start of plan                                        | 2024-03-01         |
| End\_Date         | DATE    | End of plan                                          | 2024-04-01         |
| Price             | FLOAT   | Price of the plan                                    | 9.99               |
| New?              | INTEGER | 1 if user’s first plan                               | 1                  |
| Downgrade churn?  | BOOLEAN | TRUE if user downgraded to a cheaper plan next cycle | TRUE               |
| Duration to Renew | INTEGER | Days between this plan’s end and next plan’s start   | 34                 |
| Churn\_Status     | STRING  | active, at-risk, or churned                          | churned            |
| Churn\_Type       | STRING  | Complete Churn, Downgrade Churn, or No Churn         | Downgrade Churn    |
| Churn\_Reason     | STRING  | Reason if churned                                    | Too expensive      |
| Rating            | INTEGER | User satisfaction rating (1–5)                       | 2                  |
| Lost\_Revenue     | FLOAT   | Revenue lost if churned or downgraded                | 5.00               |

**3. Streming Log** - User Streaming Activity Table
| Field Name                | Type    | Description                    | Example    |
| ------------------------- | ------- | ------------------------------ | ---------- |
| Stream\_Date              | DATE    | Date user streamed content     | 2024-02-12 |
| User\_ID                  | STRING  | Foreign key to main user table | USER-00023 |
| Stream\_Duration\_Minutes | INTEGER | Duration of session in minutes | 78         |


## SQL CODES
The dataset used in this churn analysis project was synthetically generated using ChatGPT to simulate real-world behavior of subscribers on a music streaming platform.
Several new insights-driving columns such as churn classification, churn type, and lost revenue were calculated using SQL and Python to ensure consistency, automation, and analytical depth.
I initially used Python to calculate these columns, but later transitioned to SQL for better scalability. This allows Tableau to connect live to the database, enabling real-time automation and smoother integration.

**Sample of the Raw Data**
| User\_ID   | Plan                | Start\_Date | End\_Date  | Price | Rating | Churn\_Reason  |
| ---------- | ------------------- | ----------- | ---------- | ----- | ------ | -------------- |
| USER-00001 | Family Plan         | 4/19/2022   | 6/4/2022   | 14.99 | 5      |                |
| USER-00001 | Free (Ad-Supported) | 8/13/2022   | 9/13/2022  | 0.00  | 2      | Billing issues |
| USER-00002 | Annual Premium      | 6/21/2020   | 6/21/2021  | 99.99 | 4      |                |
| USER-00002 | Family Plan         | 9/6/2021    | 10/31/2021 | 14.99 | 5      |                |
| USER-00003 | Individual Premium  | 2/18/2021   | 4/15/2021  | 9.99  | 4      |                |

-- Using window functions to calculate next plan start
-- Using window functions to calculate next plan start
WITH plan_ranks AS (
  SELECT *,
         LEAD(Start_Date) OVER (PARTITION BY User_ID ORDER BY Start_Date) AS Next_Start_Date,
         LEAD(Price) OVER (PARTITION BY User_ID ORDER BY Start_Date) AS Next_Price,
         ROW_NUMBER() OVER (PARTITION BY User_ID ORDER BY Start_Date) AS rn,
         COUNT(*) OVER (PARTITION BY User_ID) AS user_plan_count
  FROM [dbo].[plan_history raw data]
),

classified AS (
  SELECT *,
         CASE 
           WHEN Next_Start_Date IS NULL THEN DATEDIFF(DAY, End_Date, GETDATE())
           ELSE DATEDIFF(DAY, End_Date, Next_Start_Date)
         END AS Duration_to_Renew,

         CASE 
           WHEN End_Date IS NULL THEN 'Active'
           WHEN DATEDIFF(DAY, End_Date, Next_Start_Date) BETWEEN 0 AND 20 THEN 'Active'
           WHEN DATEDIFF(DAY, End_Date, Next_Start_Date) BETWEEN 21 AND 30 THEN 'At Risk'
           WHEN DATEDIFF(DAY, End_Date, Next_Start_Date) > 30 THEN 'Churned'
           ELSE 'Active'
         END AS Churn_Status
  FROM plan_ranks
)

SELECT 
  User_ID,
  [Plan],
  Start_Date,
  End_Date,
  Price,
  Rating,
  ISNULL(Churn_Reason, '') AS Churn_Reason,

  -- Churn Type only if churned
  ISNULL(
    CASE 
      WHEN Churn_Status = 'Churned' AND Churn_Reason = 'Billing issues' THEN 'Involuntary Churn'
      WHEN Churn_Status = 'Churned' AND Price > ISNULL(Next_Price, 0) THEN 'Downgrade Churn'
      WHEN Churn_Status = 'Churned' AND Next_Start_Date IS NULL THEN 'Complete Churn'
      WHEN Churn_Status = 'Churned' THEN 'Voluntary Churn'
      ELSE NULL
    END,
    'No Churn'
  ) AS Churn_Type,

  CASE WHEN rn = 1 THEN 1 ELSE 0 END AS [New?],

  CASE 
    WHEN Price > ISNULL(Next_Price, 0) THEN 1 
    ELSE 0 
  END AS [Downgrade churn?],

  Duration_to_Renew,
  Churn_Status,

  CASE 
    WHEN Churn_Status = 'Churned' AND Churn_Reason = 'Billing issues' THEN Price
    WHEN Churn_Status = 'Churned' AND Next_Start_Date IS NULL THEN Price
    WHEN Churn_Status = 'Churned' AND Price > ISNULL(Next_Price, 0) THEN Price - Next_Price
    ELSE 0
  END AS Lost_Revenue

FROM classified;

## PYHON APPROACH

import pandas as pd
import numpy as np

# Load your data
df = pd.read_csv("plan_history_raw_data.csv")  # Replace with your actual data path

# Ensure date columns are in datetime format
df['Start_Date'] = pd.to_datetime(df['Start_Date'])
df['End_Date'] = pd.to_datetime(df['End_Date'])

# Sort by user and start date
df = df.sort_values(['User_ID', 'Start_Date'])

# Compute lead columns (next plan's start date and price)
df['Next_Start_Date'] = df.groupby('User_ID')['Start_Date'].shift(-1)
df['Next_Price'] = df.groupby('User_ID')['Price'].shift(-1)

# Row number per user
df['rn'] = df.groupby('User_ID').cumcount() + 1

# Duration to renew
df['Duration_to_Renew'] = (df['Next_Start_Date'] - df['End_Date']).dt.days
df['Duration_to_Renew'] = df['Duration_to_Renew'].fillna((pd.Timestamp.today() - df['End_Date']).dt.days)

# Churn Status
def classify_status(end, duration):
    if pd.isna(end):
        return 'Active'
    elif 0 <= duration <= 20:
        return 'Active'
    elif 21 <= duration <= 30:
        return 'At Risk'
    elif duration > 30:
        return 'Churned'
    else:
        return 'Active'

df['Churn_Status'] = df.apply(lambda row: classify_status(row['End_Date'], row['Duration_to_Renew']), axis=1)

# Churn Reason fix (null becomes "")
df['Churn_Reason'] = df['Churn_Reason'].fillna('')

# Churn Type
def get_churn_type(row):
    if row['Churn_Status'] != 'Churned':
        return 'No Churn'
    if row['Churn_Reason'] == 'Billing issues':
        return 'Involuntary Churn'
    elif pd.isna(row['Next_Start_Date']):
        return 'Complete Churn'
    elif row['Price'] > row['Next_Price']:
        return 'Downgrade Churn'
    else:
        return 'Voluntary Churn'

df['Churn_Type'] = df.apply(get_churn_type, axis=1)

# New?
df['New?'] = df['rn'].apply(lambda x: 1 if x == 1 else 0)

# Downgrade churn?
df['Downgrade churn?'] = (df['Price'] > df['Next_Price']).astype(int)

# Lost Revenue
def get_lost_revenue(row):
    if row['Churn_Status'] != 'Churned':
        return 0
    if row['Churn_Reason'] == 'Billing issues':
        return row['Price']
    elif pd.isna(row['Next_Start_Date']):
        return row['Price']
    elif row['Price'] > row['Next_Price']:
        return row['Price'] - row['Next_Price']
    else:
        return 0

df['Lost_Revenue'] = df.apply(get_lost_revenue, axis=1)

# Final selected columns
final_df = df[[
    'User_ID', 'Plan', 'Start_Date', 'End_Date', 'Price', 'Rating',
    'Churn_Reason', 'Churn_Type', 'New?', 'Downgrade churn?', 
    'Duration_to_Renew', 'Churn_Status', 'Lost_Revenue'
]]

# Export if needed
# final_df.to_csv("processed_plan_history.csv", index=False)

## DATA MODELLING
The main users table was related to the plan history and streaming log table using the User ID column


<img width="722" height="239" alt="image" src="https://github.com/user-attachments/assets/7f343f25-6704-44b9-8d6f-297e905c119c" />

## DATA TRANSFROMATION
I grouped some columns after connecting the data in Tableau
-  Voluntary and involuntary churns as "Complete Churn" of the churn_type column in tableau
-  Billing issues and App issues as App/Billing Issues of the churn_reason column

## ANALYSIS
 **Excecutive Summary**

1. Churn Rate
- January 2024 churn rate was 30.2%, which is 11.3% lower than December 2023 — a strong start to the year.

- However, churn peaked in June 2024 at 38%, indicating possible mid-year dissatisfaction or external competition.

- Notably, only 5 out of 12 months in 2024 recorded a churn rate higher than the same month in 2023, suggesting an overall year-on-year improvement in user retention.

2. Lost Revenue
Lost revenue in January 2024 was $24,011, representing a 15.2% increase from December 2023 — signaling that churned users in early 2024 were on higher-priced plans or more frequent.

June 2024 recorded the highest lost revenue of the year, aligning with its churn rate peak (38%), suggesting both high volume and value of lost users during that month.

Notably, June 2024 was the only month where lost revenue exceeded the same month in 2023, reinforcing a critical spike in customer value loss that demands further investigation.

The trend implies that while churn rates improved YoY in several months, the financial impact of churn was still significant, particularly when high-value users exited.

3. Active Users
- January 2024 saw 235 active users, matching December 2023 and surpassing January 2023, indicating stable user engagement entering the year.

- August 2024 recorded the peak with 268 active users, suggesting successful campaigns or high seasonal engagement.

- However, July 2024 had the lowest active users, and along with November, these were the only months in 2024 where active user counts fell below their 2023 equivalents.

