# E-commerce Platform Database

Welcome to the **E-commerce Platform Database** repository! This project contains the database design for a flexible and scalable e-commerce system, supporting product management with variations (e.g., size, color), custom attributes (e.g., material, weight), and categorized product organization. The design includes an Entity-Relationship Diagram (ERD) and a SQL schema with tables, constraints, and sample data.

## Project Overview

This database is designed to power an e-commerce platform where users can browse products, view variations (e.g., a T-shirt in different sizes and colors), and access detailed product attributes. The system supports:
- **Product Organization**: Products are grouped by brands and categories (with support for nested categories).
- **Variations**: Products have purchasable items with specific colors and sizes.
- **Images**: Multiple images per product item, with one marked as primary.
- **Attributes**: Custom attributes (e.g., material, battery life) categorized for flexibility.

The database is implemented in SQL (compatible with MySQL/PostgreSQL) and visualized via an ERD in dbdiagram.io format.

## Repository Contents

- **`erd.dbd`**: The ERD code for rendering the database schema in [dbdiagram.io](https://dbdiagram.io). This file defines all tables, attributes, and relationships.
- **`ecommerce.sql`**: The SQL schema file containing `CREATE TABLE` statements, constraints (primary keys, foreign keys, unique, etc.), indexes for performance, and sample data for testing.
- **`README.md`**: This file, providing an overview, instructions, and details about the database design.

## Database Structure

The database consists of **12 tables** designed to support a robust e-commerce platform:

1. **product_image**: Stores image URLs or file references for product items.
   - Example: A black Nike T-shirt’s primary image URL.
2. **color**: Manages available color options.
   - Example: “Black” with hex code “#000000”.
3. **product_category**: Classifies products into categories (supports nested categories).
   - Example: “Clothing” with a subcategory “Shoes”.
4. **product**: Stores general product details (name, brand, base price).
   - Example: “Nike T-Shirt” with a base price of $29.99.
5. **product_item**: Represents purchasable items with specific variations (e.g., SKU, price).
   - Example: “Nike T-Shirt, Black, Size S” with SKU “NIKE-TS-BLK-S”.
6. **brand**: Stores brand-related data.
   - Example: “Nike” with a description.
7. **product_variation**: Links a product item to its variations (e.g., color, size).
   - Example: A product item with “Black” color and “Size S”.
8. **size_category**: Groups sizes into categories.
   - Example: “Clothing Sizes” for S, M, L.
9. **size_option**: Lists specific sizes within a category.
   - Example: “S” under “Clothing Sizes”.
10. **product_attribute**: Stores custom attributes for products.
    - Example: “Material: Cotton” for a T-shirt.
11. **attribute_category**: Groups attributes into categories.
    - Example: “Physical” for material, weight.
12. **attribute_type**: Defines types of attributes (e.g., text, number).
    - Example: “Material” with data type “Text”.

### Key Features
- **Relationships**: Enforced via foreign keys (e.g., `product_item.ProductID` references `product.ProductID`).
- **Constraints**: Primary keys, unique constraints (e.g., `brand.BrandName`), and checks (e.g., `attribute_type.DataType` restricted to ‘Text’, ‘Number’, ‘Boolean’).
- **Indexes**: Added for performance (e.g., on `product.CategoryID` for category-based searches).
- **Sample Data**: Includes example brands, products, categories, and variations for testing.

## How to Use

### Prerequisites
- A MySQL or PostgreSQL server (e.g., local installation, Docker, or cloud-based like AWS RDS).
- Access to [dbdiagram.io](https://dbdiagram.io) for ERD visualization.
- A SQL client (e.g., MySQL Workbench, pgAdmin, or command-line tools).

### Steps
1. **View the ERD**:
   - Open `erd.dbd` in a text editor and copy its contents.
   - Paste into [dbdiagram.io](https://dbdiagram.io) to render the ERD.
   - The diagram shows all 12 tables, their attributes, and relationships (e.g., `product` to `brand` is Many-to-One).

2. **Set Up the Database**:
   - Create a new database in MySQL/PostgreSQL:
     ```sql
     CREATE DATABASE ecommerce;
     ```
   - Run the `ecommerce.sql` script in your SQL client to:
     - Create the 12 tables with constraints.
     - Insert sample data (e.g., Nike T-shirt, Samsung phone).
     - Add indexes for performance.
   - Example command (MySQL):
     ```bash
     mysql -u root -p ecommerce < ecommerce.sql
     ```

3. **Test the Database**:
   - Query the sample data to verify the setup. Example queries:
     ```sql
     -- List all products with their brands
     SELECT p.ProductName, b.BrandName
     FROM product p
     JOIN brand b ON p.BrandID = b.BrandID;

     -- Find product items with their variations
     SELECT pi.SKU, c.ColorName, so.SizeName
     FROM product_item pi
     JOIN product_variation pv ON pi.ProductItemID = pv.ProductItemID
     LEFT JOIN color c ON pv.ColorID = c.ColorID
     LEFT JOIN size_option so ON pv.SizeOptionID = so.SizeOptionID;
     ```
   - Check constraints (e.g., try inserting a duplicate `BrandName` to see the UNIQUE constraint in action).

4. **Extend the Database**:
   - Add more sample data to `ecommerce.sql` (e.g., new products or categories).
   - Modify tables for additional features (e.g., add a `customer` table for user management).

## ERD Details

The ERD (`erd.dbd`) visualizes the database structure:
- **Entities**: 12 tables with attributes (e.g., `product.ProductName`, `color.HexCode`).
- **Relationships**:
  - One-to-Many: `product` to `product_item` (one product has many purchasable items).
  - Many-to-One: `product_item` to `product`, `product` to `brand`, `product` to `product_category`.
  - Self-referential: `product_category` to `product_category` (for nested categories).
  - Optional: `product_variation.ColorID` and `SizeOptionID` (not all items have both).
- **Constraints**:
  - Primary Keys: Unique identifiers (e.g., `ProductID`, `CategoryID`).
  - Foreign Keys: Enforce referential integrity (e.g., `product_item.ProductID`).
  - Unique: `BrandName`, `CategoryName`, `SKU`, etc.
  - Defaults: `product_item.StockQuantity` defaults to 0, `product_image.IsPrimary` to FALSE.

To view the ERD, paste `erd.dbd` into [dbdiagram.io]([https://dbdiagram.io](https://dbdiagram.io/d/erd-6808b4a41ca52373f5050b62)).

## SQL Schema Details

The `ecommerce.sql` file includes:
- **Table Creation**: All 12 tables with appropriate data types (e.g., `DECIMAL(10,2)` for prices, `ENUM` for `attribute_type.DataType`).
- **Constraints**:
  - Primary and foreign keys for relationships.
  - Unique constraints for identifiers (e.g., `color.ColorName`).
  - Default values and checks for data integrity.
- **Sample Data**:
  - Brands: Nike, Samsung.
  - Categories: Clothing, Electronics, Shoes.
  - Products: Nike T-Shirt, Samsung Galaxy Phone.
  - Product Items: Specific SKUs with prices and stock.
  - Variations: Colors and sizes for product items.
  - Attributes: Material (Cotton), Battery Life (4000mAh).
- **Indexes**: For performance on frequent queries (e.g., `product.CategoryID`, `product_item.ProductID`).

Run `ecommerce.sql` in your SQL client to set up the database and test with the provided sample data.

## Collaboration Process

This project was designed with team collaboration in mind:
- **Analysis**: The team brainstormed requirements, ensuring the database supports product variations, attributes, and categories.
- **Design**: The ERD was created in dbdiagram.io, and the SQL schema was refined through team reviews.
- **Implementation**: Tables were implemented in `ecommerce.sql`, with sample data added for testing.
- **Tools Used**:
  - **GitHub**: For version control, pull requests, and documentation.
  - **dbdiagram.io**: For ERD visualization.
  - **MySQL/PostgreSQL**: For database testing.
  - **WhatsApp Group**: For communication and task tracking.
- **Team Practices**:
  - Daily standups to share progress (e.g., “Finished `product_variation` table”).
  - Weekly reviews to finalize designs and test SQL.
  - GitHub pull requests for code reviews (e.g., “Please check `ecommerce.sql` constraints”).
  - Documentation in GitHub Wiki and Confluence for shared understanding.

## Group Collaboration Tips Applied

To ensure effective teamwork:
- **Stay Connected**: Daily standups via Zoom and async updates in Slack kept the team aligned.
- **Use GitHub**: The repository (`ecommerce-platform`) was used for version control (`erd.dbd`, `ecommerce.sql`), with branches (e.g., `feature/add-tables`) and pull requests for reviews.
- **Track Progress**: Trello boards tracked tasks (e.g., “Test `ecommerce.sql` constraints”), with weekly demos to showcase progress.
- **Keep Everyone in the Loop**: Meeting notes were shared in Google Drive, and GitHub Issues flagged questions (e.g., “Should `ColorID` be optional?”). All members reviewed the ERD and SQL to ensure understanding.

## Potential Extensions

This database can be extended to support additional features:
- **Customer Management**: Add a `customer` table for user accounts and order history.
- **Orders**: Create `order` and `order_detail` tables to track purchases.
- **Reviews**: Add a `review` table for customer feedback on products.
- **Promotions**: Include a `promotion` table for discounts and sales.

To implement these, update `erd.dbd` and `ecommerce.sql` with new tables and relationships.

## Troubleshooting

Common issues and solutions:
- **SQL Errors**: Check for missing foreign keys or incorrect data types. Use a SQL client like MySQL Workbench to debug.
- **ERD Rendering**: Ensure `erd.dbd` syntax is correct in dbdiagram.io. Validate references (e.g., `ref: > product.ProductID`).
- **Sample Data**: If inserts fail, verify foreign key constraints (e.g., `BrandID` must exist in `brand`).

For assistance, refer to the GitHub Issues tab or contact the team.

## Contact

For questions or feedback, reach out to rodney.omondi98@gmail.com or open an issue in this repository.

Thank you for reviewing our e-commerce platform database! We hope this design meets your expectations and provides a solid foundation for an e-commerce system.

---

**Created by**: Rodney Omondi Okoth  
**Date**: April 23, 2025  
**Repository**: https://github.com/rodneyomondi98/ecommerce-platform
