Tanda de ejercicios SQL — Nivel avanzado (base de datos Sakila)

## /ejercicios-intermedios/

### 1️⃣️ Mostrar para cada categoría de película:

-nombre de la categoría

-cantidad total de películas en esa categoría

-cantidad de películas disponibles actualmente (inventory disponible)

```sql
WITH stock_no_disp AS (
SELECT
      category.name AS categoria,
    	COUNT(DISTINCT inventory.inventory_id) AS pelis_no_disponibles
FROM category
INNER JOIN film_category ON category.category_id=film_category.category_id
INNER JOIN inventory ON film_category.film_id=inventory.film_id
INNER JOIN rental ON inventory.inventory_id=rental.inventory_id
WHERE rental.return_date IS NULL
GROUP BY category.name),
stock AS (
SELECT
	  category.name AS categoria,
    COUNT(DISTINCT inventory.inventory_id) AS inv_pelis
FROM category
INNER JOIN film_category ON category.category_id=film_category.category_id
INNER JOIN inventory ON film_category.film_id=inventory.film_id
GROUP BY category.name
)
SELECT
	  stock.categoria AS categorias,
    stock.inv_pelis,
    stock.inv_pelis-stock_no_disp.pelis_no_disponibles AS pelis_disponibles
FROM stock
LEFT JOIN stock_no_disp ON stock.categoria=stock_no_disp.categoria
ORDER BY categorias ASC;
```

2️⃣ Listar los actores que trabajaron en más de 25 películas. Mostrar su nombre, apellido y cantidad.

```sql
 SELECT 
	  actor.actor_id,
	  actor.first_name AS nombre,
    actor.last_name AS apellido,
    COUNT(film_actor.film_id) AS cant_pelis
FROM actor
INNER JOIN film_actor ON actor.actor_id=film_actor.actor_id
GROUP BY actor.actor_id, nombre, apellido
HAVING COUNT(film_actor.film_id) > 25
ORDER BY 1 ASC;
```

3️⃣ Mostrar los nombres de los clientes que realizaron su primer alquiler después del 1 de enero de 2006.

```sql
SELECT
	  customer.customer_id,
	  customer.first_name AS nombre,
    customer.last_name AS apellido,
	  MIN(DATE(rental.rental_date)) AS primer_alquiler
FROM customer
INNER JOIN rental ON customer.customer_id=rental.customer_id
GROUP BY customer.customer_id, nombre, apellido
HAVING MIN(DATE(rental.rental_date)) > '2006-01-01'
ORDER BY primer_alquiler;
```

4️⃣ Mostrar pomedio de alquileres por categoría de película.

```sql
WITH alq_por_pelicula AS (
	SELECT
		film.film_id,
		COUNT(rental.rental_id) AS total_alquileres
FROM film
LEFT JOIN inventory ON film.film_id=inventory.film_id
LEFT JOIN rental ON inventory.inventory_id=rental.inventory_id
GROUP BY film.film_id),
categoria_alq AS (
	SELECT
		film.film_id,
        category.name AS categoria
FROM film
JOIN film_category ON film.film_id=film_category.film_id
JOIN category ON film_category.category_id=category.category_id
GROUP BY film.film_id, category.name)
SELECT
		categoria_alq.categoria AS categoria,
        ROUND(AVG(alq_por_pelicula.total_alquileres)) AS promedio_alq
FROM categoria_alq
JOIN alq_por_pelicula ON categoria_alq.film_id=alq_por_pelicula.film_id
GROUP BY categoria_alq.categoria
ORDER BY categoria_alq.categoria;
```
