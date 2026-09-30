# Student JDBC Management System

## 📌 Project Overview

The **Student JDBC Management System** is a Java-based console application that uses **JDBC (Java Database Connectivity)** to connect with a MySQL database.

This project allows users to manage student records through a menu-driven system.

The application demonstrates:

- JDBC database connectivity
- CRUD operations
- PreparedStatement
- ResultSet
- Scanner input
- Searching records
- Transaction management
- Commit and Rollback
- Exception handling
- Resource management using `finally`

---

## 🛠️ Technologies Used

| Technology | Purpose |
|---|---|
| Java | Application development |
| JDBC | Connecting Java with MySQL |
| MySQL | Database |
| MySQL Connector/J | JDBC driver |
| Eclipse | Development IDE |

---

## 🗄️ Database Details

### Database Name

```sql
student
```

### Table Name

```sql
student
```

### Student Table

```sql
CREATE TABLE student (
    id INT PRIMARY KEY AUTO_INCREMENT,
    name VARCHAR(100),
    email VARCHAR(100)
);
```

### Example Records

| ID | Name | Email |
|---:|---|---|
| 1 | Rakshitha | rakshitha@gmail.com |
| 2 | Arun | arun@gmail.com |
| 3 | Anu | anu@gmail.com |

---

## 🔌 JDBC Connection

The application connects to MySQL using:

```java
static String URL = "jdbc:mysql://localhost:3306/student";
static String USER = "root";
static String PASSWORD = "root";
```

The MySQL JDBC driver is loaded using:

```java
Class.forName("com.mysql.cj.jdbc.Driver");
```

The database connection is established using:

```java
con = DriverManager.getConnection(URL, USER, PASSWORD);
```

---

## 📋 Application Menu

The application provides the following options:

```text
======================================
       STUDENT MANAGEMENT SYSTEM
======================================
1. Add Student
2. View All Students
3. Search Student by ID
4. Search Student by Name
5. Search Student by Email
6. Update Student
7. Delete Student
8. Transaction Management
9. Exit
======================================
```

---

# 🔹 Features

## 1. Add Student

The user can add a new student by entering:

- Student name
- Student email

The application uses `PreparedStatement`:

```java
psInsert = con.prepareStatement(
    "INSERT INTO student (name, email) VALUES (?, ?)");
```

The values are supplied using:

```java
psInsert.setString(1, name);
psInsert.setString(2, email);
```

```java
psInsert.setString(2, email);
```

The record is inserted using:

```java
psInsert.executeUpdate();
```

---

## 2. View All Students

The application retrieves and displays all students from the database.

SQL query:

```sql
SELECT * FROM student;
```

The result is processed using `ResultSet`.

Example output:

```text
---------- ALL STUDENTS ----------

ID    : 1
Name  : Rakshitha
Email : rakshitha@gmail.com
--------------------------------

ID    : 2
Name  : Arun
Email : arun@gmail.com
--------------------------------
```

---

## 3. Search Student by ID

The user enters a student ID.

SQL query:

```sql
SELECT * FROM student WHERE id = ?;
```

The ID is passed using:

```java
ps.setInt(1, id);
```

If the student exists, the student's details are displayed.

---

## 4. Search Student by Name

The user can search for a student using their name.

SQL query:

```sql
SELECT * FROM student WHERE name = ?;
```

The name is passed using:

```java
ps.setString(1, name);
```

Multiple matching records can be displayed.

---

## 5. Search Student by Email

The user can search for a student using their email address.

SQL query:

```sql
SELECT * FROM student WHERE email = ?;
```

The email is passed using:

```java
ps.setString(1, email);
```

---

## 6. Update Student

The application allows the user to update:

- Student name
- Student email

The user provides the student ID and the new details.

SQL query:

```sql
UPDATE student
SET name = ?, email = ?
WHERE id = ?;
```

The values are passed using:

```java
psUpdate.setString(1, name);
psUpdate.setString(2, email);
psUpdate.setInt(3, id);
```

---

## 7. Delete Student

The user can delete a student using the student's ID.

SQL query:

```sql
DELETE FROM student WHERE id = ?;
```

The ID is passed using:

```java
psDelete.setInt(1, id);
```

---

# 🔹 8. Transaction Management

Transaction management is used to treat multiple database operations as **one unit of work**.

