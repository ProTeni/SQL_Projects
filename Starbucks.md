 
## Starbucks

By examining this dataset from Starbucks, some invaluable insights were derived around the various aspects of the company's operations. This was done with the objective of helping Starbucks understand and cater to their target audience.
Some of the driving factors to understand this target audience included; analyzing customer demographics like age, gender, and location- which was used to create personalized marketing campaigns and product offerings. 
Additionally, the purchase data and customer demographics were analyzed to identify the top-selling products, optimize inventory, and increase sales. 
Membership cards were also analyzed to enable efficient tracking of customers frequency, amidst many others. <br>

 <br> ![kal-visuals-zMNaLdiZ2_I-unsplash](https://user-images.githubusercontent.com/97428597/232640458-2e9aa4ea-f053-4bf6-ad7b-e069216e1a40.jpg)



 ### Data Cleaning Process
- In an effort to explore the Starbucks customer experience, I acquired the [Starbucks satisfactory survey.csv dataset](https://github.com/ProTeni/SQL_Projects.me/files/11256744/Starbucks.satisfactory.survey.csv)from Kaggle.
- I then meticulously cleaned the dataset using Excel, ensuring its suitability for analysis.
- Next, I created a comprehensive table schema on Postgres SQL,

**The table schema**
> CREATE TABLE starbucks(  <br>
    customer_id SERIAL primary key,  <br>
    gender VARCHAR,  <br>
    age SMALLINT,  <br>
    current_status VARCHAR,  <br>
    annual_income BIGINT,  <br>
    visit_frequency VARCHAR,  <br>
    preferred_eating_method VARCHAR,  <br>
    duration_per_visit VARCHAR,  <br>
    nearest_branch VARCHAR,  <br>
    membership_card BOOLEAN,  <br>
    starbucks_products VARCHAR,  <br>
    brand_quality VARCHAR,  <br>
    price_rate VARCHAR,  <br>
    promotion_impact VARCHAR,  <br>
    ambience_rate VARCHAR,  <br>
    wifi_quality VARCHAR,  <br>
    service_rate VARCHAR,  <br>
    invite_friends_to_starbucks VARCHAR,  <br>
    promotion_source VARCHAR,  <br>
    continued_purchase_at_starbucks BOOLEAN  <br>
)
 - Finally, I imported the [cleaned starbucks datase](https://github.com/ProTeni/SQL_Projects.me/files/11256741/starbucks.csv) into PostgresSQL to begin the analysis.
  ------------------------------------------------------------------------------------
  
  
