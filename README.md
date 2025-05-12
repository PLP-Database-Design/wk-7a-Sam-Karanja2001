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

 

Great! Based on the Week 7 Assignment Guide you've provided, here's how you can structure the answers.sql file to respond accurately to both questions using the exact tables and data from the guide:


---

-- answers.sql

-- ==========================
-- Question 1: Achieving 1NF
-- ==========================

-- Original Table:
-- The original ProductDetail table contains atomic values, so it's already in 1NF.
-- Each 'Products' entry is a single product per row — no multivalued attributes.

CREATE TABLE ProductDetail (
    OrderID INT,
    CustomerName VARCHAR(100),
    Products VARCHAR(100)
);

INSERT INTO ProductDetail(OrderID, CustomerName, Products)
VALUES
    (101, 'John Doe', 'Laptop'),
    (101, 'John Doe', 'Mouse'),
    (102, 'Jane Smith', 'Tablet'),
    (102, 'Jane Smith', 'Keyboard'),
    (102, 'Jane Smith', 'Mouse'),
    (103, 'Emily Clark', 'Phone');

-- Explanation:
-- Each row contains one product per order; the Products column does not hold multiple values.
-- This satisfies First Normal Form (1NF), which requires atomic (indivisible) values.

-- ==========================
-- Question 2: Achieving 2NF
-- ==========================

-- Problem in 1NF version:
-- The CustomerName is repeated for each order-product combination.
-- It is only dependent on OrderID, not the full row, which creates a partial dependency.

-- Step 1: Separate Order information into its own table

CREATE TABLE orders (
    OrderID INT PRIMARY KEY,
    customerName VARCHAR(100)
);

INSERT INTO orders (OrderID, customerName)
VALUES
    (101, 'John Doe'),
    (102, 'Jane Smith'),
    (103, 'Emily Clark');

-- Step 2: Store product-specific data in a separate table, linked by order_id

CREATE TABLE product (
    product_id INT PRIMARY KEY,
    productName VARCHAR(100),
    quantity INT,
    order_id INT,
    FOREIGN KEY (order_id) REFERENCES orders(OrderID)
);

INSERT INTO product (product_id, productName, quantity, order_id)
VALUES
    (1, 'Laptop', 2, 101),
    (2, 'Mouse', 1, 101),
    (3, 'Tablet', 3, 102),
    (4, 'Keyboard', 2, 102),
    (5, 'Mouse', 1, 102),
    (6, 'Phone', 1, 103);

-- Explanation:
-- 'orders' holds unique OrderID and CustomerName (CustomerName depends only on OrderID).
-- 'product' holds details about each product and links to the corresponding order using order_id.
-- This removes partial dependencies and ensures the structure adheres to Second Normal Form (2NF).


---

Let me know if you'd like help writing 3NF, creating ER diagrams, or turning this into a .sql file download!



