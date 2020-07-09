Joins:
1. select * from invoice join invoice_line 
on invoice_line.invoice_id = invoice.invoice_id
where invoice_line.unit_price >= 1;

2. select invoice.invoice_date, customer.first_name, customer.last_name, invoice.total
from invoice join customer on invoice.customer_id = customer.customer_id;

3. select customer.first_name, customer.last_name, employee.first_name, employee.last_name
from customer join employee on customer.support_rep_id = employee.employee_id;

4. select album.title, artist.name 
from album join artist on album.artist_id = artist.artist_id;

5. select playlist_track.track_id
from playlist_track join playlist 
on playlist.name = 'Music';

6. select track.name 
from track join playlist_track on playlist_track.track_id = track.track_id
where playlist_track.playlist_id = 5;

7. select track.name, playlist.name 
from track join playlist_track on track.track_id = playlist_track.track_id
join playlist on playlist.playlist_id = playlist_track.playlist_id;

8. select track.name, album.title
from track join album on track.album_id = album.album_id
join genre on genre.genre_id = track.genre_id
where genre.genre_id = 4;

Black Diamond:

select track.name, album.title, genre.name, artist.name  
from artist join album on album.artist_id = artist.artist_id
join track on track.album_id = album.album_id
join genre on track.genre_id = genre.genre_id
join playlist_track on playlist_track.track_id = track.track_id
join playlist on playlist.playlist_id = playlist_track.playlist_id
where playlist.name = 'Music';

Nested Queries: 
1. select * from invoice
where invoice_id in ( select invoice_id from invoice_line where unit_price > 0.99 );

2. select * from playlist_track where playlist_id in (select playlist_id from playlist where name = 'Music');

3. select name from track 
where track_id in (select track_id from playlist_track where playlist_id = 5);

4. select * from track 
where genre_id in (select genre_id from genre where name = 'Comedy');

5. select * from track 
where album_id in (select album_id from album where title = 'Fireball');

6. select * from track 
where album_id in (select album_id from album where artist_id in (select artist_id from artist where name = 'Queen'));

Updating Rows:
1. update customer
set fax = null;

2. update customer
set company = 'Shelf'
where company is null;

3. update customer set last_name = 'Thompson' 
where first_name = 'Julia' and last_name = 'Barnett'

4. update customer 
set support_rep_id = 4
where email = 'luisrojas@yahoo.cl';

5. update track
set composer = 'The darkness around us'
where genre_id in (select genre_id from genre where name = 'Metal')
and composer is null;
                   
Group By:
1. select count(*), genre.name
from track join genre on track.genre_id = genre.genre_id
group by genre.name;

2. select count(*), genre.name 
from track join genre on genre.genre_id = track.genre_id
where genre.name = 'Pop' or genre.name = 'Rock'
group by genre.name;

3. select count(*), artist.name
from artist join album on album.artist_id = artist.artist_id
group by artist.name;

Use Distinct:
1. select distinct composer from track;

2. select distinct billing_postal_code from invoice;

3. select distinct company from customer;

Delete Rows:
1. delete from practice_delete
where type = 'bronze';

2. delete from practice_delete
where type = 'silver';

3. delete from practice_delete
where value = 150;

eCommerce: 
--create table users (user_id serial primary key, name varchar(40), email varchar(50));

--create table products (product_id serial primary key, name varchar(50), price int);

--create table orders (order_id serial primary key, 
--product_id int references products(product_id));

--insert into users (name, email) values ('Grant', 'grant@gmail.com');
--insert into users (name, email) values ('Jack', 'jack@gmail.com');
--insert into users (name, email) values ('Jill', 'jill@gmail.com');
--insert into products (name, price) values ('widget', 10);
--insert into products (name, price) values ('gizmo', 50);
--insert into products (name, price) values ('car', 10000);

select * from products where product_id in (select product_id from orders where order_id = 1);
select * from orders;
select price from products where product_id in (select product_id from orders where order_id = 3);

--alter table orders add user_id int references users(user_id);

--update orders set user_id = 3 where order_id = 1;
--update orders set user_id = 2 where order_id = 2;
--update orders set user_id = 1 where order_id = 3;

select * from orders where user_id in (select user_id from users where user_id = 1);

select count(*), users.name 
from orders join users on orders.user_id = users.user_id
group by users.name;

Black Diamond: 

select sum(price), users.name 
from products join orders on products.product_id = orders.product_id
join users on orders.user_id = users.user_id
group by users.name;
