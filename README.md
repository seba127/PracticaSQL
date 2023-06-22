

# SQL Challenge 2: Pizza Runner 

He querido realizar este desafío de cero para poder adentrarme nuevamente en el mundode SQL, para eso he realizado el ejercicio de cero en donde documentaré los siguientes pasos hasta responder 8 preguntas.

Reglas generales del ejercicio:

https://8weeksqlchallenge.com/case-study-2/

Pasos realizados:
-----------------------------------------------

1- Instalar la aplicacion Postgress

2- Generar la estructura en la BD

-----------------------------------------------
-----------------------------------------------
CREATE SCHEMA pizza_runner;

DROP TABLE IF EXISTS runners;

CREATE TABLE runners (

  runner_id INTEGER,

  registration_date DATE

);

INSERT INTO runners

  (runner_id, registration_date)

VALUES

  (1, '2021-01-01'),

  (2, '2021-01-03'),

  (3, '2021-01-08'),

  (4, '2021-01-15');

DROP TABLE IF EXISTS customer_orders;

CREATE TABLE customer_orders (

  order_id INTEGER,

  customer_id INTEGER,

  pizza_id INTEGER,

  exclusions VARCHAR(4),

  extras VARCHAR(4),

  order_time TIMESTAMP
  
);

INSERT INTO customer_orders

  (order_id, customer_id, pizza_id, exclusions, extras, order_time)

VALUES

  ('1', '101', '1', '', '', '2020-01-01 18:05:02'),

  ('2', '101', '1', '', '', '2020-01-01 19:00:52'),

  ('3', '102', '1', '', '', '2020-01-02 23:51:23'),

  ('3', '102', '2', '', NULL, '2020-01-02 23:51:23'),

  ('4', '103', '1', '4', '', '2020-01-04 13:23:46'),

  ('4', '103', '1', '4', '', '2020-01-04 13:23:46'),

  ('4', '103', '2', '4', '', '2020-01-04 13:23:46'),

  ('5', '104', '1', 'null', '1', '2020-01-08 21:00:29'),

  ('6', '101', '2', 'null', 'null', '2020-01-08 21:03:13'),

  ('7', '105', '2', 'null', '1', '2020-01-08 21:20:29'),

  ('8', '102', '1', 'null', 'null', '2020-01-09 23:54:33'),

  ('9', '103', '1', '4', '1, 5', '2020-01-10 11:22:59'),

  ('10', '104', '1', 'null', 'null', '2020-01-11 18:34:49'),

  ('10', '104', '1', '2, 6', '1, 4', '2020-01-11 18:34:49');

DROP TABLE IF EXISTS runner_orders;

CREATE TABLE runner_orders (

  order_id INTEGER,

  runner_id INTEGER,

  pickup_time VARCHAR(19),

  distance VARCHAR(7),

  duration VARCHAR(10),

  cancellation VARCHAR(23)
  
);

INSERT INTO runner_orders

  (order_id, runner_id, pickup_time, distance, duration, cancellation)

VALUES

  ('1', '1', '2020-01-01 18:15:34', '20km', '32 minutes', ''),

  ('2', '1', '2020-01-01 19:10:54', '20km', '27 minutes', ''),

  ('3', '1', '2020-01-03 00:12:37', '13.4km', '20 mins', NULL),

  ('4', '2', '2020-01-04 13:53:03', '23.4', '40', NULL),

  ('5', '3', '2020-01-08 21:10:57', '10', '15', NULL),

  ('6', '3', 'null', 'null', 'null', 'Restaurant Cancellation'),

  ('7', '2', '2020-01-08 21:30:45', '25km', '25mins', 'null'),

  ('8', '2', '2020-01-10 00:15:02', '23.4 km', '15 minute', 'null'),

  ('9', '2', 'null', 'null', 'null', 'Customer Cancellation'),

  ('10', '1', '2020-01-11 18:50:20', '10km', '10minutes', 'null');


DROP TABLE IF EXISTS pizza_names;

CREATE TABLE pizza_names (

  pizza_id INTEGER,

  pizza_name TEXT

);

INSERT INTO pizza_names

  (pizza_id, pizza_name)

VALUES

  (1, 'Meatlovers'),

  (2, 'Vegetarian');

DROP TABLE IF EXISTS pizza_recipes;

