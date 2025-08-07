Tanda de 10 ejercicios SQL — Nivel Intermedio (base de datos Sakila)

## /ejercicios-intermedios/

### 1️⃣ Listar los nombres de todos los clientes junto con el nombre de la ciudad en la que viven.

```sql
SELECT 
    	customer.first_name AS nombre, 
    	customer.last_name AS apellido, 
    	city.city AS ciudad
FROM customer
INNER JOIN address ON customer.address_id=address.address_id
INNER JOIN city ON city.city_id=address.city_id
ORDER BY apellido, nombre;
```
Consulta para validar cantidad de clientes con ciudad asociada:
Cuenta cuántos clientes tienen asociada una ciudad correctamente (debe coincidir con el total de clientes registrados).

```sql
SELECT
	  COUNT(customer.customer_id) AS total_clientes
FROM customer;
```

### 2️⃣ Mostrar los nombres y apellidos de todos los actores y las películas en las que participaron.

```sql
SELECT 
    	actor.first_name AS nombre, 
    	actor.last_name AS apellido, 
    	film.title AS pelicula
FROM actor
INNER JOIN film_actor ON actor.actor_id=film_actor.actor_id
INNER JOIN film ON film.film_id=film_actor.film_id
ORDER BY actor.last_name, actor.first_name;
```

### 3️⃣ Obtener los títulos de las películas y el nombre de su categoría.

```sql
SELECT 
    	film.title AS pelicula, 
   	category.name AS categoria
FROM film
INNER JOIN film_category ON film.film_id=film_category.film_id
INNER JOIN category ON film_category.category_id=category.category_id
ORDER BY pelicula;
```

### 4️⃣ Listar los nombres de las categorías y la cantidad total de películas que tiene cada una.

```sql
SELECT 
	  category.name AS categoria_peli, 
	  COUNT(film_category.film_id) AS cant_pelis 
FROM category
INNER JOIN film_category ON category.category_id=film_category.category_id
GROUP BY category.name
ORDER BY cant_pelis DESC;
```

### 5️⃣ Mostrar los nombres de los clientes que hayan alquilado una película de la categoría 'Comedy'.

```sql
SELECT DISTINCT
	customer.first_name AS nombre, 
	customer.last_name AS apellido, 
	category.name AS categoria 
FROM customer
INNER JOIN payment ON customer.customer_id=payment.customer_id
INNER JOIN rental ON payment.rental_id=rental.rental_id
INNER JOIN inventory ON rental.inventory_id=inventory.inventory_id
INNER JOIN film ON inventory.film_id=film.film_id
INNER JOIN film_category ON film.film_id=film_category.film_id
INNER JOIN category ON film_category.category_id=category.category_id
WHERE category.name='Comedy'
ORDER BY apellido, nombre;
```

### 6️⃣ Listar los nombres de los clientes que hayan alquilado más de 3 películas distintas en total. Mostrar nombre, apellido y cantidad de películas alquiladas.

```sql
SELECT
	customer.first_name AS nombre,
	customer.last_name AS apellido,
    	COUNT(DISTINCT film.film_id) AS pelis_alquiladas
FROM customer
INNER JOIN payment ON customer.customer_id=payment.customer_id
INNER JOIN rental ON payment.rental_id=rental.rental_id    
INNER JOIN inventory ON rental.inventory_id=inventory.inventory_id
INNER JOIN film ON inventory.film_id=film.film_id
GROUP BY 1, 2 
HAVING COUNT(DISTINCT film.film_id) > 3
ORDER BY 3 DESC , 2, 1;
```

### 7️⃣ Mostrar las 5 categorías con mayor cantidad de películas disponibles en inventario (es decir, que tengan copias cargadas). Mostrar nombre de categoría y cantidad de películas.

```sql
 SELECT  
	category.name AS categoria,
	COUNT(DISTINCT inventory.inventory_id) AS cantidad_pelis
FROM category
INNER JOIN film_category ON category.category_id=film_category.category_id
INNER JOIN film ON film_category.film_id=film.film_id
INNER JOIN inventory ON film.film_id=inventory.film_id
GROUP BY 1
ORDER BY 2 DESC
LIMIT 5;
```

### 8️⃣ Obtener los nombres de los clientes que hayan realizado al menos 2 pagos por montos mayores a 5.00. Mostrar nombre, apellido y cantidad de pagos.

```sql
SELECT 
	customer.first_name AS nombre,
    	customer.last_name AS apellido,	
    	COUNT(payment.payment_id) AS cant_pagos
FROM customer 
INNER JOIN payment ON customer.customer_id=payment.customer_id
WHERE payment.amount > 5.00
GROUP BY 1,2
HAVING COUNT(payment.payment_id) >= 2
ORDER BY cant_pagos DESC;
```

### 9️⃣ Listar los nombres de los actores que trabajaron en películas de más de 3 categorías distintas. Mostrar nombre, apellido y cantidad de categorías.

```sql
SELECT
	actor.first_name AS nombre,
    	actor.last_name AS apellido,
    	COUNT(DISTINCT film_category.category_id) AS cantidad_categorias
FROM actor
INNER JOIN film_actor ON actor.actor_id=film_actor.actor_id
INNER JOIN film ON film_actor.film_id=film.film_id
INNER JOIN film_category ON film.film_id=film_category.film_id
INNER JOIN category ON film_category.category_id=category.category_id
GROUP BY nombre, apellido
HAVING COUNT(DISTINCT film_category.category_id) > 3
ORDER BY cantidad_categorias DESC;
```

### 🔟 Mostrar los nombres de las ciudades que tienen más de 5 clientes registrados, ordenadas de mayor a menor cantidad de clientes.

```sql
SELECT 
	city.city AS ciudad,
	COUNT(DISTINCT customer.customer_id) AS cantidad_clientes
FROM city
INNER JOIN address ON city.city_id=address.city_id
INNER JOIN customer ON address.address_id=customer.address_id
GROUP BY city.city
HAVING COUNT(DISTINCT customer.customer_id) > 5
ORDER BY cantidad_clientes DESC;
```
