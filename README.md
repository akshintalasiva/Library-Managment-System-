-- =========================================================
-- LIBRARY MANAGEMENT SYSTEM
-- COMPLETE SQL CODE WITH COMMANDS
-- =========================================================


-- =========================================================
-- COMMAND 1: CREATE DATABASE
-- =========================================================

CREATE DATABASE IF NOT EXISTS Library_Management_System;

USE Library_Management_System;


-- =========================================================
-- COMMAND 2: DROP EXISTING TABLES
-- =========================================================

DROP TABLE IF EXISTS Fine CASCADE;

DROP TABLE IF EXISTS Loan CASCADE;

DROP TABLE IF EXISTS Book_Author CASCADE;

DROP TABLE IF EXISTS Book CASCADE;

DROP TABLE IF EXISTS Author CASCADE;

DROP TABLE IF EXISTS Publisher CASCADE;

DROP TABLE IF EXISTS Member CASCADE;


-- =========================================================
-- COMMAND 3: CREATE PUBLISHER TABLE
-- =========================================================

CREATE TABLE Publisher
(
    publisher_id INT PRIMARY KEY,
    publisher_name VARCHAR(100) NOT NULL,
    country VARCHAR(50),
    website VARCHAR(100)
);


-- =========================================================
-- COMMAND 4: CREATE AUTHOR TABLE
-- =========================================================

CREATE TABLE Author
(
    author_id INT PRIMARY KEY,
    first_name VARCHAR(50) NOT NULL,
    last_name VARCHAR(50),
    nationality VARCHAR(50)
);


-- =========================================================
-- COMMAND 5: CREATE BOOK TABLE
-- =========================================================

CREATE TABLE Book
(
    book_id INT PRIMARY KEY,
    title VARCHAR(100) NOT NULL,
    isbn VARCHAR(20) UNIQUE NOT NULL,
    total_copies INT DEFAULT 1,
    genre VARCHAR(50),
    publication_year INT,
    publisher_id INT,

    FOREIGN KEY (publisher_id)
    REFERENCES Publisher(publisher_id)
);


-- =========================================================
-- COMMAND 6: CREATE BOOK_AUTHOR TABLE
-- =========================================================

CREATE TABLE Book_Author
(
    book_id INT,
    author_id INT,

    PRIMARY KEY (book_id, author_id),

    FOREIGN KEY (book_id)
    REFERENCES Book(book_id),

    FOREIGN KEY (author_id)
    REFERENCES Author(author_id)
);


-- =========================================================
-- COMMAND 7: CREATE MEMBER TABLE
-- =========================================================

CREATE TABLE Member
(
    member_id INT PRIMARY KEY,
    roll_no VARCHAR(20) UNIQUE,
    first_name VARCHAR(50) NOT NULL,
    last_name VARCHAR(50),
    email VARCHAR(100) UNIQUE NOT NULL,
    membership_type VARCHAR(30) DEFAULT 'Student',
    membership_date DATE,
    membership_expiry DATE
);


-- =========================================================
-- COMMAND 8: CREATE LOAN TABLE
-- =========================================================

CREATE TABLE Loan
(
    loan_id INT PRIMARY KEY,
    book_id INT,
    member_id INT,
    loan_date DATE,
    due_date DATE,
    return_date DATE,
    status VARCHAR(20) DEFAULT 'Issued',

    FOREIGN KEY (book_id)
    REFERENCES Book(book_id),

    FOREIGN KEY (member_id)
    REFERENCES Member(member_id)
);


-- =========================================================
-- COMMAND 9: CREATE FINE TABLE
-- =========================================================

CREATE TABLE Fine
(
    fine_id INT PRIMARY KEY,
    loan_id INT,
    member_id INT,
    fine_amount DECIMAL(10,2) DEFAULT 0,
    fine_date DATE,
    payment_status VARCHAR(20) DEFAULT 'Unpaid',

    FOREIGN KEY (loan_id)
    REFERENCES Loan(loan_id),

    FOREIGN KEY (member_id)
    REFERENCES Member(member_id)
);


-- =========================================================
-- COMMAND 10: INSERT PUBLISHER DATA
-- =========================================================

INSERT INTO Publisher
VALUES
(1, 'Pearson', 'India', 'www.pearson.com'),
(2, 'McGraw Hill', 'USA', 'www.mheducation.com'),
(3, 'OReilly Media', 'USA', 'www.oreilly.com');


-- =========================================================
-- COMMAND 11: INSERT AUTHOR DATA
-- =========================================================

INSERT INTO Author
VALUES
(1, 'Robert', 'Martin', 'American'),
(2, 'James', 'Gosling', 'Canadian'),
(3, 'Dennis', 'Ritchie', 'American');


-- =========================================================
-- COMMAND 12: INSERT BOOK DATA
-- =========================================================

INSERT INTO Book
VALUES
(1, 'Programming Basics', '978100000001', 5,
 'Programming', 2024, 1),

(2, 'Database Systems', '978100000002', 4,
 'Database', 2023, 2),

(3, 'Python Programming', '978100000003', 6,
 'Programming', 2025, 3);


-- =========================================================
-- COMMAND 13: INSERT BOOK_AUTHOR DATA
-- =========================================================

INSERT INTO Book_Author
VALUES
(1, 1),
(2, 2),
(3, 3);


-- =========================================================
-- COMMAND 14: INSERT YOUR DETAILS
-- =========================================================

INSERT INTO Member
VALUES
(
    101,
    '25B11AI018',
    'Akshintala Devi',
    'Sri Charan',
    'devisrcharan@example.com',
    'Student',
    '2026-07-01',
    '2027-06-30'
),

