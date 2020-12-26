---
title: Example queries on the Sakila MySQL database
description: A small collection of example MySQL queries on the Sakila database
first-published: 2015-07-25
Last Updated: 2015-07-25
tags:
- MySQL
---

<a href='http://dev.mysql.com/doc/sakila/en/index.html'><b>Sakila</b></a> is a 
sample database provided by MySQL, meant to be used in tests and documentation 
examples. This page lists some example queries against that database.


Get a non-normalized selection of actors and films in which they played:

```sql
SELECT  actor.first_name, 
        actor.last_name, 
        film.title 
FROM actor, film, film_actor 
WHERE film_actor.actor_id = actor.actor_id 
AND film_actor.film_id = film.film_id;
```

Get the same result with double `INNER JOIN`:

```sql
SELECT actor.first_name, actor.last_name, film.title 
FROM film_actor 
INNER JOIN actor ON film_actor.actor_id = actor.actor_id 
INNER JOIN film ON film_actor.film_id = film.film_id;
```
