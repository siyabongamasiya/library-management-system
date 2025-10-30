# Library Management Database – SQL Schema & Queries

## 1.Create the required tables
CREATE TABLE Authors (
id SERIAL PRIMARY KEY,
name varchar(100),
nationality varchar(50),
birth_year INT,
death_year INT
);

CREATE TABLE Books (
id SERIAL PRIMARY KEY,
title varchar(200),
author_id INT References Authors(id),
genres TEXT[],
published_year INT,
available BOOLEAN
);

CREATE TABLE Patrons (
id SERIAL PRIMARY KEY,
name varchar(100),
email varchar(100),
borrowed_books INT[]
);

## 2.Insert at least 10 sample books.
INSERT INTO books (id, title, author_id, genres, published_year, available) VALUES
(1, '1984', 1, ARRAY['Dystopian', 'Political Fiction'], 1949, TRUE),
(2, 'To Kill a Mockingbird', 2, ARRAY['Southern Gothic', 'Bildungsroman'], 1960, TRUE),
(3, 'The Great Gatsby', 3, ARRAY['Tragedy'], 1925, TRUE),
(4, 'Brave New World', 4, ARRAY['Dystopian', 'Science Fiction'], 1932, TRUE),
(5, 'The Catcher in the Rye', 5, ARRAY['Realist Novel', 'Bildungsroman'], 1951, TRUE),
(6, 'Moby-Dick', 6, ARRAY['Adventure Fiction'], 1851, TRUE),
(7, 'Pride and Prejudice', 7, ARRAY['Romantic Novel'], 1813, TRUE),
(8, 'War and Peace', 8, ARRAY['Historical Novel'], 1869, TRUE),
(9, 'Crime and Punishment', 9, ARRAY['Philosophical Novel'], 1866, TRUE),
(10, 'The Hobbit', 10, ARRAY['Fantasy'], 1937, TRUE);

## 3.Insert at least 10 sample authors.
INSERT INTO authors (id, name, nationality, birth_year, death_year) VALUES
(1, 'George Orwell', 'British', 1903, 1950),
(2, 'Harper Lee', 'American', 1926, 2016),
(3, 'F. Scott Fitzgerald', 'American', 1896, 1940),
(4, 'Aldous Huxley', 'British', 1894, 1963),
(5, 'J.D. Salinger', 'American', 1919, 2010),
(6, 'Herman Melville', 'American', 1819, 1891),
(7, 'Jane Austen', 'British', 1775, 1817),
(8, 'Leo Tolstoy', 'Russian', 1828, 1910),
(9, 'Fyodor Dostoevsky', 'Russian', 1821, 1881),
(10, 'J.R.R. Tolkien', 'British', 1892, 1973);

## 4.Insert at least 10 sample patrons.
INSERT INTO patrons (id, name, email, borrowed_books) VALUES
(1, 'Alice Johnson', 'alice@example.com', ARRAY[]::INT[]),
(2, 'Bob Smith', 'bob@example.com', ARRAY[1, 2]),
(3, 'Carol White', 'carol@example.com', ARRAY[]::INT[]),
(4, 'David Brown', 'david@example.com', ARRAY[3]),
(5, 'Eve Davis', 'eve@example.com', ARRAY[]::INT[]),
(6, 'Frank Moore', 'frank@example.com', ARRAY[4, 5]),
(7, 'Grace Miller', 'grace@example.com', ARRAY[]::INT[]),
(8, 'Hank Wilson', 'hank@example.com', ARRAY[6]),
(9, 'Ivy Taylor', 'ivy@example.com', ARRAY[]::INT[]),
(10, 'Jack Anderson', 'jack@example.com', ARRAY[7, 8]);

## 5.Get all books.
SELECT * FROM Books;

## 6.Get a book by title.
SELECT * 
FROM Books
WHERE title = 'The Great Gatsby';

## 7.Get all books by a specific author.
SELECT b.title,b.published_year,b.available,a.name,a.nationality 
FROM Books as b
JOIN Authors as a
ON b.author_id = a.id
WHERE a.name = 'Harper Lee';

## 8.Get all available books.
SELECT * 
FROM Books
WHERE available ='true';

## 9.Mark a book as borrowed (set available = false).
UPDATE Books
SET available = false
WHERE title = 'The Hobbit';

## 10.Add a new genre to an existing book.
UPDATE Books
SET genres = array_append(genres, 'Horror')
WHERE title = '1984';

## 11.Add a borrowed book to a patron’s record.
-- UPDATE Patrons
-- SET borrowed_books = array_append(borrowed_books,10)
-- WHERE name = 'Alice Johnson';

## 12.Delete a book by title.
DELETE 
FROM Books
WHERE title = 'Brave New World';

## 13.Delete an author by ID.
DELETE FROM books WHERE author_id = 7;
DELETE FROM authors WHERE id = 7;

## 14.Find books published after 1950.
SELECT * 
FROM Books
WHERE published_year > 1950 AND available = true;

## Find all American authors.
SELECT * 
FROM Authors
WHERE nationality = 'American';

## Set all books as available.
UPDATE Books
SET available = true;

## Find all books that are available AND published after 1950.
SELECT * 
FROM Books
WHERE published_year > 1950 AND available = true;

## Find authors whose names contain "George".
SELECT *
FROM Authors
WHERE name LIKE '%George%';

## Increment the published year 1869 by 1.
UPDATE Books
SET published_year = published_year + 1
WHERE published_year = 1925; 