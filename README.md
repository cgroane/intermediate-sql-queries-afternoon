<img src="https://devmounta.in/img/logowhiteblue.png" width="250" align="right">

# Project Summary

In this project, we'll continue to use <a href="http://jxs.me/chinook-web/">Chinook</a> to practice joins, nested queries, updating rows, group by, distinct, and foreign keys.

Any new tables or records that you add into the database will be removed after you refresh the page.

Use <a href="www.sqlteaching.com">SQL Teaching</a> or <a href="www.sqlbolt.com">SQL Bolt</a> as resources for referencing on keywords you'll need.

## Practice joins

### Instructions

<details>

<summary> <code> Syntax Hint </code> </summary>

```sql
SELECT [Column names] 
FROM [Table] [abbv]
JOIN [Table2] [abbv2] ON abbv.prop = abbv2.prop WHERE [Conditions];

SELECT a.Name, b.Name FROM SomeTable a JOIN AnotherTable b ON a.someid = b.someid;
SELECT a.Name, b.Name FROM SomeTable a JOIN AnotherTable b ON a.someid = b.someid WHERE b.email = 'e@mail.com';
```

</details>

<br />

1. Get all invoices where the `UnitPrice` on the `InvoiceLine` is greater than $0.99.
2. Get the `InvoiceDate`, customer `FirstName` and `LastName`, and `Total` from all invoices.
3. Get the customer `FirstName` and `LastName` and the support rep's `FirstName` and `LastName` from all customers. 
    * Support reps are on the Employee table.
4. Get the album `Title` and the artist `Name` from all albums.
5. Get all PlaylistTrack TrackIds where the playlist `Name` is Music.
6. Get all Track `Name`s for `PlaylistId` 5.
7. Get all Track `Name`s and the playlist `Name` that they're on ( 2 joins ).
8. Get all Track `Name`s and Album `Title`s that are the genre `"Alternative"` ( 2 joins ).

select * from invoice
join InvoiceLine on invoice.invoiceId = invoiceLine.invoiceId
where invoiceline.unitprice > .99

select invoice.InvoiceDate, Customer.firstname, customer.lastname, invoice.total
from invoice
join Customer on invoice.customerId = customer.customerid

select * from customer
join employee on employee.employeeId = customer.supportrepid

select album.title, artist.name from album
join artist on artist.artistid = album.artistid

select playlisttrack.trackid from playlisttrack
join playlist on playlist.playlistid = playlisttrack.playlistid
where playlist.name = 'Music'

select track.name from track
join playlisttrack on playlisttrack.trackid = track.trackid
where playlisttrack.playlistid = 5

select track.name, playlist.name as playlist_name from track
join playlisttrack on playlisttrack.trackid = track.trackid
join playlist on playlisttrack.playlistid = playlist.playlistid

select track.name, album.title from track
join album on track.albumid = album.albumid
join genre on track.genreid = genre.genreid
where genre.name = 'Alternative'

select track.name as track_name, genre.name as genre_name, album.title as album_name, artist.name as artist_name
from track
join playlisttrack on track.trackid = playlisttrack.trackid
join playlist on playlisttrack.playlistid = playlist.playlistid
join genre on track.genreId = genre.genreid
join album on track.albumId = album.albumid
join artist on album.artistid = artist.artistid
where playlist.name = 'Music'



### Solution

<details>

<summary> <code> SQL Solutions </code> </summary>

<details>

<summary> <code> #1 </code> </summary>

```sql
SELECT *
FROM Invoice i
JOIN InvoiceLine il ON il.invoiceId = i.invoiceId
WHERE il.UnitPrice > 0.99;
```

</details>

<details>

<summary> <code> #2 </code> </summary>

```sql
SELECT i.InvoiceDate, c.FirstName, c.LastName, i.Total
FROM Invoice i
JOIN Customer c ON i.CustomerId = c.CustomerId;
```

</details>

<details>

<summary> <code> #3 </code> </summary>

```sql
SELECT c.FirstName, c.LastName, e.FirstName, e.LastName
FROM Customer c
JOIN Employee e ON c.SupportRepId = e.EmployeeId;
```

</details>

<details>

<summary> <code> #4 </code> </summary>

```sql
SELECT al.Title, ar.Name
FROM Album al
JOIN Artist ar ON al.ArtistId = ar.ArtistId;
```

</details>

<details>

<summary> <code> #5 </code> </summary>

```sql
SELECT pt.TrackId
FROM PlaylistTrack pt
JOIN Playlist p ON p.PlaylistId = pt.PlaylistId
WHERE p.Name = 'Music';
```

</details>

<details>

<summary> <code> #6 </code> </summary>

