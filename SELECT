# SQL-Youtube
SQL Practical

SELECT ship_city, order_date
FROM orders
WHERE ship_city = 'London'
ORDER BY order_date;

SELECT MIN(order_date)
FROM orders
WHERE ship_city = 'London';

SELECT SUM(units_in_stock)
FROM products
WHERE discontinued <> 1;

SELECT last_name, first_name
FROM employees
WHERE last_name LIKE '_uch%';

SELECT product_name, unit_price
FROM products
LIMIT 10;

SELECT ship_city, ship_region, ship_country
FROM orders
WHERE ship_region IS NOT NULL;

SELECT ship_country, COUNT(*)
FROM orders
WHERE freight > 50
GROUP BY ship_country
ORDER BY COUNT(*) DESC;

SELECT category_id, SUM(units_in_stock)
FROM products
GROUP BY category_id
ORDER BY SUM(units_in_stock)DESC;

SELECT category_id, SUM(unit_price * units_in_stock)
FROM products
WHERE discontinued <> 1
GROUP BY category_id
HAVING SUM(unit_price * units_in_stock) > 5000
ORDER BY SUM(unit_price * units_in_stock);

SELECT country
FROM customers
UNION
SELECT country
FROM employees;

SELECT country
FROM customers
INTERSECT
SELECT country
FROM suppliers;

SELECT country
FROM customers
EXCEPT
SELECT country
FROM suppliers;

SELECT product_name, suppliers.company_name, units_in_stock
FROM products
INNER JOIN suppliers ON products.supplier_id = suppliers.supplier_id
ORDER BY units_in_stock DESC;

SELECT category_name, SUM(units_in_stock)
FROM products
INNER JOIN categories  ON products.category_id = categories.category_id
GROUP BY category_name
ORDER BY SUM(units_in_stock) DESC
LIMIT 5;

SELECT category_name, SUM(unit_price * units_in_stock)
FROM products
INNER JOIN categories ON products.category_id = categories.category_id
WHERE discontinued <> 1
GROUP BY category_name
HAVING SUM(unit_price * units_in_stock) > 5000
ORDER BY SUM(unit_price * units_in_stock) DESC;

SELECT order_id, customer_id, first_name, last_name, title
FROM orders
INNER JOIN employees ON orders.employee_id = employees.employee_id;

SELECT order_date, product_name, ship_country, products.unit_price, quantity, discount
FROM orders
INNER JOIN order_details ON orders.order_id = order_details.order_id
INNER JOIN products ON order_details.product_id = products.product_id;

SELECT contact_name, company_name, phone, first_name, last_name, title, order_date, product_name, ship_country, products.unit_price, quantity, discount
FROM orders
JOIN order_details ON orders.order_id = order_details.order_id
INNER JOIN products ON order_details.product_id = products.product_id
JOIN customers ON orders.customer_id = customers.customer_id
JOIN employees ON orders.employee_id = employees.employee_id
WHERE ship_country = 'UK';

SELECT company_name, order_id
FROM customers
LEFT JOIN orders ON orders.customer_id = customers.customer_id
WHERE order_id IS NULL;

SELECT last_name, order_id
FROM employees 
LEFT JOIN orders ON orders.employee_id = employees.employee_id
WHERE order_id IS NULL;

SELECT contact_name, company_name, phone, first_name, last_name, title, order_date, product_name, ship_country, products.unit_price, quantity, discount
FROM orders
JOIN order_details USING(order_id) -- ON orders.order_id = order_details.order_id
JOIN products USING(product_id) -- ON order_details.product_id = products.product_id
JOIN customers USING(customer_id) -- ON orders.customer_id = customers.customer_id
JOIN employees USING(employee_id) -- ON orders.employee_id = employees.employee_id
WHERE ship_country = 'UK';

SELECT order_id, customer_id, first_name, last_name, title
FROM orders
NATURAL JOIN employees;

SELECT COUNT(*) AS emplo_count
FROM employees;

SELECT category_id, SUM(units_in_stock) AS units_in_stock
FROM products
GROUP BY category_id
ORDER BY units_in_stock DESC
LIMIT 5;

SELECT c.company_name, CONCAT(e.first_name,' ', e.last_name)
FROM orders AS o
JOIN customers AS c USING(customer_id)
JOIN employees AS e USING (employee_id)
JOIN shippers AS s ON o.ship_via = s.shipper_id
WHERE c.city = 'London' AND e.city = 'London' AND s.company_name = 'Speedy Express';

