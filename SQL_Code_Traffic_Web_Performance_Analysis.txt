-- Data Preparation
-- Find the total number of website sessions
Use mavenfuzzyfactory;
Select Count(Distinct website_session_id) As number_of_sessions
From website_sessions;

 
-- Check missing values

SELECT
    SUM(website_session_id IS NULL) AS sessionid_no,
    SUM(created_at IS NULL) AS createdat_no,
    SUM(user_id IS NULL) AS userid_no,
    SUM(is_repeat_session IS NULL) AS isrepeatsession_no,
    SUM(utm_source IS NULL) AS utmsource_no,
    SUM(utm_campaign IS NULL) AS utmcampaign_no,
    SUM(utm_content IS NULL) AS utmcontent_no,
    SUM(device_type IS NULL) AS devicetype_no,
    SUM(http_referer IS NULL) AS httpreferer_no
FROM
    website_sessions
	
	
-- Check the number of duplicates
SELECT 
website_session_id, 
created_at,
user_id,
is_repeat_session,
utm_source,
utm_campaign,
utm_content,
device_type,
http_referer,
COUNT(*) as duplicate_number
FROM website_sessions
GROUP BY website_session_id, 
created_at,
user_id,
is_repeat_session,
utm_source,
utm_campaign,
utm_content,
device_type,
http_referer
HAVING COUNT(*) > 1;

-- Check the time range of the dataset
Use mavenfuzzyfactory;
Select Min(created_at) as start_time,
Max(created_at) As end_time
From website_sessions


-- Show the unique values for the categorical columns
SELECT 
DISTINCT utm_source, 
utm_campaign,
utm_content,
http_referer
FROM mavenfuzzyfactory.website_sessions;


Exploratory Data Analysis

-- 1. What is the primary traffic source of website traffic?
Select utm_source,
utm_campaign,
http_referer,
Count(Distinct website_session_id) as session_count
From website_sessions
Group By utm_source,
utm_campaign,
http_referer
Order BY session_count Desc


-- 2. How do different traffic channels perform in terms of conversion rates
Select 
ws.utm_source,
ws.utm_campaign,
ws.http_referer,
Count(Distinct ws.website_session_id) As sessions,
Count(Distinct o.order_id) As orders,
Count(Distinct o.order_id)
/Count(Distinct ws.website_session_id) As session_to_order_conversion_rate
From website_sessions as ws
Left Join orders as o
On o.website_session_id=ws.website_session_id
Group by ws.utm_source,
ws.utm_campaign,
ws.http_referer
Order By session_to_order_conversion_rate Desc




-- 3.What are the top entry pages?

With entry_page AS
(SELECT 
website_session_id,
Min(website_pageview_id) as entry_point_id
FROM mavenfuzzyfactory.website_pageviews
Group By website_session_id)
Select 
wp.pageview_url,
Count(Distinct ep.website_session_id) As entrypoint_count
From CTE_entry_page as ep
Left Join website_pageviews AS wp
On wp.website_pageview_id=ep.entry_point_id
Group By wp.pageview_url
Order By entrypoint_count Desc

-- 4. What are the bounce rates associated with different entry pages?
With bounced_table AS
(Select
website_session_id,
Count(Distinct website_pageview_id) As pageview_count
From website_pageviews
Group By website_session_id
Having Count(Distinct website_pageview_id)=1)
Select wp.pageview_url,
Count(Distinct wp.website_session_id) As sessions,
Count(Distinct bt.website_session_id) AS bounced_sessions,
Count(Distinct bt.website_session_id)
/Count(Distinct wp.website_session_id) As bounced_rate 
From website_pageviews as wp
Left Join bounced_table As bt
On wp.website_session_id=bt.website_session_id
Group By pageview_url 
Having pageview_url In ('/home','/lander-1','/lander-2','/lander-3','/lander-4','/lander-5')
Order By bounced_rate


-- 5. What are the overall paid Gsearch Nonbrand bounce rate trend monthly of different entry page?
Use mavenfuzzyfactory;
Create Temporary Table session_with_first_pageview_ids 
SELECT 
    ws.website_session_id,
    ws.created_at As session_create_time,
    MIN(wp.website_pageview_id) AS min_pageview_id,
    COUNT(DISTINCT wp.website_pageview_id) AS num_pages_visited