```sql
SELECT t.Name
FROM Track t
JOIN PlaylistTrack pt ON pt.TrackId = t.TrackId
WHERE pt.PlaylistId = 5;
```

</details>

<details>

<summary> <code> #7 </code> </summary>

```sql
SELECT t.name, p.Name
FROM Track t
JOIN PlaylistTrack pt ON t.TrackId = pt.TrackId
JOIN Playlist p ON pt.PlaylistId = p.PlaylistId;
```

</details>

<details>

<summary> <code> #8 </code> </summary>

```sql
SELECT t.Name, a.title
FROM Track t
JOIN Album a ON t.AlbumId = a.AlbumId
JOIN Genre g ON g.GenreId = t.GenreId
WHERE g.Name = "Alternative";
```

</details>

</details>

### Black Diamond

* Get all tracks on the playlist(s) called Music and show their name, genre name, album name, and artist name.
  * At least 5 joins.


## Practice nested queries

### Summary

Complete the instructions without using any joins. Only use nested queries to come up with the solution.

### Instructions

<details>

<summary> <code> Syntax Hint </code> </summary>

```sql
SELECT [Column names] 
FROM [Table] 
WHERE ColumnId IN ( SELECT ColumnId FROM [Table2] WHERE [Condition] );

SELECT Name, Email FROM Athlete WHERE AthleteId IN ( SELECT PersonId FROM PieEaters WHERE Flavor='Apple' );
```

</details>

<br />

1. Get all invoices where the `UnitPrice` on the `InvoiceLine` is greater than $0.99.
2. Get all Playlist Tracks where the playlist name is Music.
3. Get all Track names for `PlaylistId` 5.
4. Get all tracks where the `Genre` is Comedy.
5. Get all tracks where the `Album` is Fireball.
6. Get all tracks for the artist Queen ( 2 nested subqueries ).

/* select * from invoice
where invoiceid IN (select invoiceid from invoiceline where unitprice > .99) */

/* select trackid from playlisttrack
where playlistid IN (select playlistid from playlist where name = 'Music') */

/* select name from track
where trackid IN (select trackid from playlisttrack where playlistid = 5) */

/* select * from track
where genreid in (select genreid from genre where name = 'Comedy') */

/* select * from track
where albumid in (select albumid from album where title = 'Fireball') */

/* select * from track 
where albumid in (
select albumid from album where artistid in (
select artistid from artist where name = 'Queen'
)) */

### Solution

<details>

<summary> <code> SQL Solutions </code> </summary>

<details>

<summary> <code> #1 </code> </summary>

```sql
SELECT *
FROM Invoice
WHERE InvoiceId IN ( SELECT InvoiceId FROM InvoiceLine WHERE UnitPrice > 0.99 );
```

</details>

<details>

<summary> <code> #2 </code> </summary>

```sql
SELECT *
FROM PlaylistTrack
WHERE PlaylistId = ( SELECT PlaylistId FROM Playlist WHERE Name = "Music" );
```

</details>

<details>

<summary> <code> #3 </code> </summary>

```sql
SELECT Name
FROM Track
WHERE TrackId IN ( SELECT TrackId FROM PlaylistTrack WHERE PlaylistId = 5 );
```

</details>

<details>

<summary> <code> #4 </code> </summary>

```sql
SELECT *
FROM Track
WHERE GenreId IN ( SELECT GenreId FROM Genre WHERE Name = "Comedy" );
```

</details>

<details>

<summary> <code> #5 </code> </summary>

```sql
SELECT *
FROM Track
WHERE AlbumId IN ( SELECT AlbumId FROM Album WHERE Title = "Fireball" );
```

</details>

<details>

<summary> <code> #6 </code> </summary>

```sql
SELECT *
FROM Track
WHERE AlbumId IN ( 
  SELECT AlbumId FROM Album WHERE ArtistId IN ( 
    SELECT ArtistId FROM Artist WHERE Name = "Queen" 
  )
); 
```

</details>

</details>

## Practice updating Rows

### Instructions

<details>

<summary> <code> Syntax Hint </code> </summary>

```sql
UPDATE [Table] 
SET [column1] = [value1], [column2] = [value2] 
WHERE [Condition];

UPDATE Athletes SET sport = 'Picklball' WHERE sport = 'pockleball';
```

</details>

<br />

1. Find all customers with fax numbers and set those numbers to `null`.
2. Find all customers with no company (null) and set their company to `"Self"`.
3. Find the customer `Julia Barnett` and change her last name to `Thompson`.
4. Find the customer with this email `luisrojas@yahoo.cl` and change his support rep to `4`.
5. Find all tracks that are the genre `Metal` and have no composer. Set the composer to `"The darkness around us"`.
6. Refresh your page to remove all database changes.