SELECT product_name, units_in_stock, contact_name, phone
FROM products
JOIN categories USING(category_id)
JOIN suppliers USING(supplier_id)
WHERE category_name IN('Beverages', 'Seafood') AND discontinued = 0 AND units_in_stock <20
ORDER BY units_in_stock;

SELECT contact_name, order_id
FROM orders
RIGHT JOIN customers USING(customer_id)
WHERE order_id IS NULL
ORDER BY contact_name;

SELECT company_name
FROM suppliers
WHERE country IN (SELECT DISTINCT country
     FROM customers);
     
SELECT DISTINCT suppliers.company_name
FROM suppliers
JOIN customers USING(country);

SELECT category_name, SUM(units_in_stock)
FROM products
INNER JOIN categories USING(category_id)
GROUP BY category_name
ORDER BY SUM(units_in_stock) DESC
LIMIT (SELECT MIN(product_id)+4 FROM products);

SELECT AVG(units_in_stock)
FROM products;

SELECT product_name, units_in_stock
FROM products
WHERE units_in_stock > (SELECT AVG(units_in_stock)FROM products)
ORDER BY units_in_stock;

SELECT company_name, contact_name
FROM customers
WHERE EXISTS(SELECT customer_id FROM orders
   WHERE customer_id = customers.customer_id
   AND freight BETWEEN 50 AND 100);


-- вывести продукты, которые не заказывались в период между теми датами
SELECT product_name
FROM products
WHERE NOT EXISTS (SELECT orders.order_id FROM orders
     JOIN order_details USING(order_id)
     WHERE order_details.product_id = product_id
     AND order_date BETWEEN '1995-02-01' AND '1995-02-15');
     
-- выбрать все уникальные компании заказчиков, которые делали заказы более 40 единиц товаров
SELECT DISTINCT company_name
FROM customers
JOIN orders USING(customer_id)
JOIN order_details USING(order_id)
WHERE quantity > 40;
--аналог
SELECT DISTINCT company_name
FROM customers 
WHERE customer_id = ANY(SELECT customer_id 
      FROM orders 
      JOIN order_details USING(order_id)
      WHERE quantity > 40);

-- такие продукты, количество которых больше среднего по заказакам

SELECT DISTINCT product_name, quantity
FROM products
JOIN order_details USING(product_id)
WHERE quantity > (
 SELECT AVG(quantity)
    FROM order_details);

-- все продукты, количество которых больше среднего значения количества заказанных товаров из групп, полученных группированием по product_id
-- среднее значение количества заказанных товаров разбитых на группы по product_id

SELECT AVG(quantity)
FROM order_details
GROUP BY product_id;

SELECT DISTINCT product_name, quantity
FROM products
JOIN order_details USING(product_id)
WHERE quantity > ALL (SELECT AVG(quantity)
                 FROM order_details
                 GROUP BY product_id)
ORDER BY quantity;

-- вывести продукты, количество которых в продаже меньше самого малого среднего количества продуктов в деталях заказов
SELECT product_name, units_in_stock
FROM products
WHERE units_in_stock < ALL(SELECT AVG(quantity)
FROM order_details
GROUP BY product_id
)
ORDER BY units_in_stock DESC;

--вывести общую сумму фрахтов, для компаний-заказчиков заказов, стоимость фрахтов заказов больше или равно средней величине стоимости фрахтов всеъ заказов

SELECT customer_id, SUM(freight) AS freight_sum
FROM orders 
INNER JOIN (SELECT customer_id, AVG(freight) AS freight_avg
FROM orders
GROUP BY customer_id) oa
USING(customer_id)
WHERE freight > freight_avg AND shipped_date BETWEEN '1996-07-16' AND '1996-07-31'
GROUP BY customer_id
ORDER BY freight_sum;

-- вывести три заказа с наибольшей стоимостью, которые были созданы после первого сентября 1997 года включительно и были доставлены в страны Южной Америки
-- общая стоимость рассчитывается с учетом дисконта
SELECT customer_id, ship_country, order_price
FROM orders
JOIN (SELECT order_id, SUM(unit_price * quantity - unit_price * quantity*discount) AS order_price
FROM order_details
GROUP BY order_id) AS od
USING(order_id)
WHERE ship_country IN('Argentina')
AND order_date >= '1997-09-01'
ORDER BY order_price DESC
LIMIT 3;

-- вывести все товары или уникальные наименования продуктов, которых заказано ровно 10 ед.
SELECT product_name
FROM products
WHERE product_id = ANY
(SELECT product_id FROM order_details WHERE quantity = 10);

SELECT DISTINCT product_name, quantity
FROM products
JOIN order_details USING(product_id)
WHERE quantity = 10;


