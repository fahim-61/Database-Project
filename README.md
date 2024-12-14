# Database-Project SQL code

# Users Table 
CREATE TABLE Users (
    UserID SERIAL PRIMARY KEY,
    Name VARCHAR(100) NOT NULL,
    Email VARCHAR(100) UNIQUE NOT NULL,
    Password TEXT NOT NULL,
    Role ENUM('Buyer’, 'Seller’, 'Admin') DEFAULT 'Buyer',
    Address VARCHAR(255),
    PhoneNumber VARCHAR(20),
    CreatedAt TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);

# Books Table
CREATE TABLE Books (
    BookID SERIAL PRIMARY KEY,
    Title VARCHAR(255) NOT NULL,
    AuthorID INT REFERENCES Authors(AuthorID),
    CategoryID INT REFERENCES Categories(CategoryID),
    PublisherID INT REFERENCES Publishers(PublisherID),
    ISBN VARCHAR(20) UNIQUE,
    PublicationYear INT,
    Price DECIMAL(10,2) NOT NULL,
    StockQuantity INT DEFAULT 0
);

# Authors Table
CREATE TABLE Authors (
    AuthorID SERIAL PRIMARY KEY,
    Name VARCHAR(100) NOT NULL,
    Biography TEXT
);

#  Category Table
CREATE TABLE Categories (
    CategoryID SERIAL PRIMARY KEY,
    CategoryName VARCHAR(100) UNIQUE NOT NULL
);	

# Publishers Table
CREATE TABLE Publishers (
    PublisherID SERIAL PRIMARY KEY,
    Name VARCHAR(100) NOT NULL,
    Address VARCHAR(255),
    ContactInfo VARCHAR(100)
);

# Orders Table
CREATE TABLE Orders (
    OrderID SERIAL PRIMARY KEY,
    UserID INT REFERENCES Users(UserID),
    OrderDate TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    TotalAmount DECIMAL(10,2),
    ShippingAddress VARCHAR(255),
    OrderStatus ENUM('Pending', 'Shipped', 'Delivered', 'Cancelled') DEFAULT 'Pending'
);

# OrderDetails Table
CREATE TABLE OrderDetails (
    OrderDetailID SERIAL PRIMARY KEY,
    OrderID INT REFERENCES Orders(OrderID),
    BookID INT REFERENCES Books(BookID),
    Quantity INT NOT NULL,
    UnitPrice DECIMAL(10,2),
    TotalPrice DECIMAL(10,2) GENERATED ALWAYS AS (Quantity * UnitPrice) STORED
);

# Payments Table(Stores payment details)
CREATE TABLE Payments (
    PaymentID SERIAL PRIMARY KEY,
    OrderID INT REFERENCES Orders(OrderID),
    PaymentMethod ENUM('Credit Card', 'PayPal', 'Bank Transfer') NOT NULL,
    PaymentDate TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    AmountPaid DECIMAL(10,2),
    PaymentStatus ENUM('Pending', 'Completed', 'Failed') DEFAULT 'Pending'
);

# Reviews (Users can review books)
CREATE TABLE Reviews (
    ReviewID SERIAL PRIMARY KEY,
    UserID INT REFERENCES Users(UserID),
    BookID INT REFERENCES Books(BookID),
    Rating INT CHECK(Rating BETWEEN 1 AND 5),
    ReviewText TEXT
);

# Wishlist 
CREATE TABLE Wishlist (
    WishlistID SERIAL PRIMARY KEY,
    UserID INT REFERENCES Users(UserID),
    BookID INT REFERENCES Books(BookID),
    UNIQUE (UserID, BookID)
);

# Promotions Table(For campaigns like seasonal discounts)
CREATE TABLE Promotions (
    PromotionID SERIAL PRIMARY KEY,
    BookID INT REFERENCES Books(BookID),
    DiscountPercentage INT CHECK(DiscountPercentage BETWEEN 0 AND 100),
    StartDate DATE,
    EndDate DATE
);

