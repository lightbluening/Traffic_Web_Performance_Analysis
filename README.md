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

# Part 1 Data Preparation

In this part, besides data type, we aim to get an overview of the data by finding out its number of data points, missing values, duplicates, and unique values of categorical data. By conducting these assessments, we can gain a comprehensive overview of the dataset, including its size, data quality, and the uniqueness of categorical values. This information will form the foundation for further analysis and assist in making informed decisions throughout the project.

## Step 1 Check the total number of website sessions
SQL Codes and Results
![image](https://github.com/lightbluening/Traffic_Web_Performance_Analysis/assets/93415125/d17152b5-adda-4a28-a4e1-04383de55481)
We can see that in the "website_sessions" table, there are a total of 472,871 data points or records.

## Step 2 Check missing values for each column
SQL Codes and Result
![image](https://github.com/lightbluening/Traffic_Web_Performance_Analysis/assets/93415125/18097bc9-1a09-47e9-ab30-f6bc5d5865ee)
Based on the results displayed above, we can observe missing values in the 'utm_source', 'utm_campaign', 'utm_content', and 'http_referer' columns. This is expected and reasonable since we have only tagged the most frequently used search engine and source in the dataset.

## Step 3 Check the duplicates.
SQL Codes and Results
![image](https://github.com/lightbluening/Traffic_Web_Performance_Analysis/assets/93415125/5ffdc21b-f0ab-44c2-9325-f0bd6b73d4c7)

There are 0 duplicates in our dataset.

## Step 4 Check the time range of the dataset
SQL Codes and Results

![image](https://github.com/lightbluening/Traffic_Web_Performance_Analysis/assets/93415125/a21f7e7a-ab16-4fbe-9b9b-542cd04ca074)
In our dataset, we can observe that the earliest recorded website session took place on March 19, 2012, while the most recent session occurred on March 19, 2015. Therefore, the dataset spans a period of three years. However, it is important to note that due to the mentioned reason, the data for the first three months of 2012 and the third month of 2015 are incomplete.

## Step 5 Check the unique values for the categorical columns
SQL Codes and Results

![image](https://github.com/lightbluening/Traffic_Web_Performance_Analysis/assets/93415125/8dbcbbce-c1e0-4ba1-9e40-9c935eb24732)

Based on the results above, we can observe the following unique values in each column:
'utm_source' column: There are 3 unique values: 'gsearch', 'bsearch', and 'socialbook'.
'utm_campaign' column: There are 4 unique values: 'nonbrand', 'brand', 'pilot', and 'desktop_targeted'.
'utm_content' column: There are 6 unique values: 'g_ad_1', 'g_ad_2', 'b_ad_1', 'b_ad_2', 'social_ad_1', and 'social_ad_2'.
'http_referer' column: There are 3 unique values: 'https://www.gsearch.com', 'https://www.bsearch.com', and 'https://www.socialbook.com'.

# Part 2 Exploratory Data Analysis

# 2.1 Traffic Source Analysis

The practice of assessing and comprehending the various channels or sources that bring traffic to a website is known as traffic source analysis. It entails examining and classifying the sources or referrers of website visitors in order to learn whether marketing initiatives or platforms are significantly increasing traffic.
Identification and evaluation of the efficiency of various marketing channels and campaigns in generating website traffic are the objectives of traffic source analysis. It assists companies in making data-driven decisions about their marketing plans, financial planning, and audience targeting.
In this part, we aim to answer the following 2 questions.

● What is the primary source of website traffic?

● How do different traffic channels perform in terms of conversion rates?


## 1.	What is the primary source of website traffic?
SQL Codes and Results
![image](https://github.com/lightbluening/Traffic_Web_Performance_Analysis/assets/93415125/9057298c-3a1a-4fbc-83a2-1e6cf74059ad)

The top three sources of primary traffic are Gsearch Nonbrand, Bsearch Nonbrand, and direct type-in. Based on the results, it is clear that the Gsearch Nonbrand category has the highest number of website sessions, accounting for 59.79% of the total sessions. The Bsearch Nonbrand category ranks second in terms of both session count and percentage of the total sessions, representing 11.61%. Direct type-in ranks third, occupying 8.44% of the total sessions

## 2.	How do different traffic channels perform in terms of conversion rates?
SQL Codes and Results
![image](https://github.com/lightbluening/Traffic_Web_Performance_Analysis/assets/93415125/4d544c71-9afb-45b7-9fe3-0ae66427fe42)

Based on the results mentioned above, it is apparent that there are five channels with conversion rates exceeding 7%. These channels include organic search, Bsearch Brand, Gsearch Brand, and direct type-in. However, despite Gsearch Nonbrand and Bsearch Nonbrand being the primary sources of traffic, their conversion rates are approximately 6.5%. Unfortunately, the socialbook channels perform poorly in both the desktop-targeted campaign and pilot, with conversion rates of 5.15% and 1.08%, respectively.

# 2.2	Website Performance Analysis

Website performance refers to the overall efficiency of a website in delivering content and functionality to its users. Website performance analysis typically involves measuring and analyzing key performance indicators (KPIs) such as:
Conversion rates: The percentage of website visitors who complete desired actions, such as making a purchase or placing an order.
Bounce rate: The percentage of visitors who leave the website after viewing only one page.
User engagement: Metrics related to user interaction and engagement on the website, such as page views, and click-through rates.
Website performance’s goal is to identify performance bottlenecks, improve user experience, increase conversions, and optimize the website's overall performance.
In this section, we aim to answer the following questions:


● What are the top entry pages?

● What are the bounce rates associated with different entry pages?

● What are the overall paid Gsearch Nonbrand bounce rate trend monthly of different entry page?

● What are the most viewed and least viewed pages on the website?

## 3.	What are the top entry pages?
SQL Codes and Results
![image](https://github.com/lightbluening/Traffic_Web_Performance_Analysis/assets/93415125/15b9ed90-4dd4-4aff-8d55-0e0a4e12f8b2)
Based on the results, it is evident that the top two landing pages are "/home" and "/lander-2," which account for 29.09% and 27.74% respectively. Collectively, these two landing pages represent more than half of the total traffic.

## 4.	What are the bounce rates associated with different entry pages?
SQL Codes and Results
![image](https://github.com/lightbluening/Traffic_Web_Performance_Analysis/assets/93415125/e6ee3889-c6b1-482c-893d-f88aa36ad3e1)

Based on the results above, it is evident that the entry page '-lander-5' exhibits the lowest bounce rate of 0.3687. Following that, the '/home' entry page has a slightly higher bounce rate of 0.4168, while '/lander-2' has a bounce rate of 0.4517. On the other hand, the remaining three entry pages all have bounce rates exceeding 0.5, which can be considered relatively high.

## 5.	What are the overall paid traffic bounce rate trend monthly of different entry page?
SQL Codes and Results
![image](https://github.com/lightbluening/Traffic_Web_Performance_Analysis/assets/93415125/1af54696-4c96-4d89-adb4-cab2b698c125)
![image](https://github.com/lightbluening/Traffic_Web_Performance_Analysis/assets/93415125/1cc52c18-8f67-4015-9fe1-b99662e67d7b)
![image](https://github.com/lightbluening/Traffic_Web_Performance_Analysis/assets/93415125/bfdf1ca3-e5e6-44b7-9820-b2c68e44205b)
![image](https://github.com/lightbluening/Traffic_Web_Performance_Analysis/assets/93415125/df52b87d-51bb-415f-963a-e2ea8b56b5d3)


From the results above, it is evident that the overall bounce rate of paid traffic has significantly decreased, from 0.6079 to the latest measurement of 0.4023. The lowest bounce rate occurred in February 2015, with only 0.3957. Additionally, the entry and exit times for each landing page can be extracted from the results.
Overall, there is a coexisting time period for some landing pages. However, the "/home" and "/lander2" landing pages have the longest serving durations, spanning 36 months and 24 months respectively. On the other hand, "/lander1" and "/lander4" experienced high bounce rates above 0.5, leading to their abandonment. Apart from "/home," "/lander3" and "/lander5" landing pages are still in use, while the others no longer exist.
It is worth noting that "/lander2" outperformed "/lander3" from July 2013 to October 2014. However, for reasons unspecified, "/lander2" performed poorly starting in November 2014 and was ultimately withdrawn.

## 6.	What are the most viewed and least viewed pages on the website? List the ranking of them.

SQL Codes and Result 
![image](https://github.com/lightbluening/Traffic_Web_Performance_Analysis/assets/93415125/38eb7af8-babc-4c4c-b6b7-8b9db59e02a7)

By pulling out the “pageview_url” of a complete session, we know that the sequence of placing an order is :
Page 1: entry pages which are '/home','/lander-1','/lander-2','/lander-3','/lander-4','/lander-5'
Page 2:'/products',
Page 3:'/the-original-mr-fuzzy'('/the-hudson-river-mini-bear','/the-birthday-sugar-panda','/the-forever-love-bear'),
Page 4:'/cart',
Page 5:'/shipping',
Page 6:'/billing-2'('/billing')(‘/billing’ was withdrawn in January 2013.)
Page 7:'/thank-you-for-your-order'
In our analysis, we began by excluding the landing pages' URLs. It is evident that the "/products" pageview holds the top position. Following the product page, we identified four specific product pages. Among them, the "/the-original-mr-fuzzy" page emerged as the most popular, accumulating 162,525 views. However, the remaining three product pages received significantly less attention, with the "the-hudson-river-mini-bear" page having the lowest views at only 2,610. We need to investigate the underlying reasons for the low view count of this particular page.

SQL Codes and Result
![image](https://github.com/lightbluening/Traffic_Web_Performance_Analysis/assets/93415125/81218c9c-77d0-4108-bb90-188c1296b695)
![image](https://github.com/lightbluening/Traffic_Web_Performance_Analysis/assets/93415125/889735af-1b11-4eef-879c-1a3852f1eedf)
Based on the aforementioned results, it is evident that the "/the-original-mr-fuzzy" product page has been consistently maintained for the longest duration compared to the other product pages. Following that, we have the "/the-forever-love-bear," "/the-birthday-sugar-panda," and "/the-hudson-river-mini-bear" pages, which were introduced as new products in 2013, 2014, and 2015 respectively. It is reasonable to observe that the "/the-hudson-river-mini-bear" page has the lowest number of views among these four pages. The discrepancy in release dates prompts us to delve deeper into the performance analysis of these specific product pages.
So, we attempted to determine the click-through rates of the four product pages on a monthly basis.
SQL Codes and Result
![image](https://github.com/lightbluening/Traffic_Web_Performance_Analysis/assets/93415125/306bc93b-5f58-4244-bdd8-a7cc823d47f3)
![image](https://github.com/lightbluening/Traffic_Web_Performance_Analysis/assets/93415125/eb5721a1-c0b0-4dca-803c-028e3b9d8bcf)

Based on the results above, we can observe that the click-through rate of "/the-original-mr-fuzzy" remains consistently stable over time. In contrast, the "/the-hudson-river-mini-bear" page consistently exhibits the lowest click-through rate. This outcome is expected, considering that it is the newest page and product among the four. However, it is noteworthy that while the click-through rates of the other two products surpassed 0.1 within one month after release, "/the-hudson-river-mini-bear" took four months and still maintains a click-through rate of around 0.05.

Additionally, we have identified a seasonal trend in the click-through rate of "/the-forever-love-bear." Notably, in February, the click-through rate reaches its peak, nearly 0.3. This finding aligns with the expectation, as February includes Valentine's Day, which likely generates higher interest and engagement for this product.

# 2.3 Conversion Funnel Analysis

Conversion funnel analysis is a method used to examine and analyze the progression of website visitors or users through the various stages of a conversion process. The conversion funnel represents the series of steps that users go through, starting from awareness or discovery and moving towards a desired action or conversion. These actions can include making a purchase or placing an order. 
By conducting conversion funnel analysis, businesses can gain insights into user behavior, identify potential drop-off points, and optimize the conversion process.
In this section, we aim to finish the following task:
● Create a conversion funnel to assess the conversion rates of the top two entry pages ((‘/home’,’/lander-2’) for paid traffic.

## 7.	Can the paid traffic conversion funnels be established to evaluate the conversion rates of the top 2 entry pages?

We solve this problem following the following steps:
Step 1: Select all pageviews for relevant sessions.
Step 2: Identify the sequence of placing an order.
Step 3: Build the conversion funnel.

### Step 1: Select all pageviews for relevant sessions.
SQL Codes
![image](https://github.com/lightbluening/Traffic_Web_Performance_Analysis/assets/93415125/cce0c581-cc20-443b-8afc-d97c59e31100)

### Step 2: Identify the sequence of placing an order.
SQL Codes
![image](https://github.com/lightbluening/Traffic_Web_Performance_Analysis/assets/93415125/a8401aac-a061-4e7b-a767-09b368aebd09)
Result 
![image](https://github.com/lightbluening/Traffic_Web_Performance_Analysis/assets/93415125/f31a3d0c-fa5a-47fd-b616-63e84e4b398b)

We have selected two sessions from the results: “website_session_id” 472668 and 472678. It is worth noting that both of these sessions have placed an order.
By pulling out the “pageview_url” of a complete session, we know that the sequence of placing an order is:
Page 1: entry pages (which are '/home','/lander-1','/lander-2','/lander-3','/lander-4','/lander-5')
Page 2: product index page (which is '/products)',
Page 3: specific product pages'(which are ‘/the-original-mr-fuzzy','/the-hudson-river-mini-bear','/the-birthday-sugar-panda','/the-forever-love-bear'),
Page 4: cart page (which is '/cart'),
Page 5: shipping page (which is '/shipping'),
Page 6: billing pages (which are '/billing-2','/billing')
Page 7: placing an order page (which is '/thank-you-for-your-order')

### Step 3: Build the conversion funnel.
SQL Codes
![image](https://github.com/lightbluening/Traffic_Web_Performance_Analysis/assets/93415125/e725de83-e670-4769-a012-165e50f61af8)
![image](https://github.com/lightbluening/Traffic_Web_Performance_Analysis/assets/93415125/565242cc-b5d2-40b3-841d-85c843a4fe05)

Result
![image](https://github.com/lightbluening/Traffic_Web_Performance_Analysis/assets/93415125/1ddd58e7-f378-4b8e-9eba-50a5c12554b2)

Based on the results above, we can observe the following conversion rates:
The conversion rate from the landing page to the product index page is 0.5505.
The conversion rate from the product index page to the specific product page is 0.8222.
The conversion rate from the specific product page to the cart page is 0.4561.
The conversion rate from the cart page to the shipping page is 0.6913.
The conversion rate from the shipping page to the billing page is 0.8225.
The conversion rate from the billing page to the order page is 0.6305.
Notably, the lowest conversion rate is observed from the specific product page to the cart page, while the highest conversion rate occurs from the shipping page to the billing page.

# 2.4 Channel Portfolio Analysis

Channel portfolio analysis is a strategic approach used by businesses to evaluate and optimize their marketing channels and investments. It involves analyzing the performance, effectiveness, and profitability of different marketing channels that a company utilizes to reach its target audience and promote its products or services. During channel portfolio analysis, businesses typically assess key metrics and factors for each marketing channel, including audience reach, acquisition and conversion rates, cost and return on investment (ROI), etc. By conducting a thorough channel portfolio analysis, businesses can identify high-performing channels, allocate resources efficiently, optimize marketing strategies, and maximize ROI.
The purpose of channel portfolio analysis is to gain insights into the contribution and impact of each marketing channel on the overall business goals and objectives. It helps businesses make informed decisions about resource allocation, budget allocation, and channel optimization.
Organic search refers to the process by which users find a website through non-paid search engine results. When a user enters a query into a search engine, such as Google, and clicks on one of the non-advertising search results, it is considered an organic search. Organic search results are determined by the search engine's algorithm, taking into account factors like relevance, authority, and user intent.
Direct type-in, on the other hand, refers to when a user directly enters a website's URL into their browser's address bar, rather than accessing it through a search engine or other referral sources. In this case, the user is already familiar with the website or has the URL saved/bookmarked and directly accesses it by typing the URL.
In summary, organic search involves users finding a website through non-paid search engine results, while direct type-in occurs when users directly enter a website's URL into their browser without using a search engine or referral sources. Both organic search and direct type-in contribute to a well-rounded online presence, with organic search driving targeted traffic from search engines and direct type-in reflecting brand recognition, trust, and loyalty. By focusing on optimizing for organic search and building brand awareness, businesses can enhance their visibility, attract valuable traffic, and foster long-term relationships with their audience.

## 8.	What is the distribution of traffic across various device types for each traffic source?

### Step 1: Calculate the total sessions for desktop and mobile respectively.

SQL Codes
![image](https://github.com/lightbluening/Traffic_Web_Performance_Analysis/assets/93415125/c8e52be9-0dc9-4103-a90b-126ffdab4925)


Result 

![image](https://github.com/lightbluening/Traffic_Web_Performance_Analysis/assets/93415125/292257ea-5faa-4c84-8f7a-3bebb30ee88b)

### Step 2: Calculate the percentage for desktop and mobile of different utm_source.
SQL Codes
![image](https://github.com/lightbluening/Traffic_Web_Performance_Analysis/assets/93415125/00742c9e-27fe-41d3-932c-51c8e3079833)

Result

![image](https://github.com/lightbluening/Traffic_Web_Performance_Analysis/assets/93415125/73bcefe8-4e3d-4b05-a1cc-77163938d120)
Based on the above results, it is evident that Gsearch is the dominant channel for both mobile and desktop searches, with a share of 0.6922 and 0.6577, respectively. In contrast, the other three channels exhibit significantly lower shares, regardless of whether it is in the mobile or desktop category.

## 9.	What percentage of sessions originate from organic search, direct type-ins, and paid searches on a yearly basis?

SQL Codes

![image](https://github.com/lightbluening/Traffic_Web_Performance_Analysis/assets/93415125/e0d71379-706b-4c59-9c79-9e438749b0ff)

Result

![image](https://github.com/lightbluening/Traffic_Web_Performance_Analysis/assets/93415125/da05e3a7-8c26-4342-8a52-71e431d4ce27)

Based on the results above, it is evident that over a span of 4 years, the brand has significantly improved its reputation and recognition. The organic search rate has increased from an initial 0.0476 to the latest measurement of 0.1214, showcasing notable growth. Similarly, the direct type-in rate has seen an improvement from 0.0445 to 0.111, indicating increased brand familiarity among users.
Furthermore, it is noteworthy that the paid brand search rate has also witnessed growth, rising from the original 0.0455 to the latest measurement of 0.1138. This suggests an increased investment in brand-specific advertising and an associated rise in user engagement.
However, it is important to mention that the paid nonbrand search rate has experienced a decline, dropping from the initial 0.8624 to the latest measurement of 0.6539. This indicates a decrease in the effectiveness or focus on non-branded paid search campaigns, which warrants further investigation and potential optimization strategies.

# Part 3 Findings



# Part 4 Suggestions





