# Restaurant
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


WITH restaurant_ranked AS (
    SELECT
        Restaurant_ID,
        Popularity_score,
        NTILE(4) OVER (
            ORDER BY Popularity_score DESC
        ) AS performance_group
    FROM r_stats
)
SELECT
    Restaurant_ID,
    Popularity_score
FROM restaurant_ranked
WHERE performance_group = 1;