# LoyaltyPointsHistory Table(Tracks users' points activity)
CREATE TABLE LoyaltyPointsHistory (
    PointID SERIAL PRIMARY KEY,
    UserID INT REFERENCES Users(UserID),
    PointsEarned INT,
    TransactionDate TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);



# Data Insert for Users Table
INSERT INTO Users (Name, Email, Password, Role, Address, PhoneNumber, CreatedAt) VALUES
('Nur Muhammad', 'nur@gmail.com', 'password1', 'Buyer', 'Tangail', '123-456-7890', '2024-01-01 10:00:00'),
('Fahim Ferdous', 'fahim@gmail.com', 'password2','Buyer', 'Pabna', '234-567-8901', '2024-01-02 11:00:00'),
('Samanta Islam', 'samanta@gmail.com', 'password3',  'Seller', 'Ashulia', '345-678-9012', '2024-01-03 12:00:00'),
('Tanvir Mahmud', 'tanvir@gmail.com', 'password4', 'Buyer', 'Mymansingh', '456-789-0123', '2024-01-04 13:00:00'),
('Azra Zerin', 'azra@gmail.com', 'password5', 'Buyer', 'Natore', '567-890-1234', '2024-01-05 14:00:00'),
('Sakib Khan', 'sakib@gmail.com', 'password6', 'Admin', 'Khulna', '678-901-2345', '2024-01-06 15:00:00'),
('Halim Madbor', 'halim@gmail.com', 'password7',  'Buyer', 'Bogra', '789-012-3456', '2024-01-07 16:00:00'),
('Hannah Ali', 'hannah@gmail.com', 'password8',  'Buyer', 'Sylhet', '890-123-4567', '2024-01-08 17:00:00'),
('Iman Ali', 'iman@gmail.com', 'password9', 'Seller', 'Dhaka', '901-234-5678', '2024-01-09 18:00:00'),
('Amir Khan', 'amir@gmail.com', 'password10',  'Buyer', 'Rajshahi', '012-345-6789', '2024-01-10 19:00:00');

# Data Insert for Books Table
INSERT INTO Books (Title, AuthorID, CategoryID, PublisherID, ISBN, PublicationYear, Price, StockQuantity) VALUES
('Feluda Shomogro', 1, 1, 1, '1234567890123', 2020, 350, 5),
('Shei Shomoy', 2, 2, 2, '2345678901234', 2021, 250, 2),
('Chader Pahar', 3, 3, 3, '3456789012345', 2022, 125, 9),
('Dipu Number Two', 4, 4, 4, '4567890123456', 2023, 290, 11),
('Hazar Bochor Dhore', 5, 5, 5, '5678901234567', 2024, 200, 13),
('Deshe Bideshe', 6, 6, 6, '6789012345678', 2025, 170, 18),
('Ami Topu', 7, 7, 7, '7890123456789', 2026, 350, 7),
('Podda Nodir Majhi', 8, 8, 8, '8901234567890', 2027, 190, 12),
('Durbin', 9, 9, 9, '9012345678901', 2028, 210, 21),
('Putul Nacher Itikotha', 10, 10, 10, '0123456789012', 2029, 250, 11);
Authors Table- Data Insert
INSERT INTO Authors (Name, Biography) VALUES
('Kazi Nazrul Islam', 'National Poet of Bangladesh'),
('Rabindranath Tagore', 'Bengali Famous Writer'),
('Jashim Uddin', 'Polli Kobi'),
('Humayon Ahmed', 'Drama and Film Maker'),
('Jibonanondo Das', 'Schoolteacher and magazine Publisher');

# Data Insert for Categories Table
INSERT INTO Categories (CategoryName) VALUES
('Thriller'),
('Ghost Story'),
('Adult 18+'),
('Science Friction'),
('Classic'),
('Documentary'),
('Experimental');

# Data Insert for Publishers Table
INSERT INTO Publishers (Name, Address, ContactInfo) VALUES
('Mayurpankhi', 'Dhanmondi', 'mayur@gmail.com'),
('Guba Books', 'Mirpur', 'guba@gmail.com'),
('Somoy', 'Uttora', 'somoy@gmail.com'),
('Chandramukhi', 'Jassore', 'chandra@gmail.com'),
('Niladri', 'Bashundhara', 'niladri@gmail.com');

