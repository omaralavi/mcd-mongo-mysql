# Practica Modelo Relacional.

La siguiente practica evalua su conocimiento de consultas con Modelo Relacional.

## Como enviar sus respuesta.

1. Debe crear este archivo en su `fork` con el mismo nombre.
2. Debe copiar el contenido.
3. Debe comenzar a resolver las consultas.
4. Debe guardar el progreso en cada paso, puede cambiar la respuesta las veces que quiera, pero debe guardar el progreso cada vez. (Muy importante).

## Guardar progreso.

Para guardar su progreso cada vez que termine un ejercicio, o cambie una consulta. Ejecuta lo siguiente en un nuevo terminal:

```bash
git add .
git commit -m "Enviando progreso"
git push
```

## Ejercicios:

1. Listar la cantidad de tiendas por ciudad.

Salida:
```
+---------+------------+-------------+
| city_id | city       | store_count |
+---------+------------+-------------+
|     300 | Lethbridge |           1 |
|     576 | Woodridge  |           1 |
+---------+------------+-------------+
```

Respuesta:
```sql
-- Su respuesta aqui:

select city.city_id, city.city, count(store.store_id) as cantidad_sucursal from store, address, city where store.address_id = address.address_id and address.city_id = city.city_id group by 1, 2;

2. Listar la cantidad de películas que se hicieron por lenguaje.

Salida:
```
+-------------+----------+------------+
| language_id | language | film_count |
+-------------+----------+------------+
|           1 | English  |       1000 |
|           2 | Italian  |          0 |
|           3 | Japanese |          0 |
|           4 | Mandarin |          0 |
|           5 | French   |          0 |
|           6 | German   |          0 |
+-------------+----------+------------+
```

Respuesta:
```sql
select language.language_id, language.name, (select count(*) from film where language.language_id=film.language_id ) film_count from language;

```

3.  Seleccionar todos los actores que participaron mas de 35 peliculas.

Salida:

```
+----------+------------+-----------+------------+
| actor_id | first_name | last_name | film_count |
+----------+------------+-----------+------------+
|       23 | SANDRA     | KILMER    |         37 |
|       81 | SCARLETT   | DAMON     |         36 |
|      102 | WALTER     | TORN      |         41 |
|      107 | GINA       | DEGENERES |         42 |
|      181 | MATTHEW    | CARREY    |         39 |
|      198 | MARY       | KEITEL    |         40 |
+----------+------------+-----------+------------+
6 rows in set (0.01 sec)
```

Respuesta:
```sql
-- Su respuesta aqui:

 select actor.actor_id,  actor.first_name, actor.last_name, count(*) from actor, film_actor, film where actor.actor_id= film_actor.actor_id and film_actor.film_id = film.film_id group by 1, 2, 3 having count(*)>35 order by 1, 2, 3;

```

4. Mostrar el listado de los 10 de actores que mas peliculas realizó en la categoria `Comedy`.

Salida:
```
+----------+------------+-----------+-------------------+
| actor_id | first_name | last_name | comedy_film_count |
+----------+------------+-----------+-------------------+
|      196 | BELA       | WALKEN    |                 6 |
|      143 | RIVER      | DEAN      |                 5 |
|      149 | RUSSELL    | TEMPLE    |                 5 |
|       82 | WOODY      | JOLIE     |                 4 |
|      127 | KEVIN      | GARLAND   |                 4 |
|       58 | CHRISTIAN  | AKROYD    |                 4 |
|       24 | CAMERON    | STREEP    |                 4 |
|      129 | DARYL      | CRAWFORD  |                 4 |
|       83 | BEN        | WILLIS    |                 4 |
|       37 | VAL        | BOLGER    |                 4 |
+----------+------------+-----------+-------------------+
```

Respuesta:
```sql
-- Su respuesta aqui:

select actor.actor_id, actor.first_name, actor.last_name, count(actor.actor_id) as comedy_film_count from film, film_actor, film_category, category, actor where film.film_id = film_actor.film_id and film_actor.actor_id=actor.actor_id and film.film_id=film_category.film_id and film_category.category_id = category.category_id and category.name = "Comedy" group by 1,2,3 order by count(actor.actor_id) desc limit 10;

```

5. Obtener la lista de actores que NO han participado en ninguna película de categoría "Comedy":

Salida:
```
+----------+------------+-------------+
| actor_id | first_name | last_name   |
+----------+------------+-------------+
|        3 | ED         | CHASE       |
|        6 | BETTE      | NICHOLSON   |
|        7 | GRACE      | MOSTEL      |
|        8 | MATTHEW    | JOHANSSON   |
|        9 | JOE        | SWANK       |
... Se corta por longitud
|      189 | CUBA       | BIRCH       |
|      190 | AUDREY     | BAILEY      |
|      195 | JAYNE      | SILVERSTONE |
|      200 | THORA      | TEMPLE      |
+----------+------------+-------------+

53 filas total
```
Respuesta:
```sql
-- Su respuesta aqui:

select actor.actor_id, actor.first_name, actor.last_name from actor where actor.actor_id not in(select actor.actor_id from film, film_actor, film_category, category, actor where film.film_id = film_actor.film_id and film_actor.actor_id=actor.actor_id and film.film_id=film_category.film_id and film_category.category_id = category.category_id and category.name = "Comedy" group by 1);

```