(
    102,
    '25B11AI357',
    'Sriram',
    '',
    'sriram@example.com',
    'Student',
    '2026-07-01',
    '2027-06-30'
);


-- =========================================================
-- COMMAND 15: INSERT LOAN DATA
-- =========================================================

INSERT INTO Loan
VALUES
(
    1001,
    1,
    101,
    '2026-09-01',
    '2026-09-15',
    '2026-09-12',
    'Returned'
),

(
    1002,
    2,
    102,
    '2026-09-05',
    '2026-09-19',
    NULL,
    'Issued'
);


-- =========================================================
-- COMMAND 16: INSERT FINE DATA
-- =========================================================

INSERT INTO Fine
VALUES
(
    501,
    1001,
    101,
    0.00,
    '2026-09-12',
    'Paid'
),

(
    502,
    1002,
    102,
    50.00,
    '2026-09-20',
    'Unpaid'
);


-- =========================================================
-- COMMAND 17: SELECT ALL PUBLISHERS
-- =========================================================

SELECT * FROM Publisher;


-- =========================================================
-- COMMAND 18: SELECT ALL AUTHORS
-- =========================================================

SELECT * FROM Author;


-- =========================================================
-- COMMAND 19: SELECT ALL BOOKS
-- =========================================================

SELECT * FROM Book;


-- =========================================================
-- COMMAND 20: SELECT ALL BOOK_AUTHORS
-- =========================================================

SELECT * FROM Book_Author;


-- =========================================================
-- COMMAND 21: SELECT ALL MEMBERS
-- =========================================================

SELECT * FROM Member;


-- =========================================================
-- COMMAND 22: SELECT ALL LOANS
-- =========================================================

SELECT * FROM Loan;


-- =========================================================
-- COMMAND 23: SELECT ALL FINES
-- =========================================================

SELECT * FROM Fine;


-- =========================================================
-- COMMAND 24: SEARCH PROGRAMMING BOOKS
-- =========================================================

SELECT *
FROM Book
WHERE genre = 'Programming';


-- =========================================================
-- COMMAND 25: COUNT TOTAL BOOKS
-- =========================================================

SELECT COUNT(*) AS total_books
FROM Book;


-- =========================================================
-- COMMAND 26: SORT BOOKS BY COPIES
-- =========================================================

SELECT title, total_copies
FROM Book
ORDER BY total_copies DESC;


-- =========================================================
-- COMMAND 27: MEMBER AND BOOK DETAILS
-- =========================================================

SELECT
    Member.roll_no,
    Member.first_name,
    Member.last_name,
    Book.title,
    Loan.loan_date,
    Loan.due_date,
    Loan.return_date,
    Loan.status
FROM Member
JOIN Loan
ON Member.member_id = Loan.member_id
JOIN Book
ON Loan.book_id = Book.book_id;


-- =========================================================
-- COMMAND 28: AUTHOR AND BOOK DETAILS
-- =========================================================

SELECT
    Book.title,
    Author.first_name,
    Author.last_name
FROM Book
JOIN Book_Author
ON Book.book_id = Book_Author.book_id
JOIN Author
ON Book_Author.author_id = Author.author_id;


-- =========================================================
-- COMMAND 29: BOOK AND PUBLISHER DETAILS
-- =========================================================

SELECT
    Book.title,
    Book.isbn,
    Publisher.publisher_name,
    Publisher.country
FROM Book
JOIN Publisher
ON Book.publisher_id = Publisher.publisher_id;


-- =========================================================
-- COMMAND 30: CURRENTLY ISSUED BOOKS
-- =========================================================

SELECT
    Member.roll_no,
    Member.first_name,
    Book.title,
    Loan.loan_date,
    Loan.due_date,
    Loan.status
FROM Loan
JOIN Member
ON Loan.member_id = Member.member_id
JOIN Book
ON Loan.book_id = Book.book_id
WHERE Loan.status = 'Issued';


-- =========================================================
-- COMMAND 31: RETURNED BOOKS
-- =========================================================

SELECT
    Member.roll_no,
    Member.first_name,
    Book.title,
    Loan.return_date
FROM Loan
JOIN Member
ON Loan.member_id = Member.member_id
JOIN Book
ON Loan.book_id = Book.book_id
WHERE Loan.status = 'Returned';


-- =========================================================
-- COMMAND 32: FINE DETAILS
-- =========================================================

SELECT
    Member.roll_no,
    Member.first_name,
    Book.title,
    Fine.fine_amount,
    Fine.fine_date,
    Fine.payment_status
FROM Fine
JOIN Member
ON Fine.member_id = Member.member_id
JOIN Loan
ON Fine.loan_id = Loan.loan_id
JOIN Book
ON Loan.book_id = Book.book_id;


-- =========================================================
-- COMMAND 33: UPDATE BOOK COPIES
-- =========================================================

UPDATE Book
SET total_copies = 6
WHERE book_id = 1;


-- =========================================================
-- COMMAND 34: UPDATE LOAN STATUS
-- =========================================================

UPDATE Loan
SET status = 'Returned',
    return_date = '2026-09-18'
WHERE loan_id = 1002;


-- =========================================================
-- COMMAND 35: UPDATE FINE STATUS
-- =========================================================

UPDATE Fine
SET payment_status = 'Paid'
WHERE fine_id = 502;


-- =========================================================
-- COMMAND 36: FINAL DISPLAY
-- =========================================================

SELECT * FROM Publisher;

SELECT * FROM Author;

SELECT * FROM Book;

SELECT * FROM Book_Author;

SELECT * FROM Member;

SELECT * FROM Loan;

SELECT * FROM Fine;
