# Generate Sales Data Just By Using SQL

## Story
<p>
I had one task to create a Business Intelligence proof of concept for a client, and it wasn't an easy one. The catch was that the client didn't want to give me their actual data. After some serious investigation, I came up with an idea to generate millions of fake data rows using only SQL. And to make things happen, I went with Postgres.
</p>

## Here is the select statement I've used

```
  SELECT
        floor(RANDOM()*(1000-100)+100) AS id,
        generate_series('2018-01-01'::date, '2020-12-31'::date, '1 day'::interval)::date sales_date,
        (ARRAY['Riyadh','Jeddah','Dammam','Taif','Madina'])[floor(RANDOM()*5)+1] region,
        floor(RANDOM()*(5000-1000)+100) sales_amt,
        floor(RANDOM()*(12-1)+1) qty,
        (ARRAY['Katherine Underwood','Jason McDonald','Bella Kelly','Kimberly Ferguson','Paul Springer'])[floor(RANDOM()*5)+1] sales_manager,
        (ARRAY['Electronics','Food','Clothing','Furniture','Toys','Cosmetics'])[floor(RANDOM()*6)+1] category,
        (ARRAY['Online','In-store'])[floor(RANDOM()*2)+1] sales_channel,
        (ARRAY['Discount','Full price'])[floor(RANDOM()*2)+1] price_type,
        (ARRAY['Visa','Mastercard','American Express','Discover','Paypal','Cash'])[floor(RANDOM()*6)+1] payment_method,
        (ARRAY['New customer','Returning customer'])[floor(RANDOM()*2)+1] customer_type,
        (ARRAY['Saudi Arabia','United Arab Emirates','Qatar','Bahrain','Oman','Kuwait'])[floor(RANDOM()*6)+1] country
  FROM 
        GENERATE_SERIES(1, 10000);
```
<p>
    - This select statement will generate 10960000 rows. 
    - It took me around 1 and half minutes to complete the query.  
    -  *GENERATE_SERIES(1, 10000);* you can increase or decrease the number of values by modifying values here
</p>

## It is better to create a table using the above select statement

```
DROP TABLE IF EXISTS sales_data_001;
CREATE TABLE sales_data AS  -- creates new table on the go from the select statement 
SELECT
        floor(RANDOM()*(1000-100)+100) AS id,
        generate_series('2018-01-01'::date, '2020-12-31'::date, '1 day'::interval)::date sales_date,
        (ARRAY['Riyadh','Jeddah','Dammam','Taif','Madina'])[floor(RANDOM()*5)+1] region,
        floor(RANDOM()*(5000-1000)+100) sales_amt,
        floor(RANDOM()*(12-1)+1) qty,
        (ARRAY['Katherine Underwood','Jason McDonald','Bella Kelly','Kimberly Ferguson','Paul Springer'])[floor(RANDOM()*5)+1] sales_manager,
        (ARRAY['Electronics','Food','Clothing','Furniture','Toys','Cosmetics'])[floor(RANDOM()*6)+1] category,
        (ARRAY['Online','In-store'])[floor(RANDOM()*2)+1] sales_channel,
        (ARRAY['Discount','Full price'])[floor(RANDOM()*2)+1] price_type,
        (ARRAY['Visa','Mastercard','American Express','Discover','Paypal','Cash'])[floor(RANDOM()*6)+1] payment_method,
        (ARRAY['New customer','Returning customer'])[floor(RANDOM()*2)+1] customer_type,
        (ARRAY['Saudi Arabia','United Arab Emirates','Qatar','Bahrain','Oman','Kuwait'])[floor(RANDOM()*6)+1] country
	FROM 
        GENERATE_SERIES(1, 10000);

 SELECT * FROM sales_data_001;       

```

![qry](https://user-images.githubusercontent.com/15865285/231485244-5cf21e21-b68a-4d57-bfae-5db9534ce360.png)

