## /ejercicios-basicos/

### 1️⃣  Listar los nombres de todas las películas disponibles.

```sql
SELECT
	film.title
FROM film
ORDER BY title ASC;
```
Consulta para validar cantidad total de películas.

```sql
SELECT
	COUNT(film_id) AS total_peliculas
FROM film;
```

### 2️⃣ Mostrar el nombre y apellido de todos los clientes registrados.

```sql
SELECT 
	customer.first_name,
	customer.last_name
FROM customer
ORDER BY last_name, first_name;
```
Consulta para validar cantidad total de clientes:
Esta consulta cuenta el total de clientes registrados para comparar con la cantidad de filas obtenidas.

```sql
SELECT
	COUNT(customer_id) AS total_clientes
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
Consulta para validar cantidad de películas con duración >= 120 minutos:
Cuenta cuántas películas tienen una duración mayor o igual a 120 minutos para validar la consulta principal.

```sql
SELECT 
	COUNT(film.length) AS total_peliculas_120
FROM film
WHERE film.length >= 120;
```

### 4️⃣ Listar el nombre de todas las ciudades que comienzan con la letra ‘A’.

```sql
SELECT
	city.city
FROM city
WHERE city.city LIKE 'A%'
ORDER BY city.city;
```
Consulta para validar cantidad de ciudades que comienzan con la letra A:

```sql
SELECT 
	COUNT(city.city_id) AS total_ciudades_A
FROM city
WHERE city.city LIKE 'A%';
```

### 5️⃣ Mostrar los nombres de los actores que tienen un apellido que termina en 'S'.

```sql
SELECT
	actor.first_name AS nombre,
	actor.last_name AS apellido
FROM actor
WHERE last_name LIKE '%S'
ORDER BY first_name;
```
Consulta para validar cantidad de actores que su apellido termina en S:

```sql
SELECT
	COUNT(actor.actor_id) AS total_apellidos_s
FROM actor
WHERE actor.last_name LIKE '%S';
```

### 6️⃣ Obtener el título y la descripción de todas las películas que contengan la palabra 'ACTION' en su descripción.

```sql
SELECT 
	film.title AS pelicula,
    film.description
FROM film
WHERE film.description LIKE '%ACTION%'
ORDER BY pelicula;
```
Consulta para validar cantidad de películas que en el campo descripción contengan la parabra 'ACTION':

```sql
SELECT
	COUNT(film_id) AS peliculas_action
FROM film
WHERE film.description LIKE '%ACTION%';
```



