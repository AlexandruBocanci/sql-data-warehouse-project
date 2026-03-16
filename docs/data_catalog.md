# Data Catalog for Gold Layer

## Overview

The **Gold Layer** represents the business-ready data model used for analytics, dashboards, and reporting.  
It is organized into:

- **Dimension tables** for descriptive business entities
- **Fact tables** for measurable business events and metrics

---

## 1. `gold.dim_customers`

**Purpose:** Stores customer details enriched with demographic and geographic data.

### Columns

| Column Name | Data Type | Description |
|---|---|---|
| `customer_key` | `INT` | Surrogate key uniquely identifying each customer record in the dimension table. |
| `customer_id` | `INT` | Unique numerical identifier assigned to each customer. |
| `customer_number` | `NVARCHAR(50)` | Alphanumeric identifier representing the customer, used for tracking and referencing. |
| `first_name` | `NVARCHAR(50)` | The customer's first name, as recorded in the system. |
| `last_name` | `NVARCHAR(50)` | The customer's last name or family name. |
| `country` | `NVARCHAR(50)` | The country of residence for the customer (for example, `Australia`). |
| `marital_status` | `NVARCHAR(50)` | The marital status of the customer (for example, `Married`, `Single`). |
| `gender` | `NVARCHAR(50)` | The gender of the customer (for example, `Male`, `Female`, `n/a`). |
| `birthdate` | `DATE` | The date of birth of the customer, formatted as `YYYY-MM-DD` (for example, `1971-10-06`). |
| `create_date` | `DATE` | The date the customer record was created in the system. |

---

## 2. `gold.dim_products`

**Purpose:** Provides information about products and their business attributes.

### Columns

| Column Name | Data Type | Description |
|---|---|---|
| `product_key` | `INT` | Surrogate key uniquely identifying each product record in the product dimension table. |
| `product_id` | `INT` | Unique identifier assigned to the product for internal tracking and referencing. |
| `product_number` | `NVARCHAR(50)` | Structured alphanumeric code representing the product, often used for categorization or inventory tracking. |
| `product_name` | `NVARCHAR(50)` | Descriptive name of the product, including key details such as type, color, and size. |
| `category_id` | `NVARCHAR(50)` | Unique identifier for the product category, linking the product to its high-level classification. |
| `category` | `NVARCHAR(50)` | Broader classification of the product (for example, `Bikes`, `Components`). |
| `subcategory` | `NVARCHAR(50)` | More detailed classification of the product within a category. |
| `maintenance_required` | `NVARCHAR(50)` | Indicates whether the product requires maintenance (for example, `Yes`, `No`). |
| `cost` | `INT` | Cost or base price of the product, measured in whole currency units. |
| `product_line` | `NVARCHAR(50)` | Product line or series to which the product belongs (for example, `Road`, `Mountain`). |
| `start_date` | `DATE` | The date when the product became available for sale or use. |

---

## 3. `gold.fact_sales`

**Purpose:** Stores transactional sales data for analytical and reporting purposes.

### Columns

| Column Name | Data Type | Description |
|---|---|---|
| `order_number` | `NVARCHAR(50)` | Unique alphanumeric identifier for each sales order (for example, `SO54496`). |
| `product_key` | `INT` | Surrogate key linking the order to the product dimension table. |
| `customer_key` | `INT` | Surrogate key linking the order to the customer dimension table. |
| `order_date` | `DATE` | The date when the order was placed. |
| `shipping_date` | `DATE` | The date when the order was shipped to the customer. |
| `due_date` | `DATE` | The date when the payment for the order was due. |
| `sales_amount` | `INT` | Total monetary value of the sale for the line item, in whole currency units. |
| `quantity` | `INT` | Number of units ordered for the line item. |
| `price` | `INT` | Price per unit for the line item, in whole currency units. |

---

## Gold Layer Summary

| Table Name | Type | Business Role |
|---|---|---|
| `gold.dim_customers` | Dimension | Customer master data for segmentation and reporting |
| `gold.dim_products` | Dimension | Product master data for catalog and category analysis |
| `gold.fact_sales` | Fact | Sales transactions used for revenue and performance analysis |

---
