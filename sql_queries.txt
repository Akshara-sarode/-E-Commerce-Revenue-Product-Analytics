SELECT SUM(TotalPrice) AS total_revenue FROM sales

SELECT Description AS product, SUM(TotalPrice) AS revenue  
FROM sales  
GROUP BY product  
ORDER BY revenue DESC  
LIMIT 5

SELECT strftime('%Y-%m', InvoiceDate) AS month, SUM(TotalPrice) AS monthly_revenue  
FROM sales  
GROUP BY month  
ORDER BY month

SELECT CustomerID, SUM(TotalPrice) AS total_spent  
FROM sales  
GROUP BY CustomerID  
ORDER BY total_spent DESC  
LIMIT 5


SELECT CustomerID, AVG(TotalPrice) AS avg_order_value  
FROM sales  
GROUP BY CustomerID  
ORDER BY avg_order_value DESC  
LIMIT 5


SELECT Description, COUNT(*) AS purchase_count  
FROM sales  
GROUP BY Description  
ORDER BY purchase_count DESC  
LIMIT 5

SELECT Description, SUM(Quantity) AS total_quantity  
FROM sales  
GROUP BY Description  
ORDER BY total_quantity DESC  
LIMIT 5

SELECT InvoiceNo, COUNT(*) AS item_count  
FROM sales  
GROUP BY InvoiceNo  
HAVING item_count > 1  
ORDER BY item_count DESC  
LIMIT 5

SELECT Country, SUM(TotalPrice) AS total_revenue  
FROM sales  
GROUP BY Country  
ORDER BY total_revenue DESC  
LIMIT 5

SELECT Description, SUM(TotalPrice) / SUM(Quantity) AS revenue_per_unit  
FROM sales  
GROUP BY Description  
ORDER BY revenue_per_unit DESC  
LIMIT 5

SELECT strftime('%Y-%m-%d', InvoiceDate) AS day, SUM(TotalPrice) AS daily_revenue  
FROM sales  
GROUP BY day  
ORDER BY day

SELECT  
  CASE WHEN order_count > 1 THEN 'Repeat Customer' ELSE 'One-Time Customer' END AS customer_type,  
  COUNT(*) AS count  
FROM (  
  SELECT CustomerID, COUNT(DISTINCT InvoiceNo) AS order_count  
  FROM sales  
  GROUP BY CustomerID  
)  
GROUP BY customer_type


SELECT CustomerID, MIN(InvoiceDate) AS first_purchase, MAX(InvoiceDate) AS last_purchase  
FROM sales  
GROUP BY CustomerID


SELECT InvoiceNo, AVG(Quantity) OVER() AS avg_items_per_order  
FROM sales  
GROUP BY InvoiceNo


SELECT Description, COUNT(DISTINCT CustomerID) AS unique_buyers  
FROM sales  
GROUP BY Description  
ORDER BY unique_buyers DESC  
LIMIT 5


WITH monthly AS (  
  SELECT strftime('%Y-%m', InvoiceDate) AS month, SUM(TotalPrice) AS revenue  
  FROM sales  
  GROUP BY month  
)  
SELECT month,  
       revenue,  
       LAG(revenue) OVER (ORDER BY month) AS previous_month_revenue,  
       ROUND((revenue - LAG(revenue) OVER (ORDER BY month)) * 100.0 / LAG(revenue) OVER (ORDER BY month), 2) AS growth_percent  
FROM monthly


SELECT a.Description AS product_1, b.Description AS product_2, COUNT(*) AS times_bought_together  
FROM sales a  
JOIN sales b ON a.InvoiceNo = b.InvoiceNo AND a.Description < b.Description  
GROUP BY product_1, product_2  
ORDER BY times_bought_together DESC  
LIMIT 5


SELECT Description, SUM(TotalPrice) AS revenue  
FROM sales  
GROUP BY Description  
ORDER BY revenue ASC  
LIMIT 5


SELECT Country, InvoiceNo, SUM(TotalPrice) AS order_value  
FROM sales  
GROUP BY Country, InvoiceNo  
ORDER BY Country, order_value DESC


SELECT StockCode, SUM(TotalPrice) AS total_revenue  
FROM sales  
GROUP BY StockCode  
ORDER BY total_revenue DESC  
LIMIT 10

SELECT Country, Description, SUM(Quantity) AS total_quantity  
FROM sales  
GROUP BY Country, Description  
ORDER BY total_quantity DESC  
LIMIT 10