FROM website_sessions As ws
Left Join website_pageviews As wp
On ws.website_session_id=wp.website_session_id
Where utm_source='gsearch' 
And utm_campaign='nonbrand'
Group by 1;

Create Temporary Table session_with_entry_pages
Select 
session_with_first_pageview_ids.website_session_id,
session_with_first_pageview_ids.session_create_time,
session_with_first_pageview_ids.min_pageview_id As first_pageview_id,
session_with_first_pageview_ids.num_pages_visited,
website_pageviews.pageview_url As entry_page
From session_with_first_pageview_ids
Left Join website_pageviews
On session_with_first_pageview_ids.min_pageview_id=website_pageviews.website_pageview_id;

Select 
Year(session_with_entry_pages.session_create_time) As year,
Month(session_with_entry_pages.session_create_time) As month,
Count(Distinct website_session_id) As total_sessions,
Count(Distinct Case When num_pages_visited=1 Then website_session_id Else Null End) As bounced_sessions,
Count(Distinct Case When num_pages_visited=1 Then website_session_id Else Null End)
/Count(Distinct website_session_id) As total_bounce_rate,
Count(Distinct Case When entry_page='/home' And  num_pages_visited=1 Then website_session_id Else Null End)
/Count(Distinct Case When entry_page='/home' Then website_session_id Else Null End) As home_bounce_rate,
Count(Distinct Case When entry_page='/lander-1' And  num_pages_visited=1 Then website_session_id Else Null End)
/Count(Distinct Case When entry_page='/lander-1' Then website_session_id Else Null End) As lander1_bounce_rate,
Count(Distinct Case When entry_page='/lander-2' And  num_pages_visited=1 Then website_session_id Else Null End)
/Count(Distinct Case When entry_page='/lander-2' Then website_session_id Else Null End) As lander2_bounce_rate,
Count(Distinct Case When entry_page='/lander-3' And num_pages_visited=1  Then website_session_id Else Null End)
/Count(Distinct Case When entry_page='/lander-3' Then website_session_id Else Null End) As lander3_bounce_rate,
Count(Distinct Case When entry_page='/lander-4'And num_pages_visited=1 Then website_session_id Else Null End)
/Count(Distinct Case When entry_page='/lander-4' Then website_session_id Else Null End) As lander4_bounce_rate,
Count(Distinct Case When entry_page='/lander-5'And num_pages_visited=1 Then website_session_id Else Null End)
/Count(Distinct Case When entry_page='/lander-5' Then website_session_id Else Null End) As lander5_bounce_rate
From session_with_entry_pages
Group By 1,2


-- 6. What are the most viewed and least viewed pages on the website? List the ranking of them.

With pageview_number As
(Select pageview_url,
Count(Distinct website_pageview_id) As pageview_count
From website_pageviews
Group By pageview_url
Having pageview_url Not In ('/home','/lander-1','/lander-2','/lander-3','/lander-4','/lander-5')
Order By pageview_count Desc)

Select pageview_url,
pageview_count,
Dense_rank()Over(Order By pageview_count Desc) pageview_rank
From pageview_number







