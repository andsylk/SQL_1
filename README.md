#### SQL Skills using sakila database

#### Using the sakila database, in MYSQL, I created many SQL queries that analyzed the data.

#### SQL Query Responses 
###### 1a) 

    select first_name, last_name 
    from actor;

###### 1b) 

    select upper(concat(first_name, " ", last_name)) as "Actor Name"
    from actor;

###### 2a) 

    select actor_id, first_name, last_name
    from actor
    where first_name = 'Joe';

###### 2b)

    select * 
    from actor
    where last_name like '%gen%';

###### 2c)

    select *
    from actor 
    where last_name like '%li%'
    order by last_name, first_name;

###### 2d)

    select country_id, country
    from country
    where country in ('Afghanistan', 'Bangladesh', 'China');

###### 3a) 

    alter table actor
    add column middle_name varchar(50) after first_name;

###### 3b) 

    alter table actor
    modify column middle_name blob;

###### 3c)

    alter table actor
    drop column middle_name;

###### 4a)

    select last_name, count(*)
    from actor
    group by last_name;


###### 4b)

    select last_name, count(*)
    from actor
    group by 1
    having count(*) >=2;


###### 4c)

    update actor
    set first_name = 'HARPO'
    where first_name = 'groucho'
    and last_name = 'williams';


###### 4d) 

    update actor
    set first_name = if (first_name = 'harpo' and last_name = 'williams', 'GROUCHO', if(first_name != 'harpo' and last_name = 'williams', 'MUCHO GROUCHO',first_name));

###### 5a)

    show create table sakila.address;


###### 6a)

    select s.first_name, s.last_name, a.address
    from staff s 
    join address a
    on s.address_id = a.address_id;

###### 6b)

    select s.first_name, s.last_name, sum(p.amount) as total_amount
    from staff s
    join payment p
    on s.staff_id = p.staff_id
    where p.payment_date >= '2005-08-01'
    and p.payment_date < '2005-09-01'
    group by s.first_name, s.last_name;


###### 6c)

    select f.title, count(*) as number_of_actors
    from film f
    inner join film_actor fa
    on f.film_id = fa.film_id
    group by f.title;

###### 6d)

    select count(i.film_id) as copies
    from inventory i
    join film f
    on i.film_id = f.film_id
    where f.title = 'Hunchback Impossible';

###### 6e)

    select c.first_name, c.last_name, sum(p.amount) as total_paid
    from customer c
    join payment p
    on c.customer_id = p.customer_id
    group by c.first_name, c.last_name
    order by last_name;
 
###### 7a)

    select title
    from film
    where (title like 'K%' or title like 'Q%')
    and language_id in (select language_id from language where name = 'English');

###### 7b)

    select first_name, last_name
    from actor
    where actor_id in (select actor_id from film_actor where film_id in (select film_id from film where title = 'Alone Trip'));

###### 7c)

    select first_name, last_name, lower(email) as email
    from customer cu
    join address a
    on cu.address_id = a.address_id
    join city ci
    on ci.city_id = a.address_id
    join country co
    on co.country_id = ci.country_id
    where co.country = 'Canada';


###### 7d)

    select f.title
    from film f
    join film_category fc
    on f.film_id = fc.film_id
    join category c
    on fc.category_id = c.category_id
    where c.name = 'Family';


###### 7e)

    select f.title, count(distinct r.rental_id)
    from rental r
    join inventory i
    on r.inventory_id = i.inventory_id
    join film f
    on i.film_id = f.film_id
    group by f.title
    order by 2 desc;


###### 7f)

    select s.store_id, sum(p.amount) as total
    from store s
    join staff sf
    on s.store_id = sf.store_id
    join payment p
    on sf.staff_id = p.staff_id
    group by 1;

###### 7g)

    select s.store_id, c.city, co.country
    from store s
    join address a
    on s.address_id = a.address_id
    join city c
    on a.city_id = c.city_id
    join country co
    on c.country_id = co.country_id;

###### 7h)

    select c.name , sum(p.amount) as revenue
    from category c
    join film_category fc
    on c.category_id = fc.category_id
    join inventory i
    on fc.film_id = i.film_id
    join rental r
    on i.inventory_id = r.inventory_id
    join payment p
    on p.rental_id = r.rental_id
    group by 1
    order by 2 desc
    limit 5;


###### 8a)

    create view top_five_genres as 
    select c.name, sum(p.amount) as revenue
    from category c
    join film_category fc
    on c.category_id = fc.category_id
    join inventory i
    on fc.film_id = i.film_id
    join rental r
    on i.inventory_id = r.inventory_id
    join payment p
    on p.rental_id = r.rental_id
    group by 1
    order by 2 desc
    limit 5;



###### 8b)

    select * from top_five_genres;

###### 8c)

    drop view if exists top_five_genres;
