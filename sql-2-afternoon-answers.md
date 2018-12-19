<<<<<<<<<<<<<>>>>>>>>>>>>>
------ Practice Joins-----
<<<<<<<<<<<<<>>>>>>>>>>>>>

--- 1 ---
SELECT * FROM Invoice I
JOIN InvoiceLine Il
ON I.InvoiceId = Il.InvoiceId
WHERE UnitPrice > .99;

--- 2 ---
SELECT I.Total, I.InvoiceDate, C.FirstName, C.LastName
FROM Invoice I
JOIN Customer C
ON I.CustomerId = C.CustomerId;

--- 3 ---
SELECT C.FirstName, C.LastName, E.FirstName, E.LastName
FROM Customer C
JOIN Employee E
ON C.SupportRepId = E.EmployeeId;

--- 4 ---
SELECT A.Name, Al.Title
FROM Artist A
JOIN Album Al
ON A.ArtistId = Al.ArtistId;

--- 5 ---
SELECT Pl.TrackId 
FROM PlaylistTrack Pl
JOIN Playlist P
ON Pl.PlaylistId = P.PlaylistId
WHERE P.name = 'Music';

--- 6 ---
SELECT T.Name
FROM Track T
JOIN PlaylistTrack P
ON T.TrackId = P.TrackId
WHERE P.PlaylistId = 5;

--- 7 ---
SELECT T.Name, P.Name
FROM Track T
JOIN PlaylistTrack Pt
ON T.TrackId = Pt.TrackId
JOIN Playlist P
ON Pt.PlaylistId = P.PlaylistId;

--- 8 ---
SELECT T.Name, A.Title 
FROM Track T
JOIN Album A
ON A.AlbumId = T.AlbumId
JOIN Genre G
ON T.GenreId = G.GenreId
WHERE G.Name = 'Alternative';

--- Black Diamond ---
SELECT G.Name, Al.Title, A.name, T.Name
FROM Artist A
JOIN Album Al
ON A.ArtistId = Al.ArtistId
JOIN Track T
ON Al.AlbumId = T.AlbumId
JOIN Genre G
ON G.GenreId = T.GenreId
JOIN PlaylistTrack Pt
ON Pt.TrackId = T.TrackId
JOIN Playlist P
ON P.PlaylistId = Pt.PlaylistId
WHERE P.Name = 'Music';


<<<<<<<<<<<<<<<<<<<>>>>>>>>>>>>>>>>>>>
------------ Nested Queries-----------
<<<<<<<<<<<<<<<<<<<>>>>>>>>>>>>>>>>>>>

--- 1 ---
SELECT * FROM Invoice
WHERE InvoiceId IN (SELECT InvoiceId FROM InvoiceLine
WHERE UnitPrice > .99);

--- 2 ---
SELECT * FROM PlaylistTrack
WHERE PlaylistId IN (
  SELECT PlaylistId FROM Playlist
  WHERE Playlist.Name = 'Music'
);

--- 3 ---
SELECT Track.Name FROM Track
WHERE TrackId IN (
  SELECT TrackId FROM  PlaylistTrack
  WHERE PlaylistId = 5
);

--- 4 ---
SELECT * FROM Track
WHERE GenreId In (
  SELECT GenreId FROM Genre
  WHERE Name = 'Comedy'
);

--- 5 ---
SELECT * FROM Track
WHERE AlbumId = (
SELECT AlbumId FROM Album
WHERE Title = 'Fireball'
);

--- 6 ---
SELECT * FROM Track
WHERE AlbumId = (
	SELECT AlbumId FROM Album
	WHERE ArtistId IN (
		SELECT ArtistId FROM Artist
  		WHERE Name = 'Queen'
		)
	)
;


<<<<<<<<<<<<<<<>>>>>>>>>>>>>>>
------- Updating Rows -------
<<<<<<<<<<<<<<<>>>>>>>>>>>>>>>

--- 1 ---
UPDATE Customer
SET Fax = NULL
WHERE Fax IS NOT NULL;

