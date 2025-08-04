## /ejercicios-basicos/

### 1️⃣  Listar los nombres de todas las películas disponibles.

```sql
SELECT
	film.title
FROM film
ORDER BY title ASC;
```
Validación de datos - Coincidencia de cantidad de películas obtenidas con la cantidad de film_id de la tabla.

```sql
SELECT
	COUNT(film_id)
FROM film;
```

### 2️⃣ Mostrar el nombre y apellido de todos los clientes registrados.

```sql
SELECT 
	customer.first_name,
    customer.last_name
FROM customer
ORDER BY 1;
```
Validación de datos - Coincidencia de cantidad de nombres de clientes obtenidos con la cantidad de customer_id de la tabla.

```sql
SELECT
	COUNT(customer_id)
FROM customer;
```

### 3️⃣ Obtener los títulos de las películas que duran más de 120 minutos.

```sql
SELECT
	film.title
FROM film
WHERE film.length >= 120
ORDER BY film.title;
```
Validación de datos - Coincidencia de cantidad de títulos de películas obtenidos con la cantidad de filas de duración >= 120.

```sql
SELECT 
	COUNT(film.length)
FROM film
WHERE film.length >= 120;
```

### 4️⃣ Listar los actores que trabajaron en más de 25 películas. Mostrar su nombre, apellido y cantidad.

```sql
SELECT 
  actor.actor_id,
  actor.first_name AS nombre,
  actor.last_name AS apellido,
  COUNT(film_actor.film_id) AS cant_pelis
FROM actor
INNER JOIN film_actor ON actor.actor_id = film_actor.actor_id
GROUP BY actor.actor_id, nombre, apellido
HAVING COUNT(film_actor.film_id) > 25
ORDER BY actor.actor_id ASC;
```

### 5️⃣ Mostrar los nombres de los clientes que realizaron su primer alquiler después del 1 de enero de 2006.

```sql
SELECT
  customer.customer_id,
  customer.first_name AS nombre,
  customer.last_name AS apellido,
  MIN(DATE(rental.rental_date)) AS primer_alquiler
FROM customer
INNER JOIN rental ON customer.customer_id = rental.customer_id
GROUP BY customer.customer_id, nombre, apellido
HAVING MIN(DATE(rental.rental_date)) > '2006-01-01'
ORDER BY primer_alquiler;
```

### 6️⃣ Listar las 5 ciudades con mayor cantidad de clientes, junto con la cantidad.

```sql
SELECT
  city.city AS ciudad,
  COUNT(DISTINCT customer.customer_id) AS clientes
FROM city
INNER JOIN address ON city.city_id = address.city_id
INNER JOIN customer ON address.address_id = customer.address_id
GROUP BY ciudad
ORDER BY clientes DESC, ciudad
LIMIT 5;
```

### 7️⃣ Mostrar para cada cliente:

* nombre
* apellido
* cantidad de alquileres realizados
* cantidad de pagos mayores a \$5

```sql
WITH pagos AS (
  SELECT
    customer.customer_id,
    customer.first_name,
    customer.last_name,
    COUNT(payment.payment_id) AS pagos_mayores
  FROM customer
  INNER JOIN payment ON customer.customer_id = payment.customer_id
  WHERE payment.amount > 5
  GROUP BY customer.customer_id, customer.first_name, customer.last_name
),
alquileres AS (
  SELECT  
    customer.customer_id,
    customer.first_name,
    customer.last_name,
    COUNT(rental.rental_id) AS cant_alquileres
  FROM customer
  INNER JOIN rental ON customer.customer_id = rental.customer_id
  GROUP BY customer.customer_id, customer.first_name, customer.last_name
)
SELECT
  pagos.first_name AS nombre,
  pagos.last_name AS apellido,
  alquileres.cant_alquileres,
  pagos.pagos_mayores
FROM pagos
LEFT JOIN alquileres ON pagos.customer_id = alquileres.customer_id
ORDER BY cant_alquileres DESC;
```

### 8️⃣ Mostrar las películas cuyo promedio de monto de pago por alquiler supera los \$3.50.

```sql
SELECT
  film.title AS pelicula,
  ROUND(AVG(payment.amount), 2) AS promedio
FROM film
LEFT JOIN inventory ON film.film_id = inventory.film_id
LEFT JOIN rental ON inventory.inventory_id = rental.inventory_id
LEFT JOIN payment ON rental.rental_id = payment.rental_id
GROUP BY pelicula
HAVING AVG(payment.amount) > 3.50
ORDER BY promedio;
```

### 9️⃣ Mostrar para cada categoría:

* nombre
* total recaudado por películas de esa categoría

```sql
SELECT
  category.name AS categoria,
  SUM(payment.amount) AS recaudado
FROM category
LEFT JOIN film_category ON category.category_id = film_category.category_id
LEFT JOIN inventory ON film_category.film_id = inventory.film_id
LEFT JOIN rental ON inventory.inventory_id = rental.inventory_id
LEFT JOIN payment ON rental.rental_id = payment.rental_id
GROUP BY categoria
ORDER BY recaudado;
```

### 🔟 Listar las 10 películas más alquiladas junto a su cantidad de alquileres, usando `ROW_NUMBER()` para numerarlas de mayor a menor.

```sql
WITH alquileres AS (
  SELECT
    film.title AS pelicula,
    COUNT(rental.rental_id) AS alquileres
  FROM film
  LEFT JOIN inventory ON film.film_id = inventory.film_id
  LEFT JOIN rental ON inventory.inventory_id = rental.inventory_id
  GROUP BY film.title
)
SELECT
  ROW_NUMBER() OVER (ORDER BY alquileres DESC) AS ranking,
  alquileres.pelicula,
  alquileres.alquileres
FROM alquileres
LIMIT 10;
```
