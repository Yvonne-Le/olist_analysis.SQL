# olist_analysis.SQL
 Results & Business Insights：
 
 ###SQL QUERY:

 ### Data Perparation ###
 
 SELECT *
FROM `olist-sql-project.olist_sql_project.olist_order` AS i
JOIN `olist-sql-project.olist_sql_project.olist_orders` AS o ON i.order_id = o.order_id
JOIN `olist-sql-project.olist_sql_project.olist_customer` AS c ON o.customer_id = c.customer_id
JOIN `olist-sql-project.olist_sql_project.olist_order_payments` AS p ON o.order_id = p.order_id
JOIN `olist-sql-project.olist_sql_project.olist_products` AS pr ON i.product_id = pr.product_id
WHERE o.order_status = 'approved';



 
 Q1 Which month has the highest total sales?
 
 "The results shows April is the highest total sales." 

####SQL QUERY:

SELECT 
  DATE_TRUNC(DATE(o.order_purchase_timestamp),MONTH)AS month,
  SUM(i.price + i.freight_value) AS total_sales
FROM `olist-sql-project.olist_sql_project.olist_order` i
JOIN `olist-sql-project.olist_sql_project.olist_orders` o ON i.order_id = o.order_id
WHERE o.order_status = 'approved'
GROUP BY month
ORDER BY total_sales DESC;



<img width="592" height="366" alt="Screenshot 2025-10-17 at 4 57 24 PM" src="https://github.com/user-attachments/assets/d7c2bbda-370e-44db-b5a3-1923237a4ae0" />







Q2 Which products or categories are the best sellers?

"The best seller is beleza_saude." 

###SQL QUERY:

SELECT 
    pr.product_category_name AS category,
    SUM(i.price) AS total_revenue,
    COUNT(DISTINCT i.order_id) AS total_orders
FROM `olist-sql-project.olist_sql_project.olist_order` i
JOIN `olist-sql-project.olist_sql_project.olist_orders` o ON i.order_id = o.order_id
JOIN `olist-sql-project.olist_sql_project.olist_products` pr ON i.product_id = pr.product_id
WHERE o.order_status = 'delivered'
GROUP BY 1
ORDER BY total_revenue DESC
LIMIT 10;



Q3 Which cities or states contribute the most to sales?
"The most to sales is sao paulo, SP." 

###SQL QUERY:

SELECT 
  c.customer_city,
  c.customer_state,
  SUM(i.price + i.freight_value) AS total_sales
FROM `olist-sql-project.olist_sql_project.olist_order`i
JOIN `olist-sql-project.olist_sql_project.olist_orders` o ON i.order_id = o.order_id
JOIN `olist-sql-project.olist_sql_project.olist_customer` c ON o.customer_id = c.customer_id
WHERE o.order_status = 'delivered'
GROUP BY 1, 2
ORDER BY total_sales DESC
LIMIT 10;





### Q4 What is the average order value (AOV)? ###
"The AOV is 159.8.

###SQL QUERY:

SELECT 
    AVG(order_total) AS avg_order_value
FROM (
    SELECT 
        i.order_id,
        SUM(i.price + i.freight_value) AS order_total
    FROM `olist-sql-project.olist_sql_project.olist_order`i
    JOIN `olist-sql-project.olist_sql_project.olist_orders` o ON i.order_id = o.order_id
    WHERE o.order_status = 'delivered'
    GROUP BY i.order_id
) AS sub;






Q5: What is the distribution of different payment methods? 
'The percentage of credit card is 77%, boleto is 20%, voucher is 3.89%, debit card is 1.54%.

###SQL QUERY:

SELECT 
    p.payment_type,
    COUNT(DISTINCT p.order_id) AS num_orders,
    ROUND(100.0 * COUNT(DISTINCT p.order_id) / 
        (SELECT COUNT(DISTINCT order_id) FROM `olist-sql-project.olist_sql_project.olist_order_payments`), 2) AS percentage
FROM `olist-sql-project.olist_sql_project.olist_order_payments` p
GROUP BY 1
ORDER BY num_orders DESC;




Q6 Is there a seasonal trend in sales?
"Yes, The most sales is May to Auguest."


###SQL QUERY:

SELECT 
    EXTRACT(MONTH FROM o.order_purchase_timestamp) AS month,
    SUM(i.price + i.freight_value) AS total_sales
FROM `olist-sql-project.olist_sql_project.olist_order` i
JOIN `olist-sql-project.olist_sql_project.olist_orders` o ON i.order_id = o.order_id
WHERE o.order_status = 'delivered'
GROUP BY month
ORDER BY month;








