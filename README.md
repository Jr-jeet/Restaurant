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

#Which cuisines have the highest popularity?
select * from cuisines ;
select * from nutrition ;restaurants