SELECT CustomerID, Description, COUNT(*) AS purchase_count  
FROM sales  
GROUP BY CustomerID, Description  
HAVING purchase_count > 1  
ORDER BY purchase_count DESC  
LIMIT 10

SELECT strftime('%H', InvoiceDate) AS hour, SUM(TotalPrice) AS revenue  
FROM sales  
GROUP BY hour  
ORDER BY hour

SELECT CustomerID, COUNT(DISTINCT InvoiceNo) AS total_orders  
FROM sales  
GROUP BY CustomerID  
ORDER BY total_orders DESC  
LIMIT 10

SELECT strftime('%Y-%m', InvoiceDate) AS month, Description, SUM(TotalPrice) AS revenue  
FROM sales  
GROUP BY month, Description  
ORDER BY month, revenue DESC  
LIMIT 10

SELECT CustomerID, COUNT(DISTINCT Description) AS unique_products  
FROM sales  
GROUP BY CustomerID  
ORDER BY unique_products DESC  
LIMIT 10

SELECT Description, SUM(Quantity) * COUNT(DISTINCT CustomerID) AS popularity_score  
FROM sales  
GROUP BY Description  
ORDER BY popularity_score DESC  
LIMIT 10

SELECT Description, Country, COUNT(*) AS frequency  
FROM sales  
GROUP BY Description, Country  
ORDER BY frequency DESC  
LIMIT 10

SELECT Description,  
       COUNT(DISTINCT CustomerID) AS customers,  
       COUNT(*) AS purchases,  
       ROUND(CAST(COUNT(*) AS FLOAT) / COUNT(DISTINCT CustomerID), 2) AS repeat_rate  
FROM sales  
GROUP BY Description  
ORDER BY repeat_rate DESC  
LIMIT 10

SELECT *  
FROM sales  
WHERE TotalPrice <= 0  
ORDER BY TotalPrice

SELECT InvoiceNo, SUM(TotalPrice) AS total_value  
FROM sales  
GROUP BY InvoiceNo  
ORDER BY total_value DESC  
LIMIT 5

SELECT AVG(item_count) AS avg_items  
FROM (  
  SELECT InvoiceNo, COUNT(*) AS item_count  
  FROM sales  
  GROUP BY InvoiceNo  
)

WITH ranked AS (  
  SELECT CustomerID, InvoiceDate,  
         LAG(InvoiceDate) OVER (PARTITION BY CustomerID ORDER BY InvoiceDate) AS prev_date  
  FROM sales  
)  
SELECT CustomerID, AVG(julianday(InvoiceDate) - julianday(prev_date)) AS avg_days_between  
FROM ranked  
WHERE prev_date IS NOT NULL  
GROUP BY CustomerID  
ORDER BY avg_days_between


WITH monthly_products AS (  
  SELECT strftime('%Y-%m', InvoiceDate) AS month, Description  
  FROM sales  
  GROUP BY month, Description  
)  
SELECT Description  
FROM monthly_products  
GROUP BY Description  
HAVING COUNT(DISTINCT month) = (SELECT COUNT(DISTINCT strftime('%Y-%m', InvoiceDate)) FROM sales)


SELECT CustomerID, Description, COUNT(*) AS frequency  
FROM sales  
GROUP BY CustomerID, Description  
ORDER BY CustomerID, frequency DESC

SELECT Description, COUNT(DISTINCT CustomerID) AS buyers  
FROM sales  
GROUP BY Description  
ORDER BY buyers ASC  
LIMIT 5

SELECT CustomerID, AVG(TotalPrice) AS avg_order_value  
FROM sales  
GROUP BY CustomerID  
HAVING avg_order_value > 500  
ORDER BY avg_order_value DESC

SELECT strftime('%H', InvoiceDate) AS hour, COUNT(*) AS order_count  
FROM sales  
GROUP BY hour  
ORDER BY order_count DESC  
LIMIT 5

SELECT a.StockCode AS code1, b.StockCode AS code2, COUNT(*) AS count  
FROM sales a  
JOIN sales b ON a.InvoiceNo = b.InvoiceNo AND a.StockCode < b.StockCode  
GROUP BY code1, code2  
ORDER BY count DESC  
LIMIT 10

SELECT Country, Description, AVG(TotalPrice) AS avg_revenue  
FROM sales  
GROUP BY Country, Description  
ORDER BY avg_revenue DESC  
LIMIT 10