# Data Insert for Orders Table
INSERT INTO Orders (UserID, OrderDate, TotalAmount, ShippingAddress, OrderStatus) VALUES
(1, '2024-01-01 10:00:00', 500, 'Tangail', 'Pending'),
(2, '2024-01-02 11:00:00', 450, 'Mirzapur', 'Shipped'),
(4, '2024-01-04 13:00:00', 370, 'Dhaka', 'Cancelled'),
(5, '2024-01-05 14:00:00', 270, 'Mirpur', 'Pending'),
(7, '2024-01-07 16:00:00', 480, 'Dhanmondi', 'Delivered'),
(8, '2024-01-08 17:00:00', 510, 'Sylhet', 'Cancelled'),
(10, '2024-01-10 19:00:00', 290, 'Pabna', 'Shipped');

# Data Insert for OrderDetails Table
INSERT INTO OrderDetails (OrderID, BookID, Quantity, UnitPrice) VALUES
(1, 1, 1, 80),
(2, 7, 3, 90),
(3, 1, 1, 50),
(4, 9, 2, 125),
(5, 7, 4, 110),
(6, 2, 3, 70),
(7, 3, 5, 190);

# Data Insert for Payments Table
BEGIN TRANSACTION;
INSERT INTO Payments (OrderID, PaymentMethod, PaymentDate, AmountPaid, PaymentStatus) VALUES
(1, 'Credit Card', '2024-01-01 10:00:00', 500, 'Completed'),
(2, 'PayPal', '2024-01-02 11:00:00', 450, 'Completed'),
(3, 'Bank Transfer', '2024-01-03 12:00:00', 370, 'Completed'),
(4, 'Credit Card', '2024-01-04 13:00:00', 270, 'Failed'),
(5, 'PayPal', '2024-01-05 14:00:00', 480, 'Pending'),
(6, 'Bank Transfer', '2024-01-06 15:00:00', 510, 'Completed'),
(7, 'Credit Card', '2024-01-07 16:00:00', 290, 'Completed');
COMMIT;

# Data Insert forReviews Table
INSERT INTO Reviews (UserID, BookID, Rating, ReviewText) VALUES
(5, 3, 4, 'Excellent book!'),
(7, 5, 3, 'Very good read!');

# Data Insert for  Wishlist Table
INSERT INTO Wishlist (UserID, BookID) VALUES
(1, 3),
(4, 6),
(5, 5);

# Data Insert for Promotions Table
INSERT INTO Promotions (BookID, DiscountPercentage, StartDate, EndDate) VALUES
(1, 10, '2024-01-01', '2024-01-31'),
(3, 30, '2024-02-01', '2024-02-28'),
(4, 20, '2024-03-01', '2024-03-31'),
(7, 15, '024-04-01', '2024-04-30'),
(10, 25, '2024-05-01', '2024-05-31');

# Data Insert for LoyaltyPointsHistory Table
INSERT INTO LoyaltyPointsHistory (UserID, PointsEarned, TransactionDate) VALUES
(2, 400, '2024-01-06 15:00:00'),
(5, 700, '2024-01-07 16:00:00'),
(8, 300, '2024-01-08 17:00:00'),
(7, 600, '2024-01-09 18:00:00'),
(4, 200, '2024-01-10 19:00:00');



# Create Buyers, Sellers and Admins table for using Trigger
CREATE TABLE Buyers (
    BuyerID INT PRIMARY KEY,
    UserID INT REFERENCES Users(UserID),
    Name VARCHAR(100),
    Email VARCHAR(100),
    Address VARCHAR(255),
    PhoneNumber VARCHAR(20),
    CreatedAt TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);
