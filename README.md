# IDS706 Package a Python Script into a Command-Line Tool (or Rust)

## Continuous Integration with GitHub Actions
[![Install](https://github.com/Reby0217/ids706-miniProj7/actions/workflows/install.yml/badge.svg)](https://github.com/Reby0217/ids706-miniProj7/actions/workflows/install.yml)
[![Lint](https://github.com/Reby0217/ids706-miniProj7/actions/workflows/lint.yml/badge.svg)](https://github.com/Reby0217/ids706-miniProj7/actions/workflows/lint.yml)
[![Format](https://github.com/Reby0217/ids706-miniProj7/actions/workflows/format.yml/badge.svg)](https://github.com/Reby0217/ids706-miniProj7/actions/workflows/format.yml)
[![Tests](https://github.com/Reby0217/ids706-miniProj7/actions/workflows/test.yml/badge.svg)](https://github.com/Reby0217/ids706-miniProj7/actions/workflows/test.yml)




This project focuses on designing, querying, and interacting with a MySQL database using Python. The database contains tables representing customers, products, and orders. The SQL queries include complex operations like joins, aggregations, and sorting, providing insights into customer purchases and order histories.

---
## Deliverables

### SQL Query:
```sql
SELECT
    customers.customer_name,
    customers.customer_email,
    COUNT(orders.order_id) AS total_orders,
    COALESCE(SUM(orders.quantity * products.price), 0) AS total_spent,
    MAX(orders.order_date) AS last_order_date
FROM
    customers
LEFT JOIN
    orders ON customers.customer_id = orders.customer_id
LEFT JOIN
    products ON orders.product_id = products.product_id
GROUP BY
    customers.customer_id
ORDER BY
    total_spent DESC;
```

### Query Explanation:

The SQL query in `src/cli.py` retrieves aggregated customer details by joining the **customers**, **orders**, and **products** tables, providing information on:
- Total number of orders each customer has placed.
- The total amount each customer has spent.
- The last date when each customer placed an order.

1. **Joins**:
   - **LEFT JOIN**: Connects the `customers` table with the `orders` table on the `customer_id`. A left join ensures that every customer is included, even if they haven't placed any orders.
   - The second join connects the `orders` table with the `products` table, allowing access to product prices to calculate the total amount spent by each customer.

2. **Aggregation**:
   - **COUNT(orders.order_id)**: Counts the number of orders each customer has placed.
   - **SUM(orders.quantity * products.price)**: Multiplies the quantity of each ordered product by its price to calculate the total amount spent by the customer.
   - **MAX(orders.order_date)**: Retrieves the latest date on which the customer placed an order.

3. **Handling NULL values**:
   - **COALESCE(SUM(...), 0)**: Ensures that if a customer has not placed any orders, the result will show `$0` spent instead of `NULL`.

4. **Sorting**:
   - **ORDER BY total_spent DESC**: Sorts the customers in descending order based on how much they have spent, with the highest spender first.

### Expected Results:

The expected result is a list of customers showing their total number of orders, total amount spent, and the last order date. The data is sorted by the total amount spent, with the highest spender listed first:

```
Customer: John Doe (john@example.com)
Total Orders: 2
Total Spent: $1399.97
Last Order Date: 2023-09-16

Customer: Jane Smith (jane@example.com)
Total Orders: 1
Total Spent: $49.99
Last Order Date: 2023-09-17
```

- **John Doe** has placed 2 orders and spent $1399.97, with the last order on `2023-09-16`.
- **Jane Smith** has placed 1 order and spent $49.99, with the last order on `2023-09-17`.



## Makefile

The project uses a `Makefile` to streamline development tasks, including testing, formatting, linting, and installing dependencies. Key Makefile commands:

- **Install**: Upgrade `pip` and install project dependencies.
  ```bash
  make install
  ```
  
- **Setup**: Create and activate a virtual environment, ensuring the latest version of `pip` is installed.
  ```bash
  make setup
  ```

- **Format**: Automatically format all Python files with `black`.
  ```bash
  make format
  ```

- **Lint**: Check code quality using `ruff`.
  ```bash
  make lint
  ```

- **Test**: Run the unit tests for the project.
  ```bash
  make test
  ```

- **Run**: Execute the main Python script to interact with the database.
  ```bash
  make run
  ```

- **All**: Run the full suite of tasks—install, setup, lint, test, and format.
  ```bash
  make all
  ```

## Getting Started

### Prerequisites

- Python 3.9+
- `pip` for managing dependencies

### Installation

1. Clone the repository:

   ```bash
   git clone https://github.com/Reby0217/ids706-miniProj6.git
   cd ids706-miniProj6
   ```

2. Install dependencies:

   ```bash
   make install
   ```
3. Create and activate a virtual environment:
   ```bash
   make setup
   ```

4. Run the Python script:
   ```bash
   make run
   ```

5. Run tests:
   ```bash
   make test
   ```

6. Format and lint the code:
   ```bash
   make format
   make lint
   ```

---

## Running MySQL in Docker

To run the MySQL database in a Docker container, follow these steps:

1. **Run MySQL in Docker**:
   ```bash
   docker run --name mysql-db -e MYSQL_ROOT_PASSWORD=qwer1234 -e MYSQL_DATABASE=ecommerce_db -p 3306:3306 -d mysql:8.0
   ```

---

## Running the Project in Docker

To run the project in a Docker container, follow these steps:

1. **Build the Docker Image**:
   ```bash
   make docker-build
   ```

2. **Run the Docker Container**:
   ```bash
   make docker-run
   ```

3. **Run Tests in Docker**:
   ```bash
   make docker-test
   ```

### Screenshots
![Docker](screenshots/dockerBuild.png)

![dockerRun.png](screenshots/dockerRun.png)

![test](screenshots/test.png)
---

## Accessing MySQL in Docker

To access the MySQL client inside your running MySQL container:

```bash
docker exec -it mysql-db mysql -u root -p
```

Once you're inside the MySQL client, you can run queries to view the tables:

```sql
USE ecommerce_db;
DESCRIBE customers; SELECT * FROM customers;
DESCRIBE products; SELECT * FROM products;
DESCRIBE orders; SELECT * FROM orders;
```

### Screenshots

![Table](screenshots/table1.png)

![Table](screenshots/table2.png)

![Table](screenshots/table3.png)