-- 7. Can conversion funnels be established to evaluate the conversion rates of top 2 entry pages?
-- Figure out the sequence of website pages for placing an order
With max_pageview_count AS
(
SELECT website_session_id,
Count(Distinct website_pageview_id) As pageview_count
FROM mavenfuzzyfactory.website_pageviews
Group By website_session_id
Order by pageview_count Desc
-- Having Count(Distinct website_pageview_id) =7
)
Select mpc.website_session_id,
wp.website_pageview_id,
wp.pageview_url
From max_pageview_count As mpc
Left Join website_pageviews As wp
On mpc.website_session_id=wp.website_session_id
Group By website_session_id,wp.website_pageview_id
Order By pageview_count Desc
-- By pulling out the pageview_url of a complete session, we know that the sequence of placing an order is :
Page 1: entry pages which are '/home','/lander-1','/lander-2','/lander-3','/lander-4','/lander-5'
Page 2:'/products',
Page 3:'/the-original-mr-fuzzy'('/the-hudson-river-mini-bear','/the-birthday-sugar-panda','/the-forever-love-bear'),
Page 4:'/cart',
Page 5:'/shipping',
Page 6:'/billing-2'('/billing')
Page 7:'/thank-you-for-your-order' 
-- Select sessions with specific entry pages (‘/home’,’/lander-2’)
Create Temporary Table top2_entry_page_sessions
Select 
website_session_id,
Min(website_pageview_id) As entry_page_id
From website_pageviews
Where pageview_url ='/home' Or pageview_url ='/lander-2'
Group By website_session_id;
Create Temporary Table top2_entry_page_conversion_funnel
-- Build conversion funnel
Select 
teps.*,
wp.pageview_url,
Case When pageview_url='/products' Then 1 Else 0 End As to_product_index_page,
Case When pageview_url IN ('/the-original-mr-fuzzy',
'/the-hudson-river-mini-bear',
'/the-birthday-sugar-panda',
'/the-forever-love-bear') Then 1 Else 0 End As to_product_page,
Case When pageview_url='/cart' Then 1 Else 0 End As to_cart_page,
Case When pageview_url='/shipping' Then 1 Else 0 End As to_shipping_page,
Case When pageview_url In('/billing-2','/billing') Then 1 Else 0 End As to_billing_page,
Case When pageview_url='/thank-you-for-your-order' Then 1 Else 0 End As to_order_page
From top2_entry_page_sessions As teps
Left Join website_sessions As ws
On teps.website_session_id=ws.website_session_id
Left Join website_pageviews As wp
On teps.website_session_id=wp.website_session_id
Where ws.utm_source='gsearch' and ws.utm_campaign='nonbrand';
--  Calculte conversion rate of each step
Select 
-- Count(Distinct website_session_id) As total_sessions,
Sum(to_product_index_page)/Count(Distinct website_session_id) As product_index_conv_rate,
Sum(to_product_page)/Sum(to_product_index_page) As product_conv_rate,
Sum(to_cart_page)/Sum(to_product_page) As cart_conv_rate,
Sum(to_shipping_page)/Sum(to_cart_page) As shipping_conv_rate,
Sum(to_billing_page)/Sum(to_shipping_page) As billing_conv_rate,
Sum(to_order_page)/Sum(to_billing_page) As order_conv_rate
-- Sum(to_product_index_page) As total_to_product_index,
-- Sum(to_product_page) As total_to_product,
-- Sum(to_cart_page) As total_to_cart,
-- Sum(to_shipping_page) As total_to_shipping,
-- Sum(to_billing_page) As total_to_billing,
-- Sum(to_order_page) As total_to_order
From top2_entry_page_conversion_funnel

8.	What is the distribution of traffic across various device types for each traffic source?
-- Check distinct utm_source
SELECT 
distinct utm_source
FROM mavenfuzzyfactory.website_sessions;

Select 
utm_source,
Count(Distinct website_session_id) As total_sessions,
Count(Distinct Case When device_type='mobile' then website_session_id Else Null End) As mobile_sessions,
Count(Distinct Case When device_type='desktop' then website_session_id Else Null End) As desktop_sessions,
Count(Distinct Case When device_type='mobile' then website_session_id Else Null End)
/Count(Distinct website_session_id) As mobile_percentage,
Count(Distinct Case When device_type='desktop' then website_session_id Else Null End)
/Count(Distinct website_session_id) As desktop_percentage
From website_sessions
Group By 1
Order By 2

9.	What percentage of sessions originate from organic search, direct type-ins, and paid searches?
SELECT 
Year(created_at),
Count(Distinct Case When utm_source Is Null And utm_campaign Is Null And http_referer In ('https://www.gsearch.com','https://www.bsearch.com') Then website_session_id Else Null End)
/Count(Distinct website_session_id) As organic_search_rate,
Count(Distinct Case When utm_source Is Null And utm_campaign Is Null And http_referer Is Null Then website_session_id Else Null End)
/Count(Distinct website_session_id) As direct_type_in_rate,
Count(Distinct Case When utm_campaign='brand' Then website_session_id Else Null End)
/Count(Distinct website_session_id) As paid_brand_search_rate,
Count(Distinct Case When utm_campaign='nonbrand' Then website_session_id Else Null End)
/Count(Distinct website_session_id) As paid_nonbrand_search_rate
-- When utm_source Is Null And utm_campaign Is Null And http_referer Is Null Then 'direct_type_in'
-- When utm_campaign='brand' Then 'paid_brand_search'
-- When utm_campaign='nonbrand' Then 'paid_nonbrand_search'
-- End As search_type,
-- Count(Distinct website_session_id) As sessions
FROM mavenfuzzyfactory.website_sessions
Group By 1