The transaction in this project performs:

```text
INSERT
   ↓
UPDATE
   ↓
DELETE
   ↓
COMMIT / ROLLBACK
```

Auto-commit is disabled using:

```java
con.setAutoCommit(false);
```

After completing the operations, the user gets two choices:

```text
1. Commit
2. Rollback
```

### Commit

```java
con.commit();
```

`commit()` permanently saves the changes made during the transaction.

### Rollback

```java
con.rollback();
```

`rollback()` cancels the changes made during the transaction.

### Transaction Example

```text
              Transaction Start
                     ↓
             AutoCommit = false
                     ↓
                  INSERT
                     ↓
                  UPDATE
                     ↓
                  DELETE
                     ↓
              Commit / Rollback
                 ↙         ↘
             COMMIT      ROLLBACK
                ↓            ↓
          Save changes   Cancel changes
```

---

# 🔹 PreparedStatement

This project uses `PreparedStatement` for database operations that require parameters.

Examples:

### INSERT

```java
psInsert = con.prepareStatement(
    "INSERT INTO student (name, email) VALUES (?, ?)");
```

### UPDATE

```java
psUpdate = con.prepareStatement(
    "UPDATE student SET name = ?, email = ? WHERE id = ?");
```

### DELETE

```java
psDelete = con.prepareStatement(
    "DELETE FROM student WHERE id = ?");
```

The `PreparedStatement` objects are created inside their **respective methods**.

```text
addStudent()
    ↓
psInsert

updateStudent()
    ↓
psUpdate

deleteStudent()
    ↓
psDelete

transactionManagement()
    ↓
psInsert
psUpdate
psDelete
```

---

# 🔹 Exception Handling

The project uses `try-catch` blocks to handle database-related exceptions.

Example:

```java
catch (SQLException e) {
    e.printStackTrace();
}
```

The JDBC driver loading is also handled:

```java
catch (ClassNotFoundException e) {
    System.out.println("JDBC Driver not found.");
    e.printStackTrace();
}
```

---

# 🔹 Resource Management

The project uses a `finally` block at the end of the `main()` method to close resources.

Resources closed include:

```java
sc.close();
con.close();
```

The purpose of closing resources is to release system and database resources properly.

---

# 📁 Project Structure

```text
StudentJDBC1
│
├── src
│   └── com.jdbc_practice
│       └── StudentJDBC1.java
│
└── README.md
```

---

# ⚙️ How to Run the Project

### Step 1: Install Java

Install Java JDK on your system.

Recommended version:

```text
Java 17
```

### Step 2: Install MySQL

Install MySQL Server and MySQL Workbench.

### Step 3: Create Database

Run:

```sql
CREATE DATABASE student;
```

Select the database:

```sql
USE student;
```

### Step 4: Create Student Table

Run:

```sql
CREATE TABLE student (
    id INT PRIMARY KEY AUTO_INCREMENT,
    name VARCHAR(100),
    email VARCHAR(100)
);
```

### Step 5: Add MySQL Connector/J

Add the MySQL Connector/J library to the Java project's build path.

### Step 6: Update Database Credentials

If your MySQL username or password is different, change:

```java
static String USER = "root";
static String PASSWORD = "root";
```

### Step 7: Run the Java Program

Run:

```text
StudentJDBC1.java
```

The Student Management System menu will appear in the console.

---

# 🎯 Concepts Learned

This project helps demonstrate the following Java and JDBC concepts:

- Java classes and methods
- Static methods
- Scanner
- Exception handling
- JDBC
- JDBC Driver
- DriverManager
- Connection
- Statement
- PreparedStatement
- ResultSet
- CRUD operations
- SQL queries
- Parameterized queries
- Auto-commit
- Transactions
- Commit
- Rollback
- Finally block
- Resource management

---

# 🚀 Future Enhancements

The project can be extended by adding:

- Input validation
- Email validation
- Login system
- Student course management
- Department management
- GUI using Java Swing or JavaFX
- Search using partial names
- Pagination
- DAO architecture
- Service layer
- Maven project structure
- Connection pooling

---

# 👩‍💻 Author

**Rakshitha**

Java Developer Trainee / CSE Data Science Graduate

---

## 📄 License

This project is created for **learning and educational purposes**.
