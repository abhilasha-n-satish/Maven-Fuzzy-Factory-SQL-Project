# Data for Board Meeting Presentation

## Introduction :
Maven Fuzzy Factory is fictitious and forward-thinking online retailer that has embarked on its e-commerce Journey. It is driven by a desire to bring novel and exciting products to the market, leveraging the power of e-commerce to reach a wide audience.

Refer below for the situation.

![Introduction Situation](images/Situation.png)

## Database Schema: 

We will be using the following tables for analysis.

![DBSchema](images/dbschema.png)

### Table Creation:
The table is a very long one and hence unable to upload here.

### Scenarios and their respective codes:

#### 1. Gsearch seems to be the biggest driver of our business. Could you pull monthly trends for gsearch sessions and orders so that we can showcase the growth there? 

``` SQL
SELECT
YEAR(ws.created_at) AS Year,
MONTH(ws.created_at) AS Month,
COUNT(DISTINCT ws.website_session_id) AS sessions,
COUNT(DISTINCT o.order_id) AS orders
FROM
website_sessions AS ws
LEFT JOIN 
orders AS o
ON ws.website_session_id = o.website_session_id
WHERE ws.created_at < '2012-11-27'
AND ws.utm_source = 'gsearch'
GROUP BY Year, Month;
```

#### 2. Next, it would be great to see a similar monthly trend for Gsearch, but this time splitting out nonbrand and brand campaigns separately. I am wondering if brand is picking up at all. If so, this is a good story to tell. 

``` SQL
SELECT
YEAR(ws.created_at) AS yr,
MONTH(ws.created_at) AS mon,
COUNT(DISTINCT CASE WHEN utm_campaign = 'brand' THEN ws.website_session_id ELSE NULL END) AS brand_sessions,
COUNT(DISTINCT CASE WHEN utm_campaign = 'brand' THEN o.order_id ELSE NULL END) AS brand_orders,
COUNT(DISTINCT CASE WHEN utm_campaign = 'nonbrand' THEN ws.website_session_id ELSE NULL END) AS nonbrand_sessions,
COUNT(DISTINCT CASE WHEN utm_campaign = 'nonbrand' THEN o.order_id ELSE NULL END) AS nonbrand_orders
FROM
website_sessions AS ws
LEFT JOIN 
orders AS o
ON ws.website_session_id = o.website_session_id
WHERE ws.created_at < '2012-11-27'
AND ws.utm_source = 'gsearch'
GROUP BY yr, mon;
```

#### 3. While we’re on Gsearch, could you dive into nonbrand, and pull monthly sessions and orders split by device type? I want to flex our analytical muscles a little and show the board we really know our traffic sources. 

``` SQL
SELECT
	YEAR(ws.created_at) AS yr, 
    MONTH(ws.created_at) AS mon, 
    COUNT(DISTINCT CASE WHEN device_type = 'desktop' THEN ws.website_session_id ELSE NULL END) AS desktop_sessions, 
    COUNT(DISTINCT CASE WHEN device_type = 'desktop' THEN o.order_id ELSE NULL END) AS desktop_orders,
    COUNT(DISTINCT CASE WHEN device_type = 'mobile' THEN ws.website_session_id ELSE NULL END) AS mobile_sessions, 
    COUNT(DISTINCT CASE WHEN device_type = 'mobile' THEN o.order_id ELSE NULL END) AS mobile_orders
FROM website_sessions AS ws
	LEFT JOIN orders AS o
		ON ws.website_session_id = o.website_session_id
WHERE ws.created_at < '2012-11-27'
	AND ws.utm_source = 'gsearch'
    AND ws.utm_campaign = 'nonbrand'
GROUP BY yr,mon;
```

#### 4. I’m worried that one of our more pessimistic board members may be concerned about the large % of traffic from Gsearch. Can you pull monthly trends for Gsearch, alongside monthly trends for each of our other channels?

``` SQL

```