/* update customer
set fax = null
where fax is not null */

/* update customer
set company = 'Self'
where company is null */

/* update customer
set lastname = 'Thompson'
where firstname = 'Julia' and lastname = 'Barnett' */

/* update customer
set supportrepid = 4
where email = 'luisrojas@yahoo.cl' */

/* update track
set composer = 'The darkness around us'
where composer is null and genreid in (select genreid from genre where name = 'Metal')
 */

### Solution

<details>

<summary> <code> SQL Solutions </code> </summary>

<details>

<summary> <code> #1 </code> </summary>

```sql
UPDATE Customer
SET Fax = null
WHERE Fax IS NOT null;
```

</details>

<details>

<summary> <code> #2 </code> </summary>

```sql
UPDATE Customer
SET Company = "Self"
WHERE Company IS null;
```

</details>

<details>

<summary> <code> #3 </code> </summary>

```sql
UPDATE Customer 
SET LastName = "Thompson" 
WHERE FirstName = "Julia" AND LastName = "Barnett";
```

</details>

<details>

<summary> <code> #4 </code> </summary>

```sql
UPDATE Customer
SET SupportRepId = 4
WHERE Email = "luisrojas@yahoo.cl";
```

</details>

<details>

<summary> <code> #5 </code> </summary>

```sql
UPDATE Track
SET Composer = "The darkness around us"
WHERE GenreId = ( SELECT GenreId FROM Genre WHERE Name = "Metal" )
AND Composer IS null;
```

</details>

</details>

## Group by

### Instructions

<details>

<summary> <code> Syntax Hint </code> </summary>

```sql
SELECT [Column1], [Column2]
FROM [Table] [abbr]
GROUP BY [Column];
```

</details>

<br />

1. Find a count of how many tracks there are per genre. Display the genre name with the count.
2. Find a count of how many tracks are the `"Pop"` genre and how many tracks are the `"Rock"` genre.
3. Find a list of all artists and how many albums they have.

/* select count(trackid), track.genreid, genre.name from track
join genre on genre.genreid = track.genreid
group by genre.name */

/* select count(*), genre.name from track
join genre on track.genreid = genre.genreid
where genre.name = 'Pop' or genre.name = 'Rock'
group by genre.name */

/* select count(*) as number_of_albums, artist.name from album
join artist on album.artistid = artist.artistid
group by artist.name */

### Solution

<details>

<summary> <code> SQL Solutions </code> </summary>

<details>

<summary> <code> #1 </code> </summary>

```sql
SELECT Count(*), g.Name
FROM Track t
JOIN Genre g ON t.GenreId = g.GenreId
GROUP BY g.Name;
```

</details>

<details>

<summary> <code> #2 </code> </summary>

```sql
SELECT Count(*), g.Name
FROM Track t
JOIN Genre g ON g.GenreId = t.GenreId
WHERE g.Name = 'Pop' OR g.Name = 'Rock'
GROUP BY g.Name;
```

</details>

<details>

<summary> <code> #3 </code> </summary>

```sql
SELECT ar.Name, Count(*)
FROM Artist ar
JOIN Album al ON ar.ArtistId = al.ArtistId
GROUP BY al.ArtistId;
```

</details>

</details>

## Use Distinct

<details>

<summary> <code> Syntax Hint </code> </summary>

```sql
SELECT DISTINCT [Column]
FROM [Table];
```

</details>

<br />

1. From the `Track` table find a unique list of all `Composer`s.
2. From the `Invoice` table find a unique list of all `BillingPostalCode`s.
3. From the `Customer` table find a unique list of all `Company`s.

/* select distinct composer from track */

/* select distinct Billingpostalcode from invoice */

/* select distinct company from customer */

<details>

<summary> <code> SQL Solutions </code> </summary>

<details>

<summary> <code> #1 </code> </summary>

```sql
SELECT DISTINCT Composer
FROM Track;
```

</details>

<details>

<summary> <code> #2 </code> </summary>

```sql
SELECT DISTINCT BillingPostalCode
FROM Invoice;
```

</details>

<details>

<summary> <code> #3 </code> </summary>

```sql
SELECT DISTINCT Company
FROM Customer;
```

</details>

</details>

## Delete Rows

### Summary

Always do a select before a delete to make sure you get back exactly what you want and only what you want to delete! Since we cannot delete anything from the pre-defined tables ( foreign key restraints ), use the following SQL code to create a dummy table:

<details>

<summary> <code> practice_delete TABLE </code> </summary>

