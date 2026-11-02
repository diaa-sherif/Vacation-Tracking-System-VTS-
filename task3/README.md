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

# monthly report 
    SELECT  p.name AS product_name, 
        SUM(od.quantity) AS total_quantity_sold,
        max(od.unit_price) as price_per_unit,
        sum(od.quantity * od.unit_price) as total 
    FROM  Orders o
    JOIN  Order_details od ON o.order_id = od.order_id
    JOIN  Product p ON od.product_id = p.product_id
    WHERE 
        -- my data is far away so i had to make it in years 
        o.order_date >= '1970-09-01' AND o.order_date < '1980-9-30'
    GROUP BY p.product_id
    ORDER BY total_quantity_sold DESC;

# list of customers

    SELECT c.first_name, 
        c.last_name, 
        SUM(o.total_amount) AS total_order_amount
    FROM  Customer c
    JOIN  Orders o ON c.customer_id = o.customer_id
    WHERE 
        -- my data is far away so i had to make it in years
        o.order_date >= CURRENT_DATE - INTERVAL 250 month and o.order_date <= CURRENT_DATE
    GROUP BY c.customer_id, c.first_name, c.last_name
    HAVING  SUM(o.total_amount) > 500
    ORDER BY  total_order_amount DESC;

# denormalization mechanism 

we put the customer's name in the order in a column for it so we won't have the need to use join as much
like this

    create table Orders(
    order_id int PRIMARY KEY,
    order_date date,
    total_amount DECIMAL(10,2),
    customer_full_name varchar(100), -- extra column for customer name
    customer_id int,
    CONSTRAINT fk_customer
    FOREIGN KEY (customer_id)
    REFERENCES customer(customer_id)
    );
