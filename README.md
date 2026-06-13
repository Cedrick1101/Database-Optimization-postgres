# 🚚 SwiftRide Logistics PostgreSQL Database Project

## Overview

SwiftRide Logistics is a simulated enterprise logistics and delivery company designed to demonstrate real-world database engineering concepts using PostgreSQL.

This project was built to solve common business challenges faced by logistics organizations, including customer management, order tracking, delivery operations, fleet management, warehouse inventory monitoring, payment processing, and data security.

The database follows a multi-schema architecture and implements Role-Based Access Control (RBAC) to simulate how large organizations restrict access to departmental data.

---

## Business Problem

SwiftRide Logistics operates across multiple cities and manages thousands of customers, deliveries, vehicles, drivers, warehouses, and payments.

The company faces several operational challenges:

* Difficulty identifying top-spending customers.
* Limited visibility into driver performance.
* Poor tracking of warehouse inventory levels.
* Inconsistent delivery reporting.
* Lack of centralized payment monitoring.
* Need for secure departmental access to business data.

To address these challenges, a centralized PostgreSQL database solution was designed and implemented.

---

## Project Objectives

This project aims to:

* Design a scalable relational database for a logistics company.
* Demonstrate database normalization and data modeling principles.
* Implement foreign key relationships and constraints.
* Enforce data integrity using CHECK constraints.
* Demonstrate cascading updates and deletions.
* Implement Role-Based Access Control (RBAC).
* Generate business intelligence reports using SQL.
* Simulate real-world departmental access controls.

---

## Database Architecture

The database is organized into four business domains:

### Operations Schema

Manages customer and order operations.

Tables:

* customers
* orders
* deliveries

### Fleet Schema

Manages transportation resources.

Tables:

* drivers
* vehicles

### Warehouse Schema

Manages inventory and storage facilities.

Tables:

* warehouses
* inventory

### Finance Schema

Manages payment transactions.

Tables:

* payments

---

## Entity Relationship Diagram (ERD)

![ERD](docs/Swiftride_logistics_Arc(ERD).png)

---

## Database Relationships

### Customers → Orders

One customer can place many orders.

```sql
customers.customer_id
    ↓
orders.customer_id
```

### Orders → Deliveries

One order generates one delivery record.

```sql
orders.order_id
    ↓
deliveries.order_id
```

### Drivers → Deliveries

A driver can complete multiple deliveries.

```sql
drivers.driver_id
    ↓
deliveries.driver_id
```

### Vehicles → Deliveries

A vehicle can be assigned to multiple deliveries.

```sql
vehicles.vehicle_id
    ↓
deliveries.vehicle_id
```

### Warehouses → Inventory

A warehouse stores multiple inventory items.

```sql
warehouses.warehouse_id
    ↓
inventory.warehouse_id
```

### Orders → Payments

Every order can have a corresponding payment record.

```sql
orders.order_id
    ↓
payments.order_id
```

---

## Technologies Used

* PostgreSQL
* pgAdmin 4
* SQL
* ERD Modeling
* Git
* GitHub

---

## Data Integrity Features

### Primary Keys

Used to uniquely identify records.

Example:

```sql
customer_id SERIAL PRIMARY KEY
```

### Foreign Keys

Used to enforce relationships between tables.

Example:

```sql
FOREIGN KEY (customer_id)
REFERENCES operations.customers(customer_id)
```

### CHECK Constraints

Used to validate incoming data.

Example:

```sql
CHECK (
delivery_status IN
('Pending','In Transit','Delivered','Cancelled')
)
```

### Cascading Operations

Implemented using:

```sql
ON DELETE CASCADE
ON UPDATE CASCADE
```

This ensures referential integrity across related tables.

---

## Role-Based Access Control (RBAC)

The project implements departmental security using PostgreSQL roles.

### Department Roles

* operations_team
* fleet_team
* finance_team
* warehouse_team

### Login Users

* operations_user
* fleet_user
* finance_user
* warehouse_user

Each department can only access its assigned schema.

Example:

```sql
GRANT USAGE ON SCHEMA operations
TO operations_team;
```

### Security Demonstration

Operations users can access:

```sql
operations.customers
operations.orders
operations.deliveries
```

But cannot access:

```sql
finance.payments
fleet.drivers
warehouse.inventory
```

This simulates real-world enterprise security policies.

---

## Business Questions Solved

### 1. Top 5 Customers by Spending

Identifies the highest-value customers.

### 2. Drivers with More Than 20 Deliveries

Measures driver productivity.

### 3. Warehouses with Low Inventory

Helps prevent stock shortages.

### 4. Complete Delivery Report

Provides operational visibility.

### 5. Total Paid Revenue

Measures business performance.

---

## Example Query

Order Classification Report:

```sql
SELECT
    order_id,
    total_amount,

    CASE
        WHEN total_amount >= 200000
            THEN 'High Value'

        WHEN total_amount >= 100000
            THEN 'Medium Value'

        ELSE 'Low Value'
    END AS order_category

FROM operations.orders;
```

---

## Project Structure

```text
swiftride-logistics-postgresql-project
│
├── README.md
│
├── sql
│   ├── 01_create_schemas.sql
│   ├── 02_create_tables.sql
│   ├── 03_constraints.sql
│   ├── 04_rbac_setup.sql
│   └── 05_business_queries.sql
│
├── datasets
│   ├── customers.csv
│   ├── orders.csv
│   ├── deliveries.csv
│   ├── drivers.csv
│   ├── vehicles.csv
│   ├── warehouses.csv
│   ├── inventory.csv
│   └── payments.csv
│
├── docs
│   └── ERD.png
│
└── screenshots
```

---

## Key Learning Outcomes

Through this project, the following database concepts were demonstrated:

* Database Design
* Schema Design
* SQL Querying
* DDL
* DML
* DQL
* Constraints
* Cascading Actions
* ERD Modeling
* Data Governance
* Role-Based Access Control
* Business Reporting
* PostgreSQL Administration

---

## Author

**Ayomikun Adaramola**

Senior Data Engineer

* LinkedIn: https://www.linkedin.com/in/ayomikun-adaramola-/
* Portfolio: https://ayomikun-adaramola.netlify.app
* GitHub: https://github.com/ayomikunadaramola

---

## License

This project is for educational, portfolio, and demonstration purposes.