```sql
CREATE TABLE practice_delete ( Name string, Type string, Value integer );
INSERT INTO practice_delete ( Name, Type, Value ) VALUES ("delete", "bronze", 50);
INSERT INTO practice_delete ( Name, Type, Value ) VALUES ("delete", "bronze", 50);
INSERT INTO practice_delete ( Name, Type, Value ) VALUES ("delete", "bronze", 50);
INSERT INTO practice_delete ( Name, Type, Value ) VALUES ("delete", "silver", 100);
INSERT INTO practice_delete ( Name, Type, Value ) VALUES ("delete", "silver", 100);
INSERT INTO practice_delete ( Name, Type, Value ) VALUES ("delete", "gold", 150);
INSERT INTO practice_delete ( Name, Type, Value ) VALUES ("delete", "gold", 150);
INSERT INTO practice_delete ( Name, Type, Value ) VALUES ("delete", "gold", 150);
INSERT INTO practice_delete ( Name, Type, Value ) VALUES ("delete", "gold", 150);

SELECT * FROM practice_delete;
```

</details>

### Instructions

<details>

<summary> <code> Syntax Hint </code> </summary>

```sql
DELETE FROM [Table] WHERE [Condition]
```

</details>

<br />

1. Copy, paste, and run the SQL code from the summary.
2. Delete all `"bronze"` entries from the table.
3. Delete all `"silver"` entries from the table.
4. Delete all entries whose value is equal to `150`.

/* delete from practice_delete
where type ='bronze' */

/* delete from practice_delete
where type = 'silver' */

/* delete from practice_delete
where value = '150' */

### Solution

<details>

<summary> <code> SQL Solutions </code> </summary>

<details>

<summary> <code> #1 </code> </summary>

```sql
DELETE 
FROM practice_delete 
WHERE Type = "bronze";
```

</details>

<details>

<summary> <code> #2 </code> </summary>

```sql
DELETE 
FROM practice_delete 
WHERE Type = "silver";
```

</details>

<details>

<summary> <code> #3 </code> </summary>

```sql
DELETE 
FROM practice_delete 
WHERE Value = 150;
```

</details>

</details>


## eCommerce Simulation - No Hints

### Summary

Let's simulate an e-commerce site. We're going to need users, products, and orders.

* Users need a name and an email.
* Products need a name and a price
* Orders need a ref to product.
* All 3 need primary keys.

### Instructions

* Create 3 tables following the criteria in the summary.

create table users (
id integer primary key autoincrement,
name varchar(40),
email varchar(40)
)

create table product (
id integer primary key autoincrement,
name varchar(40),
price float
)

create table orders (
id integer primary key autoincrement,
productid integer,
foreign key (productid) references product(id)
)

* Add some data to fill up each table.
  * At least 3 users, 3 products, 3 orders.
  
insert into user (name, email) values ('croane', 'connor@email.com');
insert into user (name, email) values ('gil', 'gil@email.com');
insert into user (name, email) values ('ron', 'ron@email.com');

insert into product (name, price) values ('shirt', 'connor@email.com')
insert into product (name, price) values ('socks', 'gil@email.com')
insert into product (name, price) values ('hat', 'ron@email.com')

insert into orders (productid) values (1);
insert into orders (productid) values (2);
insert into orders (productid) values (3)
  
* Run queries against your data.
  * Get all products for the first order.
  select * from products
  * Get all orders.
    select * from orders
  * Get the total cost of an order ( sum the price of all products on an order ).
  
* Add a foreign key reference from Orders to Users.

create table orders (
id integer primary key autoincrement,
productid integer,
user_id integer,
foreign key (productid) references product(id)
  foreign key (user_id) references user(id)
)  

create junction table between orders and products -->
   create table ordersItems (
product_id integer,
order_id integer,
foreign key (product_id) references product(id)
  foreign key (order_id) references orders(id)
)  

* Update the Orders table to link a user to each order.
* Run queries against your data.
  * Get all orders for a user.
  
  select * from orders
join user on user.id = orders.user_id

  * Get how many orders each user has.
  
  select count(*) from orders
join user on user.id = orders.user_id
group by user.id

### Black Diamond

* Get the total amount on all orders for each user.


## Contributions

If you see a problem or a typo, please fork, make the necessary changes, and create a pull request so we can review your changes and merge them into the master repo and branch.

## Copyright

© DevMountain LLC, 2017. Unauthorized use and/or duplication of this material without express and written permission from DevMountain, LLC is strictly prohibited. Excerpts and links may be used, provided that full and clear credit is given to DevMountain with appropriate and specific direction to the original content.

<p align="center">
<img src="https://devmounta.in/img/logowhiteblue.png" width="250">
</p>
