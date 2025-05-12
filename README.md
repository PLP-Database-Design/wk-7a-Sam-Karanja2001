# 📝 Assignment: Database Design and Normalization

## 🎯 **Learning Objectives**
* 🛠️ **Understand the principles of good database design** and **normalization**.
* 💡 **Apply normalization techniques** to improve database structure and efficiency.
* 🔍 **Learn First, Second, and Third Normal Forms** (1NF, 2NF, 3NF) to eliminate redundancy and optimize data storage.

---

## 📋 **What You'll Need**
* 💻 A computer with internet access.
* ✍️ A code editor (e.g., Visual Studio Code).
* 🖥️ MySQL Workbench or another SQL database environment.

---


## 📝 Submission Instructions  
📂 Write all your SQL queries in the **answers.sql** file.  
✍️ Answer each question concisely and make sure your queries are clear and correct.  
🗣️ Structure your responses clearly, and use comments if necessary to explain your approach.

--- 

## 📚 Assignment Questions

---

### Question 1 Achieving 1NF (First Normal Form) 🛠️
Task:
- You are given the following table **ProductDetail**:

| OrderID | CustomerName  | Products                        |
|---------|---------------|---------------------------------|
| 101     | John Doe      | Laptop, Mouse                   |
| 102     | Jane Smith    | Tablet, Keyboard, Mouse         |
| 103     | Emily Clark   | Phone                           |


- In the table above, the **Products column** contains multiple values, which violates **1NF**.
- **Write an SQL query** to transform this table into **1NF**, ensuring that each row represents a single product for an order

--- 

### Question 2 Achieving 2NF (Second Normal Form) 🧩

- You are given the following table **OrderDetails**, which is already in **1NF** but still contains partial dependencies:

| OrderID | CustomerName  | Product      | Quantity |
|---------|---------------|--------------|----------|
| 101     | John Doe      | Laptop       | 2        |
| 101     | John Doe      | Mouse        | 1        |
| 102     | Jane Smith    | Tablet       | 3        |
| 102     | Jane Smith    | Keyboard     | 1        |
| 102     | Jane Smith    | Mouse        | 2        |
| 103     | Emily Clark   | Phone        | 1        |

- In the table above, the **CustomerName** column depends on **OrderID** (a partial dependency), which violates **2NF**. 

- Write an SQL query to transform this table into **2NF** by removing partial dependencies. Ensure that each non-key column fully depends on the entire primary key.

---
Good luck 🚀



-- answers.sql

-- ========================================
-- Question 1: Achieving First Normal Form (1NF)
-- ========================================

-- The original table had multivalued fields in the 'Products' column.
-- To achieve 1NF, we split each product into a separate row.
CREATE TABLE ProductDetail (
    OrderID INT,
    CustomerName VARCHAR(100),
    Products VARCHAR(100)
);

-- Each product appears in a separate row to satisfy 1NF
INSERT INTO ProductDetail (OrderID, CustomerName, Products)
VALUES
    (101, 'John Doe', 'Laptop'),
    (101, 'John Doe', 'Mouse'),
    (102, 'Jane Smith', 'Tablet'),
    (102, 'Jane Smith', 'Keyboard'),
    (102, 'Jane Smith', 'Mouse'),
    (103, 'Emily Clark', 'Phone');

-- ========================================
-- Question 2: Achieving Second Normal Form (2NF)
-- ========================================

-- The 'ProductDetail' table in 1NF has a partial dependency:
-- CustomerName depends only on OrderID, not the composite key (OrderID, Products).
-- To achieve 2NF, we separate the data into two tables:
-- 1. 'Orders' for customer info (depends only on OrderID)
-- 2. 'OrderItem' for the relationship between orders and products (depends on both OrderID and ProductID)

-- Step 1: Create the 'Orders' table
CREATE TABLE Orders (
    OrderID INT PRIMARY KEY,
    CustomerName VARCHAR(100)
);

INSERT INTO Orders (OrderID, CustomerName)
VALUES
    (101, 'John Doe'),
    (102, 'Jane Smith'),
    (103, 'Emily Clark');

-- Step 2: Create the 'Product' table to store unique product information (for potential 3NF later)
CREATE TABLE Product (
    ProductID INT PRIMARY KEY,
    ProductName VARCHAR(100)
);

INSERT INTO Product (ProductID, ProductName)
VALUES
    (1, 'Laptop'),
    (2, 'Mouse'),
    (3, 'Tablet'),
    (4, 'Keyboard'),
    (5, 'Phone');

-- Step 3: Create the 'OrderItem' table to link orders and products
CREATE TABLE OrderItem (
    OrderItemID INT PRIMARY KEY,
    OrderID INT,
    ProductID INT,
    Quantity INT,
    FOREIGN KEY (OrderID) REFERENCES Orders(OrderID),
    FOREIGN KEY (ProductID) REFERENCES Product(ProductID)
);

INSERT INTO OrderItem (OrderItemID, OrderID, ProductID, Quantity)
VALUES
    (1, 101, 1, 1), -- John Doe, Laptop
    (2, 101, 2, 1), -- John Doe, Mouse
    (3, 102, 3, 1), -- Jane Smith, Tablet
    (4, 102, 4, 1), -- Jane Smith, Keyboard
    (5, 102, 2, 1), -- Jane Smith, Mouse
    (6, 103, 5, 1); -- Emily Clark, Phone

-- Explanation:
-- The 'Orders' table now holds information solely about the order.
-- The 'Product' table holds unique product names.
-- The 'OrderItem' table establishes a many-to-many relationship between orders and products,
-- and the primary key (OrderItemID) ensures each item in an order is uniquely identified.
-- This structure removes the partial dependency of CustomerName on only OrderID,
-- thus achieving Second Normal Form (2NF).

 
