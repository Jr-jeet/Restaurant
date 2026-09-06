<img width="151" height="148" alt="image" src="https://github.com/user-attachments/assets/b4e0a97a-454a-4ed3-a058-65d5e99edb66" />

# Restaurant
# RESTAURANT ARE THE ONE OF THE MOST RISKY BUSINESS IN THE WORLD "Failure Rate: Around 60% of restaurants fail in their first year, and up to 80% fail within five years"


                         countries
                             │
                          Country
                             │
                           cities
                             │
                     ┌───────┴────────┐
                     │                │
              city_statistics     cuisines
                 City,Country          ▲
                                       │
                                Cuisine_name
                                       │
                          Most_common_cuisine
                                       │
                         restaurant_statistics
                                │
                          Restaurant_ID
                    ┌───────────┴───────────┐
                    │                       │
          restaurant_features       delivery_metrics
                    │
                    │
             Restaurant_ID
                    │
         ┌──────────┴──────────┐
         │                     │
       Menu_ID             Menu_ID
         │                     │
     nutrition         price_history

     

use reasturant;

# which city we have to open your new branch ?





# Are restaurants with WiFi associated with higher popularity?


# Which cuisines have the highest popularity?
select * from cuisines ;
select * from r_stats , cuisines ;
select Most_common_cuisine,avg(Popularity_score)
from r_stats
group by Most_common_cuisine
order by avg(Popularity_score)  DESC;

# Which restaurants offer the best value?
select * from restaurants ;
select * from r_stats;select Estimated_value_score , Restaurant_ID
from r_stats
order by Estimated_value_score DESC ;
select * from r_f ;

alter table reasturant.restaurants
rename column ï»¿Restaurant_ID to Restaurant_ID ;


# What was common on high performance restaurant ?
select * from restaurants ,r_stats ;
select * from restaurants ;
select * from r_stats ;

select restaurants.Average_rating ,Restaurant_name ,Review_count,
r_stats.Popularity_score
from restaurants
left join r_stats
on restaurants.Restaurant_ID=  r_stats.Restaurant_ID 
order by Average_rating DESC ;

select restaurants.Average_rating ,Restaurant_name ,Review_count,
r_stats.Popularity_score
from restaurants
left join r_stats
on restaurants.Restaurant_ID=  r_stats.Restaurant_ID 
order by Popularity_score DESC ;

with P_S_T as (select restaurants.Average_rating ,Restaurant_name ,Review_count,
r_stats.Popularity_score
from restaurants
left join r_stats
on restaurants.Restaurant_ID=  r_stats.Restaurant_ID 
order by Popularity_score DESC
limit 30 )
select sum(Review_count) from P_S_T as P_S_T01 ;

with A_R_T as (select restaurants.Average_rating ,Restaurant_name ,Review_count,
r_stats.Popularity_score
from restaurants
left join r_stats
on restaurants.Restaurant_ID=  r_stats.Restaurant_ID 
order by Average_rating DESC
limit 30 ) 
select sum(Review_count) from A_R_T  as A_R_T01 ;

with  popular_reataurant as (select Popularity_score ,Restaurant_name,Delivery_available,Takeaway,Dine_in,Reservations
from  restaurants
left join r_stats
on restaurants.Restaurant_ID=r_stats.Restaurant_ID
order by Popularity_score DESC 
limit 30  
 )

select round(sum(Delivery_available = 'TRUE')/30 *100) as Delivery_count,
round(sum(Takeaway = 'TRUE' )/30 *100) as Takeaway_count,
round(sum(Dine_in = 'TRUE')/30 *100) as Dine_in_count,
round(sum(Reservations= 'TRUE' )/30 *100) as Reservations_count
from popular_reataurant ;
# Here is the  Delivery_available is the common 

# Which restaurants have the highest cancellation rates?
select * from  restaurants ,delivery_metrics ;
with  Highest_Cancel as (select r.Restaurant_ID,
r.Restaurant_name ,
d.Cancellation_rate,
d.Average_delivery_time,
d.Estimated_delivery_time,
d.Delivery_fee
from restaurants as r
join delivery_metrics as d
on r.Restaurant_ID=d.Restaurant_ID )
select Restaurant_name, Restaurant_ID,Cancellation_rate from Highest_Cancel 
order by d.Cancellation_rate DESC
limit 30;

# #Which restaurants have good value scores but low popularity and could be growth opportunities?
select * from r_stats,restaurants;
with growing_restaurant as (select rs.Popularity_score,
rs.Estimated_value_score,
r.Restaurant_name,
r.Average_rating
from r_stats as rs
join restaurants as r
on rs.Restaurant_ID=r.Restaurant_ID)
select * , row_number() over() from growing_restaurant 
where Estimated_value_score >6.6 
order by  Popularity_score 
limit 20 ;

# Which restaurants have the worst total customer fees (Delivery_fee + Service_fee + Packaging_fee)?
select * from delivery_metrics ;

with worst_customer_fee as (select Restaurant_ID,
Delivery_fee,
Service_fee,
Packaging_fee,
round(Delivery_fee+
Service_fee+
Packaging_fee) as total_fee
from delivery_metrics
order by total_fee DESC 
limit 30 )

select 
r.Restaurant_ID,
r.Restaurant_name,
w.total_fee
from worst_customer_fee as w
join restaurants as r
    on w.Restaurant_ID = r.Restaurant_ID
order by w.total_fee DESC ;

# Are restaurants with WiFi associated with higher popularity?
select * from r_stats , r_f ;
with new01 as (select s.Restaurant_ID,
s.Popularity_score,
f.WiFi
from r_stats as s
join r_f as f
on s.Restaurant_ID = f.Restaurant_ID  
order by s.Popularity_score DESC
 limit 400)
 select sum(WiFi ='True') as wifi_aviable,
sum( WiFi = 'FALSE') AS WIFI_NOT_AVIABLE
from new01;
# SO THE POINT WAS WIFI IS NOT A PART OF SUCCESSFULL REATURANT, BUT THE POINT IS THAT IF WIFI IS AVIABLE ROTATION OF TABLE BECOME SLOW "IF RESTAURANT WAS POPULAR AND CUSTUMBER ARE WAITING FOR TABLE SO NO NEEDED OF WIFI AND VICE-VERSA"

# which CITY has your best selling reasturant 
SELECT * FROM restaurants  ,r_stats ;
