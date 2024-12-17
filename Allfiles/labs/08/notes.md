
---

## **Scenario**: Online Retail Store Sales Data

### **Goal**:
Combine sales data from **different areas** (e.g., Products, Customers, Time, and Store Locations) into a **Data Warehouse** with a **Star Schema**.

---

## **Data Model Overview**

1. **Fact Table**: Contains transactional data, such as **Sales Orders**.  
   - Includes Surrogate Keys for related **Dimensions**.  
   - Contains measurable facts like **Sales Amount**, **Quantity Sold**, etc.

2. **Dimension Tables**: Provide descriptive details. Each has its own **Surrogate Key** and a unique **Alternate Key**.

   - **DimProduct**: Details about products.  
   - **DimCustomer**: Details about customers.  
   - **DimDate**: Details about dates.  
   - **DimStore**: Details about store locations.

---

## **Star Schema Example**

Below is a visual example:

```
                       +-------------------+
                       |   DimProduct      |
                       |-------------------|
                       | ProductSK (PK)    |
                       | ProductAK         |
                       | ProductName       |
                       | ProductCategory   |
                       +-------------------+

                       +-------------------+
                       |   DimCustomer     |
                       |-------------------|
                       | CustomerSK (PK)   |
                       | CustomerAK        |
                       | CustomerName      |
                       | Region            |
                       +-------------------+

                       +-------------------+
                       |   DimDate         |
                       |-------------------|
                       | DateSK (PK)       |
                       | DateAK            |
                       | Date              |
                       | Month             |
                       | Year              |
                       +-------------------+

                       +-------------------+
                       |   DimStore        |
                       |-------------------|
                       | StoreSK (PK)      |
                       | StoreAK           |
                       | StoreName         |
                       | City              |
                       +-------------------+

                              |
                              |  Foreign Keys (Surrogate Keys)
                              v

                       +-------------------+
                       |   FactSales       |
                       |-------------------|
                       | SalesSK (PK)      |
                       | ProductSK (FK)    |
                       | CustomerSK (FK)   |
                       | DateSK (FK)       |
                       | StoreSK (FK)      |
                       | SalesAmount       |
                       | QuantitySold      |
                       +-------------------+
```

---

### **Key Terminologies**

1. **Surrogate Key (SK)**:  
   - A unique, system-generated key (e.g., `ProductSK`, `CustomerSK`).
   - Used as a **Primary Key (PK)** in the dimension table.
   - Acts as a **Foreign Key (FK)** in the fact table.

2. **Alternate Key (AK)**:  
   - A natural key that uniquely identifies a record in the source system.  
   - Example: `ProductAK` (product ID), `CustomerAK` (customer ID).

---

## **Steps to Build Star Schema**

### 1. **Create Dimension Tables**
Each dimension table gets:
   - Surrogate Key (SK) as the primary key.
   - Alternate Key (AK) as a unique natural key.

#### Sample Data
**`DimProduct`**
| ProductSK | ProductAK | ProductName | ProductCategory |
|-----------|-----------|-------------|-----------------|
| 1         | P001      | Laptop      | Electronics     |
| 2         | P002      | Headphones  | Accessories     |

**`DimCustomer`**
| CustomerSK | CustomerAK | CustomerName | Region      |
|------------|------------|--------------|-------------|
| 1          | C001       | John Doe     | North       |
| 2          | C002       | Jane Smith   | South       |

**`DimDate`**
| DateSK | DateAK    | Date       | Month     | Year |
|--------|-----------|------------|-----------|------|
| 1      | 20240101  | 2024-01-01 | January   | 2024 |
| 2      | 20240102  | 2024-01-02 | January   | 2024 |

**`DimStore`**
| StoreSK | StoreAK | StoreName  | City      |
|---------|---------|------------|-----------|
| 1       | S001    | Store A    | New York  |
| 2       | S002    | Store B    | Chicago   |

---

### 2. **Create Fact Table**
The **Fact Table** references the Surrogate Keys from dimension tables.

**`FactSales`**
| SalesSK | ProductSK | CustomerSK | DateSK | StoreSK | SalesAmount | QuantitySold |
|---------|-----------|------------|--------|---------|-------------|--------------|
| 1       | 1         | 1          | 1      | 1       | 1200        | 1            |
| 2       | 2         | 2          | 2      | 2       | 150         | 2            |

---

## **Relationships**

1. **Surrogate Keys**:  
   - In the `FactSales` table, `ProductSK` links to `DimProduct.ProductSK`.
   - Similarly, `CustomerSK`, `DateSK`, and `StoreSK` link to their respective dimension tables.

2. **Alternate Keys**:  
   - Used for mapping data when importing into the warehouse.
   - Example: `ProductAK` is a natural key for tracking products in the source system.

---

## **How They Relate**

- **FactSales** is the central table containing transactional data (facts like `SalesAmount` and `QuantitySold`).  
- **Dimension Tables** provide descriptive attributes to analyze facts.  
- The **Surrogate Keys** allow linking between the fact and dimension tables for efficient **joins** in queries.

---
