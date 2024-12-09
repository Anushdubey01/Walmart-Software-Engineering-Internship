# Walmart Internship Projects

This repository contains a series of tasks and projects completed during my internship at Walmart as a Software Engineer. The projects involve algorithm design, data structure implementation, system architecture, database modeling, and data processing. This document provides an in-depth explanation of each task, including the problem, my approach, and the final solution.

---

## Table of Contents

1. [Power of Two Max Heap Implementation](#1-power-of-two-max-heap-implementation)
2. [Data Processor UML Class Diagram](#2-data-processor-uml-class-diagram)
3. [Entity-Relationship Diagram (ERD) for Pet Product Database](#3-entity-relationship-diagram-erd-for-pet-product-database)
4. [Data Population and Integration with Python](#4-data-population-and-integration-with-python)
5. [Technologies Used](#technologies-used)
6. [Key Skills](#key-skills)

---

## 1. Power of Two Max Heap Implementation

### Task Overview:
During my internship, I was tasked with implementing a novel data structure called the "Power of Two Max Heap." The heap had specific requirements:
- It must maintain the heap property, where every parent node is greater than its child nodes.
- Each parent node must have \( 2^x \) children, where \( x \) is a constructor parameter (a dynamic input).
- The heap should be efficient in terms of both time and space and should implement core operations such as `insert` and `popMax`.

### Problem Breakdown:
- **Max Heap Property:** In a max heap, for every node, the value must be greater than or equal to its children.
- **Power of Two Constraint:** The main challenge was enforcing that each parent node should have exactly \( 2^x \) children. This created complexity because heaps typically allow for any number of children (usually 2 for binary heaps).
- **Performance Consideration:** Given the custom nature of this heap, I had to ensure that both the `insert` and `popMax` operations were efficient. The `insert` method must add elements in a way that respects the power of two constraint, and `popMax` must remove the maximum element while maintaining the heap property.

### Approach:
1. **Heap Initialization:**
   - The heap was designed to store elements in an array (standard approach in heap implementations).
   - The constructor of the heap accepted an integer `x`, which determined the number of children each parent node would have. I used a variable name `numChildren` instead of `x` to make the code more descriptive.
   
2. **Insert Method:**
   - I implemented the `insert` method by first adding the new element at the end of the heap array.
   - Then, I "bubbled up" the element to ensure that the max heap property was maintained. 
   - The challenge was to ensure that the parent node always had exactly \( 2^x \) children, which I managed by adding checks after the insert to validate this structure.

3. **PopMax Method:**
   - In the `popMax` method, I first swapped the root (maximum element) with the last element in the heap.
   - After the swap, I removed the last element (now at the root) and performed a "heapify down" to restore the heap property.
   - Additionally, I ensured that after each pop, the remaining heap adhered to the \( 2^x \) children constraint.

4. **Performance Optimizations:**
   - I focused on maintaining the time complexity of both `insert` and `popMax` operations as O(log n), where n is the number of elements in the heap.
   - I carefully tracked the number of child nodes at each level to ensure that the heap did not degrade in performance due to violation of the \( 2^x \) children rule.

### Challenges:
- Ensuring the heap maintains the \( 2^x \) children constraint while also satisfying the max heap property required careful handling during both insertion and deletion.
- I tested the heap implementation with both very small and very large values of \( x \) to verify that the heap structure worked under varying constraints.

### Link to Task 1 GitHub Repository:
You can view and explore the implementation of the Power of Two Max Heap on my GitHub repository here:  
[Power of Two Max Heap Implementation](https://github.com/Anushdubey01/PowerOfTwoMaxHeap)

---

## 2. Data Processor UML Class Diagram

### Task Overview:
The task involved designing a UML class diagram for a data processor within a pipeline. The data processor receives raw data, performs operations on it depending on the mode (Dump, Passthrough, or Validate), and interacts with different databases (Postgres, Redis, Elastic). 

### Problem Breakdown:
- The data processor must be flexible enough to switch between different modes of operation (Dump, Passthrough, Validate).
- The processor must support interaction with different databases, each requiring distinct operations (insert, validate, etc.).
- The focus was on designing the class diagram, not on the implementation details, but ensuring the system was easily extendable.

### Approach:
1. **Class Design:**
   - **Processor Class:** I created a central `Processor` class responsible for managing the state (mode and database) and processing data.
   - **ModeIdentifier Enum:** This enum defines the three modes that the processor can be in: `Dump`, `Passthrough`, and `Validate`. Each mode dictates the behavior of the `process` method.
   - **DatabaseIdentifier Enum:** This enum is used to select the database (Postgres, Redis, or Elastic) that the processor will interact with. Each database required specific methods for connecting, inserting, and validating data.
   
2. **Configure Method:**
   - The `configure` method accepts two parameters: `ModeIdentifier` and `DatabaseIdentifier`. This allows the processor to adjust its behavior (mode) and database configuration at runtime.

3. **Process Method:**
   - The `process` method takes a `Datapoint` as input and behaves differently based on the current mode and database configuration. 
     - In `Dump` mode, the data is discarded.
     - In `Passthrough` mode, the data is inserted into the selected database.
     - In `Validate` mode, the data is first validated and then inserted into the database.

4. **Database Interactions:**
   - Each database (Postgres, Redis, Elastic) was modeled as a separate class with a standard interface for connecting, inserting, and validating data. I kept the methods abstract so that the actual implementation of these operations could be added later based on specific requirements.

5. **UML Class Diagram:**
   - I used UML tools to design the structure of the classes and their interactions. This included defining relationships like "uses," "associates with," and "inherits" between the processor class and the database classes.

### Challenges:
- Designing the system to be flexible yet maintainable was a key challenge. I needed to ensure that new databases or processing modes could be added without significant refactoring.
- The main challenge was abstracting the database-specific operations, such as insertions and validations, while keeping the system generalized.

---

## 3. Entity-Relationship Diagram (ERD) for Pet Product Database

### Task Overview:
This task required designing an ERD for a relational database to manage pet products, customers, transactions, and shipments. The database needed to store information about pet food, toys, and apparel, and track the relationship between these products and customers.

### Problem Breakdown:
- The database must normalize data for efficient storage and retrieval.
- Each product (pet food, pet toys, pet apparel) has unique attributes such as name, manufacturer, and additional attributes like flavor or size.
- Products are linked to animals and manufacturers, and transactions involve multiple products.
- The database also needs to track shipments to Walmart locations, with each shipment containing a quantity of products.

### Approach:
1. **Entity Design:**
   - **Products:** I designed separate tables for pet food, pet toys, and pet apparel. Each table had fields specific to the product type.
   - **Customers and Transactions:** Customers are linked to their transactions, which contain a list of products purchased.
   - **Shipments:** I created a `Shipments` table to track shipments, with details about the origin, destination, and associated products/quantities.
   
2. **Normalization:**
   - I ensured that the data was normalized to reduce redundancy and improve data integrity. For example, instead of repeating manufacturer information across all product records, I created a separate `Manufacturers` table and referenced it in the products table.

3. **Relationships:**
   - I established many-to-many relationships between products and transactions, as customers can purchase multiple products in one transaction, and each product can be part of multiple transactions.
   - I used foreign keys to ensure referential integrity between tables (e.g., linking products to manufacturers and transactions to customers).

4. **ERD Creation:**
   - I used ERD tools to visually represent entities, relationships, and attributes. The diagram captures all necessary relationships (e.g., products to animals, customers to transactions).

### Challenges:
- Modeling the database schema to avoid redundancy while ensuring it could support complex queries (e.g., joining products and transactions).
- Ensuring referential integrity across multiple related tables, particularly with many-to-many relationships.

---

## 4. Data Population and Integration with Python

### Task Overview:
I was tasked with writing a Python script to process and insert data from spreadsheets into a SQLite database. The data came in multiple spreadsheets, and I had to combine them correctly before inserting them into the database.

### Problem Breakdown:
- **Multiple Spreadsheets:** Spreadsheet 1 contains product information, while Spreadsheet 2 contains shipment data. Spreadsheet 1's rows need to be combined based on a shipping identifier, and I need to determine the quantity of products in each shipment.
- **Data Transformation:** I had to transform the raw data into a format that could fit into the relational database schema.

### Approach:
1. **Reading Data:**
   - I used the `pandas` library to read the data from the provided spreadsheets into DataFrames for easy manipulation.
   - I merged the two spreadsheets based on the shipping identifier to combine product and shipment data into a unified format.

2. **Data Transformation:**
   - I cleaned and processed the data, ensuring that fields were correctly mapped to the database schema.
   - I handled any missing or inconsistent data by performing validation checks during the transformation process.

3. **Database Insertion:**
   - After transforming the data, I inserted it into the SQLite database using the `sqlite3` library in Python.
   - I ensured that foreign key relationships (e.g., between products and transactions) were maintained during the insertion process.

4. **Automation:**
   - The Python script was automated to process and load data into the database in a consistent and repeatable manner.

### Challenges:
- Handling the transformation of complex data relationships (e.g., combining multiple rows based on identifiers).
- Ensuring that data was inserted correctly into the database without violating any referential integrity constraints.

---

## Technologies Used:
- **Java**: For implementing the custom heap data structure.
- **UML**: For designing system architecture and class diagrams.
- **ERD Tools**: For database schema design (Lucidchart, draw.io).
- **Python**: For data processing and integration tasks.
- **SQLite**: For database management.

---

## Key Skills:
- **Data Structures & Algorithms**: Implemented complex data structures like heaps and optimized them for performance.
- **System Design**: Gained hands-on experience in designing flexible and maintainable systems using UML.
- **Database Design & Modeling**: Designed normalized relational databases with ERDs and managed data relationships.
- **Data Processing**: Proficient in using Python for reading, transforming, and inserting data into databases.
- **Problem Solving**: Worked through complex requirements and constraints, ensuring efficient and reliable solutions.

---

## How to Run the Code

### Power of Two Max Heap Implementation
1. Clone this repository:
   ```bash
   git clone https://github.com/Anushdubey01/Walmart-Software-Engineering-Internship.git
   ```
2. Navigate to the `PowerOfTwoMaxHeap` directory:
   ```bash
   cd PowerOfTwoMaxHeap
   ```
3. Compile and run the Java code:
   ```bash
   javac PowerOfTwoMaxHeap.java
   java PowerOfTwoMaxHeap
   ```

### Data Population Script (Python)
1. Clone this repository:
   ```bash
   git clone https://github.com/Anushdubey01/Walmart-Software-Engineering-Internship.git
   ```
2. Navigate to the `DataPopulationScript` directory:
   ```bash
   cd DataPopulationScript
   ```
3. Install dependencies (if needed):
   ```bash
   pip install -r requirements.txt
   ```
4. Run the script:
   ```bash
   python populate_database.py
   ```

