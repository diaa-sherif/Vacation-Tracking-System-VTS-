# DB schema

create database task1;

use task1;

 create table Category(
 category_id int PRIMARY KEY,
 category_name varchar(50)
 );
 
  create table Customer(
 customer_id int PRIMARY KEY,
 first_name varchar(50),
 last_name varchar(50),
 email varchar(50),
 password varchar(50)
 );
 
  create table Product(
 product_id int PRIMARY KEY,
 name varchar(50),
 description varchar(50),
 price DECIMAL(10,2),
 stock_quantity int,
 category_id int,
 CONSTRAINT fk_category
 FOREIGN KEY (category_id)
 REFERENCES category(category_id)
 );
 
   create table Orders(
 order_id int PRIMARY KEY,
 order_date date,
 total_amount DECIMAL(10,2),
 customer_id int,
 CONSTRAINT fk_customer
 FOREIGN KEY (customer_id)
 REFERENCES customer(customer_id)
 );
 
   create table Order_details(
 order_detail_id int PRIMARY KEY,
 quantity int,
 unit_price DECIMAL(10,2),
 product_id int,
 order_id int,
 
 CONSTRAINT fk_product
 FOREIGN KEY (product_id)
 REFERENCES product(product_id),
 
 CONSTRAINT fk_order
 FOREIGN KEY (order_id)
 REFERENCES orders(order_id)
 ); 
 
# ERD diagram

<img width="1096" height="701" alt="image" src="https://github.com/user-attachments/assets/064c16c3-5833-4ab5-bdb9-b57421d89895" />

# total daily revenue 

SELECT order_date, sum(total_amount) FROM orders
group by order_date
order by order_date;

# total revenue for a specific date
SELECT order_date, sum(total_amount) FROM orders
where order_date = '1946-01-23'



 
