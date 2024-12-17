/*Traffic source analysis*/
select  ws.utm_content,
count(distinct ws.website_session_id) as sessions,
count(distinct od.order_id) as orders
from website_sessions  ws left join orders  od on ws.website_session_id = od.website_session_id
where utm_content is not null
group by  1
order by 2 desc;

/*where the bulk of our website sessions are coming from*/
select count(website_session_id) as sessions , utm_source , utm_campaign,http_referer
from website_sessions
where utm_source is not null
group by utm_source , utm_campaign,http_referer 
order by sessions desc;

/*calculate the conversion rate (CVR) from session to order*/
select 
count(distinct ws.website_session_id ) as sessions,
count(distinct od.order_id) as orders,
count(distinct od.order_id)/count(distinct ws.website_session_id )*100 as CVR
from website_sessions as ws left join orders od on ws.website_session_id = od.website_session_id
where  utm_source = 'gsearch' and utm_campaign='nonbrand';

/*Calculating the top websites which has more no of views*/
select pageview_url,count(distinct website_pageview_id) as sessions
from website_pageviews
group by 1
order by 2 desc
limit 5;

/*Calculating pageviews hourly*/
select count(wp.website_pageview_id) as no_of_views, hour(wp.created_at) as hour
from website_pageviews as wp left join website_sessions as ws using(website_session_id)
group by 2
order by 1 desc
limit 7;

/*Identifying Monthly Trends*/
Could you pull monthly trends for gsearch sessions and orders so that we can showcase the growth there?*/
select year(ws.created_at) as year ,month(ws.created_at) as month , count(distinct od.order_id) as no_of_orders , 
count(distinct ws.website_session_id) as no_of_sessions,
count(distinct od.order_id)/count(distinct ws.website_session_id)*100 as CVR
from website_sessions as ws left join orders as od on ws.user_id = od.user_id
where utm_source = "gsearch" and year(ws.created_at) = 2012
group by 1,2
order by 5 desc ;

select weekday(created_at) as week ,year(created_at) as year , count(order_id) as cnt_of_orders 
from orders 
where year(created_at) = 2012 
group by 1 , 2
order by 3 desc;

/*Product Analysis*/
select primary_product_id , count(order_id) as orders,sum(price_usd) as revenue , 
sum(price_usd-cogs_usd) as margin,avg(price_usd) as Avg_order_value
from orders 
group by 1
order by 5 desc ;

select 
month(created_at) as month, 
year(created_at) as year, 
count(distinct order_id) as orders,
sum(price_usd) as revenue , 
sum(price_usd-cogs_usd) as margin
from orders 
where year(created_at) = 2012
group by month(created_at) , year(created_at)
order by 5 desc;

/*Product refund analysis product return rate */
with cte as(
select ot.product_id as product_id,
count(oi.order_item_id) as no_of_orders_returned
from  order_item_refunds as oi
left join order_items as ot on oi.order_item_id = ot.order_item_id
group by 1
order by 2 desc),
cte2 as 
(select primary_product_id as product_id ,count(order_id) as no_of_orders_placed
from orders 
group by primary_product_id)
select cte.product_id,
no_of_orders_returned/no_of_orders_placed*100 as product_return_rate
from cte left join cte2 using(product_id)
group by product_id
order by 2 desc;

/*product return analysis through time */
select year(oi.created_at) as month , count(oi.order_item_refund_id) as no_of_return__orders
from  order_item_refunds as oi
left join order_items as ot on oi.order_item_id = ot.order_item_id
group by 1
order by 2 desc;