CREATE TABLE pizza_recipes (

  pizza_id INTEGER,

  toppings TEXT
);

INSERT INTO pizza_recipes

  (pizza_id, toppings)

VALUES

  (1, '1, 2, 3, 4, 5, 6, 8, 10'),

  (2, '4, 6, 7, 9, 11, 12');

DROP TABLE IF EXISTS pizza_toppings;

CREATE TABLE pizza_toppings (

  topping_id INTEGER,

  topping_name TEXT
);

INSERT INTO pizza_toppings

  (topping_id, topping_name)

VALUES

  (1, 'Bacon'),

  (2, 'BBQ Sauce'),

  (3, 'Beef'),

  (4, 'Cheese'),

  (5, 'Chicken'),

  (6, 'Mushrooms'),

  (7, 'Onions'),

  (8, 'Pepperoni'),

  (9, 'Peppers'),

  (10, 'Salami'),

  (11, 'Tomatoes'),

  (12, 'Tomato Sauce');
  
--------------------------------------

3- GENERAR LIMPIEZA SENCILLA DE DATOS:

Después de identificar los problemas con las tablas, esta es la etapa donde se tratan los problemas de calidad de los datos.

Para la customer_orderstabla: Los problemas de calidad de datos observados son

En la exclusionscolumna, la presencia de 'null' y ' ' en lugar de NULL

En la extrascolumna, la presencia de 'null' y ' ' en lugar de NULL

-columna exclusiones 

UPDATE customer_orders 

SET exclusiones =  CASE  WHEN exclusiones =  ''  THEN  NULL  

                 WHEN exclusiones =  'null'  THEN  NULL  

                 ELSE exclusiones END ; 

-columna extras 

ACTUALIZAR pedidos_clientes 

SET extras =  CASE  WHEN extras =  ''  THEN  NULL  

             WHEN extras =  'null'  THEN NULL  

             ELSE extras END ;



Para la runner_orderstabla: Los problemas de calidad de datos observados son

la columna pickup_time tiene valores 'nulos'

la columna distance tiene la apariencia de 'km', valores 'nulos' y 'km'

la columna durationc tiene la apariencia de 'minutos', 'null', 'mins', 'minute'

la columna cancellation tiene la apariencia de 'null' y ' '

Necesito cambiar los tipos de datos de las columnas corregidas

-------------------------------------------------
-------------------------------------------------
UPDATE runner_orders 

SET pickup_time =  CASE  WHEN pickup_time =  'null'  THEN  NULL  

                  ELSE pickup_time END ; 
                    
ACTUALIZAR runner_orders 

SET distancia =  CASO  CUANDO distancia =  'nulo'  ENTONCES NULO CUANDO  distancia LIKE ' 

%km' LUEGO RECORTAR (REEMPLAZAR(distancia, 'km' , '' )) ELSE distancia FIN ; ACTUALIZAR 

runner_orders 
                  
                  
            

SET duración =  CASO  CUANDO duración LIKE  '%min%'  THEN  LEFT (duración, 2 ) 

               CUANDO duración =  'nulo'  THEN  NULL  

               ELSE duración END ; 
            
ACTUALIZAR runner_orders 

SET cancelación =  CASO  CUANDO cancelación EN ( 'null' , '' ) THEN  NULL  

                   ELSE cancelación END ;



Necesito verificar el tipo de datos de la columna.

---------------------------------------
---------------------------------------

-- Para cambiar el tipo de datos de las columnas 

ALTER  TABLE runner_orders 

MODIFY COLUMN pickup_time DATETIME; 

ALTER  TABLE runner_orders 

MODIFY COLUMN distancia INT ; 

ALTER  TABLE runner_orders 

MODIFY COLUMN duración INT ;


4- Responder las siguientes consultas:

1. ¿Cuántas pizzas se ordenaron?


SELECT COUNT(pizza_id) AS Cantidad_de_pizzas

FROM customer_orders;

Respuesta:

Cantidad_de_pizzas
14

2. ¿Cuántos pedidos únicos de clientes se realizaron?

SELECT COUNT(distinct order_id) AS cantidad_pizza_unica

FROM customer_orders;

Respuesta:

cantidad_pizza_unica 
10

3. ¿Cuántos pedidos exitosos entregó cada corredor?


SELECT runner_id, 

