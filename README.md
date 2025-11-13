-- Step 1: Create Tables
CREATE TABLE Members (
    MemberID INT PRIMARY KEY AUTO_INCREMENT,
    MemberName VARCHAR(100) NOT NULL,
    Email VARCHAR(100) UNIQUE,
    Phone VARCHAR(15),
    JoinDate DATE
);

CREATE TABLE Books (
    BookID INT PRIMARY KEY AUTO_INCREMENT,
    Title VARCHAR(150) NOT NULL,
    Author VARCHAR(100),
    Genre VARCHAR(50),
    TotalCopies INT,
    AvailableCopies INT
);

CREATE TABLE IssuedBooks (
    IssueID INT PRIMARY KEY AUTO_INCREMENT,
    BookID INT,
    MemberID INT,
    IssueDate DATE,
    ReturnDate DATE,
    FOREIGN KEY (BookID) REFERENCES Books(BookID),
    FOREIGN KEY (MemberID) REFERENCES Members(MemberID)
);

-- Step 2: Insert Sample Data
INSERT INTO Members (MemberName, Email, Phone, JoinDate) VALUES
('Likhitha Mirdoddi', 'likhitha@gmail.com', '9390469312', '2025-01-15'),
('Rahul Verma', 'rahul@gmail.com', '9876543210', '2025-02-20'),
('Sneha Reddy', 'sneha@gmail.com', '9876123456', '2025-03-05');

INSERT INTO Books (Title, Author, Genre, TotalCopies, AvailableCopies) VALUES
('Introduction to Python', 'John Smith', 'Programming', 5, 5),
('Data Structures in C', 'Mark Wilson', 'Computer Science', 3, 3),
('Machine Learning Basics', 'Andrew Ng', 'AI/ML', 4, 4),
('Digital Electronics', 'M. Morris Mano', 'Electronics', 2, 2);

INSERT INTO IssuedBooks (BookID, MemberID, IssueDate, ReturnDate) VALUES
(1, 1, '2025-03-10', '2025-03-20'),
(3, 2, '2025-03-12', '2025-03-25');

-- Step 3: Perform CRUD Operations

-- View all members
SELECT * FROM Members;

-- View all books
SELECT * FROM Books;

-- Add a new book
INSERT INTO Books (Title, Author, Genre, TotalCopies, AvailableCopies)
VALUES ('Artificial Intelligence', 'Stuart Russell', 'AI', 3, 3);

-- Update available copies (reduce by 1 when issued)
UPDATE Books
SET AvailableCopies = AvailableCopies - 1
WHERE BookID = 1;

-- Delete a member
DELETE FROM Members
WHERE MemberName = 'Rahul Verma';

-- Step 4: Join Query – Show who borrowed which book
SELECT m.MemberName, b.Title, i.IssueDate, i.ReturnDate
FROM IssuedBooks i
JOIN Members m ON i.MemberID = m.MemberID
JOIN Books b ON i.BookID = b.BookID;

-- Step 5: Report – Most Issued Books
SELECT b.Title, COUNT(i.IssueID) AS TimesIssued
FROM IssuedBooks i
JOIN Books b ON i.BookID = b.BookID
GROUP BY b.Title
ORDER BY TimesIssued DESC;