CREATE TABLE Sellers (
    SellerID INT PRIMARY KEY,
    UserID INT REFERENCES Users(UserID),
    Name VARCHAR(100),
    Email VARCHAR(100),
    Address VARCHAR(255),
    PhoneNumber VARCHAR(20),
    CreatedAt TIMESTAMP DEFAULT CURRENT_TIMESTAMP
); 
CREATE TABLE Admins (
    AdminID INT PRIMARY KEY,
    UserID INT REFERENCES Users(UserID),
    Name VARCHAR(100),
    Email VARCHAR(100),
    Address VARCHAR(255),
    PhoneNumber VARCHAR(20),
    CreatedAt TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);

# Create Trigger on User table:
DELIMITER $$
CREATE TRIGGER after_user_insert
AFTER INSERT ON Users
FOR EACH ROW
BEGIN
    IF NEW.Role = 'Buyer' THEN
        INSERT INTO Buyers (BuyerID, UserID, Name, Email, Address, PhoneNumber, CreatedAt)
        VALUES (NEW.UserID, NEW.UserID, NEW.Name, NEW.Email, NEW.Address, NEW.PhoneNumber, NEW.CreatedAt);
    ELSEIF NEW.Role = 'Seller' THEN
        INSERT INTO Sellers (SellerID, UserID, Name, Email, Address, PhoneNumber, CreatedAt)
        VALUES (NEW.UserID, NEW.UserID, NEW.Name, NEW.Email, NEW.Address, NEW.PhoneNumber, NEW.CreatedAt);
    ELSEIF NEW.Role = 'Admin' THEN
        INSERT INTO Admins (AdminID, UserID, Name, Email, Address, PhoneNumber, CreatedAt)
        VALUES (NEW.UserID, NEW.UserID, NEW.Name, NEW.Email, NEW.Address, NEW.PhoneNumber, NEW.CreatedAt);
    END IF;
END$$
DELIMITER ;



# Creating a View FilterBooks for using "Nested SQL" on Books table
CREATE VIEW FilteredBooks AS
SELECT *
FROM Books
WHERE BookID IN (
    SELECT BookID
    FROM Books
    WHERE Price >= 200 AND BookID > 5
);

# Creating a View MaxCreditCardPayment for using "Aggregate Function" on Payment table
CREATE VIEW MaxCreditCardPayment AS
SELECT 
    MAX(AmountPaid) AS MaxPayment
FROM 
    Payments
WHERE 
    PaymentMethod = 'Credit Card';

# Creating a View OrderDetailsView for using "Join" on Orders & OrderDetails table
CREATE VIEW OrderDetailsView AS
SELECT 
    Orders.OrderID,
    Orders.UserID,
    Orders.OrderDate,
    Orders.TotalAmount,
    Orders.ShippingAddress,
    Orders.OrderStatus,
    OrderDetails.BookID,
    OrderDetails.Quantity,
    OrderDetails.UnitPrice,
    (OrderDetails.Quantity * OrderDetails.UnitPrice) AS LineTotal
FROM 
    Orders
INNER JOIN 
    OrderDetails
ON 
    Orders.OrderID = OrderDetails.OrderID;

# Creating a View OrderDetails for using "Sql Operator" on OrderDetails table
CREATE VIEW ViewOrderDetails AS
SELECT 
    OrderDetailID,
    OrderID,
    BookID,
    Quantity,
    UnitPrice,
    (Quantity * UnitPrice) AS CalculatedTotalPrice
FROM OrderDetails;

# Creating a View OrderDetailsWithStock for using "Having Clause" on Books table
CREATE VIEW ViewBooksWithStock AS
SELECT 
    CategoryID,
    COUNT(BookID) AS TotalBooks,
    SUM(StockQuantity) AS TotalStock
FROM Books
GROUP BY CategoryID
HAVING SUM(StockQuantity) > 10;


# Use Store Procedure on Payments Table to show PaymentMethod ('Credit Card', 'PayPal', 'Bank Transfer')
DELIMITER //
CREATE PROCEDURE payment_method (IN pm VARCHAR(50)) 
BEGIN
    SELECT *
    FROM Payments 
    WHERE PaymentMethod = pm; 
END //
DELIMITER ;

# Call the Store Procedure
CALL payment_method ('Credit Card');