COUNT(order_id) AS order_count

FROM runner_orders_post

WHERE duration_mins IS NOT NULL

GROUP BY runner_id;

Respuesta:

runner_id	order_count
1		      4
2		      3
3		      1

4. ¿Cuántas pizzas de cada tipo se entregaron?

WITH pizza_counter AS (  
SELECT

    c.order_id,
    COUNT(c.pizza_id) AS pizza_count 

FROM cust_orders c

LEFT JOIN runner_orders_post r 

    ON c.order_id = r.order_id

WHERE duration_mins IS NOT NULL

GROUP BY c.order_id

)
  
SELECT 

    SUM(pizza_count) AS total_pizzas

FROM pizza_counter;

Respuesta:

total_pizzas 

12

5. ¿Cuántos vegetarianos y carnivoros ordenó cada cliente?

WITH pizza_counter_1 AS (

SELECT 

    customer_id,
    COUNT(pizza_id) AS pizza_count

FROM customer_orders

WHERE pizza_id = 1

GROUP BY customer_id

),

pizza_counter_2 AS (

SELECT

    customer_id,
    COUNT(pizza_id) AS pizza_count
FROM customer_orders

WHERE pizza_id = 2

GROUP BY customer_id

)

SELECT DISTINCT 

    pc1.customer_id,
    pc1.pizza_count AS total_carnivoros,
    pc2.pizza_count AS total_vegetarianos

FROM pizza_counter_1 pc1, pizza_counter_2 pc2

ORDER BY 1;

Respuesta:

customer_id	total_carnivoros	total_vegetarianos

101		      2		               1

102		      2		               1

103		      3		               1

104		      3		               1

6. ¿Cuál fue el número máximo de pizzas entregadas en un solo pedido?

SELECT

    order_id,
    COUNT(pizza_id) AS total_pizzas

FROM customer_orders

GROUP BY order_id

ORDER BY total_ordenes DESC;

Respuesta:

order_id		total_ordenes

4		        3

3		        2

10		      2

1		        1

2		        1

5		        1

6		        1

7		        1

8	        	1

9		        1

7. Para cada cliente, ¿cuántas pizzas entregadas tenían al menos 1 cambio y cuántas no tenían cambios?

WITH pizza_changes_counter AS (

SELECT 

    co.customer_id,
    CASE 

        WHEN co.exclusions_cleaned LIKE '%' OR co.extras_cleaned LIKE '%' THEN 1
        ELSE 0

    END AS pizza_change_count,
    CASE

        WHEN co.exclusions_cleaned IS NULL AND co.extras_cleaned IS NULL THEN 1
        WHEN co.exclusions_cleaned IS NULL AND co.extras_cleaned = 'NaN' THEN 1
        ELSE 0

    END AS pizza_no_change_count

FROM cust_orders co

LEFT JOIN runner_orders_post ro 

    ON co.order_id = ro.order_id

WHERE ro.duration_mins IS NOT NULL

)
  
SELECT

    customer_id,
    SUM(pizza_change_count) AS total_pizzas_con_ cambios,
    SUM(pizza_no_change_count) AS total_pizzas_sin_cambios

FROM pizza_changes_counter

GROUP BY customer_id;

Respuesta:

customer_id	total_pizzas_con_ cambios	total_pizzas_sin_cambios

101		       0				          2

102		       1				          2

103		       3				          0

104		       3				          0

105		       1				          0

8. How many pizzas were delivered that had both exclusions and extras?
Query #8

WITH pizzas_changes_counter AS (

SELECT

    co.customer_id,
    CASE 

        WHEN co.exclusions_cleaned LIKE '%' AND co.extras_cleaned LIKE '%' THEN 1

    ELSE 0
    END AS pizza_change_count

FROM cust_orders co

LEFT JOIN runner_orders_post ro 

    ON co.order_id = ro.order_id

WHERE ro.duration_mins IS NOT NULL

)

SELECT 

    customer_id,

  	SUM(pizza_change_count) AS pizzas_con_cambios

FROM pizzas_changes_counter

GROUP BY customer_id

ORDER BY pizzas_con_cambios DESC;

Respuesta:
customer_id	pizzas_con_cambios

104		      1

101		      0

102		      0

103		      0

105		      0

## Authors

- [@seba127](https://www.github.com/seba127)

