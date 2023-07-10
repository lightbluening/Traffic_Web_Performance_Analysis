# Traffic_Web_Performance_Analysis
Use MySQL to analyze Maven Fuzzy Factory' website performance, conversion rates, and bounce rates,etc.
# Background
Maven Fuzzy Factory, founded in 2013, is a small online business specializing in the sale of Mr. Fuzzy-themed toys. Their product range includes popular items such as the Original Mr. Fuzzy and the Birthday Sugar Panda. Operating exclusively online, the company does not have a physical store. To gain market share and enhance brand recognition, Maven Fuzzy Factory has implemented advertising campaigns through various search channels, including gsearch and bsearch. Over the course of three years(2013-2015), the company has amassed a comprehensive database comprising information on their products, orders, website activities, and more. As a data analyst, our objective is to utilize MySQL to gain insights into customer behavior and interaction with the website. This involves analyzing website performance, conversion rates, and investigating potential causes of high bounce rates,etc. Our analysis aims to address the following questions:

1.	What is the primary source of website traffic?
2.	How do different traffic channels perform in terms of conversion rates?
3.	What are the top entry pages?
4.	What are the bounce rates associated with different entry pages?
5.	What are the overall paid traffic bounce rate trend monthly of different entry page?
6.	What are the most viewed and least viewed pages on the website? List the ranking of them.
7.	Can the paid traffic conversion funnels be established to evaluate the conversion rates of the top 2 entry pages?
8.	What is the distribution of traffic across various device types for each traffic source?
9.	What percentage of sessions originate from organic search, direct type-ins, and paid searches on a yearly basis?

![image](https://github.com/lightbluening/Traffic_Web_Performance_Analysis/assets/93415125/68e0dec0-fe6f-40a1-b1f7-e14683371e82)

The database comprises 6 interrelated tables that contain comprehensive Ecommerce data, including information on website activity, products, orders, refunds, and more. For this particular project, our focus will be on querying data from two interconnected tables: "website_sessions" , “orders” and "website_pageviews." These tables provide valuable insights into website sessions and pageviews, enabling us to analyze user behavior and website performance effectively. 
The website_sessions table contains 9 columns: 
"website_session_id": This column contains a unique identifier for each website session. It serves as a reference number or label for individual sessions.
"created_at": The "created_at" column records the timestamp indicating when each website session was created. This information helps track the timing and frequency of sessions.
"user_id": Each customer or user is assigned a unique identifier in the "user_id" column. This allows for tracking and distinguishing different users within the dataset.
"is_repeat_session": This binary variable indicates whether a session is a repeat session or not. It provides insight into user behavior and helps identify returning visitors.
"utm_source", "utm_campaign", "utm_content": These three columns, namely "utm_source", "utm_campaign", and "utm_content," correspond to the urchin tracking module parameters used to measure marketing paid activities. They help identify the sources and campaigns that drove traffic to the website.
"device_type": The "device_type" column categorizes the type of device used by the user during the website session. It has two possible values: "desktop" or "mobile."
"http_referer": The "http_referer" column indicates the source or origin of the website traffic. It provides information on the website or page from which the user arrived at the current website session.
![image](https://github.com/lightbluening/Traffic_Web_Performance_Analysis/assets/93415125/7291b2f9-0185-4331-934d-c80d66ce8114)
The ‘website_pageviews’ table contains 4 columns:
"website_pageview_id": This column contains a unique identifier for each page that the customer viewed. It serves as a distinct reference number or label for individual page views.
"created_at": The "created_at" column records the timestamp indicating when each page view occurred. This information helps track the timing and sequence of page views within a website session.
"website_session_id": Similar to the corresponding column in the "website_sessions" table, this column contains a unique identifier for each website session. It serves as a reference number or label that links the page views to their respective website sessions.
"pageview_url": The "pageview_url" column contains the specific URL or webpage that the customer viewed during their website session. This information allows us to understand which pages were accessed and analyze user navigation patterns.
![image](https://github.com/lightbluening/Traffic_Web_Performance_Analysis/assets/93415125/e7de0e83-291f-466b-ab72-49549df42f51)
The "orders" table comprises nine columns:
"order_id": This column contains a unique identifier for each order.
"created_at": This column indicates the timestamp when the order was placed.
"website_session_id": This column contains a unique identifier for each website session, same to the "website_session_id" in the previous two tables.
"user_id": This column corresponds to the "user_id" in the "website_sessions" table. Each customer or user is assigned a unique identifier in the "user_id" column.
"primary_product_id": This binary variable indicates whether the product is the primary product in an order or not. It is used for cross-sell analysis.
"items_purchased": This column indicates the items that were purchased in an order.
"price_usd": This column denotes the price of the order in US dollars.
"cogs_usd": This column represents the cost of goods sold in US dollars.
![image](https://github.com/lightbluening/Traffic_Web_Performance_Analysis/assets/93415125/9b30c22f-5722-4c09-aad7-9a0a3aa1b231)

# Data Analysis 

For my analysis, I have divided it into 4 parts:
1. Data Preparation: This phase involves preparing and organizing the dataset for analysis. 
2. Exploratory Data Analysis: In this part of the analysis, I will focus on the following specific areas: 
Website Performance Analysis: This analysis will assess various aspects of website performance, such as traffic source, bounce rate and conversion rates. It aims to identify areas of improvement and suggest strategies to enhance overall website performance.
Conversion Funnel Analysis: This analysis will examine the conversion funnel, tracking the user journey from initial website entry to completing a desired action (e.g., placing an order). It will identify bottlenecks or drop-off points within the funnel and propose strategies to optimize conversions and improve the overall user experience.
Channel Portfolio Analysis: This analysis will evaluate the effectiveness and performance of different marketing channels in driving website traffic and conversions. 
By conducting these analyses and providing suggestions in Part 2, we aim to gain valuable insights into website performance, optimize conversion funnels, and improve the management of various marketing channels. 
3. Findings: Based on the exploratory analysis, this section will present the key findings and insights obtained from the data. It will highlight significant observations or discoveries that are relevant to the project's objectives.
4. Suggestions: In this part, I will provide actionable suggestions or recommendations based on the findings. These suggestions aim to improve website performance, optimize conversion funnels, and enhance channel portfolio management.

Additionally, SQL codes and corresponding results for Part 1 and Part 2 will be presented in the screenshots.