--- 2 ---
UPDATE Customer
SET Company = 'Self'
WHERE Company IS NULL;

--- 3 ---
UPDATE Customer
SET LastName = 'Thompson'
WHERE FirstName = 'Julia' AND LastName = 'Barnett';

--- 4 ---
UPDATE Customer
SET SupportRepId = 4
WHERE Email = 'luisrojas@yahoo.cl';

--- 5 ---
UPDATE Track
SET Composer = 'The darkness around us'
WHERE Composer IS NULL AND GenreId = (
	SELECT GenreId FROM Genre
  	WHERE Name = 'Metal'
);


<<<<<<<<<<<<<<<>>>>>>>>>>>>>>>
---------- Group By ----------
<<<<<<<<<<<<<<<>>>>>>>>>>>>>>>

--- 1 ---
SELECT G.Name, count(T.GenreId)
FROM Track T
JOIN Genre G
ON T.GenreId = G.GenreId
GROUP BY T.GenreId;

--- 2 ---
SELECT count(G.Name), G.Name
FROM Track T
JOIN Genre G
ON G.GenreId = T.GenreId
WHERE G.Name IN ('Rock', 'Pop')
GROUP BY G.Name;

--- 3 ---
SELECT A.Name, count(Al.ArtistId)
FROM Artist A
JOIN Album Al
ON A.ArtistId = Al.ArtistId
GROUP BY A.Name;


<<<<<<<<<<<<<<<<>>>>>>>>>>>>>>>>
----------- Distinct -----------
<<<<<<<<<<<<<<<<>>>>>>>>>>>>>>>>

--- 1 ---
SELECT DISTINCT Composer FROM Track;

--- 2 ---
SELECT DISTINCT BillingPostalCode
FROM Invoice;

--- 3 ---
SELECT DISTINCT Company
FROM Customer;


<<<<<<<<<<<<<<<>>>>>>>>>>>>>>>
--------- Delete Rows --------
<<<<<<<<<<<<<<<>>>>>>>>>>>>>>>

--- 1 ---
DELETE FROM practice_delete 
WHERE Type = 'bronze';

--- 2 ---
DELETE FROM practice_delete
WHERE Type = 'silver';

--- 3 ---
DELETE FROM practice_delete
WHERE Value = 150;


<<<<<<<<<<<<<<<>>>>>>>>>>>>>>>
---- eCommerce Simulation ----
<<<<<<<<<<<<<<<>>>>>>>>>>>>>>>

--- 1 ---
CREATE TABLE Users (
  UserId INTEGER PRIMARY KEY AUTOINCREMENT,
  name VARCHAR(50),
  email TEXT
);

CREATE TABLE Products (
  ProductsId INTEGER PRIMARY KEY AUTOINCREMENT,
  name VARCHAR(50),
  price DECIMAL
);

CREATE TABLE Orders (
  OrderId INTEGER PRIMARY KEY AUTOINCREMENT,
  ProductsId INTEGER,
  FOREIGN KEY (ProductsId) REFERENCES Products(ProductsId)
);

--- 2 ---
INSERT INTO Users (name, email)
VALUES ('David', 'fake@email.us'), 
('Genesis', 'wife@likewife.we'),
('Tim', 'timmy@false.it');

INSERT INTO Products (name, price)
VALUES ('hammer', 25), ('drill', 15), ('nail', 5);

INSERT INTO Orders (UserId, ProductId)
VALUES (2, 3), (1, 2), (3, 1);

--- 4 ---
SELECT ProductId FROM Orders
WHERE OrderId = 1;

Select * FROM Orders;

SELECT sum(P.price) FROM Orders O
JOIN Products P
ON O.ProductId = P.ProductId;

--- 5 ---
ALTER TABLE Orders
ADD COLUMN UserId INTEGER
REFERENCES Users(UserId); 

--- 6 --- 
SELECT * FROM Orders
WHERE UserId = 1;

SELECT count(UserId) FROM Orders
GROUP BY UserId;








